* 백그라운드 잡
    * 실행 : `Start-Job -ScriptBlock {스크립트} -Name 잡이름`
    * 목록 : `Get-Job`
    * 상태
    	* `Receive-Job -Id 아이디 -Keep`
    	* `Receive-Job -Name 잡이름 -Keep`
    * 정지
    	* `Stop-Job -Id 아이디`
    	* `Stop-Job -Name 잡이름`

* 히스토리
	* 목록 : `Get-History`
	* 내보내기 : `Get-History | Export-Clixml history.xml`
	* 가져오기 : `Import-Clixml history.xml | Add-History`

* Putty - pscp
    * `pscp [options] [user@]host:source target`
    * 옵션

옵션|설명
-|-
-V|버전 정보 인쇄 및 종료
-pgpfp|PGP 키 지문 인쇄 및 종료
-p|파일 속성 유지
-q|통계를 표시하지 않음
-r|디렉토리를 재귀적으로 복사
-v|자세한 메시지 표시
-load sessname|저장된 세션의 설정 로드
-P|지정된 포트에 포트 연결
-l|지정된 사용자 이름으로 사용자 연결
-pw|지정된 비밀번호로 로그인
-1 -2|특정 SSWH 프로토콜 버전 강제 사용
-4 -6|IPv4 또는 IPv6 강제 사용
-C|압축 사용
-i key|인증에 사용할 개인 키 파일
-batch|모든 대화형 프롬프트 사용 안함
-unsafe|서버 측 와일드카드 허용 (위험)
-sftp|SFTP 프로토콜 강제 사용
-scp|SCP 프로토콜 강제 사용