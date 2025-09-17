# This repo contains a reproduction of a bug in git check-ignore

The bug: `git check-ignore` has a different return code when `-v` is passed and the file checked matches a negated pattern in a .gitignore file.

```
$ git check-ignore --no-index foo; echo $?
1
$ git check-ignore --no-index -v foo; echo $?
.gitignore:1:!foo       foo
0
```

According to the man page, `git check-ignore` should return 0 if the file is ignored and 1 if it is not ignored. So it seems the presence of `-v` should not change the meaning of the return code.

## Reproduction steps

1) find a non-ignored file
2) add a negated pattern to .gitignore that matches the file

Then `git check-ignore <file>` returns 1 but `git check-ignore -v <file>` returns 0.  If the file is committed, the `--no-index` flag must also be used to observe the bug.

Expected: both return codes should be 1

Reproduced using a fork of git 2.46.0 on Ubuntu 22.04 as well as with git 2.51.0.windows.1 under git bash on Windows 11.
