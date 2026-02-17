# kind(리소스 타입)

1. Workload: Pod, Deployment, StatefulSet, DaemonSet, Job, CronJob
2. Service/Ingress: Service, Ingress
3. Config/Secret: ConfigMap, Secret
4. Storage: PersistentVolume, PersistentVolumeClaim, StorageClass
5. Access: ServiceAccount, Role, RoleBinding, ClusterRole, ClusterRoleBinding
6. Policy: NetworkPolicy, PodDisruptionBudget


# Kubernetes 주요 개념 정리

## 1. Workload (워크로드)
실제로 애플리케이션 컨테이너를 실행하는 리소스
* Pod → 가장 기본 단위
* Deployment → stateless 앱
* StatefulSet → 상태가 있는 앱 (DB 등)
* DaemonSet → 모든 노드에 1개씩 실행
* Job → 일회성 작업
* CronJob → 정기적 작업


### Pod
- K8s 가장 기본 단위
- 1개 이상의 컨테이너를 포함 (보통 1개)
- IP 주소를 가지지만 자주 변함
- Pod이 삭제되면 IP도 사라짐
- 직접 Pod을 만드는 것보다 Deployment/StatefulSet 같은 상위 리소스 사용

### Deployment
- Stateless(상태 없는) 애플리케이션용
- 가장 일반적으로 쓰는 워크로드
- 예: 당신의 ftmiddle-shopping 서비스
- 특징:
  - Pod 개수 자동 관리 (replicas)
  - 배포 시 Rolling Update 지원
  - 자동 재시작 (Pod 죽으면 다시 생성)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ftmiddle-shopping
spec:
  replicas: 5
  selector:
    matchLabels:
      app: ftmiddle-shopping
  template:
    metadata:
      labels:
        app: ftmiddle-shopping
    spec:
      containers:
      - name: shopping
        image: ftmiddle-shopping:v1
        port: 8080
```

### StatefulSet
- Stateful(상태 있는) 애플리케이션용
- 각 Pod이 고유한 신원 유지
- 예: MySQL, MongoDB, Redis 등 데이터베이스
- 특징:
  - Pod 이름이 순서대로 고정 (pod-0, pod-1, pod-2...)
  - 저장소(PersistentVolume) 연결 가능
  - 시작/종료 순서 보장

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        volumeMounts:
        - name: data
          mountPath: /var/lib/mysql
```

### DaemonSet
- 모든 노드에 1개씩 실행
- 노드가 새로 추가되면 자동으로 그 노드에도 실행
- 예: 모니터링 에이전트, 로그 수집기, 네트워크 플러그인
- 특징:
  - 각 노드마다 1개씩만 존재
  - 노드와 함께 생성/삭제

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
spec:
  selector:
    matchLabels:
      app: node-exporter
  template:
    metadata:
      labels:
        app: node-exporter
    spec:
      containers:
      - name: exporter
        image: prom/node-exporter:latest
```

### Job / CronJob
- 일회성 작업용
- Job: 한 번 실행 후 종료
- CronJob: 정기적으로 실행
- 예: 배치 작업, 정기 백업, 데이터 정제
- 특징:
  - 완료까지 기다림
  - 실패하면 재시도

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: backup-job
spec:
  template:
    spec:
      containers:
      - name: backup
        image: backup-tool:latest
      restartPolicy: Never
---
apiVersion: batch/v1
kind: CronJob
metadata:
  name: daily-backup
spec:
  schedule: "0 2 * * *"  # 매일 2시
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: backup-tool:latest
          restartPolicy: Never
```


## 2. Service/Ingress (트래픽 연결)
Pod에 접근하는 방법
* Service → Pod에 고정 주소 제공 (ClusterIP, NodePort, LoadBalancer)
* Ingress → 도메인/경로 기반 라우팅 (HTTP/HTTPS)

### Service
- Pod에 고정된 접근 주소 제공
- Pod은 IP가 자주 바뀌므로, Service가 안정된 엔드포인트 역할
- Pod이 여러 개일 때 자동으로 로드 밸런싱

#### Service 타입

**ClusterIP (기본값)**
- 클러스터 내부에서만 접근 가능
- Pod들 간 통신용
- 예: ftmiddle-shopping이 내부 API 호출

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ftmiddle-shopping
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: ftmiddle-shopping
```

**NodePort**
- 각 노드의 특정 포트로 외부 접근 가능
- 예: http://server1:30000, http://server2:30000
- 테스트/개발용으로 주로 사용

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ftmiddle-shopping
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30000  # 30000~32767 범위만 사용 가능
  selector:
    app: ftmiddle-shopping
```

**LoadBalancer**
- 클라우드 환경에서 외부 LB 자동 생성
- 예: AWS ALB/NLB, Azure Load Balancer
- 물리서버 환경에서는 보통 사용 안 함 (클라우드용)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ftmiddle-shopping
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: ftmiddle-shopping
```

### Service 흐름

```
사용자 요청
    ↓
클라이언트: http://ftmiddle-shopping:80
    ↓
Service (안정된 IP)
    ↓
[Pod 1] [Pod 2] [Pod 3] [Pod 4] [Pod 5]
    ↓
자동 로드 밸런싱 (라운드 로빈)
```

### Ingress
- HTTP/HTTPS 트래픽을 도메인/경로 기반으로 라우팅
- L7(애플리케이션 레이어) 리소스
- 웹 서비스용 (MySQL 같은 TCP는 대상 아님)
- 하나의 IP/도메인으로 여러 서비스 라우팅 가능

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: shopping-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: shopping.search.kakao.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: ftmiddle-shopping
            port:
              number: 80
  - host: product.search.kakao.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: product-service
            port:
              number: 80
```

### Service vs Ingress

| 항목 | Service | Ingress |
| --- | --- | --- |
| **계층** | L4 (전송 계층) | L7 (애플리케이션 계층) |
| **지원 프로토콜** | TCP/UDP 모두 | HTTP/HTTPS만 |
| **라우팅** | IP/Port 기반 | 도메인/경로 기반 |
| **예** | ftmiddle-shopping:80 | shopping.search.kakao.com |
| **용도** | 내부 통신, 외부 노출 | 웹 서비스 노출 |


## 3. Config/Secret (설정 관리)
* ConfigMap → 민감하지 않은 설정값 (환경변수, 설정파일)
* Secret → 민감한 값 (비밀번호, API 키)

### ConfigMap
- 민감하지 않은 설정값 저장
- 평문(plain text)으로 저장
- 코드와 분리해서 관리 가능
- 예: 환경변수, 설정파일, 로그 레벨

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ftmiddle-config
data:
  PROFILE: "prod"
  LOG_LEVEL: "INFO"
  API_TIMEOUT: "1000"
  application.yml: |
    server:
      port: 8080
    logging:
      level: INFO
```

### ConfigMap 사용 (Pod에서)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ftmiddle-shopping
spec:
  template:
    spec:
      containers:
      - name: shopping
        image: ftmiddle-shopping:v1
        env:
        - name: PROFILE
          valueFrom:
            configMapKeyRef:
              name: ftmiddle-config
              key: PROFILE
        - name: LOG_LEVEL
          valueFrom:
            configMapKeyRef:
              name: ftmiddle-config
              key: LOG_LEVEL
        volumeMounts:
        - name: config-volume
          mountPath: /etc/config
      volumes:
      - name: config-volume
        configMap:
          name: ftmiddle-config
```

### Secret
- 민감한 값 저장 (비밀번호, API 키, 인증서 등)
- Base64 인코딩 (암호화 아님!)
- 권한 관리가 핵심
- 예: DB 비밀번호, JWT 시크릿 키, API 토큰

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  username: YWRtaW4=  # base64(admin)
  password: cGFzc3dvcmQxMjM=  # base64(password123)
---
apiVersion: v1
kind: Secret
metadata:
  name: api-keys
type: Opaque
data:
  kakao-api-key: "..."
  search-api-key: "..."
```

### Secret 주의사항

```
Secret은 base64 인코딩일 뿐 암호화 아님!
echo "password123" | base64
→ cGFzc3dvcmQxMjM=

echo "cGFzc3dvcmQxMjM=" | base64 -d
→ password123

즉, kubectl get secret -o yaml로 조회하면 값이 보임!
```

### Secret 보안 강화 방법

1. RBAC (Role-Based Access Control)
   - Secret 조회 권한 제한
   
2. 별도 Secret 관리 도구 사용
   - HashiCorp Vault
   - AWS Secrets Manager
   - Google Cloud Secret Manager

3. Secret 암호화
   - etcd 암호화 활성화

### ConfigMap vs Secret 비교

| 항목 | ConfigMap | Secret |
| --- | --- | --- |
| **민감도** | 낮음 (일반 설정) | 높음 (비밀번호/키) |
| **인코딩** | Plain text | Base64 인코딩 |
| **용도** | 환경변수, 설정파일 | 인증정보, API 키 |
| **보안** | 일반 권한으로 조회 가능 | 권한 제한 필수 |
| **예** | PROFILE=prod | DB_PASSWORD=... |

---

## 실제 배포 예시 (모든 요소 통합)

```yaml
# 1. ConfigMap (설정)
apiVersion: v1
kind: ConfigMap
metadata:
  name: ftmiddle-config
data:
  PROFILE: "prod"
  LOG_LEVEL: "INFO"

---
# 2. Secret (민감 정보)
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  password: cGFzc3dvcmQxMjM=

---
# 3. Deployment (워크로드)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ftmiddle-shopping
spec:
  replicas: 5
  selector:
    matchLabels:
      app: ftmiddle-shopping
  template:
    metadata:
      labels:
        app: ftmiddle-shopping
    spec:
      containers:
      - name: shopping
        image: ftmiddle-shopping:v2
        ports:
        - containerPort: 8080
        env:
        - name: PROFILE
          valueFrom:
            configMapKeyRef:
              name: ftmiddle-config
              key: PROFILE
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password

---
# 4. Service (트래픽 연결)
apiVersion: v1
kind: Service
metadata:
  name: ftmiddle-shopping
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: ftmiddle-shopping

---
# 5. Ingress (외부 접근)
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: shopping-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: shopping.search.kakao.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: ftmiddle-shopping
            port:
              number: 80
```

### 배포 흐름

```
kubectl apply -f deployment.yaml
    ↓
K8s가 모든 리소스 생성:
1. ConfigMap (PROFILE=prod)
2. Secret (DB 비밀번호)
3. Deployment (5개 Pod 생성)
4. Service (안정된 IP 제공)
5. Ingress (도메인 라우팅)
    ↓
사용자 접속:
shopping.search.kakao.com
    ↓
Ingress (도메인 해석)
    ↓
Service (로드 밸런싱)
    ↓
Pod 1~5 (요청 처리)
    ↓
ConfigMap/Secret (설정/비밀 로드)
    ↓
응답
```

## 4. Storage (저장소)
* PersistentVolume → 저장소 리소스
* PersistentVolumeClaim → 저장소 요청
* StorageClass → 저장소 타입 정의

## 5. Access (권한 관리)
* ServiceAccount → K8s 계정
* Role → 권한 정의 (네임스페이스)
* RoleBinding → 권한 할당 (네임스페이스)
* ClusterRole → 권한 정의 (전체 클러스터)
* ClusterRoleBinding → 권한 할당 (전체 클러스터)

## 6. Policy (정책)
* NetworkPolicy → 네트워크 접근 제한
* PodDisruptionBudget → Pod 중단 정책


