tags: #terminal
# Using Terminal
## Preferences
Open the terminal (Super + T)

1. Preferences:
	1. Profile > Colours > Text and Background Color > Solarized Dark
	2. Profile > Colours > Palette > Solarized
2. Install [[Vim]] -- `sudo apt install vim`
3. Install [Git](https://git-scm.com/download/linux) -- Add the repo and then `sudo apt update; sudo install git`.
4. Install [gh](https://github.com/cli/cli/blob/trunk/docs/install_linux.md) (GitHub CLI)
	1. Sign in by using `gh auth login`, then follow the instructions via HTTPS and through a browser.

## Dotfiles
This is a good time to create some directories where we will put all our repos. Input the following commands in the terminal:

	mkdir -p ~/github/<username>
	cd ~/github/<username>

Ideally, `<username>` should be the same as your github profile as your `.bashrc` uses the `$USER` variable. While staying in the same terminal, paste the following:

	gh repo clone ControlShiftEd/dotfiles
	cd dotfiles
	sh setup

The setup file will add a symlink to your `.bashrc`, `.inputrc`, `.profile`, `.vimrc` and `tmux.conf` files and the `Scripts` folder from the dotfiles to your `$HOME`, as well as add the `Scripts` folder to $PATH. It creates some new aliases for the terminal, which you can view in `.bashrc`. Finally, it enables auto-completion for `gh`.

It will also install [Ethan Shoover's Solarized for VIM](https://github.com/altercation/vim-colors-solarized) and [Tim Pope's Pathogen](https://github.com/tpope/vim-pathogen).

## Commands

`type <command>` - Will tell you what type of command it is.

`clear` - Erase all text on the terminal.  
`fg` - Displays an input (like  a loop) on the foreground.  
`top` - Display processes running on the system.

### Directory related
`pwd` - Show current Directory  
`dir` - Show list of folders in the current directory  
`ls` - Show a list of all files within current directory
`ls -a` - Will also show all hidden files in current directory
`ls -l` - Will show you all files, with details in following order: Read/Write Permissions, Owner / Group, File Size, Last accessed, Name of File.

`ln -s` /path/to/file /path/to/link - Symbolic Link

`cd` - Base to change directory. Use alone to return *Home*.
	`cd ..` - Go to parent directory
	`cd -` - Go to previous directory

`diff` - Tells you the difference between two files.

### File / Folder Related
`mkdir` - Create a folder inside a directory  
`mv` - move a file from one directory to another. Also used to rename files.  
`rm` - Remove a file from a directory
`rm -rf` - Force remove a directory and everything in it. *Be careful when using this*

`cp` - Copy a file or directory
	`cp ~` - Copy from home directory

`vi` - Open a file in *[[Vim]]*.
`touch` - Create an empty file, or update last time it's been accessed.  
`cat` - Displays what's inside a file. *Great for single line files*  

## Shortcuts

`Ctrl + C` - Cancel an input.  
`Ctrl + D` - Uuuuh...yes.
