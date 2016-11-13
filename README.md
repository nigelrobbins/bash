
# Linux commands
http://ss64.com/bash/

# search manuals
apropos <text>

# further doc
/usr/share/doc

# regular expressions
https://autohotkey.com/docs/misc/RegEx-QuickRef.htm

# display contents of files side-by-side
ls file[0-9] | xargs paste

# offset within string
cat garbage | while read line
do
  echo $line | grep -o -b "from\|to"
done

# ignore lines that have / and print first token
cat output | grep -v "\/" | awk '{ print $1 }'

# regular expression (\w not sed)
grep -E "^[ \t]*int[ \t]+\w[ \t]*\(*" *.c

# float
float=3.3446
printf "%.2f \n" $float
 
# redirect stdout/err
echo "redirect this STDOUT to STDERR" 1>&2
ls $0 some-text 2> stderr
 
# special chars in string, lower
declare -l lc_string="...\nAb\n..."
echo -e lower [$lc_string]
 
# ternary test
declare -i var1=99
(( var0=var1<98?9:21))
echo var0=$var0
 
# length of string
${#string}

# exit code
rm gshsks
echo exit[$?]

# find files 1 day old
find . -mtime -1

# find/list directories 1 level below current directory
find . -maxdepth 1 -type d -exec ls -ld '{}' +

# tar
tar -cvf /tmp/backup.tar --exclude="log" --exclude="*.tmp" .

# local vars
local VAR=abc
echo VAR=$VAR
echo all args [$@]

# calculator, float
read expression
echo "scale=3;$expression"|bc

# numeric for loop
for i in `seq 1 3`;
do
  echo i [$i]
done

# sort file, insert line number
sort wordlist | nl | head -3

# sort based on key
sort -k 2 names

# most recent processes
ps -A | tail -5 | cut -b 1-5

# for loop, remainder
for i in {1..99}
do
  mod=`expr $i % 2`
  if [ $mod -ne 0 ]; then
  echo $i
  fi
done

# while loop
ls -l | grep -v "^total" | awk '{ print $5 }' | while read output
do
  echo output [$output]
done

# create file
var="Hi"
cat <<EOF > /tmp/aFile

echo $var

exit 0

EOF


# process args
while (( "$#" )); do
  echo arg [$1]
  shift
done

# catch interrupt
trap catch INT
catch()
{
  echo
  echo Got Ctrl-C
  exit 1
}

# parse output, squeeze repeats, cut and delete chars
disk_space()
{
  space=`df -h | tr -s ' ' | grep -v "^File" | cut -f 5 -d ' ' | tr -d '%' | tail -1`
  echo space[$space]
  case $space in
  [1-9])
    message="all good"
    ;;
  [1]*)
    message="1 or more"
    ;;
  *)
    message="other"
    ;;
  esac
  echo message [$message]
}

# char handling, xmas tree
tree()
{
i=0
while [ $i -lt $size ]
do
  j=0
  # loop round all chars in line
  num_blanks=$((size-1-i))
  line=""
  while [ $j -lt $size ]
  do
    ch="x"
    if [ $j -ge $num_blanks ]; then
      ch="*"
    fi
    line="${line}${ch}"
    j=$((j+1))
  done
  echo [${line}] | tr -d '[' | tr -d ']' | tr x ' '
  i=$((i+1))
done
}
echo Enter size
read size
tree $size
 
# check if root, case statement
ROOTUID=0
usage()
{
  echo You must be root to  run this script
  exit 1
}
if [ "$UID" != "$ROOTUID" ]; then
  usage
fi
case "$1" in
  [0-9]*)
    LINES=$1
    ;;
  *)
    LINES=50
    ;;
esac
echo LINES=$LINES

# read into array
echo Enter 3 colours
read -a colours
echo colours... ${colours[0]} ${colours[1]} ${colours[2]}

# array processing
names=("1" "4" "2" "2" "3")

echo ${names[@]}

len=${#names[@]}

i=0

while [ $i -lt $len ]

do

  if [ "${names[$i]}" != "${names[$i+1]}" ]; then

    echo not pair "${names[$i]}" "${names[$i+1]}"

  fi

  i=$(($i+2))

done


# array processing, calculations, return from function
do_sum()
{
echo enter array size
read size
count=0; sum=0
while [ $count -lt $size ]
do
  read num
  sum=$((sum+num))
  count=$((count+1))
done
return $sum
}
do_sum
echo ret=$?

# array processing, numeric sort
parseNums()
{
nums=("${@}")
sorted=`echo ${nums[@]} |tr " " "\n"|sort -n|tr "\n" " "`
sortedArray=(${sorted// / })
echo sortedArray="[${sortedArray[@]}]"
len=${#sortedArray[@]}
i=0
while [ $i -lt $len ]
do
  i=$(($i+1))
  if [ $i -lt $len ]; then
    echo ${sortedArray[$i-1]} ${sortedArray[$i]}
  fi
done
}
declare -a nums_in=("-1" "2" "1" "0")
echo nums="${nums_in[@]}"
parseNums "${nums_in[@]}"

# array processing, function return number
lonely()
{
names=("${@}")
len=${#names[@]}
i=0
while [ $i -lt $len ]
do
  if [ "${names[$i]}" != "${names[$i+1]}" ]; then
    return ${names[$i]}
  fi
  i=$(($i+2))
done
}
echo enter array size
read size
declare -a names_in
count=0
while [ $count -lt $size ]
do
  read num
  names_in[$count]=$num
  count=$((count+1))
done
lonely "${names_in[@]}"
echo lonely=$?

# array processing, regular expression search, function return string
userPasswd()
{
envs=("${@}")
len=${#envs[@]}
i=0
while [ $i -lt $len ]
do
  if [[ ${envs[$i]} =~ ^user=[a-zA-Z0-9]* ]]; then
    user=`echo "${envs[$i]}" | cut -d '=' -f 2`
  fi
  if [[ "${envs[$i]}" =~ "pass" ]]; then
    pass=`echo "${envs[$i]}" | cut -d '=' -f 2`
  fi
  i=$(($i+1))
done
userPasswdReturn="${user}:${pass}"
}
declare -a envs_in=("a=b" "user=joe" "pass=secret")
userPasswd "${envs_in[@]}"
echo userPasswdReturn=$userPasswdReturn

# array processing, regular expression search, function return array
parseLog()
{
log=("${@}")
len=${#log[@]}
i=0
while [ $i -lt $len ]
do
  if [[ ${log[$i]} =~ [0-9]\.[0-9]\.[0-9]\.[0-9] ]]; then
    ind[$i]="true"
  else
    ind[$i]="false"
  fi
  i=$(($i+1))
done
}
declare -a log_in=("line1" "ip address is 10.0.2.15" "mac addr = 08:00:27:12:96:98")
echo log="${log_in[@]}"
parseLog "${log_in[@]}"
echo "${ind[@]}"

# sequence
sequence()
{
if [ $# -eq 1 ]
then
  Num=$1
else
  echo -n "Enter a Number :"
  read Num
fi
f1=0
f2=1
echo "The sequence for the number $Num is : "
for (( i=0;i<=Num;i++ ))
do
   echo -n "$f1 "
   fn=$((f1+f2))
   f1=$f2
   f2=$fn
done
echo
}
sequence $1
