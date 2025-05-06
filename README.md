# 🐧 Learning Linux Commands  
Learning Linux commands so I can do some actual engineering – one terminal at a time.
![image](https://github.com/user-attachments/assets/e808416f-a5ac-4408-8abd-44a0d6bccd4c)
---

## 📁 Navigating the File System
- `pwd` — Show the current working directory
  - `pwd -L` — Prints the logical current working directory, including any symbolic links (default behavior)
  - `pwd -P` — Prints the physical directory, with all symbolic links resolved
- `cd` — Change directory  
- `ls` — List directory contents  
  - `ls -a` — Show hidden files  
  - `ls -l` — Long listing format  
  - `ls -r` — Reverse order  
- `~` — Shortcut for home directory

---

## 📂 Creating and Managing Files/Directories
- `touch` — Create a file  
Here’s the updated explanation with `-v` and `-m` options added:
* `mkdir` — Create a directory
  * `mkdir -p` — Create parent directories as needed
  * `mkdir -v` — Print a message for each directory created (verbose mode)
  * `mkdir -m MODE` — Set file mode (permissions) for the created directory (e.g., `-m 755`)
* `cp` — Copy a file or directory
  * `cp file.txt dir/` — Copy a single file to a directory
  * `cp file1.txt file2.txt dir/` — Copy multiple files to a directory
  * `cp -i` — Prompt before overwriting (interactive mode)
  * `cp -r` — Recursively copy directories
  * `cp -p` — Preserve file attributes (timestamps, mode, ownership)
  * `cp -u` — Copy only if the source is newer than the destination or if the destination is missing
  * `cp -v` — Verbose mode; print what is being copied
  * `cp -n` — No clobber; do not overwrite existing files
  * `cp -l` — Create hard links instead of copying
  * `cp -s` — Create symbolic links instead of copying
  * `cp *.txt dir/` — Use wildcards to copy selected files
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

## 🔐 File Permissions
- `ls -l example.txt` — Display detailed info about a file  
    - Example: `-rw-rw-r-- 1 root root 0 Jul 29 15:11 example.txt`  
        - `-rw-rw-r--`:  
            - `-` — Regular file (`d` for directory, `l` for symlink)  
            - `rw-` — Owner can read & write  
            - `rw-` — Group can read & write  
            - `r--` — Others can only read  
        - `root` — File owner  
        - `root` — File group  
        - `0` — File size in bytes  
        - `Jul 29 15:11` — Last modified time  
        - `example.txt` — File name

- `sudo chown root:root example.txt` — Change file owner and group  
    - `sudo` — Run with superuser privileges  
    - `chown` — Command to change ownership  
    - `root:root` — New owner:group  
    - `example.txt` — Target file  

- `sudo chmod 700 example.txt` — Set file permissions  
    - `7` — Owner: read (4) + write (2) + execute (1) = `rwx`  
    - `0` — Group: no permission = `---`  
    - `0` — Others: no permission = `---`  
    - Final: `rwx------`

- `ls -ld ~/test-dir` — Show permission info for a directory  
    - Example: `drwx------ 2 labex labex 4096 Jul 29 15:45 /home/labex/test-dir`  
        - `d` — Indicates a directory  
        - `rwx------` — Owner can read/write/execute; group and others have no access  
            - `Read (r)` — Allows listing contents  
            - `Write (w)` — Allows creating/deleting files  
            - `Execute (x)` — Allows entering the directory (`cd`)
---
Sure! Here’s the **updated version** with all your requested additions while keeping the original structure intact:

---

## 🧑‍💻 User Account Management

- `whoami` — Display the current user  
- `id` — Display user and group information  
  Example output:  
  `uid=1000(labex) gid=1000(labex) groups=1000(labex),4(adm),27(sudo)...`  
- `sudo` — Superuser do (run commands as another user, default is root)

---

### **Creating a New User**

- **Using `useradd` (non-interactive)**  
    - `sudo useradd joker`  
        - `sudo` — Grants superuser privileges  
        - `useradd` — Command to create a new user (low-level)  
        - `joker` — The username being created  

    - `sudo grep -w 'joker' /etc/passwd`  
        - Example output: `joker:x:5001:5001::/home/joker:/bin/sh`  
            - `Username:` joker  
            - `Password:` x (actual password is stored securely in `/etc/shadow`)  
            - `User ID:` 5001  
            - `Group ID:` 5001  
            - `Home Directory:` /home/joker (not created yet)  
            - `Default Shell:` /bin/sh

- **Creating a User with a Home Directory**  
    - Add the `-m` flag to create the home directory:  
        - `sudo useradd -m bob`  
    - Check directory: `sudo ls -ld /home/bob`  
        - Example output: `drwxr-x--- 2 bob bob 57 Jan 19 13:33 /home/bob`  
            - `d` — Indicates it's a directory  
            - `rwxr-x---` — Permissions (owner: rwx, group: r-x, others: ---)  
            - `bob bob` — User and group ownership  
            - `57` — Directory size in bytes  
            - `Jan 19 13:33` — Creation date/time  
            - `/home/bob` — Path to the home directory

- **Using `adduser` (interactive)**  
    - `sudo adduser jack`  
        - Prompts for password, full name, room number, etc.  
        - Automatically creates home directory and copies default config files (`/etc/skel`)  
    - Check user:  
        - `sudo grep -w 'jack' /etc/passwd`  

---

### **Comparing `useradd` vs `adduser`**

| Command      | Type               | Home Dir Created? | Interactive? | Copies `/etc/skel`? |
|--------------|--------------------|-------------------|--------------|---------------------|
| `useradd`    | Low-level binary   | No (use `-m`)     | No           | No                  |
| `adduser`    | High-level script  | Yes               | Yes          | Yes                 |

- `useradd` is available on all Linux distros (minimal setup).
- `adduser` is available on Debian/Ubuntu-based distros (easier to use).

---

### **Setting a User Password**

- `sudo passwd joker`  
    - Sets or changes the user’s password  
    - Passwords are securely stored in `/etc/shadow`

---

### **Modifying User Properties**

- Change home directory:  
    - `sudo usermod -d /home/wayne joker`  

- Change shell:  
    - `sudo usermod -s /bin/bash joker`  
    - Verify: `sudo grep -w 'joker' /etc/passwd`

---

### **Adding a User to a Group**

- `sudo usermod -aG sudo joker`  
    - `usermod` — Modify user account  
    - `-aG` — Append user to a group  
    - `sudo` — Target group  
    - `joker` — Target user  

- Example:  
    - `sudo usermod -aG sudo jack`  

- Validate:  
    - `groups joker`  
    - `groups jack`  
    - `su - joker` — Switch to joker's shell  
    - `exit` — Return to previous user

---

### **Group Management**

- **View all groups (sorted)**:  
    - `cat /etc/group | sort`  

- **Search for specific group/user**:  
    - `cat /etc/group | grep -E "labex"`  

- **Create a new group**:  
    - `sudo groupadd developers`  

- **Add user to a group**:  
    - `sudo usermod -aG developers joker`  

- **Check group membership**:  
    - `groups joker`  
    - `groups jack`  

---

### **Locking and Unlocking User Accounts**

- Lock user account:  
    - `sudo passwd -l joker`  

- Unlock user account:  
    - `sudo passwd -u joker`  

---

### **Deleting a User**

- Delete user and home directory:  
    - `sudo userdel -r bob`  

- Verify deletion:  
    - `sudo grep -w 'bob' /etc/passwd`  
    - `sudo ls -ld /home/bob`  

---

## 📦 Package Management
- `apt install <package>` — Install a package using APT (Debian-based distros)

---

## 🔍 Hidden Files

- Files starting with `.` are hidden  
  Example: `.zshrc`, `.bash_profile`cd 

---

## 🧵 Wildcards and Ranges

- **List all `.txt` files**  
  ```bash
  ls *.txt
  ```
  - `*` matches any characters — `*.txt` lists all `.txt` files

- **Create multiple files with a range**  
  ```bash
  touch note_{1..5}.txt
  ```
  - Creates `note_1.txt` to `note_5.txt`

- **List files starting with "note"**  
  ```bash
  ls note*
  ```

- **Wildcard Summary**
  - `*` — Matches any number of characters  
  - `?` — Matches any single character  
  - `[abc]` — Matches one character in the set

---

## 📚 Linux Command Info

- `type <command>` — Check if built-in or external  
- `<command> --help` — Quick summary  
- `man <command>` — Detailed manual  
- `apropos <keyword>` — Search commands by keyword

--- 

Here’s your **README for File and Directory Operations** styled to match the user management section:

---

## 🗂️ File and Directory Operations

- `tree /` — Displays a **tree view** of the entire filesystem starting from root (`/`)  
- `tree nested` — Displays a tree view of the `nested` directory  

- `ls /home` — Lists contents of the `/home` directory  
- `ls /etc` — Lists contents of the `/etc` directory  
- `ls /bin` — Lists contents of the `/bin` directory  

---

### **Navigating Directories**

- `cd /` — Change to the **root directory** (`/`)  
- `ls -l` — List files in **long format** (permissions, ownership, size, date)  

- `cd ~` or `cd` — Change to your **home directory** (`/home/username`)  
- `cd ..` — Move **up one level** in the directory hierarchy  
- `cd -` — Switch to the **previous directory**

---

### **Finding Files and Directories**
- `find . -name "*.txt"`  
    - Search for all `.txt` files in the **current directory** and subdirectories  

- `sudo find / -name "passed"`  
    - Search for files/directories named `passed` starting from the **root directory (`/`)**  
    - `sudo` is required for full system access

- `find ~ -size +1M`  
    - Find files **larger than 1MB** in the **home directory**

- `find ~ -mtime -<n>`  
    - Find files **modified within the last `<n>` days** in the **home directory**  
    - Example:  
    ```bash
    find ~ -mtime -7  # Files modified in the last 7 days
    ```

---

## 🔐 Environment Variables

### **Shell Variables vs Environment Variables**

* Shell variables are **local** to the shell session
* Environment variables are **exported** to **child processes**
* `$` — Used to **access** variable values
* `env` — View all **environment variables**
* `echo $PATH` — Shows the **PATH** variable: directories (colon `:` separated) where the shell looks for commands
* `export` — Makes a shell variable an **environment variable**

  * Example:

  ```bash
  export MY_ENV_VAR="Hello World"
  ```
* `env | grep MY_ENV_VAR` — **Filter** and confirm variable is set
* Environment variables defined in **startup files** like `.zshrc` are applied to **new sessions**

---

### **Modifying the PATH Variable**

* Add a directory to PATH to make scripts/executables available globally

  ```bash
  export PATH="$PATH:$HOME/my_scripts"
  ```
* This allows running any script in `my_scripts` from **any directory**
* Order matters: the shell checks directories in **sequence**

---

### **Making Environment Variables Permanent**

* Open your shell startup file:

  ```bash
  nano ~/.zshrc
  ```
* Add lines:

  ```bash
  export MY_ENV_VAR="This is an environment variable"
  export PATH="$PATH:$HOME/my_scripts"
  ```
* Apply changes:

  ```bash
  source ~/.zshrc
  ```

---

### **Removing Environment Variables**

* Use `unset` to remove a variable from the current session:

  ```bash
  unset MY_ENV_VAR
  ```

---
