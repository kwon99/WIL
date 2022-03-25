## Definition
```C
time_t time(time_t *second);
```

## Incident
*__난수 생성시에 time(NULL)을 사용하는데, NULL 값을 넣는 이유__*

## Report
*__time 함수는 하나의 인자를 필요로 하기 때문에 쓰지 않을 경우 NULL을 넣음__*

time 함수는 시간을 리턴값으로 반환하기도 하지만, 인자에 반환하기도 합니다. 즉 아래 코드에서 둘 다 같은 값이 나옵니다. 

인자에 반환하는 방법을 사용하지 않을 땐, 인자 부분에 NULL을 넣어줘서 필수로 채워야 하는 부분을 채워줍니다.

## Code
```C
time_t t1;
time_t t2;
t1 = time(&t2); // t1 == t2
```

## Reference
* [What is time(NULL) in C](https://stackoverflow.com/questions/7550269/what-is-timenull-in-c)