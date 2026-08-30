# overthewire-
# 🔐 OverTheWire Bandit Writeup

## Level 1 → 2

### 🎯 Objective

Find the password for the next level. The password is stored in a file named `-`.

### 🔎 Enumeration

After logging into Level 1, I listed the files in the home directory:

```bash
ls
```

The file containing the password was named:

```text
-
```

A filename consisting only of `-` can be interpreted specially by Linux commands, so I needed to specify that it was a file in the current directory.

### 💡 Solution

I used:

```bash
cat ./-
```

Here, `./` explicitly tells Linux that `-` is a filename in the current directory.

### 🔑 Password

```text
PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
```

### 🧠 What I Learned

* `cat` displays the contents of a file.
* `./` specifies a file in the current directory.
* Some filenames can have special meanings to Linux commands.

### 📌 Key Takeaway

When a filename contains a special character such as `-`, using `./filename` can prevent the shell or command from interpreting it as an option.

---

# Level 2 → 3

### 🎯 Objective

Find the password stored in a file whose name contains spaces.

### 🔎 Enumeration

The password file was named:

```text
--spaces in this filename--
```

Since spaces separate command-line arguments, typing the filename normally would cause problems.

### 💡 Solution

I used quotation marks around the complete filename:

```bash
cat "./--spaces in this filename--"
```

The quotes make the entire filename a single argument.

### 🔑 Password

```text
7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
```

### 🧠 What I Learned

* Spaces normally separate arguments in Linux commands.
* Filenames containing spaces should be enclosed in quotes.
* Quotation marks allow the shell to treat the filename as one argument.

### 📌 Key Takeaway

When a filename contains spaces, use quotes or escape the spaces.

---

# Level 3 → 4

### 🎯 Objective

Find the password stored inside a hidden file in the `inhere` directory.

### 🔎 Enumeration

First, I changed to the required directory:

```bash
cd inhere
```

To search for hidden files, I used:

```bash
find . -type f -name ".*"
```

The command searches the current directory and finds files whose names begin with `.`.

### 💡 Solution

The hidden password file was found, and I read its contents using:

```bash
cat ./"...Hiding-From-You"
```

### 🔑 Password

```text
xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
```

### 🧠 What I Learned

* `cd` changes the current directory.
* Hidden Linux files generally begin with `.`.
* `find` can search for files based on their names.
* `-type f` restricts the search to regular files.

### 📌 Key Takeaway

Hidden files may not appear in a normal directory listing, so searching specifically for filenames beginning with `.` can be useful.

---

# Level 4 → 5

### 🎯 Objective

Find the password stored in the only human-readable file inside the `inhere` directory.

### 🔎 Enumeration

I first entered the directory:

```bash
cd inhere
```

Then I listed its contents:

```bash
ls
```

There were several files:

```text
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

The challenge indicated that only one of these contained human-readable text.

### 💡 Solution

I identified the readable file and opened it using:

```bash
cat ./-file07
```

### 🔑 Password

```text
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
```

### 🧠 What I Learned

* `ls` lists files in a directory.
* `cat` displays file contents.
* Files beginning with `-` should be accessed carefully.
* `./` can be used to explicitly specify a filename.

### 📌 Key Takeaway

When several files are present and only one contains readable information, identifying the correct file is more efficient than randomly opening files.

---

# Level 5 → 6

### 🎯 Objective

Find the password stored somewhere under the `inhere` directory.

The file has these properties:

* Human-readable
* Exactly 1033 bytes
* Not executable

### 🔎 Enumeration

Instead of manually checking all the directories and files, I used the `find` command.

### 💡 Solution

I used:

```bash
find . -type f -size 1033c ! -executable
```

The command can be understood as:

* `find .` — search from the current directory.
* `-type f` — search only regular files.
* `-size 1033c` — find files exactly 1033 bytes in size.
* `! -executable` — exclude executable files.

The command identified:

```text
./maybehere07/.file2
```

I then read the file:

```bash
cat ./maybehere07/.file2
```

### 🔑 Password

```text
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
```

### 🧠 What I Learned

* `find` can search recursively.
* `-size` can filter files according to their size.
* `! -executable` excludes executable files.
* Using multiple conditions makes searching much faster.

### 📌 Key Takeaway

When a challenge provides specific file properties, `find` can combine those properties and locate the required file efficiently.

---

# Level 6 → 7

### 🎯 Objective

Find the password somewhere on the server.

The file has the following properties:

* Owned by user `bandit7`
* Owned by group `bandit6`
* Exactly 33 bytes in size

### 🔎 Enumeration

Since the challenge said the file was located **somewhere on the server**, I searched from the root directory `/`.

### 💡 Solution

I used:

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

The command searches the entire filesystem.

The important parts are:

* `-user bandit7` — file owner must be `bandit7`.
* `-group bandit6` — group owner must be `bandit6`.
* `-size 33c` — file must be exactly 33 bytes.
* `2>/dev/null` — hides permission-denied error messages.

The required file was:

```text
/var/lib/dpkg/info/bandit7.password
```

I read it using:

```bash
cat /var/lib/dpkg/info/bandit7.password
```

### 🔑 Password

```text
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
```

### 🧠 What I Learned

* `/` represents the root of the filesystem.
* `find` can search using ownership and size.
* `-user` checks file ownership.
* `-group` checks group ownership.
* `2>/dev/null` suppresses error messages.

### 📌 Key Takeaway

When a file can be located anywhere on a Linux system, start the `find` search from `/` and use the properties provided by the challenge.

---

# Level 7 → 8

### 🎯 Objective

The password is stored in `data.txt` next to the word `millionth`.

### 🔎 Enumeration

I listed the files:

```bash
ls
```

The required file was:

```text
data.txt
```

Because the file contained many lines, manually searching through it would be inefficient.

### 💡 Solution

I searched for the keyword `millionth` using:

```bash
grep millionth data.txt
```

This returned the line containing the password.

### 🔑 Password

```text
VR1ljMayciFxbnUokuQmJFw6QC9VKtub
```

### 🧠 What I Learned

* `grep` searches for specific text.
* Searching with a known keyword is faster than manually reading a large file.
* Passwords must be entered exactly; even an extra space can cause authentication to fail.

### 📌 Key Takeaway

`grep` is extremely useful when a challenge gives you a specific word or pattern to search for.

---

# Level 8 → 9

### 🎯 Objective

The password is stored in `data.txt` and is the only line that occurs exactly once.

### 🔎 Enumeration

The file contained many repeated lines.

Simply using `uniq` would not be enough because `uniq` only detects adjacent duplicate lines.

### 💡 Solution

I first sorted the file and then searched for the unique line:

```bash
sort data.txt | uniq -u
```

Here:

* `sort data.txt` sorts all lines.
* `|` passes the output to the next command.
* `uniq -u` displays lines that occur only once.

### 🔑 Password

```text
EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
```

### 🧠 What I Learned

* `sort` sorts lines alphabetically.
* `uniq` removes or identifies duplicate lines.
* `uniq` works properly for duplicates when identical lines are adjacent.
* Pipes allow commands to work together.

### 📌 Key Takeaway

The combination:

```bash
sort data.txt | uniq -u
```

is useful for finding a line that occurs only once in a file.

---

# Level 9 → 10

### 🎯 Objective

Find the password in `data.txt`.

The password is contained in one of the few human-readable strings and is preceded by several `=` characters.

### 🔎 Enumeration

I checked the file:

```bash
ls
```

The file was:

```text
data.txt
```

Since the file contained non-readable data, using `cat` was not the best approach.

### 💡 Solution

I used `strings` to extract readable text and piped the result into `grep`:

```bash
strings data.txt | grep "=="
```

The relevant output contained:

```text
========== B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
```

### 🔑 Password

```text
B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
```

### 🧠 What I Learned

* `strings` extracts human-readable strings from binary data.
* `grep` can filter the output.
* Combining commands using `|` makes searching easier.

### 📌 Key Takeaway

For files containing binary or unreadable data, `strings` can reveal useful text that can then be filtered using `grep`.

---

# Level 10 → 11

### 🎯 Objective

The password is stored in `data.txt`, but the contents are Base64 encoded.

### 🔎 Enumeration

I listed the file:

```bash
ls
```

Then checked its contents:

```bash
cat data.txt
```

The content appeared to be Base64 encoded.

### 💡 Solution

I decoded the file using:

```bash
base64 -d data.txt
```

The decoded output revealed the password.

### 🔑 Password

```text
pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
```

### 🧠 What I Learned

* Base64 is an encoding method.
* `base64 -d` decodes Base64 data.
* Encoding and encryption are different concepts.

### 📌 Key Takeaway

When data is Base64 encoded, the `base64 -d` command can be used to decode it.

---

# Level 11 → 12

### 🎯 Objective

The password is stored in `data.txt`, but all uppercase and lowercase letters have been rotated by 13 positions.

This is known as **ROT13**.

### 🔎 Enumeration

I listed the file:

```bash
ls
```

Then viewed the contents:

```bash
cat data.txt
```

The text was not immediately readable.

### 💡 Solution

I used `tr` to reverse the ROT13 transformation:

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

The output revealed the password.

### 🔑 Password

```text
GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```

### 🧠 What I Learned

* `tr` can translate characters.
* ROT13 rotates alphabetic characters by 13 positions.
* A pipe can pass file contents into another command.

### 📌 Key Takeaway

Linux text-processing commands such as `tr` can be used to reverse simple character substitutions.

---

# Level 12 → 13

### 🎯 Objective

The password is stored in `data.txt`.

The file is a **hexdump of a repeatedly compressed file**.

### 🔎 Step 1 — Create a Working Directory

I created a temporary directory:

```bash
mkdir /tmp/bandit12work
cd /tmp/bandit12work
```

Then copied the original file:

```bash
cp ~/data.txt .
```

### 💡 Step 2 — Reverse the Hexdump

I converted the hexdump back into binary data:

```bash
xxd -r data.txt > data
```

Then checked its type:

```bash
file data
```

The file was identified as gzip compressed data.

### Step 3 — Extract the Compressed Data

I renamed the file:

```bash
mv data data.gz
```

Then extracted it:

```bash
gunzip data.gz
```

I repeatedly used:

```bash
file data
```

to identify the next compression format.

Depending on the format, I used commands such as:

```bash
bunzip2
gunzip
tar -xf
```

For example:

```bash
mv data data.bz2
bunzip2 data.bz2
```

and:

```bash
mv data data.tar
tar -xf data.tar
```

The extraction process continued through several layers.

Eventually, the final readable file was:

```text
data8.bin
```

I checked it:

```bash
file data8.bin
```

It was ASCII text.

Finally:

```bash
cat data8.bin
```

### 🔑 Password

```text
qQYQiHOBPR8zR61qxYqX45quvihF2uzk
```

### 🧠 What I Learned

* `xxd -r` reverses a hexdump.
* `file` identifies the actual type of a file.
* `gunzip` extracts gzip files.
* `bunzip2` extracts bzip2 files.
* `tar -xf` extracts TAR archives.
* File extensions do not always tell you the actual file type.

### 📌 Key Takeaway

The most important command in this level was:

```bash
file data
```

After every extraction, I checked the file type and selected the correct extraction command.

---

# Level 13 → 14

### 🎯 Objective

The password for Level 14 is stored in:

```text
/etc/bandit_pass/bandit14
```

However, only the `bandit14` user can read it.

Instead of receiving a password directly, Level 13 provides an SSH private key.

### 🔎 Enumeration

I listed the files:

```bash
ls
```

The important file was:

```text
sshkey.private
```

I viewed the private key:

```bash
cat sshkey.private
```

### 💡 Solution

I copied the private key to my local machine and saved it as:

```text
bandit14.key
```

I then restricted its permissions:

```bash
chmod 600 bandit14.key
```

Finally, I connected to Level 14 using the private key:

```bash
ssh -i bandit14.key bandit14@bandit.labs.overthewire.org -p 2220
```

After logging in:

```bash
cat /etc/bandit_pass/bandit14
```

This displayed the password.

### 🔑 Password

```text
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

### 🧠 What I Learned

* SSH supports key-based authentication.
* A private key can be supplied using `-i`.
* `chmod 600` restricts the private key so that it is not publicly accessible.
* SSH keys can be used instead of passwords.

### 📌 Key Takeaway

The important concept in this level is **SSH private-key authentication**.

The general command is:

```bash
ssh -i <private-key> <user>@<host> -p <port>
```

---

# Level 14 → 15

### 🎯 Objective

Submit the current password to a service running on:

```text
localhost:30000
```

The service returns the password for Level 15.

### 🔎 Enumeration

I first obtained the current password:

```bash
cat /etc/bandit_pass/bandit14
```

### 💡 Solution

I connected to port `30000` using Netcat:

```bash
nc localhost 30000
```

I entered the current password:

```text
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

The server responded with:

```text
Correct!
```

and provided the next password.

An alternative way is:

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

### 🔑 Password

```text
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

### 🧠 What I Learned

* `nc` stands for Netcat.
* Netcat can communicate with network services.
* `localhost` refers to the current machine.
* A pipe can send command output directly to another program.

### 📌 Key Takeaway

Netcat is useful for interacting with services running on specific ports.

---

# Level 15 → 16

### 🎯 Objective

Submit the current password to a service running on port `30001` on localhost.

The service uses **SSL/TLS**.

### 🔎 Enumeration

I first obtained the current password:

```bash
cat /etc/bandit_pass/bandit15
```

The password was:

```text
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

### 💡 Solution

Because the service uses SSL/TLS, I used OpenSSL rather than normal Netcat:

```bash
echo "pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7" | openssl s_client -connect localhost:30001 -quiet
```

The server returned the password for Level 16.

### 🔑 Password

```text
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

### 🧠 What I Learned

* `openssl s_client` can establish an SSL/TLS connection.
* Some services require encrypted communication.
* Netcat alone is not sufficient when the server expects SSL/TLS.

### 📌 Key Takeaway

When a service requires SSL/TLS, `openssl s_client` can be used to establish the connection.

---

# Level 16 → 17

### 🎯 Objective

Find the correct service running on a port between `31000` and `32000`.

The correct service will provide the credentials for Level 17.

### 🔎 Enumeration

The challenge required finding which ports had servers listening and then determining which services supported SSL/TLS.

I used port scanning to identify the available services.

The correct SSL service was on port:

```text
31790
```

### 💡 Solution

I submitted the current password to the SSL service:

```bash
echo "$(cat /etc/bandit_pass/bandit16)" | openssl s_client -connect localhost:31790 -quiet 2>/dev/null
```

The server responded:

```text
Correct!
```

and provided an OpenSSH private key.

I saved the private key and used it to authenticate as `bandit17`.

### 🔑 Credentials

The server returned an **OpenSSH private key** for authentication to Level 17.

I used the key with:

```bash
ssh -i <private-key> bandit17@bandit.labs.overthewire.org -p 2220
```

### 🧠 What I Learned

* Port scanning can identify available network services.
* SSL/TLS services can be tested using OpenSSL.
* SSH private keys can be returned by network services.
* `2>/dev/null` can hide unwanted error output.

### 📌 Key Takeaway

This level combines **port scanning, SSL/TLS communication and SSH key-based authentication**.

---

# Level 17 → 18

### 🎯 Objective

Two files are provided:

```text
passwords.old
passwords.new
```

The password is the line that changed between the two files.

### 🔎 Enumeration

I listed the files:

```bash
ls
```

The output showed:

```text
passwords.new
passwords.old
```

### 💡 Solution

I used the `diff` command:

```bash
diff passwords.old passwords.new
```

The output showed:

```text
42c42
< qOg5pVOjPx9x9VccyYBADiT4xxyoUB8D
---
> OQxXZjELndr90zuhOTDYBEomI0SZITXI
```

The line beginning with `>` belongs to `passwords.new`, so it is the new password.

### 🔑 Password

```text
OQxXZjELndr90zuhOTDYBEomI0SZITXI
```

### 🧠 What I Learned

* `diff` compares two files.
* It can quickly identify changed lines.
* `<` indicates content from the first file.
* `>` indicates content from the second file.

### 📌 Key Takeaway

When two files are almost identical, `diff` is a simple and powerful way to identify what changed.

---

# Level 18 → 19

### 🎯 Objective

Log into Level 18 and retrieve the password for Level 19.

### 🔎 Enumeration

The password I obtained for this level was:

```text
KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
```

The level has a modified shell configuration that causes the SSH session to terminate immediately.

### 💡 Solution

Instead of relying on an interactive shell, the solution is to execute the required command directly through SSH.

For example:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

This executes `cat readme` directly on the remote system.

The contents of `readme` provide the password for Level 19.

### 🔑 Password

```text
KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
```

### 🧠 What I Learned

* SSH can execute commands directly on a remote system.
* An interactive shell is not always necessary.
* Shell configuration can affect how an SSH session behaves.
* Remote commands can be enclosed in quotes.

### 📌 Key Takeaway

If an interactive SSH shell immediately closes, executing a command directly through SSH can still allow access to required files.

---

# Level 19 → 20

### 🎯 Objective

Use the provided SUID program to access the password for Level 20.

### 🔎 Enumeration

I listed the files with their permissions:

```bash
ls -l
```

The important file was:

```text
-rwsr-x--- 1 bandit20 bandit19 14880 Jun 24 14:58 bandit20-do
```

The `s` in the permission string indicates that the file has the **SUID** permission set.

### 💡 Solution

I executed the program:

```bash
./bandit20-do
```

It displayed:

```text
Run a command as another user.
Example: ./bandit20-do whoami
```

I tested which user the command executes as:

```bash
./bandit20-do whoami
```

It runs commands as:

```text
bandit20
```

Therefore, I could use it to read the Level 20 password:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

### 🔑 Password

```text
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```

### 🧠 What I Learned

* SUID allows an executable to run with the privileges of its owner.
* `ls -l` displays file permissions.
* `whoami` shows the current effective user.
* Linux file permissions are an important part of system security.

### 📌 Key Takeaway

The main concept in this level is **SUID**.

The program:

```bash
./bandit20-do
```

allows commands to be executed with `bandit20` privileges, which makes it possible to read the password file.

