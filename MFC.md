### Edit control

- 입력창 안에서 여러 라인을 입력하는 경우
  - 속성
    - Multiline : True
    - Want Return : True
- 스크롤
  - Multiline : True여야 함
  - Auto H/Vscroll : True
  - Horizontal scroll : True (가로)
  - Vertical scroll : True (세로)
- 결과값 가져오기

```C++
UpdateData(TRUE);
CString strName;
GetDlgItemText(IDC_에디트박스이름, strName);
```

- 결과값 입력하기

```c++
CString strValue = "내용";
SetDlgItemText(IDC_에디트박스이름, strValue);
```

- 

### 다이얼로그 띄우기

- `다이얼로그 삽입`으로 하나 만들고 그 창 클릭 > `클래스 생성`
- 클래스 만들고 부모 다이얼로그에다가 헤더 파일 include
- 객체 하나 만들고 `DoModal()`로 창띄우기

