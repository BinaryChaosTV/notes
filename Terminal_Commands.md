# Linux Notes
## Markdown
[Visit rwxrob's website on how to use Markdown](https://rwx.gg/markdown)

## List of Terminal Commands

**clear** - Erase all text on the terminal.  
**fg** - Displays an input (like  a loop) on the foreground.  
**top** - Display processes running on the system.
**type** <command> - Will tell you what type of command it is.
**\<command>** - Runs original command if it's being aliased.

**curl <link> > ix** - pull a file as a pastebin.
**ix < <filename>** - Copy a file as a pastable link.

### Directory related
**pwd** - Show current Directory  
**dir** - Show list of folders in the current directory  
**ls** - Show a list of all files within current directory
* ls -a - Will also show all hidden files in current directory
* ls -l - Will show you all files, with details in following order: Read/Write Permissions, Owner / Group, File Size, Last accessed, Name of File.

**ln -s** /path/to/file /path/to/link - Symbolic Link

**cd** - Base to change directory. Use alone to return *Home*.
* cd .. - Go to parent directory
* cd - - Go to previous directory

**diff** - Tells you the difference between two files.

### File / Folder Related
**mkdir** - Create a folder inside a directory  
**mv** - move a file from one directory to another. Also used to rename files.  
**rm** - Remove a file from a directory
* rm -rf - Force remove a directory and everything in it. *Be careful when using this*

**cp** - Copy a file or directory
* cp ~ - Copy from home directory

**vi** - Open a file in *Vim*.  
**nano** - Open a file in *Nano*. *Just use Vim instead.*  
**touch** - Create an empty file, or update last time it's been accessed.  
**cat** - Displays what's inside a file. *Great for single line files*  

## Terminal Shortcuts

**Ctrl + C** - Cancel an input.  
**Ctrl + D** - Uuuuh...yes.

----

# Vim Commands

[Visit rwxrob's website for Vim breakdown](https://rwx.gg/vim)

**i** - Insert mode  
**:** - Command Mode  
* :q - Quit without saving
* :w - Save file

**Ctrl + [** OR **Esc** - Escape  
**Shift + ZZ** - Save and Quit
**Shift + ZQ** - Quit without saving
