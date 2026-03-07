The same set of commands used in the previous level can be executed again, with the difference that the *README* file now contains different information:
```bash
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: <no passwords in production!>
```
To inspect the available branches in the repository, the following `git-branch` command can be used (documentation: [git-branch - List, create, or delete branches](https://git-scm.com/docs/git-branch)).
```bash
git branch -a #lists both remote-tracking branches and local branches 
origin/HEAD -> origin/master
origin/dev
origin/master
origin/sploits-dev
```
These branches represent different versions of the repository that may contain additional or modified information.  
The `git-checkout` command can be used to switch between branches and inspect their contents. Switching to the `dev` branch allows access to a version of the repository where the *README.md* file contains the actual password.
```bash
git checkout origin/dev
cat README.md
```
