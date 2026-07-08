<a id="network-vpc-api-guide"></a>
## Network > VPC > API 가이드 { #network-vpc-api-guide }

API는 현재 한국 리전에서만 사용할 수 있습니다.

<a id="prerequisites"></a>
## 사전 준비 { #prerequisites }

네트워크 VPC API를 사용하려면 앱키와 토큰이 필요합니다. [API Endpoint URL](/Compute/Instance/ko/api-guide/#api-endpoint-url)과 [토큰 API](/Compute/Instance/ko/api-guide/#api)를 이용하여 앱키와 토큰을 준비합니다. 앱키는 API Endpoint URL에 토큰은 Request Body에 포함하여 사용합니다.

예를 들어, 보안 그룹 목록 조회는 다음 URL로 요청해야 합니다.

	GET https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}


<a id="security-group-api"></a>
## 보안 그룹 API { #security-group-api }
보안 그룹 생성, 삭제, 조회 및 업데이트 기능을 제공합니다. 보안 그룹을 인스턴스에 등록/해제하는 기능은 [인스턴스 API](/Compute/Instance/ko/api-guide/)를 통해 제공됩니다.

<a id="list-security-groups"></a>
### 보안 그룹 목록 조회 { #list-security-groups }
접근 가능한 보안 그룹의 정보를 조회합니다.

<a id="list-security-groups-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupId | Query | String | O | 조회할 보안 그룹 ID. 기재하지 않을 경우 모든 보안 그룹의 정보를 조회합니다. |

<a id="list-security-groups-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="list-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroups": [
        {
            "description": "{Desctiption}",
            "id": "{Security Group ID}",
            "name": "{Name}",
            "securityGroupRules": [
                {
                    "direction": "egress",
                    "ethertype": "IPv4",
                    "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff",
                    "portRangeMax": null,
                    "portRangeMin": null,
                    "protocol": null,
                    "remoteGroupId": null,
                    "remoteIpPrefix": null,
                    "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5",
                    "description": ""
                }
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | 보안 그룹 설명 |
| Security Group ID | Body | String | 보안 그룹 ID |
| Name | Body | String | 보안 그룹 이름 |
| securityGroupRules | Body | List | 보안 그룹 규칙 목록, [보안 그룹 규칙 API](#api_1) 참조 |

<a id="create-security-groups"></a>
### 보안 그룹 생성 { #create-security-groups }
새로운 보안 그룹을 생성합니다.

<a id="create-security-groups-method-url"></a>
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-groups
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |

<a id="create-security-groups-request-body"></a>
#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| name | Body | String | - |보안 그룹 이름 |
| description | Body | String | O | 보안 그룹 설명 |

<a id="create-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroup": {
        "description": "{Description}",
        "id": "{Security Group ID}",
        "name": "{Name}",
        "securityGroupRules": [
            {
                "direction": "egress",
                "ethertype": "IPv4",
                "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff",
                "portRangeMax": null,
                "portRangeMin": null,
                "protocol": null,
                "remoteGroupId": null,
                "remoteIpPrefix": null,
                "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5",
                "description": ""
            }
        ]
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | 보안 그룹 설명 |
| Security Group ID | Body | String | 보안 그룹 ID |
| Name | Body | String | 보안 그룹 이름 |
| securityGroupRules | Body | List | 보안 그룹 규칙 목록, [보안 그룹 규칙 API](#api_1) 참조 |

<a id="modify-security-groups"></a>
### 보안 그룹 수정 { #modify-security-groups }
보안 그룹의 이름, 설명을 변경합니다.

<a id="modify-security-groups-method-url"></a>
#### Method, URL
```
PUT /v1.0/appkeys/{appkey}/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupId | Path | String | - | 변경할 보안 그룹의 ID |

<a id="modify-security-groups-request-body"></a>
#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Name | Body | String | - | 보안 그룹 이름 |
| Description | Body | String | O | 보안 그룹 설명 |

<a id="modify-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroup": {
        "id": "{Security Group ID}",
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Security Group ID | Body | String | 보안 그룹 ID |
| Name | Body | String | 보안 그룹 이름 |
| Description | Body | String | 보안 그룹 설명 |

<a id="delete-security-groups"></a>
### 보안 그룹 삭제 { #delete-security-groups }
지정한 보안 그룹을 삭제합니다.

<a id="delete-security-groups-method-url"></a>
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupId | Query | String | - | 삭제할 보안 그룹 ID |

<a id="delete-security-groups-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="delete-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    }
}
```


<a id="security-group-rules-api"></a>
## 보안 그룹 규칙 API { #security-group-rules-api }
보안 그룹 규칙 추가/삭제 및 조회 기능을 제공합니다.

<a id="list-security-group-rules"></a>
### 보안 그룹 규칙 조회 { #list-security-group-rules }
접근 가능한 모든 보안 그룹 규칙의 정보를 조회합니다.
<a id="list-security-group-rules-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupRuleId | Query | String | O | 조회할 보안 그룹 규칙 ID. 기재하지 않을 경우 모든 보안 그룹 규칙의 정보를 조회합니다. |

<a id="list-security-group-rules-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="list-security-group-rules-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroupRules": [
        {
            "direction": "{Direction}",
            "ethertype": "{Ethernet Type}",
            "id": "{Rule ID}",
            "portRangeMax": "{Port Range MAX}",
            "portRangeMin": "{Port Range MIN}",
            "protocol": "{Protocol}",
            "remoteGroupId": "{Remote Group ID}",
            "remoteIpPrefix": "{Remote IP Prefix}",
            "securityGroupId": "{Security Group ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | 규칙이 적용되는 방향, "ingress" 또는 "egress" |
| Ethernet Type | Body | String | "IPv4" 또는 "IPv6" |
| Rule ID | Body | String | 보안 그룹 규칙 ID |
| Port Range MAX | Body | Integer | 규칙이 적용되는 최대 포트 번호 |
| Port Range MIN | Body | Integer | 규칙이 적용되는 최소 포트 번호 |
| Protocol | Body | String | IP 프로토콜 "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | 규칙이 적용되는 Remote Group의 ID |
| Remote IP Prefix | Body | String | 규칙이 적용되는 Remote IP의 Prefix |
| Security Group ID | Body | String | 규칙이 적용되는 보안 그룹의 ID |

<a id="create-security-group-rules"></a>
### 보안 그룹 규칙 생성 { #create-security-group-rules }
새로운 보안 그룹 규칙을 생성합니다.
<a id="create-security-group-rules-method-url"></a>
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-group-rules
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |

<a id="create-security-group-rules-request-body"></a>
#### Request Body
```json
{
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "portRangeMax": "{Port Range MAX}",
        "portRangeMin": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| Direction | Body | String | - | 규칙이 적용되는 방향, "ingress" 또는 "egress" |
| Ethernet Type | Body | String | - | "IPv4" 또는 "IPv6" |
| Port Range MAX | Body | Integer | O | 규칙이 적용되는 최대 포트 번호. 1~65535 범위. 설정 시 "protocol" 항목 생략 불가 |
| Port Range MIN | Body | Integer | O | 규칙이 적용되는 최소 포트 번호. 1~65535 범위. 설정 시 "protocol" 항목 생략 불가 |
| Protocol | Body | String | O | IP 프로토콜. "icmp", "tcp", "udp", null. |
| Remote Group ID | Body | String | O | 규칙이 적용되는 Remote 보안 그룹의 ID. <br />"remoteIpPrefix" 값을 설정할 경우 생략<Paste> |
| Remote IP Prefix | Body | String | O | 규칙이 적용되는 Remote IP의 Prefix. <br />"remoteGroupId" 값을 설정할 경우 생략. |
| Security Group ID | Body | String | - | 규칙이 적용되는 보안 그룹의 ID |

> [Notice]
> `Remote Group ID`와 `Remote IP Prefix` 둘 다 생략하면, 기본 값인 0.0.0.0/0으로 지정됩니다.
> 즉, 모든 트래픽에 적용되는 규칙이 생성됩니다.

##### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "id": "{Rule ID}",
        "portRangeMax": "{Port Range MAX}",
        "portRangeMin": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | 규칙이 적용되는 방향, "ingress" 또는 "egress" |
| Ethernet Type | Body | String | "IPv4" 또는 "IPv6" |
| Rule ID | Body | String | 보안 그룹 규칙 ID |
| Port Range MAX | Body | Integer | 규칙이 적용되는 최대 포트 번호 |
| Port Range MIN | Body | Integer | 규칙이 적용되는 최소 포트 번호 |
| Protocol | Body | String | IP 프로토콜. "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | 규칙이 적용되는 Remote 보안 그룹의 ID |
| Remote IP Prefix | Body | String | 규칙이 적용되는 Remote IP의 Prefix |
| Security Group ID | Body | String | 규칙이 적용되는 보안 그룹의 ID |

<a id="delete-security-group-rules"></a>
### 보안 그룹 규칙 삭제 { #delete-security-group-rules }
지정한 보안 그룹 규칙을 삭제합니다.
<a id="delete-security-group-rules-method-url"></a>
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuelsId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupRuleId | Query | String | - | 삭제할 보안 그룹 규칙 ID |

<a id="delete-security-group-rules-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="delete-security-group-rules-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    }
}
```

<a id="network-api"></a>
## 네트워크 API { #network-api }
인스턴스에서 연결할 수 있는 네트워크 정보 조회 기능을 제공합니다.

<a id="network-status"></a>
### 네트워크 상태 { #network-status }
네트워크는 다음 상태 값을 같습니다.

| Status | Description |
| -- | -- |
| BUILD | 네트워크 구축 중 |
| ACTIVE | 네트워크 활성화 상태 |
| DOWN | 네트워크 비활성화 상태 |
| ERROR | 에러 발생 |

<a id="get-network-information"></a>
### 네트워크 정보 조회 { #get-network-information }
접근 가능한 네트워크의 정보를 조회합니다.

<a id="get-network-information-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/networks?id={networkId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | 토큰 ID |
| networkId | Query | String | O | 조회할 네트워크 ID. 기재하지 않을 경우 모든 네트워크의 정보를 조회합니다. |

<a id="get-network-information-request-body"></a>
#### Request Body
이 Request는 Body를 필요로 하지 않습니다.

<a id="get-network-information-response-body"></a>
#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "networks": [
        {
            "adminStateUp": "{Administrative State}",
            "id": "{Network ID}",
            "name": "{Network Name}",
            "router:external": "{External Router Provided}",
            "status": "{Network Status}",
            "subnets": [
                "{Subnet ID}"
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Administrative State | Body | Boolean |네트워크 관리 상태. true: up, false: down |
| Network ID | Body | String | 네트워크 ID |
| Network Name | Body | String | 네트워크 이름 |
| External Router Provided | Body | Boolean | 라우터를 통한 플로팅 IP 제공 가능 여부 |
| Network Status | Body | String | 네트워크 상태. ACTIVE, DOWN, BUILD, ERROR |
| Subnet ID | Body | String | 서브넷 ID |

<a id="subnet-api"></a>
## 서브넷 API { #subnet-api }
<a id="get-subnet-information"></a>
### 서브넷 정보 조회 { #get-subnet-information }
접근 가능한 서브넷의 정보를 조회합니다.
<a id="get-subnet-information-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/subnets
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | 토큰 ID |

<a id="get-subnet-information-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="get-subnet-information-response-body"></a>
#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "subnets": [
        {
            "allocationPools": [
                {
                    "start": "{Start IP}",
                    "end": "{End IP}"
                }
            ],
            "cidr": "{CIDR}",
            "enableDhcp": "{Enable DHCP}",
            "gatewayIp": "{Gateway IP}",
            "hostRoutes": [],
            "id": "{Subnet ID}",
            "ipVersion": "{IP version}",
            "name": "{Subnet Name}",
            "networkId": "{Network ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Start IP | Body | String | 할당 Pool의 시작 IP. 예) 10.161.244.13 |
| End IP | Body | String | 할당 Pool의 마지막 IP. 예) 10.161.244.121 |
| CIDR | Body | String | Classless Inter-Domain Routing. 예) 10.161.244.0/25 |
| Enable DHCP | Body | Boolean | DHCP 활성화 여부 |
| Subnet ID | Body | String | 서브넷 ID |
| IP Version | Body | Integer | 서브넷의 IP 버전 |
| Subnet Name | Body | Integer | 서브넷 이름 |
| Network ID | Body | Integer | 서브넷이 속한 네트워크 ID |

<a id="floating-ip-api"></a>
## 플로팅 IP API { #floating-ip-api }
플로팅 IP 생성, 삭제, 정보 조회 기능을 제공합니다.

<a id="floating-ip-status"></a>
### 플로팅 IP Status { #floating-ip-status }
플로팅 IP는 다음 상태값을 갖습니다.

| Status | Description |
| -- | -- |
| ACTIVE | 플로팅 IP가 인스턴스와 연결되어 사용중인 상태 |
| DOWN | 플로팅 IP가 연결되어 있지 않은 상태 |
| ERROR | 에러 발생 |


<a id="list-pool-of-floating-ips"></a>
### 플로팅 IP Pool 조회 { #list-pool-of-floating-ips }
플로팅 IP Pool 목록을 조회합니다.

<a id="list-pool-of-floating-ips-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ip-pools
X-Auth-Token: {tokenId}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

<a id="list-pool-of-floating-ips-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="list-pool-of-floating-ips-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true,
        "resultCode" :  0
    },
    "pools" : [
         {
              "id" :  "{Pool ID}",
              "name" :  "{Pool Name}"
         }
    ]
}
```
|  Name | In | Type | Description |
|--|--|--|--|
| Pool ID | Body | String | 플로팅 IP Pool 식별자 |
| Pool Name | Body | String | 플로팅 IP Pool 이름 |


<a id="get-floating-ip"></a>
### 플로팅 IP 조회 { #get-floating-ip }
사용 가능한, 또는 사용 중인 플로팅 IP 정보를 조회합니다.
<a id="get-floating-ip-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |토큰 ID |
| floatingIpId | Query | String | O | 조회할 플로팅 IP의 ID. 기재하지 않을 경우 모든 플로팅 IP의 정보를 조회합니다. |

<a id="get-floating-ip-request-body"></a>
#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

<a id="get-floating-ip-response-body"></a>
#### Response Body
```json
{
	"header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "floatingips": [
        {
        	"id": "{Floating IP ID}",
            "floatingIpAddress": "{Floating IP Address}",
            "fixedIpAddress": "{Fixed IP Address}",
            "portId": "{Port ID}",
            "routerId": "{Router ID}",
            "pool" : {
                "id" :  "{Pool ID}",
                "name" :  "{Pool Name}"
            },
            "status": "{Status}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | 플로팅 IP ID |
| Floating IP Address | Body | String | 플로팅 IP 주소 |
| Fixed IP Address | Body | String | 플로팅 IP가 연결된 인스턴스 NIC의 IP 주소. Status가 "ACTIVE" 인 경우에만 표시 |
| Port ID | Body | String | 플로팅 IP가 연결된 포트 ID. 상태가 "ACTIVE" 인 경우에만 표시 |
| Router ID | Body | String | 플로팅 IP의 라우터 ID. 상태가 "ACTIVE" 인 경우에만 표시 |
| Pool ID | Body | String | 플로팅 IP가 속한 Pool 식별자 |
| Pool Name | Body | String | 플로팅 IP가 속한 Pool 이름 |
| Status | Body | String | 플로팅 IP의 상태 |

<a id="create-floating-ips"></a>
### 플로팅 IP 생성 { #create-floating-ips }
플로팅 IP를 생성합니다.
<a id="create-floating-ips-method-url"></a>
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/floating-ips
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

<a id="create-floating-ips-request-body"></a>
#### Request Body
```json
{
        "pool" : {
                "id" :  "{Pool ID}"
        }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
|  Pool ID | Body | String | - | 플로팅 IP Pool 식별자 |

<a id="create-floating-ips-response-body"></a>
#### Response Body
```json
{
	"header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "floatingip": {
    	"id": "{Floating IP ID}",
        "floatingIpAddress": "{Floating IP Address}",
        "pool": {
              "id" :  "{Pool ID}",
              "name" :  "{Pool Name}"
        },
        "status": "{Status}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | 플로팅 IP ID |
| Floating IP Address | Body | String | 플로팅 IP 주소 |
| Pool ID | Body | String | 플로팅 IP가 속한 Pool 식별자 |
| Pool Name | Body | String | 플로팅 IP가 속한 Pool 이름 |
| Status | Body | String | 플로팅 IP의 상태 |

<a id="delete-floating-ip"></a>
### 플로팅 IP 삭제 { #delete-floating-ip }
지정한 플로팅 IP를 삭제합니다. 사용중(ACTIVE)인 플로팅 IP는 연결 해제 후 삭제할 수 있습니다.
<a id="delete-floating-ip-method-url"></a>
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| floatingIpId | Path | String | - | 삭제할 플로팅 IP ID |

<a id="delete-floating-ip-request-body"></a>
#### Request Body
이 API는 request body를 필요로 하지 않습니다.

<a id="delete-floating-ip-response-body"></a>
#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
