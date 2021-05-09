## Linux recipes and reusable code

#### Inspect the storage in use across directories
This `du` command estimates file space usage excluding the `/proc` pseudo-filesystem directory. It displays the grand total (`-c`) and individual totals (`-s`) for each directory (`*`) in the current working directory. The totals are displayed in kilobytes, megabytes or gigabytes (`-h`).

    du -shc --exclude=proc *

The output can look like this:
```
0	bin
147M	boot
0	dev
19M	etc
24K	home
0	lib
0	lib64
0	media
0	mnt
0	opt
32K	root
1.1M	run
0	sbin
0	srv
0	sys
0	tmp
1.1G	usr
456M	var
1.7G	total
```
T
