---
layout: post
title:  "Git from zero!"
date:   2016-12-14 14:49:39 +0100
categories: jekyll update
---

Now you have git downloaded and ready to start.

# Table of contents
1. [Git config](#gitconfig)
2. [Adding an existing project to GitHub using the command line](#addproj)
3. [Checking the Status of Your Files](#statuscheck)
4. [Get git repository](#gitpull)

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
You clone a repository with `git clone [url]`. For example, if you want to clone the Git linkable library called libgit2, you can do so like this:  
```
$ git clone https://github.com/libgit2/libgit2
```  
That creates a directory named “libgit2”, initializes a .git directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new libgit2 directory, you’ll see the project files in there, ready to be worked on or used. If you want to clone the repository into a directory named something other than “libgit2”, you can specify that as the next command-line option:  
```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```  
That command does the same thing as the previous one, but the target directory is called mylibgit.


