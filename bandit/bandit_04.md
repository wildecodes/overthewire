This one was trickier at first. The clue threw me off: "the password is in the 
only human readable file in this dir"

Right, because all of the 10 files exept one have gibberish in them. (though 
probably not gibberish to a hex editor)

The hard part was that the filenames all began with a dash:
"-file00"

The tricky part is that `cat -file*` or `vi -file*` doesn't work because the dash 
is interpreted as a flag, and then the filename of course is read as an invalid option. 
fail. 

The solution is to use either:
`cat -- -file*`
 -or-
(my preferred solution, which I actually tried but just didn't recognize that it 
worked because I catted all of the files at once and saw gibberish output)
`cat ./-file*`

Rather than going file by file just do `cat ./*`
to get them all, but that makes it hard to distinguish the output of one file 
from another since it's all concatenated together.

To solve this I used one a bash one liner that loops over the files in the 
directory with the command:

```
for x in `ls`; do cat ./$x && echo ""; done
```

This gets the output of cat on a newline for each file. 

Another way:
```
for x in `ls`; do cat ./$x | xxd && echo ""; done
```

What this says: "for (declare a variable 'x') in (what we get from the output of 
`ls`, which is each of the files we wanna `cat`) do (the `cat` command, and 
tell `cat` what we want which is `./$x`, where `$x` is the variable representing each 
file from output of `ls`) and then pipe that to `xxd` so we get a hexdump of each 
file printed to the screen, and then `echo` an empty string so we get new lines. 
`;` to end the statement, `done` to end the loop.

The flag:
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
