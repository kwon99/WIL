## Definition
```C
int scanf_s(const char *format [, argument]...);
```
## Background
많은 CRT(C Runtime Library) 함수에 buffer overflow, buffer overrun 등의 취약점이 계속 발견됩니다. 이에 함수 자체에 취약점이 있음을 인정하고 안정성을 높힌 뒤 본래 함수이름 뒤에 _s(secure을 뜻하는 접미사)를 달고 세상에 나옵니다. scanf_s는 그 안정성을 높힌 새 함수들 중 하나입니다.

## Usage


## Reference

* [Security Features in the CRT](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2012/8ef0s5kh(v=vs.110))
* [Microsoft Docs](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/scanf-s-scanf-s-l-wscanf-s-wscanf-s-l?view=msvc-170)