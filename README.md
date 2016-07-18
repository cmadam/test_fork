# How to fork a git project across different github servers

## Problem

Need to fork a project from github.com, and work with the forked
repository under a different git server (github.ibm.com).  Need to
pull any changes made to the original project after the fork into my
working repository.

## Solution:

Follow these steps:

1. Fork project https://github.com/cmadam/test_fork under
https://github.com/cmadam-1/test_fork

2. Clone the forked repository:
   ```
   git clone https://github.com/cmadam-1/test_fork.git
   ```

3. Add the original project repo to the list of remote sources, under
the name `upstream`.  The `upstream` remote will be used to retrieve
the changes to the original project:
   ```
   git remote add upstream https://github.com/cmadam/test_fork.git
   ```

4. Create the new working repository under github.ibm.com, and point
the `origin` remote to this new repository:
   ```
   git remote set-url origin git@github.ibm.com:cmadam/test_fork.git
   ```

5. Push the forked code in the new working repository:
   ```
   git push origin master
   ```

6. To test that everything works, make a change (update this README.md
file) in the original git repository.  The three steps below update
the new working repository with the latest changes made to the
original repo:

  7. Get the latest code from the master branch or the original
     repository:
     ```
     git fetch upstream master
     ```

  8. Merge these changes to the master branch of the working repo:
     ```
     git checkout master
     git merge upstream/master
     ```

  9. Push these changes to the remote working repo:
     ```
     git push
     ```
10. Finally, in order to avoid retyping the commands above, create a
Makefile that will contain a target, like this:
   ```
   update-code:
       @git fetch upstream master
       @git checkout master
       @git merge upstream/master
       @git push
   ```
   Now, typing `make update-code` will get the latest updates in the
   original source code, merge them into the working repo, and push them
   back into the working repo.  This will work if the merge can perform
   successfully.  TODO: handle gracefully merging conflicts.

11. Do not add the Makefile above to the git repo, as this is only a
local convenience tool.
