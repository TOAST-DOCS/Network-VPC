<a id="network-vpc-api-guide"></a>
## Network > VPC > API Guide { #network-vpc-api-guide }

API is currently available only in the Korea region.

<a id="prerequisites"></a>
## Prerequisites { #prerequisites }

Using a network VPC API requires an appkey and a token. Get an appkey and a token by using [API Endpoint URL](/Compute/Instance/en/api-guide/#api-endpoint-url) and [Token API](/Compute/Instance/en/api-guide/#api): include the appkey to API Endpoint URL and the token to the Request Body.

For example, List Security Groups must be requested to the following URL:  

	GET https://api-compute.nhncloudservice.com/compute/v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}


<a id="security-group-api"></a>
## Security Group API { #security-group-api }
Create, delete, list and update security groups are available. Register/unregister security groups to instances is provided by [Instance API](/Compute/Instance/en/api-guide/).

<a id="list-security-groups"></a>
### List Security Groups { #list-security-groups }
List information of accessible security groups.

<a id="list-security-groups-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupId | Query | String | O | Security Group ID to list: if left empty, list information of all security groups. |

<a id="list-security-groups-request-body"></a>
#### Request Body
This API does not require a request body.

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
| Description | Body | String | Description of a security group |
| Security Group ID | Body | String | Security group ID |
| Name | Body | String | Name of a security group |
| securityGroupRules | Body | List | List of security group rules, in reference of [Security Group Rules API](#api_1) |

<a id="create-security-groups"></a>
### Create Security Groups { #create-security-groups }
Create a new security group.

<a id="create-security-groups-method-url"></a>
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-groups
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |

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
| Name | Body | String | - |Name of a security group |
| Description | Body | String | O | Description of a security group |

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
| Description | Body | String | Description of a security group |
| Security Group ID | Body | String | Security group ID |
| Name | Body | String | Name of a security group |
| securityGroupRules | Body | List | List of security group rules, in reference of [Security Group Rules API](#api_1) |

<a id="modify-security-groups"></a>
### Modify Security Groups { #modify-security-groups }
Modify name and description of a security group.

<a id="modify-security-groups-method-url"></a>
#### Method, URL
```
PUT /v1.0/appkeys/{appkey}/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupId | Path | String | - | Security group ID to modify |

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
| Name | Body | String | - | Name of a security group |
| Description | Body | String | O | Description of a security group |

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
| Security Group ID | Body | String | Security group ID |
| Name | Body | String | Name of a security group |
| Description | Body | String | Description of a security group |

<a id="delete-security-groups"></a>
### Delete Security Groups { #delete-security-groups }
Delete a specified security group.

<a id="delete-security-groups-method-url"></a>
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupId | Query | String | - | Security group ID to delete |

<a id="delete-security-groups-request-body"></a>
#### Request Body
This API does not require a request body.

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
## Security Group Rules API { #security-group-rules-api }
Add/Delete and List security group rules.  

<a id="list-security-group-rules"></a>
### List Security Group Rules { #list-security-group-rules }
List information of all accessible security group rules.  
<a id="list-security-group-rules-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupRuleId | Query | String | O | ID of a security group rule to retrieve: if left empty, retrieve information of all security group rules. |

<a id="list-security-group-rules-request-body"></a>
#### Request Body
This API does not require a request body.

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
| Direction | Body | String | Direction where rules are applied: "ingress" or "egress" |
| Ethernet Type | Body | String | "IPv4" or "IPv6" |
| Rule ID | Body | String | Security group rules ID |
| Port Range MAX | Body | Integer | Maximum port number where rules are applied |
| Port Range MIN | Body | Integer | Minimum port number where rules are applied |
| Protocol | Body | String | IP protocol: "icmp", "tcp", "udp", or "null" |
| Remote Group ID | Body | String | ID of a remote group where rules are applied |
| Remote IP Prefix | Body | String | Prefix of a remote IP where rules are applied |
| Security Group ID | Body | String | ID of a security group where rules are applied |

<a id="create-security-group-rules"></a>
### Create Security Group Rules { #create-security-group-rules }
Create a new security group rule.
<a id="create-security-group-rules-method-url"></a>
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-group-rules
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |

<a id="create-security-group-rules-request-body"></a>
#### Request Body
```json
{
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "portRangeMin": "{Port Range MAX}",
        "portRangeMax": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| Direction | Body | String | - | Direction where rules are applied: "ingress" or "egress" |
| Ethernet Type | Body | String | - | "IPv4" or "IPv6" |
| Port Range MAX | Body | Integer | O | Maximum port number where rules are applied, between 1 and 65535: with this setting, cannot omit "protocol" |
| Port Range MIN | Body | Integer | O | Minimum port number where rules are applied, between 1 and 65535: with this setting, cannot omit "protocol" |
| Protocol | Body | String | O | IP protocol: "icmp", "tcp", "udp", or "null" |
| Remote Group ID | Body | String | O | ID of a remote security group where rules are applied <br />: to be omitted if "remoteIpPrefix" is set <Paste> |
| Remote IP Prefix | Body | String | O | Prefix of a remote IP where rules are applied <br />: to be omitted if "remoteGroupId" is set. |
| Security Group ID | Body | String | - | ID of a security group where rules are applied |

> [Notice]
> If both `Remote Group ID` and` Remote IP Prefix` is omitted, the default value 0.0.0.0/0 is set to.
> In other words, a rule that applies to all traffic is created.

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
| Direction | Body | String | Direction where rules are applied: "ingress" or "egress" |
| Ethernet Type | Body | String | "IPv4" or "IPv6" |
| Rule ID | Body | String | ID of security group rules |
| Port Range MAX | Body | Integer | Maximum port number where rules are applied |
| Port Range MIN | Body | Integer | Minimum port number where rules are applied |
| Protocol | Body | String | IP protocol: "icmp", "tcp", "udp", or "null" |
| Remote Group ID | Body | String | ID of a remote security group where rules are applied |
| Remote IP Prefix | Body | String | Prefix of a remote IP where rules are applied |
| Security Group ID | Body | String | ID of a security group where rules are applied |

<a id="delete-security-group-rules"></a>
### Delete Security Group Rules { #delete-security-group-rules }
Delete specified security group rules.
<a id="delete-security-group-rules-method-url"></a>
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupRuleId | Query | String | - | ID of a security group rule to delete |

<a id="delete-security-group-rules-request-body"></a>
#### Request Body
This API does not require a request body.

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
## Network API { #network-api }
Get network information which can be accessed from an instance.

<a id="network-status"></a>
### Network Status { #network-status }
Networks have the following status values:

| Status | Description |
| -- | -- |
| BUILD | Network is building |
| ACTIVE | Network is active |
| DOWN | Network is inactive |
| ERROR | Error has occurred |

<a id="get-network-information"></a>
### Get Network Information { #get-network-information }
Get information of an accessible network.

<a id="get-network-information-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/networks?id={networkId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | Token ID |
| networkId | Query | String | O | Network ID to retrieve: if left empty, retrieve information of all networks. |

<a id="get-network-information-request-body"></a>
#### Request Body
This API does not require a request body.

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
| Administrative State | Body | Boolean |Network management status: true: up, or false: down |
| Network ID | Body | String | Network ID |
| Network Name | Body | String | Network name |
| External Router Provided | Body | Boolean | Availability of a floating IP provided via router |
| Network Status | Body | String | Network status: ACTIVE, DOWN, BUILD, or ERROR |
| Subnet ID | Body | String | Subnet ID |

<a id="subnet-api"></a>
## Subnet API { #subnet-api }
<a id="get-subnet-information"></a>
### Get Subnet Information { #get-subnet-information }
Get information of an accessible subnet.
<a id="get-subnet-information-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/subnets
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | Token ID |

<a id="get-subnet-information-request-body"></a>
#### Request Body
This API does not require a request body.

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
| Start IP | Body | String | Starting IP of an allocated pool e.g) 10.161.244.13 |
| End IP | Body | String | Last IP of an allocated pool e.g) 10.161.244.121 |
| CIDR | Body | String | Refers to Classless Inter-Domain Routing, one of the ways to allocate IP addresses  e.g.) 10.161.244.0/25 |
| Enable DHCP | Body | Boolean | Whether DHCP is enable |
| Subnet ID | Body | String | Subnet ID |
| IP Version | Body | Integer | IP version of a subnet |
| Subnet Name | Body | Integer | Subnet name |
| Network ID | Body | Integer | Network ID where a subnet belongs to |

<a id="floating-ip-api"></a>
## Floating IP API { #floating-ip-api }
Create, Delete, and Retrieve Floating IPs.

<a id="floating-ip-status"></a>
### Floating IP Status { #floating-ip-status }
Floating IPs have the following status values:

| Status | Description |
| -- | -- |
| ACTIVE | Floating IP is associated to an instance and now in use |
| DOWN | Floating IP is not associated |
| ERROR | Error has occurred |


<a id="list-pool-of-floating-ips"></a>
### List Pool of Floating IPs { #list-pool-of-floating-ips }
List the pool of floating IPs.

<a id="list-pool-of-floating-ips-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ip-pools
X-Auth-Token: {tokenId}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |

<a id="list-pool-of-floating-ips-request-body"></a>
#### Request Body
This API does not require a request body.

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
| Pool ID | Body | String | Pool identifier of a floating IP |
| Pool Name | Body | String | Pool name of a floating IP |


<a id="get-floating-ip"></a>
### Get Floating IP { #get-floating-ip }
Get information of an available or in-use floating IP.
<a id="get-floating-ip-method-url"></a>
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |Token ID |
| floatingIpId | Query | String | O | ID of a floating IP to retrieve: if left empty, retrieve information of all floating IPs. |

<a id="get-floating-ip-request-body"></a>
#### Request Body
This API does not require a request body.

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
| Floating IP ID | Body | String | ID of a floating IP |
| Floating IP Address | Body | String | Address of a floating IP |
| Fixed IP Address | Body | String | IP address of instance NIC to which floating IP is associated. Displays only when the status is "ACTIVE". |
| Port ID | Body | String | Port ID to which a floating IP is associated. Displays only when the status is "ACTIVE". |
| Router ID | Body | String | Router ID of a floating IP. Displays only when the status is "ACTIVE". |
| Pool ID | Body | String | Pool identifier where a floating IP belongs to |
| Pool Name | Body | String | Name of a pool where a floating IP belongs to |
| Status | Body | String | Status of a floating IP |

<a id="create-floating-ips"></a>
### Create Floating IPs { #create-floating-ips }
Create a floating IP.
<a id="create-floating-ips-method-url"></a>
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/floating-ips
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |

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
|  Pool ID | Body | String | - | Pool identifier of a floating IP |

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
| Floating IP ID | Body | String | ID of a floating IP |
| Floating IP Address | Body | String | Address of a floating IP |
| Pool ID | Body | String | Pool identifier to where a floating IP belongs |
| Pool Name | Body | String | Name of a pool to where a floating IP belongs |
| Status | Body | String | Status of a floating IP |

<a id="delete-floating-ip"></a>
### Delete Floating IP { #delete-floating-ip }
Delete a designated floating IP. For an active floating IP, disassociate first and delete.  
<a id="delete-floating-ip-method-url"></a>
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |
| floatingIpId | Path | String | - | ID of a floating IP to delete |

<a id="delete-floating-ip-request-body"></a>
#### Request Body
This API does not require a  request body.

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
