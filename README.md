# oss_1 project
### getopt & getopts & sed & awk 명령어
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
|----|----|
|OptionString|**getopts** 명령이 인식하는 옵션 문자의 문자열을 포함한다. 문자 뒤에 콜론이 오는 경우 해당 옵션은 인수가 있는 것으로 간주되며 이 인수는 별도의 인수로 제공되어야 한다.|
|이름|**getopts** 명령에 의해 발견된 옵션 문자로 설정된다.|
|Argument...|공백으로 분리된 하나 이상의 문자열이며 올바른 옵션인지 **getopts** 명령이 검사한다. Argument가 생략되는 경우 위치 매개변수가 사용된다.|

**[종료상태]**

이 명령은 다음과 같은 종료값을 리턴합니다.
|항목|설명|
|----|----|
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
### ***sed***

**[설명]**

> *sed*는 Stream EDitor의 약자로 매우 컴팩트한 명령 체계를 이용하여 텍스트를 파싱하고 변형하는 (고대의) 텍스트 편집 도구이다. *sed*는 그 전신이 되는 ed의 스크립팅 체계를 기반으로 하고 있다. vim과 같이 편집될 텍스트를 화면상에 보면서 내용을 작성/수정하는 개념의 텍스트 편집기가 개발되기 이전의 텍스트 편집기이다.
> 
> *sed*명령어는 1개 라인씩 입력 라인을 읽어들여 표준출력으로 출력한다.
> 
> *sed*는 각 라인을 읽을 때마다 ed에서 사용하던 형식의 대치작업을 실행한다. 일치하는 문자열이 있으면 그 문자열을 대치한 후 출력하고 일치하는 문자열이 없으면 그 라인은 수정되지 않고 그대로 출력된다. 
> 
> 라인들을 하나씩 읽고, 수정하고, 출력하기 때문에 기억장치 안의 버퍼를 사용하지 않는다. 버퍼를 사용하지 않으면 파일의 크기에 제한 없이 작업을 할 수 있다. 
> 
> *sed*는 아주 큰 파일을 처리할 때 주로 사용된다.
> 
>> *sed*명령어 사용 전 주의 
>> - 텍스트 내용을 완벽히 알고 있어야한다.
>> - 어느라인 혹은 어떤 내용을 수정할지 알고있어야 한다.
>> - 스크립트를 활용해서 해당 라인을 수정한다.

**[구문]**

**sed** [옵션] 스크립트 입력파일1 [입력파일2 ... ]


**[옵션]**

|옵션|별칭|기능|
|:----|:----:|----|
|-n|–quiet, –silent|읽어들인 라인을 암시적으로 자동출력하는 것을 중단한다.|
|-e|–expression=script|실행될 명령에 스크립트를 추가한다.|
|-f script_file|–file=script_file|스크립트 파일의 내용을 가져와서 추가로 실행한다.|
|–follow-symlinks||제자리 처리시에 심볼릭 링크를 따르도록 한다. 하드링크는 깨진다.|
|-i [SUFFIX]|–in-place[=SUFFIX]|파일을 제자리 처리한다. (즉 변경된 내용을 파일에 적용한다.)|
|-c||제자리 처리시에 사본을 이용한다.|
|-r|–posix|확장된 정규식 패턴을 사용한다.|
|-s|–regexp-extended|파일을 하나의 긴 스트림이 아닌 분리된 데이터들로 처리한다.|
|-u|–separate|입력으로부터 최소한의 내용만 읽고 더 자주 플러시한다.|

**[예제]**
> 치환
> 
>> **sed 's/addrass/address/' list.txt** : addrass를 address로 바꾼다. 단, 원본파일을 바꾸지 않고 표준출력만 한다.
>> 
>> **sed 's/\t/\ /' list.txt** : 탭문자를 엔터로 변환
>
> 삭제
>> **sed '/TD/d' 1.html** : TD 문자가 포함된 줄을 삭제하여 출력한다.
>> 
>> **sed '/Src/!d' 1.html** : Src 문자가 있는 줄만 지우지 않는다.
>> 
>> **sed '1,2d' 1.html** : 처음 1줄, 2줄을 지운다.
>> 
>> **sed '/^$/d 1.html** : 공백라인을 삭제하는 명령이다. ☆
> 
> 특정 문자열 바꾸기
>> ![바꾸기](https://user-images.githubusercontent.com/76990397/142642982-0faf22d6-1dcd-48ff-b707-886ffd3aeeca.jpg)
>
>  특정 문자열이 포함된 행 삭제
>> ![삭제](https://user-images.githubusercontent.com/76990397/142643161-97b91b78-7970-46fc-8890-d2117182e79d.jpg)
> 
> 파일의 일부만 편집해야 할 경우 라인의 범위를 지정하여 사용 가능
>> ![라인범위지정](https://user-images.githubusercontent.com/76990397/142643418-d3ac7b9b-50e5-4d94-904a-70bd6eabf4d0.jpg)
>
> 라인 1부터 hello를 포함하고 있는 첫번째 라인까지 모든라인들을 삭제
>> ![f](https://user-images.githubusercontent.com/76990397/142643615-eb8436c2-d137-4c25-974f-719a67b726dc.png)
>
> datafile 안의 데이터로부터 처음 세 개의 문자들을 삭제
>>![dd](https://user-images.githubusercontent.com/76990397/142643682-185fe0f6-f518-436e-8fcd-07c0dd46f519.png)
>
> 각 라인마다 뒤에 Hello World! 문자를 입력
>>![ffff](https://user-images.githubusercontent.com/76990397/142643795-4be01120-4cfb-4ba6-9e15-7c1f6eff2ef3.png)

---
### ***awk***

**[설명]**

> *awk*명령어는 패턴 탐색과 처리를 위한 명령어로 간단하게 파일에서 결과를 추려내고 가공하여 원하는 결과물을 만들어내는 유틸리티이다. 즉, 파일에서 패턴이 일치하는 행을 찾아서 지정한 조치를 수행해주는 명령어이다. 
> 
> *awk* 명령은 사용자가 정의한 명령어 집합을 이용하여 파일 집합과 사용자가 제공한 확장 표현식을 한번에 한 행씩 비교한 다음 확장 정규식과 일치하는 모든 행에 작용하여 특별한 작업을 해 준다.
> 
> 초기 개발자 Aho, Weinberger, Kernighan의 첫글자를 따서 이름지어진 *awk*는 GNU 프로젝트에서 만들어진 텍스트 처리 프로그래밍 언어로 유닉스 계열의 OS에서 사용 가능하며, 텍스트 형태로 되어있는 입력 데이터를 행과 단어 별로 처리해 출력한다.
 
**[구문]**

**awk** [OPTION...] [awk program] [ARGUMENT...]

+ awk 명령에서 awk program은 ‘ ‘(single quotation marks) 안에 작성합니다.

**[옵션]**

|옵션|기능|
|:----|----|
|-F|필드 구분 문자 지정|
|-f|awk program 파일 경로 지정|
|-v|awk program에서 사용될 특정 variable값 지정|


