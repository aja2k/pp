# 프로젝트 기술 스택 상세 정리

## 1. 컨테이너 & 쿠버네티스

### Docker
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| 컨테이너 기본 개념 | 컨테이너화의 개념과 이점 이해 | `docker run nginx` |
| Dockerfile 작성 | 멀티스테이지 빌드, 레이어 최적화 | `FROM node:alpine\nWORKDIR /app\nCOPY . .\nRUN npm install` |
| 컨테이너 네트워킹 | 브리지/호스트/오버레이 네트워크 | `docker network create my-net` |
| 이미지 관리 | 빌드, 태깅, 푸시, 레지스트리 관리 | `docker build -t my-app:1.0 .` |

### Kubernetes
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| Pod 관리 | 파드 생명주기, 볼륨 마운트 | `apiVersion: v1\nkind: Pod\nmetadata:\n  name: nginx` |
| Deployment | 롤링 업데이트, 스케일링 | `kubectl scale deployment nginx --replicas=3` |
| Service | ClusterIP, NodePort, LoadBalancer | `kind: Service\ntype: LoadBalancer` |
| 네트워킹 | CNI 플러그인, 서비스 메시 | Calico, Istio 구성 |
| Calico CNI | 네트워크 정책 및 라우팅 | `kind: IPPool\ncidr: 192.168.0.0/16` |
| MetalLB | 베어메탈 로드밸런서 | `addresses: 10.20.100.177-180` |
| Ingress-Nginx | 트래픽 라우팅, SSL 종료 | `kind: Ingress\nspec:\n  rules:` |

## 2. Helm
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| Chart.yaml | 메타데이터, 의존성 정의 | `apiVersion: v2\nname: my-app\nversion: 1.0.0` |
| values.yaml | 환경별 설정값 관리 | `replicaCount: 2\nimage:\n  tag: latest` |
| 템플릿 | Go 템플릿 문법 활용 | `{{ .Values.replicaCount }}` |
| 의존성 관리 | 서브차트, 조건부 설치 | `dependencies:\n  - name: nginx\n    version: "1.0"` |
| Chart 개발 | 애플리케이션 패키징 | `Chart.yaml`, `values.yaml` 구성 |
| 릴리스 관리 | 버전 관리 및 롤백 | `helm upgrade`, `helm rollback` |
| 의존성 관리 | 차트 간 의존성 설정 | `dependencies` in `Chart.yaml` |

## 3. Infrastructure as Code

### Terraform
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| HCL 문법 | 리소스 선언, 변수 관리 | `resource "aws_instance" "web" {\n  ami = "ami-123"\n}` |
| 상태 관리 | 원격 상태 저장, 잠금 관리 | `backend "s3" {\n  bucket = "tf-state"\n}` |
| 모듈화 | 재사용 가능한 모듈 개발 | `module "vpc" {\n  source = "./modules/vpc"\n}` |
| NCP Provider | 네이버 클라우드 리소스 관리 | `provider "ncloud" { support_vpc = true }` |
| Local Provider | 로컬 파일 시스템 관리 | `resource "local_file" "config" { content = "data" }` |
| State 관리 | 인프라 상태 추적 및 관리 | `terraform.tfstate` 파일 관리 |
| 변수 관리 | tfvars를 통한 환경별 설정 | `terraform.tfvars`, `variables.tf` 구성 |

### Ansible
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| Playbook | 태스크 정의, 핸들러 관리 | `- name: Install nginx\n  tasks:\n    - yum: name: nginx` |
| Role | 역할 기반 구성 관리 | `roles:\n  - common\n  - webserver` |

## 4. 네트워킹
| 구분 | 세부 기술 | 설명 | 예시/실습 |
|------|-----------|------|------------|
| CNI | Calico | BGP 라우팅, 네트워크 정책 | `kind: IPPool\ncidr: 192.168.0.0/16` |
| | MetalLB | 베어메탈 로드밸런서 | `addresses:\n  - 192.168.1.240-192.168.1.250` |
| Ingress | NGINX Controller | 트래픽 라우팅, SSL 종료 | `annotations:\n  nginx.ingress.kubernetes.io/ssl-redirect: "true"` |
| | TLS 설정 | 인증서 관리, SSL 구성 | `tls:\n  - secretName: tls-secret` |

## 5. 스토리지 & 모니터링
| 구분 | 세부 기술 | 설명 | 예시/실습 |
|------|-----------|------|------------|
| NFS | StorageClass | 동적 볼륨 프로비저닝 | `provisioner: kubernetes.io/nfs` |
| | 백업 관리 | 볼륨 스냅샷, 복구 | `kubectl create snapshot volume-snap-1` |
| Metrics Server | 쿠버네티스 리소스 메트릭 | `kubectl top nodes/pods` |
| 리소스 모니터링 | CPU, 메모리 사용량 추적 | 메트릭 수집 및 대시보드 |

## 6. CI/CD & 보안
| 구분 | 세부 기술 | 설명 | 예시/실습 |
|------|-----------|------|------------|
| GitLab CE | 소스 코드 관리 | `gitlab-ce:17.4.2` 컨테이너 구성 |
| GitLab Runner | CI/CD 파이프라인 실행 | `.gitlab-ci.yml` 파이프라인 정의 |
| Container Registry | 컨테이너 이미지 저장소 | `docker push registry.gitlab.com/project` |
| ArgoCD | GitOps | 선언적 배포 관리 | `source:\n  repoURL: https://github.com/org/app` |
| | 롤백 | 버전 관리, 히스토리 | `argocd app rollback guestbook` |
| 보안 | RBAC | 역할 기반 접근 제어 | `kind: Role\nrules:\n  - apiGroups: [""]` |
| | Secret | 민감정보 관리 | `kind: Secret\nstringData:\n  key: value` |

## 7. 클라우드 플랫폼

### Naver Cloud Platform
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| VPC/Subnet | 네트워크 구성 | `10.101.0.0/16` CIDR 설계 |
| NKS | 관리형 쿠버네티스 | 클러스터 및 노드풀 구성 |
| ACG | 보안 그룹 관리 | 인바운드/아웃바운드 규칙 설정 |
| Load Balancer | 부하 분산 | 로드밸런서 타입 및 설정 |

## 8. 프로그래밍 언어

### 개발 환경
| 세부 기술 | 설명 | 예시/실습 |
|-----------|------|------------|
| Java | JDK 21 기반 애플리케이션 | Temurin JDK 컨테이너 이미지 |
| Python | 자동화 스크립트 | Python 3 기반 관리 도구 |
| Shell Script | 시스템 관리 스크립트 | 초기화 및 설정 스크립트 |


