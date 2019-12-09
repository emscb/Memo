# Argparse Tutorial

&gt; [https://docs.python.org/3/howto/argparse.html](https://docs.python.org/3/howto/argparse.html)

## 시작하기에 앞서...

```shell
$ ls
cpython  devguide  prog.py  pypy  rm-unused-function.patch
$ ls pypy
ctypes_configure  demo  dotviewer  include  lib_pypy  lib-python ...
$ ls -l
total 20
drwxr-xr-x 19 wena wena 4096 Feb 18 18:51 cpython
drwxr-xr-x  4 wena wena 4096 Feb  8 12:04 devguide
-rwxr-xr-x  1 wena wena  535 Feb 19 00:05 prog.py
drwxr-xr-x 14 wena wena 4096 Feb  7 00:59 pypy
-rw-r--r--  1 wena wena  741 Feb 18 01:01 rm-unused-function.patch
$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.
...
```

* `ls`는 보통 아무 옵션 없이 사용한다. \(현재 디렉토리의 내용\)
* `ls pypy`를 보면, 다른 디렉토리의 내용을 바로 볼 수 있다.
  * `ls`와 다른 점은 **positional argument**를 줬다는 점이다.
  * 프로그램이 이 값을 가지고 뭘 할지 이미 알고 있다. 단지 이 값이 command line에 어디 위치해 있다는 것만으로 이미 정해져 있다.
  * 더 나아가보면, `cp`의 경우 `cp SRC DEST`로 많이 사용하는데, 같은 경우다.
* `ls -l`
  * `-l`을 줌으로써 프로그램의 행동이 바뀌었다. 단순히 이름만 보여주는 것이 아니라, 더 많은 정보를 보여준다.
  * 이 경우는 **optional argument**라 한다.

## The basics

```text
import argparse
parser = argparse.ArgumentParser()
parser.parse_args()
```

```text
$ python3 prog.py
$ python3 prog.py --help
usage: prog.py [-h]

optional arguments:
    -h, --help    show this help message and exit
$ python3 prog.py --verbose
usage: prog.py [-h]
prog.py: error: unrecognized arguments: --verbose
$ python3 prog.py foo
usage: prog.py [-h]
prog.py: error: unrecognized arguments: foo
```

* 스크립트를 아무 옵션없이 돌리면 아무것도 출력되지 않는다. \(무쓸모\)
* 두 번째는 `argparse` 모듈이 뭔가를 보여준다. 아무것도 안했지만 꽤 괜찮은 도움말이 출력됐다.
* `-h`, `--help` 옵션은 명시해주지 않아도 자동으로\(~~공짜로~~\) 생긴다. 뭔갈 명시해준다면 그 메시지가 뜰 것이다.

## Introducing Positional arguments

```text
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("echo")
args = parser.parse_args()
print(args.echo)
```

```text
$ python3 prog.py
usage: prog.py [-h] echo
prog.py: error: the following arguments are required: echo
$ python3 prog.py --help
usage: prog.py [-h] echo

positional arguments:
  echo

optional arguments:
  -h, --help  show this help message and exit
$ python3 ptog.py foo
foo
```

* `add_argument()` : 프로그램이 받아들이는 command-line 옵션을 명시하는데 사용된다.
* 앞으로 이 프로그램을 부르기 위해선 옵션이 필요하다.
* `parse_args()` : 명시된 옵션의 정보를 반환한다.

현재로썬 `echo`가 뭐하는 메서드인지 알아낼 수가 없기 때문에, 더 추가해보자.

```text
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("echo", help="echo the string you use here")
args = parser.parse_args()
print(args.echo)
```

```text
$ python3 prog.py -h
usage: prog.py [-h] echo

positional arguments:
  echo        echo the string you use here

optional arguments:
  -h, --help  show this help message and exit
```

