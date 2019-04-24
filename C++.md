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
혹은
while(getline(inFile, r, '\n')) { }
```



> `#include <string>`

```c++
#include <string>
using namespace std;

string s;
char input[1000] = { 0 };

getline(cin, s);
if (s.empty()){
    exit(0);
}

strcpy(input, s.c_str());
```

### 파일 쓰기

> `#include <fstream>`

```c++
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

ofstream out("text.txt");

if (out.is_open()){
    out << "내용";
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

### 문자열 입출력

> `#include <sstream>`

```c++
#include <sstream>
#include <string>

int n;
stringstream ss;
ss.str("12 345 6789");
for(int i=0; i<5; i++){
    ss >> n;
    cout << n << endl;
}
```

- `!ss.eof()` 사용 가능 (`iostream` 상속 받았기 때문)
- `while (ss >> n)` 가능 (받을 게 없으면 0을 받는다. n이 받는 것이 아니고...)
  - `cin`에서는 효과 없고 파일 입력에서는 가능
- `ss.str()` : 그냥 쓰면 내부 문자열 다 보여줌

```c++
#include <iostream>
#include <sstream>

std::string input = "abc,def,ghi";
std::istringstream ss(input);
std::string token;

while(std::getline(ss, token, ',')) {
    std::cout << token << '\n';
}
```



### 템플릿 클래스

- **헤더 파일에 정의까지 다 쓰셔야 합니다.**
- 특수화

```c++
template <class T>
    class Stack{
    public:
        T pop(){
            return data[top];
        }
    };

template <> int Stack<int>::pop() {}
```



### CString, string 형변환

- CString to string

```c++
CString strSrc("내용");
std::string strDst;
strDst = strSrc;
```

- string to CString
  - `CString cs(s.c_str());`
- CString to int
  - `int = _ttoi(CString);`
- int to CString
  - `CString.Format(_T("%d"), int);`

### String, char 형변환

- string to char
  - `strcpy(char*, string.c_str());`
- char to string

```c++
char* ch1 = "Hello!";
char ch2[] = "Hi!";

string str1(ch1);
string str2;
str2 = ch2;
```



### string

- End of string
  - `*(&inp.at(i)+1`
- string 붙이기
  - `+=` 연산자 이용 or `append()`
- `\r\n`이 순서로 써야 줄바꿈이 되더라~
- 일부분만 보기
  - `a.substr(시작위치, [개수]);`

### int, string 형변환

- int to string
  - `string str = to_string(intValue);`
- string to int
  - `int intValue = atoi(str.c_str());` (c++ 2008 이하)
  - `int n = stoi(str);`

### 포인터 배열의 초기화

- 배열은 그냥 `{ 0 }` 이렇게 하면 되는데 포인터 배열은 안되네요
- 그냥 `for`문으로 하나하나 NULL로...

### 배열 정렬

- `sort()` 사용
  - `#include <algorithm>`
  - sort(시작지점, 끝나는지점+1) = sort(배열 포인터, 배열 포인터 + 배열 크기)

### 길이

- `sizeof()`는 그 대상의 크기 (**byte**)지 길이가 아님!

### 실행 시간 측정

```c++
#include <time.h>

clock_t start, end;
start = clock();
end = clock();
double result = (double)(end - start);
```

- `time()`으로 하면 초 단위 (`clock()`은 ms 단위)
- 