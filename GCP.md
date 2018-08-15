# Google Cloud Platform

This  prescribed manner in which DEL projects should construct and consume GCP elements for consumption in demonstrations.

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
    * **ACL** for Network ACLs
    * **RTT** for Route Tables
    * **GCP** for Google Compute Engine
    * **PIP** for Public IP Addresses
   * YYYYYYYY is a description or unique ID - for GCP instances use the first 3 characters for the OS e.g. WIN, LNX, RHL (suggest eight characters max for YYYYYYYY)
   * Four Tags are required on all entities to allow AWS resource consumption to be identified
        * Name	the value generated above if not already provided
        * Organisation	always DEL
        * Project	the related DEL project or initiative
        * Owner the owning DEL team member in the form firstname-lastname
        
## Virtual Private Cloud

To provide a home for your resources a Virtual Private Cloud (VPC) must be created.  Each project/demo/initiative that requires GCP resources can create its own VPC.

Accessed under Navigation Menu Services, Networking, VPC network, VPC networks

Click on the **CREATE VPC NETWORK** button

In the console, enter the following information:

**VPC**

| Field         | Value                        |
| ------------- |:----------------------------:|
| Name          | del-vpc-yyyyyyyy             |
| Description   | Description of demo/project  |

**Subnet**




