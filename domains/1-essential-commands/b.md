## Search for files

Find files with depth 3 and size above 2 mb:
`find . -maxdepth 3 -type f -size +2M`

Find files with permission 777 and remove them:
`find /home/user -perm 777 -exec rm '{}' +`

> Using `-exec` with a semicolon (eg. "find . -exec ls '{}' \;"), will execute the command separately for each argument passed, while using a plus sign instead (e.g "find . -exec ls '{}' +"), as many arguments as possible are passed to a single command: if the number of arguments exceeds the system's maximum command line length, the command will be called multiple times.

Find files based on how many times they have been accessed (`-atime n`) or modified (`-mtime n`) (with `n` = n*24 hours ago):
`find /etc -iname "*.conf" -mtime -180 –print`

To combine two conditions:
`find . \( -name name1 -o -name name2 \)`

To negate a condition:
`find . \! -user owner`

To search for a filename ignoring the case:
`find . -iname name`

Find all files with at least (`-` sign before `g` in this example) permission write for group:
`find . -perm -g=w`

Boolean conditions for searching by permission mode:
- `-perm mode`: file's permission bits are exactly mode (octal or symbolic)
- `-perm -mode`: all of the permission bits mode are set for the file
- `-perm /mode`: any of the permission bits mode are set for the file.

Audit a system to find files with root SUID/SGID:
`sudo find / -user root \( -perm 4000 -o -perm 2000 \) -print`

Remove a file by inode:
`find . -maxdepth 1 -type f -inum 7404301 -delete`

Combine find and grep:
`find . -name "*.md" -exec grep -Hni --color=always inode {} \;`

Alternatively, you can use the `locate` command, which searches for a given pattern through a database file that is generated by the `updatedb` command. The found results are displayed on the screen, one per line.

During the installation of the `mlocate` package, a cron job is created that runs the updatedb command every 24 hours. This ensures the database is regularly updated. For more information about the cron job check the /etc/cron.daily/mlocate file.

The syntax for the locate command is as follows:
`locate [OPTION] PATTERN`

For example to search for a file named *.bashrc* you would type:
`locate .bashrc`

Compared to the more powerful find command that searches the file system, locate operates much faster but can search only for entries present in its cache.

---

### File Globbing

*File globbing* is a feature provided by the UNIX/Linux shell to represent multiple filenames by using special characters called wildcards with a single file name. Long ago, in UNIX V6, there was a program /etc/glob that would expand wildcard patterns. Soon afterward this became a shell built-in.

A wildcard is essentially a symbol which may be used to substitute for one or more characters. Therefore, we can use wildcards for generating the appropriate combination of file names as per our requirement.

Use `*` to mean "every character": `ls -l a*`

Use `?` for a single character: `ls -l a?`

Use square brackets to declare a set of characters: `ls -l a[ab]`

Wildcards can be combined: `ls -l a[a-c]*`

Use curly braces to consider any listed element: `mkdir /etc/{public,private,protected}`

`!` is used to exclude characters from the list that is specified within the square brackets: `ls /lib[!x]*`

> Note: Beware that the syntax for excluding specific characters is slightly different with regex: you have to use the square brackets and the ^ (hat). For example, the pattern `[^abc]` will match any single character except for the letters `a`, `b`, or `c`.

Named character classes ([[:named:]]) are used inside brackets to represent an entire class of chars. Their interpretation depends on the LC_CTYPE locale. Some of them are listed below:

- ‘[:alnum:]’, prints all those files having alphabets and digits, both lower and uppercases are considered.
- ‘[:alpha:]’, prints all those files having alphabets only, both lower and uppercases are considered.
- ‘[:digit:]’, prints all those files having digits.
- ‘[:lower:]’, prints all those files having lower-case letters.
- ‘[:punct:]’, prints all those files having punctuation characters, will search for ! ” # $ % & ‘ ( ) * + , – . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~.
- ‘[:space:]’, prints all those files having space characters.
- ‘[:upper:]’, prints all those files having lower-case letters.