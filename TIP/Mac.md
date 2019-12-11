# MAC OS

## 터미널 명령어
### 사용하고 있는 포트(port) 정보 보기

```
$ sudo lsof -PiTCP -sTCP:LISTEN
```

```
COMMAND   PID        USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
UserEvent    90        root  135u  IPv6 0x2c400a513681fe6b      0t0  TCP [fe80:4::aede:48ff:fe00:1122]:49158 (LISTEN)
Code\x20H   853 parksungbum   47u  IPv4 0x2c400a514a049e03      0t0  TCP *:5500 (LISTEN)
node      13122 parksungbum   24u  IPv4 0x2c400a514a046d1b      0t0  TCP *:8080 (LISTEN)
```

- [참고 링크](https://apple.stackexchange.com/questions/117644/how-can-i-list-my-open-network-ports-with-netstat)

### 프로세스 죽이기

```
$ sudo kill -9 PID
$ kill -9 PID
```

- [참고 링크](https://stackoverflow.com/questions/903076/how-to-kill-a-process-in-macos)
