# 2 Arrays 배열

- Abstract data types (ADT)
  - Data type
    - Object + Operation
    - Object : 객체, computation을 수행하는 주체 (변수)
    - Operation : 함수, 연산자
  - Abstraction을 구현(실현)해놓은 data type
- C++에서는 ADT를 `class`를 이용하여 구현한다.

### Class

1. Class name : Struct name을 type define한 것 (`struct`처럼 앞에 붙일 필요 없음)
2. Data member : Struct member, 변수
3. Member function : Data member에 적용할 수 있는 함수 (C에서는 함수 포인터를 넣는 것 외에 함수를 넣을 수 있는 방법이 없음)
4. Access level : Member와 member function에 대한 접근 가능 레벨
   - `public` : 제어x
   - `private` : Class의 member만 접근 가능 (내부에서만), friend class까지 가능!
     - 내가 지정한 friend는 내 private data를 볼 수 있지만 나는 friend의 private data를 볼 순 없어 ~~짝사랑~~
   - `protected` : `private`에서 subclass가 접근 가능하다는 것만 다르다. Subclass는 상속받은 class

### 2.1.4 Special Class Operations

#### Constructors & Destructors

- Constructors (생성자)

  - 따로 정의하지 않아도 default로 컴퓨터가 정의한다.

- Sophisticated definition

  - 생성자에 default값을 준다.

  - ```c++
    Rectangle::Rectangle(int x=0, int y=0, int h=0, int w=0)
    : xLow(x), yLow(y), height(h), width(w) {}
    ```

- Destructors (파괴자)

  - 안만들어도 자동으로 만들어진다.
  - 동적 메모리 할당을 받았을 때는 끝날 때 반환해줘야하니 만들어야 한다.

#### Operator Overloading

- 함수 뿐만 아니라 연산자에 대해서도 오버로딩이 가능하다.

- Call by reference : 직접 그 변수에 접근해서 값을 가져옴

- Call by value : 그 값의 복사본을 저장하고 거기로 접근

- ```c++
  bool Rectangle::operator== (const Rectangle &s) {
      if (this == &s) return true;
      if ((xLow == s.xLow) && (yLow == s.yLow)
          && (height == s.height) && (width == s.width)) return true;
      else return false;}
  ```

  - `this` : 함수를 호출한 객체의 **주소**
  - `&s` : `s`의 주소
  - `private`한 멤버 변수이지만 연산자 `==` 또한 멤버 함수로 들어가기 때문에 값에 접근할 수 있다.

- ```C++
  ostream& operator<< (ostream& os, Rectangle& r) {
      os << "Position is: " << r.xLow << " ";
      os << r.yLow << endl;
      os << "Height is: " << r.height << endl;
      os << "Width is: " << r.width << endl;
      return os;}
  ```

  - `&r` : `<<` 뒤에 오는 `Rectangle` 객체
  - `cout`도 어떤 객체인데, 그 자료형이 `ostream`인 것
  - 하지만 `Rectangle` 클래스에 접근할 수 있는 권한이 없다.
  - `Rectangle` 클래스 정의에 연산자를 `friend`로 넣어줘야 한다.

### 2.1.5 Miscellaneous Topic

#### Struct in C++

- `struct`도 사용 가능하다.
- `private`이나 `public`을 설정할 수 있다.
- `class` 선언 시 default는 `private`이지만 `struct`에서는 `public`이다.

#### Union

- 데이터 타입이 서로 달라질 수 있는 경우

#### Static class data member

- C의 `static` 로컬 변수 같이 앞에 `static`이라고 키워드를 써주면 된다.
- C++에서는 같은 클래스의 객체들이 공유할 수 있는 공간을 할당해 주는 것

## 2.2 The Array as an Abstract Data Type

- <index, value> 쌍의 집합이라고 보자
- 배열은 실제로 메모리에서 연속적으로 존재하기 때문에 index값으로 value를 바로 부를 수 있는 것이다.

## 2.3 The Polynomal Abstract Data Type

- 항이 3개면 3 terms

### 2.3.1 Polynomial Representation

- Representation 1

  - ```C++
    private:
    	int degree;		// degree <= MaxDegree
    	float coef[MaxDegree + 1];		// coefficient array
    ```

  - 내가 설정한 `MaxDegree`에 맞춰서 array를 만들어 놓고 계수만 순서대로 저장

  - 단점

    - `MaxDegree`보다 큰 차수의 다항식은 저장할 수 없다.
    - 남은 공간이 너무 많다. (보통 2, 3차 다항식만 쓰는 경우)

- Repersentation 2

  - ```c++
    private:
    	int degree;
    	float *coef;
    ```

  - 생성자가 실행될 때 동적 할당하면 된다.

    - ```c++
      Polynomial::Polynomial(int d) {
          degree = d;
          coef = new float[degree + 1];}
      ```

  - 단점

    - 최고차항만 큰 경우 결국 메모리 낭비

- Representation 3

  - ```c++
    class Polynomial;		// forward declaration (서로 얽혀서)
    class Term {
        friend class Polynomial;
        private:
        	float coef;
        	int exp;		// exponent
    };
    ```

  - 계수와 지수를 같이 array에 저장해보자

  - ```c++
    private:
    	Term *termArray;		// array of nonzero terms (상수항 제외)
    	int capacity;			// size of termArray
    	int terms;				// number of nonzero terms
    ```

  - Term의 개수 만큼만 array를 할당 받는다. (`capacity`개)

  - `capacity`는 항의 개수보다 크거나 같다.

  - 코드가 조금 복잡해졌지만 공간 낭비를 최소화했다.

### 2.3.2 Polynomial Addition

- ```C++
  void Polynomial::NewTerm(const float theCoeff, const int theExp) {
      if (terms == capacity) {	// 꽉 찼으면
          capacity *= 2;
          Term *temp = new Term[capacity];	// 새 array
          copy(termArray, termArray + terms, temp);
          delete [] termArray;				// 이전 메모리 할당 해제
          termArray = temp;}
      termArray[terms].coef = theCoef;
      termArray[terms++].exp = theExp;}
  ```

  - 필요할 때마다 크기를 키워가며 저장

## 2.4 Sparse Matrices 희박 행렬

- 행렬의 원소 중에 0이 좀 많은 (대체적으로) 행렬
- 행렬의 크기가 클 때, 0이 많은 건 메모리 낭비다.

### 2.4.3 Trnasposing a Matrix

- 전치 : 행과 열을 바꾸는 것
- 공간을 아낄려고 `smArray` 클래스를 사용했지만 sparse하지 않은 애들이 오면 오히려 2차원 배열을 사용하는게 더 간단하게 처리될 수 있다.
  - `smArray` 사용 시 : O(cols\*terms), terms <= cols\*rows
  - 2차원 배열 사용 시 : O(cols\*rows)
- 시작점을 찾아서 바로 넣어주는 알고리즘(**이해**)의 경우
  - ~~왜 이걸 쓰는걸까요~~
  - O(terms+cols)가 된다.

