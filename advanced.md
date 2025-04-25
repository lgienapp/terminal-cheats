# Advanced Shell Command Cheat Sheet

This cheat sheet covers advanced shell techniques and commands for experienced users. For basics, refer to the [basics cheat sheet](basics.md).

## Shell Techniques

###### Brace Expansion
```bash
mkdir -p project/{src,test,docs}/{main,utils}  # Create multiple nested directories
cp file.{txt,bak}                              # Copy file.txt to file.bak
mv file.txt{,.bak}                             # Rename file.txt to file.txt.bak
touch file{1..10}.txt                          # Create file1.txt through file10.txt
echo {a..z}{0..9}                              # Print all combinations of a-z with 0-9
```

###### Process Substitution
```bash
diff <(ls dir1) <(ls dir2)                     # Compare output of two commands
grep pattern <(zcat file.gz)                   # Grep through compressed file without extracting
```

###### Parameter Expansion & String Manipulation
```bash
${var:-default}            # Use default if var is unset or empty
${var:=default}            # Set var to default if unset or empty
${var:+value}              # Use value if var is set, otherwise empty string
${var:offset:length}       # Substring extraction
${var#pattern}             # Remove shortest match from beginning
${var##pattern}            # Remove longest match from beginning
${var%pattern}             # Remove shortest match from end
${var%%pattern}            # Remove longest match from end
${var/pattern/replacement} # Replace first match
${var//pattern/replacement}# Replace all matches
${var^}                    # Uppercase first character
${var^^}                   # Uppercase all characters
${var,}                    # Lowercase first character
${var,,}                   # Lowercase all characters
```

###### Command Substitution & Arithmetic
```bash
files=$(find . -name "*.txt")   # Store command output in variable
echo "Found $(($(wc -l < file) + 10)) lines"  # Arithmetic in command
for i in {1..10}; do echo $((i * 2)); done    # Arithmetic in loop
[[ $(date +%u) -lt 6 ]] && echo "Weekday"     # Conditional with arithmetic
```

## File Operations

###### File Operations with `find`
```bash
find . -type f -exec chmod 644 {} \;         # Change permissions for found files
find . -name "*.tmp" -mtime +7 -delete       # Delete files older than 7 days
find . -size +100M -exec du -h {} \; | sort -hr  # Find and sort large files
find . -type f -newer file.txt                # Find files newer than specified file
```

###### Advanced grep
```bash
grep -P '(?<=pattern1)pattern2(?=pattern3)'   # Perl regex with lookbehind/lookahead
grep -o 'pattern' file.txt | wc -l            # Count occurrences of pattern
grep -l 'pattern' *.txt                       # List only filenames containing pattern
grep -A 3 -B 2 'pattern' file.txt             # Show lines before and after match
```

###### Advanced sed
```bash
sed '3,5d' file.txt                            # Delete lines 3-5
sed '/pattern/,/end/d' file.txt                # Delete from pattern to end
sed '/^$/d' file.txt                           # Remove blank lines
sed -i.bak 's/old/new/g' file.txt              # Edit in place with backup
sed -n '10,20p' file.txt                       # Print only lines 10-20
```

## Remote Operations

###### Advanced SSH
```bash
ssh -J jumphost user@destination                  # Jump host (proxy) connection
ssh -L 8080:localhost:80 user@server              # Local port forwarding
ssh -R 8080:localhost:80 user@server              # Remote port forwarding
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server      # Copy SSH key to server
ssh user@server 'bash -s' < local_script.sh       # Run local script on remote server
vim scp://user@server//path/to/file               # Edit remote file with local editor
sshfs user@server:/remote/path /local/mountpoint  # Mount remote directory with SSHFS

```

###### SSH Config File (~/.ssh/config)
```
Host alias
    HostName server.example.com
    User username
    Port 2222
    IdentityFile ~/.ssh/special_key
    ForwardAgent yes
    
Host bastion-*
    ProxyJump bastion.example.com
    
Host *
    ServerAliveInterval 60
    ControlMaster auto
    ControlPath ~/.ssh/control/%r@%h:%p
    ControlPersist 10m
```

###### Advanced curl
```bash
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://api.example.com  # POST request with JSON
curl -C - -O https://example.com/large-file.zip   # Resume download
curl -A "Custom User Agent" https://example.com   # Set user agent
curl --cookie "name=value" https://example.com    # Send cookie
curl -k https://example.com                       # Ignore SSL certificate errors
curl -L -s https://bit.ly/short | grep title      # Follow redirects, silent mode
```

## Shell Scripting Techniques

###### Error Handling
```bash
#!/bin/bash
set -e          # Exit on error
set -u          # Exit on undefined variable
set -o pipefail # Exit if any command in pipe fails
trap 'echo "Error on line $LINENO"; exit 1' ERR # Custom error handler
```

###### Parallel Execution
```bash
# GNU Parallel with job control
parallel --jobs 4 --eta 'command {}' ::: item1 item2 item3 item4

# Wait for background jobs
for i in {1..5}; do (task $i &); done; wait

# Process substitution for parallel commands
paste <(cmd1) <(cmd2) <(cmd3) | process
```


## One-liners

```bash
# Lines common to both files (like comm -12)
grep -Fxf file1 file2

# Find duplicated files by content
find . -type f -exec md5sum {} \; | sort | uniq -w32 -dD

# Extract all email addresses from files
grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" files*

# Find and replace in multiple files with confirmation
find . -type f -name "*.txt" -exec grep -l "old" {} \; | xargs -I{} bash -c 'read -p "Change {}? [y/N] " r; [[ $r == [yY] ]] && sed -i "s/old/new/g" {}'
```
