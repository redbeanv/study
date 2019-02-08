# mac 내장 apache 설정법

## 아파치 구동방법
- 구동: sudo apachectl start
- 정지: sudo apachectl stop
- 재구동: sudo apachectl restart

## 기본 설정
- 설정 변경 없이 구동시 /Library/WebServer/Documents/index.html.en 실행  
메시지 출력 **It works!**
- Root 폴더 수정
    - 경로: /private/etc/apache2/httpd.conf
    - 설정 내용: 아래 코드를 찾아 경로 변경  
    ~~~
    DocumentRoot "/Library/WebServer/Documents"  
    <Directory "/Library/WebServer/Documents">
    ~~~
- 위 방법은 루트를 병경하는 방법으로 좋은 방법은 아님.

## 유저 경로 추가
- /private/etc/apache2/httpd.conf 의 아래 코드 주석 해제  
    ~~~
    LoadModule userdir_module libexec/apache2/mod_userdir.so  
    Include /private/etc/apache2/extra/httpd-userdir.conf
    ~~~
- /private/etc/apache2/extra/httpd-userdir.conf 에서 아래 코드 수정 및 주석 해제
    ~~~
    UserDir Sites // 아래와 같이 변경 또는 /Users/사용자이름/Sites 폴더 생성 후 사용가능
    UserDir /Users/redbean/git/genie.resort.web/b2b_resort_web/webapp
    ~~~
    ~~~
    Include /private/etc/apache2/users/*.conf //주석 해제
    ~~~
- /private/etc/apache2/users/사용자이름.conf 생성 후 다음 코드 추가
    ~~~
    // 인터넷 검색에선 대부분 DocumentRoot를 같이 쓰라는 말이 없음
    // DocumentRoot는 js,css 등을 모아둔 resource 위치를 잡아야 할 경우 사용
    DocumentRoot "/Users/redbean/git/genie.resort.web/b2b_resort_web/webapp/"
    <Directory "/Users/redbean/git/genie.resort.web/b2b_resort_web/webapp/">
        Options Indexes MultiViews
        AllowOverride None
        Require all granted
    </Directory>
    ~~~
---
## 추가내용
- 2019-02-07 맥 모하비로 os업그레이드 후 httpd.conf, httpd-userdir.conf가 초기화 됨.