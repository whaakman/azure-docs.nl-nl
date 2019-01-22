---
title: SQL Server Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP | Microsoft Docs
description: SQL Server Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 78ad40796a31e0c803b892e0c1b50e66b32c2b0a
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425873"
---
# <a name="sql-server-azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>SQL Server Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver

[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
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
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide_general.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms_guide_sqlserver.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

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

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f 
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c 
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef 
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a 
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 

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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam 
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
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
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
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
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/eresources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
[virtual-network-deploy-multinic-arm-cli]:../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-network-deploy-multinic-classic-ps.md
[virtual-networks-nsg]:../../../virtual-network/security-overview.md
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




In dit document bevat informatie over de verschillende onderdelen om te overwegen bij het implementeren van SQL Server voor SAP-workloads in Azure IaaS. Als een voorwaarde in dit document hebt lees het document [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) evenals andere handleidingen in de [SAP-workloads op Azure-documentatie](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 



> [!IMPORTANT]
> Het bereik van dit document is de Windows-versie van SQL Server. De Linux-versie van SQL Server met een van de SAP-software wordt niet ondersteund door SAP. Het document is geen Microsoft Azure SQL Database, bespreken die is een Platform als een Service-aanbieding van de Microsoft Azure-Platform. De discussie in dit artikel is over het uitvoeren van de SQL Server-product als oplossing staat bekend om on-premises implementaties in Azure Virtual Machines, gebruik te maken van de infrastructuur als een Service-mogelijkheden van Azure. Database-functies en functionaliteiten tussen deze twee aanbiedingen zijn verschillend en moeten niet worden verward met elkaar. Zie ook: <https://azure.microsoft.com/services/sql-database/>
> 
>

In het algemeen moet u rekening houden met behulp van de meest recente SQL Server-versies voor het uitvoeren van SAP-workloads in Azure IaaS. De meest recente versies van SQL Server bieden betere integratie in enkele van de Azure-services en -functionaliteit. Of over de wijzigingen die in een Azure IaaS-infrastructuur optimaliseren.

Het verdient aanbeveling om te controleren [dit] [ virtual-machines-sql-server-infrastructure-services] documentatie voordat u doorgaat.

In de volgende secties worden onderdelen van onderdelen van de documentatie onder de bovenstaande koppeling samengevoegd en die worden vermeld. Specifieke informatie over SAP worden ook vermeld en enkele concepten in meer detail worden beschreven. Echter is het raadzaam om te werken door de documentatie boven eerste voordat u de documentatie van SQL Server-specifieke leest.

Er is een SQL-Server in IaaS specifieke gegevens die u voordat u doorgaat weten moet:

* **Ondersteuning voor de versie van SQL**: Voor SAP-klanten, SQL Server 2008 R2 en hoger wordt ondersteund op Microsoft Azure-Machine. Eerdere versies worden niet ondersteund. Bekijk deze algemene [ondersteuningsverklaring](https://support.microsoft.com/kb/956893) voor meer informatie. In het algemeen, wordt SQL Server 2008 door Microsoft ook ondersteund. Maar vanwege een belangrijke functionaliteit voor SAP, die is geïntroduceerd in SQL Server 2008 R2, SQL Server 2008 R2 de minimale versie voor SAP is. In het algemeen moet u rekening houden met behulp van de meest recente SQL Server-versies voor het uitvoeren van SAP-workloads in Azure IaaS. De meest recente versies van SQL Server bieden betere integratie in enkele van de Azure-services en -functionaliteit. Of over de wijzigingen die in een Azure IaaS-infrastructuur optimaliseren. Het document is daarom beperkt tot de SQL Server 2016 en SQL Server 2017.
* **SQL-prestaties**: Microsoft Azure gehoste virtuele Machines goed in vergelijking met andere openbare cloud virtualization aanbiedingen, maar de afzonderlijke resultaten uitvoeren kunnen variëren. Zie het artikel [aanbevolen procedures voor prestaties voor SQL Server in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).
* **Met behulp van installatiekopieën van Azure Marketplace**: De snelste manier om een nieuwe Microsoft Azure-VM implementeren is het gebruik van een installatiekopie van de Azure Marketplace. Er zijn in de Azure Marketplace-installatiekopieën die de meest recente versies van SQL Server bevatten. De afbeeldingen waarop SQL Server al is geïnstalleerd worden niet onmiddellijk gebruikt voor SAP NetWeaver-toepassingen. De reden is dat de standaardsortering van SQL Server is geïnstalleerd in de afbeeldingen en niet de sortering die zijn vereist voor SAP NetWeaver-systemen. Als u wilt gebruiken die afbeeldingen, controleert u de stappen die zijn beschreven in het hoofdstuk [met behulp van een installatiekopie van SQL Server buiten de Microsoft Azure Marketplace][dbms-guide-5.6]. 


## <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Aanbevelingen voor virtuele machine/VHD-structuur voor SAP-gerelateerde SQL Server-implementaties
In overeenstemming met de algemene beschrijving uitvoerbare bestanden van SQL Server moet zich bevindt of in het systeemstation van de besturingssysteemschijf van de virtuele machine geïnstalleerd (station C:\).  De meeste van de databases van SQL Server system zijn normaal gesproken niet gebruikt op hoog niveau voor SAP NetWeaver-werkbelasting. Als gevolg hiervan kunnen het systeemdatabases van SQL Server (master, msdb en model) blijven op het station C:\ ook. Een uitzondering moet tempdb, die in het geval van SAP-workloads, hogere gegevensvolume of volume voor i/o-bewerkingen vereist. I/o-werkbelasting die niet moet worden toegepast op de VHD met het besturingssysteem. Voor dergelijke systemen, moeten de volgende stappen worden uitgevoerd:


* Met alle SAP gecertificeerde VM-typen (Zie SAP-notitie [1928533]), met uitzondering van A-serie VM's, tempdb-gegevens en logboekbestanden kunnen worden geplaatst op de schijf niet-persistente D:\. 
* Het verdient echter gebruik van meerdere tempdb-gegevensbestanden. Let erop D:\ stationsvolumes verschillen op basis van het type virtuele machine. Raadpleeg het artikel voor exacte grootte van het station D:\ van de verschillende virtuele machines, [grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

Deze configuraties inschakelen tempdb te verbruiken meer ruimte dan het systeemstation is kunnen bieden. De niet-permanente D:\ station biedt ook hogere i/o-latentie en doorvoer (met uitzondering van de A-serie VM's). Om te bepalen van de grootte van de juiste tempdb, kunt u de tempdb-grootten op bestaande systemen te controleren. 

>[!NOTE]
> Als u tempdb-gegevensbestanden en het logboekbestand plaats in een map op station D:\ die u hebt gemaakt, moet u om ervoor te zorgen dat de map nadat een virtuele machine opnieuw is opgestart bestaat. Omdat het station D:\ is opnieuw worden geïnitialiseerd nadat een virtuele machine opnieuw opstarten worden alle bestanden en mappen structuren gewist. Een mogelijkheid uiteindelijke mapstructuren op station D:\ opnieuw maken voordat u het begin van de SQL Server-service wordt gedocumenteerd in [in dit artikel](http://www.sqlserver.co.uk/index.php/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/).

Een VM-configuratie, die SQL Server wordt uitgevoerd met een SAP-database en waarin het tempdb-gegevensbestanden en tempdb-logboekbestand worden geplaatst op het station D:\ zou er als volgt uitzien:

![Diagram van eenvoudige VM-schijfconfiguratie voor SQL Server](./media/dbms_sqlserver_deployment_guide/Simple_disk_structure.PNG)

In het bovenstaande diagram wordt een eenvoudig voorbeeld weergegeven. Als op in het artikel eluded [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md), number, en grootte van Premium Storage-schijven is afhankelijk van verschillende factoren. Maar in het algemeen raden we aan:

- Met opslagruimten om te vormen van één of een klein aantal volumes, die de SQL Server-gegevensbestanden bevat. De reden achter deze configuratie is dat zich in de praktijk talrijke SAP-databases met verschillende grootte databasebestanden met verschillende i/o-werkbelasting.
- Met behulp van opslagruimten op te geven van voldoende IOPS en voor het logboekbestand van de SQL Server-transactie. Mogelijke IOPS-workload is vaak de leidende lijn voor de grootte van het logboekvolume transactie en niet de potentiële volume van het transactievolume tijdens SQL Server
- Gebruik de D:\drive voor tempdb zolang de prestaties goed genoeg is. Als de algemene workload in de prestaties wordt beperkt door tmepdb dat zich op station D:\ moet u mogelijk rekening houden met tempdb te scheiden van Premium Storage-schijven, zoals aanbevolen in verplaatsen [in dit artikel](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).


### <a name="special-for-m-series-vms"></a>Speciale voor M-serie VM 's
Voor Azure-VM uit de M-serie, kan de latentie schrijven naar het transactielogboek worden teruggebracht door factoren, vergeleken met Azure Premium Storage-prestaties bij het gebruik van Azure Write Accelerator. Daarom moet u Azure Write Accelerator implementeren voor de virtuele harde schijven die het volume voor de SQL Server-transactielogboek vormen. Meer informatie vindt u in het document [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).
  

### <a name="formatting-the-disks"></a>De schijven formatteren
Voor SQL Server moet de grootte van het NTFS-blok voor schijven met SQL Server-gegevens en logboekbestanden bestanden 64KB. Er is niet nodig om de schijf D:\ te formatteren. Dit station wordt vooraf ingestelde geleverd.

Om ervoor te zorgen dat het terugzetten of het maken van databases is niet tijdens de initialisatie van de bestanden door het licht van de inhoud van de bestanden, moet u ervoor zorgen dat de context van de gebruiker die de SQL Server-service wordt uitgevoerd in een bepaalde machtiging heeft. Meestal hebben gebruikers in de groep Windows-beheerder deze machtigingen. Als de SQL Server-service wordt uitgevoerd in de gebruikerscontext van niet - Windows-beheerder, moet u het recht voor de gebruiker toewijzen **onderhoudstaken op volume uitvoeren**.  Zie de details in deze Microsoft Knowledge Base-artikel: <https://support.microsoft.com/kb/2574695>

### <a name="impact-of-database-compression"></a>Gevolgen van het database-compressie
In configuraties waar i/o-bandbreedte een beperkende factor kan worden, kan elke meting, waardoor IOP's helpen om te vergroten van de werkbelasting een kan worden uitgevoerd in een IaaS-scenario, zoals Azure. Daarom als nog niet gedaan, wordt toepassen van de pagina SQL Server-compressie aanbevolen door zowel Microsoft als SAP voordat u een bestaande SAP-database uploadt naar Azure.

De aanbeveling om uit te voeren van compressie van de Database voordat u uploadt naar Azure wordt gegeven uit twee redenen:

* De hoeveelheid gegevens moet worden geüpload is lager.
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat een sterkere hardware met meer CPU's of hogere i/o-bandbreedte of minder i/o latentie on-premises gebruiken kunt.
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Database compressie werkt ook in een Azure Virtual Machines als on-premises. Voor meer informatie over hoe u voor het comprimeren van bestaande SAP NetWeaver SQL Server-databases, controleert u het artikel [SAP verbeterde compressiehulpprogramma MSSCOMPRESS](https://blogs.msdn.microsoft.com/saponsqlserver/2016/11/25/improved-sap-compression-tool-msscompress/). 

## <a name="sql-server-2014-and-more-recent---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 en meer recente - opslag-databasebestanden rechtstreeks in Azure Blob Storage
SQL Server 2014 en latere releases open de mogelijkheid voor het opslaan van bestanden rechtstreeks in Azure Blob Store zonder de 'wrapper' van een VHD rond deze database. Met name bij het gebruik van standaard Azure Storage of kleinere VM-typen kunt dit type implementatie scenario's waarin u kunt strijden tegen de beperkingen van de IOPS dat zou worden afgedwongen door een beperkt aantal schijven die aan bepaalde kleinere VM-typen kunnen worden gekoppeld. Deze manier van implementatie werkt voor gebruikersdatabases echter niet voor systeemdatabases van SQL Server. Het werkt ook voor gegevens en logboekbestanden van SQL Server. Als u wilt implementeren van een SAP SQL Server-database op deze manier in plaats van 'wrapping' naar VHD's, houd er rekening mee:

* Het Opslagaccount gebruikte moet zich in dezelfde Azure-regio bevinden als de naam die wordt gebruikt voor het implementeren van de virtuele machine SQL-Server wordt uitgevoerd in.
* Overwegingen met betrekking tot eerder vermelde met betrekking tot de distributie van VHD's die via andere Azure Storage-Accounts gelden voor deze methode ook implementaties. Betekent dat het aantal i/o-bewerkingen op basis van de grenzen van de Azure Storage-Account.
* In plaats van boekhouding op van de VM-opslag i/o-quotum, zal het verkeer op basis van de storage-blobs die de bestanden van de gegevens en logboekbestanden voor SQL Server, worden verwerkt in de bandbreedte van de virtuele machine van het specifieke VM-type. Voor netwerk- en bandbreedte van een bepaald type van de virtuele machine, moet u het artikel [grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
* Als gevolg van i/o via het netwerk quotum pushen, moet u het opslagquotum zijn voornamelijk gestrand en gebruiken met die de totale bandbreedte van de virtuele machine slechts gedeeltelijk.
* De IOPS en i/o doorvoer prestatiedoelen met Azure Premium Storage voor de verschillende schijfgrootten niet meer van toepassing. Zelfs als de blobs die u hebt gemaakt, bevinden zich in Azure Premium Storage. De doelen worden beschreven het artikel [High-performance Premium Storage en beheerde schijven voor virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage#scalability-and-performance-targets). Als gevolg van het plaatsen van SQL Server-gegevensbestanden en logboekbestanden rechtstreeks op blobs die zijn opgeslagen op Azure Premium Storage, kunnen de prestatiekenmerken afwijken in vergelijking met VHD's op Azure Premium Storage.
* Hostcache in op basis van als beschikbaar voor Azure Premium Storage-schijven is niet beschikbaar bij het plaatsen van SQL Server-gegevensbestanden rechtstreeks op de Azure-blobs.
* Op virtuele machines uit de M-serie, worden niet Azure Write Accelerator gebruikt voor de ondersteuning van onder de milliseconde schrijfbewerkingen ten opzichte van het logboekbestand van de SQL Server-transactie. 

Details van deze functionaliteit kunnen worden gevonden in het artikel [SQL Server-gegevensbestanden in Microsoft Azure](https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure?view=sql-server-2017)

Aanbeveling voor productiesystemen is om te voorkomen dat deze configuratie en in plaats daarvan kiest u de plaatsingen van SQL Server-gegevens en logboekbestanden in Azure Premium Storage VHD's in plaats van rechtstreeks op de Azure-blobs.


## <a name="sql-server-2014-buffer-pool-extension"></a>De Buffergroepuitbreiding van SQL Server 2014
SQL Server 2014 geïntroduceerd een nieuwe functie, genaamd [Buffergroepuitbreiding](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension?view=sql-server-2017). Deze functionaliteit wordt uitgebreid voor de buffergroep van SQL Server, die wordt opgeslagen in het geheugen met een tweede niveau cache die wordt ondersteund door de lokale SSD van een server of virtuele machine. De buffergroepuitbreiding kunt houden van een grotere werkset van gegevens 'in het geheugen'. In vergelijking met toegang tot Azure Standard-opslag is de toegang tot de extensie van de buffergroep die is opgeslagen op lokale SSD van een virtuele machine van Azure veel factoren sneller. Vergelijken van de Buffergroepuitbreiding met Azure Premium Storage-lezen Cache, zoals aanbevolen voor SQL Server-gegevensbestanden, worden geen grote voordelen voor Buffergroepuitbreidingen verwacht. Reden is dat beide caches (SQL Server-Buffergroepuitbreiding en Premium Storage-lezen Cache) van de lokale schijven van de Azure compute-knooppunt gebruikmaakt.

Ervaringen in de tussentijd opgedaan met SQL Server de Buffergroepuitbreiding met SAP-werkbelasting gemengd is en u nog steeds niet kunt wissen aanbevelingen op of moet worden gebruikt in alle gevallen. Het ideale geval is dat de werkset die voor de SAP-toepassing moet hoofdgeheugen past. Met Azure in de tussentijd zorgen met virtuele machines die worden geleverd met maximaal 4 TB aan geheugen, moet het haalbare te houden van de werkset in het geheugen zijn. Daarom het gebruik van de Buffergroepuitbreiding is beperkt tot sommige zeldzame gevallen en mag geen een algemene aanvraag.  

## <a name="backuprecovery-considerations-for-sql-server"></a>Overwegingen voor back-up/herstel voor SQL Server
Wanneer u SQL Server in Azure implementeert, moet worden gecontroleerd op uw back-upmethode. Zelfs als het systeem niet een productiesysteem is, moet de SAP-database die wordt gehost door de SQL Server worden back-ups regelmatig. Omdat Azure Storage kan drie afbeeldingen voldoen, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een storage compenserende. De reden van de prioriteit voor het onderhouden van een juiste back-up en recovery-abonnement nog meer dingen die u logische/handmatige fouten compenseren kunt door punt in tijd herstel mogelijkheden te bieden. Zodat het doel om een van beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of is het gebruik van de back-ups in Azure als seeding voor een ander systeem door het kopiëren van de bestaande database. 

Om te kijken naar andere SQL-Server back-mogelijkheden in Azure Lees het artikel [back-up en herstel voor SQL Server in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery). Het artikel bevat informatie over diverse verschillende mogelijkheden.

### <a name="manual-backups"></a>Handmatige back-ups
U hebt verschillende mogelijkheden om uit te voeren 'manual' back-ups door:

1. Uitvoeren van de conventionele SQL Server back-ups op direct gekoppelde Azure-schijven. Deze methode heeft het voordeel dat u de back-ups beschikbaar snel voor systeem wordt vernieuwd hebt en van nieuwe systemen als kopieën van bestaande SAP-systemen bouwen
2.  SQL Server 2012 CU4 en hoger kunt back-up van databases naar een Azure storage-URL.
3.  Momentopname van de bestanden back-ups van databasebestanden in Azure Blob-opslag. Deze methode werkt alleen als uw SQL Server-gegevens en logboekbestanden bestanden bevinden zich in Azure blob-opslag

De eerste methode is erg bekend is en toegepast in veel gevallen in de on-premises ter wereld. Echter, krijgt u met de taak om op te lossen de langere termijn back-uplocatie. Omdat u niet dat uw back-ups behouden gedurende meer dan 30 dagen in de lokaal gekoppelde Azure Storage wilt, hebt u de noodzaak om te gebruiken Azure Backup-Services of een ander hulpprogramma van derden back-up/herstel met toegang en retentie management voor uw back-ups. Of u maken van een grote bestandsserver in Azure met behulp van Windows-opslagruimten.

De tweede methode dichter bij in het artikel wordt beschreven [SQL Server back-up naar URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017). Verschillende versies van SQL Server hebben enkele variaties in deze functionaliteit. U moet daarom de documentatie bekijken voor de controle van uw specifieke SQL Server-release. Belangrijk te weten dat dit artikel vindt u een groot aantal beperkingen. U hebt de mogelijkheid om uit te voeren van de back-up op basis van:

- Een enkele Azure pagina-blob, zodat ze worden vervolgens de grootte van de back-up op 1000 GB beperkt. Hiermee wordt ook beperkt de doorvoer die u kunt bereiken.
- Meerdere (maximaal 64) Azure blok-blobs, waarmee een theoretisch back-grootte van 12 TB. Tests met databases van klanten bleek echter dat de maximale grootte van de back-up kan niet kleiner zijn dan de theoretische limiet. In dit geval bent u verantwoordelijk voor het bewaren van back-ups en toegang o ook de back-ups beheren.


### <a name="automated-backup-for-sql-server"></a>Automatische back-up voor SQL Server
Automatische back-up biedt een automatische back-upservice voor SQL Server Standard en Enterprise-edities die worden uitgevoerd in een Windows-VM in Azure. Deze service wordt geleverd door de [SQL Server IaaS Agent-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension), die automatisch is geïnstalleerd op SQL Server Windows-installatiekopieën voor virtuele machines in Azure portal. Als u uw eigen installatiekopieën van het besturingssysteem met SQL Server is geïnstalleerd implementeert, moet u de VM-extensies afzonderlijk installeren. De stappen die nodig zijn, worden beschreven in deze [artikel](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Meer informatie over de mogelijkheden van deze methode kunnen worden gevonden in deze artikelen:

- SQL Server 2014: [Automatische back-up voor virtuele Machines (Resource Manager) voor SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup)
- SQL Server 2016/2017: [Automatische back-up v2 voor virtuele Azure-Machines (Resource Manager)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)

Zoek in de documentatie, kunt u zien dat de functionaliteit met de meest recente versies van SQL Server is verbeterd. Sommige meer informatie over SQL Server automatische back-ups worden vrijgegeven in het artikel [SQL Server Managed Backup naar Microsoft Azure](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?view=sql-server-2017). De theoretische limiet voor back-up is 12 TB.  De automatische back-ups kunnen een geschikte methode voor back-up van de grootte van maximaal 12 TB zijn. Omdat meerdere blobs naar parallel worden geschreven, kunt u verwachten dat een doorvoer van groter is dan 100 MB per seconde. 
 

### <a name="azure-backup-for-sql-server-vms"></a>Azure Backup voor SQL Server-VM 's
Deze nieuwe methode voor back-ups van SQL Server wordt vanaf juni 2018 als openbare preview aangeboden door Azure Backup-services. De methode voor het back-up van SQL Server is hetzelfde als andere hulpprogramma's van derden gebruikt, namelijk de SQL Server VSS/VDI-interface naar stream back-ups naar een doellocatie. In dit geval is de doellocatie Azure Recovery Services-kluis.

Een meer dan gedetailleerde beschrijving van deze back-upmethode, profiteren van talrijke voordelen van centrale back-configuraties, bewaking, toevoegen en beheer is beschikbaar [hier](https://docs.microsoft.com/azure/backup/backup-azure-sql-database). 


### <a name="third-party-backup-solutions"></a>Back-oplossingen van derden
Voor een groot aantal SAP-klanten is er geen mogelijkheid om te beginnen en compleet nieuwe back-oplossingen voor het deel van hun SAP-landschap dat werd uitgevoerd op Azure te introduceren. Als gevolg hiervan wordt de bestaande back-upoplossingen nodig om te worden gebruikt en uitgebreid naar Azure. Bestaande back-upoplossingen uitbreiden naar Azure meestal ook met de meeste van de belangrijkste leveranciers op dit gebied worden gewerkt. 


## <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Met behulp van een installatiekopie van SQL Server buiten de Microsoft Azure Marketplace
Microsoft biedt virtuele machines in de Azure Marketplace, die al versies van SQL Server bevatten. Voor SAP-klanten die behoefte hebben aan licenties voor SQL Server en Windows, met behulp van deze installatiekopieën mogelijk een kans om de noodzaak van licenties te zetten van virtuele machines met SQL Server is al geïnstalleerd. Als u wilt gebruiken die afbeeldingen voor SAP, moeten de volgende overwegingen worden gemaakt:

* De versies van SQL Server niet kan worden beoordeeld aan te schaffen hogere kosten dan een 'Windows-only' virtuele machine geïmplementeerd vanuit Azure Marketplace. Zie de volgende artikelen om te vergelijken prijzen: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> en <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* U kunt alleen SQL Server-versies die worden ondersteund door SAP gebruiken.
* De sortering van de SQL Server-exemplaar is geïnstalleerd in de virtuele machines die worden aangeboden in de Azure Marketplace is niet de sortering die SAP NetWeaver vereist de SQL Server-exemplaar om uit te voeren. U kunt de sortering wijzigen met de richtlijnen in de volgende sectie.

### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Wijzigen van de SQL Server-sortering van een Microsoft Windows/SQL Server-VM
Aangezien de SQL Server-installatiekopieën in de Azure Marketplace zijn niet ingesteld voor het gebruik van de sortering die nodig is voor SAP NetWeaver-toepassingen, moet onmiddellijk na de implementatie niet worden gewijzigd. Voor SQL Server, wordt deze wijziging van de sortering kunt doen met de volgende stappen uit als u de virtuele machine is geïmplementeerd en er is een beheerder aanmelden bij de geïmplementeerde virtuele machine:

* Open een Windows-opdrachtvenster als beheerder.
* Wijzig de map in C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Voer de opdracht uit: Setup.exe /QUIET /ACTION=REBUILDDATABASE /INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS=`<local_admin_account_name`> /SQLCOLLATION=SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`> is de account die is gedefinieerd als het administrator-account bij het implementeren van de virtuele machine voor het eerst via de galerie.

Het proces duurt slechts enkele minuten. Als u wilt controleren of de stap is het juiste resultaat, moet u de volgende stappen uitvoeren:

* Open SQL Server Management Studio.
* Open een queryvenster.
* Voer de opdracht sp_helpsort in de hoofddatabase van SQL Server.

Het gewenste resultaat er moet uitzien:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Als het resultaat is, stopt de implementatie van SAP en onderzoeken waarom de setup-opdracht niet werkt zoals verwacht. Implementatie van SAP NetWeaver-toepassingen naar SQL Server-exemplaar met verschillende SQL Server-codepagina's dan de hierboven genoemde is **niet** ondersteund.

## <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server hoge beschikbaarheid voor SAP in Azure
Met behulp van SQL Server in Azure IaaS-implementaties voor SAP, hebt u meerdere verschillende mogelijkheden om toe te voegen voor het implementeren van de maximaal beschikbare DBMS-laag. Zoals besproken in [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) al, Azure verschillende Sla's voor beschikbaarheid bieden voor een enkele virtuele machine en een combinatie van virtuele machines die worden geïmplementeerd in een Azure-Beschikbaarheidsset. Verondersteld wordt dat station u voor de SLA voor beschikbaarheid voor uw productie-implementaties waarvoor de implementatie in Beschikbaarheidssets van Azure. In dat geval moet u minimaal twee virtuele machines in deze een Beschikbaarheidsset te implementeren. Een virtuele machine kan het actieve exemplaar van SQL Server wordt uitgevoerd. De andere virtuele machine wordt het passieve exemplaar uitgevoerd

### <a name="sql-server-clustering-using-windows-scale-out-file-server"></a>SQL Server Clustering met behulp van Windows Scale-out bestandsserver
Met Windows Server 2016, Microsoft geïntroduceerd [Storage Spaces Direct](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview). Op basis van Storage Spaces Direct-implementatie, wordt SQL Server FCI clustering ondersteund. Meer informatie vindt u in het artikel [configureren Failover clusterexemplaar van SQL Server op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-create-failover-cluster). De oplossing vereist een Azure load balancer te maken met het virtuele IP-adres van de clusterresources. De SQL Server-databasebestanden worden opgeslagen in opslagruimten. Het is dus een gezien het feit dat u zou zijn vereist voor het bouwen van de Windows-opslagruimten op basis van Azure Premium Storage. Omdat deze oplossing is voor een te lange nog niet is ondersteund, zijn er zijn geen bekende SAP-klanten die gebruikmaken van deze oplossing in SAP productiescenario's.  

### <a name="sql-server-log-shipping"></a>De logboekbestanden van SQL Server
Een van de methoden voor hoge beschikbaarheid (HA) is upfunctie van SQL Server. Als de virtuele machines die deel uitmaken van de HA-configuratie naamomzetting werkt hebt, is er geen probleem en de instellingen in Azure verschilt niet van alle instellingen die on-premises wordt uitgevoerd. Met betrekking tot het instellen van upfunctie en de principes met betrekking tot upfunctie. Details van upfunctie van SQL Server kunnen u vinden in het artikel [over upfunctie (SQL Server)](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server?view=sql-server-2017).

De functionaliteit voor upfunctie van SQL Server is niet gebruikt in Azure voor hoge beschikbaarheid binnen één Azure-regio. Maar in de volgende scenario's SAP-klanten zijn met behulp van back-upfunctie geslaagde in combinatie met Azure:

- Herstel na noodgevallen van één Azure-regio naar een andere Azure-regio
- Configuratie van on-premises in een Azure-regio
- Overgang naar scenario's van on-premises naar Azure. In deze gevallen back-upfunctie gebruikt om te synchroniseren van de nieuwe implementatie DBMS in Azure met de continue productie system on-premises. Op het moment van knippen in productie wordt afgesloten en zorgen dat de laatste en de nieuwste transactielogboekback-ups is overgebracht naar de Azure-DBMS-implementatie wordt gemaakt. Vervolgens zijn de DBMS in Azure-implementatie geopend voor productie.  



### <a name="database-mirroring"></a>Databasespiegeling
Database spiegelen zoals ondersteund door SAP (Zie SAP-notitie [965908]) is afhankelijk van het definiëren van een failover-partner in de SAP-verbindingsreeks. Voor de gevallen Cross-Premises, gaan we ervan uit dat de twee virtuele machines zich in hetzelfde domein en dat de gebruiker context de twee SQL Server-exemplaren worden uitgevoerd onder een domein-gebruiker en beschikken over voldoende bevoegdheden in de twee SQL Server-exemplaren die betrokken zijn. De installatie van het spiegelen van de Database in Azure daarom niet verschillen tussen een typische on-premises setup/configuratie.

Vanaf de alleen-implementaties is de eenvoudigste methode dat setup met een ander domein in Azure om deze DBMS-VM's (en in het ideale geval toegewezen SAP-virtuele machines) binnen één domein.

Als een domein niet mogelijk is, kunt een certificaten ook gebruiken voor het spiegelen van de eindpunten zoals hier wordt beschreven: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Een zelfstudie voor het instellen van het spiegelen van de Database in Azure vindt u hier: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

### <a name="sql-server-always-on"></a>SQL Server Always On
Zoals altijd op voor SAP on-premises wordt ondersteund (Zie SAP-notitie [1772688]), wordt ondersteund in combinatie met SAP in Azure. Er zijn enkele speciale overwegingen voor het implementeren van de SQL Server-Listener voor beschikbaarheidsgroep (niet te verwarren met de Azure-Beschikbaarheidsset) omdat Azure op dit moment niet toe het maken van een AD-en DNS-object dat staat omdat deze mogelijk on-premises. Daarom zijn sommige stappen van de andere installatie nodig zijn om te strijden tegen het bepaald gedrag van Azure.

Er zijn enkele overwegingen met behulp van een Listener voor beschikbaarheidsgroep:

* Met behulp van een beschikbaarheidsgroep-Listener is alleen mogelijk met Windows Server 2012 of hoger als gast-OS van de virtuele machine. Voor Windows Server 2012 moet u ervoor zorgen dat deze patch wordt toegepast: <https://support.microsoft.com/kb/2854082> 
* Voor Windows Server 2008 R2, deze patch niet bestaat en altijd op moeten op dezelfde manier als het spiegelen van de Database worden gebruikt door een failoverpartner in de verbindingsreeks op te geven (gedaan via de SAP default.pfl parameter databases/mss/server - Zie SAP-notitie [965908]).
* Wanneer u een Listener voor de beschikbaarheidsgroep gebruikt, moet de Database-VM's zijn verbonden met een specifieke Load Balancer. Om te voorkomen dat Azure toewijzen van nieuwe IP-adressen in gevallen waarin beide VM's incidenteel niet worden afgesloten, een moet statische IP-adressen toewijzen aan de netwerkinterfaces van de virtuele machines in de AlwaysOn-configuratie (definiëren van een statisch IP-adres is beschreven in [dit] [ virtual-networks-reserved-private-ip] artikel)
* Er zijn speciale stappen zijn vereist wanneer het bouwen van de configuratie van het WSFC-cluster waar het cluster een speciale IP-adres moet toegewezen, omdat Azure met de huidige functionaliteit naam van het cluster hetzelfde IP-adres toewijzen zou als op het knooppunt van het cluster is gemaakt. Dit betekent dat een handmatige stap moet worden uitgevoerd als u wilt een ander IP-adres toewijzen aan het cluster.
* De beschikbaarheidsgroep-Listener is gaan in Azure worden gemaakt met de TCP/IP-eindpunten die zijn toegewezen aan de virtuele machines die de primaire en secundaire replica's van de beschikbaarheidsgroep wordt uitgevoerd.
* Er is mogelijk nodig voor het beveiligen van deze eindpunten met ACL's.

Gedetailleerde informatie over het implementeren van Always On met SQL Server in Azure-VM's bevat, zoals:

- [Maak kennis met SQL Server Always On-beschikbaarheidsgroepen op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).
- [Een AlwaysOn-beschikbaarheidsgroep configureren op Azure virtual machines in verschillende regio's](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr).
- [Een load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

>[!NOTE]
> Als u de Azure load balancer voor het virtuele IP-adres van de beschikbaarheidsgroep-listener configureert, zorg ervoor dat de DirectServerReturn is geconfigureerd. configureren van deze optie beperkt u het netwerk round trip vertraging tussen het niveau van de SAP-toepassing en de DBMS-laag. 

SQL Server Always On is de meest voorkomende gebruikte hoge beschikbaarheid en herstel na noodgevallen in Azure gebruikt voor workloadimplementaties voor SAP. De meeste klanten gebruiken altijd op voor hoge beschikbaarheid binnen één Azure-regio. Als de implementatie beperkt tot alleen twee knooppunten is, hebt u twee opties voor connectiviteit:

- Met behulp van de beschikbaarheidsgroep-Listener. Met de beschikbaarheidsgroep-Listener moet u een Azure load balancer te implementeren. Dit is meestal de standaard-methode van de implementatie. SAP-toepassingen zou worden geconfigureerd om op basis van de beschikbaarheidsgroep-listener en niet op basis van één knooppunt te verbinden
- Met behulp van de parameters van de connectiviteit van het SQL Server-Database Mirroring. In dit geval moet u de connectiviteit van de SAP-toepassingen configureren op een manier waarbij beide knooppuntnamen zijn met de naam. Meer informatie over deze een SAP configuratie aan clientzijde wordt beschreven in de SAP-notitie [#965908](https://launchpad.support.sap.com/#/notes/965908). Deze optie gebruikt, hoeft u hoeft niet te configureren van een beschikbaarheidsgroep-listener. En met die geen Azure load balancer voor de hoge beschikbaarheid van SQL Server. Als gevolg hiervan, worden de netwerklatentie tussen de SAP-toepassingslaag en de DBMS-laag is lager omdat het binnenkomende verkeer naar de SQL Server-exemplaar niet wordt doorgestuurd via de Azure load balancer. Maar intrekt, wordt deze optie werkt alleen als u uw groep beschikbaarheid twee instanties omvatten beperken. 

Een groot aantal klanten zijn gebruik te maken van de SQL Server Always On-functionaliteit voor extra herstel na noodgevallen tussen Azure-regio's. Verschillende klanten gebruiken ook de mogelijkheid om uit te voeren van back-ups van een secundaire replica. 

## <a name="sql-server-transparent-data-encryption"></a>Transparent Data Encryption voor SQL Server
Er is een aantal klanten die gebruik van SQL Server maken [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) wanneer hun SAP-SQL-Server implementeren in Azure-databases. De SQL Server TDE-functionaliteit volledig wordt ondersteund door SAP (Zie SAP-notitie [#1380493](https://launchpad.support.sap.com/#/notes/1380493)). 

### <a name="applying-sql-server-tde"></a>SQL Server TDE toepassen
In gevallen waarin u een heterogene migratie vanaf een andere DBMS-systemen uitvoert, on-premises uitgevoerd op Windows/SQL Server wordt uitgevoerd in Azure, moet u uw leeg doel in SQL Server-database vooraf maken. Als volgende stap zou u de functionaliteit van SQL Server TDE toepassen. Terwijl u nog steeds uw productie-systeem on-premises uitvoert. U wilt uitvoeren in deze reeks reden is dat heel even duren het proces van het versleutelen van de lege database voordat kan. De processen voor het importeren van SAP zouden de gegevens vervolgens importeren in de versleutelde database tijdens de fase downtime. De overhead van het importeren in een versleutelde database heeft een veel lagere invloed van de tijd dan het versleutelen van de database na de export-fase in de pijl-omlaag tijd fase. Negatieve ervaringen wanneer bij het toepassen van TDE met SAP-werkbelastingen uitgevoerd boven op de database. De aanbeveling is daarom de implementatie van TDE behandelen als een activiteit die moet worden uitgevoerd zonder de SAP-werkbelasting op de betreffende database.

In gevallen waarin u SAP SQL Server-databases van on-premises naar Azure verplaatsen, wordt u aangeraden testen op welke infrastructuur u de versleuteling snelste toegepast krijgt. Houd rekening met deze gegevens voor deze:

- U kunt geen definiëren hoeveel threads worden gebruikt voor het toepassen van versleuteling van gegevens naar de database. Het aantal threads is majorly afhankelijk van het nummer van de SQL Server-gegevens en logboekbestanden bestanden worden gedistribueerd via schijfvolumes. Betekent dat de duidelijker volumes (stationsletters), hoe meer threads wordt gelijktijdig worden ingezet om uit te voeren van de versleuteling. Een dergelijke configuratie in strijd met een beetje met eerdere schijf configuratie suggestie voor het bouwen van een of een kleiner aantal opslagruimten voor de SQL Server-databasebestanden in Azure VM's. Een configuratie met een klein aantal volumes zou leiden tot een klein aantal threads dat de versleuteling wordt uitgevoerd. Het versleutelen van een enkele thread gebieden van 64KB wordt gelezen, versleutelt deze en klikt u vervolgens een record te schrijven naar het transactielogbestand aangeeft dat de omvang is versleuteld. Als gevolg hiervan is de belasting van het transactielogboek gematigd.
- Bij oudere versies van SQL Server heeft back-upcompressie niet efficiëntie ophalen meer wanneer u uw SQL Server-database versleuteld. Dit probleem kan zich ontwikkelen in een probleem wanneer uw abonnement is voor het versleutelen van uw SQL Server-database on-premises en kopieer vervolgens een back-up naar Azure om de database herstellen in Azure. SQL Server-back-upcompressie realiseert meestal een compressieverhouding van factor 4.
- Met SQL Server 2016 introduceert SQL Server de nieuwe functionaliteit waarmee de versleutelde databases op een efficiënte manier ook het comprimeren. Zie [deze blogs](https://blogs.msdn.microsoft.com/sqlcat/2016/06/20/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases/) voor sommige details.
 
De toepassing van TDE versleuteling zonder dat er te weinig SAP werkbelasting te behandelen, moet u testen in uw specifieke configuratie op of is het beter om toe te passen TDE naar uw SAP-database on-premises of om dit te doen in Azure. In Azure, u zeker beschikt over meer flexibiliteit in termen van te veel inrichting infrastructuur en de infrastructuur te verkleinen nadat TDE is toegepast.

### <a name="using-azure-key-vault"></a>Met behulp van Azure Key Vault
Azure biedt de service van een [Key Vault](https://azure.microsoft.com/services/key-vault/) versleutelingssleutels wilt opslaan. SQL Server op de andere zijde bieden een connector gebruikmaken van Azure Key Vault als archief voor de TDE-certificaten.

Meer informatie naar Azure Key Vault gebruiken voor SQL Server TDE bevat, zoals:

- [Uitbreidbaar Sleutelbeheer met Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server?view=sql-server-2017).
- [SQL Server TDE uitbreidbaar Sleutelbeheer met Azure Key Vault - instellingsstappen](https://docs.microsoft.com/sql/relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault?view=sql-server-2017).
- [SQL Server-Connector onderhoud en probleemoplossing](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting?view=sql-server-2017).
- [Meer vragen van klanten over SQL Server Transparent Data Encryption – TDE + Azure Key Vault](https://blogs.msdn.microsoft.com/saponsqlserver/2017/04/04/more-questions-from-customers-about-sql-server-transparent-data-encryption-tde-azure-key-vault/).


>[!IMPORTANT]
>Met behulp van SQL Server TDE, met name met Azure key Vault, is het aanbevolen gebruik van de meest recente patches van SQL Server 2014, SQL Server 2016 en SQL Server 2017. Reden is dat op basis van feedback van klanten, optimalisatie en correcties is toegepast op de code. Controleer bijvoorbeeld [KBA #4058175](https://support.microsoft.com/help/4058175/tde-enabled-backup-and-restore-slow-if-encryption-key-is-stored-in-ekm).
>  

## <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Algemene SQL Server voor SAP op Azure samenvatting
Er zijn veel aanbevelingen in deze handleiding en het is raadzaam dat u lees deze meer dan één keer voordat uw Azure-implementatie plannen. In het algemeen echter worden de bovenste algemene DBMS Volg op Azure-specifieke aanbevelingen:

1. Gebruik de nieuwste versie van het DBMS-systemen, zoals SQL Server 2017, die de meeste voordelen in Azure heeft. 
2. Uw SAP-landschap dat systeem in Azure om de gegevens bestand-indeling en de Azure-beperkingen in balans zorgvuldig plannen:
   * Niet te veel schijven hebben, maar hebt u genoeg om te controleren of dat u kunt de vereiste IOPS bereiken.
   * Als u Managed Disks niet gebruikt, vergeet dan niet IOP's zijn ook beperkt per Azure Storage-Account en Storage-Accounts geldt binnen elk Azure-abonnement ([meer details][azure-subscription-service-limits]). 
   * Alleen streep over schijven als u wilt een hogere doorvoer te realiseren.
3. Nooit software te installeren of bestanden waarvoor persistentie op station D:\, omdat het niet-permanente en alles op dit station verloren gaan op een Windows opnieuw opstarten is geplaatst.
4. Gebruik geen opslaan in schijfcache voor Azure Standard-opslag.
5. Gebruik geen Azure geo-gerepliceerd Azure Standard Storage-Accounts.  Gebruik lokaal redundante voor DBMS-werkbelastingen.
6. Gebruik van de leverancier van uw DBMS HA-/ DR-oplossing voor het repliceren van gegevens uit een database.
7. Altijd naamomzetting gebruiken, Vertrouw niet alleen op IP-adressen.
8. De meest recente patches voor SQL Server met behulp van SQL Server TDE, van toepassing.
9. Gebruik de hoogste compressie op de database mogelijk. Dit is de compressie van de pagina voor SQL Server.
10. Wees voorzichtig met behulp van SQL Server-installatiekopieën op Azure Marketplace. Als u de SQL Server gebruikt, moet u de sortering van het exemplaar voordat u een SAP NetWeaver-systeem installeert op deze wijzigen.
11. Installeren en configureren van de SAP-Host-bewaking voor Azure zoals wordt beschreven in [Deployment Guide][deployment-guide].