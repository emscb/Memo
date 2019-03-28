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

### 포인터

- `int i` : 내가 정수를 넣을거니까 그 공간을 만들어줘!
- `int *i` : 내가 int형을 가리키는 주소를 저장할 공간이 필요해!
  - 주소는 어떤 자료형을 가리켜도 필요한 주소 길이가 똑같다.
  - 4바이트래
  - 포인터의 이름이 `i`인데, **`*i`는 그 주소로 가서 값을 가져오라고 (call by reference)**
  - `&i` : 주소값이 저장된 주소, **어쨌든 주소값.**
- `char[]` : 글자 하나 당 1바이트를 먹고, 첫 글자를 가리키는데 포인터(4byte)가 쓰인다.
- 보이드 포인터는 아무나 가리킬 수 있다.
- `new`가 그냥 메모리에 공간을 잡고 그 첫 bit를 가리키는 포인터를 준다.
- 포인터의 자료형은 끝점을 알기 위해 필요하다. (시작점은 안다.)
  - String에는 끝점을 모르니까 뒤에 `\0`를 붙여놓는다. (1byte 더 붙음)
- 