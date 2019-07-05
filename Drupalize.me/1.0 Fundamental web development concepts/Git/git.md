#Git

Git is version control software.
Version control is a way to keep track of changes to lots of files, it will always you to save something and go back if you did not like your changes.

With version control you can create `branches` this is a way of creating a new version of the code without effecting the current release, once you are happy with everything on your branch you can `merge` them back in.

## Common use

### Git init

Before you can use git you need to create a repo, to do this you can simply run `git init`, this will create a `.git` folder inside your folder.
This folder will keep all the information about the repo

### Git status

To find out what has been added/changed or delete you can run `git status` this will list out all the changes inside a repo.

### Git add

Once you know what you want to upload to the global repo you can use `git add FILENAME`
A quick way of adding all the files is to use the command `git add .` this will add everything that has changed to the `stage`.

### Git stage

The stage is a place displayed when you run `git status`, the stage is self is basically a list of things that are ready to be `committed`.

### Git commit

Committing a file is basically like saying "yes I want you to save these changes" to simply commit data you will have to staged things first using `git add` to select all the files you need.

A commit requires a message, to use it you can say `git commit -m "this is my message"` - `-m` stands for message.

### Git push

To save the changes to the remote you can run `git push` this will take everything you added to the stage and `push` it up to the remote repo.

## Advance use

### Git checkout

Git checkout allows you to pull out certain versions of the code as well as well as branches and tags.

To check out a specif revision of you code you would run `git log` this will display all the changes as well as a revision number once you get this number you can use it to roll back doing `git checkout [revision] .` allowing you to be back at that revision.

To checkout a branch is the same but you use the branch name `git checkout [branch-name]`.

To checkout a tag you would use `git checkout tags/[tag-name]`

### Git merge

if you have changes on another branch that you want on your branch you will need to do a merge. A common use for this is that you have been doing work for say a ticket on your own branch called `ticket-0123`, you are now ready to push your changes to the live branch `master` so you will need to check that out.

Once you have checked it out you can run `git merge ticket-0123` and this will pull all the differences inside your branch into master, once complete you can simply run a git commit with a message about merging followed by a git push to sync it all up with the remote. 