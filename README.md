# PastejackingProtection
A bash script/function against pastejacking. If you don't know what pastejacking is, check these links:

* https://github.com/dxa4481/Pastejacking
* http://www.ush.it/team/ascii/hack-tricks_253C_CCC2008/wysinwyc/what_you_see_is_not_what_you_copy.txt
* https://thejh.net/misc/website-terminal-copy-paste (good demo)

Add the following bash script to your `~/.bashrc`:

```bash
function pp {
    xclip -o | cat -A
    echo -ne "\nExecute? (y/n): "
    read execute
    # only execute when 'y' was answered
    # all other input is ignored
    if [[ $execute == "y" ]]; then
        eval `xclip -o`
    fi
}
```

It shows the clipboard to you and gives you the option to execute the contents if you press `y`. Other input is ignored. It also prints special characters.

# Example usage

Using the demo from the first link, we get:

```
aaron@aaron-pc:~$ pp
echo "evil"$

Execute? (y/n): n
aaron@aaron-pc:~$
```

```
aaron@aaron-pc:~$ pp
echo "evil"$

Execute? (y/n): y
evil
aaron@aaron-pc:~$ 
```

That demo uses Javascript, but CSS can be used as well and NoScript can't protect you from that. The 3rd link is an example of that. You expected `git clone git://git.kernel.org/pub/scm/utils/kup/kup.git` but got:

```
aaron@aaron-pc:~$ pp
git clone /dev/null; clear; echo -n "Hello ";whoami|tr -d '\n';echo -e '!\nThat was a bad idea. Don'"'"'t copy code from websites you don'"'"'t trust!$
Here'"'"'s the first line of your /etc/passwd: ';head -n1 /etc/passwd$
git clone git://git.kernel.org/pub/scm/utils/kup/kup.git
Execute? (y/n): n
aaron@aaron-pc:~$
```


The second link has a much bigger script that you can test it on, but it's too big for this purpose to paste it here.

# Requirements

* `xclip` needs to be installed

Tested on Ubuntu 15.04 and 16.04
