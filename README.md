# 🐧 Learning Linux Commands  
Learning Linux commands so I can do some actual engineering – one terminal at a time.

---

## 🧑‍💻 User & Permissions
- `whoami` — Display the current user  
- `id` — Display user and group information  
  Example output:  
  `uid=1000(labex) gid=1000(labex) groups=1000(labex),4(adm),27(sudo)...`  
- `sudo` — Superuser do (run commands as another user, default is root)

---

## 📁 Navigating the File System
- `pwd` — Show current working directory  
- `cd` — Change directory  
- `ls` — List directory contents  
  - `ls -a` — Show hidden files  
  - `ls -l` — Long listing format  
  - `ls -r` — Reverse order  
- `~` — Shortcut for home directory

---

## 📂 Creating and Managing Files/Directories
- `touch` — Create a file  
- `mkdir` — Create a directory  
- `cp` — Copy a file or directory  
  - `cp -r` — Recursively copy directories  
- `mv` — Move or rename a file/directory  
- `rm` — Remove a file  
  - `rm -i` — Prompt before every removal  
  - `rm -f` — Force removal without prompt  
- `rmdir` — Remove empty directories

---

## 🗃️ File Content & Comparison
- `cat` — Display the entire content of a file 
    - `cat-n`: to number all output lines.
- `head` — View the beginning of a file  
    - `head -n 1`: Show only the first line of the file.  
    - `head -c 1`: Show only the first byte (character) of the file.  
- `tail` — View the end of a file  
    - `tail -n 1`: Show only the last line of the file.  
    - `tail -c 1`: Show only the last byte (character) of the file.  
- `diff` — Compare content of two files
    - `diff -r`: The `-r` option tells `diff`difff to recursively compare subdirectories as well.

---

## 📦 Package Management
- `apt install <package>` — Install a package using APT (Debian-based distros)

---

## 🔍 Hidden Files

- Files starting with `.` are hidden  
  Example: `.zshrc`, `.bash_profile`cd 