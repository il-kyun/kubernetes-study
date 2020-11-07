# 쿠버네티스란 무엇인가

    - 컨테이너화 된 워크로드와 서비스를 관리하기 위한 portable & extensible 한 플랫폼
    - 선언적 구성
    - [Borg, Google] (<https://storage.googleapis.com/pub-tools-public-publication-data/pdf/43438.pdf>)
    - 분산 시스템을 탄력적으로 실행하기 위한 프레임워크 제공 : 스케일링, 장애 조치, 배포 패턴
    - 서비스 디스커버리와 로드 밸런싱 
    - 스토리지 오케스트레이션
    - 자동화된 롤아웃과 롤백
    - 자동화된 빈 패킹(bin packing)
    - 자동화된 복구(self-healing)
    - 시크릿과 구성 관리

# 쿠버네티스 컴포넌트

- node: pod 관리

## Control Plane Components

- control plane: node & pod 관리
- 클러스터에 대한 전반적인 결정(ex:schdeduling), 이벤트(ex:replica condition)를 처리
- control plane components를 별도 머신에 관리
- kube-apiserver: control plane's front-end (수평 확장)
- etcd: Consistent and highly-available key value store
- kube-scheduler:
  - pod를 어떤 node에 할당 할 지 결정하는 컴포넌트 (individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.)
- kube-controller-manager: runs controller processes
- Node controller: 노드 다운 시 알림 및 응답
- Replication controller: replica 관리
- Endpoints controller: joins Services & Pods
- Service Account & Token controllers: 새로운 네임스페이스에 대한 기본 계정과 API 접근 토큰 생성
- cloud-controller-manager: 클라우드 provider api에 연동
- Node controller: 노드가 응답을 멈춘 후 클라우드 상에서 삭제되었는지 판별
- Route controller: 기본 클라우드 인프라에 경로 구성
- Service controller: 클라우드 제공 사업자 로드밸런서 관리

## Node Components

- 모든 노드에서 작동하고 pod 관리
- kubelet:
  - 각 노드에서 실행 되는 에이전트
  - pod에서 container가 확실하게 동작하도록 관리한다.
  - PodSpecs에 스펙에 따라서 container가 동작하게 한다.
  - 쿠버네티스에서 생성한게 아닌 container는 관리 안한다.
- kube-proxy:
  - 네트워크 프록시, service의 구현
  - node's 네트워크 규칙 유지 관리 ( 운영체제에 패킷 필터링 계층 있으면 사용하고 아니면 트래픽 포워드)
- container runtime: docker, containerd, cri-o
- addons:  쿠버네티스 리소스(데몬셋, 디플로이먼트 등)를 이용하여 클러스터 기능을 구현
- dns
- web ui
- container resource monitoring: 컨테이너에 대한 시계열 데이터 저장
- cluster-level logging: container log 저장

# 쿠버네티스 API

- http api 제공
- api 로 pods, namespaces, config maps and events 오브젝트의 상태를 변경할 수 있다. 

# 쿠버네티스 오브젝트로 작업하기

## 쿠버네티스 오브젝트 이해하기

- 오브젝트: 쿠버네티스 시스템에서 영속성을 가지는 개체
- What containerized applications are running (and on which nodes)
- The resources available to those applications
- The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance
- spec: object 생성 시 원하는 특징을 설정
- status: 쿠버네티스 시스템과 컴포넌트에 의해서 제공된다. control plane은 모든 오브젝트의 실제 상태를 사용자가 의동한 상태와 일치시키기 위해 능동적으로 관리한다.

## 쿠버네티스 오브젝트 관리

- 커맨드, 명령형, 선언형

## 오브젝트 이름과 ID

- object는 해당 유형에 대하여 고유한 name을 갖고 전체 클러스터에 걸쳐 고유한 UID를 갖는다.
- dns sub domain name: max 253 , lowercase, alphanumeric, -, .
- dns label name: max 63, lowercase, alphanumeric, -
- UID

## 네임스페이스

- default
- kube-system: for Kubernetes system
- kube-public
- kube-node-lease: 클러스터가 스케일링될 때 노드 하트비트의 성능을 향상시키는 각 노드와 관련된 리스(lease) 오브젝트에 대한 네임스페이스

### Namespaces and DNS

- `<service-name>.<namespace-name>.svc.cluster.local`
- nodes, persistentVolumes 같은 저수준 리소스는 namespace에 속하지 않는다.

## 레이블과 셀렉터

- label: 오브젝트 집합 선택 및 구성 하는데 사용
- 식별되지 않는 정보는 annotation으로 기록

### Syntax and character set

- [a-z0-9A-Z], (-), (_), (.)

### Label selectors

- services, replicationcontrollers: Equality-based requirement
- Job, Deployment, ReplicaSet, DaemonSet: Equality-based requirement && Set-based requirement

## 어노테이션

## 필드 셀렉터

## 권장 레이블