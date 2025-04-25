# Basic Shell Command Cheat Sheet 

## Navigation & File Management

###### `pwd` - Print Working Directory
```bash
pwd          # Print your current location in filesystem
```

###### `ls` - List Files and Directories
```bash
ls           # List contents of current directory
ls -l        # Long format with permissions and sizes
ls -a        # Show hidden files (starting with .)
ls -h        # Human-readable file sizes (KB, MB, GB)
```

###### `cd` - Change Directory
```bash
cd directory_name    # Move to specified directory
cd                   # Go to home directory
cd ..                # Go up one directory
cd -                 # Go to previous directory
```

###### `mkdir` - Make Directory
```bash
mkdir directory_name     # Create directory
mkdir -p path/to/dir     # Create nested directories as needed
```

###### `touch` - Create Empty File
```bash
touch filename.txt       # Create empty file or update timestamp
```

###### `cp` - Copy Files/Directories
```bash
cp source.txt destination.txt       # Copy file
cp -r source_dir destination_dir    # Copy directory and its contents
```

###### `mv` - Move/Rename Files/Directories
```bash
mv old_name.txt new_name.txt        # Rename file
mv file.txt directory/              # Move file to directory
```

###### `rm` - Remove Files/Directories
```bash
rm filename.txt          # Delete file (no undo!)
rm -r directory          # Delete directory and its contents
rm -f filename.txt       # Force delete without confirmation
```

###### `cat` - Concatenate and Display File Contents
```bash
cat filename.txt         # Display file contents
cat file1 file2          # Display contents of multiple files
```

###### `less` - View File Contents Page by Page
```bash
less filename.txt        # View file (q to quit, space for next page)
```

###### `head` - Display Beginning of File
```bash
head filename.txt        # Show first 10 lines
head -n 20 filename.txt  # Show first 20 lines
```

###### `tail` - Display End of File
```bash
tail filename.txt        # Show last 10 lines
tail -n 20 filename.txt  # Show last 20 lines
tail -f filename.txt     # Follow file in real-time (useful for logs)
```

## File Operations

###### `wc` - Word, Line, Character Count
```bash
wc filename.txt          # Display lines, words, bytes
wc -l filename.txt       # Count lines only
wc -w filename.txt       # Count words only
```

###### `find` - Find Files and Directories
```bash
find . -name "*.txt"           # Find all .txt files in current directory and subdirectories
find . -type d -name "*data*"  # Find directories with "data" in the name
```

###### `grep` - Search Text Patterns
```bash
grep "pattern" filename.txt          # Search for pattern in file
grep -i "pattern" filename.txt       # Case-insensitive search
grep -r "pattern" directory/         # Recursive search in directory
grep -v "pattern" filename.txt       # Show lines NOT matching pattern
```

###### `gzip`/`gunzip` - Compress/Decompress Files
```bash
gzip filename.txt              # Compress file (creates filename.txt.gz)
gunzip filename.txt.gz         # Decompress file (deletes original)
gzip -k filename.txt           # Keep original file when compressing/decompressing
```

###### `tar` - Archive Files
```bash
tar -cvf archive.tar files/         # Create archive
tar -xvf archive.tar                # Extract archive
tar -czvf archive.tar.gz files/     # Create compressed archive
tar -xzvf archive.tar.gz            # Extract compressed archive
```

## Remote Operations

###### `ssh` - Secure Shell
```bash
ssh username@hostname                    # Connect to remote server
ssh -p 2222 username@hostname            # Connect on specific port
ssh -i private_key.pem username@hostname # Connect using key file
ssh alias                                # Connect to remote with options specified under "alias" in ~/.ssh/config
```

###### `scp` - Secure Copy
```bash
scp file.txt username@hostname:/path/       # Copy local file to remote server
scp username@hostname:/path/file.txt .      # Copy remote file to local machine
scp -r directory/ username@hostname:/path/  # Copy entire directory
```

###### `rsync` - Remote Sync
```bash
rsync -av source/ destination/              # Sync directories (a=archive, v=verbose)
rsync -avz source/ user@host:destination/   # Sync to remote with compression
rsync -av --delete source/ destination/     # Delete files in destination not in source
```

###### `wget` - Download Files
```bash
wget https://example.com/file.zip           # Download file
wget -c https://example.com/file.zip        # Resume interrupted download
wget -r -np https://example.com/            # Download website recursively
```

###### `tmux` - Terminal Multiplexer
```bash
tmux                           # Start new session
tmux new -s session_name       # Start named session
tmux ls                        # List sessions
tmux attach -t session_name    # Attach to existing session
```

Common tmux key bindings (prefix is Ctrl+b):
- `prefix c` - Create window
- `prefix ,` - Rename window
- `prefix n` - Next window
- `prefix p` - Previous window
- `prefix %` - Split vertically
- `prefix "` - Split horizontally
- `prefix arrow` - Navigate panes
- `prefix d` - Detach from session

## Wrangling Data

###### `jq` - JSON Processor
```bash
jq '.' file.json                      # Pretty print JSON
jq '.items[0]' file.json              # Access specific element
jq '.[] | .name' file.json            # Extract field from each item
jq -r '.name' file.json               # Raw output (no quotes)
```

###### `awk` - Text Processing
```bash
awk '{print $1}' file.txt             # Print first column
awk -F, '{print $2}' file.csv         # Use comma delimiter, print second column
awk '{sum+=$1} END {print sum}' file  # Sum first column
```

###### `sort` - Sort Lines
```bash
sort file.txt                         # Sort alphabetically
sort -n file.txt                      # Sort numerically
sort -r file.txt                      # Reverse sort
sort -k2 file.txt                     # Sort by second column
```

###### `uniq` - Filter Duplicate Lines
```bash
sort file.txt | uniq                  # Show unique lines (must sort first)
sort file.txt | uniq -c               # Count occurrences of each line
```

## System Information

###### `df` - Disk Space Usage
```bash
df -h                                 # Show disk usage in human-readable format
```

###### `du` - Directory Space Usage
```bash
du -sh directory/                     # Size of directory in human-readable format
du -h --max-depth=1                   # Size of all subdirectories (one level)
```

###### `htop`/`top` - Process Viewer
```bash
top                                   # Show running processes (q to quit)
htop                                  # Interactive process viewer (if installed)
```

###### `ps` - Process Status
```bash
ps aux                                # Show all running processes
ps aux | grep "pattern"               # Filter processes by pattern
```

###### `kill` - Terminate process
```bash
kill process_id                       # Terminate the process with id process_id (you can find it in top/ps)
```


## Tips & Tricks

###### Pipes and Redirects
```bash
command1 | command2                   # Pipe output of command1 to command2
command > file.txt                    # Redirect output to file (overwrite)
command >> file.txt                   # Append output to file
command 2> error.log                  # Redirect error output
command &> all.log                    # Redirect both regular and error output
```

###### Command History
```bash
history                               # Show command history
!n                                    # Run command number n from history
!!                                    # Repeat last command
!string                               # Run most recent command starting with "string"
Ctrl+r                                # Reverse search through history
```

###### Helpful Shortcuts
- `Ctrl+c` - Terminate current command
- `Ctrl+d` - Exit current shell
- `Ctrl+l` - Clear screen
- `Ctrl+a` - Move cursor to beginning of line
- `Ctrl+e` - Move cursor to end of line
- `Ctrl+w` - Delete word before cursor
- `Tab` - Autocomplete command or filename
