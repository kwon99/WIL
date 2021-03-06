## Definition
`def send(self, data)` <br />
`def sendline(self, line=b'')`

*Sendline의 경우 Send로 보내는 구문에 "\n"(개행)를 추가해서 전달합니다*

## read, gets and scanf
1. `ssize_t read(int fildes, void *buf, size_t nbyte);`
2. `char *gets(char *str)`
3. `int scanf( const char *format, ... );`
   
모두 입력과 관련된 함수지만 미세하게 다른 부분이 존재하지만 집중해서 볼 차이점은 *__어디까지 받아들이냐__* 입니다. <br /><br />

1. `read는 버퍼의 크기만큼 읽을 수 있다면 전부 읽어들입니다.`
2. `gets()는 "\n"(개행)앞까지 읽어들입니다. (이 뒤에 '\0'을 붙여 문자열을 완성합니다)`
3. `scanf는 공백앞까지 읽어들입니다. (단 "[]"를 사용해 조절할 수는 있습니다)`

## Terminal
Terminal은 개행이 이루어질 때까지 사용자 입력을 전송하지 않고 대기하는데, 이는 많은 사람들로 하여금 read와 같은 입력 함수의 읽어들이는 한도를 공백 혹은 개행으로 판단하게 합니다.<br /><br />
하지만 이는 Terminal 자체의 동작일 뿐, *__입력 함수와는 관계가 없습니다.__*

## Conclusion
지금까지 나온 이야기를 종합하면 하나의 결론에 도달할 수 있습니다.<br /><br />

* read 함수는 *__개행을 기준으로 받아들이는 것이 아니기 때문에__* sendline을 사용하지 않아도 입력값만큼 받습니다.
* gets, scanf 함수는 *__개행 혹은 공백을 기준으로 받아들이기 때문에__* sendline을 사용해야 입력값만큼 보낼 수 있습니다.

## Reference

* [Dreamhack - send와 sendline](https://dreamhack.io/forum/qna/1126)
* [read 함수의 정의와 사용법](https://badayak.com/4486)
* [gets 함수의 정의와 사용법](https://blockdmask.tistory.com/343)
* [scanf 함수의 정의와 사용법](https://clgnsdl94.tistory.com/m/24)