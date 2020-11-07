# Pod

- Pod
  - pea pod, 밀접하게 결합된 하나 이상의 애플리케이션 컨테이너

## What is a Pod?

- 도커 개념 측면에서, 파드는 공유 네임스페이스와 공유 파일시스템 볼륨이 있는 도커 컨테이너 그룹과 비슷하다.

## Using Pods

- Deployment,Job 같은 워크로드 리소스를 사용하여 생성한다. 상태 추적이 필요하면 StatefulSet
- Pods that run a single container
- Pods that run multiple containers that need to work together
- Grouping multiple co-located and co-managed containers in a single Pod is a relatively advanced use case. You should use this pattern only in specific instances in which your containers are tightly coupled.
- 복제된 파드는 일반적으로 워크로드 리소스와 해당 컨트롤러에 의해 그룹으로 생성되고 관리(스케일링과 자동 복구)된다.
- 초기화 컨테이너는 앱 컨테이너가 시작되기 전에 실행되고 완료된다.
- 파드에 속한 컨테이너에 네트워킹과 스토리지라는 두 가지 종류의 공유 ​​리소스를 제공

## Working with Pods

## Pods and controllers

- 리소스에 대한 컨트롤러는 파드 장애 시 복제 및 롤아웃과 자동 복구를 처리한다.
- 파드를 관리하는 워크로드 리소스
  - 디플로이먼트
  - 스테이트풀셋
  - 데몬셋(DaemonSet)
  
## Pod templates

- 디플로이먼트 컨트롤러는 실행 중인 파드가 각 디플로이먼트 오브젝트에 대한 현재 파드 템플릿과 일치하는지 확인한다. 템플릿이 업데이트된 경우, 디플로이먼트는 기존 파드를 제거하고 업데이트된 템플릿을 기반으로 새로운 파드를 생성해야 한다. 각 워크로드 리소스는 파드 템플릿의 변경 사항을 처리하기 위한 자체 규칙을 구현한다.
- 노드에서 kubelet은 파드 템플릿과 업데이트에 대한 상세 정보를 직접 관찰하거나 관리하지 않는다. 이러한 상세 내용은 추상화된다. 이러한 추상화와 관심사 분리(separation of concerns)는 시스템 시맨틱을 단순화하고, 기존 코드를 변경하지 않고도 클러스터의 동작을 확장할 수 있게 한다.

## Resource sharing and communication

### Storage in Pods

- 파드의 모든 컨테이너는 공유 볼륨에 접근할 수 있으므로, 해당 컨테이너가 데이터 공유
- 볼륨은 내부 컨테이너 중 하나를 다시 시작해야 하는 경우 파드의 영구 데이터를 유지하도록 허용

### Pod networking

- Inside a Pod (and only then), the containers that belong to the Pod can communicate with one another using localhost

## Privileged mode for containers

## Static Pods

- kubelet 데몬에 의해 직접 관리
- 정적 파드의 주요 용도는 자체 호스팅 컨트롤 플레인을 실행

# Pod Lifecycle

- Pending - Running - Succeeded or Failed
- 파드는 파드의 수명 중 한 번만 스케줄된다. 파드가 노드에 스케줄(할당)되면, 파드는 중지되거나 종료될 때까지 해당 노드에서 실행된다.

## Pod lifetime

- 파드는 자체적으로 자가 치유되지 않는다. 파드가 노드에 스케줄된 후에 실패하거나, 스케줄 작업 자체가 실패하면, 파드는 삭제된다. 마찬가지로, 파드는 리소스 부족 또는 노드 유지 관리 작업으로 인해 축출되지 않는다. 쿠버네티스는 컨트롤러라 부르는 하이-레벨 추상화를 사용하여 상대적으로 일회용인 파드 인스턴스를 관리하는 작업을 처리한다.
- UID로 정의된 특정 파드는 다른 노드로 절대 "다시 스케줄"되지 않는다. 대신, 해당 파드는 사용자가 원한다면 이름은 같지만, UID가 다른, 거의 동일한 새 파드로 대체될 수 있다.

## Pod phase

- Pending: 파드가 쿠버네티스 클러스터에서 승인되었지만, 하나 이상의 컨테이너가 설정되지 않았고 실행할 준비가 되지 않았다. 여기에는 파드가 스케줄되기 이전까지의 시간 뿐만 아니라 네트워크를 통한 컨테이너 이미지 다운로드 시간도 포함된다.	
- Running: 파드가 노드에 바인딩되었고, 모든 컨테이너가 생성되었다. 적어도 하나의 컨테이너가 아직 실행 중이거나, 시작 또는 재시작 중에 있다.
- Succeeded: 파드에 있는 모든 컨테이너들이 성공적으로 끝났고, 재시작되지 않을 것이다.
- Failed: 파드에 있는 모든 컨테이너가 종료되었고, 적어도 하나 이상의 컨테이너가 실패로 종료되었다. 즉, 해당 컨테이너는 non-zero 상태로 빠져나왔거나(exited) 시스템에 의해서 종료(terminated)되었다.
- Unknown: 어떤 이유에 의해서 파드의 상태를 얻을 수 없다. 이 단계는 일반적으로 파드가 실행되어야 하는 노드와의 통신 오류로 인해 발생한다.

## Container states

- Waiting, Running, Terminated
- Hook: postStart, preStop

## Container restart policy

- Always(Default), OnFailure, Never
- 5분으로 제한되는 지수 백오프 지연(10초, 20초, 40초, …)으로 컨테이너를 재시작한다. 컨테이너가 10분 동안 아무런 문제없이 실행되면, kubelet은 해당 컨테이너의 재시작 백오프 타이머를 재설정한다.

## Pod conditions

- PodScheduled: 파드가 노드에 스케줄되었다.
- ContainersReady: 파드의 모든 컨테이너가 준비되었다.
- Initialized: 모든 초기화 컨테이너가 성공적으로 시작되었다.
- Ready: 파드는 요청을 처리할 수 있으며 일치하는 모든 서비스의 로드 밸런싱 풀에 추가되어야 한다.

## Pod readiness

## Container probes

- ExecAction: Executes a specified command inside the container. The diagnostic is considered successful if the command exits with a status code of 0.
- TCPSocketAction: Performs a TCP check against the Pod's IP address on a specified port. The diagnostic is considered successful if the port is open.
- HTTPGetAction: Performs an HTTP GET request against the Pod's IP address on a specified port and path. The diagnostic is considered successful if the response has a status code greater than or equal to 200 and less than 400.

- Success: 컨테이너가 진단을 통과함.
- Failure: 컨테이너가 진단에 실패함.
- Unknown: 진단 자체가 실패하였으므로 아무런 액션도 수행되면 안됨.

- livenessProbe: 컨테이너가 동작 중인지 여부를 나타낸다. 만약 활성 프로브(liveness probe)에 실패한다면, kubelet은 컨테이너를 죽이고, 해당 컨테이너는 재시작 정책의 대상이 된다. 만약 컨테이너가 활성 프로브를 제공하지 않는 경우, 기본 상태는 Success이다.

- readinessProbe: 컨테이너가 요청을 처리할 준비가 되었는지 여부를 나타낸다. 만약 준비성 프로브(readiness probe)가 실패한다면, 엔드포인트 컨트롤러는 파드에 연관된 모든 서비스들의 엔드포인트에서 파드의 IP주소를 제거한다. 준비성 프로브의 초기 지연 이전의 기본 상태는 Failure이다. 만약 컨테이너가 준비성 프로브를 지원하지 않는다면, 기본 상태는 Success이다.

- startupProbe: 컨테이너 내의 애플리케이션이 시작되었는지를 나타낸다. 스타트업 프로브(startup probe)가 주어진 경우, 성공할 때까지 다른 나머지 프로브는 활성화되지 않는다. 만약 스타트업 프로브가 실패하면, kubelet이 컨테이너를 죽이고, 컨테이너는 재시작 정책에 따라 처리된다. 컨테이너에 스타트업 프로브가 없는 경우, 기본 상태는 Success이다.

## Termination of Pods

- 파드의 컨테이너 중 하나가 preStop 훅을 정의한 경우, kubelet은 컨테이너 내부에서 해당 훅을 실행한다. 유예 기간이 만료된 후 preStop 훅이 계속 실행되면, kubelet은 2초의 작은 일회성 유예 기간 연장을 요청한다.
- 참고: preStop 훅을 완료하는 데 기본 유예 기간이 허용하는 것보다 오랜 시간이 필요한 경우, 이에 맞게 terminationGracePeriodSeconds 를 수정해야 한다.

## Init Containers

### 초기화 컨테이너 이해하기

- initContainers에 선언 된 컨테이너들이 순차대로 실행된다.

### 초기화 컨테이너 사용하기

- 유틸리티 또는 초기 셋업 맞춤 코드 (sed, awk, python, dig)
- 앱 컨데이터한테 없는 시크릿 접근 권한 가능
- 순차 실행

# 파드 토폴로지 분배 제약 조건

- 노드 레이블
  
## 파드의 분배 제약 조건

- spec.topologySpreadConstraints
  - maxSkew: 파드가 균등하지 않게 분산될 수 있는 정도
  - topologyKey: 노드 레이블 키
  - whenUnsatisfiable: DoNotSchedule, ScheduleAlways
  - labelSelector
- PodAffinity, PodAntiAffinity

## 파드 프리셋

- 파드프리셋은 파드 생성 시간에 파드에 특정 정보를 주입하기 위한 오브젝트이다. 해당 정보에는 시크릿, 볼륨, 볼륨 마운트, 환경 변수가 포함될 수 있다.

### 파드 프리셋 이해하기

- 파드프리셋은 파드 생성 시간에 파드에 추가적인 런타임 요구사항을 주입하기 위한 API 리소스이다.

### 클러스터에서 파드프리셋 활성화하기

## 중단

### 자발적 중단과 비자발적 중단

- 비자발적 중단 으로 부른다. 예시:
  - 노드를 지원하는 물리 머신의 하드웨어 오류
  - 클러스터 관리자의 실수로 VM(인스턴스) 삭제
  - 클라우드 공급자 또는 하이퍼바이저의 오류로 인한 VM 장애
  - 커널 패닉
  - 클러스터 네트워크 파티션의 발생으로 클러스터에서 노드가 사라짐
  - 노드의 리소스 부족으로 파드가 축출됨

- 자발적 중단
  - 복구 또는 업그레이드를 위한 노드 드레이닝.
  - 클러스터의 스케일 축소를 위한 노드 드레이닝
  - 노드에 다른 무언가를 추가하기 위해 파드를 제거.

## 임시 컨테이너

# Controller

## 디플로이먼트

- Pod-template-hash 레이블
  - 이 레이블은 디플로이먼트의 자식 레플리카셋이 겹치지 않도록 보장한다. 레플리카셋의 PodTemplate 을 해싱하고, 해시 결과를 레플리카셋 셀렉터, 파드 템플릿 레이블 및 레플리카셋 이 가질 수 있는 기존의 모든 파드에 레이블 값으로 추가해서 사용하도록 생성한다.

### 디플로이먼트 업데이트

- .spec.template이 변경된 경우에만 디플로이먼트의 롤아웃이 트리거(trigger)
- 롤 아웃 시 최대 25% 중단
- 롤 아웃 시 최대 125%  생성 제한

### 롤오버(일명 인-플라이트 다중 업데이트)

- 예를 들어 디플로이먼트로 nginx:1.14.2 레플리카를 5개 생성을 한다. 하지만 nginx:1.14.2 레플리카 3개가 생성되었을 때 디플로이먼트를 업데이트해서 nginx:1.16.1 레플리카 5개를 생성성하도록 업데이트를 한다고 가정한다. 이 경우 디플로이먼트는 즉시 생성된 3개의 nginx:1.14.2 파드 3개를 죽이기 시작하고 nginx:1.16.1 파드를 생성하기 시작한다. 이것은 과정이 변경되기 전 nginx:1.14.2 레플리카 5개가 생성되는 것을 기다리지 않는다.

### 디플로이먼트의 롤아웃 기록 확인

- kubectl rollout history deployment.v1.apps/nginx-deployment
- kubectl rollout undo deployment.v1.apps/nginx-deployment (--to-revision=2)

## 디플로이먼트 스케일링

### 비례적 스케일링(Proportional Scaling)

## 레플리카셋
## 스테이트풀셋
## 데몬셋
## 잡
## 가비지(Garbage) 수집
## 완료된 리소스를 위한 TTL 컨트롤러
## 크론잡
## 레플리케이션 컨트롤러