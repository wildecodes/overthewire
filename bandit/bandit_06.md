The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size

Commands you may need to solve this level
ls, cd, cat, file, du, find, grep

looking in ~/ there is nothing.

the clue said "somewhere on the server" so could be anywhere. navigating one directory up takes us to /home, in which there are many bandit# dirs. bandit6 corresponds to ~. 

trying to use find doesn't work! permission denied! We don't have root so there's no way to chmod or chown. most things I tried don't work because of lack of permissions.

reading the man page for find, we can use -gid and -uid flags to find files that belong to user bandit7 and group bandit6.

however this doesn't work because -uid and -gid needs a number not a name.

we can get user info by looking at /etc/passwd:
$ cat /etc/passwd

outputs something like this:
bandit5:x:11005:11005:bandit level 5:/home/bandit5:/bin/bash
bandit6:x:11006:11006:bandit level 6:/home/bandit6:/bin/bash
bandit7:x:11007:11007:bandit level 7:/home/bandit7:/bin/bash
bandit8:x:11008:11008:bandit level 8:/home/bandit8:/bin/bash
bandit9:x:11009:11009:bandit level 9:/home/bandit9:/bin/bash

With this info, we can call:
find -gid 11006 -uid 11007

but we still get:
find: './bandit5/inhere': Permission denied
find: './bandit30-git': Permission denied
find: './bandit28-git': Permission denied
find: './bandit29-git': Permission denied
find: './bandit31-git': Permission denied
find: './bandit27-git': Permission denied

interestingly, these locations seem to be owned by group bandit6 and user bandit7 even though they are named with higher numbers

Well, the clue said "somewhere on the server," so maybe if we try searching root, we'll come up with something. 

.
.
.
find: '/home/bandit27-git': Permission denied
find: '/var/log': Permission denied
find: '/var/lib/puppet': Permission denied
find: '/var/lib/apt/lists/partial': Permission denied
find: '/var/lib/update-notifier/package-data-downloads/partial': Permission denied
/var/lib/dpkg/info/bandit7.password
find: '/var/lib/polkit-1': Permission denied
find: '/var/spool/rsyslog': Permission denied
find: '/var/spool/bandit24': Permission denied
find: '/var/spool/cron/atspool': Permission denied
.
.
. skipping stuff because there was a lot.

most things are still permission denied, but among the list of results is one file we can access:
/var/lib/dpkg/info/bandit7.password

$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
