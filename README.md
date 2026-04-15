# ROS2_Service

`service_demo`는 ROS 2 Service의 요청-응답 통신을 보여주는 예제 패키지입니다.  
서버 노드는 두 정수를 받아 더한 값을 반환하고, 클라이언트 노드는 서버에 요청을 보내 결과를 출력합니다.

## Service 개념

Service는 ROS 2에서 1회성 요청-응답을 처리할 때 사용하는 통신 방식입니다.

- Client: 요청을 보내는 쪽
- Server: 요청을 받아 처리하고 응답하는 쪽
- Service Type: 요청과 응답 데이터 형식

Topic과 달리 Service는 요청에 대한 결과를 직접 돌려받는 구조입니다.  
빠르게 끝나는 계산, 상태 조회, 단발성 명령 처리에 적합합니다.

## 패키지 구성

| 실행 파일 | 노드 이름 | 역할 |
| --- | --- | --- |
| `add_two_ints_server` | `add_two_ints_server` | `/add_two_ints` 서비스 제공 |
| `add_two_ints_client` | `add_two_ints_client` | 서버에 정수 두 개를 보내고 결과 출력 |

## 사용 인터페이스

- 서비스 이름: `/add_two_ints`
- 서비스 타입: `example_interfaces/srv/AddTwoInts`

요청 필드:

- `a` (`int64`)
- `b` (`int64`)

응답 필드:

- `sum` (`int64`)

## 빌드 방법

```bash
cd ~/ros2_ws
colcon build --packages-select service_demo
source install/setup.bash
```

## 실행 방법

먼저 서버를 실행합니다.

```bash
cd ~/ros2_ws
source install/setup.bash
ros2 run service_demo add_two_ints_server
```

다른 터미널에서 패키지에 포함된 클라이언트를 실행합니다.

```bash
cd ~/ros2_ws
source install/setup.bash
ros2 run service_demo add_two_ints_client
```

기본 클라이언트는 `10 + 32`를 요청하고 결과를 출력합니다.

## ROS 2 CLI로 직접 호출하기

CLI에서 바로 서비스를 호출할 수도 있습니다.

```bash
ros2 service list
ros2 service type /add_two_ints
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 7, b: 5}"
```

예상 응답:

```text
sum: 12
```

## 실행 흐름

1. 서버가 `/add_two_ints` 서비스를 생성합니다.
2. 클라이언트가 서버가 준비될 때까지 대기합니다.
3. 클라이언트가 `a`, `b` 값을 담은 요청을 보냅니다.
4. 서버가 합계를 계산해 `sum`으로 응답합니다.
5. 클라이언트가 결과를 받아 로그로 출력합니다.

## 학습 포인트

- Service는 짧고 명확한 요청-응답 작업에 적합합니다.
- 계산 요청, 설정 조회, 단발성 제어 명령에 자주 사용합니다.
- 중간 진행 상황이 필요하거나 오래 걸리는 작업은 Action이 더 적합합니다.
