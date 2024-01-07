---
layout: post
title: The Missing Semester of Your CS Education - 1장
hide_title: true
---

# 1. the shell

- 공부하면서 작성했기 때문에 틀리거나 부정확한 부분이 있을 수 있다.
- macOS, zsh 기준으로 작성되었다.

1\. For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don’t have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use Windows Subsystem for Linux or a Linux virtual machine to use Unix-style command-line tools. To make sure you’re running an appropriate shell, you can try the command echo $SHELL. If it says something like /bin/bash or /usr/bin/zsh, that means you’re running the right program.

A. 생략

2\. Create a new directory called missing under /tmp.

A.

```zsh
mkdir /tmp/missing
```

3\. Look up the touch program. The man program is your friend.

A.

```zsh
man touch
```

4\. Use touch to create a new file called semester in missing.

A.

```zsh
touch /tmp/missing/semester
```

5\. Write the following into that file, one line at a time:

```
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
```

A.

```zsh
echo '#!/bin/sh' > semester
echo 'curl --head --silent https://missing.csail.mit.edu' >> semester

```

zsh에서 "" 내부의 문자열은 변수의 값으로 치환되거나 명령으로 실행될 수 있다.  
#! 는 유닉스의 shebang이라는 것으로 해당 파일을 해석할 인터프리터를 지정하는 기능을 가지고 있어서, 만약 "#!/bin/sh" 라고 적으면 단순히 문자열로 인식되지 않고 shebang의 기능이 작동하게 된다.  
따라서 단순히 문자열 그대로를 파일에 입력하기 위해서는 "" 대신 '' 를 사용해야 한다.
또한 \> 를 사용하면 내용이 덮어씌워지기 때문에 두번째 줄을 입력할 때에는 >> 를 사용해야 한다.

6\. Try to execute the file, i.e. type the path to the script (./semester) into your shell and press enter. Understand why it doesn’t work by consulting the output of ls (hint: look at the permission bits of the file).

A.

```zsh
./semester
zsh: permission denied: ./semester
```

위와 같이 오류가 발생하게 된다.

```zsh
ls -l semester
```

로 권한을 확인해 보면 해당 파일에 대한 x(execute) 권한이 설정되어 있지 않음을 확인할 수 있다.

7\. Run the command by explicitly starting the sh interpreter, and giving it the file semester as the first argument, i.e. sh semester. Why does this work, while ./semester didn’t?

```zsh
sh semester
```

./semester 는 파일을 직접 실행하는 것으로 권한이 없어서 거부된다.

하지만 sh semester는 작동하는데, sh는 'sh'셸을 호출해서 파일을 실행하도록 하는ㅊ 명령어로 'sh'가 셸 스크립트 실행 권한을 가지고 있기 때문에 파일을 작동시킬 수 있다.

8\. Look up the chmod program (e.g. use man chmod).

```zsh
man chmod
```

9\. Use chmod to make it possible to run the command ./semester rather than having to type sh semester. How does your shell know that the file is supposed to be interpreted using sh? See this page on the shebang line for more information.

```zsh
chmod +x semester
chmod 755 semester
```

+x 로 실행 권한을 줄 수도 있고, 각 권한을 숫자로 나타낸 표기법을 사용할 수 도 있다.

10\. Use \\| and \> to write the “last modified” date output by semester into a file called last-modified.txt in your home directory.

A. 먼저 파일 실행결과는 다음과 같다.

```zsh
./semester

HTTP/2 200
server: GitHub.com
content-type: text/html; charset=utf-8
last-modified: Sat, 10 Jun 2023 13:13:30 GMT
access-control-allow-origin: *
etag: "648476fa-1f86"
expires: Thu, 29 Jun 2023 08:42:27 GMT
cache-control: max-age=600
x-proxy-cache: MISS
x-github-request-id: 635C:691D:928AF:C9A24:649D419B
accept-ranges: bytes
date: Thu, 29 Jun 2023 09:08:49 GMT
via: 1.1 varnish
age: 0
x-served-by: cache-icn1450041-ICN
x-cache: HIT
x-cache-hits: 1
x-timer: S1688029729.930088,VS0,VE317
vary: Accept-Encoding
x-fastly-request-id: 250602762d61f0f580348cceaf6e23958bc1a634
content-length: 8070
```

코드는 다음과 같다.

```zsh
./semester | grep -w last-modified | cut -d' ' -f2- > last-modified.txt
```

- ./semester \| : 파일을 실행한다. \| 는 파이프 연산자 라는 것으로 파이프 앞의 출력을 뒤의 입력으로 전달한다.
- grep -i last-modified \| : \| 로 ./semester 의 실행 결과를 받아왔다. grep은 파일에서 문자열을 탐색해서 행을 출력하는 기능이 있다. -w 는 입력된 문자열이 있는 행을 출력한다. 따라서 여기까지 결과는 last-modified: Sat, 10 Jun 2023 13:13:30 GMT 이다.
- cut -d' ' -f2- : cut은 문자열을 잘라낸다. -d 옵션은 뭘 기준으로 자를 건지를 결정하고 -d' ' 는 공백을 기준으로 자르겠다는 뜻이다. -f 는 추출해낼 부분을 정하는 것으로 -f2- 는 2번째 덩어리부터 끝까지를 의미한다. 따라서 결과물은 Sat, 10 Jun 2023 13:13:30 GMT 가 된다.
- \> last-modified.txt : 파일에 입력한다.

11\. Write a command that reads out your laptop battery’s power level or your desktop machine’s CPU temperature from /sys. Note: if you’re a macOS user, your OS doesn’t have sysfs, so you can skip this exercise.

A. 맥북 사용중이라 생략
