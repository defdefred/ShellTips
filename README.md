# ShellTips
Tips for shell programming with shellcheck validation.
## Process Substitution
Probably the best way to avoid using external command like `awk` and to keep new value of variable changed in a `while read` statement.
```
SUM=0
while read -r DEV SIZE DONTCARE
do
  SUM=$(($SUM + $SIZE))
done << EOT                     
$(df -lk | grep -E "^/")   
EOT

$ echo $SUM
248314248
```
For `bash` user, it is:
```
SUM=0
while read -r DEV SIZE DONTCARE
do
  SUM=$(($SUM + $SIZE))
done < <(df -lk | grep -E "^/")   

$ echo $SUM
248314248
```

## Set multiple variables from command result
Everybody knowns how to keep command results in a variable:
```
DATE=$(date)

$ echo $DATE
Sat Feb 11 22:25:04 CET 2023
```
But how to split the result in multiple variable in `dash` without using external command like `awk`? 
```
read -r D M N H L Y << EOT
$(date)
EOT

$ echo "$Y $M $N $H $D $L"
2023 Feb 11 22:29:01 Sat CET
```
For `bash` user, it is:
```
read -r D M N H L Y < <(date -u)
$ echo "$Y $M $N $H $D $L"
2023 Feb 11 21:37:31 Sat UTC
```

## Compare 2 folders
With `bash`, you can do:
```
$ ls -1 dir?
dir1:
file1
file2
file3

dir2:
file1
file3
file4

$ diff <(ls -1 dir1) <(ls -1 dir2)
2d1
< file2
3a3
> file4
```
But for POSIX compliance, you have to do:
```
$ ls -1 dir1 | diff /dev/stdin /dev/stderr 2<<EOT
$(ls -1 dir2)
EOT
2d1
< file2
3a3
> file4
```

## Get yesterday date for log rotation
```
$ YESTERDAY=$(date -d "1 day ago" +"%Y%m%d")
$ echo $YESTERDAY
20241126

```
