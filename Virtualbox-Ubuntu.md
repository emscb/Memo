## 게스트 확장 이미지 설치

* `sudo apt-get install virtualbox-guest-additions-iso`

## 공유 폴더 마운트

* `sudo mkdir 공유할폴더`
* `sudo mount -t vboxsf 호스트폴더이름 게스트폴더이름`

  * `sudo mount -t vboxsf ushare ./ushare`

  * mount할 때 마지막에

    `호스트의공유이름 마운트할위치`

    를 써주는데 호스트의공유이름과 동일한 파일이 명령을 타이핑한 현재 위치에 있어서 마운트가 안되더군요.

## 프로그램 컴파일 & 설치

* `make`로 내부 컴파일 실시
* `sudo make install` : 내용 설치, `sudo`로 했기 때문에 경로 없이 실행 가능

