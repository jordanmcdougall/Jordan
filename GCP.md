# Google Cloud Platform

This prescribed manner in which DEL projects should construct and consume GCP elements for consumption in demonstrators.

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
    * **EIP** for External IP Addresses (Public)
    * **IIP** for Internal IP Addresses (Private)
    * **VNP** for Virtual Network Peering
    * **DSK** for Virtual Hard Disk
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
| Flow logs               | **Off** as some systems generate a large number of logs, which can increase costs in Stackdriver  |

You can add additional Subnets if required by clicking the **Add subnet** button.

Set Dynamic routing model to use the **Regional** option by default unless you plan on configuring a Hybrid cloud, in which case use the **Global** option.

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

If successfully reserved you will see a list of all External IP Addresses that have been requested. 

Clicking on an external IP address in the list will allow you to add labels to that entry. The suggested labels are:

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
| Priority   | Used to specify the order in which a rule will be applied. Can be 0 - 65535, where the lower the number, the higher the priority |
| Direction of traffic   | **Ingress** for data entering the VPC, **Egress** for data leaving the VPC  |
| Action on match   | Specifies whether rule will **Allow** (whitelisted) or **Block** (blacklisted) traffic    |
| Targets   |  All instances in the network |
| Source filter (*ingress only*)  | Subnets  |
| Source IP ranges (*ingress only*)  | To allow/block all IP addresses use **0.0.0.0/0** (less secure), for more granular rules, enter in specific IP Address or IP Address range (in CIDR notation)  |
| Second source filter   | None (unless further granularity is required)  |
| Protocols and ports   | Specified protocols and ports (standard ports & protocols below), then delimit with a semi-colon (**;**) - e.g. *Protocol1:Port1; Port2:Protocol2 Prot...* |
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

You can validate they have been provisioned correctly by finding the two entries with the **Network** value equal to the recently created network. Confirm the **Destination IP Ranges** value for the VPC network match the IP Address range Subnet that was created, and that the Default Gateway is set to 0.0.0.0/0.

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

| Field                | Value                                                                                   |
|:-------------------- |:----------------------------------------------------------------------------------------|
| Name                 | del-eng-xxxxxxxx                                                                        |
| Region               | europe-west2                                                                            |
| Zone                 | europe-west2-a/b/c depending on site resiliency requirements                            |
| Machine type         | Select appropriate hardware resources for desired workload                              |
| Container            | Select if instance will host containers                                                 |
| Boot disk            | Select the OS of the VM instance                                                        |
| Boot disk type       | SSD persistent disk                                                                     |
| Boot disk size (GB)  | Will vary between OS - see OS vendor recommendation for boot partition capacity         |
| Service Account      | Compute Engine default service account                                                  |
| Access scopes        | Allow default access                                                                    |
| Firewall             | Always select **Allow HTTPS traffic**, only select **Allow HTTP traffic** if essential  |

Click **Management, security, disks, networking, sole tenancy** to perform advanced configuration.

### Management

| Field                | Value                                            |
|:-------------------- |:-------------------------------------------------|
| Description          | Description of demo/project                      |
| Labels               | Add label for Organization, Project and Owner    |
|Deletion protection   | Enable deletion protection                       |
| Metadata             | Add lable for Organization, Project and Owner    |
| Preemptibility       | Off, unless VM instance is required for < 24hrs  |
| Automatic restart    | On                                               |
| On host maintenance  | Migrate VM instance                              |


## Security 

No changes required.

## Disks

| Field                | Value                                       |
|:-------------------- |:--------------------------------------------|
| Deletion rule        | Delete boot disk when interface is deleted  |
| Encryption           | Google-managed key                          |


If a new disk is required, click **Add new disk**.

*Note: It's recommended that applications and data are stored on a separate drive to the boot partition.* 

Be aware there is limited SSD capacity that is available in the GCP free tier. 

| Field                | Value                                                                                 |
|:---------------|:--------------------------------------------------------------------------------------------|
| Name           | del-dsk-xxxxxxxx                                                                            |
| Type           | SSD persistent disk for workload that require higher IOPS, Standard persistent disk if not  |
| Source type    | Blank disk                                                                                  |
| Source image   | Google-managed key                                                                          |
| Mode           | Read/Write                                                                                  |
| Deletion rule  | Keep disk                                                                                   |
| Size (GB)      | Will vary project to project                                                                |
| Encryption     | Google-managed key                                                                          |

Click **Done**



## Networking 

In the Network tags enter the VPC name.

Remove the default interface, then click **Add network interface**.

| Field                | Value                                                 |
|:-------------------- |:------------------------------------------------------|
| Network              | del-vpc-xxxxxxxx                                      |
| Subnetwork           | del-sbn-xxxxxxxx                                      |
| Primary internal IP  | Reserve static IP address                             |
| Name                 | del-iip-xxxxxxxx                                      |
| Description          | Description of demo/project                           |
| Subnetwork           | del-sbn-xxxxxxxx                                      |
| Static IP address    | X.X.X.that is unallocated and exists in subnet range  |
| External IP address  | Empheral                                              |
| Network Service Tier | Premium                                               |

Click **Done**

## Sole Tenancy

This step is not required and can therefore not be ignored.


Click **Create**.
