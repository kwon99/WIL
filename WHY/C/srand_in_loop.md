## Definition
```C
void srand(unsigned int seed);
```
*seed − This is an integer value to be used as seed by the pseudo-random number generator algorithm*

## Incident
*__srand(time(NULL))이 loop 내부에 있으면 같은 값이 반복적으로 출력되는 현상__*

![same](/image/srand_in_loop/same.png)

## Report
*__loop 내부에 있기 때문이 아닌 time(NULL)을 시드로 사용하기 때문에 발생하는 현상__*

srand 함수는 인자를 이용해 seed 값을 초기화합니다. 따라서 for문의 작동이 1초 이하로 걸릴 경우 time(NULL)은 모두 같은 값을 띄게 되고 즉, 계속 같은 값으로 seed를 초기화하게 되므로 생기는 일입니다.

## Code
```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <Windows.h> // #include <unistd.h>

int main()
{
    int random = 0;

    // srand 사이 간격을 1초 이상으로 설정했을 경우
    for (int i = 0; i < 5; i++)
    {
        Sleep(1000); // Windows

        srand(time(NULL));
        random = rand() % 9;
        printf("%d\n", random);
    }

    // for 문이 1초 이상으로 걸릴 경우
    for (int i = 0; i < 3000000; i++)
    {
        srand(time(NULL));
        random = rand() % 9;
        printf("%d\n", random);
    }
}
```

## Reference
* [srand() function in loop](https://stackoverflow.com/questions/27280947/c-srand-function-in-loop)