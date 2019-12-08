# 애플리케이션 관점에서 Sync VS Async
- 예를 들어 물건 주문하는데 0.5초, 주문 완료 후 메일발송에 2초가 걸린다고 하자.
  - Sync 응답 속도 = 2.5초
  - Async 응답 속도 = 메일 발송을 Async로 할 경우 0.5초 + 알파


# I/O 관점에서 Sync/Async, Blocking/Non-Blocking
- Sync/Async은 대표적으로 애플리케이션을 기준으로 한다.(호출하는 함수)
- Blocking/Non-Blocking은 대표적으로 커널을 기준으로 한다.(호출되는 함수)


## Sync VS Async
- Sync: 호출하는 함수(애플리케이션)가 호출되는 함수의 작업이 끝나서 결과값을 반환하기를 기다리거나, 지속적으로 호출되는 함수에게 확인 요청을 하면서 신경쓰는 경우이다.
- Async: 호출하는 함수가 호출되는 함수에게 작업을 맡겨놓고 신경쓰지 않는 경우이다.(대표적으로 callback을 호출되는 함수에게 전달하는 경우가 있다.)

## Blocking VS Non-Blocking
- Blocking: 호출된 함수(커널)가 자기 자신의 작업이 모두 끝나기 전까지 제어권을 넘겨주지 않는 경우이다.
- Non-Blocking: 호출된 함수가 작업 완료 여부와 상관없이 바로 반환하여 제어권을 호출한 함수에게 넘겨주어 호출한 함수가 다른 일을 할 수 있도록 해주는 경우이다.

## 예시
### Synchronous Blocking I/O

<img width="512" alt="synchronous_blocking_I_O" src="https://user-images.githubusercontent.com/34755287/68672822-c0bb7c80-0595-11ea-91da-fe91d2853985.png">

- Synchronous: 애플리케이션이 커널에게 작업 요청을 하고 커널의 작업이 끝날 때까지 기다린다.
- Blocking: 커널이 자기 자신의 작없이 모두 끝날 때까지 제어권을 가지고 있다.
- 예제 코드

```python
device = IO.open()
data = device.read() # 이 thread는 데이터를 읽을 때까지 아무 일도 할 수 없음
print(data)
```

### Synchronous Non-Blocking I/O

<img width="451" alt="synchronous_non-blocking_I_O" src="https://user-images.githubusercontent.com/34755287/68672823-c0bb7c80-0595-11ea-81e9-08e3a88b4c69.png">

- Synchronous: 애플리케이션이 커널에게 작업 요청을 하고 이 작업이 끝났는지 지속적으로 확인하고 있다.
- Non-Blocking: 애플리케이션으로부터 요청을 받은 커널은 작업 완료 여부와 상관없이 바로 반환하여 제어권을 애플리케이션에게 넘겨준다. 커널의 작업이 완료되면 작업 결과를 애플리케이션에게 반환한다.
- 대표적인 예로는 멀티플랙싱을 수행하는 `select()`, `epoll()` 함수가 있다.
- 예제 코드

```python
device = IO.open()
ready = False
while not ready:
    print("There is no data to read!")
    # 다른 작업을 처리할 수 있음
    ready = IO.poll(device, IO.INPUT, 5) # while 문 내부의 다른 작업을 다 처리하면 데이터가 도착했는지 확인한다.
data = device.read()
print(data)
```

### Asynchronous Non-Blocking I/O

<img width="484" alt="asynchronous_non-blocking_I_O" src="https://user-images.githubusercontent.com/34755287/68672821-c0bb7c80-0595-11ea-86f6-d1acacd5befc.png">

- Asynchronous: 애플리케이션은 커널에게 작업 요청을 보낸 직후 바로 자신의 작업을 수행한다.
- Non-Blocking: 작업이 끝나면 애플리케이션에게 시그널 또는 콜백을 보낸다.
- 대표적인 예로는 윈도우에서 멀티플랙싱을 수행하는 IOCP가 있다.(`epoll()`보다 성능이 좋다.)
- 예제 코드

```python
ios = IO.IOService()
device = IO.open(ios)

def inputHandler(data, err):
    "Input data handler"
    if not err:
        print(data)

device.readSome(inputHandler)
# 이 thread는 데이터가 도착했는지 신경쓰지 않고 다른 작업을 처리할 수 있다.
ios.loop()
```

### Asynchronous Blocking I/O
- 거의 존재하지 않는다.

## 더 알아볼 점
- Node.js
- Spring webflux
