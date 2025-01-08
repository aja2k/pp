# example

## GITOPS 배포환경 설정

### 배포 환경 구성도
![architecture](./GitOps-ssot.png)

## 인프라 배포. (Terraform : dev, stg, prd )

### 클러스터 정보

| 구분 |   인프라   |  K8s 설치  | K8s 리소스 배포 | 클러스터 수 |  환경   |
|:----:|:----------:|:----------:|:--------------:|:----------:|:-------:|
| dev  |  Hyper-V   |  Ansible   |  Helm Charts   |     1      | On-prem |
| stg  | Terraform  | Terraform  |  Helm Charts   |     4      |   NCP   |
| prd  | Terraform  | Terraform  |  Helm Charts   |     4      |   NCP   |

### 인프라 환경 구성도
![infra_nks](./pp-pa-infra-env.png)

### NCP 변수값 설정
    1. variables.tf 파일 수정 (terraform/dev/variables.tf)
    - variable "env" 수정
    - variable "vpc_cidr" 수정

    2. variables.tf 파일 수정 (terraform/stg/variables.tf, terraform/prd/variables.tf)
    - variable "env" 수정
    - variable "vpc_cidr" 수정
    - variable "argocd_password" 수정

## Ansible 배포
![k8s_node](./ansible-k8s-node.png)

### Ansible 타겟서버 및 Roles
```mermaid
graph TD
    A[site.yml] --> B[All Hosts]
    A --> C[K8s Cluster<br/>k8s_ms,k8s_wk]
    A --> D[Master Node<br/>k8s_ms]
    A --> E[Worker Node<br/>k8s_wk]
    A --> F[CICD Server<br/>cicd]

    B --> B1[00.Ansible-Prepare<br/>- SSH 키 설정<br/>- known_hosts 관리]
    B --> B2[01.OS-Prepare<br/>- 호스트명/시간대 설정<br/>- 사용자/그룹 설정<br/>- 시스템 프로파일 설정]

    C --> C1[03.k8s-initial<br/>- K8s 사전 설정<br/>- 시스템 설정 변경<br/>- 필수 패키지 설치]

    D --> D1[04.k8s-ms<br/>- K8s 마스터 초기화<br/>- 인증서/설정 관리<br/>- Helm/Calico 설치]
    D --> D2[06.helm<br/>- Helm 차트 설치<br/>- K8s 애드온 구성]

    E --> E1[05.k8s-wk<br/>- kubelet 상태 확인<br/>- 마스터 노드 조인<br/>- kubeconfig 설정]

    F --> F1[02.cicd<br/>- Docker 설치/설정<br/>- GitLab 관련 설정<br/>- SSL 인증서 구성]

    classDef default fill:#f9f,stroke:#333,stroke-width:2px;
    classDef common fill:#bbf,stroke:#333,stroke-width:2px;
    classDef k8s fill:#ffd,stroke:#333,stroke-width:2px;
    classDef master fill:#bfb,stroke:#333,stroke-width:2px;
    classDef worker fill:#fbb,stroke:#333,stroke-width:2px;
    classDef cicd fill:#dff,stroke:#333,stroke-width:2px;

    class B1,B2 common;
    class C1 k8s;
    class D1,D2 master;
    class E1 worker;
    class F1 cicd;
```