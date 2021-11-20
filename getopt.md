# getopt 설명
- 쉘에서 명령을 실행할 때 옵션을 사용하는데 이때 옵션 해석작업을 쉽게 도와주는 명령.
- stdio.h 와 unistd.h 모두에 정의되어 있음

## 참고사항으로 short 옵션의 특징
```python
command -a -b -c            ##처럼 그냥 사용도 가능하고
command -abc                ##붙여서도 사용할 수 있고
command -b -ca              ##순서가 바뀌어도 된다.
command -a xxx -b -c yyy    ##옵션 인수를 가질 수 있다.
command -axxx -bcyyy        ##위와 똑같이 해석이 된다.
command -a -b -- -c         ##옵션 구분자 '--'의 우측에 있는 값은 옵션으로 해석하면안됨
```
## long옵션의 경우
`--posix`,`--warning level` 와 같은 형태로 사용되는 long 옵션은 붙여서 쓸수가 없음

getopt는 /usr/bin/getopt에 위치한 외부 명령어로 short, long 옵션을 모두 지원하고 : 에 따라 옵션 -a는 옵션 인수를 가짐\
short옵션 지정은 -o옵션으로, : 에 따라 옵션 -a 는 옵션 인수를 가짐.\
`getopt -o a:bc`

long 옵션지정은 -l 옵션으로, 옵션명은 ',' 로 구분\
:에 따라 옵션 --path와 --name은 옵션 인수를 가짐\
명령 라인에서 옵션 인수 사용은 "--name foo" 또는 "--name=foo" 두가지 모두 가능\
`getopt -l help, path:,name:`

명령 마지막에는 --와 함께 "$@"를 붙임\
`getopt -o a:bc -l help,path:,name: -- "$@"`

설정하지 않은 옵션이 사용되거나 옵션 인수가 빠질 경우 오류메시지를 출력해줌\
getopt 명령은 사용자가 입력한 옵션들을 case 문에서 사용하기 좋게 정렬해줌\

## SYNOPSIS
```
       getopt optstring parameters
       getopt [options] [--] optstring parameters
       getopt [options] -o|--options optstring [options] [--] parameters
```

## OPTIONS
```

       -a, --alternative
              Allow long options to start with a single '-'.

       -h, --help
              Display help text and exit.  No other output is generated.

       -l, --longoptions longopts
              The long (multi-character) options to be recognized.  More than one option name  may  be  specified  at
              once,  by  separating the names with commas.  This option may be given more than once, the longopts are
              cumulative.  Each long option name in longopts may be followed by one colon to indicate it  has  a  re‐
              quired argument, and by two colons to indicate it has an optional argument.

       -n, --name progname
              The  name  that  will  be  used  by the getopt(3) routines when it reports errors.  Note that errors of
              getopt(1) are still reported as coming from getopt.

       -o, --options shortopts
              The short (one-character) options to be recognized.  If this option is not found, the  first  parameter
              of  getopt  that does not start with a '-' (and is not an option argument) is used as the short options
              string.  Each short option character in shortopts may be followed by one colon to indicate it has a re‐
              quired  argument,  and  by  two colons to indicate it has an optional argument.  The first character of
              shortopts may be '+' or '-' to influence the way options are parsed and output is generated  (see  sec‐
              tion SCANNING MODES for details).

       -q, --quiet
              Disable error reporting by getopt(3).

       -Q, --quiet-output
              Do not generate normal output.  Errors are still reported by getopt(3), unless you also use -q.

       -s, --shell shell
              Set  quoting  conventions  to  those of shell.  If the -s option is not given, the BASH conventions are
              used.  Valid arguments are currently 'sh' 'bash', 'csh', and 'tcsh'.

       -T, --test
              Test if your getopt(1) is this enhanced version or an old version.  This generates no output, and  sets
              the  error  status to 4.  Other implementations of getopt(1), and this version if the environment vari‐
              able GETOPT_COMPATIBLE is set, will return '--' and error status 0.

       -u, --unquoted
              Do not quote the output.  Note that whitespace and special (shell-dependent) characters can cause havoc
              in this mode (like they do with other getopt(1) implementations).

       -V, --version
              Display version information and exit.  No other output is generated.
```
getopts의 경우는 getopt의 short처리부분이다.
---
