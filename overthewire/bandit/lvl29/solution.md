The same set of commands used in the previous level can be executed again, with the difference that the *README* file now contains different information:

```bash
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
The commit history of the repository can be examined using `git log`, which displays all commits in the current branch along with their associated metadata (documentation: [git-log - Show commit logs]((https://git-scm.com/docs/git-log)). 
```bash
git log
```
This reveals several commits together with information such as the commit hash, author, date, and commit message. One of the commits includes the message “add missing data”, which suggests that additional information may have been introduced at that point in the repository’s history.  
To inspect the contents of a repository at a specific commit, `git checkout` can be used with the corresponding commit hash.
```bash
git checkout *commit_hash*
cat README
```
