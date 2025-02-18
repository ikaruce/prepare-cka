# 마스터 노드
- kube-apiserver
- etcd: 컨테이너가 언제 시작되고
- kube-sheduler
- controller-manager
    - node-controller
    - replication-controller
    - 그외 다양한 컨트롤러가 존재. 

# 워커 노드 
- kubelet
- kube-proxy


# docker vs containerd

container runtime interface(CRI)) -> Open Container initiative(OCI)

-> dockershim: docker를 계속 지원하기 위한 방법 

# ETCD 
## key-value store
클러스터에 대한 정보를 저장
- 노드
- 팟
- 컨피그
- 시크릿
- 어카운트
- 롤
- 바인딩
- 기타 등등

설치 
- 수동으로 다운로드하여 설치
- 클러스터로 구성.

# kube-api server 
- 사용자 인증
- 요청 검증
- 데이터 조회
- ETCD 정보 갱신

- 스케쥴러, 큐블렛과 통신

# kube controller manager
다양한 컨트롤러 관리. 
상태 관찰 
상황에 따른 조치. 
- node controller
    - 노드의 상태확인, 팟을 적절한 노드에 배포되도록 함.
 
- replication-controller
- 그외 다양한 컨트롤러가 존재.
- 이러한 컨트롤러를 관리.

- 설치

# kube scheduler 
노드의 스케쥴링을 관리. 어떤 노드에 팟이 위치할지를 지정. 실제 배치는 kubelet이 담당. 
다음 순서로 노드를 지정. 
1. 필터 노드: 적합하지 않은 노드를 제거
2. 랭크 노드: 최적의 노드를 도출
- 스케쥴러는 위와 같은 순서로 동작하지만, 특정 룰을 지정하거나 개별적으로 스케쥴러를 적용할 수도 있음.

# kubelet
배의 선장같은 역할 
마스터 노드와 통신 역할 (일정간격으로 노드의 상태를 전달) 
컨테이너 런타임을 이용하여, 마스터 노드의 명령을 수행. 

# kube-proxy
pod는 서로 통신할 수 있음. 
각 노드에서 실행되어 서비스로 트래픽을 전달하는 역할 iptables 규칙을 이용하여 서비스의 IP와 트래픽을 관리. 

# Pods 
앱(컨테이너의 묶음)의 싱글 인스턴스 
같은 pod 내의 컨테이너는 서로 통신이 가능함/스토리지 공유. 
보통은 pod에 1개의 컨테이너를 사용함. 

```bash
# pod 배포. 
$ kubectl run nginx --image nginx
$ kubectl get pods
``` 

## pod 생성 from yaml 
k8s에서는 pod, service, replica, deployment를 yaml으로 관리. 

4개의 root요소가 있음(yaml 에 반드시 존재해야 함, 대소문자를 구분함.) 
- apiVersion: pod를 생성할때 사용하는 k8s api 버전 
- kind: 만들려는 개체 유형. 여기서는 pod를 만들기 위함이므로 pod가 됨.
        (Pod, Service, Replicaset, Deployment)
- metadata: 개체 이름, label등을 지정. (key, value가 존재)
    - name: myapp-pod
    - labels:
        - app: my-app
        - type: front-end # label은 원하는 key-value가 될 수 있음 
- spec: 사양을 지정. 생성하려는 개체에 따라 형식이 다름. pod를 만들때는 컨테이너 속성필요
    - containers:
        - name: nginx-container
        - image: nginx

```bash
# 생성된 pod 정보 확인하기 
$ kubectl get pods 
$ kubectl describe pods myapp-pod
```

## yaml 파일 만들기
- 텍스트에디터를 사용

```bash
# pod 생성은 create, apply 모두 가능. (변경된 사항을 반영할 경우에는 apply 사용)
$ kubectl create -f pod.yaml 
$ kubectl apply -f pod.yaml

# pod 정보 확인  
$ kubectl get pods 
```
