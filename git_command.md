## Git Command Guide

1. **git status**  : show the status of git repository , present branch

2. **git init** : make a directory coverted to a git repository

3. **git add <file>** : the file in the directory may be not in the repository. You should use the command `git add <file>` to add the file to the repository

4. **git commit -m 'first commit'**  ：hand in the change (write the cache in file), `-m` reprsents the message you note.

5. **git  log** : view the history of `commit`

6. **git branch** : After the command `git init`, the repository will generate a main branch **master** automatically that the deault branch you will be in.  `git branch` can view all the branch and the present branch. `git branch a` to build a new branch **a**. the branch *a* will be the same as the branch where you execute the command `git branch a`

7. **git checkout a** : Switch to the branch **a**

8. **git checkout -b a** : build a new branch **a** and switch to the branch **a**.

9. **git merge <branch>**  :  merge the branch you worked in to the main branch **master**.
   Firtst, `git branch master`switch to the main branch **master**
   Second, `git merge a`, merge the branch **a** to the present branch **master**.

10. **git branch -d <branch>** : delete branch which is empty.

11. **git branch -D <branch>** : delete a branch with force.

12. **git tag** : the usage is the same as `git branch`. For example, in branch **a**, you can use `git tag v1.0`  to lable the present code with **v1.0**. Use `git checkout v1.0` to switch to the tag **v1.0**.

13. **SSH** :  use the command `ssh-keygen -t rsa` to generate two file **id_rsa** and **id_rsa.pub** in the repository, **id_rsa** is the private key and **id_rsa.pub** is the public key. Then add the content of **id_rsa.pub** to the GitHub.  After authorization succeed, we can hand the repository to th GitHub. Use the command `ssh -T git@github.com` to test if the authorization succeed or not.

14.  **git config** : use the command`git config -l` to view the configuration of the git. Use the command `git config user.name XXXX` to config the present git repository. Use the command `git config --global user.name XXXX` to config all git repositories.
15.  **git reset --hard <HEAD> & git reflog** : Use the command `git reflog` view all the history version of the present branch. Use the command `git reset --hard <HEAD> ` to come back to the history version .
16.  **git remote -v** : view all the remote host alias name and remote host address.
17.  **git remote add <remote-host-alias-name> <remote-host-address>** : Connect the local git repository with the remote git repository. For example, `git remote add origin git@github.com:fragementaryworld/HappyNewYear` or  `git remote add origin https://github.com/fragementaryworld/HappyNewYear.git`
18.  **git push <remote-host-alias-name> <branch>** : For example, use the command `git push origin master` to push the local repository to the remote repository's branch **master**.
19.  **git pull <remote-host-alias-name> <branch>** : Use the command `git pull origin master` to pull the remote repository's branch to local repository to keep the local repository updated.
20.  **git clone <remote-repository-address>** : Use the command `git clone git@github.com:fragementaryworld/HappyNewYear` or `git clone https://github.com/fragementaryworld/HappyNewYear.git` to clone remote repository to the local. 



