# Chapter 4 : The file permissions {#chapter-4}

Now we have seen how do Linux organizes its filesystem, let's see how it manages the permissions of the files and folders.

It won't be a surprise, Linux's file permissions are also inherited from Unix. The file permissions are one of the core features of Linux's security model : determine who can access to what and what they can do on it. Of course, and the security parenthesis later will confirm that, the file permissions are just a component among several other ways to secure a Linux installation.

## How to see the file permissions {#chapter-4-how-to-see-the-file-permissions}

Let's list my `/etc` folder content. This time, I use the `ls -l` command that will display the files metadata.

```bash
drwxr-xr-x.  3 root        root         4096 Mar 10  2022 abrt
-rw-r--r--.  1 root        root           16 Feb  4  2020 adjtime
```
In this result, I have to types of files : a directory, and a file. How can I see the difference ? Here is how to read this output.

For the `adjtime` file, we have the following elements that interest us in this particular case :

- `-rw-r--r–.` : This attribute can be split into :
    - `-` : The file type. In this case, it's a regular file.
    - `rw-r--r-` : The permissions settings
    - `.` : The extended attributes
- `root` : the user owning the file
- `root` : the group owning the file

For the `abrt` file, you may had noticed a difference with the first character : `d`. That means it's a directory. You may also noticed that the permissions are different.

Remember : for Unix, everything's a file ! No matter if its a directory or a text file or a hard drive or an executable program.


## How to read the file permissions {#chapter-4-how-to-read-the-file-permissions}

In our previous example, we had two different set of permissions for a file : `rwxr-xr-x` and `rw-r--r--`. These strings are actually composed of three blocks : 

- For the `abrt` directory :
    - `rwx`
    - `r-x`
    - `r-x`
- For the `adjtime` file :
    - `rw-`
    - `r--`
    - `r--`

These three blocks stand for, in this order :

- The permissions for the user owning the file (**owner**)
- The permissions for the group owning the file (**group**)
- The permissions for the other users (which are different of the owner, and not in the group - **others**)

Now, we have letters, what do they mean ?

- `r` : Read
- `w` : Write
- `x` : Execute

If the letter is present, the permission is granted for the related set of permissions (owner/group/others). If not, the permission is replaced by the `-` symbol.

When a user requests an access to a file, the system will check in the following order if the permissions allow it :

1. Check if the user is the owner of the file. If so, no other checks will be made.
2. If you're not the owner of the file, the system will check if you're a member of the group owning the file. If that's the case, you will have access to it according to the group's permissions
3. If you're neither the owner or belonging to the group, the system will use the others permissions.

The three permissions sets are mutually exclusive.

The permissions grant a specific access the file :

- Read (`r`) will authorize the file's content access. Tools like `cat`, `less` or `vim` are able to open it and display it's content. However, it's impossible to modify its name or its content. Read permission is also required to be able to copy a file. For a directory, this permission allows the usage of the `ls` command and its counterparts to display the content.
- Write (`w`) will authorize to rename or modify the file's content. If this attribute is set to a directory, it means the user can create, modify, or delete a file present inside this directory. Write is also required in order to be able to use the shell's redirection operations (`>` or `>>`) to a file.
- Execute (`x`) will authorize the execution of the content of the file like a program. Each executable binary in the system has this permission. Also, this permission is required in order to be able to navigate into a folder with the `cd` command.
    - Please note that the execute permission is not mandatory for scripts such as Bash or Python scripts, because you can run them by using their interpreter which is an executable file itself : `bash script.sh` or `python script.py`.

Let's show an example, I've wrote a file name `file`.

```bash
$ ls -l file
-rw-r--r--. 1 seb  seb     6 Feb  8 22:41 file
# my user and group are owner, and the others can read.
$ echo something >> file
$ ls -l file
-rw-r--r--. 1 seb  seb    16 Feb  8 22:42 file
# The content size changed.
# I've changed the owner, now it's root.
$ ls -l file
-rw-r--r--. 1 root root   16 Feb  8 22:42 file
$ echo somethingelse >> file
zsh: permission denied: file
# I can't write anymore in this file.
$ cat file
pouet
something
# but I can still read it
# I remove the Read access to Others
$ ls -l file
-rw-r-----. 1 root root   16 Feb  8 22:42 file
# Let's try to read it
$ cat file
cat: file: Permission denied
# :(
```

## A security parenthesis {#chapter-4-a-security-parenthesis}

A little note about Linux's security. A common mistake is usually to believe that removing the Execute attribute to a file will protect the system from uncontrolled script executions. But like we said, using the interpreter executable command will make this idea irrelevant for an interpreted programmation language such as the shell, Python, Perl or Java. If you want to prevent the execution of commands and script from a specific filesystem (typically : the home users directory), you need to apply a specific flag to the mountpoint : `noexec`. Of course, it's just one possibility among a lot of other one in order to secure a Linux installation.

Another common mistake is to believe that thanks to its file permissions system, Linux is "more secure" than Windows, but that's terribly wrong. Linux is today as much targeted by malwares[^linuxmalwares] as Windows, but the attack patterns are different. So this reminder seems important to me :

**A Linux Distribution is not more or less secured by design than any other operating system, and especially not out of box. Securing and hardening a Linux installation is a specific work requiring advanced competences in matter of system administration and IT security. And more important : Linux is not foolproof !**

Additionally, I'll complete with another very despicable practice I can still see today… Applying the full permissions access to a file. If you read any tutorial or advice telling you "use `chmod 777 file`", don't do that unless you fully understand, and have challenged, this requirement. In other cases, that's a terrible practice. It's basically giving an open bar access to the content without any control over it.

## The octal values for permissions {#chapter-4-the-octal-value-permissions}

The example about the bad practice is a nice transition to a second way to read (and specify) your filesystem's permissions : the octal value. Instead of using the letters notation (r, w, x), you use numbers : respectively 4, 2, 1. 

- Read : `r` = `4`
- Write : `w` = `2`
- Execute : `x` = `1`

In the bad example I've exposed just above, I've used the command `chmod`. This command changes the permissions of a file, and in this case, I've used the octal notation. `777` = `rwxrwxrwx` here. The three numbers represent the Owner, Group, and Others just like the three `rwx` groups.

If we recall our first examples with the `adjtime` file and `abrt` folder, here is the correspondance of their permissions :

- `abrt` folder : 
    - Letters notation : `rwxr-xr-x`
    - Octal notation : `755`
- `adjtime` file :
    - Letters notation : `rw-r--r--`
    - Octal notation : `644`

Each permission set is the addition of the three octal values :

- `rwx` = 4 + 2 + 1 = 7
- `rw-` = 4 + 2 + 0 = 6
- `r-x` = 4 + 0 + 1 = 5
- `r--` = 4 + 0 + 0 = 4
- `---` = 0 + 0 + 0 = 0

So, if I use a concrete application of the command `chmod` :

- Give full permissions to Owner, Read to Group, Nothing to Others : 
    - Octal notation : `chmod 740 file`
    - Letters notation :
        - `chmod u+rwx file`
        - `chmod g+r-w file`
        - `chmod o-rwx file`

And that's why I prefer the octal notation, it's easier to apply. Once applied, we have the following result :

```bash
$ ls file
-rwxr-----. 1 seb  seb    16 Feb  8 22:42 file
# Want to display the octal notation ? use the 'stat' command
# And check "Access"
$ stat file
  File: file
  Size: 16        	Blocks: 8          IO Block: 4096   regular file
Device: 0,33	Inode: 2013        Links: 1
Access: (0740/-rwxr-----)  Uid: ( 1000/     seb)   Gid: ( 1000/     seb)

# I'm curious, what is the Access on a folder ?
$ stat snap-private-tmp
  File: snap-private-tmp
  Size: 40        	Blocks: 0          IO Block: 4096   directory
Device: 0,33	Inode: 2           Links: 2
Access: (0700/drwx------)  Uid: (    0/    root)   Gid: (    0/    root)
```

If you read these two `stat` output you may want to recall the `d` letter before the permission suggesting a folder. For `snap-private-tmp` yes, we have it : `drwx------`. This folder has very restrictive accesses : only `root` can read, write and access to its content, nobody else.

But if you read carefully both of these outputs you may possibly ask : 

> Hey, why is there a `0` before `740` and `700` ? Is that the octal value for the directories ?

Nope, that's something else :

## The special file permissions {#chapter-4-the-special-file-permissions}

Actually, the octal notation is on 4 numbers, not 3. The first digit is here to apply the special file permission. These special permissions grant additional privileges to the files and directories :

- SUID (Set User ID) : this special permission is set for the Owner access level. If the file is executable, it will always be executed as the user owning the file, regardless of the current user passing the command.
- SGID (Set Group ID) : this special permission has two possibilities :
    - If set on a regular file, the result will be the same as the SGID. The execution of the file will have the same permissions as the Group's permission applied on it.
    - If set on a directory, any files created in it that have the same group's ownership as the directory owning them

A very good example of the SUID usage is the `sudo` command. `sudo` is an administrative tool that permit an authorized user to impersonate another identity on the system. To be able to work, the command uses the SUID and is owned by `root`. Since `root` can switch to any other user with no password prompt, this command will temporary grants a `root` permission to the user invoking it.

```bash
$ stat /usr/bin/sudo                                                                                           
  File: /usr/bin/sudo
  Size: 202328    	Blocks: 400        IO Block: 4096   regular file
Device: 253,0	Inode: 2497570     Links: 1
Access: (4111/---s--x--x)  Uid: (    0/    root)   Gid: (    0/    root)

```

`sudo` has very interesting permissions : `411`

- The owner cannot read the file's content
- The group owner cannot read it tool
- ... Neither the others

But these permissions grant the possibility to execute the command for anybody. And because of the presence of the SUID flag (`4`), the command will always be executed with `root`'s permissions.

Example :

```bash
# Using the whoami command, that displays
# the current user's name
$ whoami
seb
# by default sudo switches to root
$ sudo whoami
root
# but you can specify a username
$ sudo -u apache whoami
apache
# that's the magic of the SUID
```

I'm sure you have a question here :

> Mmmh how can the file permissions secure Linux if a command can allow anybody to become super admin ?!

Good question : `sudo` is not a dumb command. It relies on a list of allowed users with the commands they can perform as another one, the `sudoers`. So if the current user is not in the `sudoers`, `sudo` will reject the input and report the incident[^reportsudo].

Now let's make an example for the SGID, which ensure the files created in a directory will always have the directory's permissions whoever is writing them.

```bash
# I have created two directories
# One "sgid" owned by me and the "root" group, with the SGID
# One "nosgid" owned by me and the "root" group, with no SGID
# The Others has no permissions
$ ls -l
drwxrws---. 2 seb  root   60 Feb  9 23:01  sgid
drwxrwx---. 2 seb  root   40 Feb  9 23:01  nosgid
# I create a file in the nosgid folder using the "touch" command
$ touch nosgid/test
$ ls -l nosgid/test
-rw-r--r--. 1 seb seb 0 Feb  9 23:04 nosgid/test
# Me and my group are owner
# Now let's try in the sgid folder
$ touch sgid/test
$ ls -l sgid/test
-rw-r--r--. 1 seb root 0 Feb  9 23:06 sgid/test
# The group owning the file is root's, not mine
```

The special permissions have both the letter and octal notation, just like the regular one.

- No special permission : `-` = `0` (`0` in `0755`)
- SUID : `s` (at owner level) = `4` (`4` in `4111`)
- SGID : `s` (at group level) = `2` (`2` in `2770`)
- Sticky : `t` (at others level) = `1` (`1` in `1755`)

We will explain Sticky in the next step.

The permissions I've used in the example above have been set like this :

- Letters notation :
    - `chmod u+rwx sgid`
    - `chmod g+rws sgid`
    - `chmod o-rwx sgid`
- Octal notation :
    - `chmod 2770 sgid`

### Another special permission, the Sticky Bit {#chapter-4-another-special-permission-the-sticky-bit}

The Sticky bit is set at the Others level and is different than the two previous special ones because it doesn't affect the files themselves. Set at the directory level, the sticky bit restricts the file deletion inside this directory. Only the **Owner of a file** is able to delete it inside this directory. And of course, `root` is also capable of it because it can do anything.

Basically, `root` doesn't care about the file permissions. 

The sticky bit is noted with the `T` letter at the Others level. It's octal value is `1`.

Example in application :

```bash
# I've created a "sticky" folder with permissions 1770
# And a "nosticky" folder with permissions 0770
# apache is the group owner of the folder
$ ls -l
drwxrwx--T. 2 seb  apache    40 Feb  9 23:19  sticky
drwxrwx---. 2 seb  apache   60 Feb  9 23:28  nosticky
# I've been able to create a file inside
# apache too.
$ ls -l *sticky
nosticky:
total 0
-rw-r--r--. 1 apache apache 0 Feb  9 23:30 file-apache
-rw-r--r--. 1 seb    seb    0 Feb  9 23:30 file-seb

sticky:
total 0
-rw-r--r--. 1 apache apache 0 Feb  9 23:30 file-apache
-rw-r--r--. 1 seb    seb    0 Feb  9 23:30 file-seb

# apache as full access to the directory thanks to the group
# this user can delete a file owned by somebody else
$ sudo -u apache rm nosticky/file-seb 
rm: remove write-protected regular empty file 'nosticky/file-seb'? y
# but in the folder protected by the sticky bit,
# despite having a full access, apache is denied from 
# removing a file owned by seb
$ sudo -u apache rm sticky/file-seb
rm: remove write-protected regular empty file 'sticky/file-seb'? y
rm: cannot remove 'sticky/file-seb': Operation not permitted
```


[^linuxmalwares]: Linux and the malwares https://blog.zedas.fr/posts/linux-and-the-malwares/

[^reportsudo]: This incident will be reported xkcd https://xkcd.com/838/

