# Terraform 101 매뉴얼: 네이버 클라우드 인프라 구성

## 제공자(Provider)
제공자는 특정 인프라 플랫폼(AWS, Azure, GCP 등)이나 서비스와 상호작용하기 위한 플러그인입니다.

## 테라폼 주요 기능

### API 추상화
- 각 서비스의 API를 Terraform 형식으로 변환
- 인증 및 권한 관리 처리

### 리소스 관리
- CRUD(Create, Read, Update, Delete) 작업 수행
- 상태 관리 및 동기화

## Terraform WorkFlow

### 1. init
- 작업 디렉토리의 모든 .tf 파일을 스캔
- 필요한 프로바이더와 모듈을 다운로드
- 파일 이름과 관계없이 모든 구성을 하나의 그래프로 병합

### 2. plan
- 모든 .tf 파일의 리소스를 분석
- 리소스 간 의존성 그래프 생성
- 생성/수정/삭제될 리소스 계획 수립

### 3. apply
- 의존성 그래프에 따라 순차적으로 리소스 생성

## 테라폼의 특징

### 1. 암묵적 의존성
Terraform은 리소스 블록 내에서 다른 리소스를 참조하는 경우, 해당 리소스가 먼저 생성되어야 한다는 것을 이해합니다. 예를 들어, ncloud_subnet 리소스에서 vpc_no 속성에 ncloud_vpc.my_vpc.id를 사용하면, Terraform은 my_vpc가 먼저 생성되어야 한다고 판단합니다.

### 2. 의존성 그래프
Terraform은 모든 리소스와 그 의존성을 기반으로 의존성 그래프를 생성합니다. 이 그래프를 통해 Terraform은 리소스를 생성할 최적의 순서를 결정합니다.

### 3. 명시적 의존성
때때로, 명시적으로 의존성을 설정해야 할 경우가 있습니다. 이럴 때는 depends_on 속성을 사용하여 의존성을 명시적으로 지정할 수 있습니다.

## 파일 구조
| 파일명 | 설명 |
|--------|------|
| provider.tf | 프로바이더 구성 |
| variables.tf | 변수 정의 |
| outputs.tf | 출력 정의 |
| locals.tf | 로컬 변수 |
| vpc.tf | VPC 관련 리소스(vpc,subnet,lb,nat) |
| nks.tf | NKS 클러스터 관련 리소스 |
| nodepool.tf | 노드풀 관련 리소스 |
| security.tf | 보안 그룹 관련 리소스 |

# 1. 전체 작업 목적
 - 네이버 클라우드 플랫폼에 인프라 자동 구성
 - VPC, 서브넷, 보안그룹 등 네트워크 리소스 생성
 - 서버 인스턴스 및 스토리지 프로비저닝
 - 쿠버네티스 클러스터(NKS) 구성

# 2. 실행 전 확인사항
 - Terraform 설치 (v1.9.8 이상)
 - 네이버 클라우드 플랫폼 계정 및 인증 정보
 - API 인증키 (access_key, secret_key)
 - 리소스 할당량 확인

# 3. 실행순서
 - 초기화: terraform init
 - 계획 확인: terraform plan
 - 적용: terraform apply
 - 삭제 (필요시): terraform destroy

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_ncloud"></a> [ncloud](#requirement\_ncloud) | 3.2.1 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_local"></a> [local](#provider\_local) | 2.5.2 |
| <a name="provider_ncloud"></a> [ncloud](#provider\_ncloud) | 3.2.1 |
| <a name="provider_null"></a> [null](#provider\_null) | 3.2.3 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [local_file.bastion_svr_root_pw](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file) | resource |
| [local_file.cluster_info](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file) | resource |
| [local_file.node_info](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file) | resource |
| [local_sensitive_file.ssh_key](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/sensitive_file) | resource |
| [ncloud_access_control_group.acg](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/access_control_group) | resource |
| [ncloud_access_control_group_rule.acg_rules](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/access_control_group_rule) | resource |
| [ncloud_login_key.key](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/login_key) | resource |
| [ncloud_nks_cluster.clusters](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/nks_cluster) | resource |
| [ncloud_nks_node_pool.node_pool](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/nks_node_pool) | resource |
| [ncloud_public_ip.bastion_ip](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/public_ip) | resource |
| [ncloud_route_table.private](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/route_table) | resource |
| [ncloud_route_table.public](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/route_table) | resource |
| [ncloud_route_table_association.subnet_associations](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/route_table_association) | resource |
| [ncloud_server.bastion_server](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/server) | resource |
| [ncloud_subnet.subnets](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/subnet) | resource |
| [ncloud_vpc.vpc](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/resources/vpc) | resource |
| [null_resource.apply_acg](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [null_resource.apply_init_script](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [null_resource.argocd_login](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [ncloud_nks_kube_config.kube_config](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/nks_kube_config) | data source |
| [ncloud_nks_server_images.image](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/nks_server_images) | data source |
| [ncloud_nks_server_products.product](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/nks_server_products) | data source |
| [ncloud_nks_versions.version](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/nks_versions) | data source |
| [ncloud_root_password.bastion](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/root_password) | data source |
| [ncloud_server_image_numbers.kvm-image](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/server_image_numbers) | data source |
| [ncloud_server_specs.kvm-spec](https://registry.terraform.io/providers/NaverCloudPlatform/ncloud/3.2.1/docs/data-sources/server_specs) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_access_key"></a> [access\_key](#input\_access\_key) | Naver Cloud Platform access key | `string` | n/a | yes |
| <a name="input_clusters"></a> [clusters](#input\_clusters) | cluster configuration | <pre>map(object({<br/>    name        = string<br/>    environment = string<br/>    zone        = string<br/>    nodepool = object({<br/>      node_count = number<br/>      storage_size = number<br/>      autoscale = object({<br/>        min = number<br/>        max = number<br/>      })<br/>      labels = map(string)<br/>    })<br/>  }))</pre> | <pre>{<br/>  "bof-1": {<br/>    "environment": "stg",<br/>    "name": "bof",<br/>    "nodepool": {<br/>      "autoscale": {<br/>        "max": 4,<br/>        "min": 2<br/>      },<br/>      "labels": {<br/>        "environment": "stg-bof-1",<br/>        "role": "worker"<br/>      },<br/>      "node_count": 2,<br/>      "storage_size": 600<br/>    },<br/>    "zone": "KR-1"<br/>  },<br/>  "bof-2": {<br/>    "environment": "stg",<br/>    "name": "bof",<br/>    "nodepool": {<br/>      "autoscale": {<br/>        "max": 1,<br/>        "min": 1<br/>      },<br/>      "labels": {<br/>        "environment": "stg-bof-2",<br/>        "role": "worker"<br/>      },<br/>      "node_count": 1,<br/>      "storage_size": 600<br/>    },<br/>    "zone": "KR-2"<br/>  },<br/>  "cmn-1": {<br/>    "environment": "stg",<br/>    "name": "cmn",<br/>    "nodepool": {<br/>      "autoscale": {<br/>        "max": 4,<br/>        "min": 2<br/>      },<br/>      "labels": {<br/>        "environment": "stg-cmn-1",<br/>        "role": "worker"<br/>      },<br/>      "node_count": 2,<br/>      "storage_size": 600<br/>    },<br/>    "zone": "KR-1"<br/>  },<br/>  "cmn-2": {<br/>    "environment": "stg",<br/>    "name": "cmn",<br/>    "nodepool": {<br/>      "autoscale": {<br/>        "max": 1,<br/>        "min": 1<br/>      },<br/>      "labels": {<br/>        "environment": "stg-cmn-2",<br/>        "role": "worker"<br/>      },<br/>      "node_count": 1,<br/>      "storage_size": 600<br/>    },<br/>    "zone": "KR-2"<br/>  }<br/>}</pre> | no |
| <a name="input_env"></a> [env](#input\_env) | environment | `string` | `"poc-stg"` | no |
| <a name="input_network"></a> [network](#input\_network) | 네트워크 설정 | <pre>object({<br/>    vpc_cidr = string<br/>    subnets = map(object({<br/>      name = string<br/>      cidr = string<br/>      type = string<br/>      usage = string<br/>    }))<br/>  })</pre> | <pre>{<br/>  "subnets": {<br/>    "bof-lb-pri-1": {<br/>      "cidr": "10.102.3.0/24",<br/>      "name": "bof-lb-pri-sbn-1",<br/>      "type": "PRIVATE",<br/>      "usage": "LOADB"<br/>    },<br/>    "bof-lb-pri-2": {<br/>      "cidr": "10.102.103.0/24",<br/>      "name": "bof-lb-pri-sbn-2",<br/>      "type": "PRIVATE",<br/>      "usage": "LOADB"<br/>    },<br/>    "bof-lb-pub-1": {<br/>      "cidr": "10.102.1.0/24",<br/>      "name": "bof-lb-pub-sbn-1",<br/>      "type": "PUBLIC",<br/>      "usage": "LOADB"<br/>    },<br/>    "bof-lb-pub-2": {<br/>      "cidr": "10.102.101.0/24",<br/>      "name": "bof-lb-pub-sbn-2",<br/>      "type": "PUBLIC",<br/>      "usage": "LOADB"<br/>    },<br/>    "bof-np-pri-1": {<br/>      "cidr": "10.102.5.0/24",<br/>      "name": "bof-np-pri-sbn-1",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "bof-np-pri-2": {<br/>      "cidr": "10.102.105.0/24",<br/>      "name": "bof-np-pri-sbn-2",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "bof-vm-pri-1": {<br/>      "cidr": "10.102.4.0/24",<br/>      "name": "bof-vm-pri-sbn-1",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "bof-vm-pri-2": {<br/>      "cidr": "10.102.104.0/24",<br/>      "name": "bof-vm-pri-sbn-2",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "bof-vm-pub-1": {<br/>      "cidr": "10.102.2.0/24",<br/>      "name": "bof-vm-pub-sbn-1",<br/>      "type": "PUBLIC",<br/>      "usage": "GEN"<br/>    },<br/>    "bof-vm-pub-2": {<br/>      "cidr": "10.102.102.0/24",<br/>      "name": "bof-vm-pub-sbn-2",<br/>      "type": "PUBLIC",<br/>      "usage": "GEN"<br/>    },<br/>    "cmn-lb-pri-1": {<br/>      "cidr": "10.102.8.0/24",<br/>      "name": "cmn-lb-pri-sbn-1",<br/>      "type": "PRIVATE",<br/>      "usage": "LOADB"<br/>    },<br/>    "cmn-lb-pri-2": {<br/>      "cidr": "10.102.108.0/24",<br/>      "name": "cmn-lb-pri-sbn-2",<br/>      "type": "PRIVATE",<br/>      "usage": "LOADB"<br/>    },<br/>    "cmn-lb-pub-1": {<br/>      "cidr": "10.102.6.0/24",<br/>      "name": "cmn-lb-pub-sbn-1",<br/>      "type": "PUBLIC",<br/>      "usage": "LOADB"<br/>    },<br/>    "cmn-lb-pub-2": {<br/>      "cidr": "10.102.106.0/24",<br/>      "name": "cmn-lb-pub-sbn-2",<br/>      "type": "PUBLIC",<br/>      "usage": "LOADB"<br/>    },<br/>    "cmn-np-pri-1": {<br/>      "cidr": "10.102.10.0/24",<br/>      "name": "cmn-np-pri-sbn-1",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "cmn-np-pri-2": {<br/>      "cidr": "10.102.110.0/24",<br/>      "name": "cmn-np-pri-sbn-2",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "cmn-vm-pri-1": {<br/>      "cidr": "10.102.9.0/24",<br/>      "name": "cmn-vm-pri-sbn-1",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "cmn-vm-pri-2": {<br/>      "cidr": "10.102.109.0/24",<br/>      "name": "cmn-vm-pri-sbn-2",<br/>      "type": "PRIVATE",<br/>      "usage": "GEN"<br/>    },<br/>    "cmn-vm-pub-1": {<br/>      "cidr": "10.102.7.0/24",<br/>      "name": "cmn-vm-pub-sbn-1",<br/>      "type": "PUBLIC",<br/>      "usage": "GEN"<br/>    },<br/>    "cmn-vm-pub-2": {<br/>      "cidr": "10.102.107.0/24",<br/>      "name": "cmn-vm-pub-sbn-2",<br/>      "type": "PUBLIC",<br/>      "usage": "GEN"<br/>    }<br/>  },<br/>  "vpc_cidr": "10.102.0.0/16"<br/>}</pre> | no |
| <a name="input_region"></a> [region](#input\_region) | 네이버 클라우드 리전 | `string` | `"KR"` | no |
| <a name="input_secret_key"></a> [secret\_key](#input\_secret\_key) | Naver Cloud Platform secret key | `string` | n/a | yes |

## Outputs

No outputs.
