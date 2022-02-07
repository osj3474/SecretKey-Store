# Private Repo 넣기



1. 중요한 키들은 private repo를 만들어서 관리를 합니다.

2. 그리고 public repo에 다음 명령어를 입력합니다.

   ```shell
   git submodule add [repo주소]
   ```

3. 이렇게 해두면 다른 사람들은 못 보고, owner권한 있는 사람만 볼 수 있답니다~!

<br />

cf) git action이랑 함께 사용하는 경우라면

```yaml
- uses: actions/checkout@v2
  with:
  token: ${{ secrets.REPO_ACCESS_TOKEN }}
  submodules: true
```

<br />

access token은 https://github.com/settings/tokens 로 들어가서 repo, workflow를 체크하고 발급합시다.

그리고 git secret에 넣어두세요:)

