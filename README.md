## My tmux config

### Content
- [Setup](#setup)
- [Setup - Install Homebrew](#install-homebrew)
- [Setup - Install tmux](#install-tmux)
- [Setup - Configure True Colors](#configure-true-colors)
- [Setup - Change default prefix](#change-default-tmux-prefix-to-ctrl-a)
- [Setup - Enable mouse in tmux](#enable-mouse-in-tmux)
- [Tmux Themes](https://github.com/jimeh/tmux-themepack#themes)
- [Keybinds](#keybinds)

This `README.md` and `plugins` directory are located in the `~/.tmux` directory.

### Setup

#### install **Homebrew**

Open a terminal window on your mac. Could be the default terminal or something else like iTerm2 which is what I'm currently using.<br>
Run the following command to install Homebrew.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

If necessary, when prompted, enter your password here and press enter.
If you haven't installed the XCode Command Line Tools, when prompted, press enter and homebrew will install this as well.

#### Add to path (Only Apple silicon Macbooks)

After installing, add it to the path. This step shouldn't be necessary on Intel macs.<br>
Run the following two commands to do so.

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

#### Install tmux

Run the following command to install tmux.

```bash
brew install tmux
```

You can check by entering `tmux -V`.

#### Setup `.tmux.conf` file

##### Go to your home directory.

```bash
cd
```

##### Create your `.tmux.conf` file

```bash
touch ~/.tmux.conf
```

##### Add Configuration Options to File

Open with preferred editor and add the following ("open -t yabairc", "code yabairc" (visual studio code), "vim yabairc", etc...).<br>
There are some configuration options that will be available only if you partially disable SIP (Sytem Integrity Protection).<br>
All of the options I've configured below will work without disabling SIP.<br>
<br>

```bash
vim ~/.tmux.conf
nano ~/.tmux.conf
open ~/.tmux.conf
# ect ...
```

#### Configure True Colors

```bash
set -g default-terminal "screen-256color"
```

#### Change Default TMUX Prefix to "Ctrl-a"

Add the following to ~/.tmux.conf.

```bash
set -g prefix C-a
unbind C-b
bind-key C-a send-prefix
```

#### Change keybinds for splitting windows

Add this to ~/.tmux.conf.<br>

```bash
unbind %
bind | split-window -h

unbind '"'
bind - split-window -v
```

#### Add keybind for easily refreshing tmux configuration

Add this to ~/.tmux.conf to be able to refresh tmux config with "Ctrl-a" and then "r"

```bash
unbind r
bind r source-file ~/.tmux.conf
```

#### Add keybinds for easily resizing tmux panes

```bash
bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
bind -r h resize-pane -L 5

# This keybind is for maximizing & minimizing your tmux pane.
bind -r m resize-pane -Z
```

#### Enable mouse in tmux

```bash
set -g mouse on
```

#### Configure vim movements for tmux's copy mode

```bash
set-window-option -g mode-keys vi

bind-key -T copy-mode-vi 'v' send -X begin-selection # start selecting text with "v"
bind-key -T copy-mode-vi 'y' send -X copy-selection # copy text with "y"

unbind -T copy-mode-vi MouseDragEnd1Pane # don't exit copy mode after dragging with mouse
```

#### Install tpm (tmux plugin manager)

Run the following command.<br>
This wil cinstall tpm and will add it in `~/.tmux/plugins/tpm`.

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

#### Add and configure tmux plugins with tpm

**Note**: `run '~/.tmux/plugins/tpm/tpm'` needs to be at the bottom of at all times!

```bash
# tpm plugin
set -g @plugin 'tmux-plugins/tpm'

# list of tmux plugins
set -g @plugin 'christoomey/vim-tmux-navigator' # for navigating panes and vim/nvim with Ctrl-hjkl
set -g @plugin 'jimeh/tmux-themepack' # to configure tmux theme
set -g @plugin 'tmux-plugins/tmux-resurrect' # persist tmux sessions after computer restart
set -g @plugin 'tmux-plugins/tmux-continuum' # automatically saves sessions for you every 15 minutes

# tmux theme config
set -g @themepack 'powerline/default/cyan' # use this theme for tmux

set -g @resurrect-capture-pane-contents 'on' # allow tmux-ressurect to capture pane contents
set -g @continuum-restore 'on' # enable tmux-continuum functionality

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

For more themes you can check [Tmux Themepack](https://github.com/jimeh/tmux-themepack#themes)

### Keybinds

here is a list of usefull keybindings in regards to my tmux setup.<br>
Note `prefix` in my default case is `Ctrl + a`, this behavior can be changed by editing `~/.tmux.conf`

#### Install new plugins

Add the plugin in your `~/.tmux.conf` file then do `prefix + I`.


#### Create, kill, attach new tmux sessions

`tmux new -s <session name>`        - create new tmux session<br>
`tmux a -t <session name>`          - Attach to a specific session<br>
`tmux kill-session <session name>`  - Kill a specific session<br>
`tmux kill-server`                  - kill clients, sessions and server<br>
`tmux ls`                           - List up all tmux sessions<br>

#### Navigate in your Window, panes

```
In a session you have windows and panes.
A window is basically a collection of split panes.

If you create a new session you see a window with a single pane.
You can add a second pane in that window with "prefix + |" OR "prefix + -"
```

**Create split windows**

`prefix + -`    - Create a horizontal split<br>
`prefix + |`    - Create a vertical split<br>

**Navigate through your splitted windows.**
You can only navigate like this if you have the `christoomey/vim-tmux-navigator` plugin installed.

`Ctrl + h`    - Navigate to the left<br>
`Ctrl + j`    - Navigate up<br>
`Ctrl + k`    - Navigate down<br>
`Ctrl + l`    - Navigate to the right<br>

**Navigate between windows**

`prefix + n`    - Go to the next window<br>
`prefix + p`    - Go to the previous window<br>

**Exit window by.**

`exit` or `Ctrl + d`

#### Resize your window

`prefix + h`    - Resize your window to the left<br>
`prefix + j`    - Resize your window up<br>
`prefix + k`    - Resize your window down<br>
`prefix + l`    - Resize your window to the right<br>
`prefix + m`    - Toggle window between full screen<br>

#### Check sessions and panes

`prefix + s`    - Shows active sessions<br>
`prefix + w`    - Show all windows<br>

#### Check time

`prefix + t`    - Shows current time

#### Rename sessions and windows

`prefix + :rename` or `prefix + :rename-session` <new session name> - Renames the session<br>
`prefix + :renamew` or `prefix + :rename-window` <new window name>  - Renames the current window<br>

#### Copy mode

`prefix + [`    - Enable copy mode<br>
`v`             - Enable selection<br>
`y`             - Copy into your clipboard<br>

You can use `$` to go to the end of a line.<br>
And you can use `^` to go to the beginning of a line.<br>


