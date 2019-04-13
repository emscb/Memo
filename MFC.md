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

### CString을 string으로

```c++
CString strSrc("내용");
std::string strDst;
strDst = strSrc;
```

### String, char 형변환

- string to char
  - `strcpy(char*, string.c_str());`
- char to string
  - `str(char);`

