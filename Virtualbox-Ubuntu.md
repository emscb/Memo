## <a name="install_guest"></a>게스트 확장 이미지 설치

* `sudo apt-get install virtualbox-guest-additions-iso`

## <a name="share"></a>공유 폴더 마운트

* `sudo mkdir 공유할폴더`
* `sudo mount -t vboxsf 호스트폴더이름 게스트폴더이름`

## <a name="install_program"></a> 프로그램 컴파일 & 설치
* `make`로 내부 컴파일 실시
* `sudo make install` : 내용 설치, `sudo`로 했기 때문에 경로 없이 실행 가능

