---
title: Azure virtuele Machines-implementatie voor SAP NetWeaver | Microsoft Docs
description: Informatie over het implementeren van SAP-software op Linux virtuele machines in Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.openlocfilehash: 4c06b1c8265a12af6764124e3c753e9456a2be20
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>Azure virtuele Machines-implementatie voor SAP NetWeaver
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (Azure Virtual Machines DBMS deployment for SAP)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching for VMs and VHDs)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structure of a RDBMS deployment)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (High availability and disaster recovery with Azure VMs)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 and later)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 and earlier releases)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Using a SQL Server image from the Azure Marketplace)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (General SQL Server for SAP on Azure summary)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Specifics to SQL Server RDBMS)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Storage configuration)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Backup and restore)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Performance considerations for backup and restore)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Other)
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md (Azure Virtual Machines deployment for SAP)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resources)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Deploying a VM by using a custom image)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Deploying a VM from the Azure Marketplace for SAP)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Deploying a VM with a custom image for SAP)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Moving a VM from on-premises using a non-generalized Azure VHD with SAP)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Deployment scenarios of VMs for SAP on Microsoft Azure)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Deploying Azure PowerShell cmdlets)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download and import SAP-relevant PowerShell cmdlets)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join a VM to an on-premises domain - Windows only)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Download, install, and enable the Azure VM Agent)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Configure the Azure Enhanced Monitoring Extension for SAP)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness check for Azure Enhanced Monitoring for SAP)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health check for the Azure monitoring infrastructure)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Troubleshooting Azure monitoring for SAP)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure monitoring)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure the proxy)
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and troubleshooting for setting up end-to-end monitoring)

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md (Azure Virtual Machines planning and implementation for SAP)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High availability and disaster recovery for SAP NetWeaver running on Azure Virtual Machines)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (High availability for SAP Application Servers)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Using Autostart for SAP instances)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Cloud-only - Virtual Machine deployments in Azure without dependencies on the on-premises customer network)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - Deployment of single or multiple SAP VMs in Azure fully integrated with the on-premises network)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regions)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fault domains)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domains)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure availability sets)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtual machines concept)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Moving a VM from on-premises to Azure with a non-generalized disk)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Deploying a VM with a customer specific image)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Preparation for moving a VM from on-premises to Azure with a non-generalized disk)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Preparation for deploying a VM with a customer specific image for SAP)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Preparing VMs with SAP for Azure)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Difference between an Azure disk and an Azure image)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Uploading a VHD from on-premises to Azure)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Copying disks between Azure Storage accounts)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD structure for SAP deployments)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Setting automount for attached disks)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Single VM with SAP NetWeaver demo/training scenario)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepts of Cloud-Only deployment of SAP instances)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring Solution for SAP)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Managed Disks)
[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and data disks)

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-os-disk-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk-md%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image-md%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image-md%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../windows/premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Manage virtual machines by using Azure Resource Manager and PowerShell)
[virtual-machines-windows-agent-user-guide]:../../windows/agent-user-guide.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Virtuele Machines in Azure is de oplossing voor organisaties die berekenings-en opslagbronnen in de minimale tijd en zonder langdurige inkoop cycli. U kunt Azure Virtual Machines gebruiken voor het implementeren van klassieke toepassingen, zoals SAP NetWeaver gebaseerde toepassingen in Azure. Uitbreiden van een toepassing betrouwbaarheid en beschikbaarheid zonder dat extra on-premises resources. Virtuele Machines in Azure ondersteunt cross-premises connectiviteit, zodat u kunt Azure Virtual Machines integreren in uw organisatie lokale domeinen, privéclouds en SAP system Liggend.

In dit artikel komen we de stappen voor het implementeren van SAP-toepassingen op virtuele machines (VM's) in Azure, met inbegrip van alternatieve implementatie-opties en het oplossen van problemen. In dit artikel is gebaseerd op de informatie in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide]. Ook als aanvulling SAP installatie documentatie en SAP-opmerkingen, de primaire resources zijn voor het installeren en implementeren van software voor SAP op.

## <a name="prerequisites"></a>Vereisten
Instellen van een virtuele machine van Azure voor SAP-software-implementatie omvat meerdere stappen en bronnen. Voordat u begint, controleert u dat u voldoet aan de vereisten voor het SAP-software installeren op virtuele machines in Azure.

### <a name="local-computer"></a>Lokale computer
U kunt een PowerShell-script en de Azure portal gebruiken voor het beheren van Windows of Linux-machines. Voor beide hulpprogramma's moet u een lokale computer met Windows 7 of een latere versie van Windows. Als u wilt beheren, alleen virtuele Linux-machines en u wilt een Linux-computer om deze taak gebruiken, kunt u Azure CLI.

### <a name="internet-connection"></a>Internetverbinding
Als u wilt downloaden en uitvoeren van de hulpprogramma's en scripts die vereist voor software-SAP-implementatie zijn, moet u verbonden zijn met Internet. De Azure-VM met de Azure verbeterde extensie Monitoring voor SAP moet ook toegang tot het Internet. Als de virtuele machine in Azure deel uit van een virtuele Azure-netwerk of het lokale domein maakt, zorg ervoor dat de relevante proxy-instellingen zijn ingesteld, zoals beschreven in [configureert de proxy][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure-abonnement
U moet een actief Azure-account.

### <a name="topology-and-networking"></a>Topologie en netwerken
U moet de topologie en de architectuur van de SAP-implementatie definiëren in Azure:

* Azure storage-accounts moeten worden gebruikt
* Virtueel netwerk waar u het SAP-systeem implementeren
* Resourcegroep die u wilt implementeren de SAP-systeem
* Azure-regio waar u het SAP-systeem implementeren
* SAP-configuratie (twee lagen of drie lagen)
* VM-grootten en het aantal extra gegevensschijven worden gekoppeld aan de virtuele machines
* Configuratie van SAP correctie en Transport-systeem (CTS)

Maak en configureer Azure storage-accounts (indien nodig) of Azure virtual networks voordat u begint met de SAP-software-implementatieproces. Zie voor meer informatie over het maken en configureren van deze resources [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide].

### <a name="sap-sizing"></a>SAP-schaling
Weten voor SAP-schaling van de volgende informatie:

* Verwachte SAP-werkbelasting, bijvoorbeeld met behulp van de SAP toepassing prestaties Standard (SAP's) en het hulpprogramma snelle Sizer SAP-nummer
* Vereiste resource en geheugen processorgebruik van het SAP-systeem
* Vereiste input/output (I/O)-bewerkingen per seconde
* Netwerkbandbreedte van uiteindelijke communicatie tussen virtuele machines in Azure vereist
* Vereiste netwerkbandbreedte tussen lokale activa en het Azure geïmplementeerd SAP-systeem

### <a name="resource-groups"></a>Resourcegroepen
In Azure Resource Manager, u kunt resourcegroepen gebruiken voor het beheren van alle toepassingsresources in uw Azure-abonnement. Zie voor meer informatie [overzicht van Azure Resource Manager][resource-group-overview].

## <a name="resources"></a>Resources

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-resources
Wanneer u uw SAP-software-implementatie instelt, moet u de volgende SAP-bronnen:

* SAP-notitie [1928533], die is:
  * Lijst met Azure VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van de database
  * Vereiste SAP-kernel-versie voor Windows en Linux op Microsoft Azure

* SAP-notitie [2015553] bevat vereisten voor software-implementaties SAP SAP ondersteund in Azure.
* SAP-notitie [2178632] bevat gedetailleerde informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [1409604] heeft de vereiste Hostagent SAP-versie voor Windows in Azure.
* SAP-notitie [2191498] heeft de vereiste Hostagent SAP-versie voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over SAP licentieverlening op Linux in Azure.
* SAP-notitie [1984787] heeft algemene informatie over SUSE Linux Enterprise Server 12.
* SAP-notitie [2002167] heeft algemene informatie over Red Hat Enterprise Linux 7.x.
* SAP-notitie [2069760] heeft algemene informatie over Oracle Linux 7.x.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure verbeterde extensie Monitoring voor SAP.
* SAP-notitie [1597355] bevat algemene informatie over wisselruimte voor Linux.
* [SAP op de pagina Azure SCN](https://wiki.scn.sap.com/wiki/x/Pia7Gg) nieuws en een verzameling van nuttige informatiebronnen heeft.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) heeft alle SAP-opmerkingen voor Linux vereist.
* SAP-specifieke PowerShell-cmdlets die deel van uitmaken [Azure PowerShell][azure-ps].
* SAP-specifieke Azure CLI-opdrachten die deel van uitmaken [Azure CLI][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows-bronnen
Deze artikelen Microsoft omvatten SAP-implementaties in Azure:

* [Azure virtuele Machines, planning en implementatie voor SAP NetWeaver][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP NetWeaver (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Implementatiescenario's voor SAP-software op Azure Virtual machines
U hebt meerdere mogelijkheden voor het implementeren van virtuele machines en gekoppelde schijven in Azure. Het is belangrijk om te begrijpen van de verschillen tussen implementatieopties, omdat u andere stappen nemen mogelijk om uw virtuele machines voorbereiden voor implementatie op basis van het implementatietype dat u kiest.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Implementatie van een virtuele machine uit Azure Marketplace voor SAP
U kunt een installatiekopie van een geleverd door Microsoft of door een derde partij in Azure Marketplace gebruiken voor het implementeren van uw virtuele machine. De Marketplace biedt een aantal standaard installatiekopieën van het besturingssysteem van Windows Server en verschillende Linux-distributies. U kunt een installatiekopie die database-beheer bevat ook implementeren system (DBMS) SKU's, bijvoorbeeld Microsoft SQL Server. Zie voor meer informatie over het gebruik van afbeeldingen met DBMS SKU's [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide].

Het volgende stroomdiagram toont de SAP-specifieke volgorde van stappen voor het implementeren van een virtuele machine uit Azure Marketplace:

![Stroomdiagram van VM-implementatie voor SAP-systemen met behulp van een installatiekopie van een virtuele machine uit Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Een virtuele machine maken met behulp van de Azure-portal
De eenvoudigste manier om een nieuwe virtuele machine maken met een installatiekopie vanuit Azure Marketplace is via de Azure-portal.

1.  Ga naar <https://portal.azure.com/#create/hub>.  Of Selecteer in het menu van Azure portal, **+ nieuw**.
2.  Selecteer **Compute**, en selecteer vervolgens het type van het besturingssysteem die u wilt implementeren. Bijvoorbeeld, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) of Oracle Linux 7.2. De standaardweergave voor de lijst wordt niet weergegeven voor dat alle ondersteunde besturingssystemen. Selecteer **alle** voor een volledige lijst. Zie voor meer informatie over ondersteunde besturingssystemen voor software-implementatie voor SAP SAP-notitie [1928533].
3.  Controleer de voorwaarden en bepalingen op de volgende pagina.
4.  In de **een implementatiemodel selecteren** Selecteer **Resource Manager**.
5.  Selecteer **Maken**.

De wizard begeleidt u bij het instellen van de vereiste parameters voor het maken van de virtuele machine, naast alle vereiste bronnen, zoals netwerkinterfaces en storage-accounts. Enkele van deze parameters zijn:

1. **Basics**:
 * **Naam**: de naam van de resource (de naam van de virtuele machine).
 * **VM-schijftype**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruiken van Premium-opslag voor uw gegevensschijven wilt, raden wij u Premium-opslag voor de OS-schijf.
 * **Gebruikersnaam en wachtwoord** of **openbare SSH-sleutel**: Geef de gebruikersnaam en wachtwoord van de gebruiker die is gemaakt tijdens het inrichten. Voor een virtuele Linux-machine, kunt u de openbare sleutel voor Secure Shell (SSH) die u aan te melden bij de computer gebruikt.
 * **Abonnement**: Selecteer het abonnement dat u gebruiken wilt voor het inrichten van de nieuwe virtuele machine.
 * **Resourcegroep**: de naam van de resourcegroep voor de virtuele machine. U kunt u de naam van een nieuwe resourcegroep of de naam van een resourcegroep die al bestaat.
 * **Locatie**: waar u de nieuwe virtuele machine te implementeren. Als u wilt dat de virtuele machine verbinden met uw on-premises netwerk Controleer of selecteert u de locatie van het virtuele netwerk die Azure met uw on-premises netwerk verbindt. Zie voor meer informatie [Microsoft Azure-netwerken] [ planning-guide-microsoft-azure-networking] in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver] [ planning-guide].
2. **De grootte van**:

     Zie voor een lijst van ondersteunde VM typen SAP-notitie [1928533]. Zorg ervoor dat u het juiste type van de virtuele machine selecteren als u wilt gebruiken van Azure Premium-opslag. Niet alle VM-typen bieden ondersteuning voor Premium-opslag. Zie voor meer informatie [opslag: Microsoft Azure Storage- en gegevensschijven] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] en [Azure Premium-opslag] [ planning-guide-azure-premium-storage] in [ Azure virtuele Machines, planning en implementatie voor SAP NetWeaver][planning-guide].

3. **Instellingen**:
  * **Storage**
    * **Schijftype**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruiken van Premium-opslag voor uw gegevensschijven wilt, raden wij u Premium-opslag voor de OS-schijf.
    * **Gebruik de schijven van de beheerde**: Selecteer Ja als u gebruiken van schijven worden beheerd wilt,. Zie voor meer informatie over beheerde schijven hoofdstuk [schijven beheerd] [ planning-guide-managed-disks] in de planning guide.
    * **Storage-account**: Selecteer een bestaand opslagaccount of maak een nieuwe. Niet alle opslagtypen werken voor SAP-toepassingen. Zie voor meer informatie over opslagtypen [Microsoft Azure Storage] [ dbms-guide-2.3] in [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver] [ dbms-guide].
  * **Netwerk**
    * **Virtueel netwerk** en **Subnet**: voor het integreren van de virtuele machine met het intranet, selecteer het virtuele netwerk die is verbonden met uw on-premises netwerk.
    * **Openbaar IP-adres**: Selecteer het openbare IP-adres dat u wilt gebruiken, of parameters voor het maken van een nieuw openbaar IP-adres invoeren. U kunt een openbare IP-adres gebruiken voor toegang tot uw virtuele machine via het Internet. Zorg ervoor dat u ook een netwerkbeveiligingsgroep voor beveiligde toegang tot uw virtuele machine te maken.
    * **Netwerkbeveiligingsgroep**: Zie voor meer informatie [beheren van netwerkverkeer met netwerkbeveiligingsgroepen][virtual-networks-nsg].
  * **Extensies**: U kunt extensies van virtuele machine installeren door ze toe te voegen aan de implementatie. U hoeft niet de extensies toevoegen in deze stap. De extensies die zijn vereist voor ondersteuning van SAP worden later geïnstalleerd. Zie hoofdstuk [configureren van de Azure verbeterde extensie Monitoring voor SAP] [ deployment-guide-4.5] in deze handleiding.
  * **Hoge beschikbaarheid**: een beschikbaarheidsset selecteren of geef de parameters voor het maken van een nieuwe beschikbaarheidsset. Zie voor meer informatie [Azure beschikbaarheidssets][planning-guide-3.2.3].
  * **Monitoring**
    * **Diagnostische gegevens starten**: U kunt selecteren **uitschakelen** voor diagnostische gegevens over opstarten.
    * **Gastbesturingssysteem diagnostics**: U kunt selecteren **uitschakelen** voor het bewaken van diagnostische gegevens.

4. **Samenvatting**:

  Bekijk uw selecties en selecteer vervolgens **OK**.

De virtuele machine wordt geïmplementeerd in de resourcegroep die u hebt geselecteerd.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Een virtuele machine met behulp van een sjabloon maken
U kunt een virtuele machine maken met behulp van een van de SAP-sjablonen gepubliceerd in de [GitHub-opslagplaats voor azure-snelstartsjablonen][azure-quickstart-templates-github]. U kunt ook handmatig maken een virtuele machine met behulp van de [Azure-portal][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], of [Azure CLI][virtual-machines-linux-tutorial].

* [**Met twee lagen (slechts één virtuele machine) configuratiesjabloon** (sap-2-laag-marketplace-installatiekopie)][sap-templates-2-tier-marketplace-image]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine, moet u deze sjabloon gebruiken.
* [**Met twee lagen (slechts één virtuele machine) configuratiesjabloon - beheerde schijven** (sap-2-tier-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine en beheerd schijven, moet u deze sjabloon gebruiken.
* [**Configuratie met drie lagen (meerdere virtuele machines) sjabloon** (sap-3-tier-marketplace-installatiekopie)][sap-templates-3-tier-marketplace-image]

  Voor het maken van een systeem met drie lagen met behulp van meerdere virtuele machines, moet u deze sjabloon gebruiken.
* [**Configuratie met drie lagen (meerdere virtuele machines) sjabloon - beheerde schijven** (sap-3-tier-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  Voor het maken van een systeem met drie lagen met meerdere virtuele machines en schijven beheerd, moet u deze sjabloon gebruiken.

Voer de volgende parameters voor de sjabloon in de Azure-portal:

1. **Basics**:
  * **Abonnement**: het abonnement moet worden gebruikt om de sjabloon te implementeren.
  * **Resourcegroep**: de resourcegroep moet worden gebruikt om de sjabloon te implementeren. U kunt een nieuwe resourcegroep maken of u kunt een bestaande resourcegroep selecteren in het abonnement.
  * **Locatie**: waar u de sjabloon implementeert. Als u een bestaande resourcegroep hebt geselecteerd, wordt de locatie van die resourcegroep wordt gebruikt.

2. **Instellingen**:
  * **ID van het systeem SAP**: de SAP-systeem-ID (SID).
  * **Type besturingssysteem**: het besturingssysteem die u implementeren wilt, bijvoorbeeld, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) of Oracle Linux 7.2.

    De lijstweergave wordt niet weergegeven voor dat alle ondersteunde besturingssystemen. Zie voor meer informatie over ondersteunde besturingssystemen voor software-implementatie voor SAP SAP-notitie [1928533].
  * **Grootte van het SAP**: de grootte van het SAP-systeem.

    Het aantal SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator.
  * **Beschikbaarheid van het systeem** (alleen drie lagen-sjabloon): de beschikbaarheid van het systeem.

    Selecteer **HA** voor een configuratie die geschikt is voor de installatie van een hoge beschikbaarheid. Twee databaseservers en twee servers voor ABAP SAP centrale Services (ASC's) worden gemaakt.
  * **Opslagtype** (alleen twee lagen-sjabloon): het type opslag te gebruiken.

    Voor grotere systemen ten zeerste aangeraden Azure Premium-opslag. Zie de volgende bronnen voor meer informatie over opslagtypen:
      * [Gebruik van Azure Premium-SSD-opslag voor SAP DBMS exemplaar][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] in [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]
      * [Premium-opslag: Krachtige opslag voor Azure Virtual Machine-werkbelasting][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
  * **Gebruikersnaam van de beheerder** en **beheerderswachtwoord**: een gebruikersnaam en wachtwoord.
    Een nieuwe gebruiker is voor aanmelding bij de virtuele machine gemaakt.
  * **Nieuwe of bestaande subnet**: bepaalt of een nieuw virtueel netwerk en subnet worden gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
  * **Subnet-ID**: de ID van het subnet in de virtuele machines verbinding maken. Selecteer het subnet van uw virtueel particulier netwerk (VPN) of Azure ExpressRoute virtueel netwerk te gebruiken voor de virtuele machine verbinding met uw on-premises netwerk. De ID meestal uitziet: /subscriptions/&lt;abonnements-id > /resourceGroups/&lt;Resourcegroepnaam > /providers/Microsoft.Network/virtualNetworks/&lt;virtuele-netwerknaam > /subnets/&lt;subnetnaam >

3. **Voorwaarden en bepalingen**:  
    Lees en accepteer de juridische voorwaarden.

4.  Selecteer **aankoop**.

De Azure VM-Agent wordt geïmplementeerd standaard, wanneer u een installatiekopie uit Azure Marketplace gebruiken.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Afhankelijk van hoe uw on-premises netwerk is geconfigureerd, moet u mogelijk de proxy op de virtuele machine niet instellen. Als uw VM is verbonden met uw on-premises netwerk via VPN- of ExpressRoute, de virtuele machine mogelijk geen toegang tot Internet, en niet kunnen downloaden van de vereiste extensies of bewakingsgegevens verzamelen. Zie voor meer informatie [configureert de proxy][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>Lid worden van een domein (alleen Windows)
Als uw Azure-implementatie is verbonden met een lokale Active Directory- of DNS-exemplaar op via een Azure site-naar-site VPN-verbinding of ExpressRoute (dit heet *cross-premises* in [Azure Virtual Machines plannen en de implementatie voor SAP NetWeaver][planning-guide]), wordt verwacht dat de virtuele machine lid wordt een lokaal domein. Zie voor meer informatie over overwegingen voor deze taak [een virtuele machine toevoegen aan een lokaal domein (alleen Windows)][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Bewaking configureren
Om er zeker van te zijn dat uw omgeving, instellen van de extensie Azure Monitoring voor SAP, zoals beschreven in biedt ondersteuning voor SAP [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5]. Controleer de vereisten voor het bewaken van SAP en vereist minimaal vereiste versies van de Kernel SAP en SAP-Agent Host in de resources die worden weergegeven [SAP resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Bewaking van selectievakje
Controleer of bewaking werkt, zoals beschreven in [controles en het oplossen van problemen voor het instellen van het end-to-end bewaking][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Stappen na de implementatie
Nadat u de virtuele machine maken en de virtuele machine is geïmplementeerd, moet u de vereiste software-onderdelen installeren op de virtuele machine. Vanwege de volgorde van de installatie van implementatie/software in dit type van de VM-implementatie van moet de software worden geïnstalleerd al beschikbaar zijn, in Azure, op een andere virtuele machine of als een schijf die kan worden gekoppeld. Of Overweeg het gebruik van een cross-premises-scenario in welke verbinding met de on-premises assets (installatie shares) is opgegeven.

Wanneer u uw virtuele machine in Azure implementeert, volgt u dezelfde richtlijnen en hulpprogramma's voor de SAP-software installeren op de virtuele machine als in een on-premises omgeving. SAP om software te installeren op een virtuele machine in Azure, zowel SAP en Microsoft raden u aan dat u uploadt en opslaan van de SAP-installatiemedia op Azure VHD's of schijven beheerd, of dat u een virtuele machine van Azure die maakt fungeert als een bestandsserver met alle de vereiste SAP-installatiemedia.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Een virtuele machine met een aangepaste installatiekopie voor SAP implementeren
Omdat verschillende versies van een besturingssysteem of DBMS patch verschillende behoeften hebben, de installatiekopieën u in Azure Marketplace vindt mogelijk niet voldoet aan uw behoeften. Mogelijk wilt u in plaats daarvan een virtuele machine maken met behulp van uw eigen OS/DBMS VM-installatiekopie, die u later opnieuw kunt implementeren.
Verschillende stappen u maakt een persoonlijke installatiekopie voor Linux dan als u wilt maken van een voor Windows.

- - -
> ![Windows][Logo_Windows] Windows
>
> Als u een Windows-installatiekopie die u gebruiken kunt om meerdere virtuele machines te implementeren, moeten de Windows-instellingen (zoals Windows SID en de hostnaam) worden gescheiden of op de lokale virtuele machine worden gegeneraliseerd. U kunt [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) om dit te doen.
>
> ![Linux][Logo_Linux] Linux
>
> Als u een Linux-installatiekopie die u gebruiken kunt om meerdere virtuele machines te implementeren, worden sommige instellingen Linux gescheiden of op de lokale virtuele machine worden gegeneraliseerd. U kunt `waagent -deprovision` om dit te doen. Zie voor meer informatie [vastleggen van een virtuele Linux-machine uitgevoerd op Azure] [ virtual-machines-linux-capture-image] en de [gebruikershandleiding voor Azure Linux agent][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
U kunt voorbereiden en een aangepaste installatiekopie maken en vervolgens worden gebruikt voor het maken van meerdere nieuwe virtuele machines. Dit wordt beschreven in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide]. Uw database inhoud instellen met behulp van SAP Software inrichting Manager voor het installeren van een nieuw SAP-systeem (u herstelt een databaseback-up van een schijf die gekoppeld aan de virtuele machine) of door rechtstreeks een databaseback-up terugzetten vanuit Azure-opslag, als het DBMS ondersteuning voor biedt. Zie voor meer informatie [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]. Als u al een SAP-systeem op uw lokale virtuele machine (vooral voor systemen met twee lagen) hebt geïnstalleerd, kunt u de instellingen van het SAP na de implementatie van de Azure VM aanpassen met behulp van de procedure System wijzigen ondersteund door SAP Software inrichting Manager (SAP-notitie [1619720]). Anders kunt u de SAP-software installeren nadat u de virtuele machine in Azure implementeert.

Het volgende stroomdiagram toont de SAP-specifieke volgorde van stappen voor het implementeren van een virtuele machine van een aangepaste installatiekopie:

![Stroomdiagram van VM-implementatie voor SAP-systemen met behulp van een VM-installatiekopie in Marketplace persoonlijke][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Een virtuele machine maken met behulp van de Azure-portal
De eenvoudigste manier om een nieuwe virtuele machine maken van een beheerd schijfkopie is via de Azure-portal. Lees voor meer informatie over het maken van een Schijfinstallatiekopie beheren [beheerde-installatiekopie van een gegeneraliseerde virtuele machine in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)

1.  Ga naar <https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages>. Of Selecteer in het menu van Azure portal, **installatiekopieën**.
2.  Selecteer de installatiekopie van het beheerd schijf die u wilt implementeren en klik op **VM maken**

De wizard begeleidt u bij het instellen van de vereiste parameters voor het maken van de virtuele machine, naast alle vereiste bronnen, zoals netwerkinterfaces en storage-accounts. Enkele van deze parameters zijn:

1. **Basics**:
 * **Naam**: de naam van de resource (de naam van de virtuele machine).
 * **VM-schijftype**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruiken van Premium-opslag voor uw gegevensschijven wilt, raden wij u Premium-opslag voor de OS-schijf.
 * **Gebruikersnaam en wachtwoord** of **openbare SSH-sleutel**: Geef de gebruikersnaam en wachtwoord van de gebruiker die is gemaakt tijdens het inrichten. Voor een virtuele Linux-machine, kunt u de openbare sleutel voor Secure Shell (SSH) die u aan te melden bij de computer gebruikt.
 * **Abonnement**: Selecteer het abonnement dat u gebruiken wilt voor het inrichten van de nieuwe virtuele machine.
 * **Resourcegroep**: de naam van de resourcegroep voor de virtuele machine. U kunt u de naam van een nieuwe resourcegroep of de naam van een resourcegroep die al bestaat.
 * **Locatie**: waar u de nieuwe virtuele machine te implementeren. Als u wilt dat de virtuele machine verbinden met uw on-premises netwerk Controleer of selecteert u de locatie van het virtuele netwerk die Azure met uw on-premises netwerk verbindt. Zie voor meer informatie [Microsoft Azure-netwerken] [ planning-guide-microsoft-azure-networking] in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver] [ planning-guide].
2. **De grootte van**:

     Zie voor een lijst van ondersteunde VM typen SAP-notitie [1928533]. Zorg ervoor dat u het juiste type van de virtuele machine selecteren als u wilt gebruiken van Azure Premium-opslag. Niet alle VM-typen bieden ondersteuning voor Premium-opslag. Zie voor meer informatie [opslag: Microsoft Azure Storage- en gegevensschijven] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] en [Azure Premium-opslag] [ planning-guide-azure-premium-storage] in [ Azure virtuele Machines, planning en implementatie voor SAP NetWeaver][planning-guide].

3. **Instellingen**:
  * **Storage**
    * **Schijftype**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruiken van Premium-opslag voor uw gegevensschijven wilt, raden wij u Premium-opslag voor de OS-schijf.
    * **Gebruik de schijven van de beheerde**: Selecteer Ja als u gebruiken van schijven worden beheerd wilt,. Zie voor meer informatie over beheerde schijven hoofdstuk [schijven beheerd] [ planning-guide-managed-disks] in de planning guide.
  * **Netwerk**
    * **Virtueel netwerk** en **Subnet**: voor het integreren van de virtuele machine met het intranet, selecteer het virtuele netwerk die is verbonden met uw on-premises netwerk.
    * **Openbaar IP-adres**: Selecteer het openbare IP-adres dat u wilt gebruiken, of parameters voor het maken van een nieuw openbaar IP-adres invoeren. U kunt een openbare IP-adres gebruiken voor toegang tot uw virtuele machine via het Internet. Zorg ervoor dat u ook een netwerkbeveiligingsgroep voor beveiligde toegang tot uw virtuele machine te maken.
    * **Netwerkbeveiligingsgroep**: Zie voor meer informatie [beheren van netwerkverkeer met netwerkbeveiligingsgroepen][virtual-networks-nsg].
  * **Extensies**: U kunt extensies van virtuele machine installeren door ze toe te voegen aan de implementatie. U hoeft niet te-extensie toevoegen in deze stap. De extensies die zijn vereist voor ondersteuning van SAP worden later geïnstalleerd. Zie hoofdstuk [configureren van de Azure verbeterde extensie Monitoring voor SAP] [ deployment-guide-4.5] in deze handleiding.
  * **Hoge beschikbaarheid**: een beschikbaarheidsset selecteren of geef de parameters voor het maken van een nieuwe beschikbaarheidsset. Zie voor meer informatie [Azure beschikbaarheidssets][planning-guide-3.2.3].
  * **Monitoring**
    * **Diagnostische gegevens starten**: U kunt selecteren **uitschakelen** voor diagnostische gegevens over opstarten.
    * **Gastbesturingssysteem diagnostics**: U kunt selecteren **uitschakelen** voor het bewaken van diagnostische gegevens.

4. **Samenvatting**:

  Bekijk uw selecties en selecteer vervolgens **OK**.

De virtuele machine wordt geïmplementeerd in de resourcegroep die u hebt geselecteerd.
#### <a name="create-a-virtual-machine-by-using-a-template"></a>Een virtuele machine met behulp van een sjabloon maken
Voor het maken van een implementatie met behulp van een persoonlijke installatiekopie van het besturingssysteem van de Azure-portal, een van de volgende SAP-sjablonen te gebruiken. Deze sjablonen zijn gepubliceerd in de [GitHub-opslagplaats voor azure-snelstartsjablonen][azure-quickstart-templates-github]. U kunt ook handmatig maken een virtuele machine, met behulp van [PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**Met twee lagen (slechts één virtuele machine) configuratiesjabloon** (sap-2-laag-gebruiker-installatiekopie)][sap-templates-2-tier-user-image]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine, moet u deze sjabloon gebruiken.
* [**Met twee lagen (slechts één virtuele machine) configuratiesjabloon: schijfimage beheerd** (sap-2-tier-user-image-md)][sap-templates-2-tier-user-image-md]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine en de installatiekopie van een schijf worden beheerd, moet u deze sjabloon gebruiken.
* [**Configuratie met drie lagen (meerdere virtuele machines) sjabloon** (sap-3-tier-gebruiker-installatiekopie)][sap-templates-3-tier-user-image]

  Voor het maken van een systeem met drie lagen met meerdere virtuele machines of uw eigen installatiekopie van het besturingssysteem, moet u deze sjabloon gebruiken.
* [**Configuratie met drie lagen (meerdere virtuele machines) sjabloon: schijfimage beheerd** (sap-3-tier-user-image-md)][sap-templates-3-tier-user-image-md]

  Voor het maken van een systeem met drie lagen met meerdere virtuele machines of uw eigen installatiekopie van het besturingssysteem en installatiekopie van een schijf worden beheerd, moet u deze sjabloon gebruiken.

Voer de volgende parameters voor de sjabloon in de Azure-portal:

1. **Basics**:
  * **Abonnement**: het abonnement moet worden gebruikt om de sjabloon te implementeren.
  * **Resourcegroep**: de resourcegroep moet worden gebruikt om de sjabloon te implementeren. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep selecteren in het abonnement.
  * **Locatie**: waar u de sjabloon implementeert. Als u een bestaande resourcegroep hebt geselecteerd, wordt de locatie van die resourcegroep wordt gebruikt.
2. **Instellingen**:
  * **ID van het systeem SAP**: de SAP-systeem-ID.
  * **Type besturingssysteem**: het besturingssysteemtype die u implementeren wilt (Windows of Linux).
  * **Grootte van het SAP**: de grootte van het SAP-systeem.

    Het aantal SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator.
  * **Beschikbaarheid van het systeem** (alleen drie lagen-sjabloon): de beschikbaarheid van het systeem.

    Selecteer **HA** voor een configuratie die geschikt is voor de installatie van een hoge beschikbaarheid. Twee databaseservers en twee servers voor ASC's worden gemaakt.
  * **Opslagtype** (alleen twee lagen-sjabloon): het type opslag te gebruiken.

    Voor grotere systemen ten zeerste aangeraden Azure Premium-opslag. Zie de volgende bronnen voor meer informatie over opslagtypen:
      * [Gebruik van Azure Premium-SSD-opslag voor SAP DBMS exemplaar][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] in [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]
      * [Premium-opslag: Krachtige opslag voor workloads van de virtuele machine van Azure][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
  * **Gebruikersinstallatiekopie VHD-URI** (alleen voor niet-beheerde schijf installatiekopie sjabloon): de URI van de persoonlijke OS-installatiekopie VHD, bijvoorbeeld: https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **Opslag van de gebruikersaccount installatiekopie** (alleen voor niet-beheerde schijf installatiekopie sjabloon): de naam van het opslagaccount waar de persoonlijke OS-installatiekopie wordt opgeslagen, bijvoorbeeld &lt;accountname > in https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **userImageId** (alleen beheerde schijven installatiekopie sjabloon): Id van de installatiekopie van de schijf beheerd die u wilt gebruiken
  * **Gebruikersnaam van de beheerder** en **beheerderswachtwoord**: gebruikersnaam en wachtwoord.

    Een nieuwe gebruiker is voor aanmelding bij de virtuele machine gemaakt.
  * **Nieuwe of bestaande subnet**: bepaalt of een nieuw virtueel netwerk en subnet wordt gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
  * **Subnet-ID**: de ID van het subnet waarmee de virtuele machines verbinding maken op. Selecteer het subnet van uw VPN- of ExpressRoute virtueel netwerk gebruiken voor de virtuele machine verbinding met uw on-premises netwerk. De ID ziet er meestal als volgt uit:

    /Subscriptions/&lt;abonnements-id > /resourceGroups/&lt;Resourcegroepnaam > /providers/Microsoft.Network/virtualNetworks/&lt;virtuele-netwerknaam > /subnets/&lt;subnetnaam >

3. **Voorwaarden en bepalingen**:  
    Lees en accepteer de juridische voorwaarden.

4.  Selecteer **aankoop**.

#### <a name="install-the-vm-agent-linux-only"></a>Installeer de VM-Agent (alleen voor Linux)
Voor het gebruik van de sjablonen die zijn beschreven in de vorige sectie, moet de Linux-Agent al zijn geïnstalleerd in de gebruikersinstallatiekopie van de of mislukt de implementatie. Download en installeer de VM-Agent in de gebruikersinstallatiekopie van de, zoals beschreven in [downloaden, installeren en inschakelen van de Azure VM-Agent][deployment-guide-4.4]. Als u de sjablonen niet gebruikt, kunt u ook de VM-Agent later installeren.

#### <a name="join-a-domain-windows-only"></a>Lid worden van een domein (alleen Windows)
Als uw Azure-implementatie is verbonden met een lokale Active Directory- of DNS-exemplaar op via een Azure site-naar-site VPN-verbinding of Azure ExpressRoute (dit heet *cross-premises* in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide]), wordt verwacht dat de virtuele machine lid wordt een lokaal domein. Zie voor meer informatie over overwegingen voor deze stap [een virtuele machine toevoegen aan een lokaal domein (alleen Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Afhankelijk van hoe uw on-premises netwerk is geconfigureerd, moet u mogelijk de proxy op de virtuele machine niet instellen. Als uw VM is verbonden met uw on-premises netwerk via VPN- of ExpressRoute, de virtuele machine mogelijk geen toegang tot Internet, en niet kunnen downloaden van de vereiste extensies of bewakingsgegevens verzamelen. Zie voor meer informatie [configureert de proxy][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Bewaking configureren
Om er zeker van te zijn dat uw omgeving, instellen van de extensie Azure Monitoring voor SAP, zoals beschreven in biedt ondersteuning voor SAP [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5]. Controleer de vereisten voor het bewaken van SAP en vereist minimaal vereiste versies van de Kernel SAP en SAP-Agent Host in de resources die worden weergegeven [SAP resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Bewaking van selectievakje
Controleer of bewaking werkt, zoals beschreven in [controles en het oplossen van problemen voor het instellen van het end-to-end bewaking][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Het verplaatsen van een lokale virtuele machine met behulp van een VHD Azure niet gegeneraliseerd met SAP
In dit scenario dat u wilt verplaatsen van een specifiek SAP-systeem van een on-premises-omgeving naar Azure. U kunt dit doen door het uploaden van de VHD met het besturingssysteem, de SAP-binaire bestanden en uiteindelijk de DBMS-binaire bestanden, plus de VHD's met de bestanden voor gegevens en logboekbestanden van het DBMS naar Azure. In tegenstelling tot het scenario dat wordt beschreven in [Scenario 2: een virtuele machine met een aangepaste installatiekopie implementeren voor SAP][deployment-guide-3.3], in dit geval blijven van de hostnaam, SAP-SID en SAP gebruikersaccounts in de Azure VM, omdat ze zijn geconfigureerd in de on-premises omgeving. U hoeft niet het besturingssysteem te generaliseren. Dit scenario geldt doorgaans voor cross-premises scenario's waarbij deel uit van de SAP-liggend lokaal wordt uitgevoerd en deel ervan wordt uitgevoerd op Azure.

In dit scenario wordt de VM-Agent is **niet** automatisch geïnstalleerd tijdens de implementatie. Omdat de VM-Agent en de Azure verbeterde extensie Monitoring voor SAP SAP NetWeaver uitvoeren op Azure vereist zijn, die u wilt downloaden, installeren en beide onderdelen handmatig inschakelen nadat u de virtuele machine hebt gemaakt.

Zie de volgende bronnen voor meer informatie over de Azure VM-Agent.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Overzicht van Azure VM-Agent][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Gebruikershandleiding voor Azure Linux-Agent][virtual-machines-linux-agent-user-guide]
>
>

- - -

Het volgende stroomdiagram toont de volgorde van stappen voor het verplaatsen van een lokale virtuele machine met behulp van een Azure niet gegeneraliseerd VHD:

![Stroomdiagram van VM-implementatie voor SAP-systemen met behulp van een VM-schijf][deployment-guide-figure-400]

Als de schijf al geüpload en gedefinieerd in Azure (Zie [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide]), voer de taken beschreven in de volgende secties.

#### <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Maakt u een implementatie met behulp van een persoonlijke besturingssysteemschijf via de Azure portal met de SAP-sjabloon is gepubliceerd in de [GitHub-opslagplaats voor azure-snelstartsjablonen][azure-quickstart-templates-github]. U kunt ook handmatig maken een virtuele machine, met behulp van PowerShell.

* [**Met twee lagen (slechts één virtuele machine) configuratiesjabloon** (sap-2-laag-gebruiker-disk)][sap-templates-2-tier-os-disk]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine, moet u deze sjabloon gebruiken.
* [**Met twee lagen (slechts één virtuele machine) configuratiesjabloon - schijf beheerd** (sap-2-tier-user-disk-md)][sap-templates-2-tier-os-disk-md]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine en een schijf worden beheerd, moet u deze sjabloon gebruiken.

Voer de volgende parameters voor de sjabloon in de Azure-portal:

1. **Basics**:
  * **Abonnement**: het abonnement moet worden gebruikt om de sjabloon te implementeren.
  * **Resourcegroep**: de resourcegroep moet worden gebruikt om de sjabloon te implementeren. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep selecteren in het abonnement.
  * **Locatie**: waar u de sjabloon implementeert. Als u een bestaande resourcegroep hebt geselecteerd, wordt de locatie van die resourcegroep wordt gebruikt.
2. **Instellingen**:
  * **ID van het systeem SAP**: de SAP-systeem-ID.
  * **Type besturingssysteem**: het besturingssysteemtype die u implementeren wilt (Windows of Linux).
  * **Grootte van het SAP**: de grootte van het SAP-systeem.

    Het aantal SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator.
  * **Opslagtype** (alleen twee lagen-sjabloon): het type opslag te gebruiken.

    Voor grotere systemen ten zeerste aangeraden Azure Premium-opslag. Zie de volgende bronnen voor meer informatie over opslagtypen:
      * [Gebruik van Azure Premium-SSD-opslag voor SAP DBMS exemplaar][2367194]
      * [Microsoft Azure Storage] [ dbms-guide-2.3] in [Azure virtuele Machine DBMS-implementatie voor SAP NetWeaver][dbms-guide]
      * [Premium-opslag: Krachtige opslag voor Azure Virtual Machine-werkbelasting][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
  * **VHD-URI op besturingssysteemschijf** (alleen voor niet-beheerde schijf-sjabloon): de URI van de persoonlijke besturingssysteemschijf, bijvoorbeeld https://&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
  * **Beheerde schijf-Id van de besturingssysteemschijf** (alleen beheerde schijf-sjabloon): de Id van de schijf beheerd schijf OS /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
  * **Nieuwe of bestaande subnet**: bepaalt of een nieuw virtueel netwerk en subnet worden gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
  * **Subnet-ID**: de ID van het subnet waarmee de virtuele machines verbinding maken op. Selecteer het subnet van het virtuele netwerk te gebruiken voor de virtuele machine verbinding met uw on-premises netwerk VPN of Azure ExpressRoute. De ID ziet er meestal als volgt uit:

    /Subscriptions/&lt;abonnements-id > /resourceGroups/&lt;Resourcegroepnaam > /providers/Microsoft.Network/virtualNetworks/&lt;virtuele-netwerknaam > /subnets/&lt;subnetnaam >

3. **Voorwaarden en bepalingen**:  
    Lees en accepteer de juridische voorwaarden.

4.  Selecteer **aankoop**.

#### <a name="install-the-vm-agent"></a>De VM-Agent installeren
Voor het gebruik van de sjablonen die zijn beschreven in de vorige sectie, de VM-Agent moet worden geïnstalleerd op de schijf met het besturingssysteem of mislukt de implementatie. Download en installeer de VM-Agent in de virtuele machine, zoals beschreven in [downloaden, installeren en inschakelen van de Azure VM-Agent][deployment-guide-4.4].

Als u de sjablonen die zijn beschreven in de vorige sectie niet gebruikt, kunt u de VM-Agent ook later installeren.

#### <a name="join-a-domain-windows-only"></a>Lid worden van een domein (alleen Windows)
Als uw Azure-implementatie is verbonden met een lokale Active Directory- of DNS-exemplaar op via een Azure site-naar-site VPN-verbinding of ExpressRoute (dit heet *cross-premises* in [Azure Virtual Machines plannen en de implementatie voor SAP NetWeaver][planning-guide]), wordt verwacht dat de virtuele machine lid wordt een lokaal domein. Zie voor meer informatie over overwegingen voor deze taak [een virtuele machine toevoegen aan een lokaal domein (alleen Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Afhankelijk van hoe uw on-premises netwerk is geconfigureerd, moet u mogelijk de proxy op de virtuele machine niet instellen. Als uw VM is verbonden met uw on-premises netwerk via VPN- of ExpressRoute, de virtuele machine mogelijk geen toegang tot Internet, en niet kunnen downloaden van de vereiste extensies of bewakingsgegevens verzamelen. Zie voor meer informatie [configureert de proxy][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Bewaking configureren
Om er zeker van te zijn dat uw omgeving, instellen van de extensie Azure Monitoring voor SAP, zoals beschreven in biedt ondersteuning voor SAP [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5]. Controleer de vereisten voor het bewaken van SAP en vereist minimaal vereiste versies van de Kernel SAP en SAP-Agent Host in de resources die worden weergegeven [SAP resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Bewaking van selectievakje
Controleer of bewaking werkt, zoals beschreven in [controles en het oplossen van problemen voor het instellen van het end-to-end bewaking][deployment-guide-troubleshooting-chapter].

## <a name="update-the-monitoring-configuration-for-sap"></a>Bijwerken van de bewakingsconfiguratie voor SAP
Werk de SAP-bewakingsconfiguratie in een van de volgende scenario's:
* Het gemeenschappelijke Microsoft/SAP-team breidt de mogelijkheden voor bewaking en meer of minder items aanvragen.
* Introduceert een nieuwe versie van de onderliggende Azure-infrastructuur die zorgt voor de bewakingsgegevens van Microsoft en de Azure verbeterde extensie Monitoring voor SAP moet worden aangepast aan deze wijzigingen.
* U extra gegevensschijven gekoppeld aan uw Azure VM of u een gegevensschijf verwijderen. Bijwerken van de verzameling van gegevens met betrekking tot opslag in dit scenario. U kunt de configuratie wijzigt door toevoegen of verwijderen van eindpunten of door het IP-adressen toewijzen aan een virtuele machine heeft geen invloed op de configuratie van bewaking.
* De grootte van uw Azure VM die u uit een grootte A5 naar een ander VM-formaat bijvoorbeeld wijzigen.
* U voegt nieuwe netwerkinterfaces aan uw Azure-VM.

Bijwerken voor het bijwerken van controle-instellingen van de bewakingsinfrastructuur door de stappen in [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment"></a>Gedetailleerde taken voor SAP-software-implementatie
In deze sectie bevat gedetailleerde stappen voor het uitvoeren van specifieke taken in de configuratie en implementatie-proces.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell-cmdlets implementeren
1.  Ga naar [Downloads van Microsoft Azure](https://azure.microsoft.com/downloads/).
2.  Onder **opdrachtregelprogramma's**onder **PowerShell**, selecteer **Windows installeren**.
3.  Selecteer in het Microsoft Download Manager in het dialoogvenster voor het gedownloade bestand (bijvoorbeeld WindowsAzurePowershellGet.3f.3f.3fnew.exe) **uitvoeren**.
4.  Voor het uitvoeren van Microsoft Web Platform Installer (Microsoft Web PI), selecteer **Ja**.
5.  Een pagina die lijkt erop dat deze wordt weergegeven:

  ![De installatiepagina voor Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Selecteer **installeren**, en accepteer de licentievoorwaarden voor Microsoft-Software.
7.  PowerShell is geïnstalleerd. Selecteer **voltooien** om de installatiewizard te sluiten.

Controleer regelmatig de updates van de PowerShell-cmdlets die meestal maandelijks worden bijgewerkt. De eenvoudigste manier om te controleren op updates is de voorgaande installatiestappen, tot aan de installatiepagina wordt weergegeven in stap 5 doen. Het versienummer voor datum en de versie van de cmdlets zijn opgenomen op de pagina wordt weergegeven in stap 5. Tenzij anders vermeld in SAP-notitie [1928533] of SAP-notitie [2015553], het is raadzaam dat u met de nieuwste versie van Azure PowerShell-cmdlets werken.

Als u wilt controleren de versie van de Azure PowerShell-cmdlets die zijn geïnstalleerd op uw computer, moet u deze PowerShell-opdracht uitvoeren:
```powershell
(Get-Module AzureRm.Compute).Version
```
Het resultaat ziet er als volgt:

![Resultaat van controle van versie van Azure PowerShell-cmdlet][deployment-guide-figure-600]
<a name="figure-6"></a>

Als de cmdlet voor Azure-versie die is geïnstalleerd op uw computer de huidige versie is, de eerste pagina van de installatiewizard aangeeft dat ze door toe te voegen **(geïnstalleerd)** naar de Producttitel (Zie de volgende schermafbeelding). Uw PowerShell Azure-cmdlets zijn bijgewerkt. Selecteer om de installatiewizard sluit, **afsluiten**.

![De installatiepagina voor Azure PowerShell-cmdlets die aangeeft dat de meest recente versie van Azure PowerShell-cmdlets zijn geïnstalleerd][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI implementeren
1.  Ga naar [Downloads van Microsoft Azure](https://azure.microsoft.com/downloads/).
2.  Onder **opdrachtregelprogramma's**onder **Azure-opdrachtregelinterface**, selecteer de **installeren** koppeling voor uw besturingssysteem.
3.  Selecteer in het Microsoft Download Manager in het dialoogvenster voor het gedownloade bestand (bijvoorbeeld WindowsAzureXPlatCLI.3f.3f.3fnew.exe) **uitvoeren**.
4.  Voor het uitvoeren van Microsoft Web Platform Installer (Microsoft Web PI), selecteer **Ja**.
5.  Een pagina die lijkt erop dat deze wordt weergegeven:

  ![De installatiepagina voor Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Selecteer **installeren**, en accepteer de licentievoorwaarden voor Microsoft-Software.
7.  Azure CLI is geïnstalleerd. Selecteer **voltooien** om de installatiewizard te sluiten.

Controleer regelmatig op updates voor Azure CLI, die meestal maandelijks wordt bijgewerkt. De eenvoudigste manier om te controleren op updates is de voorgaande installatiestappen, tot aan de installatiepagina wordt weergegeven in stap 5 doen.

Als u wilt controleren de versie van Azure CLI op uw computer is geïnstalleerd, moet u deze opdracht uitvoeren:
```
azure --version
```

Het resultaat ziet er als volgt:

![Resultaat van controle van versie van Azure CLI][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Een virtuele machine toevoegen aan een lokaal domein (alleen Windows)
Als u virtuele machines SAP in een cross-premises-scenario waarin lokale Active Directory en DNS zijn uitgebreid in Azure implementeert, wordt verwacht dat de virtuele machines zijn worden gebruikt voor het lidmaatschap van een lokaal domein. De gedetailleerde stappen u rekening houden met een virtuele machine toevoegen aan een lokaal domein en de aanvullende software vereist voor het lid zijn van een lokale domein, hangt af van de klant. Als u wilt toevoegen aan een virtuele machine op het lokale domein, moet u doorgaans aanvullende software, zoals antimalware-software en de back-up of controle-software installeren.

In dit scenario moet u ook om ervoor te zorgen dat als InternetProxy-instellingen worden afgedwongen wanneer een virtuele machine lid van een domein in uw omgeving, de Windows lokale systeemaccount (S-1-5-18) in de Gast-VM heeft de dezelfde proxyinstellingen. De eenvoudigste optie is om af te dwingen de proxy met behulp van Groepsbeleid, is van toepassing op systemen in het domein van een domein.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Downloaden, installeren en inschakelen van de Azure VM-Agent
Voor virtuele machines die vanuit een installatiekopie van het besturingssysteem die niet is gegeneraliseerd (bijvoorbeeld een afbeelding die niet afkomstig zijn uit het hulpprogramma Windows System Preparation of sysprep,) worden geïmplementeerd, moet u handmatig downloaden, installeren en inschakelen van de Azure VM-Agent.

Als u een virtuele machine uit Azure Marketplace implementeert, is deze stap niet vereist. Afbeeldingen vanuit Azure Marketplace hebben al de Azure VM-Agent.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows
1.  Download de Azure VM-Agent:
  1.  Download de [Azure VM-Agent-installatiepakket](https://go.microsoft.com/fwlink/?LinkId=394789).
  2.  Het VM-Agent-MSI-pakket lokaal opslaan op een pc of de server.
2.  De Azure VM-Agent installeren:
  1.  Verbinding maken met de geïmplementeerde virtuele machine in Azure met behulp van Remote Desktop Protocol (RDP).
  2.  Open een venster van Windows Verkenner op de virtuele machine en selecteer de doelmap voor het MSI-bestand van de VM-Agent.
  3.  Sleep de Azure VM-Agent-installatieprogramma MSI-bestand van uw lokale computer/server naar de doelmap van de VM-Agent op de virtuele machine.
  4.  Dubbelklik op het MSI-bestand op de virtuele machine.
3.  Voor virtuele machines die lid zijn van lokale domeinen, zorg ervoor dat eventuele InternetProxy-instellingen ook van toepassing op het lokale Windows-systeem-account (S-1-5-18) in de virtuele machine, zoals beschreven in [configureert de proxy][deployment-guide-configure-proxy]. De VM-Agent wordt uitgevoerd in deze context en moet kunnen verbinding maken met Azure.

Er is geen gebruikersinteractie is vereist voor het bijwerken van de Azure VM-Agent. De VM-Agent wordt automatisch bijgewerkt en vereist geen virtuele machine opnieuw opstarten.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Gebruik de volgende opdrachten voor het installeren van de VM-Agent voor Linux:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL)- of Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Als de agent al is geïnstalleerd, de stappen in voor het bijwerken van de Azure Linux Agent doen [bijwerken van de Azure Linux Agent op een virtuele machine naar de nieuwste versie van GitHub][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Configureer de proxy
De stappen waarmee u de proxy configureren in Windows wijken af van de manier waarop die u de proxy in Linux configureren.

#### <a name="windows"></a>Windows
Proxy-instellingen moeten correct worden ingesteld voor het lokale systeemaccount gebruikt voor toegang tot het Internet. Als de proxy-instellingen niet zijn ingesteld door Groepsbeleid, kunt u de instellingen voor het lokale systeemaccount configureren.

1. Ga naar **Start**, voer **gpedit.msc**, en selecteer vervolgens **Enter**.
2. Selecteer **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen** > **Internet Explorer**. Zorg ervoor dat de instelling **proxy maken instellingen per computer (in plaats per gebruiker)** is uitgeschakeld of niet is geconfigureerd.
3. In **Configuratiescherm**, gaat u naar **Netwerkcentrum** > **Internetopties**.
4. Op de **verbindingen** tabblad de **LAN-instellingen** knop.
5. Schakel de **-instellingen automatisch detecteren** selectievakje.
6. Selecteer de **een proxyserver gebruiken voor uw LAN** uit en voer vervolgens de proxy-adres en poort.
7. Selecteer de **Geavanceerd** knop.
8. In de **uitzonderingen** Voer het IP-adres **168.63.129.16**. Selecteer **OK**.

#### <a name="linux"></a>Linux
Configureer de juiste proxy in het configuratiebestand van de Microsoft Azure Guest-Agent, dat zich op \\enzovoort\\waagent.conf.

De volgende parameters instellen:

1.  **HTTP-proxyhost**. Bijvoorbeeld ingesteld op **proxy.corp.local**.
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  **HTTP-proxypoort**. Bijvoorbeeld ingesteld op **80**.
  ```
  HttpProxy.Port=<port of the proxy host>

  ```
3.  Start de agent opnieuw.

  ```
  sudo service waagent restart
  ```

De proxy-instellingen in \\enzovoort\\waagent.conf ook van toepassing op het vereiste VM-extensies. Als u wilt gebruiken, de Azure-opslagplaatsen Controleer of het verkeer naar deze opslagplaatsen gaat niet via het lokale intranet. Als u de gebruiker gedefinieerde routes geforceerde tunneling inschakelen Controleer of u een route toevoegen gemaakt routeert die verkeer naar de opslagplaatsen rechtstreeks met het Internet en niet via uw site-naar-site VPN-verbinding.

* **SLES**

  U moet ook routes toevoegen voor de IP-adressen die worden vermeld \\enzovoort\\regionserverclnt.cfg. De volgende afbeelding toont een voorbeeld:

  ![Geforceerde tunneling][deployment-guide-figure-50]


* **RHEL**

  U moet ook routes toevoegen voor de IP-adressen van de hosts die worden vermeld in \\enzovoort\\yum.repos.d\\rhui netwerktaakverdelers. Zie de voorgaande afbeelding voor een voorbeeld.

* **Oracle Linux**

  Er zijn geen opslagplaatsen voor Oracle Linux op Azure. U moet uw eigen opslagplaatsen voor Oracle Linux configureren of gebruiken van de openbare-opslagplaatsen.

Zie voor meer informatie over de gebruiker gedefinieerde routes [gebruiker gedefinieerde routes en doorsturen via IP][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Configureren van de Azure uitgebreide controle-extensie voor SAP
Wanneer u de virtuele machine hebt voorbereid, zoals beschreven in [implementatiescenario's van virtuele machines voor SAP op Azure][deployment-guide-3], de Azure VM-Agent is geïnstalleerd op de virtuele machine. De volgende stap is voor het implementeren van de Azure verbeterde extensie Monitoring voor SAP, die beschikbaar is in de Azure-opslagplaats voor uitbreiding in de globale Azure-datacenters. Zie voor meer informatie [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide-9.1].

U kunt PowerShell of Azure CLI installeren en configureren van de Azure verbeterde extensie Monitoring voor SAP. Zie de extensie installeren op een Windows- of Linux-VM met behulp van een Windows-machine [Azure PowerShell][deployment-guide-4.5.1]. Zie de extensie installeren op een Linux-VM met behulp van een bureaublad Linux [Azure CLI][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell voor Linux en Windows-VM 's
De Azure verbeterde extensie Monitoring voor SAP installeren met behulp van PowerShell:

1. Zorg ervoor dat u de nieuwste versie van de Azure PowerShell-cmdlet hebt geïnstalleerd. Zie voor meer informatie [implementeren van Azure PowerShell-cmdlets][deployment-guide-4.1].  
2. Voer de volgende PowerShell-cmdlet uit.
    Voor een lijst met beschikbare omgevingen, voert u `commandlet Get-AzureRmEnvironment`. Als u gebruiken van globale Azure wilt, uw omgeving is **AzureCloud**. Selecteer voor Azure China **AzureChinaCloud**.

    ```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>

    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Nadat u de accountgegevens van uw invoeren en identificeren van de virtuele machine van Azure, wordt het script implementeert de vereiste uitbreidingen en kunt de vereiste onderdelen. Dit kan enkele minuten duren.
Voor meer informatie over `Set-AzureRmVMAEMExtension`, Zie [Set AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].

![Voltooiing van uitvoering van SAP-specifieke Azure-cmdlet Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

De `Set-AzureRmVMAEMExtension` configuratie biedt de stappen voor het configureren van de bewaking voor SAP host.

De uitvoer van het script bevat de volgende informatie:

* Bevestiging dat voor de besturingssysteemschijf en alle extra gegevensschijven bewaking is geconfigureerd.
* De volgende twee berichten Bevestig de configuratie van Storage metrische gegevens voor een specifieke storage-account.
* Een line-of-uitvoer geeft de status van de werkelijke update van de bewakingsconfiguratie.
* Een andere line-of-uitvoer wordt bevestigd dat de configuratie zijn geïmplementeerd of bijgewerkt.
* De laatste regel van de uitvoer is informatief. Het bevat uw opties voor het testen van de bewakingsconfiguratie.
* Als u wilt controleren of alle stappen van de Azure-uitgebreide bewaking is uitgevoerd en dat de Azure-infrastructuur de benodigde gegevens biedt, doorgaan met de gereedheidscontrole voor de Azure verbeterde extensie Monitoring voor SAP, zoals beschreven in [gereedheidscontrole voor Azure-uitgebreide bewaking voor SAP][deployment-guide-5.1].
* Wacht 15-30 minuten voor Azure Diagnostics de relevante gegevens te verzamelen.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI voor virtuele Linux-machines
De Azure verbeterde extensie Monitoring voor SAP met behulp van Azure CLI installeren:

1. Installeer Azure CLI 1.0, zoals beschreven in [installeren van de Azure CLI 1.0][azure-cli].
2. Aanmelden met uw Azure-account:

  ```
  azure login
  ```

3. Schakel over naar modus Azure Resource Manager:

  ```
  azure config mode arm
  ```

4. Azure uitgebreide bewaking inschakelen:

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. Controleer of de extensie Azure verbeterde Monitoring actief zijn op de Azure Linux VM. Controleer of het bestand \\var\\lib\\AzureEnhancedMonitor\\PerfCounters bestaat. Als dit bestaat, bij een opdrachtprompt, voer deze opdracht om informatie die is verzameld door de Azure verbeterde Monitor weer te geven:
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

De uitvoer ziet er als volgt:
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
???
???
```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Controles en het oplossen van problemen voor het bewaken van end-to-end
Nadat u hebt uw Azure-virtuele machine wordt geïmplementeerd en instellen van de relevante Azure bewakingsinfrastructuur, controleert u of alle onderdelen van de extensie Azure verbeterde Monitoring werkt zoals verwacht.

Voer de gereedheidscontrole voor de Azure verbeterde extensie Monitoring voor SAP zoals beschreven [gereedheidscontrole voor de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-5.1]. Als alle resultaten van de gereedheid van de positief zijn en alle relevante prestatiemeteritems OK weergegeven, is Azure bewaking ingesteld met succes. U kunt doorgaan met de installatie van Agent voor SAP-Host, zoals beschreven in de notities SAP in [SAP resources][deployment-guide-2.2]. Als de gereedheidscontrole geeft aan dat items ontbreken, voert u de statuscontrole voor de Azure monitoring-infrastructuur, zoals beschreven in [Statuscontrole voor Azure infrastructuur controleconfiguratie][deployment-guide-5.2]. Zie voor meer opties voor probleemoplossing, [probleemoplossing voor Azure-bewaking voor SAP][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Gereedheid controleren op de Azure verbeterde extensie Monitoring voor SAP
Deze controle zorgt ervoor dat alle maatstaven voor prestaties die worden gebruikt in uw toepassing SAP worden geleverd door de onderliggende Azure monitoring-infrastructuur.

#### <a name="run-the-readiness-check-on-a-windows-vm"></a>Voer de gereedheidscontrole uit op een virtuele machine van Windows

1.  Aanmelden bij de virtuele machine van Azure (met behulp van een beheerdersaccount is niet nodig).
2.  Open een opdrachtpromptvenster.
3.  Wijzig bij de opdrachtprompt de map naar de installatiemap van de Azure verbeterde extensie Monitoring voor SAP: C:\\pakketten\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;versie >\\verwijderen

  De *versie* in het pad naar de controle-extensie kan variëren. Als u mappen voor meerdere versies van de controle-extensie in de installatiemap ziet, Controleer de configuratie van de AzureEnhancedMonitoring Windows-service en ga vervolgens naar de map aangegeven als *pad naar het uitvoerbare bestand*.

  ![Eigenschappen van de Azure verbeterde extensie Monitoring voor SAP-service][deployment-guide-figure-1000]

4.  Voer bij de opdrachtprompt **azperflib.exe** zonder parameters.

  > [!NOTE]
  > Azperflib.exe wordt uitgevoerd in een lus en updates van de verzamelde prestatiemeteritems elke 60 seconden. Als u wilt beëindigen van de lus, sluit het opdrachtpromptvenster.
  >
  >

Als de extensie Azure verbeterde Monitoring is niet geïnstalleerd of de AzureEnhancedMonitoring-service niet wordt uitgevoerd, is de extensie niet goed geconfigureerd. Zie voor gedetailleerde informatie over het implementeren van de extensie [probleemoplossing van de Azure-bewaking infrastructuur voor SAP][deployment-guide-5.3].

##### <a name="check-the-output-of-azperflibexe"></a>Controleer de uitvoer van azperflib.exe
Azperflib.exe uitvoer toont dat alle Azure-prestatiemeteritems voor SAP ingevuld. Een indicator samenvatting en health weergeven onder aan de lijst met items die worden verzameld, de status van de bewaking van Azure.

![Uitvoer van de statuscontrole van de door het uitvoeren van azperflib.exe waarmee wordt aangegeven dat er geen problemen aanwezig][deployment-guide-figure-1100]
<a name="figure-11"></a>

Controleer het resultaat geretourneerd voor de **tellers totaal** uitvoer, die wordt gerapporteerd als leeg en voor **gezondheidsstatus**, in de voorgaande afbeelding wordt weergegeven.

De resulterende waarden als volgt interpreteren:

| Waarden van Azperflib.exe | Azure health-status controleren |
| --- | --- |
| **API-aanroepen - niet beschikbaar** | Items die niet beschikbaar is mogelijk niet van toepassing op de virtuele-machineconfiguratie of fouten zijn. Zie **gezondheidsstatus**. |
| **Prestatiemeteritems van de totale - leeg** |De volgende twee Azure storage-items kunnen niet leeg zijn: <ul><li>Opslag Op latentie lezen Server msec</li><li>Opslag Op latentie lezen E2E msec</li></ul>Alle andere items moeten waarden hebben. |
| **Health-status** |Alleen OK als retourneren status bevat **OK**. |
| **Diagnostics** |Gedetailleerde informatie over de status. |

Als de **gezondheidsstatus** waarde is geen **OK**, volg de instructies in [Statuscontrole voor Azure infrastructuur controleconfiguratie][deployment-guide-5.2].

#### <a name="run-the-readiness-check-on-a-linux-vm"></a>Voer de gereedheidscontrole uit op een Linux-VM

1.  Verbinding maken met de Azure-VM met behulp van SSH.

2.  Controleer de uitvoer van de extensie Azure verbeterde Monitoring.

  a.  Voer `more /var/lib/AzureEnhancedMonitor/PerfCounters` uit.

   **Resultaat verwacht**: lijst met prestatiemeteritems retourneert. Het bestand mag niet leeg zijn.

 b. Voer `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` uit.

   **Resultaat verwacht**: retourneert één regel waar de fout is **geen**, bijvoorbeeld **3; config; Fout; 0; 0; geen; 0; 1456416792; tst-servercs;**

  c. Voer `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` uit.

    **Resultaat verwacht**: retourneert als leeg of bestaat niet.

Als de voorgaande controle niet geslaagd is, voert u deze extra controles:

1.  Zorg ervoor dat de waagent is geïnstalleerd en ingeschakeld.

  a.  Voer `sudo ls -al /var/lib/waagent/` uit.

      **Resultaat verwacht**: geeft een lijst van de inhoud van de map waagent.

  b.  Voer `ps -ax | grep waagent` uit.

   **Resultaat verwacht**: één vermelding strekking weergegeven:`python /usr/sbin/waagent -daemon`

3.   Zorg ervoor dat de extensie Azure verbeterde Monitoring geïnstalleerd en actief is.

  a.  Voer `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` uit.

    **Resultaat verwacht**: geeft een lijst van de inhoud van de extensie voor Azure verbeterde Monitoring-map.

  b. Voer `ps -ax | grep AzureEnhanced` uit.

     **Resultaat verwacht**: één vermelding strekking weergegeven:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

3. SAP Host-Agent installeren zoals is beschreven in SAP-notitie [1031096], en controleer de uitvoer van `saposcol`.

  a.  Voer `/usr/sap/hostctrl/exe/saposcol -d` uit.

  b.  Voer `dump ccm` uit.

  c.  Controleer of de **Virtualization_Configuration\Enhanced bewaking toegang** meetwaarde is **true**.

Als u al een SAP NetWeaver ABAP application server geïnstalleerd hebt, transactie ST06 openen en controleer of de uitgebreide bewaking is ingeschakeld.

Als een van deze controles mislukken, en Zie voor gedetailleerde informatie over het implementeren van de extensie [probleemoplossing van de Azure-bewaking infrastructuur voor SAP][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Serverstatus controleren voor de Azure bewakingsconfiguratie infrastructuur
Als sommige van de bewaking gegevens niet bezorgd correct zoals aangegeven door de test wordt beschreven in [gereedheidscontrole voor Azure-uitgebreide bewaking voor SAP][deployment-guide-5.1], voert de `Test-AzureRmVMAEMExtension` cmdlet om te controleren of de bewaking van infrastructuur en de bewaking Azure-extensie voor SAP correct zijn geconfigureerd.

1.  Zorg ervoor dat u de nieuwste versie van de Azure PowerShell-cmdlet hebt geïnstalleerd, zoals beschreven in [implementeren van Azure PowerShell-cmdlets][deployment-guide-4.1].
2.  Voer de volgende PowerShell-cmdlet uit. Voor een lijst met beschikbare omgevingen, voert u de cmdlet `Get-AzureRmEnvironment`. Als u globale Azure, schakelt de **AzureCloud** omgeving. Selecteer voor Azure China **AzureChinaCloud**.
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of the environment>
  Login-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  Voer de gegevens van uw account en identificeren van de virtuele machine van Azure.

  ![Invoer pagina van de SAP-specifieke Azure cmdlet Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. Het script test de configuratie van de virtuele machine die u selecteert.

  ![Uitvoer van een geslaagde test van de Azure bewakingsinfrastructuur voor SAP][deployment-guide-figure-1300]

Zorg ervoor dat elke status resultaat **OK**. Als u een aantal controles worden niet weergegeven **OK**, voert u de cmdlet update zoals beschreven in [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5]. Wacht 15 minuten en Herhaal de controles die wordt beschreven in [gereedheidscontrole voor Azure-uitgebreide bewaking voor SAP] [ deployment-guide-5.1] en [Statuscontrole voor de configuratie van Azure-infrastructuur bewaking][deployment-guide-5.2]. Als de controles nog steeds een probleem met enkele of alle items blijkt, Zie [probleemoplossing van de Azure-bewaking infrastructuur voor SAP][deployment-guide-5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>De Azure-bewaking infrastructuur voor probleemoplossing voor SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure prestatiemeteritems worden niet weergegeven op alle
De AzureEnhancedMonitoring Windows-service verzamelt maatstaven voor prestaties in Azure. Als de service is niet juist geïnstalleerd of als deze niet in uw virtuele machine wordt uitgevoerd, kunnen geen prestatiegegevens worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de extensie Azure verbeterde Monitoring is leeg

###### <a name="issue"></a>Probleem
De installatiemap C:\\pakketten\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;versie >\\vervolgkeuzelijst is leeg.

###### <a name="solution"></a>Oplossing
De extensie is niet geïnstalleerd. Bepalen of dit een probleem met de proxy (zoals eerder beschreven). U moet mogelijk opnieuw opstarten van de machine of opnieuw uit de `Set-AzureRmVMAEMExtension` configuratiescript.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Service voor Azure-uitgebreide bewaking bestaat niet

###### <a name="issue"></a>Probleem
De AzureEnhancedMonitoring Windows-service bestaat niet.

Azperflib.exe uitvoer is een fout genereert:

![De uitvoering van azperflib.exe geeft aan dat de service van de extensie Azure uitgebreide bewaking voor SAP niet wordt uitgevoerd][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Oplossing
Als de service niet bestaat, is de Azure verbeterde extensie Monitoring voor SAP niet correct is geïnstalleerd. De extensie implementeren met behulp van de stappen beschreven voor het implementatiescenario in [implementatiescenario's van virtuele machines voor SAP in Azure][deployment-guide-3].

Nadat u de extensie geïmplementeerd na een uur, controleer opnieuw of de Azure-prestatiemeteritems u in de Azure VM vindt.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Service voor Azure-uitgebreide bewaking bestaat, maar niet kan worden gestart

###### <a name="issue"></a>Probleem
De AzureEnhancedMonitoring Windows-service bestaat en is ingeschakeld, maar niet kan worden gestart. Controleer het toepassingslogboek voor meer informatie.

###### <a name="solution"></a>Oplossing
De configuratie is onjuist. Opnieuw opstarten van de controle-extensie voor de virtuele machine, zoals beschreven in [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Sommige Azure prestatiemeters ontbreken.
De AzureEnhancedMonitoring Windows-service verzamelt maatstaven voor prestaties in Azure. De service ontvangt gegevens van diverse bronnen. Sommige configuratiegegevens lokaal worden verzameld en bepaalde maatstaven voor prestaties van Azure Diagnostics worden gelezen. Tellers voor opslag worden van uw logboekregistratie op het abonnementsniveau opslag gebruikt.

Als u problemen oplossen met behulp van SAP-notitie [1999351] niet los het probleem moet opnieuw de `Set-AzureRmVMAEMExtension` configuratiescript. U moet wellicht een uur wachten omdat storage analytics of diagnostics items mogelijk niet worden gemaakt, onmiddellijk nadat ze zijn ingeschakeld. Als het probleem zich blijft voordoen, open een SAP customer support bericht op het onderdeel BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een virtuele Linux-machine.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure prestatiemeteritems worden niet weergegeven op alle
Maatstaven voor prestaties in Azure worden door een daemon verzameld. Als de daemon niet wordt uitgevoerd, kunnen geen prestatiegegevens worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de extensie Azure verbeterde Monitoring is leeg

###### <a name="issue"></a>Probleem
De map \\var\\lib\\waagent\\ heeft geen submap voor de extensie Azure verbeterde Monitoring.

###### <a name="solution"></a>Oplossing
De extensie is niet geïnstalleerd. Bepalen of dit een probleem met de proxy (zoals eerder beschreven). Mogelijk moet u start de computer opnieuw op en/of opnieuw de `Set-AzureRmVMAEMExtension` configuratiescript.

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Sommige Azure prestatiemeters ontbreken.
Maatstaven voor prestaties in Azure worden door een daemon die gegevens uit diverse bronnen ontvangt verzameld. Sommige configuratiegegevens lokaal worden verzameld en bepaalde maatstaven voor prestaties van Azure Diagnostics worden gelezen. Tellers voor opslag is afkomstig van de logboeken in uw opslagabonnement.

Zie voor een volledige en bijgewerkte lijst met bekende problemen, SAP-notitie [1999351], die extra informatie over probleemoplossing voor verbeterde Azure-bewaking voor SAP heeft.

Als u problemen oplossen met behulp van SAP-notitie [1999351] niet los het probleem moet opnieuw de `Set-AzureRmVMAEMExtension` configuratiescript zoals beschreven in [configureren van de Azure verbeterde extensie Monitoring voor SAP][deployment-guide-4.5]. Mogelijk moet wachten op een uur omdat storage analytics of diagnostics items mogelijk niet worden gemaakt, onmiddellijk nadat ze zijn ingeschakeld. Als het probleem zich blijft voordoen, open een SAP customer support bericht op het onderdeel BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een virtuele Linux-machine.
