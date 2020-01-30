# 6 Application Development

- DB application을 통해 query를 작성할 줄 모르는 user도 DB에 접근하고 데이터를 획득할 수 있다.
- SQL과 programming language를 어떻게 같이 쓸 것인가?

## 6.1 Embedded SQL

- Static approach : SQL 파트를 static하게 embed 시키겠다.
- Dynamic approach : Call level interface를 이용해 동적으로 만들어내겠다.
- 사용할 query를 **미리** 결정하고 코드 사이사이에 끼워넣는다.
- 코드를 짜고 complie한다. > 에러가 난다. (**SQL 파트를 C는 모른다.** - 여기서 C는 host-language)
- Preprocessor를 통해 **전처리 작업**을 해줘야 한다. (C 코드로 바꿔준다.)
- `EXEC SQL <embedded SQL statement>;`

### Cursors

- 왜 필요한가?
  - Impedance mismatch : 자료를 다루는 방식에서의 불일치
  - 배열로 table을 가져오면 좋으나, table size를 미리 알 수 없다.
  - 쿼리 결과 table을 메모리에 전부 올리기에는 부담이 된다.
- App이 결과 tuple을 필요로 할 때만(요청할 때만) 가져오자!
  - 배열 선언을 할 필요가 없다. (tuple 단위로 가져옴)

### Dynamic SQL

- 마찬가지로 cursor를 사용하지만 미리 SQL이 작성되어 있는 것은 아니다.
- Runtime시에 query에 해당되는 부분을 입력받고 이를 처리한다.

## 6.2 ODBC, JDBC et. al.

- DB 서버와 통신할 때 사용하는 API
- Open DB Connectivity

### Static vs. Dynamic Approaches

- p28에 한장에 정리
- Dynamic이 더 풍부하게 사용할 수 있지만 문법 체크 등의 이유로 더 느릴 수 있다.

## 6.3 Application Architectures

### Two-tier / Three-tier

### Web Servers

- 웹 서버에서 처리하지 못하는 문제는 CGI를 통해 app server에 명령을 전달한다.
- 사용자의 web browser > (Internet) > web server > app server > DBS > data (Three-Layer)
- Web server와 app server를 합친 것 (Two-Layer)

### Cookies

- HTTP는 connectionless 방식을 사용한다. (계속 연결이 끊긴다.)
- Session(여러 개의 transaction)
- 쿠키에 클라이언트 정보를 저장해놓음. 이를 통해 connectionless를 극복한다.
- 웹브라우저에서는 그 쿠키를 저장을 해두고 또 다른 request를 할 때 connection을 다시 맺지 않고 쿠키와 request를 함께 전송한다. (~~예전에 왔던 친구네!~~)
- User preference에도 사용 (선호도)
- 해킹의 가능성도 있다.

