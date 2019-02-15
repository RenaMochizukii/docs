# Reference \(2017-03-03\)

## Template structure

Below is a tree view of all possible components within an ALM Template. The following structure is not a working demo template, but rather to explain all possible key names that may contain in the template body.

```yaml
version: string
label: string
description: string
vendor: object
  aws: object
  azure: object
  gcp: object
  alicloud: object
  k5: object
configurations: array of objects
  role: string
  flag: string
  provision: object
    vpc_id: string
    availability_zone: string
    instance_type: string
    image: string
    instance_count: number
    volume_type: string
    volume_size: number
    keypair: boolean
    subnet: object
    security_group: object
    network_acl: object
    load_balancer: object
    auto_scaling: object
  container: object
    container_image: string
    container_registry_username: string
    container_registry_password: string
    container_code_dir: string
    container_code_repo: string
    container_git_reference: string
    container_git_private_key: string
    container_ports: array of numbers
    container_users: object
    container_env_vars: object
```

## version

The version of ALM Template release. This value is always **2017-03-03**.

## label

The label of the ALM Template. You can use this section to mark the labels for each of your ALM Template versions. This is useful when you update your template. This value can be empty, and you can write up to 64 characters in length.

## description

The description of the ALM Template. You can use this section to describe the purpose of the ALM Template. For example: production app stack. This value can be empty, and you can write up to 255 characters in length.

## vendor

The cloud platform vendor of which the template will be deployed to. You need to specify the vendor in every ALM Template you write, and can only specify one vendor at a time.

Valid values:

{% tabs %}
{% tab title="AWS" %}
cred_: ALM credential name created under AWS vendor_  
region_: vendor region for your deployment_

```yaml
"vendor": {
    "aws": {
      "cred": "change this to your AWS Security Key ID",
      "region": "ap-northeast-1"
    }
}
```
{% endtab %}

{% tab title="AZURE" %}
cred_: ALM credential name created under Azure vendor_  
region_: vendor region for your deployment_

```yaml
"vendor": {
    "azure": {
      "cred": "your-credential-name-in-alm",
      "region": "japaneast"
    }
}
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
cred_: ALM credential name created under Alibaba cloud vendor_  
region_: vendor region for your deployment_

```yaml
"vendor": {
    "alicloud": {
      "cred": "your-credential-name-in-alm",
      "region": "ap-northeast-1"
    }
 }
```
{% endtab %}

{% tab title="GCP" %}
cred_: ALM credential name created under AWS vendor_  
region_: vendor region for your deployment_  
project\_id_: your GCP project id_

```yaml
 "vendor": {
    "gcp": {
       "cred": "your-credential-name-in-alm",
       "region": "asia-northeast1",
       "project_id": "GCP-project-id"
      }
  }
```
{% endtab %}

{% tab title="K5" %}
_Not yet supported in this document._
{% endtab %}
{% endtabs %}

## configurations

The configurations of the stack which ALM Template is about to deploy. In the configurations section, you specify one or multiple configuration layers of your application's provision and container runtime settings. Inside each layer, there are four sections you need to specify:

### **role**

The "role" of which the stack layer defines to.

You deploy multiple layers within one ALM Template, for example two _web_ layers, one _bastion_ layer and one _database_ layer. The current available role names:

* `web`
* `bastion`

### **flag**

The unique identifier of each layer.

You must specify the _flag_ name for each configuration layer. The value must between 4 to 18 characters in length and contains only alphanumeric characters.

### provision

The infrastructure provisioning configurations.

#### vpc\_id

Reserved key name, not supported yet.

Currently, a new VPC will be created with every ALM Template execution and the default VPC has a fixed CIDR range of **10.0.0.0/16** and you cannot customize it at the moment, so when you specifying your subnets please make sure the CIDR setting for your subnets are sitting within the VPC CIDR range.

For GCP, all deployments use the region's default public network. For GCP's predefined network ranges, you can refer to this [page](https://cloud.google.com/vpc/docs/vpc#ip-ranges).

#### availability\_zone

| Type | Required | Description |
| :--- | :--- | :--- |
| string | Yes | The availability zone of which the stack deploys to. You must specify this value in your ALM Template. |

Valid values:

{% tabs %}
{% tab title="AWS" %}
```yaml
"availability_zone": "ap-northeast-1c"
```

Below are the valid availability zones for each regions on AWS.

```text
    (North Virginia)
    us-east-1a
    us-east-1b
    us-east-1c
    us-east-1d
    us-east-1e

    (Ohio)
    us-east-2a
    us-east-2b
    us-east-2c

    (North Carolina)
    us-west-1b
    us-west-1c

    (Oregon)
    us-west-2a
    us-west-2b
    us-west-2c

    (Canada)
    ca-central-1a
    ca-central-1b

    (Ireland)
    eu-west-1a
    eu-west-1b
    eu-west-1c

    (Frankfurt)
    eu-central-1a
    eu-central-1b

    (London)
    eu-west-2a
    eu-west-2b

    (Singapore)
    ap-southeast-1a
    ap-southeast-1b

    (Sydney)
    ap-southeast-2a
    ap-southeast-2b
    ap-southeast-2c

    (Seoul)
    ap-northeast-2a
    ap-northeast-2c

    (Tokyo)
    ap-northeast-1a
    ap-northeast-1c

    (Mumbai)
    ap-south-1a
    ap-south-1b

    (Sao Paulo)
    sa-east-1a
    sa-east-1b
    sa-east-1c
```
{% endtab %}

{% tab title="AZURE" %}
```yaml
"availability_zone": "ap-northeast-1c"
```

Below are the valid availability zones for Azure.

```c
AustraliaEast
AustraliaSoutheast
BrazilSouth
CanadaCentral
CanadaEast
CentralIndia
CentralUs
ChinaEast
ChinaNorth
EastAsia
EastUs
EastUs2
GermanyCentral
GermanyNortheast
JapanEast
JapanWest
KoreaCentral
KoreaSouth
NorthCentralUs
NorthEurope
SouthCentralUs
SoutheastAsia
SouthIndia
UkSouth
UkWest
WestCentralUs
WestEurope
WestIndia
WestUs
WestUs2
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
```text
"availability_zone": "ap-northeast-1"
```
{% endtab %}

{% tab title="GCP" %}
Example:

```yaml
"availability_zone": "asia-northeast1-a"
```

You can refer to this [page](https://cloud.google.com/compute/docs/regions-zones/) for supported availability zones in GCP.
{% endtab %}

{% tab title="K5" %}
```yaml
"availability_zone": "ap-northeast-1"
```
{% endtab %}
{% endtabs %}

#### instance\_type

The type of instances \(**VM**s\). If value is not provided in the template, the default values will be used for each cloud platform.

Valid values:

{% tabs %}
{% tab title="AWS" %}
Default: t2.micro

```yaml
"instance_type": "t2.micro"
```

Below are the valid instance types for AWS.

```text
    t2.nano
    t2.micro [default]
    t2.small
    t2.medium
    t2.large
    t2.xlarge
    t2.2xlarge
    m4.large
    m4.xlarge
    m4.2xlarge
    m4.4xlarge
    m4.10xlarge
    m4.16xlarge
    m3.medium
    m3.large
    m3.xlarge
    m3.2xlarge
    c5.large
    c5.xlarge
    c5.2xlarge
    c5.4xlarge
    c5.9xlarge
    c5.18xlarge
    c4.large
    c4.xlarge
    c4.2xlarge
    c4.4xlarge
    c4.8xlarge
    c3.large
    c3.xlarge
    c3.2xlarge
    c3.4xlarge
    c3.8xlarge
    x1.16large
    x1.32xlarge
    x1e.xlarge
    x1e.2xlarge
    x1e.4xlarge
    x1e.8xlarge
    x1e.16xlarge
    x1e.32xlarge
    r4.large
    r4.xlarge
    r4.2xlarge
    r4.4xlarge
    r4.8xlarge
    r4.16xlarge
    r3.large
    r3.xlarge
    r3.2xlarge
    r3.4xlarge
    r3.8xlarge
    p3.2xlarge
    p3.8xlarge
    p3.16xlarge
    p2.xlarge
    p2.8xlarge
    p2.16xlarge
    g3.4xlarge
    g3.8xlarge
    g3.16xlarge
    f1.2xlarge
    f1.16xlarge
    i3.large
    i3.xlarge
    i3.2xlarge
    i3.4xlarge
    i3.8xlarge
    i3.16large
    d2.xlarge
    d2.2xlarge
    d2.4xlarge
    d2.8xlarge
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
Default: xn4.small.

```yaml
    "instance_type": "xn4.small"
```
{% endtab %}

{% tab title="AZURE" %}
Default: Standard\_B1s.

```yaml
    "instance_type": "Standard_B1s"
```
{% endtab %}

{% tab title="GCP" %}
Default type: `n1-standard-1`

Refer to this [page](https://cloud.google.com/compute/docs/machine-types) for information on GCP machine types.
{% endtab %}

{% tab title="K5" %}
Default: 1101.

```yaml
    "instance_type": "1101"
```
{% endtab %}
{% endtabs %}

#### image

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The machine image \(id\) used to launch the instance. |

Default values will be applied for each cloud platform \(see below for default values\). However, you can also specify this value for launching with a customized machine image.

Note: If you specify this value and also specify the [container](https://docs.mobingi.com/mobingi-alm/alm-template/reference-2017-03-03#container) section in the ALM Template to take the advantage of application lifecycle management, you need to make sure the base machine image operating system supports ALM-agent installation. For supported operating systems by ALM-agent, please refer to [this guide](https://docs.mobingi.com/mobingi-alm/alm-agent/getting-started#prerequisites).

Valid values:

{% tabs %}
{% tab title="AWS" %}
Below is also the default value for AWS.

```yaml
    "image": "ami-2a69be4c"
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
Below is also the default value for Alibaba Cloud.

```yaml
    "image": "centos_7_03_64_40G_alibase_20170710.vhd"
```
{% endtab %}

{% tab title="Azure" %}
This is not supported yet.
{% endtab %}

{% tab title="GCP" %}
This is not supported yet.
{% endtab %}

{% tab title="K5" %}
Below is also the default value for K5.

```yaml
    "image": "58fd966f-b055-4cd0-9012-cf6af7a4c32b"
```
{% endtab %}
{% endtabs %}

#### instance\_count

| Type | Required | Description |
| :--- | :--- | :--- |
| int | no | Number of instances \(VMs\) to provision. This is only applicable to non-autoscaled stacks.For autoscaled stacks, see `auto_scaling` section. |

If you don't specify this declarative, the default value of _1_ will be applied.

Valid values:

{% tabs %}
{% tab title="AWS" %}
```yaml
    "instance_count": 1
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
```yaml
    "instance_count": 1
```
{% endtab %}

{% tab title="AZURE" %}
```yaml
    "instance_count": 1
```
{% endtab %}

{% tab title="GCP" %}
This value is only valid when &gt; zero **AND** `auto_scaling.min` and `auto_scaling.max` are both set to zero. See [auto\_scaling](https://docs.mobingi.com/mobingi-alm/alm-template/alm-template-reference#auto_scaling) section for more information about autoscaling.
{% endtab %}

{% tab title="K5" %}
```yaml
    "instance_count": 1
```
{% endtab %}
{% endtabs %}

#### volume\_type

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The volume type of instance. |

Valid values:

{% tabs %}
{% tab title="AWS" %}
If you don't specify this declarative, the default value of gp2 will be applied.

```yaml
    gp2 - General Purpose SSD
    io1 - Provisioned IOPS SSD
    st1 - Throughput Optimized HDD
    sc1 - Cold HDD
    standard - Magnetic volumes
```
{% endtab %}

{% tab title="Azure" %}
This is not supported yet.
{% endtab %}

{% tab title="GCP" %}
This is not supported yet.
{% endtab %}
{% endtabs %}

#### volume\_size

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The size of the volume, in gibibytes \(GiBs\). |

Valid values:

{% tabs %}
{% tab title="AWS" %}
```text
    Constraints: 1-16384 for gp2, 4-16384 for io1, 500-16384 for st1, 500-16384 for sc1, and 1-1024 for standard (magnetic disk).

    Default volume size in GB for each volume types:

    - gp2: 50
    - io1: 50
    - st1: 500
    - sc1: 500
    - standard: 50
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
This section hasn't been covered by documentation.
{% endtab %}

{% tab title="AZURE" %}
Current initial deployment size is _50GB_.

{% hint style="info" %}
**Note:** Soon to support other sizes.
{% endhint %}
{% endtab %}

{% tab title="GCP" %}
Default value is 50GB.
{% endtab %}

{% tab title="K5" %}
This section hasn't been covered by documentation.
{% endtab %}
{% endtabs %}

#### keypair

The ssh key pair used to access instances.

| Type | Required | Description |
| :--- | :--- | :--- |
| boolean | no | Enable SSH key pair to be used to access instances. Default value is **true**. |

#### subnet

| Type | Required | Description |
| :--- | :--- | :--- |
| object | no | The subnet settings. |

A _subnet_ section contains 3 key names.

* `cidr` \(string\)

  The IPv4 CIDR block that you want the subnet to cover \(for example, "10.0.0.0/24"\).

  _Required:_ No

  If you don't specify this declarative, the default CIDR block settings will be applied. See below for default values on each cloud platform.

* `public` \(boolean\)

  Defines whether this subnet is public or private. This value is either _true_ or _false_.

  If you don't specify this declarative, the default value of _true_ will be applied.

  _Required:_ No

* `auto_assign_public_ip` \(boolean\)

  Defines whether automatically assigns a public IP when instance launched into the subnet. This value is either _true_ or _false_.

  _Required:_ No

  If you don't specify this declarative, the default value of _true_ will be applied.

  If you set `public` as _false_, then this declarative will be ignored.

Valid values:

{% tabs %}
{% tab title="AWS" %}
Example below is also the default settings when deploying to AWS.

```yaml
"subnet": {
  "cidr": "10.0.0.0/24",
  "public": true,
  "auto_assign_public_ip": true
}
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
Example below is also the default settings when deploying to Alibaba Cloud.

```yaml
"subnet": {
  "cidr": "192.168.199.0/24",
  "public": true,
  "auto_assign_public_ip": true
}
```
{% endtab %}

{% tab title="AZURE" %}
Example below is also the default settings when deploying to Azure.

```yaml
"subnet": {
  "cidr": "10.0.0.0/16",
  "public": true,
  "auto_assign_public_ip": true
}
```
{% endtab %}

{% tab title="GCP" %}
This is not supported yet.
{% endtab %}

{% tab title="K5" %}
Example below is also the default settings when deploying to K5.

```text
"subnet": {
  "cidr": "10.1.0.0/24",
  "public": true,
  "auto_assign_public_ip": true
}
```
{% endtab %}
{% endtabs %}

#### network\_acl

The network action control list for a virtual private cloud. **This section supports AWS only.**

| Type | Required | Description |
| :--- | :--- | :--- |
| array | no | The network action control list for a virtual private cloud. **This section supports AWS only.** |

A _network\_acl_ section contains a list of network\_acl entry items. Each item contains the following declaratives:

* `rule_number` \(int\)

  Rule number to assign to the entry, such as 100. ACL entries are processed in ascending order by rule number. Entries can't use the same rule number unless one is an egress rule and the other is an ingress rule.

  _Required:_ Yes

  For more on valid values, please refer to [this guide](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateNetworkAclEntry.html) on aws documentation site.

* `protocol` \(string\)

  The IP protocol that the rule applies to. You must specify -1 or a protocol number. You can specify -1 for all protocols.

  _Required:_ Yes

  For more on protocol numbers, please refer to [this guide](http://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml).

* `rule_action` \(string\)

  Whether to allow or deny traffic that matches the rule; valid values are "allow" or "deny".

  _Required:_ Yes

* `acl_egress` \(boolean\)

  Whether this rule applies to egress traffic from the subnet \(true\) or ingress traffic to the subnet \(false\).

  _Required:_ No

  If you don't specify this declarative, the default value of _false_ will be applied.

* `cidr_block` \(string\)

  The IPv4 CIDR range to allow or deny, in CIDR notation \(e.g., 172.16.0.0/24\).

  _Required:_ Yes

For more information about network acl please refer to [AWS Documentation](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html)

Valid values:

{% tabs %}
{% tab title="AWS" %}
Example below is also the default settings when deploying to AWS.

```yaml
"network_acl": [
  {
    "rule_number": 100,
    "protocol": "-1",
    "rule_action": "allow",
    "cidr_block": "0.0.0.0/0"
  },
  {
    "rule_number": 100,
    "protocol": "-1",
    "rule_action": "allow",
    "acl_egress": false,
    "cidr_block": "0.0.0.0/0"
  }
]
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
This section hasn't been covered by documentation.
{% endtab %}

{% tab title="AZURE" %}
This is not supported yet.
{% endtab %}

{% tab title="GCP" %}
This is not supported yet.
{% endtab %}

{% tab title="K5" %}
This section hasn't been covered by documentation.
{% endtab %}
{% endtabs %}

#### security\_group

| Type | Required | Description |
| :--- | :--- | :--- |
| object | no | The security groups for your virtual private cloud. Security groups are associated with network interfaces and acts as a virtual firewall that controls the traffic for one or more instances. |

A _security group_ section contains two entry items, `ingress` and `egress`.

* ingress \(array\)
* egress \(array\)

Each _ingress_ or _egress_ contains the following 4 declarative:

* `cidr_ip` \(string\)

  An IPv4 CIDR range.

  _Required:_ Yes

  \(For more information on cidr calculations, there are tools like [this](http://www.subnet-calculator.com/cidr.php) to helping you get started.\)

* `from_port` \(int\)

  Start of port range for the TCP and UDP protocols, or an ICMP type number. If you specify icmp for the IpProtocol property, you can specify -1 as a wildcard \(i.e., any ICMP type number\).

  _Required:_ Yes

* `ip_protocol` \(string\)

  IP protocol name or number.

  _Required:_ Yes

* `to_port` \(int\)

  End of port range for the TCP and UDP protocols, or an ICMP code. If you specify icmp for the IpProtocol property, you can specify -1 as a wildcard \(i.e., any ICMP code\).

  _Required:_ Yes

Valid values:

{% tabs %}
{% tab title="AWS" %}
Example below is also the default settings when deploying to AWS.

```yaml
"security_group": {
        "ingress": [
            {
                "cidr_ip": "0.0.0.0/0",
                "from_port": 80,
                "ip_protocol": "tcp",
                "to_port": 80
            },
            {
                "cidr_ip": "0.0.0.0/0",
                "from_port": 443,
                "ip_protocol": "tcp",
                "to_port": 443
            },
            {
                "cidr_ip": "0.0.0.0/0",
                "from_port": 22,
                "ip_protocol": "tcp",
                "to_port": 22
            }
        ],
        "egress": [
            {
                "cidr_ip": "0.0.0.0/0",
                "from_port": 0,
                "ip_protocol": "-1",
                "to_port": 65535
            }
        ]
    }
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
This section hasn't been covered by documentation.
{% endtab %}

{% tab title="AZURE" %}
```yaml
"security_group": {
    "ingress": [
        {
            "cidr_ip": "0.0.0.0/0",
            "from_port": 80,
            "ip_protocol": "tcp",
                   "to_port": 80
        },
        {
            "cidr_ip": "0.0.0.0/0",
            "from_port": 443,
               "ip_protocol": "tcp",
            "to_port": 443
        },
        {
            "cidr_ip": "0.0.0.0/0",
            "from_port": 22,
            "ip_protocol": "tcp",
            "to_port": 22
        }
    ],
       "egress": [
        {
            cidr_ip": "0.0.0.0/0",
            "from_port": 0,
               "ip_protocol": "tcp",
            "to_port": 65535
        }
    ]
}
```
{% endtab %}

{% tab title="GCP" %}
For GCP deployments, the default network where the stack is deployed will, by default, open ports **22** and **80** from source **0.0.0.0/0**.
{% endtab %}

{% tab title="K5" %}
This section hasn't been covered by documentation.
{% endtab %}
{% endtabs %}

#### auto\_scaling

| Type | Required | Description |
| :--- | :--- | :--- |
| object, or string | no | The `auto_scaling` group defines the configuration to automatically scale up or down the number of compute resources that are being allocated to your application based on its needs at any given time. |

If you are deploying an _application_ type of load balancer \(you define in [`load_balancer`](reference-2017-03-03.md#load_balancer) declarative\), you can omit the required parameters below and use the ATL function `#share` to share the same auto scaling group which you defined in another layer. [Click here](https://docs.mobingi.com/mobingi-alm/alm-template/alm-template-language) for more on ATL functions.

{% hint style="warning" %}
[**ATL**](https://docs.mobingi.com/mobingi-alm/alm-template/alm-template-language) is only supported by AWS at the moment.
{% endhint %}

The `auto_scaling` section contains the following declarative:

* `min` \(int\)

  The minimum size of the whole \(total\) autoscaling group.

  Required_:_ No

  If you don't specify this declarative, the default value of 1 will be applied.

* `max` \(int\)

  The maximum size of the whole \(total\) autoscaling group.

  Required: No

  If you don't specify this declarative, the default value of 1 will be applied.

* `spot_to_ondemand_ratio` \(int\) The percentage of min/max values to deploy as spot/preemptible/low-priority instances. For example, if you have 10 as `min` and 100 as `max`, a `spot_to_ondemand_ratio` value of 60 \(or 60 percent\) will have the following results: Spot min: 6 \(60% of 10\) Spot max: 60 \(60% of 100\) On-demand min: 4 On-demand max: 40 For calculations that results to fractions, like 50% of 3, the results will be rounded off to the nearest integer such as `1.5 = 2`, `3.9 = 4`, `2.3 = 2`, etc. For example, if you have a `min` value of 3 and 50 as `spot_to_ondemand_ratio`, you will have a minimum of 2 spot instances \(50% of 3 is 1.5, rounded off to 2\) and a single on-demand instance.
* `spot_min` \(int\) **\[DEPRECATED\]**

  The minimum size of the spot instances in the autoscaling group. This key will still work with AWS- and GCP-based stacks. For other cloud vendors, please use `spot_to_ondemand_ratio` key.

  Required: No

* `spot_max` \(int\) **\[DEPRECATED\]**

  The maximum size of the spot instances in the autoscaling group. This key will still work with AWS- and GCP-based stacks. For other cloud vendors, please use `spot_to_ondemand_ratio` key.

  Required: No

* `availability_zones` \(string\)

  The list of availability zones for the Auto Scaling group.

  Required: Yes

* `cooldown` \(string\)

  The number of seconds after a scaling activity is completed before any further scaling activities can start.

  Required: No

  If you don't specify this declarative, the default value of _360_ will be applied.

* `healthcheck_grace_period` \(string\)

  The length of time in seconds after a new instance comes into service that Auto Scaling starts checking its health.

  Required: No

  If you don't specify this declarative, the default value of _360_ will be applied.

Valid values:

{% tabs %}
{% tab title="AWS" %}
```yaml
"auto_scaling": {
  "min": 1,
  "max": 1,
  "availability_zones": "${use(FlagName1.provision.availability_zone, FlagName2.provision.availability_zone)}",
  "cooldown": "360",
  "healthcheck_grace_period": "360"
}
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
This section hasn't been covered by documentation.
{% endtab %}

{% tab title="AZURE" %}
```yaml
"auto_scaling":{
    "min": 3,
    "max":3,
    "spot_to_ondemand_ratio": 50
},
```
{% endtab %}

{% tab title="GCP" %}
For GCP, the system will check if `min` is &gt; 0, `max` is &gt; `min`, and `instance_type` is zero before proceeding.

For [preemptible](https://cloud.google.com/compute/docs/instances/preemptible) instances, the system will calculate the values based on the provided `spot_to_ondemand_ratio` value. For compatibility with the deprecated `spot_min` and `spot_max` keys, it will also check if `spot_min` &gt; 0, `spot_max` &gt; `spot_min` and `instance_type` is zero before proceeding.
{% endtab %}

{% tab title="K5" %}
This section hasn't been covered by documentation.
{% endtab %}
{% endtabs %}

#### load\_balancer

| Type | Required | Description |
| :--- | :--- | :--- |
| object | no | The `load_balancer` for the `auto_scaling` group. |

The _load balancer_ section contains the following declarative:

* `lb_type` \(string\)

  Either _classic_, _application_ or _networking_.

  In usual case, ALM creates the classic \(standard\) load balancer.

  On AWS specifically, you can also create the _application_ load balancer and a _networking_ load balancer. For more on these two AWS load balancer types, please refer to [this guide](https://aws.amazon.com/elasticloadbalancing).

  _Required:_ No

  If you don't specify this declarative, the default value of _classic_ will be applied.

* `scheme` \(string\)

  Either _internet-facing_ or _internal_.

  Specify _internal_ to create an internal load balancer with a DNS name that resolves to private IP addresses or _internet-facing_ to create a load balancer with a publicly resolvable DNS name, which resolves to public IP addresses.

  _Required:_ No

  If you don't specify this declarative, the default value of _internet-facing_ will be applied.

* `security_groups` \(string\)

  Specifies a list of security groups assigned to this load balancer.

  _Required:_ No

  **Note:** This value is in string type and can only be the reference to [`security_group`](reference-2017-03-03.md#security_group) section you defined in same or other layers. For example if you have two layers of configurations named "Layer1" and "Layer2", you can reference the _security group_ defined in Layer2 to Layer1 by writing:

  ```javascript
    { "security_groups": "#ref(Layer2.provision.security_group)"}
  ```

* `subnets` \(string\)

  Specifies a list of subnets assigned to this load balancer defined in `lb_type` parameter above.

  _Required:_ No

  **Note:** Only specify this value if you are deploying an ALB type of load-balancer.

  **Note:** This value is in string type and can only be the reference to [`subnet`](reference-2017-03-03.md#subnet) section you defined in same or other layers. For example if you have two layers of configurations named "Layer1" and "Layer2", you can reference the _subnets_ to use both of them by:

  ```javascript
    { "subnets": "#ref(Layer2.provision.subnet,Layer1.provision.subnet)"}
  ```

* `listeners` \(array\)

  One or more listeners for this load balancer. Each listener must be registered for a specific port, and you cannot have more than one listener for a given port.

  _Required:_ Yes. _You must specify at least one entry item for this section._

  A _listeners_ section contains a list of entry items with each item contains the following declarative:

  * `load_balancer_port` \(string\)

    Specifies the external load balancer port number.

    _Required:_ Yes

  * `protocol` \(string\)

    Specifies the load balancer transport protocol to use for routing: _HTTP_, _HTTPS_, _TCP_ or _SSL_.

    _Required:_ Yes

  * `instance_port` \(string\)

    Specifies the TCP port on which the instance server listens.

    _Required:_ Yes

  * `instance_protocol` \(string\)

    Specifies the protocol to use for routing traffic to back-end instances: _HTTP_, _HTTPS_, _TCP_ or _SSL_. You can't modify this property during the life of the load balancer.

    _Required:_ No

    **Note:**

    * If the front-end protocol is HTTP or HTTPS, _instance protocol_ must be on the same protocol layer \(HTTP or HTTPS\). Likewise, if the front-end protocol is TCP or SSL, _instance protocol_ must be TCP or SSL.
    * If there is another Listener with the same _instance port_ whose _instance protocol_ is secure, \(using HTTPS or SSL\), the _instance protocol_ of the Listener must be secure \(using HTTPS or SSL\). If there is another Listener with the same _instance port_ whose _instance protocol_ is HTTP or TCP, the _instance protocol_ of the Listener must be either HTTP or TCP.

  * `cert_domain` \(string\)

    **\(AWS Only\)**

    Specifies the domain name to be used for issuing SSL certificate via AWS ACM service.

    _Required:_ No

    When you provide the domain name here, a verification email will be sent to the domain owner \(sent by AWS\), and you have to click link found in email to approve the issuance of the SSL certificate before the stack provision can be finished.

    Once the provision deployment is successfully done, the SSL certificate will be attached to your load-balancer automatically.

* `health_check` \(object\)

  **\(AWS Only\)**

  Application health check for the instances.

  _Required:_ No

  A _health check_ section contains the following declarative:

  * `healthy_threshold` \(string\)

    Specifies the number of consecutive health probe successes required before moving the instance to the Healthy state.

    _Required:_ Yes

  * `interval` \(string\)

    Specifies the approximate interval, in seconds, between health checks of an individual instance. Valid values are 5 to 300.

    _Required:_ Yes

  * `target` \(string\)

    Specifies the instance's protocol and port to check. The protocol can be TCP, HTTP, HTTPS, or SSL. The range of valid ports is 1 through 65535.

    _Required:_ Yes

  * `timeout` \(string\)

    Specifies the amount of time, in seconds, during which no response means a failed health probe. This value must be less than the value for _interval_.

    _Required:_ No

  * `unhealthy_threshold` \(string\)

    Specifies the number of consecutive health probe failures required before moving the instance to the Unhealthy state.

    _Required:_ No

Valid values:

{% tabs %}
{% tab title="AWS" %}
```yaml
"load_balancer": {
        "lb_type": "classic",
        "scheme": "internet-facing",
        "listeners": [
            {
                "load_balancer_port": "443",
                "instance_port": "80",
                "protocol": "HTTPS",
                "cert_domain": "www.example.com"
            }
        ],
        "health_check": {
            "healthy_threshold": "2",
            "interval": "10",
            "target": "TCP:80",
            "timeout": "5",
            "unhealthy_threshold": "10"
        }
    }
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
This section hasn't been covered by documentation.
{% endtab %}

{% tab title="GCP" %}
For now, GCP only supports the classic load balancer. It uses `listeners.load_balancer_port` and `health_check.target` keys for configuration and always creates an internet-facing load balancer.
{% endtab %}

{% tab title="K5" %}
This section hasn't been covered by documentation.
{% endtab %}
{% endtabs %}

#### rds

| Type | Required | Description |
| :--- | :--- | :--- |
| object | no | The `rds` is a managed relational database service. |

The _rds_ section contains the following declarative:

* `db_instance_type` \(string\)

  If you don't specify this declarative, the default values will be applied for each cloud platform.

  _Required:_ Yes

* `engine` \(string\)

  If you don't specify this declarative, the default values will be applied for each cloud platform.

  _Required:_ Yes

* `version` \(string\)

  If you don't specify this declarative, the default values will be applied for each cloud platform.

  _Required:_ Yes

* `storage` \(int\)

  Constraints: 5-3072 \(managemet disk\).

  _Required:_ Yes

* `multi_az` \(boolean\)

  Create replica in an availability zone different from the DB instance. Defines whether this nulti\_az is AZ or No AZ. This value is either true or false.

  _Required:_ Yes

* `replica` \(int\)

  Constraints: 1-5 \(replica counts\).

  _Required:_ No

Valid values:

{% tabs %}
{% tab title="AWS" %}
Default: db.t2.micro

```yaml
 "rds": {
        "db_instance_type": "db.t2.micro",
        "engine": "mysql",
        "version": "5.6"
        "storage": 10,
        "multi_az": true,
        "replica": 1
    }
```

Below are the valid db instance types for AWS.

```text
    db.t2.micro [default]
    db.t2.small
    db.t2.medium
    db.t2.large
    db.t2.xlarge
    db.t2.2xlarge
    db.m4.large
    db.m4.xlarge
    db.m4.2xlarge
    db.m4.4xlarge
    db.m4.10xlarge
    db.m4.16xlarge
    db.m3.medium
    db.m3.large
    db.m3.xlarge
    db.m3.2xlarge
    db.r3.large
    db.r3.xlarge
    db.r3.2xlarge
    db.r3.4xlarge
    db.r3.8xlarge
    db.r4.large
    db.r4.xlarge
    db.r4.2xlarge
    db.r4.4xlarge
    db.r4.8xlarge
    db.r4.16xlarge
    db.m2.xlarge
    db.m2.2xlarge
    db.m2.4xlarge
    db.m1.small
    db.m1.medium
    db.m1.large
    db.m1.xlarge
```

Below are the valid engine and version for AWS.

```text
    mysql
        - 5.6
        - 5.7
    postgres
        - 9.6
        - 10
```
{% endtab %}

{% tab title="ALIBABA CLOUD" %}
This section hasn't been covered by documentation.
{% endtab %}

{% tab title="AZURE" %}
This section hasn't been covered by documentation
{% endtab %}

{% tab title="GCP" %}
This is not supported yet.
{% endtab %}

{% tab title="K5" %}
This section hasn't been covered by documentation
{% endtab %}
{% endtabs %}

### container

The software runtime configurations _\(defined as docker images\)_ and code deployment requirement for each instance node.

#### container\_image

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The docker image to be used to launch base container. |

Example:

| `registry.mobingi.com/tompson/ubuntu-nginx-php:latest` |
| :--- |


#### container\_registry\_username

The docker image registry's username if this is a private image repository.

| Type | Example Value | Required | Supported Platforms |
| :--- | :--- | :--- | :--- |
| string | N/A | No | Platform Stateless |

#### container\_registry\_password

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The docker image registry's password if this is a private image repository. |

#### container\_code\_dir

| Type | Required | Supported Platforms |
| :--- | :--- | :--- |
| string | no | The directory of the instance where application code will be deployed to. |

Example:

| `/var/www/html` |
| :--- |


#### container\_git\_repo

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The git repository url of where application code is hosted. |

Example:

| `https://github.com/mobingilabs/default-site-php.git` |
| :--- |


#### container\_git\_reference

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The git repository branch of where application code is hosted. |

Example:

```text
master
```

#### container\_git\_private\_key

| Type | Required | Description |
| :--- | :--- | :--- |
| string | no | The private key of the git repository if it is a private repository. |

#### container\_ports

| Type | Required | Description |
| :--- | :--- | :--- |
| array | no | The ports for connection to be used by containers. |

Example:

| `[ 80 ]` |
| :--- |


#### container\_users

This parameter is not yet supported.

#### container\_env\_vars

| Type | Required | Description |
| :--- | :--- | :--- |
| object | no | The environment variables to be defined for the container operating system. |

An example `container` configuration:

```yaml
"container": {
  "container_image": "mobingi/ubuntu-apache2-php7",
  "container_code_dir": "/var/www/html",
  "container_git_repo": "https://github.com/mobingilabs/default-site-php.git",
  "container_git_reference": "master",
  "container_ports": [80],
  "container_env_vars": {
    "display_errors": "Off",
    "my_variable": "Some value"
  }
}
```

