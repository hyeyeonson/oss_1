# oss_1 project
### getopt & getopts & sed & awk 명령어 조사
---

### ***getopt***

   **[설명]**

> - *getopt*는 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석
> - 플래그는 단일 ASCII문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 한다.
> - 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없다.
> - *getopt*는 모든 토큰을 읽은 후 또는 특수 토큰 --(더블 하이픈)이 발생하는 경우 처리를 완료한다 --> 처리된 플래그, --(더블하이픈) 및 남아있는 토큰을 출력한다.
> - 토큰이 플래그와 일치하는데 실패하는 경우 *getopt*명령은 메시지를 표준 오류에 기록한다.
   
**[예제]**
```shell
#!/usr/bin/bsh
# parse command line into arguments
set -- `getopt a:bc $*`
# check result of parsing
if [ $? != 0 ]
then
        exit 1
fi
while [ $1 != -- ]
do
        case $1 in
        -a)     # set up the -a flag
                AFLG=1
                AARG=$2
                shift;;
        -b)     # set up the -b flag
                BFLG=1;;
        -c)     # set up the -c flag
                CFLG=1;;
        esac
        shift   # next flag
done
shift   # skip --
# now do the work
```
쉘에서 다음 명령을 사용하여 *getopt*명령을 실행 => set argv=`getopt OptionString $*`
