Reverse - Hashish (CBM CTF 2019 write-up)
===========================================

![rev_hashish](problem.png)

-----------------------------------------------------------------------------
# Readme.txt: 

Hi 
My friend made a hash algo which he thought was irreversible....however he was on Manali Cream[hashish ;-)].
so he print something he should not print while making hashes.
Below is the output of given binary with flag as input... get the flag.

<pre><code>
hash-0 : 138 
hash-1 : 512
hash-2 : 1645 
hash-3 : 5034 
hash-4 : 15218 
hash-5 : 45756 
hash-6 : 137391 
hash-7 : 412292 
hash-8 : 1236927
hash-9 : 3710845 
hash-10 : 11132642
hash-11 : 33398021
hash-12 : 100194167
hash-13 : 300582553 
hash-14 : 901747774 
hash-15 : 2705243426 
hash-16 : 8115730373 
hash-17 : 24347191171 
hash-18 : 73041573621 
hash-19 : 219124720917 
hash-20 : 657374162799 
hash-21 : 1972122488522
hash-22 : 5916367465576
</code></pre>

-----------------------------------------------------------------------------

# Write-up 

문자열의 길이를 N이라고 했을 때(1부터 셈), "hash-N"에서 N의 길이는 입력으로 오는 문자열의 길이보다 하나 크다. hash-(N-1)의 숫자는 문자열의 N번째 문자의 아스키코드가 1씩 증가함에 따라 1씩 증가한다. 그래서 이런 원리를 이용해서 한문자씩 맞는 문자를 찾아내었다. 

찾아내기 위해 다음과 같은 코드를 작성했다:


### a2z.cpp ; 이 코드는 커맨드라인 아규먼트로 입력받는 문자열과, 입력받는 숫자에 해당하는 문자를 결합한 문자열을 출력해주는 코드다. 

<pre><code>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
	char str[] = "!^#$%&*()+-_,./\\<>:;=?@~ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

	printf("%s", argv[1]);	
	printf("%c", str[atoi(argv[2])]);

	return 0;
}
</code></pre>


### a2z.sh ; 이 코드는 위의 a2z.cpp 코드와 파이프라인 을 이용한 명령어를 87번 반복해서 실행하기 위해서 만들어진 배쉬 쉘 스크립트 코드이다.

<pre><code>
#!/bin/bash

for ((i=0; i<87; i++)); do
	echo
	echo $i
	./a2z cbmctf{w3@k_h4sh_4l60} $i | ./hashish
done
</code></pre>


### 이제 이 두개의 코드를 이용해 다음의 명령어를 순서대로 입력해서 한 문자씩 찾아낸다 : 

<pre><code>
vi a2z.sh       -> 스크립트 파일을 열어서 지금까지 발견한 문자열로 대체한다.
bash a2z.sh > a2z_result.txt        -> a2z.sh를 실행한 결과를 a2z_result.txt에 저장한다.
grep -n 5916367465576 a2z_result.txt       -> grep 명령어에 N-1번째의 숫자를 삽입해 결과 텍스트 파일의 몇번째 줄에 해당 숫자가 있는지 알아낸다.
vi a2z_result.txt       -> a2z_result.txt를 연다
: set number        -> vim에서 줄 수를 알려주는 명령을 이용해 해당 숫자가 몇번째 줄에 있고, 그래서 어떤 문자인지 알아낸다.
./hashish       -> hashish 파일을 실행시켜 결과가 정말 맞는지 확인한다.
</code></pre>

a2z_result.txt에서 어떤 문자인지 알아낼 때는 
!^#$%&*()+-_,./\<>:;=?@{}(24) ABCDEFGHIJKLMNOPQRSTUVWXYZ(50) abcdefghijklmnopqrstuvwxyz(76) 0123456789(86)
를 참고 한다.
특수문자는 0~24, 영어 대문자는 ~50, 소문자는 ~76, 숫자는 ~86까지.


-----------------------------------------------------------------------------

# Flag 

<pre><code>
$ ./hashish
enter the string
cbmctf{w3@k_h4sh_4l60}
hash-0 : 138
hash-1 : 512
hash-2 : 1645
hash-3 : 5034
hash-4 : 15218
hash-5 : 45756
hash-6 : 137391
hash-7 : 412292
hash-8 : 1236927
hash-9 : 3710845
hash-10 : 11132642
hash-11 : 33398021
hash-12 : 100194167
hash-13 : 300582553
hash-14 : 901747774
hash-15 : 2705243426
hash-16 : 8115730373
hash-17 : 24347191171
hash-18 : 73041573621
hash-19 : 219124720917
hash-20 : 657374162799
hash-21 : 1972122488522
hash-22 : 5916367465576
</code></pre>
