# 서비스, 로드밸런싱, 네트워킹

- 파드 내의 컨테이너는 loopback을 통한 네트워킹을 사용하여 통신
- 클러스터 네트워킹은 서로 다른 파드 간의 통신을 제공한다.
- 서비스 리소스를 사용하면 파드에서 실행 중인 애플리케이션을 클러스터 외부에서 접근할 수 있다.
- 서비스를 사용하여 클러스터 내부에서 사용할 수 있는 서비스만 게시 가능

## 서비스

- 쿠버네티스는 파드에게 고유한 IP 주소와 파드 집합에 대한 단일 DNS 명을 부여하고, 그것들 간에 로드-밸런스를 수행할 수 있다.

### 서비스 리소스

- 쿠버네티스에서 서비스는 파드의 논리적 집합과 그것들에 접근할 수 있는 정책을 정의하는 추상적 개념이다.

## 서비스 정의

- 엔드포인트 IP 주소는 다른 쿠버네티스 서비스의 클러스터 IP일 수 없는데, kube-proxy는 가상 IP를 목적지(destination)로 지원하지 않기 때문이다.

## 가상 IP와 서비스 프록시

- 쿠버네티스 클러스터의 모든 노드는 kube-proxy를 실행한다. kube-proxy는 ExternalName 이외의 유형의 서비스에 대한 가상 IP 형식을 구현한다.

### 라운드-로빈 DNS를 사용하지 않는 이유

- 레코드 TTL을 고려하지 않고, 만료된 이름 검색 결과를 캐싱하는 DNS 구현에 대한 오래된 역사가 있다.
- 일부 앱은 DNS 검색을 한 번만 수행하고 결과를 무기한으로 캐시한다.
- 앱과 라이브러리가 적절히 재-확인을 했다고 하더라도, DNS 레코드의 TTL이 낮거나 0이면 DNS에 부하가 높아 관리하기가 어려워 질 수 있다.

### 유저 스페이스(User space) 프록시 모드

### iptables 프록시 모드 

- 트래픽을 처리하기 위해 iptables를 사용하면 시스템 오버헤드가 줄어드는데, 유저스페이스와 커널 스페이스 사이를 전환할 필요없이 리눅스 넷필터(netfilter)가 트래픽을 처리하기 때문이다. 이 접근 방식은 더 신뢰할 수 있기도 하다.

### IPVS 프록시 모드

- IPVS 프록시 모드는 iptables 모드와 유사한 넷필터 후크 기능을 기반으로 하지만, 해시 테이블을 기본 데이터 구조로 사용하고 커널 스페이스에서 동작한다. 이는 IPVS 모드의 kube-proxy는 iptables 모드의 kube-proxy보다 지연 시간이 짧은 트래픽을 리다이렉션하고, 프록시 규칙을 동기화할 때 성능이 훨씬 향상됨을 의미한다. 다른 프록시 모드와 비교했을 때, IPVS 모드는 높은 네트워크 트래픽 처리량도 지원한다.
- rr: 라운드-로빈
- lc: 최소 연결 (가장 적은 수의 열려있는 연결)
- dh: 목적지 해싱
- sh: 소스 해싱
- sed: 최단 예상 지연 (shortest expected delay)
- nq: 큐 미사용 (never queue)

### 서비스 퍼블리싱

- ClusterIP: 서비스를 클러스터-내부 IP에 노출시킨다. 이 값을 선택하면 클러스터 내에서만 서비스에 도달할 수 있다. 이것은 ServiceTypes의 기본 값이다.
- NodePort: 고정 포트 (NodePort)로 각 노드의 IP에 서비스를 노출시킨다. NodePort 서비스가 라우팅되는 ClusterIP 서비스가 자동으로 생성된다. <NodeIP>:<NodePort>를 요청하여, 클러스터 외부에서 NodePort 서비스에 접속할 수 있다.
- LoadBalancer: 클라우드 공급자의 로드 밸런서를 사용하여 서비스를 외부에 노출시킨다. 외부 로드 밸런서가 라우팅되는 NodePort와 ClusterIP 서비스가 자동으로 생성된다.
- ExternalName: 값과 함께 CNAME 레코드를 리턴하여, 서비스를 externalName 필드의 콘텐츠 (예:foo.bar.example.com)에 매핑한다. 어떤 종류의 프록시도 설정되어 있지 않다. ( kube-dns 버전 1.7 또는 CoreDNS 버전 1.7)
- 인그레스를 사용하여 서비스를 노출시킬 수도 있다. 인그레스는 서비스 유형이 아니지만, 클러스터의 진입점 역할을 한다. 동일한 IP 주소로 여러 서비스를 노출시킬 수 있기 때문에 라우팅 규칙을 단일 리소스로 통합할 수 있다.

# 서비스 토폴로지

# 서비스 및 파드용 DNS

