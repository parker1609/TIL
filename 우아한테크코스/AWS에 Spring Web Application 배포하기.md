# AWS에 Spring Boot Web Application 배포하기
## 1. JAVA 설치
- OpenJDK 설치
  - OpenJDK는 URL 만료 기간이 없다.

```
wget https://download.java.net/java/GA/jdk12.0.2/e482c34c86bd4bf8b56c0b35558996b9/10/GPL/openjdk-12.0.2_linux-x64_bin.tar.gz
```

- 다운로드 받은 `OpenJDK.tar.gz` 압축 풀기(설치 완료)

```
tar -xvf openjdk-12.0.2_linux-x64_bin.tar.gz
```

- `ln` 명령을 통해 symbolic link 추가
  - `ln -s 원본폴더 링크이름`

```
ln -s jdk12.0.2 jdk
```

- 계정 Home 디렉토리의 `.bashrc`파일에 `JAVA_HOME/bin` 디렉토리 path로 설정한다.

```
vi ~/.bashrc
```

해당 파일에 아래와 같은 path 설정을 추가한다.

```
JAVA_HOME=/home/ubuntu/jdk
PATH=$PATH:$JAVA_HOME/bin
```

설정 적용 후 자바가 정상적으로 설치 되었는지 버전을 검사한다.

```
source ~/.bashrc
java -version
```

## 2. MYSQL 설치
현재 서버 프로그램은 8버전대의 Mysql을 사용하고 있지만 5.7버전으로도 동작을 한다. 그러므로 아래는 5.7버전 설치 방법이다.

- `sudo` 명령으로 MySQL을 설치할 수 있는 버전을 확인한다.

```
sudo apt-cache search mysql-server
```

- MySQL 설치

```
sudo apt-get install mysql-server-5.7
```

현재 환경에서 사용하는 AWS는 ubuntu의 비밀번호를 알 수 없으므로, MySQL을 설치할 때 비밀번호를 설정하지 않는다. 하지만 `sudo` 명령은 제공해주므로 아래와 같이 MySQL에 접속하여 필요한 `USER`와 `DATABASE`를 만들어 준다.

- MySQL 접속

```
sudo mysql
```

- 유저 생성

```
create user 'username'@'localhost' identified by 'password';
```

```
CREATE USER codemcd@localhost identified by '1234';
```

- 생성한 유저에게 모든 DB 및 테이블에 접근 권한 부여

```
grant all privileges on *.* to 'username'@'localhost';
```

```
grant all privileges on *.* to codemcd@localhost;
```

- 설정한 권한 적용

```
flush privileges;
```

- 데이터베이스 생성(utf8 설정을 해주어야 한글과 관련된 이슈가 발생하지 않는다.)

```
CREATE DATABASE db_name DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```

```
CREATE DATABASE blog DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```

## 3. GIT 설치 및 clone
- `git --version` 명령을 통해 git 버전이 보이는지 확인 후, 보이지 않는다면 설치한다.

```
sudo apt update
sudo apt install git
```

- 배포할 코드를 github 저장소에서 `clone`한다.

## 4. 서버 실행
- 복사한 코드를 빌드 한다.(gradle 파일이 있는 경로에서 수행)

```
./gradlew clean build
```

- Application을 실행한다.
  - 빌드 후 `build/libs` 디렉토리에서 `jar` 파일을 찾을 수 있다.
  - `java -jar "jar파일" &`
  - `-Dserver.port=8000`옵션을 통해 port를 변경할 수 있다.

```
java -jar $JAR_FILE_NAME $
```

- 서버를 시작하는 시간이 오래 걸리는 경우 `-Djava.security.egd` 옵션을 활용할 수 있다.
  - 참고 링크: <https://lng1982.tistory.com/261>

```
java -Djava.security.egd=file:/dev/./urandom -jar $JAR_FILE_NAME &
```


### 서버 배포 스크립트
- 참고 링크: <https://jojoldu.tistory.com/263>

```
#!/bin/bash

REPOSITORY=/home/ubuntu/git

cd $REPOSITORY/jwp-blog/

echo "> Git Pull"

git pull

echo "> 프로젝트 Build 시작"

./gradlew clean build

echo "> Build 파일 복사"

cp ./build/libs/*.jar $REPOSITORY/jwp-blog/

echo "> 현재 구동중인 애플리케이션 pid 확인"

CURRENT_PID=$(pgrep -f myblog-0.0.1)

echo "$CURRENT_PID"

if [ -z $CURRENT_PID ]; then
    echo "> 현재 구동중인 애플리케이션이 없으므로 종료하지 않습니다."
else
    echo "> kill -2 $CURRENT_PID"
    kill -15 $CURRENT_PID
    sleep 5
fi

echo "> 새 어플리케이션 배포"

JAR_NAME=$(ls $REPOSITORY/jwp-blog/ | grep 'myblog' | tail -n 1)

echo "> JAR Name: $JAR_NAME"

nohup java -jar $REPOSITORY/jwp-blog/$JAR_NAME &
```
