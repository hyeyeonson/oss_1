# oss_1 project
### getopt & getopts & sed & awk 명령어 조사
---

### ***getopt***

**[설명]**

> *getopt*는 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석한다.
> 
> 플래그는 단일 ASCII문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 한다. 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없다.
> 
> *getopt*는 모든 토큰을 읽은 후 또는 특수 토큰 --(더블 하이픈)이 발생하는 경우 처리를 완료한다 --> 처리된 플래그, --(더블하이픈) 및 남아있는 토큰을 출력한다.
> 
> 토큰이 플래그와 일치하는데 실패하는 경우 *getopt*명령은 메시지를 표준 오류에 기록한다.


**[구문]**

**getopt** *Format Tokens*

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

다음 예제에서 각각 *getopt*는 플래그 및 인수를 같은 방식으로 처리한다.
 - -a ARG -b -c
 - -a ARG -bc
 - -aARG -b -c
 - -b -c -a ARG

**[파일]**

|항목|설명|
|----|----|
|/usr/bin/getopt|*getopt* 명령을 포함합니다.|

---
### ***getopts***

**[설명]**

> *getopts*는 매개변수 리스트에서 옵션 및 옵션 인수를 검색하는 Korn/POSIX 쉘 내장 명령이다.
> 
> 옵션은 +(더하기 부호) 또는 -(빼기 부호)로 시작하고 그 뒤에 문자가 온다. + 또는 -로 시작하지 않는 옵션은 **OptionString**을 종료한다.
> 
> *getopts*는 호출될때마다 Name의 다음 옵션 값과 쉘 변수 **OPTIND**에서 처리될 다음 인수의 색인을 배치한다. 쉘이 호출될 때마다 **OPTIND**는 1로 초기화된다. 
> 
> **OptionString**의 문자 뒤에는 :(콜론)이 오면 옵션에 인수가 있는 것으로 간주된다. 옵션에 옵션-인수가 필요한 경우 *getopts*는 이를 변수 **OPTARG**에 배치한다.
> 
>> **OptionString**에 포함되지 않은 옵션 문자가 발견되거나 찾은 옵션에 필요한 옵션-인수가 없는 경우:
>> - **OptionString**이 :으로 시작되지 않는 경우
>>     - Name은 **?(물음표)** 문자로 설정된다.
>>     - **OPTARG**가 설정되지 않으며 진단 메세지가 표준 오류에 기록된다. 
>     
> 이 조건은 *getopts*를 처리하는 중에 생긴 오류가 아니라 호출 애플리케이션에 인수가 표시되는 중에 발견된 오류라고 간주된다. 진단 메시지는 명시된 대로 기록되지만 종료 상태는 0이 된다.
>> - **OptionString**이 :으로 시작되는 경우
>>    - Name은 ?(물음표)로 설정되거나 누락된 필수 옵션에 대해서는 :(콜론) 문자로 설정된다.
>>    - **OPTARG**는 발견된 옵션 문자로 설정되고 출력이 표준 오류에 기록되지 않습니다.
> 
> 다음 중 하나가 옵션의 끝을 식별한다. 특수 옵션 - -,이 - 또는 +로 시작되지 않는 인수를 찾는 경우 또는 오류가 발생하는 경우이다.
>> - 옵션의 끝에 도달하는 경우
>>    - **getopts**는 0보다 큰 리턴 값으로 종료된다.
>>    - **OPTARG**는 첫번째 비옵션-인수의 색인으로 설정된다. 여기서, 첫 번째 --인수는 그 전에 다른 비옵션-인수가 나타나지 않는 경우 옵션-인수로 간주되고 비옵션-인수가 없는 경우에는 값 $#+1로 간주된다.
>>    - Name은 **?(물음표)** 문자로 설정됩니다.

**[구문]**

**getopts** *OptionString Name[ Argument ... ]*

**[매개변수]**

|항목|설명|
|:----|:----|
|OptionString|**getopts** 명령이 인식하는 옵션 문자의 문자열을 포함한다. 문자 뒤에 콜론이 오는 경우 해당 옵션은 인수가 있는 것으로 간주되며 이 인수는 별도의 인수로 제공되어야 한다.|
|이름|**getopts** 명령에 의해 발견된 옵션 문자로 설정된다.|
|Argument...|공백으로 분리된 하나 이상의 문자열이며 올바른 옵션인지 **getopts** 명령이 검사한다. Argument가 생략되는 경우 위치 매개변수가 사용된다.|

**[종료상태]**

이 명령은 다음과 같은 종료값을 리턴합니다.
|항목|설명|
|:----|:----|
|0|OptionString에 의해 지정되거나 지정되지 않은 옵션이 발견|
|>0|옵션의 끝에 도달했거나 오류가 발생|

**[예제]**
1) 다음 **getopts** 명령은 a, b 및 c가 유효한 옵션이고 a 및 c 옵션에 인수가 있음을 지정한다.

      `getopts a:bc: OPT`
      
2) 다음 **getopts** 명령은 a, b 및 c가 유효한 옵션이고 a 및 b 옵션에는 인수가 있으며 명령행에서 정의되지 않은 옵션을 발견하는 경우 getopts가 OPT의 값을 ?로 설정하도록 지정한다.

      `getopts :a:b:c OPT`
 
 3) 다음 스크립트가 인수를 구문 분석하고 표시한다.
      ```shell
      aflag=
      bflag=
      
      while getopts ab: name
      do
         case $name in
         a)     aflag=1;;
         b)     bflag=1
                  bval="$OPTARG";;
         ?)     printf "Usage: %s: [-a] [-b value] args\n" $0
                  exit 2;;
         esac
      done
      if [ ! -z "$aflag" ]; then
         printf "Option -a specified\n"
      fi
      if [ ! -z "$bflag" ]; then
         printf 'Option -b "%s" specified\n' "$bval"
      fi
      
      shift $(($OPTIND -1))
      printf "Remaining arguments are: %s\n" "$*"
      ```
---
