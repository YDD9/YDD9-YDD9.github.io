---
layout: post
title:  "Git from zero!"
date:   2016-12-14 14:49:39 +0100
categories: jekyll update
---

Now you have git downloaded and ready to start.

# Table of contents
1. [Git config](#gitconfig)
2. [Git credential](#token)
3. [Adding an existing project to GitHub using the command line](#addproj)
4. [Ignore files or directories](#ignore)
5. [Checking the Status of Your Files](#statuscheck)
6. [Get git repository](#gitpull)  
7. [Go back to specific commit](#goback)  
8. [Git push succeed, remote not updating](missupdates)  



## Git config <a name="gitconfig"></a>
The first thing you should do when you install Git is to set your user name and email address.   
This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:   

```
$ git config --global user.name "John Doe"     
$ git config --global user.email johndoe@example.com     
```   

Again, you need to do this only once if you pass the --global option, because then Git will always use that information for 
anything you do on that system. If you want to override this with a different name or email address for specific projects, 
you can run the command without the --global option when you’re in that project.

Checking Your Settings
If you want to check your settings, you can use the git config --list command to list all the settings Git can find at that point:  

```
$ git config --list    
user.name=John Doe    
user.email=johndoe@example.com      
color.status=auto     
color.branch=auto     
color.interactive=auto     
color.diff=auto   
...   
```    

You may see keys more than once, because Git reads the same key from different files (/etc/gitconfig and ~/.gitconfig, for example).  
In this case, Git uses the last value for each unique key it sees. You can also check what Git thinks a specific key’s value is by typing git config <key>:   

```
$ git config user.name   
```  


## Git credential <a name="token"></a>

### Creating GitHub Personal Access Token

Of course, you can use your GitHub username/password to authenticate, but there is a better approach - Personal Access Tokens which:

Could be easily revoked from GitHub UI;
Have a limited scope;
Can be used as credentials with Git commands (this is what we need).
[Use this GitHub guide for creating access tokens.](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

### Enabling Git credential store

Git doesn’t preserve entered credentials between calls. However, it provides a mechanism for caching credentials called Credential Store. To enable credential store we use the following command:

```
git config --global credential.helper store
```

The git credential cache runs a daemon process which caches your credentials in memory and hands them out on demand. So killing your git-credential-cache--daemon process throws all these away and results in re-prompting you for your password if you continue to use this as the cache.helper option.

You could also disable use of the git credential cache using `git config --global --unset credential.helper`. Then reset this and you would continue to have the cached credentials available for other repositories (if any). You may also need to do `git config --system --unset credential.helper` if this has been set in the system config file (eg: Git for Windows 2).

On Windows you might be better off using the manager helper `git config --global credential.helper manager`. This stores your credentials in the Windows credential store which has a Control Panel interface where you can delete or edit your stored credentials. With this store, your details are secured by your Windows login and can persist over multiple sessions. The manager helper included in Git for Windows 2.x has replaced the earlier *wincred* helper that was added in Git for Windows 1.8.1.1. A similar helper called winstore is also available online and was used with GitExtensions as it offers a more GUI driven interface. The manager helper offers the same GUI interface as winstore.

Extract from Windows manual detailing the Windows credential store panel:

Open User Accounts by clicking the Start button Picture of the Start button, clicking Control Panel, clicking User Accounts and Family Safety (or clicking User Accounts, if you are connected to a network domain), and then clicking User Accounts. In the left pane, click Manage your credentials.
source from http://stackoverflow.com/questions/15381198/remove-credentials-from-git


## Adding an existing project to GitHub using the command line <a name="addproj"></a>
Create a new repository on GitHub. To avoid errors, do not initialize the new repository with README, license, 
or gitignore files. You can add these files after your project has been pushed to GitHub. 

1.Open Git Bash.   
2.Change the current working directory to your local project. (pwd, cd tab, dir)   
3.Initialize the local directory as a Git repository.   
```
git init   
```  
4.Add the files in your new local repository. This stages them for the first commit.    
```
git add .   
```  
5.Adds the files in the local repository and stages them for commit. To unstage a file, use `git reset HEAD YOUR-FILE`.   
Commit the files that you've staged in your local repository.   
```
git commit -m "First commit"   
```    
Commits the tracked changes and prepares them to be pushed to a remote repository. To remove this commit and modify the file,   
use `git reset --soft HEAD~1` and commit and add the file again.   
6.In the Command prompt, add the URL for the remote repository where your local repository will be pushed. 'orgin' is the name of the remote repo.   
```
git remote add origin remote repository URL   
```    
Display the added remote `git remote -v`   
7.Push the changes in your local repository to GitHub.   
```
git push origin master   
```    
Pushes the changes in your local repository up to the remote repository you specified as the origin    
8.Changing a remote's URL. The `git remote set-url` command takes two arguments:  
a). An existing remote name. For example, origin or upstream are two common choices.    
b). A new URL for the remote. For example:   
  If you're updating to use HTTPS, your URL might look like: `https://github.com/USERNAME/OTHERREPOSITORY.git`    
  If you're updating to use SSH, your URL might look like: `git@github.com:USERNAME/OTHERREPOSITORY.git`     
9.Delete remote URLs by their names. As a side note, it should be pointed that there is     
also `git remote rm <name>` that deletes every remotes matching the given name, whatever the URLs.    


## Ignore files or directories <a name="ignore"></a>  


Create a file named .gitignore in your projects directory. Ignore directories by entering the directory name into the file dir_to_ignore/ (with a slash appended) or by cmd:

```
echo filename ignore.sqlite3 >> .gitignore
echo filename.log >> .gitignore

# check status, remove .pyc files and add .pyc in ignore list
git status

git rm -r --cached superlists/superlists/__pycache__

echo __pycache__ >> .gitignore
echo *.pyc >> .gitignore
```

below sample ignore: folders, A type of files, Single file, A filename with space and dash

```
Datafiles/1/
.git
.vscode

*.pyc

env.json
exe_cmd.bat
rundeploy.txt
log.txt

Datafiles/cf\ commands\ -\ sps\ perf.txt
```


PS: 

.gitignore will only ignore files that you haven't already added to your repository.

If you did a `git add .`, and the file got added to the index, .gitignore won't help you. You'll need to do `git rm -r -cached sites/default/settings.php` to remove it, and then it will be ignored. You can have a dry run `git add -n .`






## Checking the Status of Your Files <a name="statuscheck"></a>
The main tool you use to determine which files are in which state is the git status command.    
If you run this command directly after a clone, you should see something like this:    

```
$ git status   
On branch master   
Your branch is up-to-date with 'origin/master'.    
nothing to commit, working directory clean    
```   

This means you have a clean working directory – in other words, there are no tracked and modified files. Git also doesn’t see any untracked files,    
or they would be listed here. Finally, the command tells you which branch you’re on and informs you that it has not diverged from the same branch     
on the server. For now, that branch is always “master”, which is the default; you won’t worry about it here. Git Branching will go over branches and references in detail.    

To see what you’ve changed but not yet staged, type git diff with no other arguments:     

```
$ git diff   
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md    
index 8ebb991..643e24f 100644     
--- a/CONTRIBUTING.md    
+++ b/CONTRIBUTING.md     
@@ -65,7 +65,8 @@ branch directly, things can get messy.       
Please include a nice description of your changes when you submit your PR;      
if we have to read the whole diff to figure out why you're contributing    
in the first place, you're less likely to get feedback and have your changed     
-merged in.     
+merged in. Also, split your changes into comprehensive chunks if your patch is    
+longer than a dozen lines.    
```      

That command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged.     
If you want to see what you’ve staged that will go into your next commit, you can use `git diff --staged`. This command compares your staged changes to your last commit:       

```
$ git diff --staged     
diff --git a/README b/README    
new file mode 100644    
index 0000000..03902a1    
--- /dev/null    
+++ b/README    
@@ -0,0 +1 @@    
+My Project    
```    

It’s important to note that git diff by itself doesn’t show all changes made since your last commit – only changes that are still unstaged.      
This can be confusing, because if you’ve staged all of your changes, git diff will give you no output.    


## Get git repository <a name="gitpull"></a>  
+ Clone the full repository  

You clone a repository with `git clone [url]`. For example, if you want to clone the Git linkable library called libgit2, you can do so like this:  
```
$ git clone https://github.com/libgit2/libgit2
```  
That creates a directory named “libgit2”, initializes a .git directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new libgit2 directory, you’ll see the project files in there, ready to be worked on or used. If you want to clone the repository into a directory named something other than “libgit2”, you can specify that as the next command-line option:  

```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```  

That command does the same thing as the previous one, but the target directory is called mylibgit.

+ Download code changes from repository  

Incorporates changes from a remote repository into the current branch. In its default mode, `git pull` is shorthand for `git fetch` followed by `git merge FETCH_HEAD`.  

```
git pull <remote name> <remote branch name>
git pull origin master
```  

PS: If any of the remote changes overlap with local uncommitted changes, the merge will be automatically cancelled and the work tree untouched. It is generally best to get any local changes in working order before pulling or stash them away with `git-stash`.  

+ Working on different branches  

List existing branches, the current branch will be highlighted with an asterisk *  

```
git branch --list
```   

Go to an existing branch

```
git checkout mybranch1
```   

Create a branch "mycranch2" and go to this branch immediately `git checkout -b mybranch2`.   

+ Download objects and refs from another repository  

Fetch branches and/or tags (collectively, "refs") from one or more other repositories, along with the objects necessary to complete their histories. Remote-tracking branches are updated (see the description of <refspec> below for ways to control this behavior).

By default, any tag that points into the histories being fetched is also fetched; the effect is to fetch tags that point at branches that you are interested in. This default behavior can be changed by using the --tags or --no-tags options or by configuring remote.<name>.tagOpt. By using a refspec that fetches tags explicitly, you can fetch tags that do not point into branches you are interested in as well.

git fetch can fetch from either a single named repository or URL, or from several repositories at once if <group> is given and there is a remotes.<group> entry in the configuration file. (See git-config).

When no remote is specified, by default the origin remote will be used, unless there’s an upstream branch configured for the current branch.  

The names of refs that are fetched, together with the object names they point at, are written to .git/FETCH_HEAD. This information may be used by scripts or other git commands, such as git-pull.  

Update the remote-tracking branches:  

```
$ git fetch origin
```   

The above command copies all branches from the remote refs/heads/ namespace and stores them to the local refs/remotes/origin/ namespace, unless the branch.<name>.fetch option is used to specify a non-default refspec.

Using refspecs explicitly:

```
$ git fetch origin +pu:pu maint:tmp
```  

This updates (or creates, as necessary) branches pu and tmp in the local repository by fetching from the branches (respectively) pu and maint from the remote repository.  

The pu branch will be updated even if it is does not fast-forward, because it is prefixed with a plus sign; tmp will not be.

Peek at a remote’s branch, without configuring the remote in your local repository:

```
$ git fetch git://git.kernel.org/pub/scm/git/git.git maint
$ git log FETCH_HEAD
```  

The first command fetches the maint branch from the repository at git://git.kernel.org/pub/scm/git/git.git and the second command uses FETCH_HEAD to examine the branch with git-log. The fetched objects will eventually be removed by git’s built-in housekeeping (see git-gc).


## Go back to specific commit<a name="goback"></a>

`git log` to find out guid number of your desired commit
`git checkout <guid>` to go back to that specific commit



## Git push succeed, remote not updating <a name='missupdates'></a>

Here's setup case, simple and everything setup by default. Locally only one default branch: master, on the github remote origin only one branch master as well.   
After committing changes locally and `git push origin master` failed due to remote version was newer(I changed one filename on github.com), so `git pull origin master` and enter your merge commit message. Pushed again the same error.   
Nightmare began, to FORCE a push `cf push -f origin master` on github.com I lost one file(the filename changed one) and newly committed local files were not pushed. Specify with `git add newfile` and committ and push, nothing added into github.com/project.git Newly local files were always not pushed to remote github.com/project.git.

solution: `git push origin HEAD:master`
[doc link](https://git-scm.com/docs/git-push)  
`git push [remoterepo] [src]:[dst]` HEAD used in git to prepresent [your last commit snapshot](https://git-scm.com/blog/2011/07/11/reset.html), master is the branch name of your remote origin.

Normally this error happens when you are not in local master branch when push and git can not solve which master you meant.













