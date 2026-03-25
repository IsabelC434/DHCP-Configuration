<p align="center"> <img width="600" height="400" alt="DHCPFINAL" src="https://github.com/user-attachments/assets/dbdb40d7-765e-41e4-a6c8-50db67be6cc7" />

# DHCP Behavior and IP Assignment Troubleshooting in an Azure Hosted Windows Environment

## 🔹Summary
#### This project focuses on analyzing Dynamic Host Configuration Protocol (DHCP) behavior and troubleshooting client IP configuration within a Windows based environment hosted in Microsoft Azure.

#### The objective was to validate how a client machine receives automatic network configuration, identify the DHCP source in a cloud hosted environment, and troubleshoot connectivity related issues tied to IP assignment and client side configuration.

#### During testing, the client successfully received a valid IP address, default gateway, DNS server, and DHCP lease information through automatic network configuration. However, analysis of the results showed that the DHCP server in use was 168.63.129.16, which is Azure’s internal DHCP service rather than the Windows Server instance configured in the lab.

#### Because Azure manages DHCP at the virtual network level, traditional server-side DHCP testing behaved differently than it would in a fully self managed on premises lab. As a result, this project emphasizes client side validation, cloud DHCP behavior, and network troubleshooting rather than direct Windows Server DHCP lease control.


## 🔹Objectives
  - Understand how DHCP provides automatic network configuration to client systems
  - Validate successful IP assignment in an Azure-hosted environment
  - Identify the DHCP source using command-line tools
  - Analyze client network configuration with ipconfig /all
  - Recognize how Azure-managed DHCP affects expected lab behavior
  - Troubleshoot and restore proper client network configuration

## 🔹Environment Set Up
  - Server: DC-1 (Windows Server 2022)
  - Client: Client-1 (Windows 10)
  - Platform: Microsoft Azure Virtual Machines
  - Network Configuration: Both virtual machines deployed within the same Virtual Network (VNET) and subnet
  - Domain Environment: Active Directory integrated lab environment

---
## 🔹Demonstration and Analysis
- ### Breif Context
#### in enterprise environments, DHCP is used to automatically assigning IP addresses and other network settings to client systems. When DHCP related issues occur, users may lose connectivity, making IP configuration analysis a common troubleshooting responsibility in Help Desk and support roles.  This project highlights how cloud managed networking services can differ from traditional on premises configurations, requiring adaptive troubleshooting approaches. 

- ## DHCP Role Installation on DC-1
#### The DHCP Server role was installed on the Windows Server machine through Server Manager to examine DHCP behavior within the environment and compare expected server-side behavior against actual cloud-hosted results.

#### Although the role was added successfully, later testing showed that Azure continued managing DHCP assignment at the virtual network level.

- ## Client IP Configuration
#### On Client-1, the network adapter was left configured to obtain IP and DNS settings automatically.
#### The following command was used to inspect the full network configuration:
  - ipconfig /all
#### The output confirmed that the client successfully received:
  - A valid IPv4 address: 10.0.0.8
  - A subnet mask: 255.255.255.0
  - A default gateway: 10.0.0.1
  - A DNS server: 10.0.0.7 (dc-1 private IP)
#### The client also showed: 
  - DHCP Enabled: Yes
  - DHCP Server: 168.63.129.16
#### This verified that the client was using and automatic IP configuration successfully, but the DHCP source was Azure rather than the local Windows Server.

- ## Azure DHCP Behavior Obersvation
#### Analysis of the DHCP server address revealed that the client was receiving its lease from 168.63.129.16, which is Azure's internal DHCP service. 
#### This was an important finding because it explains why the client continued receiving IP configuration even after local DHCP role installation on DC-1. In Azure, DHCP is abstracted and managed by the platform itself, which changes the expected outcome compared to a traditional on premises Windows Server DHCP deployment. 
#### This observation demonstrates the importance of validationg actual network behavior rather than assuming infrastructure roles are functioning as they would in a different environment. 

- ## Release and Renew Testing
#### To validate lease behavior, the following command sequence was run on Client-1:
  - ipconfig /release && ipconfig /renew
#### The client temorarily released its current configuration and successfully renewed a valid IP configuration afterward. The resulting network information remained consistent, including: 
  - IPv4 address: 10.0.0.8
  - Default gateway: 10.0.0.1
#### This demonstrated that:
  - DHCP renewal was working properly
  - The default gateway remained consistent
  - Azure DHCP reassigned a valid network configuration to the client



---
 ## 🔹Troubleshooting and Interpretation
#### At first, the expectation was that DHCP behavior would be controlled by the Windows Server instance after role installation. However, command line analysis showed that the client lease source was Azure managed DHCP instead. 
#### Rather than treating this as a failed lab, the result was analyzed as an environmental difference between cloud hosted virtual networking and traditional server managed DHCP deployments.
#### This required adapting the tourbleshooting approach to focus on:
  - Identifying the actual DHCP provider
  - Verifying client side automatic configuration
  - Understanding why the gateway and IP lease remained valid
  - Documenting the difference between expected and actual behavior in Azure
#### this reflects a realistic troubleshooting process: verify assumptions, inspect evidence, and adjust conclusions based on the environment.


---

## 🔹Key Observations
  - DHCP was successfully providing network configuration to the client
  - The client was not using DC-1 as its DHCP server
  - Azure provided DHCP services through 168.63.129.16
  - The client maintained valid network configuration after release and renew testing
  - Cloud hosted environments can override traditional server side DHCP expectations
  - Proper troubleshooting requires validating the actual DHCP source rather than assuming local control

---

## 🔹Skills Demonstrated
  - Client side network configuration analysis
  - Use of ipconfig /all, ipconfig /release, and ipconfig /renew
  - Identification of DHCP source in a cloud environment
  - Recognition of Azure managed network behavior
  - Structured troubleshooting and interpretation of unexpected results
  - Technical documentation of lab findings and environment specific behavior 
