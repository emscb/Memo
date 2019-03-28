### 파일 입출력

#### 입력 받기

```c++
int main(int argc, char **argv) { ... }
```

- 들어오는 parameter는 0번이 본 실행 파일의 절대 경로, 1번부터가 사용자가 준 파일이 된다.
- `argc`는 절대 경로를 포함한 parameter 개수, `argv[1]`부터 사용자가 준 파일 이름이 들어있다.

### 파일 읽기

> `#include <fstream>`

```c++
#include <fstream>

int r;
ifstream inFile(argv[1]);

if (inFile.fail()){
    printf("파일 열기 실패"); exit(-1);
}

while(inFile >> r){
    printf("r : %d", r);
}
```

### 예외처리

- `if`를 사용하는 경우 : 예외 상황에서 `printf()`를 통해 알리고 `exit()`으로 나갈 수 있다. (짜기 나름)
- `try throw catch` : `try` 속 코드 실행하다가 예외 발생 시, `throw`를 통해 어떤 값하나를 `catch`의 parameter로 보낸다. 이후 `catch`에서 어떻게 처리할 지 코딩

