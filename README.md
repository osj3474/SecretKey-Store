# SecretKey-Store

<br /><br />

### (1) git-secret

 : **파일을 암호화**해서 github에 올리는 방법입니다.

예시)

***secret.txt*** 라는 파일을 올리고자 한다고 해봅시다.

```
password = 111
aws = 222
```

이 때, git-secret을 사용하면, 공개 Repository에 ***secret.txt.secret*** 라는 이름으로 파일을 암호화해서 올릴 수 있습니다.

![image](https://user-images.githubusercontent.com/42775225/146629248-0641bd73-310c-4887-8368-384522509928.png)

제일 중요한 사용법!

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

   

cf) 추적 중지

```shell
git rm --cached [파일명]
```

cf) 복호화

```shell
git-secret reveal
```

