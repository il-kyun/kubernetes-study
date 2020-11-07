# Images

- 컨테이너 이미지는 독립적으로 실행할 수 있고 런타임 환경에 대해 잘 정의된 가정을 만드는 실행 가능한 소프트웨어 번들이다.

## Image Name

- 프로덕션에서 컨테이너를 배포할 때는 latest 태그를 사용하지 않아야 한다. 실행 중인 이미지 버전을 추적하기가 어렵고 이전에 잘 동작하던 버전으로 롤백하기가 더 어렵다. 대신, v1.42.0 과 같은 의미있는 태그를 지정한다.

# Runtime Class

# Container Environment

# Container Lifecycle Hooks

- PostStart : This hook is executed immediately after a container is created. However, there is no guarantee that the hook will execute before the container ENTRYPOINT
- PreStop
  - This hook is called immediately before a container is terminated
  - It is blocking, meaning it is synchronous, so it must complete before the signal to stop the container can be sent.

### Hook handler implementations

- Exec: 컨테이너의 cgroups와 네임스페이스 안에서, pre-stop.sh와 같은, 특정 커맨드를 실행.
- HTTP: 컨테이너의 특정 엔드포인트에 대해서 HTTP 요청을 실행.

### Hook handler execution

- 이러한 동작은 PreStop 훅에 대해서도 비슷하게 일어난다. 만약 훅이 실행되던 도중에 매달려 있다면, 파드의 단계(phase)는 Terminating 상태에 머물고 해당 훅은 파드의 terminationGracePeriodSeconds가 끝난 다음에 종료된다. 만약 PostStart 또는 PreStop 훅이 실패하면, 그것은 컨테이너를 종료시킨다.
- 사용자는 훅 핸들러를 가능한 한 가볍게 만들어야 한다. 그러나, 컨테이너가 멈추기 전 상태를 저장하는 것과 같이, 오래 동작하는 커맨드가 의미 있는 경우도 있다.

### Hook delivery guarantees

- At least once