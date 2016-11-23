# Keeping Submodules up to date in Git

When your project gets complicated, it makes sense to split it up into manageable chunks.  Git supports this process using submodules.  


When you create a reference to a submodule within your repository, you actually include a reference to a specific commit.  If the repository you are referencing is updated, those changes aren’t automatically reflected in your repository.  This sort of makes sense, since git has no idea whether the submodule you are referencing is under your control or not.  


By default, the submodule is checked out in [detached head](https://git-scm.com/docs/git-checkout) state.  This is fine if you want a read only copy, but a bit of a pain if you want to commit changes.  In addition, by default, submodules aren’t updated when you pull changes to your repository meaning you can get out of sync.


Working with submodules on a specific branch, where you want to commit changes to the submodule, requires a slight change to your CLI commands.  Because you are no longer working with one repository, but multiple repositories.


##[Creating your submodule](https://git-scm.com/docs/git-submodule)
Create your submodule, pointing it to the destination branch

```git submodule add -b <branch> <repository_url> <submodule_folder>```


##Track the remote branch

Once you have your submodule, and you are checked out on a branch, you need to ensure that it is configured to track the remote branch of the submodule

```
cd <submodule_folder>
git branch --set-upstream-to=origin/<branch>
```

Now you are tracking the remote branch.  You can make changes to the sub-module, and push them to the remote repository as required.


##Updating your repository (including submodules)
When you make changes to your submodule, then you need to commit the change in the parent to ensure that the pointer is kept up to date.  However, this results in two commits - one to the submodule and one to the parent, and this may not always be desirable at the same time.


In order to ensure the submodules are up to date, when pulling changes, use the following commands:

```
cd <parent_repository>
git pull
git submodule foreach git pull
```

This will ensure that the repository and submodules are kept up to date on the appropriate branch.