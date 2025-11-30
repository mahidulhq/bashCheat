---

# **Bash Complete Tutorial & Cheat Sheet — Basic → Advanced**

*A practical guide for Linux users, programmers, and cybersecurity learners*

# **Introduction to Bash**

**Bash** (Bourne Again SHell) is a command interpreter for Linux/Unix.
You use Bash to:

* Execute commands
* Automate tasks
* Write scripts
* Manage processes
* Work with the system and network

Check version:

```bash
bash --version
```

Run Bash explicitly:

```bash
/bin/bash
```

---

# **Basic Commands**

```bash
whoami        # current user
pwd           # print working directory
clear         # clear terminal
history       # command history
date          # current date/time
uptime        # system load & uptime
uname -a      # system info
```

---

# **Filesystem Navigation**

```bash
ls            # list
ls -l         # long view
ls -a         # show hidden files
cd /path      # change directory
cd ~          # home
cd ..         # go up one level
cd -          # previous directory
```

---

# **File & Directory Operations**

### Create

```bash
touch file.txt
mkdir folder
mkdir -p nested/folder/structure
```

### Copy / Move / Delete

```bash
cp src dst
cp -r dir1 dir2
mv old new
rm file
rm -r folder
rm -rf folder      # beware: force delete
```

### File Info

```bash
stat file.txt
du -sh folder
df -h
```

---

# **Viewing & Editing Content**

```bash
cat file
tac file
head -n 20 file
tail -f logfile
nano file
vim file
```

Search inside files:

```bash
grep "text" file
grep -ri "keyword" .
```

---

# **Permissions & Ownership**

### View

```bash
ls -l
```

### Change Permissions

```bash
chmod 755 script.sh
chmod u+x file
chmod g-w file
```

### Change Ownership

```bash
sudo chown user:group file
```

Legend:

* **r** = read
* **w** = write
* **x** = execute

---

# **Environment Variables**

View:

```bash
printenv
echo $PATH
```

Set:

```bash
export VAR=value
```

Permanent (in `~/.bashrc`):

```bash
export PATH="$PATH:/custom/bin"
```

Unset:

```bash
unset VAR
```

---

# **Command Line Operators**

### Redirection

```bash
>   # overwrite
>>  # append
<   # input file
```

Examples:

```bash
ls > out.txt
echo "test" >> log.txt
```

### Logical Operators

```bash
cmd1 && cmd2   # run cmd2 only if cmd1 succeeds
cmd1 || cmd2   # run cmd2 only if cmd1 fails
cmd1 ; cmd2    # run sequentially
```

---

# **Pipelines**

```bash
cmd1 | cmd2 | cmd3
```

Example:

```bash
ps aux | grep bash | wc -l
```

---

# **Process Management**

```bash
ps aux
top
htop
kill PID
kill -9 PID      # force kill
jobs
fg %1
bg %1
```

Run in background:

```bash
command &
```

Run immune to hangup:

```bash
nohup script.sh &
```

---

# **Networking Essentials**

```bash
ip a
ip r
ping host
curl http://site.com
wget file
traceroute host
netstat -tulnp
ss -tulnp
```

DNS lookup:

```bash
dig domain.com
host domain.com
```

---

# **Package Management**

### Debian/Ubuntu

```bash
sudo apt update
sudo apt install pkg
sudo apt remove pkg
```

### RedHat/CentOS

```bash
sudo yum install pkg
sudo dnf install pkg
```

---

# **Bash Scripting Essentials**

Every script starts with a shebang:

```bash
#!/bin/bash
```

Make script executable:

```bash
chmod +x script.sh
```

Run it:

```bash
./script.sh
```

---

# **Variables**

```bash
name="Sir"
echo "$name"
```

Constants:

```bash
readonly PI=3.14
```

Arithmetic:

```bash
echo $((5 + 3))
let x=5+2
```

---

# **User Input**

```bash
read -p "Enter name: " name
echo "Hello, $name"
```

Silent input:

```bash
read -sp "Password: " pass
```

---

# **Conditions & Comparisons**

### If / Else

```bash
if [ $a -gt 10 ]; then
    echo "big"
elif [ $a -eq 10 ]; then
    echo "equal"
else
    echo "small"
fi
```

### File tests

```bash
[ -f file ]     # regular file
[ -d dir ]      # directory
[ -r file ]     # readable
[ -w file ]     # writable
[ -x file ]     # executable
```

### String tests

```bash
[ "$a" = "$b" ]
[ -z "$a" ]      # empty
```

---

# **Loops**

### For

```bash
for i in {1..5}; do echo $i; done
```

### While

```bash
while true; do
    echo "looping"
    sleep 1
done
```

### Until

```bash
until [ "$x" -eq 5 ]; do
    echo "$x"
done
```

---

# **Functions**

```bash
function greet() {
    echo "Hello $1"
}

greet "Sir"
```

Return values:

```bash
return 5
```

---

# **Arrays**

```bash
arr=(a b c)
echo ${arr[0]}
echo ${arr[@]}
echo ${#arr[@]}
```

---

# **String Operations**

```bash
str="abcdef"
echo ${str:2:3}          # substring
echo ${str#abc}          # remove prefix
echo ${str%def}          # remove suffix
echo ${str/abc/xyz}      # replace once
```

---

# **File Operations in Scripts**

### Read file line-by-line

```bash
while IFS= read -r line; do
    echo "$line"
done < file.txt
```

### Check if file exists

```bash
if [ -e file.txt ]; then
    echo "exists"
fi
```

---

# **Error Handling**

Exit code:

```bash
echo $?
```

Strict mode (recommended):

```bash
set -euo pipefail
```

Try/catch-like:

```bash
command || { echo "failed"; exit 1; }
```

---

# **Advanced Bash Concepts**

### Command substitution

```bash
files=$(ls)
```

### Here-doc

```bash
cat <<EOF
This is
a multi-line
string
EOF
```

### Cron jobs

```bash
crontab -e
```

### Aliases

```bash
alias ll="ls -al"
```

### Traps

```bash
trap "echo Ctrl-C pressed" SIGINT
```

---

# **Security-Focused Bash Tips**

### Safe file deletion (shredding)

```bash
shred -u file.txt
```

### Check open ports

```bash
ss -tulnp
```

### Find world-writable files

```bash
find / -type f -perm -002 2>/dev/null
```

### Hash a file (integrity)

```bash
sha256sum file.iso
```

### Compare directories

```bash
diff -qr dirA dirB
```

---

# **Appendix: Common Bash One-Liners**

### Find big files

```bash
find / -type f -size +100M 2>/dev/null
```

### Count files

```bash
ls -1 | wc -l
```

### Kill process by name

```bash
pkill firefox
```

### Monitor log updates

```bash
tail -f /var/log/syslog
```

### Quick web request

```bash
curl -I https://example.com
```

### Extract only IP addresses

```bash
grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+"
```

---
