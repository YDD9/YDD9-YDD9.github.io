# Add a default path for bash in linux

The simple stuff   
https://unix.stackexchange.com/questions/26047/how-to-correctly-add-a-path-to-path?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa
```
export PATH=$PATH:~/opt/bin
export PATH=~/opt/bin:$PATH
```
depending on whether you want to add `~/opt/bin` at the end (to be searched after all other directories, in case there is a program by the same name in multiple directories) or at the beginning (to be searched before all other directories).
You can add multiple entries at the same time. `PATH=$PATH:~/opt/bin:~/opt/node/bin` or variations on the ordering work just fine.

You don't need export if the variable is already in the environment.



https://superuser.com/questions/183870/difference-between-bashrc-and-bash-profile/183980#183980
Bash is a Bourne-like shell. It reads commands from `~/.bash_profile` when it is invoked as the login shell, and if that file doesn't exist, it tries reading `~/.profile` instead.

You can invoke a shell directly at any time, for example by launching a terminal emulator inside a GUI environment. If the shell is not [a login shell](https://unix.stackexchange.com/questions/38175/difference-between-login-shell-and-non-login-shell?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa), it doesn't read `~/.profile`. **When you start bash as an interactive shell** (i.e., not to run a script), it reads `~/.bashrc` (except when invoked as a login shell, then it only reads `~/.bash_profile` or `~/.profile`.

Therefore:

`~/.profile` is the place to put stuff that applies to your whole session, such as programs that you want to start when you log in (but not graphical programs, they go into a different file), and environment variable definitions.

`~/.bashrc` is the place to put stuff that applies only to bash itself, such as alias and function definitions, shell options, and prompt settings. (You could also put key bindings there, but for bash they normally go into `~/.inputrc`.)

`~/.bash_profile` can be used instead of `~/.profile`, but it is read by bash only, not by any other shell. (This is mostly a concern if you want your initialization files to work on multiple machines and your login shell isn't bash on all of them.) This is a logical place to include `~/.bashrc` if the shell is interactive. I recommend the following contents in `~/.bash_profile`:
```
if [ -r ~/.profile ]; then . ~/.profile; fi
case "$-" in *i*) if [ -r ~/.bashrc ]; then . ~/.bashrc; fi;; esac
```
