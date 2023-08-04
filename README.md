## How to remove a file that is already been pushed into remote repo such as config file

- The below command will remove from git, remote repo and local filesystem after firing this command just add and commit the changes
```bash
git rm path/to/your/file/to/remove
```
- The below command will remove from git , remote repo but not from localfilesystem

```bash
git rm --cached path/to/your/file/to/remove
```
