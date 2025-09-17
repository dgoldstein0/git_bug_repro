# This repo contains a reproduction of a bug in git check-ignore

The bug: `git check-ignore` has a different return code when `-v` is passed and the file checked matches a negated pattern in a .gitignore file.

```
$ git check-ignore foo; echo $?
1
$ git check-ignore -v foo; echo $?
.gitignore:1:!foo       foo
0
```

According to the man page, `git check-ignore` should return 0 if the file is ignored and 1 if it is not ignored. So it seems the presence of `-v` should not change the meaning of the return code.

## Reproduction steps

1) find a non-ignored file
2) add a negated pattern to .gitignore that matches the file

Then `git check-ignore <file>` returns 1 but `git check-ignore -v <file>` returns 0.

Expected: both return codes should be 1

This is using a fork of git 2.46.0
