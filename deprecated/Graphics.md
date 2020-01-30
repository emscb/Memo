### 불 켜기

> http://jenemia.tistory.com/10

### 이동 (Translation)

- `glMultiMatrixf(const GLdouble* m)`
    - m : 16개의 연속적인 값들이 Column 우선 순으로 저장되어 있는 4X4 행렬에 대한 포인터
    - 단위행렬에다가 제일 오른쪽 열이 Tx, Ty, Tz, 1.0이다.

- `glTranslatef(GLfloat x, GLfloat y, GLfloat z)`
    - 간단하게 x, y, z값을 줘서 이동시킬 수 있다.

### 회전 (Rotation)

- `glRotatef(GLfloat angle, GLfloat x, GLfloat y, GLfloat z)`
    - 각도(angle, degree)과 중심축을 정해준다.

### 크기 조절 (Scale)

- `glScalef(GLfloat x, GLfloat y, GLfloat z)`
    - 각 축에 대한 scale 값

### 좌표계와 시점 변환

- `glMatrixMode(GLenum mode)`
    - GL_MODELVIEW : Modelview Matrix stack에 후속 matrix 연산을 적용, 모델링에 대한 객체, 카메라의 위치 및 방향 등에 대한 기하 변환을 지정한다. 객체를 변환하고 제어하는 modeling transfomation이라고도 한다.
    - GL_PROJECTION : Projection Matrix stack에 적용, 모델링을 최종적으로 화면에 어떻게 표시할 것인지를 결정, 직교 투영(glOrtho)이나 원근 투영(gluPerspective) 등과 같이 구분된다.
    - GL_TEXTURE : Texture Matrix stack에 적용, texture mapping을 적용하는 과정에서 texture 좌표를 지정할 때 곱해진다.

### 직교 좌표계

* `gluLookAt()`을 구성하는 parameter들의 의미
    * eye : 카메라의 위치
    * at : 보는 목표지점
    * up : 카메라의 기울임, 위방향 벡터

## 투영

### 정사 투영

- `glOrtho(GLdouble left, right, bottom, top, near, far)`
    - left, right : 수직 절단면을 위한 X축 좌표
    - bottom, top : 수평 절단면을 위한 Y축 좌표
    - near, far : 전후방 절단면을 위한 -Z축 방향의 거리 (양수)

### 원근 투영

- `glFrustum(GLdouble left, right, bottom, top, near, far)`

- `gluPerspective(GLdouble fov, aspect, near, far)`
    - fov : Field of View angles, 시야각
    - aspect : viewport의 종횡비, W/H

## Viewport 변환

### GLUT 좌표계에서 윈도우의 위치 및 크기 (Position & Size)

* Window Coordinate System (GLUT)
    * 왼쪽 위가 원점, 가로가 X축, 세로가 Y축
    * `glutInitWindowPosition(x,y)` : x,y위치에서 윈도우의 왼쪽 위가 시작
    * `glutInitWindowSize(Width, Height)` : 저 크기로 시작 (픽셀 단위)

### GL 좌표계에서의 Viewport

* glViewport(Left, Bottom, Width, Height)` : 왼쪽 밑이 원점으로 left, bottom위치에서 시작하여 width, height 크기만 사용

### Window 변형에 따른 객체의 왜곡 현상 방지

```
glViewport(0,0,(Glsizei)w, (Glsizei)h);
gluPerspective(30, (Gldouble)w / (Gldouble)h, 1.0, 50.0);
```

# Callback programming

## Display Callback

### Display callback이 자동으로 호출되는 경우

* 처음 Window를 열 때
* Window의 위치를 이동시킬 때
* Window의 크기를 변경할 때
* 앞 Window에 가려서 안 보이던 뒤 Window가 활성화되어 앞으로 나타날 때
* `glutPostRedisplay` 함수에 의해 Event Queue에 Flag가 게시될 때

- `glutDisplayFunc(void 함수)`

## Reshape Callback

### Reshape callback이 자동으로 호출되는 경우

* 처음 Window를 열 때
* Window의 위치를 이동시킬 때
* Window의 크기를 변경할 떄

- `glutReshapeFunc(void 함수(width, height))`
    - 단위는 픽셀

## Keyboard Callback

- `glutKeyboardFunc(void 함수(unsigned char key, int x, int y))`
    - key : 키보드로 부터 입력되는 ASCII 문자
    - x,y : 마우스의 GLUT 좌표계의 위치

- `glutSpecialFunc(void 함수(int key, int x, int y)`
    - key : Special key를 위한 GLUT_KEY_\*의 상수

| 키                | 넘버     |
| ----------------- | -------- |
| F1~F12            | 1~12     |
| 좌상우하          | 100~103  |
| Page up / down    | 104, 105 |
| HOME, END, INSERT | 106~108  |

### Ctrl, Alt, Shift 등과 함께 사용하는 경우

* `(glutGetModifiers() == GLUT_ACTIVE_CTRL) && (key == 'C')`와 같이 사용

## Mouse Callback

- `glutMouseFunc(void 함수(int button, int state, int x, int y)`
    - button, state

| Mouse Buttons      | 넘버 |
| ------------------ | ---- |
| GLUT_LEFT_BUTTON   | 0    |
| GLUT_MIDDLE_BUTTON | 1    |
| GLUT_RIGHT_BUTTON  | 2    |

| Mouse Button State | 넘버 |
| ------------------ | ---- |
| GLUT_DOWN          | 0    |
| GLUT_UP            | 1    |

- `glutMotionFunc()`
    - 마우스가 움직일 때 호출 (클릭이 되었을 때)
    - 클릭이 안되었을 때도 호출하는 것은 `glutPassiveMotionFunc()`
    - App내부에 마우스가 있을 때와 없을 때는 나누는 것은 `glutEntryFunc()`
        * state : GLUT_ENTERED, GLUT_LEFT

- `glutMouseWheelFunc(void 함수(int wheel, int direction, int x, int y)`
    - wheel : Mouse Wheel Number, Default : 0
    - direction : +1은 전방, -1은 후방

## Menu Callback

- `glutCreateMenu(void 함수(int value))`
    - value : Entry 값, 이름

- `glutSetMenu(int id)`
    - id : 메뉴를 생성하기 위한 식별자

- `glutAddMenuEntry(char* name, int value)`
    - name : 메뉴 항목(entry)에 보여주기 위한 ASCII 문자열
    - value : Entry가 선택되어질 때 Menu의 callback 함수를 돌려주기 위한 정수 값

- `glutAttachMenu(int button)`
    - <-> Detach
    - button에다가 메뉴 부착

- `glutAddSubMenu(char* name, int menu)`
    - menu : 서브메뉴 항목으로 나열하기 위한 Menu의 식별자

## Idle Callback

- `glutIdleFunc(void 함수)`
    - 쉬는 동안 (아무것도 안하는 동안) 계속 호출
    - 물체를 움직이게 할 수 있다. (조금씩 이동)

# GLUT Modeling

### 구 그리기

* `glutWireSphere(Gldouble radius, Glint slices, Glint stacks)`
* `glutSolidSphere(Gldouble radius, Glint slices, Glint stacks)`
* slices : Z축 주변의 분할 개수 (세로로 쪼개기)
* stacks : Z축을 따르는 분할 개수 (가로로 쪼개기)

### 정육면체 그리기

* `glutWireCube(Gldouble size)`
* `glutSolidCube(Gldouble size)`
* size : 끝에서 끝까지의 거리 (한 선분의 3^1/2배)

### 도넛 그리기

* `glutWireTorus(Gldouble innerRadius, Gldouble outerRadius, Glint nsides, Glint rings)`
* `glutSolidTorus(Gldouble innerRadius, Gldouble outerRadius, Glint nsides, Glint rings)`
* innerRadius : 제일 바깥부터 안쪽 원까지
* outerRadius : 중심부터 안쪽 원까지
* nsides : 동그랗게 쪼개기
* rings : 단면으로 쪼개기

### 원뿔 그리기

* `glutWireCone(Gldouble base, Gldouble height, Glint slices, Glint stacks)`
* `glutSolidCone(Gldouble base, Gldouble height, Glint slices, Glint stacks)`
* base : 밑면의 직경

### 정사면체 그리기

* `glutWireTetrahedron()`
* `glutSolidTetrahedron()`
* Parameter 없음

### 정팔면체 그리기

* `glutWireOctahedron()`
* `glutSolidOctahedron()`

### 정십이면체 그리기

* `glutWireDodecahedron()`
* `glutSolidDodecahedron()`

### 정이십면체 그리기

* `glutWireIcosahedron()`
* `glutSolidIcosahedron()`

