# Additional Things

## 1. Shell Expansion Mechanics (Critical but Often Ignored)

These affect how commands actually execute:

* **Brace expansion**

  ```bash
  echo file{1..5}.txt
  ```
* **Tilde expansion**

  ```bash
  cd ~username
  ```
* **Globbing (wildcards beyond *)**

  ```bash
  ls *.log
  ls **/*.log      # requires shopt -s globstar
  ```
* **Word splitting rules** (very important for secure scripting)
* **Quote mechanics**:

  * single quotes
  * double quotes
  * backslash escaping
    These radically change how Bash interprets your script.

Missing these leads to bugs, broken scripts, and unsafe code.

---

## 2. Shell Options (shopt + set)

These enhance Bash behavior beyond defaults.

### Useful `shopt` features:

```bash
shopt -s globstar       # recursive ** globbing
shopt -s extglob        # extended globbing
shopt -s dotglob        # include dotfiles in globs
shopt -s autocd         # cd by typing folder name
```

### Useful `set` options:

```bash
set -o nounset  # error on undefined vars
set -o errexit  # exit on command failure
set -o pipefail # fail if any piped command fails
```

These are essential for writing **robust, predictable security scripts**.

---

## 3. Subshells vs. Current Shell Execution

Many new scripters don’t realize commands like this run in **subshells**:

```bash
( cd /tmp && echo $$ )
```

But this one does not:

```bash
cd /tmp && echo $$
```

Why it matters:
Environment changes inside subshells **do not persist** into parent shells. This knowledge prevents hours of debugging.

---

## 4. File Descriptors and Streams

You know about redirection, but not the underlying mechanics.

What you must learn:

* stdin = FD 0
* stdout = FD 1
* stderr = FD 2

Examples:

```bash
echo "error" >&2
command 2>/dev/null
command >out.txt 2>&1
```

Manually working with FDs:

```bash
exec 3>logfile
echo "hello" >&3
exec 3>&-
```

This matters for:

* secure logging
* debugging
* process isolation
* advanced automation

---

## 5. Process Substitution (Hugely Useful)

This lets you treat command output like a file.

Examples:

```bash
diff <(ls /etc) <(ls /usr/etc)
```

Used in automation, scanning, and comparing data streams.

---

## 6. Named Pipes (FIFOs)

A way to create persistent pipes:

```bash
mkfifo pipe
echo "data" > pipe &
cat pipe
```

Used in:

* inter-process communication
* asynchronous scripts
* advanced CTF challenges

---

## 7. xargs and Parallel Command Execution

If you’re doing automation or cybersecurity, you *must* know xargs.

Examples:

```bash
ls *.txt | xargs wc -l
```

Parallel execution:

```bash
cat hosts.txt | xargs -P 10 -n 1 ping -c 1
```

Used for:

* mass scanning
* mass file operations
* parallel job execution

---

## 8. find Command Mastery

Find is practically a programming language by itself.

You must learn:

* complex predicates
* executing commands via `-exec`
* type filters
* permission filters
* timestamp filters

Examples:

```bash
find /var/log -type f -mtime -1
```

More advanced:

```bash
find . -type f -size +10M -exec ls -lh {} \;
```

This is crucial for:

* forensics
* incident response
* Linux administration

---

## 9. awk and sed Basics (Non-Bash but Mandatory for Scripting)

These are essential companions to Bash.
You don’t need to master them fully, but you need operational fluency.

Examples:

**sed replace**

```bash
sed 's/error/warn/g' file
```

**awk column extraction**

```bash
awk '{print $1, $3}' access.log
```

Without these, your Bash skills are severely limited.

---

## 10. Job Control Beyond Basics

You know about `fg`, `bg`, and `&`, but you still need:

* disown
* trapping STOP/CONT
* process groups
* background pipeline behavior

Example:

```bash
command &
disown
```

Useful when:

* running long scans
* transferring files
* managing async operations

---

## 11. Shell Profiling and Performance Optimization

When scripts get big, you need tools to inspect them.

Examples:

Enable tracing:

```bash
set -x
```

Measure runtime:

```bash
time script.sh
```

Profiling loops:

```bash
PS4='+ $BASH_SOURCE:$LINENO:${FUNCNAME[0]} ' bash -x script.sh
```

This is necessary for writing production-ready tooling.

---

## 12. Bash Debugging Techniques

Debug mode:

```bash
bash -x script.sh
bash -v script.sh
bash -n script.sh    # syntax check only
```

Selective debugging:

```bash
set -x
command
set +x
```

Without this, you'll get stuck on weird bugs.

---

## 13. Secure Coding Practices in Bash

You must learn:

* avoiding unsafe variable expansion
* validating user input
* avoiding word splitting vulnerabilities
* avoiding unsafe globbing
* working safely with paths containing spaces
* sanitizing filenames
* using arrays instead of unquoted strings
* avoiding `eval` unless absolutely necessary

These are real security concerns.

---

## 14. POSIX vs Bash Differences

You must know what features are:

* POSIX shell-compatible
* Bash-only

Because:

* Cloud scripting
* Embedded Linux
* Docker containers
* Init systems

often use **sh**, not **bash**.

Example:

```bash
[[ ... ]]   # Bash only
[ ... ]     # POSIX
```

Knowing this prevents portability issues.