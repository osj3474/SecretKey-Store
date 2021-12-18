# (2) Github Actions

: password정보를 Settings탭의 Secrets에 저장하고, Github Actions 스크립트로 **파일을 치환하는 방법**입니다.

예시)

***appplication.yml*** 라는 파일에 db정보가 있다고 해봅시다.

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://~
    username: sangjin
    password: 1111
```

이 때, Github Actions 스크립트를 이용한다면, yml파일을 다음과 같이 보호하여 올려도 배포에 문제가 없습니다.

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://~
    username: {username}
    password: {password}
```



<br />

### 제일 중요한 사용법!

1. 숨길 영역 표시

   ***application.yml***

   ```yaml
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://~
       username: {username}   # 여기!
       password: {password}   # 여기!
   ```

2. Github Actions 스크립트 작성하기

   파일 안의 특정 문자열을 치환하는 방법을 사용할 것입니다.

   ```shell
   sed -e s/[찾을문자열]/[바꿀문자열]/g -e s/[찾을문자열]/[바꿀문자열]/g [파일명] > [임시파일명]
   mv [임시파일명] [파일명]
   ```

   <br />

   ***deploy.yml*** 

   => `translate` 부분을 보시면 됩니다.

   ```yml
   name: secret-test
   
   on:
     push:
       branches: [ master ]
   
   jobs:
     build:
       runs-on: ubuntu-latest
   
       steps:
         - uses: actions/checkout@v2
         
         - name: translate
           run: |
             cd src/main/resources
             sed -e s/{username}/${{ secrets.username }}/g -e s/{password}/${{ secrets.password }}/g application.yml > application.yml.tmp
             mv application.yml.tmp application.yml
             
         - name: Deploy to Heroku
           uses: AkhileshNS/heroku-deploy@v3.12.12
           with:
             heroku_api_key: ${{ secrets.heroku_api_key }}
             heroku_email: ${{ secrets.heroku_email }}
             heroku_app_name: ${{ secrets.heroku_app_name }}
   ```

   `${{ secrets.[변수명] }}`  부분이 Settings탭의 Secrets에 저장된 변수을 의미합니다.

