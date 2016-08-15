# 시작하기

#### 이 튜토리얼을 찍고 내 리퍼지토리로 복사하세요.

[link](https://github.com/djangogirlsseoul/djangocupcakeshop) 오른쪽 위에 있는 Fork 버튼을 누릅니다.

![](fork_djangocupcakeshop.png)

`git clone`을 사용하여 여러분의 컴퓨터에 코드를 복사합니다.


![](clone_djangocupcake.png)

```bash
$ git clone https://github.com/<user_name>/djangocupcakeshop.git

```
  > 주의: `user_name` 이 부분에 여러분의 Github username을 적어주세요. 여러분의 레파지토리에 fork 되어야 해요!


Mac/Linux 환경에서는 terminal, window 환경에서는 prompt을 열고 djangocupcakeshop 폴더로 이동합니다.

```cd djangocupcakeshop ```

`cd djangocupcakeshop` 치고 djangocupcakeshop 폴더 안으로 이동합니다.

아래 command를 콘솔에 치고 가상환경을 만듭니다.

#### Windows:
```C:\Python35\python -m venv myvenv```
#### Mac:
```bash
$ python3 -m venv myvenv
```
#### Linux
```bash
$ virtualenv --python=python3.4 myvenv
```

가상환경을 활성화 시켜 볼까요?

#### Windows

```bash
myvenv\Scripts\activate
```
#### Mac/Linux
```bash
$ source myvenv/bin/activate 
```


터미널 또는 prompt에서 다음과 같이 보이는지 확인합니다.

```bash
(myvenv)...
```


아래 명령어를 이용해서 장고를 설치합니다.

```bash
$ pip install -r requirements.txt
```
`python manage.py migrate` 통해 데이터 베이스를 만듭니다.

다음 명령어를 쳐서 데이터베이스를 만듭니다!

```bash
$ python manage.py migrate
```

[데모를 4(b)](https://djangogirlsseoul.gitbooks.io/djangocupcakeshop_ko/content/demo.html) 단계부터 따라합니다.
