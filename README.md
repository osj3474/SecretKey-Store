# SecretKey-Store

<br />

## (1) git-secret

 : **파일을 암호화**해서 github에 올리는 방법입니다.

예시)

***secret.txt*** 라는 파일을 올리고자 한다고 해봅시다.

```
password = 111
aws = 222
```

이 때, git-secret을 사용하면, 공개 Repository에 ***secret.txt.secret*** 라는 이름으로 파일을 암호화해서 올릴 수 있습니다.

![image](https://user-images.githubusercontent.com/42775225/146629248-0641bd73-310c-4887-8368-384522509928.png)

<br />

### 제일 중요한 사용법!

1. 설치 (macOS 기준)

   ```shell
   brew install gnupg
   brew install git-secret
   ```

2. gpg 키 만들기

   ```shell
   gpg --full-generate-key   // 혹은 gpg --gen-key
   ```

3. 레포지토리로 이동 후 git-secret 초기화

   ```shell
   cd [레포지토리]
   git-secret init
   ```

4. 이메일 입력

   ```shell
   git secret tell [본인 git 이메일]
   ```

5. 암호화가 필요한 파일 암호화 (git 추적 대상이면 안됨.)

   ```shell
   git-secret add [파일명]
   git-secret hide
   ```

6. push 하면 끝~!

<br />

cf) 추적 중지

```shell
git rm --cached [파일명]
```

cf) 복호화

```shell
git-secret reveal
```





<br /><br />

## (2) Github Actions

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

