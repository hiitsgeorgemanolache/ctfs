The same set of commands used in the previous level can be executed again, with the difference that the *README* file now contains different information:
```bash
just an epmty file... muahaha
```
Since the repository does not reveal useful information in the main branch, other Git objects should be examined. One possible source of additional data is tags, which can reference specific objects such as commits.
The `git-tag` command can be used to list all tags present in the repository (documentation: [git-tag - Create, list, delete or verify a tag object signed with GPG](https://git-scm.com/docs/git-tag)).
```bash
git tag
#output: secret
```
The presence of a tag named *secret* suggests that it may reference an object containing additional information. To inspect the object associated with this tag, the `git-show` command can be used (documentation: [git-show - Show various types of objects](https://git-scm.com/docs/git-show)).
```bash
git show secret
```
Unlike the previous level, where the solution involved examining branches and commits, this level requires exploring other Git objects, specifically tags. Tags can reference commits or other repository objects and may contain information not visible in the default branch history.
