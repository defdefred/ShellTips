# ShellTips
Tips for shell programming
## Process Substitution
Probably the best way to avoid using external command like `awk` and to keep new value of variable changed in a `while read` statement.
```
$ while read DEV SIZE DONTCARE
do
  SUM=$(expr $SUM + $SIZE)
done << EOT
$(df -lk | egrep "^/")
EOT
$ echo $SUM
248314248
` 
```
For `bash` user, it is:
```
$ while read DEV SIZE DONTCARE
do
  SUM=$(expr $SUM + $SIZE)
done < <(df -lk | egrep "^/")
$ echo $SUM
248314248
```

## Set multiple variables from command result
Everybody knowns how to keep command results in a variable:
```
$ DATE=$(date)
$ echo $DATE
Sat Feb 11 22:25:04 CET 2023
```
But how to split the result in multiple variable in `dash` without using external command like `awk`? 
```
$ read D M N H L Y << EOT
> $(date)
> EOT
$ echo $Y $M $N $H $D $L
2023 Feb 11 22:29:01 Sat CET
```
For `bash` user, it is:
```
$ read D M N H L Y < <(date -u)
$ echo $Y $M $N $H $D $L
2023 Feb 11 21:37:31 Sat UTC
```
