## Network > VPC > Openstack互換APIガイド

NHN Cloud NetworkサービスはOpenStack neutron APIと互換性のあるAPIを提供します。 
提供するOpenstack互換APIは次のとおりです。

| Openstack APIメソッド | 用途 |
| --- | --- |
| GET networks | ネットワークリスト表示 |
| GET subnets | サブネットリスト表示 |
| GET ports | ポートリスト表示 |
| POST port | ポートを作成する |
| PUT port | ポートを変更する |
| DELETE port | ポートを削除する |

OpenStack互換APIのリクエスト及びレスポンスに含まれるフィールドはOpenStack neutron APIが提供する項目のうち、NHN Cloudのポリシーに基づき、文書に明示された項目に制限されます。

APIを使用するにはAPIエンドポイントとトークンなどが必要です。[API使用準備](https://docs.toast.com/ja/Compute/Compute/ja/identity-api/)を参照してAPIを使用するのに必要な情報を準備します。

Openstack互換APIは`network`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本(東京)リージョン<br>米国(カリフォルニア)リージョン | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com<br>https://us1-api-network-infrastructure.nhncloudservice.com |


## ネットワーク
### ネットワークリスト表示
使用可能なネットワークリストを返します。
```
GET /v2.0/networks
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するネットワークID |
| name | Query | String | - | 照会するネットワーク名 |
| provider:network_type | Query | Enum | - | 照会するネットワークタイプ<br>`flat`、`vlan`のどちらか |
| router:external | Query | Boolean | - | 照会するネットワークの外部接続有無 |
| shared | Query | Boolean | - | 照会するネットワークの共有有無 |
| tenant_id | Query | String | - | 照会するネットワークが属しているテナントID |
| sort_dir | Query | Enum | - | 照会するネットワークのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のどちらか |
| sort_key | Query | String | - | 照会するネットワークのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するネットワークのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| networks | Body | Array | ネットワーク情報オブジェクトリスト |
| networks.status | Body | Enum | ネットワークの状態<br>**ACTIVE**、**DOWN**、**BUILD**、**ERROR**のいずれか。 |
| networks.subnets | Body | Array | ネットワークに属しているサブネットのIDリスト |
| networks.name | Body | String | ネットワーク名 |
| networks.router:external | Body | Boolean | ネットワーク外部接続有無 |
| networks.tenant_id | Body | String | テナントID |
| networks.admin_state_up | Body | Boolean | 管理者制御状態<br>`true`：使用可能<br>`false`：使用不可 |
| networks.mtu | Body | Integer | 最大転送単位(Maximum Transmission Unit) |
| networks.shared | Body | Boolean | ネットワーク共有有無 |
| networks.port_security_enabled | Body | Boolean | ネットワークポートのセキュリティ有無<br>このネットワークで作成されるポートのセキュリティ有無を決定 |
| networks.id | Body | String | ネットワークID |
| networks.name | Body | String | ネットワーク名 |
| networks_links | Body | Array | ページネーション用の情報オブジェクト<br>`limit`、`offset`を追加した場合に返す<br>次のリストを指すパスを含む |

<details><summary>例</summary>
<p>

```json
{
  "networks": [
    {
      "status": "ACTIVE",
      "subnets": [
        "b83863ff-0355-4c73-8c10-0bdf66a69aab"
      ],
      "name": "Default Network",
      "router:external": false,
      "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
      "admin_state_up": true,
      "mtu": 0,
      "shared": false,
      "port_security_enabled": true,
      "id": "245ff686-4ca2-4176-a069-81013537ac3a"
    },
    {
      "name": "public_network",
      "id": "b04b1c31-f2e9-4ae0-a264-02b7d61ad618",
      "status": "ACTIVE",
      "shared": true,
      "subnets": [
        "6b3f7d6d-df61-4345-beb5-1621fd274659",
        "f22ae5cb-5e52-4704-9c31-83dc3826efb7"
      ],
      "admin_state_up": true,
      "port_security_enabled": true,
      "router:external": true,
      "tenant_id": "e873d250f2ca40b78e2c12cfbaaeb740",
      "mtu": 0
    }
  ]
}
```

</p>
</details>

---

## サブネット
### サブネットリスト表示
使用可能なサブネットリストを返します。
```
GET /v2.0/subnets
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するサブネットID |
| name | Query | String | - | 照会するサブネット名 |
| enable_dhcp | Query | Boolean | - | 照会するサブネットのDHCP有無 |
| network_id | Query | UUID | - | 照会するサブネットのネットワークID |
| cidr | Query | String | - | 照会するサブネットのCIDR |
| shared | Query | Boolean | - | 照会するサブネットの共有有無 |
| sort_dir | Query | Enum | - | 照会するサブネットのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query | String | - | 照会するサブネットのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するサブネットのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| subnets | Body | Array | サブネット情報オブジェクトリスト |
| subnets.name | Body | String | サブネット名 |
| subnets.enable_dhcp | Body | Boolean | サブネットのDHCP有無 |
| subnets.network_id | Body | UUID | サブネットのネットワークID |
| subnets.tenant_id | Body | String | テナントID |
| subnets.dns_nameservers | Body | Array | サブネットに接続されたDNSネームサーバーリスト |
| subnets.gateway_ip | Body | String | サブネットのゲートウェイIP |
| subnets.ipv6_ra_mode | Body | Boolean | IPv6のRouter Advertisementモード |
| subnets.allocation_pools | Body | Array | サブネットIP範囲オブジェクトリスト |
| subnets.allocation_pools.start | Body | String | サブネットIP範囲の最初のIPアドレス |
| subnets.allocation_pools.end | Body | String | サブネットIP範囲の最後のIPアドレス |
| subnets.host_routes | Body | Array | サブネットの追加パス情報リスト |
| subnets.host_routes.destination | Body | String | 宛先<br>宛先アドレスが`destination`の場合<br>`nexthop`に指定されたアドレスへ伝達 |
| subnets.host_routes.nexthop | Body | String | 次のhopアドレス |
| subnets.ip_version | Body | Integer | IPプロトコルバージョン<br>4または6 |
| subnets.ipv6_address_mode | Body | String | IPv6のアドレス割り当てモード |
| subnets.cidr | Body | String | サブネットのCIDR |
| subnets.id | Body | UUID | サブネットのID |
| subnets.subnetpool_id | Body | UUID | サブネットPool ID |
| subnets_links | Body | Array | ページネーション用の情報オブジェクト<br>`limit`、`offset`を追加した場合に返す<br>次のリストを指すパスを含む |

<details><summary>例</summary>
<p>

```json
{
  "subnets": [
    {
      "name": "default",
      "enable_dhcp": true,
      "network_id": "245ff686-4ca2-4176-a069-81013537ac3a",
      "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
      "dns_nameservers": [],
      "gateway_ip": "192.168.0.1",
      "ipv6_ra_mode": null,
      "allocation_pools": [
        {
          "start": "192.168.0.2",
          "end": "192.168.0.254"
        }
      ],
      "host_routes": [],
      "ip_version": 4,
      "ipv6_address_mode": null,
      "cidr": "192.168.0.0/24",
      "id": "b83863ff-0355-4c73-8c10-0bdf66a69aab",
      "subnetpool_id": null
    }
  ]
}
```

</p>
</details>

---

## ポート
### ポートリスト表示
ポートリストを返します。
```
GET /v2.0/ports
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するポートID |
| status | Query | Enum | - | 照会するポート状態<br>`ACTIVE`, `BUILD`, `DOWN`のいずれか |
| name | Query | String | - | 照会するポート名 |
| admin_state | Query | Boolean | - | 照会するポートの管理者制御状態 |
| network_id | Query | UUID | - | 照会するポートのネットワークID |
| tenant_id | Query | String | - | 照会するポートのテナントID |
| fixed_ips | Query | array | - | 照会するポートの固定IP情報。<br>IPアドレス(`ip_address`), IPアドレス一部(`ip_address_substr`)、サブネットID(`subnet_id`)でフィルタリング可能 | 
| mac_address | Query | String | - | 照会するポートのMACアドレス |
| device_owner | Query | String | - | 照会するポートを使用するリソースの種類 |
| device_id | Query | UUID | - | 照会するポートを使用するリソースID |
| fields | Query | String | - | 照会するポートのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| ports | Body | Array | ポート情報オブジェクトリスト |
| ports.id | Body | UUID | ポートのID |
| ports.name | Body | String | ポート名 |
| ports.status | Body | Enum | ポート状態<br>`ACTIVE`, `BUILD`, `DOWN`のいずれか |
| ports.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| ports.network_id | Body | UUID | ポートのネットワークID |
| ports.tenant_id | Body | String | テナントID |
| ports.extra_dhcp_opts | Body | Array | 追加DHCPオプション |
| ports.binding:vnic_type | Body | String | ポートに接続されたvNICタイプ |
| ports.device_owner | Body | String | ポートを使用するリソースの種類 |
| ports.device_id | Body | UUID | ポートを使用するリソースID |
| ports.mac_address | Body | String | ポートのMACアドレス |
| ports.fixed_ips | Body | Array | ポートの固定IPリスト |
| ports.fixed_ips.subnet_id | Body | UUID | 固定IPが属するサブネットID |
| ports.fixed_ips.ip_address | Body | String | 固定IPアドレス |
| ports.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>有効の場合、セキュリティグループ、許可アドレスペアを設定可能 |
| ports.security_groups | Body | Array | ポートに設定されたセキュリティグループIDリスト |
| ports.allowed_address_pairs | Body | Array | ポートに設定された許可アドレスのペアリスト |

<details><summary>例</summary>
<p>

```json
{
  "ports": [
    {
      "status": "ACTIVE",
      "name": "",
      "allowed_address_pairs": [],
      "admin_state_up": true,
      "network_id": "c46ee4e8-c9fa-462e-8a3b-55b1f11dd8f8",
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "extra_dhcp_opts": [],
      "binding:vnic_type": "normal",
      "device_owner": "compute:kr-pub-a",
      "mac_address": "fa:16:3e:1a:ed:34",
      "port_security_enabled": true,
      "fixed_ips": [
        {
          "subnet_id": "eb166bdf-dd99-4b02-ac8e-64be21dff2d5",
          "ip_address": "192.168.0.15"
        }
      ],
      "id": "2a83bc25-ed76-4a2b-83b7-a4408aa99c4a",
      "security_groups": [
        "877b092c-2a31-4509-8d23-deeb02d95633"
      ],
      "device_id": "10b9643d-9bc8-4b0f-a5dd-cfcb4033d70b"
    }
  ]
}
```

</p>
</details>

---

### ポート表示

```
GET /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| portId | URL | UUID | O | ポートID |
| tokenId | Header | String | O | トークンID |
| fields | Query | String | - | 照会するポートのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| port | Body | Array | ポート情報オブジェクト |
| port.id | Body | UUID | ポートのID |
| port.name | Body | String | ポート名 |
| port.status | Body | Enum | ポート状態<br>`ACTIVE`, `BUILD`, `DOWN`のいずれか |
| port.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| port.network_id | Body | UUID | ポートのネットワークID |
| port.tenant_id | Body | String | テナントID |
| port.extra_dhcp_opts | Body | Array | 追加DHCPオプション |
| port.binding:vnic_type | Body | String | ポートに接続されたvNICタイプ |
| port.device_owner | Body | String | ポートを使用するリソースの種類 |
| port.device_id | Body | UUID | ポートを使用するリソースID |
| port.mac_address | Body | String | ポートのMACアドレス |
| port.fixed_ips | Body | Array | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | 固定IPが属するサブネットID |
| port.fixed_ips.ip_address | Body | String | 固定IPアドレス |
| port.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>有効化の場合、セキュリティグループ、許可アドレスのペアを設定可能 |
| port.security_groups | Body | Array | ポートに設定されたセキュリティグループIDリスト |
| port.allowed_address_pairs | Body | Array | ポートに設定された許可アドレスのペアリスト |


<details><summary>例</summary>
<p>

```json
{
  "port": {
    "status": "ACTIVE",
    "name": "",
    "allowed_address_pairs": [],
    "admin_state_up": true,
    "network_id": "c46ee4e8-c9fa-462e-8a3b-55b1f11dd8f8",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "extra_dhcp_opts": [],
    "binding:vnic_type": "normal",
    "device_owner": "compute:kr-pub-a",
    "mac_address": "fa:16:3e:1a:ed:34",
    "port_security_enabled": true,
    "fixed_ips": [
      {
        "subnet_id": "eb166bdf-dd99-4b02-ac8e-64be21dff2d5",
        "ip_address": "192.168.0.15"
      }
    ],
    "id": "2a83bc25-ed76-4a2b-83b7-a4408aa99c4a",
    "security_groups": [
      "877b092c-2a31-4509-8d23-deeb02d95633"
    ],
    "device_id": "10b9643d-9bc8-4b0f-a5dd-cfcb4033d70b"
  }
}
```

</p>
</details>

---

### ポートを作成する
新しいポートを作成します。作成したポートはインスタンス作成時に活用できます。
```
POST /v2.0/ports
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| port | Body | Object | O | ポート作成リクエストオブジェクト |
| port.name | Body | String | - | ポート名 |
| port.network_id | Body | UUID | O | ポートのネットワークID |
| port.admin_state_up | Body | Boolean | - | ポートの管理者制御状態。デフォルト値`true` |
| port.fixed_ips | Body | Array | - | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | - | 固定IPを割り当てるサブネットID |
| port.fixed_ips.ip_address | Body | String | - | 固定IPアドレス |
| port.port_security_enabled | Body | Boolean | - | ポートセキュリティを使用するかどうか。デフォルト値`true` | 
| port.security_groups | Body | Array | - | ポートに設定するセキュリティグループIDリスト。デフォルト値`defaultセキュリティグループのID`<br>ポートセキュリティ使用時に設定可能 |
| port.allowed_address_pairs | Body | Array | - | ポートの許可アドレスのペアリスト<br>ポートセキュリティ使用時に設定可能 |
| port.allowed_address_pairs.ip_address | Body | String | - | 許可するIPアドレス |
| port.allowed_address_pairs.mac_address | Body | String | - | 許可するMACアドレス |
| port.extra_dhcp_opts | Body | Array | - | 追加DHCPオプション |
| port.device_owner | Body | String | - | ポートを使用するリソースの種類 |
| port.device_id | Body | UUID | - | ポートを使用するリソースID。仮想IPとして使用する場合は`network:virtual_ip`に指定 |

<details><summary>例</summary>
<p>

```json
{
    "port": {
        "network_id": "a87cc70a-3e15-4acf-8205-9b711a3531b7",
        "name": "private-port",
        "admin_state_up": true
    }
}
```

</p>
</details>

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| port | Body | Array | ポート情報オブジェクト |
| port.id | Body | UUID | ポートのID |
| port.name | Body | String | ポート名 |
| port.status | Body | Enum | ポート状態<br>`ACTIVE`, `BUILD`, `DOWN`のいずれか |
| port.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| port.network_id | Body | UUID | ポートのネットワークID |
| port.tenant_id | Body | String | テナントID |
| port.extra_dhcp_opts | Body | Array | 追加DHCPオプション |
| port.binding:vnic_type | Body | String | ポートに接続されたvNICタイプ |
| port.device_owner | Body | String | ポートを使用するリソースの種類 |
| port.device_id | Body | UUID | ポートを使用するリソースID |
| port.mac_address | Body | String | ポートのMACアドレス |
| port.fixed_ips | Body | Array | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | 固定IPが属するサブネットID |
| port.fixed_ips.ip_address | Body | String | 固定IPアドレス |
| port.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>有効化の場合、セキュリティグループ、許可アドレスのペアを設定可能 |
| port.security_groups | Body | Array | ポートに設定されたセキュリティグループIDリスト |
| port.allowed_address_pairs | Body | Array | ポートに設定された許可アドレスのペアリスト |

<details><summary>例</summary>
<p>

```json
{
    "port": {
        "status": "DOWN",
        "name": "private-port",
        "allowed_address_pairs": [],
        "admin_state_up": true,
        "network_id": "a87cc70a-3e15-4acf-8205-9b711a3531b7",
        "tenant_id": "d6700c0c9ffa4f1cb322cd4a1f3906fa",
        "device_owner": "",
        "mac_address": "fa:16:3e:c9:cb:f0",
        "fixed_ips": [
            {
                "subnet_id": "a0304c3a-4f08-4c43-88af-d796509c97d2",
                "ip_address": "10.0.0.2"
            }
        ],
        "id": "65c0ee9f-d634-4522-8954-51021b570b0d",
        "security_groups": [
            "f0ac4394-7e4a-4409-9701-ba8be283dbc3"
        ],
        "device_id": ""
    }
}
```

</p>
</details>

---

### ポートを変更する
指定したポートのプロパティを変更します。
```
PUT /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| port | Body | Object | O | ポート作成リクエストオブジェクト |
| port.name | Body | String | - | ポート名 |
| port.admin_state_up | Body | Boolean | - | ポートの管理者制御状態 |
| port.fixed_ips | Body | Array | - | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | - | 固定IPを割り当てるサブネットID |
| port.fixed_ips.ip_address | Body | String | - | 固定IPアドレス |
| port.port_security_enabled | Body | Boolean | - | ポートセキュリティを使用するかどうか<br>セキュリティグループ及び許可アドレスのペアがすべて除去された後、`false`に変更可能 | 
| port.security_groups | Body | Array | - | ポートに設定するセキュリティグループIDリスト。空のリストを入力すると全て削除<br>ポートセキュリティ使用時に設定可能 |
| port.allowed_address_pairs | Body | Array | - | ポートの許可アドレスのペアリスト。空のリストを入力すると全て除去<br>ポートセキュリティ使用時に設定可能 |
| port.allowed_address_pairs.ip_address | Body | String | - | 許可するIPアドレス |
| port.allowed_address_pairs.mac_address | Body | String | - | 許可するMACアドレス |
| port.extra_dhcp_opts | Body | Array | - | 追加DHCPオプション |

<details><summary>例</summary>
<p>

```json
{
    "port": {
        "name": "private-port",
        "admin_state_up": true
    }
}
```

</p>
</details>

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| port | Body | Array | ポート情報オブジェクト |
| port.id | Body | UUID | ポートのID |
| port.name | Body | String | ポート名 |
| port.status | Body | Enum | ポート状態<br>`ACTIVE`, `BUILD`, `DOWN`のいずれか |
| port.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| port.network_id | Body | UUID | ポートのネットワークID |
| port.tenant_id | Body | String | テナントID |
| port.extra_dhcp_opts | Body | Array | 追加DHCPオプション |
| port.binding:vnic_type | Body | String | ポートに接続されたvNICタイプ |
| port.device_owner | Body | String | ポートを使用するリソースの種類 |
| port.device_id | Body | UUID | ポートを使用するリソースID |
| port.mac_address | Body | String | ポートのMACアドレス |
| port.fixed_ips | Body | Array | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | 固定IPが属するサブネットID |
| port.fixed_ips.ip_address | Body | String | 固定IPアドレス |
| port.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>有効化の場合、セキュリティグループ、許可アドレスのペアを設定可能 |
| port.security_groups | Body | Array | ポートに設定されたセキュリティグループIDリスト |
| port.allowed_address_pairs | Body | Array | ポートに設定された許可アドレスのペアリスト |


<details><summary>例</summary>
<p>

```json
{
    "port": {
        "status": "DOWN",
        "name": "private-port",
        "allowed_address_pairs": [],
        "admin_state_up": true,
        "network_id": "a87cc70a-3e15-4acf-8205-9b711a3531b7",
        "tenant_id": "d6700c0c9ffa4f1cb322cd4a1f3906fa",
        "device_owner": "",
        "mac_address": "fa:16:3e:c9:cb:f0",
        "fixed_ips": [
            {
                "subnet_id": "a0304c3a-4f08-4c43-88af-d796509c97d2",
                "ip_address": "10.0.0.2"
            }
        ],
        "id": "65c0ee9f-d634-4522-8954-51021b570b0d",
        "security_groups": [
            "f0ac4394-7e4a-4409-9701-ba8be283dbc3"
        ],
        "device_id": ""
    }
}
```

</p>
</details>

### ポートを削除する
指定したポートを削除します。
```
DELETE /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| portId | URL | UUID | O | ポートID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---
