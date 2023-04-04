# anvil_runtime
Anvil local runtime 구성 가이드 프로젝트

## git ssh 설정
1. cmd 창에서 ```ssh-keygen``` 입력
2. 모든 항목에 대해 빈 칸으로 enter 입력 (기본값 설정)
3. 파일 탐색기에서 ```C:/Users/{user name}/.ssh``` 경로로 이동
4. 해당 경로의 ```id_rsa.pub``` 파일을 에디터(vscode, notepad, notepad++,...)로 열기
5. 내려받을 Anvil Package를 열고 왼쪽 하단의 ```view history``` 클릭
6. ```clone with git``` 버튼을 클릭 후 가운데 텍스트 박스에 ```id_rsa.pub```파일 내용물 전부를 붙여넣고 ```save```버튼 클릭

    여러 사람이 해당 Anvil Package에 접근하고자 하는 경우, 각 유저의 id_rsa.pub를 해당 Anvil Package에 추가하면 됨   
    각 ssh public key는 개행으로 구분

## git에서 소스 받기
##### 반드시 anvil_runtime/app 에서 실행!
```
cd app
git clone {your anvil git url} {Package Name}
```

git ssh 설정을 했다면 비밀번호 없이 진행 됨   
계정 비밀번호 인증이 필요한 경우 anvil 계정의 비밀번호 입력

의존성이 필요한 경우 해당 의존성 Package 또한 git으로 app 하위에 내려받아야 함   
의존성 Package가 단일 yaml 파일인 경우, Anvil에 신규 Package로 import 후 git으로 내려받으면 됨

## 소스의 pip 의존성 추가
```
app/pip_list.txt
```

위 파일에 설치가 필요한 pip 패키지 추가(빈 줄 있으면 안 됨)   
패키지는 구동 시 사용할 모든 것을 추가해야하며, 의존성 Package에 사용되는 것도 추가해야함

## Docker 설정
### 경로 및 의존성 설정
의존성 Package가 있는 경우, `app/config.yaml`을 변경 또는 추가

의존성 ID가 확인이 필요한 경우, `{Main Package Name}/anvil.yaml`의 `dependencies: -app-id` 확인


`{Main Package Name}/anvil.yaml` 에 의존성 ID가 명시되지 않았고 이를 추가하고 싶은 경우,   
Anvil의 url을 확인하면 app id를 확인할 수 있음   
(ex: <code><span>https://</span>anvil.works/build#app:<b>JDNI6PLCC6MN4VHQ</b></code>)

또는 `{Package Name}`을 그대로 `app-id`로 사용해도 됨   
이 경우, 해당 의존성 Package를 사용하는 `anvil.yaml`을 수정

```
# example config.yaml
dep-id:
  "6E677T5CUOKCOUVN": "DataManager"
  # or
  "DataManager": "DataManager"
```

```
# example {Main Pacakge Name}.yaml
dependencies:
- app_id: 6E677T5CUOKCOUVN
# or
- app_id: DataManager
```

### .env 설정
{Main Package Name}을 수정
```
MainPackage=dsme
```

### 실행
``` docker-compose up -d ```

## 접근
[http://localhost:3030](http://localhost:3030)
#

파일이 대규모로 변하지 않았다면, 도커 실행 중 `git pull`을 통해 소스를 내려받고 새로고침을 하면 반영됨

실행 중 Error log는 Docker console에 출력 됨

# See Also
=================

[git에 ssh 연결](https://docs.microsoft.com/ko-kr/azure/devops/repos/git/use-ssh-keys-to-authenticate?view=azure-devops)

[Pandas DataFrame 이슈 해결](https://velog.io/@moey920/Pandas-DataFrame-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0)

[anvil-runtime](https://github.com/anvil-works/anvil-runtime/blob/master/README.md)

[anvil-runtime/getting-started](https://github.com/anvil-works/anvil-runtime/blob/master/doc/getting-started.md)

[anvil-runtime/dependencies](https://github.com/anvil-works/anvil-runtime/blob/master/doc/dependencies.md)

=================

