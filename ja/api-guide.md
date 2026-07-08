<a id="network-vpc-api-guide"></a>
## Network > VPC > APIガイド { #network-vpc-api-guide }

APIは現在、韓国リージョンでのみ使用できます。

<a id="prerequisites"></a>
## 事前準備 { #prerequisites }

ネットワークVPC APIを使用するにはアプリケーションキーとトークンが必要です。 [API Endpoint URL](/Compute/Instance/ja/api-guide/#api-endpoint-url)と[トークンAPI](/Compute/Instance/ja/api-guide/#api)を利用してアプリケーションキーとトークンを準備します。アプリケーションキーはAPI Endpoint URLにトークンはRequest Bodyに含めて使用します。

例えば、セキュリティーグループリスト照会は次のURでリクエストする必要があります。

	GET https://api-compute.nhncloudservice.com/compute/v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}


<a id="security-group-api"></a>
## セキュリティーグループAPI { #security-group-api }
セキュリティーグループの生成、削除、照会およびアップデート機能を提供します。セキュリティーグループをインスタンスに登録、解除する機能は[インスタンスAPI](/Compute/Instance/ja/api-guide/)を通して提供されます。

<a id="list-security-groups"></a>
### セキュリティーグループリスト照会 { #list-security-groups }
アクセス可能なセキュリティーグループの情報を照会します。

<a id="list-security-groups-method-url"></a>
#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |
| securityGroupId | Query | String | O | 照会するセキュリティーグループID。記載していない場合、全てのセキュリティーグループの情報を照会します。 |

<a id="list-security-groups-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="list-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }、
    "securityGroups": [
        {
            "description": "{Desctiption}"、
            "id": "{Security Group ID}"、
            "name": "{Name}"、
            "securityGroupRules": [
                {
                    "direction": "egress"、
                    "ethertype": "IPv4"、
                    "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff"、
                    "portRangeMax": null、
                    "portRangeMin": null、
                    "protocol": null、
                    "remoteGroupId": null、
                    "remoteIpPrefix": null、
                    "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5"、
                    "description": ""
                }
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | セキュリティーグループの説明 |
| Security Group ID | Body | String | セキュリティーグループのID |
| Name | Body | String | セキュリティーグループの名前 |
| securityGroupRules | Body | List | セキュリティーグループ規則リスト、 [セキュリティーグループ規則API](#api_1)参照 |

<a id="create-security-groups"></a>
### セキュリティーグループ作成 { #create-security-groups }
新たなセキュリティーグループを作成します。

<a id="create-security-groups-method-url"></a>
#### Method、 URL
```
POST /v1.0/appkeys/{appkey}/security-groups
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |

<a id="create-security-groups-request-body"></a>
#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}"、
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| name | Body | String | - |セキュリティーグループの名前 |
| description | Body | String | O | セキュリティーグループの説明 |

<a id="create-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }、
    "securityGroup": {
        "description": "{Description}"、
        "id": "{Security Group ID}"、
        "name": "{Name}"、
        "securityGroupRules": [
            {
                "direction": "egress"、
                "ethertype": "IPv4"、
                "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff"、
                "portRangeMax": null、
                "portRangeMin": null、
                "protocol": null、
                "remoteGroupId": null、
                "remoteIpPrefix": null、
                "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5"、
                "description": ""
            }
        ]
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | セキュリティーグループの説明 |
| Security Group ID | Body | String | セキュリティーグループのID |
| Name | Body | String | セキュリティーグループの名前 |
| securityGroupRules | Body | List | セキュリティーグループ規則リスト、 [セキュリティーグループ規則API](#api_1)参照 |

<a id="modify-security-groups"></a>
### セキュリティーグループ修正 { #modify-security-groups }
セキュリティーグループの名前、説明を変更します。

<a id="modify-security-groups-method-url"></a>
#### Method、 URL
```
PUT /v1.0/appkeys/{appkey}/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |
| securityGroupId | Path | String | - | 変更するセキュリティーグループのID |

<a id="modify-security-groups-request-body"></a>
#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}"、
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Name | Body | String | - | セキュリティーグループの名前 |
| Description | Body | String | O | セキュリティーグループの説明 |

<a id="modify-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }、
    "securityGroup": {
        "id": "{Security Group ID}"、
        "name": "{Name}"、
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Security Group ID | Body | String | セキュリティーグループのID |
| Name | Body | String | セキュリティーグループの名前 |
| Description | Body | String | セキュリティーグループの説明 |

<a id="delete-security-groups"></a>
### セキュリティーグループ削除 { #delete-security-groups }
指定したセキュリティーグループを削除します。

<a id="delete-security-groups-method-url"></a>
#### Method、 URL
```
DELETE /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |
| securityGroupId | Query | String | - | 削除するセキュリティーグループID |

<a id="delete-security-groups-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="delete-security-groups-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }
}
```


<a id="security-group-rules-api"></a>
## セキュリティーグループ規則API { #security-group-rules-api }
セキュリティーグループ規則追加/削除及び照会機能を提供します。

<a id="list-security-group-rules"></a>
### セキュリティーグループ規則照会 { #list-security-group-rules }
アクセス可能な全てのセキュリティーグループ規則の情報を照会します。
<a id="list-security-group-rules-method-url"></a>
#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |
| securityGroupRuleId | Query | String | O | 照会するセキュリティーグループ規則のID。記載していない場合、全てのセキュリティーグループ規則の情報を照会します。 |

<a id="list-security-group-rules-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="list-security-group-rules-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }、
    "securityGroupRules": [
        {
            "direction": "{Direction}"、
            "ethertype": "{Ethernet Type}"、
            "id": "{Rule ID}"、
            "portRangeMax": "{Port Range MAX}"、
            "portRangeMin": "{Port Range MIN}"、
            "protocol": "{Protocol}"、
            "remoteGroupId": "{Remote Group ID}"、
            "remoteIpPrefix": "{Remote IP Prefix}"、
            "securityGroupId": "{Security Group ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | 規則が適用される方向、 "ingress"または"egress" |
| Ethernet Type | Body | String | "IPv4"または"IPv6" |
| Rule ID | Body | String | セキュリティーグループ規則のID |
| Port Range MAX | Body | Integer | 規則が適用される最大ポート番号 |
| Port Range MIN | Body | Integer | 規則が適用される最小ポート番号 |
| Protocol | Body | String | IPプロトコル"icmp"、 "tcp"、 "udp"、 "null" |
| Remote Group ID | Body | String | 規則が適用されるRemote GroupのID |
| Remote IP Prefix | Body | String | 規則が適用されるRemote IPのPrefix |
| Security Group ID | Body | String | 規則が適用されるセキュリティーグループのID |

<a id="create-security-group-rules"></a>
### セキュリティーグループ規則の作成 { #create-security-group-rules }
新しいセキュリティーグループ規則を作成します。
<a id="create-security-group-rules-method-url"></a>
#### Method、 URL
```
POST /v1.0/appkeys/{appkey}/security-group-rules
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |

<a id="create-security-group-rules-request-body"></a>
#### Request Body
```json
{
    "securityGroupRule": {
        "direction": "{Direction}"、
        "ethertype": "{Ethernet Type}"、
        "portRangeMin": "{Port Range MAX}"、
        "portRangeMax": "{Port Range MIN}"、
        "protocol": "{Protocol}"、
        "remoteGroupId": "{Remote Group ID}"、
        "remoteIpPrefix": "{Remote IP Prefix}"、
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| Direction | Body | String | - | 規則が適用される方向、 "ingress"または"egress" |
| Ethernet Type | Body | String | - | "IPv4"または"IPv6" |
| Port Range MAX | Body | Integer | O | 規則が適用される最大ポート番号。 1~65535の範囲。設定時、 "protocol"項目省略不可 |
| Port Range MIN | Body | Integer | O | 規則が適用される最小ポート番号。 1~65535の範囲。設定時、 "protocol"項目省略不可 |
| Protocol | Body | String | O | IPプロトコル。 "icmp"、 "tcp"、 "udp"、 null. |
| Remote Group ID | Body | String | O | 規則が適用されるRemoteセキュリティーグループのID。 <br />"remoteIpPrefix"値を設定する場合は省略<Paste> |
| Remote IP Prefix | Body | String | O | 規則が適用されるRemote IPのPrefix。 <br />"remoteGroupId"値を設定する場合は省略。 |
| Security Group ID | Body | String | - | 規則が適用されるセキュリティーグループのID |

> [Notice]
> `Remote Group ID`と`Remote IP Prefix`両方省略した場合、デフォルト値0.0.0.0/0に指定されます。
> つまり、すべてのトラフィックに適用されるルールが作成されます。

##### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }、
    "securityGroupRule": {
        "direction": "{Direction}"、
        "ethertype": "{Ethernet Type}"、
        "id": "{Rule ID}"、
        "portRangeMax": "{Port Range MAX}"、
        "portRangeMin": "{Port Range MIN}"、
        "protocol": "{Protocol}"、
        "remoteGroupId": "{Remote Group ID}"、
        "remoteIpPrefix": "{Remote IP Prefix}"、
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | 規則が適用される方向、 "ingress"または"egress" |
| Ethernet Type | Body | String | "IPv4"または"IPv6" |
| Rule ID | Body | String | セキュリティーグループ規則ID |
| Port Range MAX | Body | Integer | 規則が適用される最大ポート番号 |
| Port Range MIN | Body | Integer | 規則が適用される最小ポート番号 |
| Protocol | Body | String | IPプロトコル。 "icmp"、 "tcp"、 "udp"、 "null" |
| Remote Group ID | Body | String | 規則が適用されるRemoteセキュリティーグループのID |
| Remote IP Prefix | Body | String | 規則が適用されるRemote IPのPrefix |
| Security Group ID | Body | String | 規則が適用されるセキュリティーグループのID |

<a id="delete-security-group-rules"></a>
### セキュリティーグループ規則の削除 { #delete-security-group-rules }
指定したセキュリティーグループ規則を削除します。
<a id="delete-security-group-rules-method-url"></a>
#### Method、 URL
```
DELETE /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuelsId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | トークンID |
| securityGroupRuleId | Query | String | - | 削除するセキュリティーグループ規則ID |

<a id="delete-security-group-rules-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="delete-security-group-rules-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true、
        "resultCode" :  0、
        "resultMessage" :  "SUCCESS"
    }
}
```

<a id="network-api"></a>
## ネットワークAPI { #network-api }
インスタンスから接続できるネットワーク情報照会機能を提供します。

<a id="network-status"></a>
### ネットワーク状態 { #network-status }
ネットワークは次の状態値を持ちます。

| Status | Description |
| -- | -- |
| BUILD | ネットワーク構築中 |
| ACTIVE | ネットワークアクティブ状態 |
| DOWN | ネットワークダウン状態 |
| ERROR | エラー発生 |

<a id="get-network-information"></a>
### ネットワーク情報の照会 { #get-network-information }
アクセス可能なネットワークの情報を照会します。

<a id="get-network-information-method-url"></a>
#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/networks?id={networkId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | トークンID |
| networkId | Query | String | O | 照会するネットワークID。記載していない場合、全てのネットワークの情報を照会します。 |

<a id="get-network-information-request-body"></a>
#### Request Body
このRequestはBodyを必要としません。

<a id="get-network-information-response-body"></a>
#### Response Body
```json
{
    "header": {
        "isSuccessful": true、
        "resultCode": 0、
        "resultMessage": "SUCCESS"
    }、
    "networks": [
        {
            "adminStateUp": "{Administrative State}"、
            "id": "{Network ID}"、
            "name": "{Network Name}"、
            "router:external": "{External Router Provided}"、
            "status": "{Network Status}"、
            "subnets": [
                "{Subnet ID}"
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Administrative State | Body | Boolean |ネットワーク管理状態。 true: up、 false: down |
| Network ID | Body | String | ネットワークID |
| Network Name | Body | String | ネットワークの名前 |
| External Router Provided | Body | Boolean | ルーターによるFloating IP提供可否 |
| Network Status | Body | String | ネットワーク状態。 ACTIVE、 DOWN、 BUILD、 ERROR |
| Subnet ID | Body | String | サブネットID |

<a id="subnet-api"></a>
## サブネットAPI { #subnet-api }
<a id="get-subnet-information"></a>
### サブネット情報照会 { #get-subnet-information }
アクセス可能なサブネットの情報を照会します。
<a id="get-subnet-information-method-url"></a>
#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/subnets
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | トークンID |

<a id="get-subnet-information-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="get-subnet-information-response-body"></a>
#### Response Body
```json
{
    "header": {
        "isSuccessful": true、
        "resultCode": 0、
        "resultMessage": "SUCCESS"
    }、
    "subnets": [
        {
            "allocationPools": [
                {
                    "start": "{Start IP}"、
                    "end": "{End IP}"
                }
            ]、
            "cidr": "{CIDR}"、
            "enableDhcp": "{Enable DHCP}"、
            "gatewayIp": "{Gateway IP}"、
            "hostRoutes": []、
            "id": "{Subnet ID}"、
            "iPvErsion": "{IP version}"、
            "name": "{Subnet Name}"、
            "networkId": "{Network ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Start IP | Body | String | 割り当てPoolの開始IP。例) 10.161.244.13 |
| End IP | Body | String | 割り当てPoolの最後のIP。例) 10.161.244.121 |
| CIDR | Body | String | Classless Inter-Domain Routing. 例) 10.161.244.0/25 |
| Enable DHCP | Body | Boolean | DHCP活性化可否 |
| Subnet ID | Body | String | サブネットID |
| IP Version | Body | Integer | サブネットのIPバージョン |
| Subnet Name | Body | Integer | サブネットの名前 |
| Network ID | Body | Integer | サブネットが属するネットワークID |

<a id="floating-ip-api"></a>
## Floating IP API { #floating-ip-api }
Floating IP生成、削除、情報照会機能を提供します。

<a id="floating-ip-status"></a>
### Floating IP Status { #floating-ip-status }
Floating IPは次の状態値を持ちます。

| Status | Description |
| -- | -- |
| ACTIVE | Floating IPがインスタンスと接続されていて使用中の状態 |
| DOWN | Floating IPが接続されていない状態 |
| ERROR | エラー発生 |


<a id="list-pool-of-floating-ips"></a>
### Floating IP Pool照会 { #list-pool-of-floating-ips }
Floating IP Poolリストを照会します。

<a id="list-pool-of-floating-ips-method-url"></a>
#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/floating-ip-pools
X-Auth-Token: {tokenId}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |

<a id="list-pool-of-floating-ips-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="list-pool-of-floating-ips-response-body"></a>
#### Response Body
```json
{
    "header" : {
        "resultMessage" :  "SUCCESS"、
        "isSuccessful" :  true、
        "resultCode" :  0
    }、
    "pools" : [
         {
              "id" :  "{Pool ID}"、
              "name" :  "{Pool Name}"
         }
    ]
}
```
|  Name | In | Type | Description |
|--|--|--|--|
| Pool ID | Body | String | Floating IP Poolの識別子 |
| Pool Name | Body | String | Floating IP Poolの名前 |


<a id="get-floating-ip"></a>
### Floating IP照会 { #get-floating-ip }
使用可能または使用中のFloating IP情報を照会します。
<a id="get-floating-ip-method-url"></a>
#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |トークンID |
| floatingIpId | Query | String | O | 照会するFloating IPのID。記載していない場合、全てのFloating IPの情報を照会します。 |

<a id="get-floating-ip-request-body"></a>
#### Request Body
このAPIはRequest Bodyを必要としません。

<a id="get-floating-ip-response-body"></a>
#### Response Body
```json
{
	"header": {
        "isSuccessful": true、
        "resultCode": 0、
        "resultMessage": "SUCCESS"
    }、
    "floatingips": [
        {
        	"id": "{Floating IP ID}"、
            "floatingIpAddress": "{Floating IP Address}"、
            "fixedIpAddress": "{Fixed IP Address}"、
            "portId": "{Port ID}"、
            "routerId": "{Router ID}"、
            "pool" : {
                "id" :  "{Pool ID}"、
                "name" :  "{Pool Name}"
            }、
            "status": "{Status}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | Floating IP ID |
| Floating IP Address | Body | String | Floating IPアドレス |
| Fixed IP Address | Body | String | Floating IPが接続されたインスタンスNICのIPアドレス。 Statusが"ACTIVE"の場合にのみ表示 |
| Port ID | Body | String | Floating IPが接続されたポートID。状態が"ACTIVE"の場合にのみ表示 |
| Router ID | Body | String | Floating IPのルーターID。状態が"ACTIVE"の場合にのみ表示 |
| Pool ID | Body | String | Floating IPが属するPoolの識別子 |
| Pool Name | Body | String | Floating IPが属するPoolの名前 |
| Status | Body | String | Floating IPの状態 |

<a id="create-floating-ips"></a>
### Floating IP生成 { #create-floating-ips }
Floating IPを生成します。
<a id="create-floating-ips-method-url"></a>
#### Method、 URL
```
POST /v1.0/appkeys/{appkey}/floating-ips
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |

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
|  Pool ID | Body | String | - | Floating IP Poolの識別子 |

<a id="create-floating-ips-response-body"></a>
#### Response Body
```json
{
	"header": {
        "isSuccessful": true、
        "resultCode": 0、
        "resultMessage": "SUCCESS"
    }、
    "floatingip": {
    	"id": "{Floating IP ID}"、
        "floatingIpAddress": "{Floating IP Address}"、
        "pool": {
              "id" :  "{Pool ID}"、
              "name" :  "{Pool Name}"
        }、
        "status": "{Status}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | Floating IP ID |
| Floating IP Address | Body | String | Floating IPアドレス |
| Pool ID | Body | String | Floating IPが属するPoolの識別子|
| Pool Name | Body | String | Floating IPが属するPoolの名前 |
| Status | Body | String | Floating IPの状態 |

<a id="delete-floating-ip"></a>
### Floating IP削除 { #delete-floating-ip }
指定したFloating IPを削除します。使用中(ACTIVE)のFloating IPは接続解除後に削除できます。
<a id="delete-floating-ip-method-url"></a>
#### Method、 URL
```
DELETE /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |
| floatingIpId | Path | String | - | 削除するFloating IPのID |

<a id="delete-floating-ip-request-body"></a>
#### Request Body
このAPIはrequest bodyを必要としません。

<a id="delete-floating-ip-response-body"></a>
#### Response Body

```json
{
    "header": {
        "isSuccessful": true、
        "resultCode": 0、
        "resultMessage": "SUCCESS"
    }
}
```

