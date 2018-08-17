# Google Cloud Platform

This  prescribed manner in which DEL projects should construct and consume GCP elements for consumption in demonstrators.

## GCP Provisioning Sequence

Mermaid code goes here

## GCP Naming & Tags

To assist managing GCP resources used by the DEL the following naming and tagging scheme must be adhered to:

* All entities shall be named in the form DEL-XXX-YYYYYYY where
  * DEL = Organization
  * XXX = Element type, one of:
    * **VPC** for Virtual Private Cloud
    * **IGW** for Internet Gateways
    * **SBN** for Subnet
    * **SGP** for Security Groups
    * **FWR** for Firewall Rules
    * **RTE** for Routes
    * **ENG** for Google Compute Engine
    * **EIP** for External IP Addresses (Public Facing)
    * **VNP** for Virtual Network Peering
   * YYYYYYYY is a description or unique ID - for GCP instances use the first 3 characters for the OS e.g. WIN, LNX, RHL (suggest eight characters max for YYYYYYYY)
   * Four Tags are required on all entities to allow AWS resource consumption to be identified
        * Name	the value generated above if not already provided
        * Organisation	always DEL
        * Project	the related DEL project or initiative
        * Owner the owning DEL team member in the form firstname-lastname

## Networking


### Virtual Private Cloud

To provide a home for your resources a Virtual Private Cloud (VPC) must be created.  Each project/demo/initiative that requires GCP resources can create its own VPC.

Accessed under Navigation Menu Services, Networking, VPC network, VPC networks

Click on the **CREATE VPC NETWORK** button

In the console, specify:

**VPC**

| Field         | Value                        |
|:------------- |:---------------------------- |
| Name          | del-vpc-yyyyyyyy             |
| Description   | Description of demo/project  |

**Subnet**

Within the VPC, resources need to be allocated to subnets Projects may require multiple subnets.

Set the Subnet creation mode to **Custom** and enter the following values:

| Field                   | Value                                                                                                  |
| :-----------------------|:-------------------------------------------------------------------------------------------------------|
| Name                    | del-vpc-yyyyyyyy                                                                                       |
| Description             | Description of demo/project                                                                            |
| Region                  | Use the [**europe_west2**](https://cloud.google.com/compute/docs/regions-zones/) region for London, UK |
| IP Address range        | IP Address range in CIDR notation  e.g 10.0.1.0/24                                                     |
| Private Google access   | Only **On** if Compute Engine will not have External IP Address                                        |
| Flow logs               | **Off** as some some systems generate a large number of logs, which can increase costs in Stackdriver  |

You can add additional Subnets if required by clicking the **Add subnet** button.

Set Dynamic routing model to use the **Regional** option by default unless you plan on configuing a Hybrid cloud, in which case use the **Global** option.

Click **Create**.

### External IP addresses

Accessed under Navigation Menu Services, Networking, VPC network, External IP addresses.

Click on the **RESERVE STATIC ADDRESS** button



| Field                 | Value                                         |
|:----------------------|:----------------------------------------------|
| Name                  | del-eip-yyyyyyyy                              |
| Description           | Description of demo/project                   |
| Network Service Tier  | The **Premium** option must be chosen as **Standard** is not currenlty available in the europe-west2 region. |
| IP Service            | IPv4                                         |
| Type                  | Use **Global**, as **Regional** will only allow 1 static external address per region                         |
| Region                | Description of demo/project                  |
| Attached to           | Select **None**, we will assign this later   |

Click **Reserve**.

If succesfully reserved you will see a list of all External IP Addresses that have been requested. 

Clicking on an external IP address in the list will allow you to add abels to that entry. The suggested lables are:

| Key           | Value                         |
|:------------- |:------------------------------|
| Organization  | DEL                           |
| Project       | Description of demo/project   |
| Owner         | The project owner             |

### Firewall Rules

To open up external ports within a VPC, an entry to the Firewall rules needs to be created.

Accessed under Navigation Menu Services, Networking, VPC network, Firewall Rules.

Click **CREATE FIREWALL RULE**

In the console, specify:

| Field         | Value                        |
|:------------- |:---------------------------- |
| Name          | del-fwr-yyyyyyyy             |
| Description   | Description of demo/project  |
| Network   | Name of the VPC that was just created |
| Priority   | Used to specify the order a rule will be appied. Can be 0 - 65535, where the lower the number, the higher the priority |
| Direction of traffic   | **Ingress** for data entering the VPC, **Egress** for data leaving the VPC  |
| Action on match   | Specifies whether rule will **Allow** (whitelisted) or **Block** (blacklisted) traffic    |
| Targets   |  All instances in the network |
| Source filter (*ingress only*)  | Subnets  |
| Source IP ranges (*ingress only*)  | To allow/block all IP addresses use **0.0.0.0/0** (less secure), for more granular rules, enter in specific IP Address or IP Address range (in CIDR notation)  |
| Second source filter   | None (unless further granularity is required)  |
| Protocols and ports   | Specified protocols and ports (standard ports & protocals below), then delimit with a semi-colon (**;**) - e.g. *Protocol1:Port1; Port2:Protocol2 Prot...* |
| Enforcement  | Enabled  |

The standard protocols and ports that should be allowed for each VPC are:
 * TCP:3389  for RDP
 * TCP:22   for SSH
 * TCP:80   for HTTP
 * TCP 443  for HTTPS
 
### Routes

Routes are used to configure the default gateway of for resources connected to the VPC to allow them to reach:
 * The internet
 * Other resources in the VPC
 
The two routes are created by default when a new VPC is created, therefore no additional action is required. There is the option to create additional routs if this is indeed required.

Routes accessed under Navigation Menu Services, Networking, VPC network, Routes.

You can validate they have been provisioned correctly by finding the two entries with the **Network** value equal to the recently created network. Confirm the **Destination IP Ranges** value for the VPC network match the IP Address range Subnet that was created, and that the Default GAteway is set to 0.0.0.0/0.

### VPC Network Peering 

*Note: This is an optional step.*

Cloud VPC Network Peering allows you to privately connect two VPC networks, which can reduce latency, cost and increase security. Note, that there should be no overlap in the subnets of the networks being peered.

VPC Network Peering accessed under Navigation Menu Services, Networking, VPC network, VPC network peering.

Click **Create connection**, then click **Continue**
The project ID (if you are connecting to a VPC network in another project)
The name of the VPC network that you want to peer with
Note: The subnet IP ranges in peered VPC networks cannot overlap.

| Field                | Value                       |
|:-------------------- |:----------------------------|
| Name                 | del-vnp-xxxxxxxx            |
| Your VPC network     | del-vpc-xxxxxxxx            |
| Peered VPC network   | Select the current project  |
| VPC network name     | Name of another VPC         |

Click **Create**.

### Shared VPC

Shared VPC is only available for projects within an organisation and is therefore not available in the GCP free tier.

## Cloud Engine

Cloud Engine can be accessed under Navigation Menu, Compute, Cloud Engine.

### VM Instances

Click **Create Instance**

Enter the following info:

| Field                | Value                       |
|:-------------------- |:----------------------------|
| Name                 | del-eng-xxxxxxxx            |
| Region               | europe-west2            |
| Zone                 | europe-west2-a/b/c depending on site resiliancy requirements |
| Machine type         | Select appropriate hardware resources for desired workload  |
| Container            | Select if instance will host containers        |
| Boot disk            | Select the OS of the VM instance         |
| Boot disk type       | SSD persistent disk        |
| Boot disk size (GB)  | Will vary beteen OS - see OS vendor recomendation for boot partition capacity         |
| Service Account      | Compute Engine default service account         |
| Access scopes        | Allow default access         |
| Firewall             | Always select **Allow HTTPS traffic**, only select **Allow HTTP traffic** if essential        |

Click **Management, security, disks, networking, sole tenancy** to perform advanced configuration.

### Management

| Field                | Value                       |
|:-------------------- |:----------------------------|
| Description                | Description of demo/project           |
| Labels               | Add lable for Organization, Project and Owner       |
| Enable deletion protection                 | europe-west2-a/b/c depending on site resiliancy requirements |
| Metadata         | Add lable for Organization, Project and Owner    |
| Preemptibility            | Select if instance will host containers        |
| Automatic restart            | Select the OS of the VM instance         |
| On host maintenence      | SSD persistent disk        |


## Security 

## Disks, 

## Networking, 

## Sole Tenancy
