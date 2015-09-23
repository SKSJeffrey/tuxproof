# Git Hook - Post Merge Update

There are many user directories created in `/var/www`, such as:

* userone/
* usertwo/
* userthree/

`whoami` must match the directories in `/var/www` **AND** a post-merge command must be run:

```bash
git fetch <remote>
git merge <remote>/<branch>
OR
git pull <remote>
```

By running git commands that create or modify files, it will take the current user's uid:gid as the modification stamp on the files. Example:

```bash
# Pre-Git Command
-rx-r--r--. 1 development development 10271 Sep 3 12:02 index.php
```

```bash
# Post-Git Command
-rx-r--r--. 1 username development 10271 Sep 3 12:02 index.php
```

After a post-merge command is run, it will trigger the post-merge hook that will change all ownership in `/var/www/<username>` to development:development. 

```bash
# Post-Merge Hook
-rx-r--r--. 1 development development 10271 Sep 3 12:02 index.php
```

The reason for the ownership change is so all files will be owned by Apache's user and group: **development**
