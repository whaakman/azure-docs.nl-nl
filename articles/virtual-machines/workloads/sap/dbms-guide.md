---
title: Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver | Microsoft Docs
description: Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1046d32a0b4b6ede027ef1931314a188c64c94bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver
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

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
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

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
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
[storage-premium-storage-preview-portal]:../../../storage/common/storage-premium-storage.md
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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-networks-multiple-nics.md
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

Deze handleiding is onderdeel van de documentatie over het implementeren en de SAP-software in Microsoft Azure implementeren. Lees voordat u deze handleiding leest, de [plannen en implementatiehandleiding][planning-guide]. Dit document bevat informatie over de implementatie van verschillende relationele Database Management Systems (RDBMS) en verwante producten in combinatie met SAP op Microsoft Azure Virtual Machines (VM's) met behulp van de Azure-infrastructuur als een Service (IaaS)-mogelijkheden.

Het artikel is een aanvulling op de SAP installatie documentatie en SAP-opmerkingen primaire resources voor installaties en -implementaties van SAP-software op de opgegeven platforms vertegenwoordigen.

## <a name="general-considerations"></a>Algemene overwegingen
In dit hoofdstuk zijn overwegingen van het SAP-gerelateerde DBMS-systemen uitvoeren in Azure VM's geïntroduceerd. Er zijn enkele verwijzingen naar specifieke DBMS-systemen in dit hoofdstuk. In plaats daarvan worden de specifieke DBMS-systemen behandeld in dit artikel, na dit hoofdstuk.

### <a name="definitions-upfront"></a>Definities van tevoren
In het hele document gebruiken we de volgende termen:

* IaaS: Infrastructuur als een Service.
* PaaS: Platform als een Service.
* SaaS: Software als een Service.
* SAP-onderdeel: een afzonderlijke SAP toepassing zoals ECC, BW, oplossing Manager of EP is geplaatst.  SAP-onderdelen kunnen worden gebaseerd op traditionele ABAP of Java-technologieën of een NetWeaver op basis van toepassing zoals zakelijke objecten.
* SAP-omgeving: een of meer onderdelen voor SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke functie zoals ontwikkeling, QAS, Training, DR of productie.
* SAP liggend: Dit verwijst naar de gehele SAP-elementen in een klant IT Liggend. De SAP-liggend bevat alle productie en niet-productieve omgevingen.
* SAP-systeem: De combinatie van DBMS laag en toepassingslaag van bijvoorbeeld een SAP ERP-ontwikkelsysteem, SAP BW testsysteem, productiesysteem SAP CRM, enzovoort. In implementaties van Azure, is het niet ondersteund voor het delen van deze twee lagen tussen on-premises en Azure. Dit betekent dat er ofwel een SAP-systeem lokale geïmplementeerd of deze is geïmplementeerd in Azure. U kunt echter de verschillende systemen van een liggend SAP in Azure of on-premises implementeren. U kan bijvoorbeeld de CRM SAP-ontwikkeling implementeren en testen van systemen in Azure, maar de SAP CRM productie system on-premises.
* Cloud-implementatie: een implementatie waarbij het Azure-abonnement niet is verbonden via een site-naar-site of een ExpressRoute-verbinding met de on-premises netwerk-infrastructuur. Gemeenschappelijk Azure-documentatie dergelijke implementaties worden ook beschreven als 'Cloud-Only'-implementaties. Virtuele Machines die worden geïmplementeerd met deze methode zijn toegankelijk via het Internet en het openbare Internet eindpunten die zijn toegewezen aan de virtuele machines in Azure. De on-premises Active Directory (AD) en DNS is niet uitgebreid naar Azure in dergelijke implementaties. Daarom is de virtuele machines maken geen deel uit van de lokale Active Directory. Opmerking: Cloud-implementaties in dit document worden gedefinieerd als volledige SAP landschappen, welke on-premises in openbare cloud uitsluitend in Azure zonder extensie van Active Directory of naamomzetting worden uitgevoerd. Alleen in de cloud-configuraties worden niet ondersteund voor productie SAP-systemen of -configuraties waar SAP stm of andere lokale bronnen moeten worden gebruikt tussen SAP-systemen die worden gehost op Azure en bronnen die zich lokaal.
* Cross-Premises: Beschrijft een scenario waarin virtuele machines zijn geïmplementeerd met een Azure-abonnement met site-naar-site meerdere locaties of ExpressRoute-verbinding tussen de lokale clientresources en Azure. Gemeenschappelijk Azure-documentatie, dit soort implementaties worden ook beschreven als Cross-Premises scenario's. De reden voor de verbinding is om lokale domeinen, op de lokale Active Directory- en lokale DNS-uitbreiden naar Azure. De lokale Liggend wordt uitgebreid naar de Azure activa van het abonnement. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van het lokale domein. Domeingebruikers van het lokale domein hebben toegang tot de servers en services kunnen worden uitgevoerd op deze virtuele machines (zoals DBMS services). Communicatie en naamomzetting tussen VM's geïmplementeerd lokale en virtuele machines die zijn geïmplementeerd in Azure is mogelijk. We verwachten dat dit geldt voor de meest voorkomende scenario voor het implementeren van SAP activa in Azure. Zie voor meer informatie [in dit artikel] [ vpn-gateway-cross-premises-options] en [in dit artikel][vpn-gateway-site-to-site-create].

> [!NOTE]
> Cross-Premises implementaties van waar Azure Virtual Machines SAP computers lid van een lokaal domein zijn SAP-systemen worden ondersteund voor productie SAP-systemen. Cross-Premises configuraties worden ondersteund voor het implementeren van onderdelen of SAP landschappen in Azure te voltooien. De volledige SAP liggend zelfs worden uitgevoerd in Azure vereist dat deze VMs deel van het lokale domein en ADVERTENTIES. In eerdere versies van de documentatie besproken we hybride IT-scenario's, waarbij de term 'Hybride' verankerd in het feit ligt dat er een cross-premises-connectiviteit tussen on-premises en Azure is. In dit geval 'Hybride', betekent dit ook dat de virtuele machines in Azure deel van de lokale Active Directory uitmaken.
> 
> 

Sommige Microsoft-documentatie worden scenario's voor Cross-Premises iets anders, met name voor DBMS HA configuraties beschreven. De Cross-Premises scenario alleen koken op een site-naar-site of privé (ExpressRoute)-verbinding en het feit dat de SAP Liggend wordt in het geval van de documenten SAP-gerelateerde gedistribueerd tussen on-premises en Azure.

### <a name="resources"></a>Resources
De volgende handleidingen zijn beschikbaar voor het onderwerp van SAP-implementaties op Azure:

* [Azure virtuele Machines, planning en implementatie voor SAP NetWeaver][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP NetWeaver][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver (in dit document)][dbms-guide]

De volgende opmerkingen bij de SAP zijn gerelateerd aan het onderwerp van SAP op Azure:

| Het nummer | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine in Azure |
| [2015553] |SAP op Microsoft Azure: ondersteuning voor vereisten |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel bewaking van metrische gegevens voor SAP op Microsoft Azure |
| [1409604] |Virtualisatie in Windows: uitgebreide bewaking |
| [2191498] |SAP op Linux met Azure: uitgebreide bewaking |
| [2039619] |SAP-toepassingen op Microsoft Azure met behulp van de Oracle-Database: ondersteunde producten en versies |
| [2233094] |DB6: SAP-toepassingen in Azure met IBM DB2 voor Linux, UNIX- en Windows - aanvullende informatie |
| [2243692] |Linux op Microsoft Azure (IaaS) VM: problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [2002167] |Red Hat Enterprise Linux 7.x: installatie en Upgrade |
| [2069760] |Oracle Linux 7.x SAP-installatie en Upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |
| [2171857] |Oracle Database 12c - ondersteuning van het bestandssysteem op Linux |
| [1114181] |Oracle Database 11g - ondersteuning van het bestandssysteem op Linux |


Lees ook de [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) die alle SAP notities voor Linux bevat.

U hebt een praktische kennis over de architectuur van Microsoft Azure en hoe Microsoft Azure Virtual Machines zijn geïmplementeerd en beheerd. U vindt meer informatie op <https://azure.microsoft.com/documentation/>

> [!NOTE]
> We zijn **niet** bespreken van Microsoft Azure-Platform als een Service (PaaS)-aanbiedingen van de Microsoft Azure-Platform. Dit artikel is over het uitvoeren van een databasebeheersysteem (DBMS) in Microsoft Azure Virtual Machines (IaaS) net zoals u de DBMS in uw on-premises omgeving uitvoeren zou. Database-mogelijkheden en functies tussen deze twee aanbiedingen heel verschillend zijn en moeten niet wordt verward met elkaar. Zie ook: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Aangezien we IaaS bespreekt, zijn in het algemeen de installatie van Windows, Linux en DBMS en configuratie in wezen hetzelfde als elke virtuele machine of bare metal machine zou u on-premises installeert. Er zijn echter enkele architectuur en system management beslissingen voor de implementatie, die verschillen bij gebruik van IaaS. Het doel van dit document is om uit te leggen van de specifieke die architecturale en de system management verschillen die u moet worden voorbereid voor wanneer u IaaS.

In het algemeen zijn de algehele gebieden van verschil dat in dit artikel wordt beschreven:

* Planning van de juiste VM/schijf-indeling van SAP-systemen om te controleren of dat u de juiste gegevens hebt indeling bestands- en voldoende IOP's voor uw workload kunt bereiken.
* Netwerken overwegingen bij het gebruik van IaaS.
* Functies met specifieke database moet gebruiken om te optimaliseren van de database-indeling.
* Overwegingen voor back-up en herstel in IaaS.
* Met behulp van verschillende typen installatiekopieën voor implementatie.
* Hoge beschikbaarheid in Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Structuur van een RDBMS-implementatie
In dit hoofdstuk volgt, is het nodig om te begrijpen wat is opgegeven [dit] [ deployment-guide-3] hoofdstuk van de [Deployment Guide][deployment-guide]. Kennis over de andere VM-reeks en hun verschillen en verschillen tussen Azure Standard en Premium-opslag moet worden begrepen en bekend voordat dit hoofdstuk wordt gelezen.

Schijven met een besturingssysteem zijn tot en met maart 2015, maximaal 127 GB groot. Deze beperking is opgeheven maart 2015 (voor meer informatie controle <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). Van daaruit op schijven met het besturingssysteem kan hebben dezelfde grootte hebben als een andere schijf. Niettemin liever we nog steeds een implementatie waarbij het besturingssysteem, de DBMS en de uiteindelijke SAP-binaire bestanden staan los van de databasebestanden-structuur. We verwachten daarom SAP-systemen uitgevoerd in Azure Virtual Machines hebben de base VM (of de logische schijf), zoals geïnstalleerd met het besturingssysteem, database management system uitvoerbare bestanden en SAP uitvoerbare bestanden. De DBMS-gegevens en logboekbestanden bestanden zijn opgeslagen in Azure Storage (Standard of Premium-opslag) in afzonderlijke schijven en als logische schijven die zijn gekoppeld aan de originele installatiekopie voor besturingssysteem Azure VM. 

Afhankelijk van het gebruik van Azure Standard of Premium-opslag (bijvoorbeeld met behulp van de DS-serie of GS-serie VM's) er zijn andere quota's in Azure, die worden beschreven [hier (Linux)] [ virtual-machines-sizes-linux] en [ Hier (Windows)][virtual-machines-sizes-windows]. Bij het plannen van de schijfindeling, moet u zoeken naar de beste balans van de quota voor de volgende items:

* Het aantal gegevensbestanden worden opgeslagen.
* Het aantal schijven die de bestanden bevatten.
* De quota van de IOPS van één schijf.
* De gegevensdoorvoer per schijf.
* Het aantal extra gegevensschijven mogelijk per VM-grootte.
* Een virtuele machine in de totale opslagdoorvoer kan bieden.

Azure zorgt ervoor dat een quotum IOP's per gegevensschijf. Deze quota zijn verschillend voor schijven die worden gehost op Azure Standard-opslag- en Premium-opslag. I/o-latenties zijn ook zeer verschil is tussen de twee opslagtypen met Premium-opslag factoren leveren betere i/o-latentie. Elk van de verschillende typen van de virtuele machine heeft een beperkt aantal gegevensschijven dat u kunt koppelen. Een andere beperking is dat alleen bepaalde typen VM van Azure Premium-opslag gebruikmaken kunnen. Dit betekent dat de beslissing voor een bepaald type van de virtuele machine kan niet alleen worden veroorzaakt door de CPU en geheugen vereisten, maar ook door de IOPS latentie en schijf doorvoer vereisten die meestal worden geschaald met het aantal schijven of het type Premium-opslag-schijven. Met Premium-opslag met name kan de grootte van een schijf ook worden bepaald door het aantal IOPS en doorvoerlimieten die moet worden bereikt door elke schijf.

Het feit dat de totale frequentie van de IOPS, het aantal schijven is gekoppeld en de grootte van de virtuele machine zijn alle aan elkaar gekoppeld, kan leiden tot een Azure-configuratie van een SAP-systeem anders dan de on-premises implementatie. De limieten IOP's per LUN worden doorgaans geconfigureerd in on-premises implementaties. Terwijl deze limieten met Azure Storage vaste of zoals in Premium-opslag die afhankelijk zijn van het schijftype zijn. Dus met on-premises implementaties zien we klant configuraties van database-servers die van veel verschillende volumes voor speciale uitvoerbare bestanden, zoals SAP en de DBMS of speciale volumes voor tijdelijke databases of tabel spaties gebruikmaken. Wanneer deze een on-premises systeem wordt verplaatst naar Azure, kan dit ertoe leiden dat een afval van mogelijke IOPS bandbreedte door een schijf voor uitvoerbare bestanden of databases, voert u niet uit één of niet een groot aantal IOPS verspilling. In Azure VM's adviseren wij daarom dat de DBMS en SAP uitvoerbare bestanden worden geïnstalleerd op de besturingssysteemschijf indien mogelijk.

De plaatsing van de bestanden van de database en logboekbestanden en het type Azure-opslag gebruikt, moet worden gedefinieerd door IOPS, doorvoer-vereisten en latentie. Om voldoende IOPS aan voor het transactielogboek, wordt u mogelijk afgedwongen gebruikmaken van meerdere schijven voor het transactielogbestand of een grotere schijf voor de Premium-opslag gebruiken. In dat geval zou een bouwen een software-RAID (voor voorbeeld Windows opslag van toepassingen voor Windows of MDADM en LVM (beheer van logische Volume) voor Linux) met de schijven die met het transactielogboek bevatten.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Station D:\ in een Azure VM is een niet-persistente-station, die wordt ondersteund door een aantal lokale schijven op het Azure computerknooppunt. Omdat het niet-persistente, betekent dit dat wijzigingen in de inhoud op het station D:\ gaat verloren wanneer de virtuele machine opnieuw is opgestart. Door 'wijzigingen' dat we opgeslagen bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure Virtual machines automatisch een station koppelen aan /mnt/resource die is een niet-persistente station ondersteund door het lokale schijven op het Azure computerknooppunt. Omdat het niet-persistente, betekent dit dat wijzigingen in inhoud in /mnt/resource gaan verloren wanneer de virtuele machine opnieuw is opgestart. De wijzigingen bedoelen we opgeslagen bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> 

- - -
Afhankelijk van de Azure VM-reeks, de lokale schijven op het rekenknooppunt weergeven verschillende prestaties die kan worden ingedeeld als volgt:

* A0-A7: Zeer beperkt prestaties. Niet worden gebruikt voor alles afgezien van wisselbestand van windows
* A8-A11: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde
* D-reeks: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde
* DS-serie: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde
* G-serie: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde
* GS-serie: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde

Instructies hierboven toepast op de VM-typen die zijn gecertificeerd met SAP. De VM-reeks met uitstekende IOPS en doorvoerlimieten in aanmerking voor gebruik door sommige DBMS-functies, zoals tempdb of tijdelijke tabelruimte.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Cache voor VM's en gegevensschijven
Wanneer we gegevensschijven via de portal of wanneer we geüploade-schijven voor virtuele machines koppelen maakt, kunnen we kiezen of het i/o-verkeer tussen de virtuele machine en de schijven zich bevinden in Azure-opslag in cache zijn opgeslagen. Azure Standard en Premium-opslag kunt u twee verschillende technologieën gebruiken voor dit type van de cache. In beide gevallen is de cache zelf schijf ondersteund op de dezelfde stations die worden gebruikt door de tijdelijke schijf (D:\ in Windows) of /mnt/resource op Linux van de virtuele machine.

De mogelijke cache-typen zijn voor Azure Standard-opslag:

* Er is geen cache
* Opslaan in cache lezen
* Lezen en schrijven in cache opslaan

Om op te halen consistent en deterministisch prestaties, moet u instellen opslaan in cache op Azure Standard-opslag voor alle schijven met **gegevens met betrekking tot de DBMS-bestanden, logboekbestanden en ruimte voor tabel 'NONE'**. Het opslaan in cache van de virtuele machine kan blijven met de standaardinstellingen.

De volgende cacheopties bestaan Azure Premium-opslag:

* Er is geen cache
* Opslaan in cache lezen

Aanbeveling voor Azure Premium-opslag is voor het benutten **gegevensbestanden opslaan in cache lezen** van de SAP-database en de gekozen **niet in cache opslaan voor de schijven van de logboekbestanden**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Software-RAID
Als al hierboven, moet u een balans vinden tussen het aantal IOPS dat nodig is voor de databasebestanden over het aantal schijven die u kunt configureren en het maximumaantal IOPS op een virtuele machine van Azure biedt per schijf- of Premium-opslag schijftype. Eenvoudigste manier om te gaan met de belasting van de IOPS via schijven is voor het bouwen van een software-RAID over de verschillende schijven. Een aantal gegevensbestanden van het SAP-DBMS plaatst op de LUN's oppervlaktevariaties buiten de software-RAID. Afhankelijk van de vereisten die u wilt mogelijk het gebruik van Premium-opslag ook sinds twee van de drie verschillende Premium-opslag-schijven bieden hogere IOPS quotum dan schijven op basis van de Standard-opslag. Naast de aanzienlijk betere i/o-latentie verstrekt door Azure Premium-opslag. 

Dit geldt ook voor het transactielogboek van de verschillende DBMS-systemen. Bij veel van deze alleen toe te voegen meer Tlog bestanden helpt niet omdat de DBMS-systemen voor het schrijven naar een van de bestanden op een tijdstip alleen. Als hogere IOPS tarieven nodig zijn dan één Standard-opslag op basis van schijf kunt leveren, kunt u stripe via meerdere standaardopslag schijven of kunt u een grotere schijftype voor Premium-opslag die buiten de tarieven voor hogere IOPS ook factoren lagere latentie voor het schrijven biedt ik / Het besturingssysteem in het transactielogboek.

Situaties opgetreden in de Azure-implementaties die met behulp van een software-RAID zou voorkeur zijn:

* Logboek/opnieuw transactielogboek vereisen meer IOPS dan Azure biedt voor één schijf. Zoals hierboven vermeld dit kan worden opgelost door het bouwen van een LUN over meerdere schijven met behulp van een software-RAID.
* Ongelijke i/o-werkbelasting verdeling over de verschillende bestanden van de SAP-database. In dergelijke gevallen kan een ervaren één gegevensbestand, kunt u in plaats van te vaak door het quotum. Terwijl andere gegevensbestanden niet zelfs dicht bij het quotum van de IOPS van één schijf krijgen zijn. In dergelijke gevallen is de gemakkelijke oplossing voor het bouwen van een LUN over meerdere schijven met behulp van een software-RAID. 
* U weet niet wat de exacte i/o-werkbelasting per gegevensbestand is en alleen ongeveer weten wat de algehele IOPS werkbelasting tegen de DBMS is. Eenvoudigste te doen is het bouwen van een LUN met behulp van een software-RAID. De som van de quota van meerdere schijven achter deze LUN moet vervolgens te voldoen aan de bekende IOPS-snelheid.

- - -
> ![Windows][Logo_Windows] Windows
> 
> U wordt aangeraden Windows opslagruimten gebruiken als u op Windows Server 2012 of hoger uitvoert. Het is efficiënter dan Windows Striping van eerdere versies van Windows. Mogelijk moet u de Windows-opslaggroepen en opslagruimten maken door het PowerShell-opdrachten bij gebruik van Windows Server 2012 als besturingssysteem. De PowerShell-opdrachten vindt u hier <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Alleen MDADM en LVM (beheer van logische volumes) worden ondersteund voor het bouwen van een software-RAID op Linux. Lees de volgende artikelen voor meer informatie:
> 
> * [Configureren van Software-RAID op Linux] [ virtual-machines-linux-configure-raid] (voor MDADM)
> * [LVM configureren op een virtuele Linux-machine in Azure][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Overwegingen voor het gebruik van VM-serie, die kunnen werken met Azure Premium Storage meestal zijn:

* Verzoeken voor i/o-latentie die zich dicht bij wat SAN/NAS-apparaten bieden.
* Verzoek tot factoren betere i/o-latentie dan Azure Standard-opslag kunt leveren.
* Hogere IOP's per VM dan wat kan worden verkregen met meerdere schijven voor Standard-opslag op basis van een bepaald type van de virtuele machine.

Aangezien de onderliggende opslag van Azure elke schijf naar ten minste drie opslagknooppunten, eenvoudige RAID 0 striping kan worden gebruikt repliceert. Er is niet nodig om RAID 5- of RAID1 te implementeren.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage
Microsoft Azure Storage slaat de base VM (met OS) en de schijven of de BLOBs aan ten minste drie afzonderlijke opslagknooppunten. Wanneer u een opslagaccount of -beheerde schijven maakt, is er een keuze uit de beveiliging als volgt te werk:

![Geo-replicatie is ingeschakeld voor Azure Storage-account][dbms-guide-figure-100]

Azure Storage lokale-replicatie (lokaal Redundant) biedt bescherming tegen gegevensverlies als gevolg van de mislukking van de infrastructuur die enkele klanten verlenen kunnen om te implementeren. Zoals hierboven er zijn vier verschillende opties met een vijfde wordt een variant van een van de eerste drie. Dichter ze zoeken kunt we onderscheiden:

* **Premium lokaal redundante opslag (LRS)**: Azure Premium-opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines met I/O-intensieve werkbelastingen. Er zijn drie kopieën van de gegevens in dezelfde Azure-datacenter van een Azure-regio. De kopieën zijn in verschillende domeinen met fouten en domeinen upgraden (voor Zie concepten [dit] [ planning-guide-3.2] hoofdstuk in de [Planningshandleiding][planning-guide]). In geval van een replica van de gegevens uit de service wegens een storing in het knooppunt opslag of een storing van de schijf gaan, wordt een nieuwe replica automatisch gegenereerd.
* **Lokaal redundante opslag (LRS)**: In dit geval zijn er drie kopieën van de gegevens in dezelfde Azure-datacenter van een Azure-regio. De kopieën zijn in verschillende domeinen met fouten en domeinen upgraden (voor Zie concepten [dit] [ planning-guide-3.2] hoofdstuk in de [Planningshandleiding][planning-guide]). In geval van een replica van de gegevens uit de service wegens een storing in het knooppunt opslag of een storing van de schijf gaan, wordt een nieuwe replica automatisch gegenereerd. 
* **Geografisch redundante opslag (GRS)**: Er is In dit geval een asynchrone replicatie waarvoor een extra drie replica's van de gegevens in een andere Azure-regio, dat zich in de meeste gevallen in dezelfde geografische regio (zoals Noord-Europa en West-Europa bevindt feeds ). Dit leidt tot drie extra replica's, zodat er zes replica's in totaal zijn. Een variant van dit is een aanvulling waar de gegevens in de geografisch gerepliceerde Azure-regio kan worden gebruikt voor lezen (Read-Access Geo-Redundant).
* **Zone-redundante opslag (ZRS)**: In dit geval de drie replica's van de gegevens in dezelfde Azure-regio blijven. Zoals uitgelegd in [dit] [ planning-guide-3.1] hoofdstuk van de [Planningshandleiding] [ planning-guide] een Azure-regio een aantal datacenters dicht kan zijn. In het geval van LRS zou de replica's worden verdeeld over de verschillende datacenters waaruit een Azure-regio.

Meer informatie vindt u [hier][storage-redundancy].

> [!NOTE]
> Voor implementaties van DBMS, wordt het gebruik van geografisch redundante opslag niet aanbevolen
> 
> Azure Storage-Geo-replicatie is asynchroon. Replicatie van afzonderlijke schijven is gekoppeld aan een enkele virtuele machine zijn niet gesynchroniseerd in de stap vergrendelen. Het is daarom niet geschikt voor replicatie van de DBMS-bestanden die worden verdeeld over verschillende schijven of geïmplementeerd op basis van een software-RAID, op basis van meerdere schijven. DBMS software vereist dat de permanente schijfopslag nauwkeurig worden gesynchroniseerd binnen andere LUN's en onderliggende schijven aandrijfassen vormen. DBMS software maakt gebruik van verschillende mechanismen voor sequence IO schrijven activiteiten en een DBMS rapporteert dat de schijfopslag die is gericht op de replicatie is beschadigd als deze zelfs door enkele milliseconden verschillen. Daarom als een echt wil dat de configuratie van een database met een database gespreid over meerdere schijven geogerepliceerde, moet dergelijke replicatie worden uitgevoerd met de database weg en functionaliteit. Is niet verstandig een op Azure Storage Geo-replicatie uit te voeren van deze taak. 
> 
> Het probleem is het het gemakkelijkst om uit te leggen met een voorbeeld-systeem. Stel dat u hebt een SAP-systeem dat is geüpload naar Azure, met acht schijven met gegevensbestanden van de DBMS plus één schijf met het transactielogboek. Elk criterium van deze negen schijven hebben gegevens naar worden geschreven in een consistente methode volgens de DBMS, of de gegevens wordt geschreven naar de logboekbestanden gegevens of transacties.
> 
> In volgorde van de gegevens correct geo repliceren en onderhouden van een installatiekopie van de database consistent en de inhoud van alle negen schijven zou moeten geografisch worden gerepliceerd in de volgorde de i/o-bewerkingen zijn uitgevoerd tegen de negen verschillende schijven. Azure Storage geo-replicatie kunnen echter niet op te geven, afhankelijkheden tussen schijven. Dit betekent dat Microsoft Azure Storage geo-replicatie niet kent het feit dat de inhoud van deze negen verschillende schijven aan elkaar zijn gerelateerd en dat de gegevenswijzigingen consistent zijn alleen bij het repliceren van de i/o-bewerkingen in de volgorde is er gebeurd voor alle de negen schijven.
> 
> Naast de kans op hoog dat geogerepliceerde afbeeldingen in het scenario bieden geen een consistente database-installatiekopie, is er een prestatiestraf die wordt weergegeven met geografisch redundante opslag die ernstige problemen kunnen ook worden de prestaties beïnvloeden. Kortom, gebruik geen dit soort redundantie van gegevensopslag voor DBMS type werkbelastingen.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>VHD's wilt toewijzen aan virtuele Machine van Azure Service Storage-Accounts
In dit hoofdstuk geldt alleen voor Azure Storage-Accounts. Als u van plan bent beheerd schijven te gebruiken, zijn de beperkingen die zijn vermeld in dit hoofdstuk niet van toepassing. Lees voor meer informatie over beheerde schijven hoofdstuk [schijven beheerd] [ dbms-guide-managed-disks] van deze handleiding.

Een Azure Storage-Account is niet alleen een administratieve constructie, maar ook een onderwerp van beperkingen. Terwijl de beperkingen of we over een standaard Azure-Opslagaccount of een Azure Premium Storage-Account hebben verschillen. De exacte mogelijkheden en beperkingen worden vermeld [hier][storage-scalability-targets]

Dus voor Azure Standard-opslag is het belangrijk te weten er geldt een limiet voor de IOP's per storage-account ('Totale snelheid van aanvragen voor' die rij [het artikel][storage-scalability-targets]). Bovendien is er een initiële limiet van 100 Opslagaccounts per Azure-abonnement (van juli 2015). Daarom is het raadzaam IOP's van VM's worden verdeeld tussen meerdere opslagaccounts bij gebruik van Azure Standard-opslag. Terwijl een enkele virtuele machine in het ideale geval één opslagaccount indien mogelijk gebruikt. Als we over DBMS implementaties waarbij elke VHD die wordt gehost op Azure Standard-opslag de quotalimiet bereiken hebben kan, moet u dus alleen 30 tot 40 VHD's per Azure-Opslagaccount die gebruikmaakt van Azure Standard-opslag implementeren. Anderzijds, als u gebruikmaken van Azure Premium-opslag en voor het opslaan van grote database volumes, u mogelijk fijn in termen van IOPS. Maar er is een Azure Premium Storage-Account manier meer beperkende in gegevensvolume dan een standaard Azure-Opslagaccount. Als gevolg hiervan kunt u slechts een beperkt aantal VHD's binnen een Azure Premium Storage-Account implementeren voordat de limiet voor het volume gegevens raken. Denken aan het einde van een Azure Storage-Account als een 'virtuele SAN' die beperkte mogelijkheden in IOPS en/of capaciteit heeft. Als gevolg hiervan blijft de taak, zoals in on-premises implementaties voor het definiëren van de indeling van de VHD's van de verschillende SAP-systemen via het verschillende 'denkbeeldige SAN-apparaten' of een Azure Storage-Accounts.

Voor Azure Standard-opslag, is het niet raadzaam naar opslag presenteren uit verschillende opslagaccounts aan één VM indien mogelijk.

Wanneer u de DS- of GS-serie van Azure Virtual machines gebruikt, is het mogelijk om te koppelen VHD's uit de Azure Storage-Accounts en Premium Storage-Accounts. Gebruiksvoorbeelden zoals back-ups geschreven naar de Standard-opslag ondersteund VHD's en DBMS gegevens en logboekbestanden op de Premium-opslag worden geleverd kunt bedenken waar deze heterogene opslag kan worden gebruikt. 

Op basis van implementaties van klanten en testen van ongeveer 30 tot 40 VHD's met de gegevensbestanden van de database en logboekbestanden kunnen worden ingericht op een enkele Azure Standard-Opslagaccount met acceptabele prestaties. Zoals eerder vermeld, is de beperking van een Azure Premium Storage-Account is waarschijnlijk de gegevenscapaciteit die kan bevatten en niet IOPS.

Als met SAN-apparaten on-premises, delen, moet een aantal controleren om te detecteren uiteindelijk knelpunten in een Azure Storage-Account. De extensie Azure Monitoring voor SAP en de Azure-portal zijn hulpprogramma's die kunnen worden gebruikt voor het detecteren van bezet Azure Storage-Accounts die van suboptimale i/o-prestaties leveren kunnen.  Als deze situatie wordt gedetecteerd, wordt het aanbevolen bezet VM's verplaatsen naar een ander Azure-Opslagaccount. Raadpleeg de [Deployment Guide] [ deployment-guide] voor meer informatie over het activeren van het SAP bewakingsmogelijkheden hosten.

Een ander artikel samenvatten aanbevolen procedures om Azure Standard-opslag- en Azure Storage-Accounts vindt u hier <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Beheerde schijven
Beheerde schijven zijn een nieuwe brontype in Azure Resource Manager die kunnen worden gebruikt in plaats van VHD's die zijn opgeslagen in Azure Storage-Accounts. Beheerde schijven automatisch zijn afgestemd op de Beschikbaarheidsset van deze zijn gekoppeld aan virtuele machine en daarom verhoogt de beschikbaarheid van uw virtuele machine en de services die worden uitgevoerd op de virtuele machine. Lees voor meer informatie de [overzichtsartikel](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

SAP ondersteunt momenteel alleen Premium-schijven worden beheerd. Lees SAP-notitie [1928533] voor meer informatie.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>DBMS virtuele machines van Azure Standard-opslag naar Azure Premium-opslag verplaatsen geïmplementeerd
We er wel een aantal scenario's waar u als klant wilt verplaatsen van een geïmplementeerde virtuele machine van Azure Standard-opslag naar Azure Premium-opslag optreden. Als de schijven zijn opgeslagen in Azure Storage-Accounts, is dit niet mogelijk zonder de gegevens fysiek worden verplaatst. Er zijn verschillende manieren om het doel te bereiken:

* Alle VHD's, basis-VHD, evenals gegevens VHD's kan u gewoon kopiëren naar een nieuwe Azure Premium Storage-Account. Vaak u hebt ervoor gekozen het nummer van VHD's in Azure Standard-opslag niet vanwege het feit dat u het gegevensvolume nodig. Maar u zoveel VHD's vanwege de IOPS nodig. Nu dat u naar Azure Premium-opslag verplaatst kan u gaan manier minder VHD's voor dezelfde IOPS doorvoer wordt gehaald. Gezien het feit dat in Azure Standard-opslag u voor de gebruikte gegevens en niet de nominaal schijfgrootte betaalt, het aantal virtuele harde schijven niet echt belangrijk in termen van kosten. Echter, met Azure Premium Storage zou betaalt u voor de nominaal schijfgrootte. De meeste klanten wilt daarom Houd het aantal virtuele harde schijven van Azure Premium-opslag op het nummer dat nodig is voor de doorvoer IOP's realiseren nodig. Ja, de meeste klanten bepalen op basis van de manier van een eenvoudige 1:1 kopiëren.
* Als u nog niet is gekoppeld, kunt u een één-VHD met een databaseback-up van uw SAP-database koppelen. Na de back-up, alle VHD's, met inbegrip van de VHD met de back-up te ontkoppelen en de basis-VHD en de VHD met de back-up kopiëren naar een Azure Premium Storage-account. U zou vervolgens Implementeer de virtuele machine op basis van de basis-VHD en de VHD koppelen met de back-up. U kunt nu extra leeg Premium opslagschijven maken voor de virtuele machine die worden gebruikt voor het herstellen van de database in. Hierbij wordt ervan uitgegaan dat de DBMS kunt u paden naar de gegevens en logboekbestanden bestanden wijzigen als onderdeel van het herstelproces.
* Een andere mogelijkheid is een variatie van het vorige proces, waar u alleen de back-up van de VHD naar Azure Premium-opslag te kopiëren en deze te koppelen op basis van een virtuele machine die u zojuist hebt geïmplementeerd en geïnstalleerd.
* De vierde mogelijkheid kiest u wanneer u nodig bent het aantal gegevensbestanden van de database te wijzigen. In dat geval voert u een SAP homogene system kopie export/import te gebruiken. Opslag die bestanden naar een VHD die is gekopieerd naar een Azure Premium Storage-Account en deze te koppelen aan een VM die u gebruikt exporteren voor het uitvoeren van de processen voor het importeren. Klanten gebruiken deze mogelijkheid, vooral wanneer ze willen Verminder het aantal gegevensbestanden worden opgeslagen.

Als u schijven beheerd gebruikt, kunt u migreren naar Premium-opslag door:

1. De virtuele machine ongedaan
2. Indien nodig, het formaat van de virtuele machine een grootte die ondersteuning biedt voor Premium-opslag (bijvoorbeeld DS of GS)
3. Het type schijf beheerd account Premium (SSD) wijzigen
4. De virtuele machine starten

### <a name="deployment-of-vms-for-sap-in-azure"></a>Implementatie van virtuele machines voor SAP in Azure
Microsoft Azure biedt verschillende manieren om virtuele machines en gekoppelde schijven te implementeren. Daardoor is het belangrijk te begrijpen van de verschillen omdat voorbereidingen van de virtuele machines afhankelijk van de manier van implementatie verschillen. In het algemeen kijken we naar de scenario's beschreven in de volgende hoofdstukken.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Implementatie van een virtuele machine uit Azure Marketplace
U wilt Microsoft of derden opgegeven installatiekopie vanuit Azure Marketplace voor het implementeren van uw virtuele machine. Wanneer u uw virtuele machine in Azure hebt geïmplementeerd, volgt u dezelfde richtlijnen en hulpprogramma's voor het installeren van de SAP-software in uw virtuele machine zoals u in een on-premises omgeving doen zou. Voor het installeren van de SAP-software in de virtuele machine in Azure, SAP en Microsoft het beste uploaden en opslaan van de installatiemedia SAP in schijven of voor het maken van een virtuele machine van Azure werkt als een 'bestandsserver', die alle de benodigde SAP-installatiemedia bevat.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Een virtuele machine met een algemene installatiekopie klantspecifieke implementeren
Als gevolg van het patchbestand vereisten met betrekking tot uw versie van besturingssysteem of DBMS, kunnen de opgegeven afbeeldingen in Azure Marketplace niet aansluiten bij uw behoeften. Daarom moet u mogelijk een virtuele machine maken met uw eigen 'persoonlijke' OS/DBMS VM-installatiekopie, die verschillende keren daarna kan worden geïmplementeerd. Als u een 'persoonlijke' installatiekopie voorbereiden voor het kopiëren, moet het besturingssysteem op de lokale virtuele machine worden gegeneraliseerd. Raadpleeg de [Deployment Guide] [ deployment-guide] voor meer informatie over het generaliseren van een virtuele machine.

Als u al SAP-inhoud in uw lokale virtuele machine (vooral voor laag 2-systemen) hebt geïnstalleerd, kunt u de instellingen van het SAP aanpassen nadat de implementatie van de virtuele Azure-machine via het exemplaar wordt ondersteund door de SAP Software inrichting Manager procedure wijzigen (SAP-notitie [1619720]). Anders kunt u de SAP-software installeren nadat de implementatie van de Azure VM.

U kunt de inhoud ofwel vers genereren met een SAP-installatie vanaf de database inhoud die wordt gebruikt door de SAP-toepassing, of u kunt uw inhoud in Azure importeren met behulp van een VHD met een DBMS databaseback-up of door gebruik te maken van de mogelijkheden van de DBMS naar rechtstreeks backup in Microsoft Azure-opslag. In dit geval kan u ook voorbereiden VHD's met de DBMS gegevens en logboekbestanden bestanden on-premises en die als schijven vervolgens importeren in Azure. Maar de overdracht van gegevens op DBMS, die wordt geladen on-premises naar Azure via de VHD-schijven die moeten worden voorbereid lokale zou werken.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Een virtuele machine verplaatsen van on-premises naar Azure met een niet-gegeneraliseerd-schijf
U van plan bent te verplaatsen van een specifiek SAP-systeem van on-premises naar Azure (lift en shift). Dit kan worden gedaan door het uploaden van de schijf, het besturingssysteem, de SAP-binaire bestanden en binaire bestanden voor uiteindelijke DBMS bevat plus de schijven met de bestanden voor gegevens en logboekbestanden van het DBMS naar Azure. In tegenovergestelde scenario #2 hierboven, u de hostnaam, SAP-SID en SAP-gebruikersaccounts in de Azure VM zoals ze zijn geconfigureerd in de on-premises omgeving. Het generaliseren van de afbeelding is daarom niet nodig. Dit geldt vooral voor Cross-Premises-scenario's waar een deel van de SAP-liggend on-premises en delen in Azure wordt uitgevoerd.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Hoge beschikbaarheid en herstel na noodgevallen met virtuele machines in Azure
Azure biedt de volgende hoge beschikbaarheid en herstel na noodgeval (DR)-functionaliteit die gelden voor de verschillende onderdelen die we voor SAP en DBMS-implementaties gebruiken zou

### <a name="vms-deployed-on-azure-nodes"></a>Virtuele machines die worden geïmplementeerd op Azure knooppunten
De Azure-Platform biedt geen functies, zoals livemigratie voor geïmplementeerde virtuele machines. Dit betekent dat als er onderhoud nodig op een cluster waarop een virtuele machine is geïmplementeerd, de virtuele machine moet ophalen gestopt en opnieuw opgestart. Onderhoud in Azure wordt uitgevoerd met behulp van zogenaamde domeinen upgraden binnen clusters van servers. Slechts één Upgrade domein tegelijk wordt onderhouden. Tijdens een herstart is het een onderbreking van de service terwijl de virtuele machine wordt afgesloten, onderhoud wordt uitgevoerd en VM opnieuw opgestart. De meeste leveranciers van DBMS bieden echter hoge beschikbaarheid en herstel na noodgevallen functionaliteit die snel de DBMS-services op een ander knooppunt opnieuw wordt opgestart, als het primaire knooppunt niet beschikbaar is. De Azure-Platform biedt functionaliteit voor het distribueren van virtuele machines, opslag en andere Azure-services tussen domeinen upgraden om ervoor te zorgen dat gepland onderhoud of infrastructuur fouten alleen gevolgen voor een kleine subset van de virtuele machines of services hebben zou.  Met een zorgvuldige planning is het mogelijk om u te bereiken beschikbaarheid niveaus die vergelijkbaar is met het lokale infrastructuur.

Microsoft Azure-Beschikbaarheidssets zijn een logische groepering van virtuele machines of Services die ervoor zorgt dat virtuele machines en andere services worden gedistribueerd naar verschillende domeinen met fouten en -domeinen upgraden binnen een cluster zodat er zou niet meer dan één knooppunt wordt uitgeschakeld op enig punt in tijd ( gelezen[deze (Linux)] [ virtual-machines-manage-availability-linux] of [deze (Windows)] [ virtual-machines-manage-availability-windows] artikel voor meer informatie).

Het moet worden geconfigureerd door doel bij het implementeren van virtuele machines, zoals hier:

![Definitie van de Beschikbaarheidsset voor DBMS HA-configuraties][dbms-guide-figure-200]

Als we willen maken van maximaal beschikbare configuraties DBMS-implementaties (onafhankelijk van de afzonderlijke DBMS HA functionaliteit gebruikt), moet de DBMS-virtuele machines aan:

* De virtuele machines toevoegen aan de hetzelfde virtuele Azure-netwerk (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* De virtuele machines van de HA-configuratie moet ook in hetzelfde subnet. Naamomzetting tussen de verschillende subnetten is niet mogelijk in de Cloud-implementaties, alleen IP-naamomzetting werkt. Met behulp van site-naar-site- of ExpressRoute-verbindingen voor Cross-Premises implementaties, wordt een netwerk met ten minste één subnet al gebruikt. Naamomzetting wordt uitgevoerd volgens de on-premises AD-beleid en de netwerkinfrastructuur. 

[comment]: <> (MSSedusch TODO Test indien nog steeds waar in ARM)

#### <a name="ip-addresses"></a>IP-adressen
Het is raadzaam om de virtuele machines voor HA-configuraties in te stellen op een flexibele manier. Vertrouwen op IP-adressen voor het oplossen van de HA partner (s) binnen de HA-configuratie is niet betrouwbaar in Azure tenzij statische IP-adressen worden gebruikt. Er zijn twee 'Afsluiten' concepten in Azure:

* Afsluiten via Azure portal of Azure PowerShell-cmdlet Stop-AzureRmVM: In dit geval wordt de virtuele Machine wordt afgesloten en ongedaan toegewezen. Uw Azure-account is niet meer in rekening gebracht voor deze virtuele machine zodat alleen de kosten in rekening worden voor de opslag gebruikt. Als het privé IP-adres van de netwerkinterface niet statisch is, het IP-adres is vrijgegeven en deze kan niet worden gegarandeerd dat de netwerkinterface, de oude IP-adres toegewezen opnieuw na het opnieuw opstarten van de virtuele machine opgehaald. Tijdens het afsluiten omlaag uitvoeren via de Azure-portal of door het aanroepen van Stop-AzureRmVM automatisch zorgt ervoor dat de toewijzing ongedaan. Als u niet wilt dat het gebruik van machine Stop-AzureRmVM - StayProvisioned ongedaan gemaakt 
* Als u de virtuele machine van een niveau OS afsluit, kan de virtuele machine opgehaald afgesloten en niet ongedaan toegewezen. Echter, in dit geval uw Azure-account is nog steeds in rekening gebracht voor de virtuele machine, ondanks het feit dat deze afgesloten wordt. In dat geval behoudt de toewijzing van het IP-adres voor een gestopte virtuele machine. Afsluiten van de virtuele machine uit binnen niet automatisch voor dat de toewijzing.

Zelfs voor Cross-Premises-scenario's standaard betekent een afsluiten en de toewijzing ongedaan deactivering toewijzing van de IP-adressen van de virtuele machine, zelfs als het lokale beleid in de DHCP-instellingen zijn verschillend. 

* De uitzondering wordt als een een statisch IP-adres aan een netwerkinterface als toewijst beschreven [hier][virtual-networks-reserved-private-ip].
* In dat geval is het IP-adres blijft ongewijzigd, zolang de netwerkinterface wordt niet verwijderd.

> [!IMPORTANT]
> Duidelijke aanbevolen wordt om te voorkomen dat de hele implementatie eenvoudig en beheerbare, voor het instellen van de virtuele machines in een DBMS HA of DR-configuratie in Azure op een manier die er een werkende naamomzetting tussen de verschillende virtuele machines die zijn betrokken is samenwerking.
> 
> 

## <a name="deployment-of-host-monitoring"></a>Implementatie van de Host bewaking
SAP vereist voor productief gebruik van SAP-toepassingen in Azure Virtual Machines, de mogelijkheid om bewakingsgegevens van de fysieke hosts met de Azure virtuele Machines host. Een specifiek niveau van de Hostagent SAP-patch is vereist waarmee deze mogelijkheid in SAPOSCOL en SAP Host-Agent. De exacte patchniveau wordt beschreven in SAP-notitie [1409604].

Voor de details met betrekking tot implementatie van onderdelen die hostgegevens SAPOSCOL en SAP Hostagent leveren en het beheer van de levenscyclus van deze onderdelen, raadpleegt u de [Implementatiehandleiding][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Specifieke informatie naar Microsoft SQL Server
### <a name="sql-server-iaas"></a>SQL Server IaaS
Beginnen met Microsoft Azure, kunt u gemakkelijk migreren uw bestaande SQL Server-toepassingen dat is gebaseerd op Windows Server-platform voor Azure Virtual Machines. SQL Server in een virtuele Machine kunt u de totale kosten van eigendom van de implementatie, beheer en onderhoud van de breedte van bedrijfstoepassingen verminderen door eenvoudig deze toepassingen met Microsoft Azure te migreren. Met SQL Server in een virtuele Machine van Azure, kunnen beheerders en ontwikkelaars blijven gebruiken de dezelfde ontwikkeling en beheerhulpprogramma's die beschikbaar on-premises. 

> [!IMPORTANT]
> We zijn geen Microsoft Azure SQL Database bespreken die is een Platform als een Service-aanbieding van de Microsoft Azure-Platform. De bespreking van de in dit artikel is over het uitvoeren van de SQL Server-product terwijl het is bekend voor on-premises implementaties in Azure Virtual Machines, gebruik van de infrastructuur als de mogelijkheid van een Service van Azure. Database-mogelijkheden en functies tussen deze twee aanbiedingen zijn verschillend en moeten niet wordt verward met elkaar. Zie ook: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Het is raadzaam om te controleren [dit] [ virtual-machines-sql-server-infrastructure-services] documentatie voordat u doorgaat.

In de volgende secties zijn onderdelen van de documentatie bij de bovenstaande koppeling stukjes geaggregeerd en vermeld. Specificaties rond SAP worden ook vermeld en enkele concepten die worden beschreven in meer detail. Echter, het is raadzaam om te werken via de documentatie boven eerste voordat het lezen van de specifieke documentatie bij SQL Server.

Er is een aantal SQL-Server in IaaS specifieke informatie die u voordat u verdergaat weten moet:

* **Virtuele Machine SLA**: Er is een SLA voor virtuele Machines worden uitgevoerd in Azure die u kunt hier vinden: <https://azure.microsoft.com/support/legal/sla/>  
* **Ondersteuning voor SQL-versie**: voor SAP-klanten we ondersteuning voor de SQL Server 2008 R2 en hoger op Microsoft Azure virtuele Machine. Eerdere versies worden niet ondersteund. Bekijk deze algemene [ondersteuningsverklaring](https://support.microsoft.com/kb/956893) voor meer informatie. Houd er rekening mee dat in het algemeen SQL Server 2008 wordt ondersteund door Microsoft ook. Maar als gevolg van belangrijke functionaliteit voor SAP, die is geïntroduceerd met SQL Server 2008 R2, SQL Server 2008 R2 de minimale release voor SAP is. Houd er rekening mee dat SQL Server 2012 en 2014 is uitgebreid met de diepgaande integratie in het IaaS-scenario (zoals back-ups van rechtstreeks met Azure Storage). Daarom beperken we dit artikel tot de SQL Server 2012 en 2014 met de meest recente patchniveau voor Azure.
* **Ondersteuning voor SQL-functie**: meest SQL Server-functies in Microsoft Azure Virtual Machines met enkele uitzonderingen worden ondersteund. **SQL Server Failover Clustering met gedeelde schijven wordt niet ondersteund**.  Gedistribueerd technologieën zoals databasespiegeling, AlwaysOn-beschikbaarheidsgroepen, replicatie, logboekverzending en Service Broker worden ondersteund in één Azure-regio. SQL Server AlwaysOn ook wordt ondersteund tussen verschillende Azure-regio's zoals hier wordt beschreven: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Controleer de [ondersteuningsverklaring](https://support.microsoft.com/kb/956893) voor meer informatie. Een voorbeeld over het implementeren van een AlwaysOn-configuratie wordt weergegeven [dit] [ virtual-machines-workload-template-sql-alwayson] artikel. Bekijk ook de aanbevolen procedures beschreven [hier][virtual-machines-sql-server-infrastructure-services] 
* **SQL-prestaties**: We zijn ervan overtuigd dat Microsoft Azure gehoste virtuele Machines zeer goed in vergelijking met andere openbare cloud virtualization aanbiedingen, maar de afzonderlijke resultaten uitvoeren kan verschillen. Bekijk [dit] [ virtual-machines-sql-server-performance-best-practices] artikel.
* **Met behulp van installatiekopieën uit Azure Marketplace**: de snelste manier voor het implementeren van een nieuwe Microsoft Azure VM is een afbeelding te gebruiken uit Azure Marketplace. Er zijn afbeeldingen in Azure Marketplace, die SQL Server bevatten. De afbeeldingen waarop SQL Server al is geïnstalleerd worden niet onmiddellijk gebruikt voor SAP NetWeaver toepassingen. De reden is dat de standaardsortering voor de SQL Server is geïnstalleerd in deze installatiekopieën en niet de sortering die vereist zijn voor SAP NetWeaver systemen. Om dergelijke afbeeldingen gebruikt, controleert u de stappen die zijn beschreven in hoofdstuk [met de installatiekopie van een SQL Server buiten de Microsoft Azure Marketplace][dbms-guide-5.6]. 
* Bekijk [prijsinformatie](https://azure.microsoft.com/pricing/) voor meer informatie. De [SQL Server 2012-licentieverlening Guide](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) en [SQL Server 2014 licentieverlening Guide](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) zijn ook een belangrijke bron.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>SQL Server-configuratie-instructies voor SAP-gerelateerde SQL Server-installaties in Azure VM 's
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Aanbevelingen voor VM/VHD-structuur voor implementaties van SQL Server SAP-gerelateerde
In overeenstemming met de algemene beschrijving uitvoerbare programma's van SQL Server moet zich bevinden of in het systeemstation van de besturingssysteemschijf van de VM geïnstalleerd (station C:\).  Normaal gesproken worden de meeste van de SQL Server-systeemdatabases gebruikt op een hoog niveau door SAP NetWeaver werkbelasting. Daarom kunnen de systeemdatabases van SQL Server (master, msdb en model) blijven op het station C:\ ook. Een uitzondering kan tempdb die in het geval van sommige ERP SAP en alle BW werkbelastingen hoger gegevensvolume of i/o-bewerkingen volume, niet in de oorspronkelijke virtuele machine passen mogelijk vereist zijn. Voor dergelijke systemen, moeten de volgende stappen worden uitgevoerd:

* Verplaats de primaire tempdb-gegevensbestanden naar dezelfde logische schijf als de primaire gegevensbestanden van de SAP-database.
* Eventuele extra tempdb-gegevensbestanden toevoegen aan elk van de andere logische stations met een gegevensbestand van de SAP-gebruikersdatabase.
* Het logboekbestand tempdb toevoegen aan de logische schijf die de gebruikersdatabase logboekbestand bevat.
* **Uitsluitend bedoeld is voor VM-typen die gebruikmaken van de lokale SSD's** op de compute-knooppunt tempdb gegevens en logboekbestanden bestanden op het station D:\ kunnen worden geplaatst. Het kan echter aanbevolen gebruik van meerdere tempdb-gegevensbestanden. Let D:\ stationsvolumes zijn verschillend op basis van de VM-type.

Deze configuraties inschakelen tempdb gebruiken meer ruimte nodig heeft dan het systeemstation is kunnen bieden. Om te bepalen van de grootte van de juiste tempdb, kunt een controleren of de grootte van tempdb op bestaande systemen die on-premises uitgevoerd. Een dergelijke configuratie zou bovendien IOPS getallen op basis van tempdb die niet kan worden voorzien van het systeemstation inschakelen. Systemen die lokaal worden uitgevoerd kunnen opnieuw worden gebruikt voor het bewaken van i/o-werkbelasting tegen tempdb zodat u de IOPS getallen die u verwacht te zien op uw tempdb kunt afleiden.

Een VM-configuratie, die SQL Server wordt uitgevoerd met een SAP-database en waar tempdb-gegevens en tempdb logboekbestand worden geplaatst op het station D:\ eruit als:

![Verwijzing configuratie van Azure IaaS VM voor SAP][dbms-guide-figure-300]

Let op het station D:\ is afhankelijk van het VM-type van verschillende grootte. Afhankelijk van de vereiste grootte van tempdb u mogelijk geforceerd worden paar tempdb gegevens en logboekbestanden bestanden met de SAP gegevens en logboekbestanden databasebestanden in gevallen waarbij D:\ station te klein is.

#### <a name="formatting-the-disks"></a>De schijven formatteren
Voor SQL Server het NTFS blokgrootte voor schijven met SQL Server-gegevens en logboekbestanden moet 64 kB. Er is niet nodig om het station D:\ te formatteren. Dit station komt vooraf ingestelde.

Om ervoor te zorgen dat de herstel- of maken van databases voor de gegevensbestanden niet wordt geïnitialiseerd door de inhoud van de bestanden zeroing, moet een ervoor zorgen dat de context van de gebruiker die de SQL Server-service wordt uitgevoerd in een bepaalde machtiging heeft. Gebruikers in de groep van de Windows-beheerders hebben meestal deze machtigingen. Als de SQL Server-service wordt uitgevoerd in de gebruikerscontext van niet - Windows-beheerder, moet u het recht uitvoeren volumeonderhoudstaken voor de gebruiker toewijzen.  Zie de informatie in dit Microsoft Knowledge Base-artikel: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Gevolgen van het database-compressie
Elke meting, waardoor IOP's kan in configuraties waar i/o-bandbreedte kan een beperkende factor u helpen om uit te rekken van de werkbelasting een in een IaaS-scenario zoals Azure uitvoeren kunt. Daarom als u nog niet doet, wordt toepassen van de pagina met SQL Server-compressie sterk aanbevolen door Microsoft en SAP voordat u een bestaande SAP-database naar Azure uploadt.

De aanbeveling om de Database compressie uitvoeren voordat u uploadt naar Azure wordt gegeven buiten twee redenen:

* De hoeveelheid gegevens te uploaden is lager.
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat sterkere hardware kunnen worden gebruikt met meer CPU's of hogere i/o-bandbreedte of minder i/o-latentie lokale.
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Database-compressie werkt ook in een Azure Virtual Machines als lokale. Controleer hier voor meer informatie over hoe u een bestaande SAP SQL Server-database comprimeren: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014--storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014: opslag databasebestanden rechtstreeks op de Azure Blob-opslag
SQL Server 2014 Hiermee opent u de mogelijkheid om op te slaan databasebestanden rechtstreeks op de Azure Blob-Store zonder 'wrapper' van een VHD omheen. Hierdoor kunnen scenario's waarin u de grenzen van de IOPS op dat zou worden afgedwongen door een beperkt aantal schijven dat kan worden gekoppeld aan bepaalde kleinere VM-typen kan ondervangen vooral met het gebruik van standaard Azure Storage of kleinere VM-typen. Dit werkt voor gebruikersdatabases maar niet voor systeemdatabases van SQL Server. Dit werkt voor gegevens en logboekbestanden van SQL Server. Als u wilt implementeren SAP SQL Server-databases op deze manier in plaats van 'wrapping' in VHD's, moet de volgende rekening houden:

* Het Opslagaccount gebruikte moet zich in hetzelfde Azure-gebied als de naam die wordt gebruikt voor het implementeren van de VM-SQL-Server wordt uitgevoerd in.
* Overwegingen met betrekking tot eerder vermelde met betrekking tot de distributie van VHD's via andere Azure Storage-Accounts voor deze implementaties ook van toepassing. Geeft het aantal i/o-bewerkingen op de ondergrenzen van de Azure Storage-Account.

[comment]: <> (MSSedusch TODO, maar dit zal gebruiken netwerkbandbreedte en geen opslag bandbreedte, het?)

Meer informatie over dit type implementatie worden hier vermeld: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

Om te kunnen opslaan in SQL Server-gegevensbestanden rechtstreeks op Azure Premium-opslag, moet u beschikken over een minimale SQL Server 2014 patch release, die hier wordt beschreven: <https://support.microsoft.com/kb/3063054>. Opslaan van SQL Server-gegevensbestanden op Azure Standard-opslag werkt in combinatie met de uitgebrachte versie van SQL Server 2014. Zeer dezelfde patches bevat echter een andere reeks correcties, waardoor de rechtstreeks gebruik van Azure Blob-opslag voor SQL Server-gegevensbestanden en back-ups betrouwbaarder. Daarom wordt u aangeraden deze patches in het algemeen.

### <a name="sql-server-2014-buffer-pool-extension"></a>De Buffergroepuitbreiding van SQL Server 2014
SQL Server 2014 geïntroduceerd om een nieuwe functie, de Buffergroepuitbreiding wordt aangeroepen. Deze functionaliteit breidt de buffergroep van SQL Server, die in het geheugen met een tweede niveau cache dat wordt ondersteund door de lokale SSD's van een server of de virtuele machine wordt bewaard. Hierdoor een grotere werkset van gegevens behouden 'in het geheugen'. Vergeleken met het openen van Azure Standard-opslag is de toegang tot de uitbreiding van de buffergroep die wordt opgeslagen op de lokale SSD's van een Azure VM veel factoren sneller.  Gebruik van het lokale station D:\ van de VM-typen waarvoor uitstekende IOPS en doorvoerlimieten kan dus een zeer redelijke manier om de belasting van de IOPS op basis van Azure Storage te verminderen en de reactietijd van query's aanzienlijk verbeteren. Dit geldt met name wanneer u geen Premium-opslag. In geval van een Premium-opslag en het gebruik van de Cache Premium Azure lezen op het rekenknooppunt, zoals aanbevolen voor gegevensbestanden, worden er zijn geen belangrijke verschillen verwacht. Reden is dat beide caches (SQL Server-Buffergroepuitbreiding en Premium-opslag lezen Cache) van de lokale schijven van de rekenknooppunten gebruikmaakt.
Controleer voor meer informatie over deze functionaliteit in deze documentatie: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Overwegingen voor back-up/herstel voor SQL Server
Uw back-upmethode moet worden gecontroleerd bij het implementeren van SQL Server in Azure. Zelfs als het systeem niet een productieve systeem is, de SAP-database gehost door SQL Server moet een back-up regelmatig. Omdat Azure Storage drie afbeeldingen houdt, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een opslagruimte compensating. De reden van de prioriteit voor het onderhouden van een juiste back-up en herstel plan is meer die u logische/handmatige fouten compenseren kunt door punt in tijd herstelfuncties te bieden. Zodat het doel om beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of de back-ups in Azure met een ander systeem seed door te kopiëren van de bestaande database is. Bijvoorbeeld, kan u overbrengen van een 2-Tier SAP-configuratie naar een 3-Laagse system-installatie van hetzelfde systeem door een back-up herstellen.

Er zijn drie verschillende manieren met back-up van SQL Server naar Azure Storage:

1. SQL Server 2012 CU4 en hoger kan systeemeigen back-databases naar een URL. Dit wordt beschreven in de blog [nieuwe functionaliteit in SQL Server 2014 – deel 5: Backup/Restore verbeteringen](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Zie hoofdstuk [SQL Server 2012 SP1 CU4 of hoger][dbms-guide-5.5.1].
2. SQL Server-versies dan SQL 2012 CU4 kunnen een omleidingsfunctionaliteit gebruiken om back-up naar een VHD en de stroom schrijven in feite verplaatsen naar een locatie voor de Azure-opslag die is geconfigureerd. Zie hoofdstuk [SQL Server 2012 SP1 CU3 en eerdere versies][dbms-guide-5.5.2].
3. De laatste methode is het uitvoeren van een conventionele SQL Server back-up op schijfopdracht op een schijfapparaat. Dit is gelijk aan het patroon van de implementatie van lokale en niet wordt uitgebreid beschreven in dit document.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 of hoger
Deze functionaliteit kunt u rechtstreeks back-up naar Azure BLOB-opslag. Zonder deze methode maakt u moet back-up naar andere schijven die u voor schijf- en IOPS capaciteit gebruiken wilt. Het idee is in feite dit:

 ![Met behulp van SQL Server 2012 Backup naar Microsoft Azure Storage-BLOB][dbms-guide-figure-400]

Het voordeel is in dit geval dat een niet hoeft te besteden aan de schijven voor het opslaan van back-ups van SQL Server op. Zodat hebt u minder schijven die zijn toegewezen en de bandbreedte van de hele schijf die IOP 's kunnen worden gebruikt voor gegevens en logboekbestanden. Houd er rekening mee dat de maximale grootte van een back-up beperkt tot maximaal 1 TB, is zoals beschreven in de sectie 'Beperkingen' in dit artikel: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations >. Als de back-up, ondanks het gebruik van SQL Server-back-compressie zou groter zijn dan 1 TB groot, de functionaliteit die wordt beschreven in hoofdstuk [SQL Server 2012 SP1 CU3 en eerdere versies] [ dbms-guide-5.5.2] moet worden in dit document gebruikt.

[Verwante documentatie](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) met een beschrijving van het terugzetten van de databases van back-ups tegen Azure Blob-opslag kunt het beste niet terug te zetten rechtstreeks uit Azure BLOB-archief als de back-up > 25 GB. De aanbeveling in dit artikel is gewoon gebaseerd op de prestatie-overwegingen en niet als gevolg van functionele beperkingen. Daarom kunnen verschillende voorwaarden van toepassing op basis van geval tot geval.

Documentatie over hoe dit type back-up is ingesteld en gebruikt kan worden gevonden in [dit](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) zelfstudie

Een voorbeeld van de volgorde van stappen kan worden gelezen [hier](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Automatisering van back-ups, is het van hoogste belang om ervoor te zorgen dat de naam van de BLOBs voor elke back-up anders. Anders worden deze overschreven en de restore-keten is verbroken.

In de volgorde niet naar elkaar spullen tussen de drie verschillende typen back-ups van wordt u aangeraden verschillende containers onder het storage-account gebruikt voor back-ups maken. De containers kunnen alleen door virtuele machine zijn of op virtuele machine en back-up-type. Het schema kan eruitzien als:

 ![Met behulp van SQL Server 2012 Backup naar Microsoft Azure Storage-BLOB – verschillende containers onder afzonderlijke Storage-Account][dbms-guide-figure-500]

In het bovenstaande voorbeeld zou de back-ups niet worden uitgevoerd in hetzelfde opslagaccount waarin de virtuele machines zijn geïmplementeerd. Er is een nieuw opslagaccount, speciaal voor de back-ups. In de storage-accounts, zou er verschillende containers die zijn gemaakt met een matrix van het type van de back-up en de naam van de VM. Dergelijke segmentering vergemakkelijkt het beheren van de back-ups van de andere virtuele machines.

De een de back-ups, rechtstreeks schrijft BLOBs zijn niet schijven toe te voegen aan het aantal van de gegevens van een virtuele machine. Daarom kan een het maximum van gegevensschijven van de specifieke VM-SKU voor de gegevens gekoppeld maximaliseren en transactie logboekbestand en een back-up op basis van een opslagcontainer nog steeds uitvoeren. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 en eerdere versies
De eerste stap die u moet uitvoeren om een back-up rechtstreeks met Azure Storage bereiken zou worden voor het downloaden van het MSI-bestand, dat is gekoppeld aan [dit](https://www.microsoft.com/download/details.aspx?id=40740) KBA artikel.

Download de x64-installatiebestand en de documentatie. Het bestand installeert een programma met de naam: 'Microsoft SQL Server Backup naar Microsoft Azure Tool'. Lees zorgvuldig de documentatie van het product.  De tool werkt in feite in de volgende manier:

* De SQL Server-kant een schijflocatie voor de SQL Server back-up is gedefinieerd (gebruik niet het station D:\ voor deze).
* Het hulpprogramma kunt u definiëren regels, die kunnen worden gebruikt om verschillende typen back-ups naar Azure Storage-containers verschillende leiden.
* Zodra de regels voldaan is, wordt de stroom schrijven van de back-up door het hulpprogramma omgeleid naar een van de VHD's / schijven naar de locatie van Azure Storage, die eerder is gedefinieerd.
* Het hulpprogramma laat een kleine stub-bestand met een aantal KB's op de VHD/de schijf die is gedefinieerd voor de SQL Server back-up. **Dit bestand moet worden links op de opslaglocatie sinds deze opnieuw te herstellen van Azure-opslag is vereist.**
  * Als zoekgeraakt het stubbestand (bijvoorbeeld via verlies van het opslagmedium die deel uitmaakt van de stubbestand) en u ervoor de mogelijkheid om een back gekozen hebt-up naar een Microsoft Azure Storage-account, kunt u het stubbestand via Microsoft Azure Storage herstellen downloaden van de opslagcontainer waarin deze is geplaatst. U moet vervolgens de stubbestand naar een map op de lokale computer waarop het hulpprogramma is geconfigureerd om te detecteren en te uploaden naar de container met het wachtwoord voor dezelfde versleuteling als versleuteling is gebruikt met de oorspronkelijke regel plaatsen. 

Dit betekent dat het schema zoals hierboven beschreven voor een meer recente versies van SQL Server kan worden ingevoerd ook voor SQL Server-versies die zijn niet toestaan direct adres een Azure-opslaglocatie.

Deze methode mag niet worden gebruikt bij een meer recente versies van SQL Server, die ondersteuning bieden voor back-ups maken van systeemeigen tegen Azure Storage. Uitzonderingen zijn waar beperkingen van de systeemeigen back-up naar Azure blokkeren het systeemeigen back-up kan worden uitgevoerd in Azure.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Andere mogelijkheden voor back-up van SQL Server-databases
Andere mogelijkheden voor back-databases is extra gegevensschijven koppelen aan een virtuele machine die u gebruikt voor het opslaan van back-ups op. In dat geval moet u ervoor zorgen dat de schijven worden niet uitgevoerd volledig. Als dit het geval is, moet u zou Ontkoppel de schijven en dus tot speak deze ' archiveren en vervang deze door een nieuwe lege schijf. Als u falen dat pad beide, die u wilt voorkomen dat deze VHD's in afzonderlijke Azure Storage-Accounts die die de VHD's met de databasebestanden.

Een tweede mogelijkheid is het gebruik van een grote virtuele machine waarvoor veel schijven die zijn gekoppeld, kan bijvoorbeeld een D14 met 32VHDs. Opslagruimten gebruiken voor het bouwen van een flexibele omgeving waarin u shares die vervolgens worden gebruikt als back-doelen voor de verschillende DBMS-servers kan samenstellen.

Enkele aanbevolen procedures zijn gedocumenteerd [hier](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) ook. 

#### <a name="performance-considerations-for-backupsrestores"></a>Prestatie-overwegingen voor back-ups/herstelacties
Prestaties van back-up/herstel is net zoals in de bare-metal implementaties, afhankelijk van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes zijn. Bovendien speelt de CPU-verbruik door back-compressie gebruikt een belangrijke rol op virtuele machines met slechts maximaal acht CPU-threads. Daarom kan een aannemen:

* De minder het aantal schijven gebruikt voor het opslaan van de gegevens bestanden, hoe kleiner de totale doorvoer in lezen.
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de invloed van back-compressie.
* De minder doelen (BLOBs, VHD's of schijven) voor het schrijven van de back-up naar de kleinste de doorvoer.
* Hoe kleiner de virtuele machine grootte, hoe kleiner de doorvoer opslaglimiet schrijven en te lezen van Azure Storage. Onafhankelijk van of de back-ups rechtstreeks op Azure Blob zijn opgeslagen of dat deze zijn opgeslagen in VHD's die opnieuw zijn opgeslagen in Azure Blobs.

Wanneer u een Microsoft Azure Storage-BLOB gebruikt als het back-updoel in recentere versies, bent u beperkt tot het toewijzen van slechts één URL-doel voor elke specifieke back-up.

Maar als u de 'Microsoft SQL Server back-up naar Microsoft Azure-hulpprogramma' in oudere versies, kunt u meer dan één bestand-doel definiëren. De back-up kan worden geschaald met meer dan één doel en de doorvoer van de back-up hoger is. Dit zou vervolgens leiden tot meerdere bestanden ook in de Azure Storage-account. Bij onze tests met behulp van meerdere bestand bestemmingen een absoluut de doorvoer, wat een met de back-extensies bereiken kan kunt bereiken in geïmplementeerd van SQL Server 2012 SP1 CU4 op. U worden ook niet geblokkeerd door de limiet van 1TB in de systeemeigen back-up naar Azure.

Houd er rekening mee echter, de doorvoer is afhankelijk van de locatie van de Azure Storage-Account die u voor de back-up gebruikt. Een idee mogelijk naar het opslagaccount niet vinden in een andere regio dan in de virtuele machines worden uitgevoerd. U zou bijvoorbeeld het uitvoeren van de VM-configuratie in West-Europa, maar plaatsen van het Opslagaccount dat u back kunt-up tegen in Noord-Europa. Die zeker gevolgen heeft voor de back-doorvoer en is niet waarschijnlijk een doorvoersnelheid van 150MB per seconde genereren als het alleen mogelijk in gevallen waar de doelopslag en de virtuele machines worden uitgevoerd in hetzelfde datacenter regionale lijkt.

#### <a name="managing-backup-blobs"></a>Back-Upblobs beheren
Er is een vereiste voor het beheren van de back-ups op uw eigen. Omdat de verwachting is dat veel blobs zijn gemaakt door het uitvoeren van regelmatige transactielogboekback-ups, kunt beheer van deze BLOB's gemakkelijk overbelast de Azure-portal. Het is daarom recommendable gebruikmaken van een Azure storage explorer. Er zijn verschillende goed beschikbaar zijn, die helpen kan bij het beheren van een Azure storage-account

* Microsoft Visual Studio met Azure SDK is geïnstalleerd (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure Opslagverkenner (<https://azure.microsoft.com/downloads/>)
* Hulpprogramma's van derden

Raadpleeg voor een uitgebreidere bespreking van de back-up en SAP op Azure [de handleiding van de back-up SAP](sap-hana-backup-guide.md) voor meer informatie.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Met behulp van een installatiekopie van SQL Server buiten de Microsoft Azure Marketplace
Microsoft biedt virtuele machines in Azure Marketplace, die al versies van SQL Server bevatten. Voor SAP-klanten die licenties voor SQL Server en Windows vereist, dit wordt mogelijk een kans om de noodzaak van licenties in feite dekken door draaiende om zo VM's met SQL Server is al geïnstalleerd. Als u wilt gebruiken dergelijke afbeeldingen voor SAP, moeten de volgende overwegingen worden gemaakt:

* De versies van SQL Server niet kan worden beoordeeld verkrijgen duurder dan slechts een 'Windows-only' virtuele machine van Azure Marketplace geïmplementeerd. Zie de volgende artikelen om te vergelijken prijzen: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> en <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* U kunt alleen SQL Server-versies worden ondersteund door SAP, zoals SQL Server 2012 gebruiken.
* De sortering van de SQL Server-exemplaar is geïnstalleerd in de virtuele machines die worden aangeboden in Azure Marketplace is niet de sortering die SAP NetWeaver vereist het SQL Server-exemplaar om uit te voeren. Hoewel met de aanwijzingen in de volgende sectie kunt u de sortering.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>De SQL Server-sortering van een virtuele machine van de Microsoft Windows/SQL-Server wijzigen
Aangezien de SQL Server-installatiekopieën in Azure Marketplace zijn niet ingesteld voor het gebruik van de sortering die wordt door SAP NetWeaver toepassingen vereist, moet deze onmiddellijk na de implementatie niet worden gewijzigd. Voor SQL Server 2012, kunt dit doen met de volgende stappen uit als u de virtuele machine is geïmplementeerd en een beheerder kan aanmelden bij de geïmplementeerde virtuele machine:

* Open een opdrachtvenster Windows 'als beheerder'.
* Wijzig de directory in C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Voer de opdracht uit: Setup.exe/quiet/Action = REBUILDDATABASE/InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION SQL_Latin1_General_Cp850_BIN2 =   
  * `<local_admin_account_name`> is de account die is gedefinieerd als het administrator-account bij het implementeren van de virtuele machine voor het eerst door de galerie.

Het proces moet alleen een paar minuten duren. Om te controleren of de stap uiteindelijk met het juiste resultaat, moet u de volgende stappen uitvoeren:

* Open SQL Server Management Studio.
* Open een queryvenster.
* De opdracht sp_helpsort uitvoeren in de SQL Server-hoofddatabase.

Het gewenste resultaat moet eruitzien als:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Als dit niet het resultaat, STOP met het implementeren van SAP en onderzoeken waarom de setup-opdracht niet werkt zoals verwacht. Implementatie van toepassingen naar SQL Server-exemplaar met een andere codepagina's van SQL Server dan de hierboven genoemde SAP NetWeaver **niet** ondersteund.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server hoge beschikbaarheid voor SAP in Azure
Zoals eerder in dit artikel wordt vermeld, is er geen mogelijkheid om aan te maken van gedeelde opslag die nodig is voor het gebruik van de functionaliteit van de oudste SQL Server-hoge beschikbaarheid. Deze functionaliteit zou twee of meer exemplaren van SQL Server installeren in een Windows Server Failover Cluster (WSFC) met een gedeelde schijf voor de gebruikersdatabases (en uiteindelijk tempdb). Dit is de methode lang standaard hoge beschikbaarheid, die ook wordt ondersteund door SAP. Omdat Azure biedt geen ondersteuning voor gedeelde opslag, kunnen niet SQL Server-configuraties voor hoge beschikbaarheid met een configuratie met een gedeelde schijf worden gerealiseerd. Veel andere methoden van hoge beschikbaarheid zijn echter nog steeds mogelijk en worden beschreven in de volgende secties.

#### <a name="sql-server-log-shipping"></a>Meld u back-ups van SQL Server
Een van de methoden van hoge beschikbaarheid (HA) is logboekverzending van SQL Server. Als de virtuele machines die deel uitmaken van de HA-configuratie naamomzetting werkt hebt, wordt er geen probleem is en de instellingen in Azure is niet het verschil tussen elke instelling die lokaal wordt uitgevoerd. Het verdient niet afhankelijk zijn van alleen IP-adresomzetting. Met betrekking tot het instellen van de back-upfunctie van logboekbestanden en de beginselen rond logboekverzending, controleert deze documentatie:

<https://docs.Microsoft.com/SQL/database-engine/log-Shipping/About-log-Shipping-SQL-Server>

Als u wilt bereiken echt een hoge beschikbaarheid, moet één voor het implementeren van de virtuele machines, die binnen een dergelijke logboekverzending configuratie binnen dezelfde Azure Beschikbaarheidsset.

#### <a name="database-mirroring"></a>Databasespiegeling
Spiegelen database zoals ondersteund door SAP (Zie SAP-notitie [965908]) is afhankelijk van het definiëren van een failoverpartner in de verbindingsreeks van SAP. De gevallen Cross-Premises nemen we aan dat de twee virtuele machines zich in hetzelfde domein en dat de gebruiker context de twee SQL Server-exemplaren worden uitgevoerd onder een domein-gebruiker voldoende rechten hebt om in de twee SQL Server-exemplaren die zijn betrokken. Daarom is de installatie van het spiegelen van de Database in Azure niet van verschillen tussen een typische on-premises setup/configuratie.

Op Cloud-implementaties is de eenvoudigste methode het installatieprogramma voor een ander domein in Azure die DBMS VMs (en in het ideale geval toegewezen SAP-virtuele machines) binnen een domein.

Als een domein niet mogelijk is, kan een ook certificaten gebruiken voor de database mirroring eindpunten, zoals hier wordt beschreven: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Een zelfstudie voor het instellen van het spiegelen van de Database in Azure vindt u hier: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQL Server AlwaysOn
Als altijd aan voor SAP on-premises wordt ondersteund (Zie SAP-notitie [1772688]), worden gebruikt in combinatie met SAP in Azure wordt ondersteund. Het feit dat u niet kunt maken van gedeelde schijven in Azure betekent niet dat een configuratie niet een altijd op Windows Server Failover Cluster (WSFC) tussen de verschillende virtuele machines maken kan. Alleen betekent dit dat u hebt geen de mogelijkheid om een gedeelde schijf gebruiken als een quorum in de configuratie van het cluster. U kunt daarom een altijd op de WSFC-configuratie in Azure bouwen en schakelt het quorumtype dat gebruikmaakt van gedeelde schijf niet. De Azure-omgeving die virtuele machines zijn geïmplementeerd los van de virtuele machines met de naam en de virtuele machines moet moet in hetzelfde domein. Dit geldt voor Azure alleen en Cross-Premises implementaties. Er zijn enkele speciale overwegingen over het implementeren van de SQL Server Beschikbaarheidsgroeplistener (niet te verwarren met de Azure-Beschikbaarheidsset) omdat Azure op dit moment niet toegestaan een AD-en DNS-object gewoon maken omdat deze mogelijk lokale. Daarom zijn sommige andere installatiestappen nodig overwinnen het specifieke gedrag van Azure.

Enkele overwegingen met behulp van een beschikbaarheidsgroep-Listener zijn:

* Met behulp van een beschikbaarheidsgroep-Listener is alleen mogelijk met Windows Server 2012 of hoger als het gastbesturingssysteem van de virtuele machine. Voor Windows Server 2012 moet u ervoor zorgen dat deze patch is toegepast: <https://support.microsoft.com/kb/2854082> 
* Voor Windows Server 2008 R2 deze patch bestaat niet en altijd op moet worden gebruikt op dezelfde manier als het spiegelen van de Database door te geven van een failoverpartner in de tekenreeks verbindingen (via het SAP default.pfl parameter databases of mss/de server: Zie SAP-notitie [965908]).
* Wanneer u een beschikbaarheidsgroep-Listener, moet de Database-virtuele machines worden verbonden met een specifieke Load Balancer. Naamomzetting in de Cloud-implementaties moet ofwel alle virtuele machines van een SAP-systeem (toepassingsservers, DBMS-server en (A) server SCS) in hetzelfde virtuele netwerk of het onderhoud van het bestand etc\host in volgorde van een SAP-toepassingslaag zou vereist ophalen van de VM-namen van de SQL Server-VM's opgelost. Om te voorkomen dat Azure toewijzen van nieuwe IP-adressen in gevallen waarin beide VM incidenteel afsluiten zijn, moet een toewijzen statische IP-adressen aan de netwerkinterfaces van die virtuele machines in de AlwaysOn-configuratie (een statisch IP-adres is beschreven in definiëren[dit] [ virtual-networks-reserved-private-ip] artikel)

[comment]: <> (Oude blogs)
[comment]: <> (< https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, < https://blogs.technet.com/b/rmilne/ Archive/2015/07/27/How-to-set-static-IP-on-Azure-VM.aspx >) 
* Er zijn speciale stappen vereist bij het bouwen van de configuratie van het WSFC-cluster waarin het cluster een speciale IP-adres moet is toegewezen, omdat Azure met de huidige functionaliteit naam van het cluster hetzelfde IP-adres toewijzen zou als het knooppunt van het cluster wordt gemaakt op. Dit betekent dat een handmatige stap moet worden uitgevoerd voor een ander IP-adres toewijzen aan het cluster.
* De Beschikbaarheidsgroeplistener zal worden gemaakt in Azure met de TCP/IP-eindpunten die zijn toegewezen aan de virtuele machines met de primaire en secundaire replica's van de beschikbaarheidsgroep.
* Er is mogelijk nodig voor het beveiligen van deze eindpunten met ACL's.

[comment]: <> (Oude TODO-blog)
[comment]: <> (De gedetailleerde stappen en de noodzakelijke items moeten meebrengen van het installeren van een AlwaysOn-configuratie op Azure zijn het best wanneer roulatie van de zelfstudie beschikbaar [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[comment]: <> (Vooraf geconfigureerde AlwaysOn-setup via de Azure-galerie < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Het maken van een beschikbaarheidsgroep-Listener is beste beschreven in de zelfstudie [this][virtual-machines-windows-classic-ps-sql-int-listener])
[comment]: <> (Beveiligen netwerkeindpunten met ACL's worden uitgelegd beste hier:)
[comment]: <> (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Het is mogelijk een SQL Server altijd op beschikbaarheidsgroep via verschillende Azure-regio's ook implementeren. Deze functie maakt gebruik van de Azure VNet-naar-Vnet-connectiviteit ([meer details][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Oude TODO-blog)
[comment]: <> (De installatie van SQL Server AlwaysOn-beschikbaarheidsgroepen in een dergelijk scenario Hier wordt beschreven: < https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Overzicht van hoge beschikbaarheid van SQL Server in Azure
Gezien het feit dat de inhoud wordt beveiligd door Azure Storage, zijn er nog een minder reden te noemen op een hot stand-by-installatiekopie. Dit betekent dat uw scenario hoge beschikbaarheid moet alleen bescherming tegen de volgende gevallen:

* Niet beschikbaar zijn van de virtuele machine als geheel vanwege onderhoud op de server-cluster in Azure of andere redenen
* Softwareproblemen met in het SQL Server-exemplaar
* Beveiligen van handmatige fout waarbij gegevens wordt verwijderd en herstel van de punt in tijd nodig is

Kijken naar overeenkomende technologieën een kunt voeren aan dat de eerste twee gevallen kunnen worden volstaan met databasespiegeling of altijd op dat het derde geval alleen kan worden volstaan met upfunctie voor logboekbestanden.

U moet een balans vinden tussen de complexere installatie van altijd op, vergeleken met het spiegelen van databases met de voordelen van altijd op. Deze voordelen kunnen worden weergegeven zoals:

* Leesbare secundaire replica's.
* Back-ups van secundaire replica's.
* Betere schaalbaarheid.
* Meer dan één secundaire replica's.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Algemene SQL Server voor SAP op Azure samenvatting
Er zijn veel aanbevelingen in deze handleiding en het is raadzaam gelezen meer dan één keer voordat u uw Azure-implementatie plannen. In het algemeen, moet u de top tien algemene DBMS Volg op Azure specifieke punten:

[comment]: <> (2.3 hogere doorvoer dan? Dan één VHD?)
1. Gebruik de meest recente release DBMS, zoals SQL Server 2014 die de meeste voordelen in Azure heeft. Dit is voor SQL Server, SQL Server 2012 SP1 CU4, waaronder de functie van de back-ups maken zich verhouden tot Azure Storage. We raden echter in combinatie met SAP zou aan ten minste CU1 voor SQL Server 2014 SP1 of SQL Server 2012 SP2 en de meest recente CU.
2. Uw SAP-systeem Liggend in Azure om de gegevensindeling van het bestand en de Azure-beperkingen in balans zorgvuldig te plannen:
   * Niet te veel schijven hebben, maar er voldoende om te controleren of dat u kunt de vereiste IOPS bereiken.
   * Als u geen beheerde schijven gebruikt, moet u IOP's zijn ook beperkt per Azure-Opslagaccount en dat Storage-Accounts beperkt in elk Azure-abonnement zijn ([meer details][azure-subscription-service-limits]). 
   * Alleen stripe naar meerdere schijven als u moet een hogere doorvoer te bereiken.
3. Nooit software te installeren of bestanden die persistentie op station D:\, vereist zoals het niet-permanente en alles op dit station op een Windows opnieuw opstarten verloren is geplaatst.
4. Gebruik geen schijfcache voor Azure Standard-opslag.
5. Gebruik geen Azure geogerepliceerde Storage-Accounts.  Lokaal Redundant voor DBMS werkbelastingen gebruiken.
6. Leverancier van uw DBMS HA-/ DR oplossing gebruiken om databasegegevens te repliceren.
7. Altijd naamomzetting gebruiken, niet vertrouwen op IP-adressen.
8. Gebruik de hoogst mogelijke database-compressie. Dit is de compressie van de pagina voor SQL Server.
9. Wees voorzichtig met behulp van SQL Server-installatiekopieën vanuit Azure Marketplace. Als u de SQL-Server een gebruikt, moet u de sortering van het exemplaar voordat u een systeem SAP NetWeaver installeert op deze wijzigen.
10. Installeer en configureer de SAP Host-bewaking voor Azure zoals beschreven in [Deployment Guide][deployment-guide].

## <a name="specifics-to-sap-ase-on-windows"></a>Foutopsporingsgegevens naar SAP-as op Windows-omgeving
Beginnen met Microsoft Azure, kunt u gemakkelijk migreren uw bestaande toepassingen voor SAP-as-omgeving op Azure Virtual Machines. SAP-as-omgeving in een virtuele Machine kunt u de totale kosten van eigendom van de implementatie, beheer en onderhoud van de breedte van bedrijfstoepassingen verminderen door eenvoudig deze toepassingen met Microsoft Azure te migreren. Met de as-SAP omgeving in een virtuele Machine van Azure, kunnen beheerders en ontwikkelaars blijven gebruiken de dezelfde ontwikkeling en beheerhulpprogramma's die beschikbaar on-premises.

Er is een SLA voor de Azure Virtual Machines, die u kunt hier vinden: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

We zijn ervan overtuigd dat Microsoft Azure gehoste virtuele Machines zeer goed werkt in vergelijking met andere openbare cloud virtualization aanbiedingen, maar de afzonderlijke resultaten kan verschillen. SAP sizing SAP's cijfers van de verschillende SAP gecertificeerd VM-SKU's vindt u in een afzonderlijke SAP-notitie [1928533].

Instructies en aanbevelingen voor het gebruik van Azure Storage, de implementatie van de SAP VM's of de SAP bewaking van toepassing op implementaties van SAP-as-omgeving in combinatie met SAP-toepassingen zoals vermeld in de eerste vier hoofdstukken van dit document.

### <a name="sap-ase-version-support"></a>Ondersteuning voor SAP-versie van de as-omgeving
SAP momenteel ondersteunt SAP-as-omgeving versie 16,0 voor gebruik met SAP Business Suite producten. Alle updates voor SAP-as-omgeving server of JDBC en ODBC-stuurprogramma's worden gebruikt met SAP Business Suite-producten worden geleverd uitsluitend via de Marketplace SAP-Service op: <https://support.sap.com/swdc>.

Als voor installaties downloaden on-premises updates voor de server SAP-as-omgeving of voor de JDBC en ODBC-stuurprogramma's rechtstreeks vanuit de Sybase-websites niet. Voor gedetailleerde informatie over patches die worden ondersteund voor gebruik met SAP Business Suite producten on-premises en in Azure Virtual Machines naar de volgende SAP-opmerkingen:

* [1590719]
* [1973241]

Algemene informatie over het uitvoeren van SAP Business Suite van SAP-as-omgeving vindt u in de [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP as-omgeving configuratie-instructies voor installatie van de SAP-gerelateerde SAP as-omgeving in virtuele machines in Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Structuur van de as-omgeving SAP-implementatie
In overeenstemming met de algemene beschrijving SAP-as-omgeving uitvoerbare bestanden moet zich bevinden of in het systeemstation van de besturingssysteemschijf van de VM geïnstalleerd (station c:\). Normaal gesproken worden de meeste van de SAP-as-omgeving system en hulpprogramma's voor databases niet echt gebruikt hard door SAP NetWeaver werkbelasting. Daarom kunnen de systeem- en hulpprogramma's voor databases (master, model, saptools, sybmgmtdb, sybsystemdb) blijven op het station C:\ ook. 

Een uitzondering kan de tijdelijke database bevat alle werktabellen en tijdelijke tabellen die zijn gemaakt door SAP as-omgeving, die in geval van een bepaalde ERP SAP en alle BW werkbelastingen hoger gegevensvolume of i/o-bewerkingen volume, niet in de oorspronkelijke VM besturingssysteem past vereist zijn schijf (station c:\).

Afhankelijk van de versie van SAPInst/SWPM gebruikt voor het installeren van het systeem, kan de database bevatten:

* Een enkele tempdb SAP-as-omgeving, die wordt gemaakt bij het installeren van SAP-as-omgeving
* Een SAP-as-omgeving tempdb gemaakt door SAP-as-omgeving en een extra saptempdb gemaakt door de SAP-installatieprogramma te installeren
* Een SAP-as-omgeving tempdb gemaakt door het installeren van SAP-as-omgeving en een extra tempdb die handmatig is gemaakt (bijvoorbeeld de volgende SAP-notitie [1752266]) om te voldoen aan specifieke tempdb ERP/BW

In geval van een specifieke ERP of alle BW-werkbelastingen, is het zinvol, met betrekking tot prestaties, zodat de tempdb-apparaten van de bovendien gemaakte tempdb (door SWPM of handmatig) op een ander station dan C:\. Als er geen aanvullende tempdb bestaat, is het raadzaam een te maken (SAP-notitie [1752266]).

Voor deze systemen moeten de volgende stappen worden uitgevoerd voor de tempdb bovendien gemaakt:

* Het eerste tempdb-apparaat naar de eerste apparaat van de SAP-database verplaatsen
* Tempdb-apparaten toevoegen aan elk van de VHD's met een apparaat van de SAP-database

Deze configuratie kan tempdb ofwel gebruiken meer ruimte nodig heeft dan het systeemstation is kunnen bieden. Als een verwijzing kunt een controleren of de grootte van het apparaat tempdb op bestaande systemen die on-premises uitgevoerd. Of een dergelijke configuratie IOPS getallen op basis van tempdb die niet kan worden voorzien van het systeemstation wilt inschakelen. Systemen die lokaal worden uitgevoerd kunnen opnieuw worden gebruikt voor het bewaken van i/o-werkbelasting op tempdb.

Plaats alle apparaten SAP-as-omgeving nooit op het station D:\ van de virtuele machine. Dit geldt ook voor de tempdb, zelfs als de objecten in de tempdb bewaard slechts tijdelijk zijn.

#### <a name="impact-of-database-compression"></a>Gevolgen van het Database-compressie
Elke meting, waardoor IOP's kan in configuraties waar i/o-bandbreedte kan een beperkende factor u helpen om uit te rekken van de werkbelasting een in een IaaS-scenario zoals Azure uitvoeren kunt. Daarom is het raadzaam om ervoor te zorgen dat de compressie SAP-as-omgeving wordt gebruikt voordat u een bestaande SAP-database naar Azure uploadt.

De aanbeveling om uit te voeren compressie voordat u uploadt naar Azure als het is niet geïmplementeerd wordt gegeven buiten verschillende redenen:

* De hoeveelheid gegevens te uploaden naar Azure is lager
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat sterkere hardware kunnen worden gebruikt met meer CPU's of hogere i/o-bandbreedte of minder i/o-latentie lokale
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Compressie van gegevens en LOB werken in een virtuele machine in Azure Virtual Machines gehost als er een lokale. Voor meer informatie over het controleren van als compressie al wordt gebruikt in een bestaande database van de SAP-as-omgeving, controleert u SAP-notitie [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit gebruiken om te controleren van Database-exemplaren
Voor SAP-systemen die van SAP-as-omgeving als databaseplatform gebruikmaken, is de DBACockpit toegankelijk als ingesloten browservensters in transactie DBACockpit of Webdynpro. De volledige functionaliteit voor bewaking en beheer van de database is echter beschikbaar in de implementatie van Webdynpro van alleen de DBACockpit.

Als met on-premises systemen verschillende stappen zijn vereist om alle SAP NetWeaver functionaliteit die wordt gebruikt door de Webdynpro uitvoering van de DBACockpit te schakelen. Ga als volgt SAP-notitie [1245200] voor het gebruik van webdynpros inschakelen en het genereren van de vereiste waarden. Wanneer u de instructies in de bovenstaande notities te volgen, configureren u ook de Internet-Communicatiemanager (icm) samen met de poorten die worden gebruikt voor http en https-verbindingen. De standaardinstelling voor HTTP-ziet er als volgt:

> ICM/server_port_0 = b = HTTP, poort = 8000, PROCTIMEOUT = 600, time-out = 600
> 
> ICM/server_port_1 = b = HTTPS, poort = 443$ $, PROCTIMEOUT = 600, time-out = 600
> 
> 

en de koppelingen in de transactie wordt gegenereerd DBACockpit ziet er ongeveer als volgt:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Afhankelijk van of en hoe de Azure-virtuele Machine die als host fungeert voor de SAP-systeem is verbonden via site-naar-site meerdere locaties of ExpressRoute (Cross-Premises implementatie), moet u ervoor zorgen dat ICM gebruikmaakt van een volledig gekwalificeerde hostnaam die kan worden omgezet op de computer waar u probeert te openen van de DBACockpit uit. Zie SAP-notitie [773830] om te begrijpen hoe ICM bepaalt de volledig gekwalificeerde hostnaam, afhankelijk van de parameters-profiel en set-parameter icm/host_name_full expliciet indien nodig.

Als u de virtuele machine in een Cloudconfiguratie scenario zonder cross-premises-connectiviteit tussen on-premises en Azure hebt geïmplementeerd, moet u een openbaar IP-adres en een domainlabel definiëren. De indeling van de openbare DNS-naam van de virtuele machine ziet er als volgt:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

Meer informatie over de DNS-naam vindt [hier][virtual-machines-azurerm-versus-azuresm].

Als u de SAP profiel parameter icm/host_name_full op de DNS-naam van de Azure VM de koppeling mogelijk uitzien:

> sap/https://mydomainlabel.westeurope.cloudapp.NET:44300/bc/sap/webdynpro/dba_cockpit
> 
> sap/http://mydomainlabel.westeurope.cloudapp.NET:8000/bc/sap/webdynpro/dba_cockpit
> 
> 

In dit geval moet u Zorg ervoor dat:

* Regels voor binnenkomende verbindingen toevoegen aan de Netwerkbeveiligingsgroep in de Azure portal voor de TCP/IP-poorten gebruikt voor communicatie met ICM
* Regels voor binnenkomende verbindingen toevoegen aan de Windows Firewall-configuratie voor de TCP/IP-poorten gebruikt voor communicatie met de ICM

Voor een geautomatiseerde geïmporteerd met alle correcties beschikbaar is, wordt aanbevolen om toe te passen periodiek de verzameling correctie SAP-notitie van toepassing op uw SAP-versie:

* [1558958]
* [1619967]
* [1882376]

Meer informatie over DBA Cockpit voor SAP-as-omgeving vindt u in de volgende SAP-opmerkingen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Overwegingen voor back-up/herstel voor SAP-as-omgeving
Uw back-upmethode moet worden gecontroleerd bij het implementeren van SAP-as-omgeving in Azure. Zelfs als het systeem niet een productieve systeem is, de SAP-database gehost door SAP-as-omgeving moet een back-up regelmatig. Omdat Azure Storage drie afbeeldingen houdt, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een opslagruimte compensating. De belangrijkste reden voor het onderhouden van een juiste back-up en herstel plan is meer die u logische/handmatige fouten compenseren kunt door punt in tijd herstelfuncties te bieden. Zodat het doel om beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of de back-ups in Azure met een ander systeem seed door te kopiëren van de bestaande database is. Bijvoorbeeld, kan u overbrengen van een 2-Tier SAP-configuratie naar een 3-Laagse system-installatie van hetzelfde systeem door een back-up herstellen.

Een back-up en herstellen van een database in Azure werkt op dezelfde manier als lokale. Zie SAP-opmerkingen:

* [1588316]
* [1585981]

voor informatie over maken dump configuraties en planning back-ups. Afhankelijk van uw strategie en behoeften kunt u configureren dumpen database en logboekbestanden voor de schijf op een van de bestaande schijven of Voeg een extra schijf voor de back-up. Als u het risico van gegevensverlies in geval van een fout opgetreden, is het aanbevolen voor het gebruik van een schijf waar geen databaseapparaat zich bevindt.

Compressie SAP-as-omgeving biedt naast gegevens- en LOB ook back-compressie. Als u wilt nemen minder ruimte met de database en logboekbestanden dumpbestanden verdient het gebruik van back-compressie. Voor meer informatie Zie SAP-notitie [1588316]. Comprimeren van de back-up is ook essentieel om te verminderen de hoeveelheid gegevens die moet worden overgezet als u van plan bent om back-ups of VHD's met back-up dumpbestanden van de virtuele Machine van Azure met on-premises te downloaden.

Gebruik geen station D:\ als doel-dump database of logboekbestanden.

#### <a name="performance-considerations-for-backupsrestores"></a>Prestatie-overwegingen voor back-ups/herstelacties
Prestaties van back-up/herstel is net zoals in de bare-metal implementaties, afhankelijk van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes zijn. Bovendien speelt de CPU-verbruik door back-compressie gebruikt een belangrijke rol op virtuele machines met slechts maximaal acht CPU-threads. Daarom kan een aannemen:

* De minder het aantal schijven gebruikt voor het opslaan van de database-apparaten, hoe kleiner totale doorvoer in lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de invloed van back-compressie
* De minder doelen (Stripe mappen, schijven) de back-up naar de kleinste de doorvoer schrijven

Vergroten van het aantal doelen te schrijven naar er zijn twee opties die gebruikt/gecombineerd afhankelijk van uw behoeften worden kunnen:

* Het volume voor back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOPS op dat volume striped
* Voor informatie over het maken van een configuratie dump op niveau SAP-as-omgeving waarin meer dan één doelmap de dump om te schrijven

Een volume striping over meerdere gekoppelde schijven is eerder in deze handleiding beschreven. Raadpleeg voor meer informatie over het gebruik van meerdere mappen in de configuratie van de dump SAP-as-omgeving de documentatie over de opgeslagen Procedure sp_config_dump, die wordt gebruikt voor het maken van de configuratie van de dump op de [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Herstel na noodgevallen met virtuele machines in Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Gegevensreplicatie met SAP Sybase replicatie Server
Met de SAP Sybase Replication Server (SRS) SAP-as-omgeving biedt een warme stand-by-oplossing voor databasetransacties asynchroon overdragen naar een externe locatie. 

De installatie en de werking van SRS werkt ook functioneel in een virtuele machine in Azure Services op virtuele Machine wordt gehost als er een lokale.

As-omgeving HADR via SAP replicatie Server is met een toekomstige release gepland. Het wordt getest met en uitgebracht voor Microsoft Azure-platforms, zodra deze beschikbaar is.

## <a name="specifics-to-sap-ase-on-linux"></a>Foutopsporingsgegevens naar SAP-as-omgeving op Linux
Beginnen met Microsoft Azure, kunt u gemakkelijk migreren uw bestaande toepassingen voor SAP-as-omgeving op Azure Virtual Machines. SAP-as-omgeving in een virtuele Machine kunt u de totale kosten van eigendom van de implementatie, beheer en onderhoud van de breedte van bedrijfstoepassingen verminderen door eenvoudig deze toepassingen met Microsoft Azure te migreren. Met de as-SAP omgeving in een virtuele Machine van Azure, kunnen beheerders en ontwikkelaars blijven gebruiken de dezelfde ontwikkeling en beheerhulpprogramma's die beschikbaar on-premises.

Voor het implementeren van Azure VM's is het belangrijk te weten van de officiële Sla's, u hier vindt: <https://azure.microsoft.com/support/legal/sla>

SAP sizinginformatie en een lijst met VM-SKU's gecertificeerd SAP wordt verstrekt in SAP-notitie [1928533]. Aanvullende SAP sizing documenten voor Azure Virtual machines u hier vindt <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> en hier <http : //blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Instructies en aanbevelingen voor het gebruik van Azure Storage, de implementatie van de SAP VM's of de SAP bewaking van toepassing op implementaties van SAP-as-omgeving in combinatie met SAP-toepassingen zoals vermeld in de eerste vier hoofdstukken van dit document.

De volgende twee SAP-opmerkingen bevatten algemene informatie over as-omgeving op Linux- en as-omgeving in de Cloud:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Ondersteuning voor SAP-versie van de as-omgeving
SAP momenteel ondersteunt SAP-as-omgeving versie 16,0 voor gebruik met SAP Business Suite producten. Alle updates voor SAP-as-omgeving server of JDBC en ODBC-stuurprogramma's worden gebruikt met SAP Business Suite-producten worden geleverd uitsluitend via de Marketplace SAP-Service op: <https://support.sap.com/swdc>.

Als voor installaties downloaden on-premises updates voor de server SAP-as-omgeving of voor de JDBC en ODBC-stuurprogramma's rechtstreeks vanuit de Sybase-websites niet. Voor gedetailleerde informatie over patches die worden ondersteund voor gebruik met SAP Business Suite producten on-premises en in Azure Virtual Machines naar de volgende SAP-opmerkingen:

* [1590719]
* [1973241]

Algemene informatie over het uitvoeren van SAP Business Suite van SAP-as-omgeving vindt u in de [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP as-omgeving configuratie-instructies voor installatie van de SAP-gerelateerde SAP as-omgeving in virtuele machines in Azure
#### <a name="structure-of-the-sap-ase-deployment"></a>Structuur van de as-omgeving SAP-implementatie
In overeenstemming met de algemene beschrijving moeten SAP-as-omgeving uitvoerbare bestanden zich bevinden of geïnstalleerd in het bestandssysteem van de hoofdmap van de virtuele machine (/sybase). Normaal gesproken worden de meeste van de SAP-as-omgeving system en hulpprogramma's voor databases niet echt gebruikt hard door SAP NetWeaver werkbelasting. Daarom kunnen de systeem- en hulpprogramma's voor databases (master, model, saptools, sybmgmtdb, sybsystemdb) blijven op het bestandssysteem hoofdmap. 

Een uitzondering kan worden de tijdelijke database bevat alle werktabellen en tijdelijke tabellen die zijn gemaakt door SAP as-omgeving, die in geval van een bepaalde ERP SAP en alle BW werkbelastingen vereisen mogelijk hoger gegevensvolume of i/o-bewerkingen, volume dat niet in de oorspronkelijke VM-besturingssysteem past schijf.

Afhankelijk van de versie van SAPInst/SWPM gebruikt voor het installeren van het systeem, kan de database bevatten:

* Een enkele tempdb SAP-as-omgeving, die wordt gemaakt bij het installeren van SAP-as-omgeving
* Een SAP-as-omgeving tempdb gemaakt door SAP-as-omgeving en een extra saptempdb gemaakt door de SAP-installatieprogramma te installeren
* Een SAP-as-omgeving tempdb gemaakt door het installeren van SAP-as-omgeving en een extra tempdb die handmatig is gemaakt (bijvoorbeeld de volgende SAP-notitie [1752266]) om te voldoen aan specifieke tempdb ERP/BW

In geval van een specifieke ERP of alle BW-werkbelastingen, is het zinvol, met betrekking tot prestaties, zodat de tempdb-apparaten van de bovendien gemaakte tempdb (door SWPM of handmatig) op een afzonderlijke bestandssysteem, kan worden gerepresenteerd door een gegevensschijf met één Azure of een Linux-RAID-spannin g meerdere Azure gegevensschijven. Als er geen aanvullende tempdb bestaat, is het raadzaam een te maken (SAP-notitie [1752266]).

Voor deze systemen moeten de volgende stappen worden uitgevoerd voor de tempdb bovendien gemaakt:

* De eerste tempdb-map verplaatsen naar het eerste bestandssysteem van de SAP-database
* Tempdb-mappen toevoegen aan elk van de schijven met een bestandssysteem van de SAP-database

Deze configuratie kan tempdb ofwel gebruiken meer ruimte nodig heeft dan het systeemstation is kunnen bieden. Als een verwijzing kunt een controleren of de grootte van de directory tempdb op bestaande systemen die on-premises uitgevoerd. Of een dergelijke configuratie IOPS getallen op basis van tempdb die niet kan worden voorzien van het systeemstation wilt inschakelen. Systemen die lokaal worden uitgevoerd kunnen opnieuw worden gebruikt voor het bewaken van i/o-werkbelasting op tempdb.

Plaats alle mappen SAP-as-omgeving nooit naar mnt of /mnt/resource van de virtuele machine. Dit geldt ook voor de tempdb, zelfs als de objecten in de tempdb bewaard slechts tijdelijk zijn omdat mnt of /mnt/resource is een standaard virtuele machine van Azure tijdelijke ruimte, die niet persistent. Meer informatie over de tijdelijke ruimte van de virtuele machine van Azure kunnen worden gevonden in [in dit artikel][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Gevolgen van het Database-compressie
Elke meting, waardoor IOP's kan in configuraties waar i/o-bandbreedte kan een beperkende factor u helpen om uit te rekken van de werkbelasting een in een IaaS-scenario zoals Azure uitvoeren kunt. Daarom is het raadzaam om ervoor te zorgen dat de compressie SAP-as-omgeving wordt gebruikt voordat u een bestaande SAP-database naar Azure uploadt.

De aanbeveling om uit te voeren compressie voordat u uploadt naar Azure als het is niet geïmplementeerd wordt gegeven buiten verschillende redenen:

* De hoeveelheid gegevens te uploaden naar Azure is lager
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat sterkere hardware kunnen worden gebruikt met meer CPU's of hogere i/o-bandbreedte of minder i/o-latentie lokale
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Compressie van gegevens en LOB werken in een virtuele machine in Azure Virtual Machines gehost als er een lokale. Voor meer informatie over het controleren van als compressie al wordt gebruikt in een bestaande database van de SAP-as-omgeving, controleert u SAP-notitie [1750510]. Zie voor aanvullende informatie met betrekking tot de database compressie SAP-notitie [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit gebruiken om te controleren van Database-exemplaren
Voor SAP-systemen die van SAP-as-omgeving als databaseplatform gebruikmaken, is de DBACockpit toegankelijk als ingesloten browservensters in transactie DBACockpit of Webdynpro. De volledige functionaliteit voor bewaking en beheer van de database is echter beschikbaar in de implementatie van Webdynpro van alleen de DBACockpit.

Als met on-premises systemen verschillende stappen zijn vereist om alle SAP NetWeaver functionaliteit die wordt gebruikt door de Webdynpro uitvoering van de DBACockpit te schakelen. Ga als volgt SAP-notitie [1245200] voor het gebruik van webdynpros inschakelen en het genereren van de vereiste waarden. Wanneer u de instructies in de bovenstaande notities te volgen, configureren u ook de Internet-Communicatiemanager (icm) samen met de poorten die worden gebruikt voor http en https-verbindingen. De standaardinstelling voor HTTP-ziet er als volgt:

> ICM/server_port_0 = b = HTTP, poort = 8000, PROCTIMEOUT = 600, time-out = 600
> 
> ICM/server_port_1 = b = HTTPS, poort = 443$ $, PROCTIMEOUT = 600, time-out = 600
> 
> 

en de koppelingen in de transactie wordt gegenereerd DBACockpit ziet er ongeveer als volgt:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit
> 
> 

Afhankelijk van of en hoe de Azure-virtuele Machine die als host fungeert voor de SAP-systeem is verbonden via site-naar-site meerdere locaties of ExpressRoute (Cross-Premises implementatie), moet u ervoor zorgen dat ICM gebruikmaakt van een volledig gekwalificeerde hostnaam die kan worden omgezet op de computer waar u probeert te openen van de DBACockpit uit. Zie SAP-notitie [773830] om te begrijpen hoe ICM bepaalt de volledig gekwalificeerde hostnaam, afhankelijk van de parameters-profiel en set-parameter icm/host_name_full expliciet indien nodig.

Als u de virtuele machine in een Cloudconfiguratie scenario zonder cross-premises-connectiviteit tussen on-premises en Azure hebt geïmplementeerd, moet u een openbaar IP-adres en een domainlabel definiëren. De indeling van de openbare DNS-naam van de virtuele machine ziet er als volgt:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

Meer informatie over de DNS-naam vindt [hier][virtual-machines-azurerm-versus-azuresm].

Als u de SAP profiel parameter icm/host_name_full op de DNS-naam van de Azure VM de koppeling mogelijk uitzien:

> sap/https://mydomainlabel.westeurope.cloudapp.NET:44300/bc/sap/webdynpro/dba_cockpit
> 
> sap/http://mydomainlabel.westeurope.cloudapp.NET:8000/bc/sap/webdynpro/dba_cockpit
> 
> 

In dit geval moet u Zorg ervoor dat:

* Regels voor binnenkomende verbindingen toevoegen aan de Netwerkbeveiligingsgroep in de Azure portal voor de TCP/IP-poorten gebruikt voor communicatie met ICM
* Regels voor binnenkomende verbindingen toevoegen aan de Windows Firewall-configuratie voor de TCP/IP-poorten gebruikt voor communicatie met de ICM

Voor een geautomatiseerde geïmporteerd met alle correcties beschikbaar is, wordt aanbevolen om toe te passen periodiek de verzameling correctie SAP-notitie van toepassing op uw SAP-versie:

* [1558958]
* [1619967]
* [1882376]

Meer informatie over DBA Cockpit voor SAP-as-omgeving vindt u in de volgende SAP-opmerkingen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Overwegingen voor back-up/herstel voor SAP-as-omgeving
Uw back-upmethode moet worden gecontroleerd bij het implementeren van SAP-as-omgeving in Azure. Zelfs als het systeem niet een productieve systeem is, de SAP-database gehost door SAP-as-omgeving moet een back-up regelmatig. Omdat Azure Storage drie afbeeldingen houdt, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een opslagruimte compensating. De belangrijkste reden voor het onderhouden van een juiste back-up en herstel plan is meer die u logische/handmatige fouten compenseren kunt door punt in tijd herstelfuncties te bieden. Zodat het doel om beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of de back-ups in Azure met een ander systeem seed door te kopiëren van de bestaande database is. Bijvoorbeeld, kan u overbrengen van een 2-Tier SAP-configuratie naar een 3-Laagse system-installatie van hetzelfde systeem door een back-up herstellen.

Een back-up en herstellen van een database in Azure werkt op dezelfde manier als lokale. Zie SAP-opmerkingen:

* [1588316]
* [1585981]

voor informatie over maken dump configuraties en planning back-ups. Afhankelijk van uw strategie en behoeften kunt u configureren dumpen database en logboekbestanden voor de schijf op een van de bestaande schijven of Voeg een extra schijf voor de back-up. Om te beperken het risico van gegevensverlies in geval van een fout die het is raadzaam om een schijf gebruiken waarin er geen database map/bestand zich bevindt.

Compressie SAP-as-omgeving biedt naast gegevens- en LOB ook back-compressie. Als u wilt nemen minder ruimte met de database en logboekbestanden dumpbestanden verdient het gebruik van back-compressie. Voor meer informatie Zie SAP-notitie [1588316]. Comprimeren van de back-up is ook essentieel om te verminderen de hoeveelheid gegevens die moet worden overgezet als u van plan bent om back-ups of VHD's met back-up dumpbestanden van de virtuele Machine van Azure met on-premises te downloaden.

Gebruik de Azure VM tijdelijke ruimte mnt of /mnt/resource niet als doel-dump database of logboekbestanden.

#### <a name="performance-considerations-for-backupsrestores"></a>Prestatie-overwegingen voor back-ups/herstelacties
Prestaties van back-up/herstel is net zoals in de bare-metal implementaties, afhankelijk van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes zijn. Bovendien speelt de CPU-verbruik door back-compressie gebruikt een belangrijke rol op virtuele machines met slechts maximaal acht CPU-threads. Daarom kan een aannemen:

* De minder het aantal schijven gebruikt voor het opslaan van de database-apparaten, hoe kleiner totale doorvoer in lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de invloed van back-compressie
* De minder doelen (Linux-software RAID, schijven) de back-up naar de kleinste de doorvoer schrijven

Vergroten van het aantal doelen te schrijven naar er zijn twee opties die gebruikt/gecombineerd afhankelijk van uw behoeften worden kunnen:

* Het volume voor back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOPS op dat volume striped
* Voor informatie over het maken van een configuratie dump op niveau SAP-as-omgeving waarin meer dan één doelmap de dump om te schrijven

Een volume striping over meerdere gekoppelde schijven is eerder in deze handleiding beschreven. Raadpleeg voor meer informatie over het gebruik van meerdere mappen in de configuratie van de dump SAP-as-omgeving de documentatie over de opgeslagen Procedure sp_config_dump, die wordt gebruikt voor het maken van de configuratie van de dump op de [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Herstel na noodgevallen met virtuele machines in Azure
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Gegevensreplicatie met SAP Sybase replicatie Server
Met de SAP Sybase Replication Server (SRS) SAP-as-omgeving biedt een warme stand-by-oplossing voor databasetransacties asynchroon overdragen naar een externe locatie. 

De installatie en de werking van SRS werkt ook functioneel in een virtuele machine in Azure Services op virtuele Machine wordt gehost als er een lokale.

As-omgeving HADR via SAP replicatie Server wordt op dit moment niet ondersteund. Het kan worden getest met en in de toekomst uitgebracht voor Microsoft Azure-platforms.

## <a name="specifics-to-oracle-database-on-windows"></a>Specificaties met Oracle-Database in Windows
Oracle-software wordt ondersteund door Oracle worden uitgevoerd op Microsoft Windows Hyper-V en Azure. Raadpleeg voor meer informatie over de algemene ondersteuning van Windows Hyper-V en Azure: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Na de algemene ondersteuning wordt ook het specifieke scenario van SAP-toepassingen die gebruik van Oracle-Databases ondersteund. Details zijn met de naam in dit gedeelte van het document.

### <a name="oracle-version-support"></a>Ondersteuning voor Oracle-versie
Oracle-versies en de bijbehorende OS-versies, die worden ondersteund voor SAP uitgevoerd op Oracle op Azure Virtual Machines u in SAP-notitie vindt [2039619].

Algemene informatie over het uitvoeren van SAP Business Suite op Oracle vindt u in 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle-configuratie-instructies voor SAP-installaties in virtuele machines in Azure
#### <a name="storage-configuration"></a>Opslagconfiguratie
Slechts één exemplaar Oracle met NTFS-geformatteerde schijven wordt ondersteund. Alle databasebestanden moeten worden opgeslagen op het NTFS-bestandssysteem op basis van de VHD's of schijven beheerd. Deze schijven zijn gekoppeld aan de virtuele machine van Azure en zijn gebaseerd op de pagina Azure BLOB-opslag (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of beheerde schijven (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Elk soort netwerkstations of externe shares zoals Azure Bestandsservices:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-connections-to-Microsoft-Azure-Files.aspx>

zijn **niet** ondersteund voor Oracle-databasebestanden!

Met behulp van schijven op basis van Azure pagina-BLOB-opslag of beheerd schijven, de instructies die zijn aangebracht in dit document in hoofdstuk [in cache opslaan voor virtuele machines en gegevensschijven] [ dbms-guide-2.1] en [Microsoft Azure Storage] [ dbms-guide-2.3] van toepassing op implementaties met de Oracle-Database.

Zoals eerder in het algemene gedeelte van het document wordt uitgelegd, bestaan de quota voor IOPS-doorvoer voor Azure-schijven. De exacte quota zijn afhankelijk van het VM-type gebruikt. Een lijst met VM-typen met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Raadpleeg voor de ondersteunde typen voor de virtuele machine van Azure SAP-notitie [1928533].

Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor het opslaan van de DB-bestanden op één enkele gekoppelde schijf. 

Als meer IOPS nodig zijn, het is raadzaam gebruik venster opslaggroepen (alleen beschikbaar in Windows Server 2012 en hoger) of Windows 2008 R2 voor Windows te verwijderen voor het maken van één grote logisch apparaat via meerdere gekoppelde schijven (Zie ook hoofdstuk [Software RAID] [ dbms-guide-2.2] van dit document). Deze aanpak vereenvoudigt de beheer-overhead voor het beheren van de schijfruimte en voorkomt de inspanning om de bestanden handmatig te verdelen over meerdere gekoppelde schijven.

#### <a name="backup--restore"></a>Back-up en herstellen
Voor back-up- / terugzetbewerking functionaliteit, de SAP-Brazilië * hulpprogramma's voor Oracle op dezelfde manier als op de standaard Windows-serverbesturingssystemen en Hyper-V worden ondersteund. Oracle Recovery Manager (RMAN) wordt ook ondersteund voor back-ups op schijf en het herstellen van de schijf.

#### <a name="high-availability"></a>Hoge beschikbaarheid
Oracle Data Guard wordt ondersteund voor hoge beschikbaarheid en noodherstel. Meer informatie vindt u in [dit] [ virtual-machines-windows-classic-configure-oracle-data-guard] documentatie.

#### <a name="other"></a>Overige
Andere algemene onderwerpen, zoals Beschikbaarheidssets van Azure of SAP bewaking van toepassing zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de Oracle-Database.

## <a name="specifics-to-oracle-database-on-oracle-linux"></a>Specificaties met Oracle-Database op Oracle Linux
Oracle-software wordt ondersteund door Oracle worden uitgevoerd op Microsoft Windows Hyper-V en Azure. Raadpleeg voor meer informatie over de algemene ondersteuning van Windows Hyper-V en Azure: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Na de algemene ondersteuning wordt ook het specifieke scenario van SAP-toepassingen die gebruik van Oracle-Databases ondersteund. Details zijn met de naam in dit gedeelte van het document.

### <a name="oracle-version-support"></a>Ondersteuning voor Oracle-versie
Oracle-versies en de bijbehorende OS-versies, die worden ondersteund voor SAP uitgevoerd op Oracle op Azure Virtual Machines u in SAP-notitie vindt [2039619].

Algemene informatie over het uitvoeren van SAP Business Suite op Oracle vindt u in 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle-configuratie-instructies voor SAP-installaties in virtuele machines in Azure
#### <a name="storage-configuration"></a>Opslagconfiguratie
Slechts wordt één exemplaar Oracle met ext3, ext4 en xfs indeling schijven ondersteund. Alle databasebestanden moeten worden opgeslagen op deze bestandssystemen op basis van de VHD's of schijven beheerd. Deze schijven zijn gekoppeld aan de virtuele machine van Azure en zijn gebaseerd op de pagina Azure BLOB-opslag (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of beheerde schijven (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Elk soort netwerkstations of externe shares zoals Azure Bestandsservices:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-connections-to-Microsoft-Azure-Files.aspx>

zijn **niet** ondersteund voor Oracle-databasebestanden!

Met behulp van schijven op basis van Azure pagina-BLOB-opslag of beheerd schijven, de instructies die zijn aangebracht in dit document in hoofdstuk [in cache opslaan voor virtuele machines en gegevensschijven] [ dbms-guide-2.1] en [Microsoft Azure Storage] [ dbms-guide-2.3] van toepassing op implementaties met de Oracle-Database.

Zoals eerder in het algemene gedeelte van het document wordt uitgelegd, bestaan de quota voor IOPS-doorvoer voor Azure-schijven. De exacte quota zijn afhankelijk van het VM-type gebruikt. Een lijst met VM-typen met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Raadpleeg voor de ondersteunde typen voor de virtuele machine van Azure SAP-notitie [1928533]

Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor het opslaan van de DB-bestanden op één enkele gekoppelde schijf. 

Als meer IOPS vereist zijn, moet het is raadzaam om LVM (beheer van logische volumes) of MDADM te maken die een groot logisch volume via meerdere gekoppelde schijven. Zie ook hoofdstuk [Software RAID] [ dbms-guide-2.2] van dit document. Deze aanpak vereenvoudigt de beheer-overhead voor het beheren van de schijfruimte en voorkomt de inspanning om de bestanden handmatig te verdelen over meerdere gekoppelde schijven.

#### <a name="backup--restore"></a>Back-up en herstellen
Voor back-up- / terugzetbewerking functionaliteit, de SAP-Brazilië * hulpprogramma's voor Oracle op dezelfde manier als op bare-metal en Hyper-V worden ondersteund. Oracle Recovery Manager (RMAN) wordt ook ondersteund voor back-ups op schijf en het herstellen van de schijf.

#### <a name="high-availability"></a>Hoge beschikbaarheid
Oracle Data Guard wordt ondersteund voor hoge beschikbaarheid en noodherstel. Meer informatie vindt u in [dit] [ virtual-machines-windows-classic-configure-oracle-data-guard] documentatie.

#### <a name="other"></a>Overige
Andere algemene onderwerpen, zoals Beschikbaarheidssets van Azure of SAP bewaking van toepassing zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de Oracle-Database.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Details voor de Database SAP MaxDB in Windows
### <a name="sap-maxdb-version-support"></a>SAP-MaxDB versie-ondersteuning
SAP momenteel ondersteunt SAP MaxDB versie 7,9 voor gebruik met producten op basis van een SAP NetWeaver in Azure. Alle updates voor SAP MaxDB server of JDBC en ODBC-stuurprogramma's worden gebruikt met producten op basis van een SAP NetWeaver vindt uitsluitend via de Marketplace SAP-Service op <https://support.sap.com/swdc>.
Algemene informatie over het uitvoeren van SAP NetWeaver op SAP MaxDB kan worden gevonden op <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Microsoft Windows-versies en de Azure VM-typen ondersteund voor SAP MaxDB DBMS
Zie informatie over de ondersteunde versie van Microsoft Windows voor SAP MaxDB DBMS op Azure:

* [SAP Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van het besturingssysteem Microsoft Windows, Microsoft Windows 2012 R2 gebruiken.

### <a name="available-sap-maxdb-documentation"></a>Beschikbare SAP MaxDB documentatie
U vindt de bijgewerkte lijst met SAP MaxDB documentatie in de volgende SAP-notitie [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP MaxDB configuratie-instructies voor SAP-installaties in virtuele machines in Azure
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Opslagconfiguratie
Aanbevolen procedures van de Azure-opslag voor SAP MaxDB Ga als volgt de algemene aanbevelingen in hoofdstuk wordt vermeld [structuur van de implementatie van een RDBMS][dbms-guide-2].

> [!IMPORTANT]
> Net als andere databases heeft SAP MaxDB ook gegevens en logboekbestanden. De juiste term is echter in SAP MaxDB terminologie 'volume' (geen ' file'). Er zijn bijvoorbeeld SAP MaxDB gegevensvolumes en logboekvolumes. Verwar deze voor OS schijfvolumes niet. 
> 
> 

Kort gezegd hebt u:

* Als u Azure Storage-accounts gebruiken, stelt u de Azure-opslagaccount waarin de SAP MaxDB gegevens en logboekbestanden volumes (dat wil zeggen-bestanden) op **lokaal redundante opslag (LRS)** zoals opgegeven in hoofdstuk [Microsoft Azure Storage][dbms-guide-2.3].
* De i/o-pad voor SAP MaxDB gegevensvolumes (dat wil zeggen-bestanden) scheiden van het i/o-pad voor logboekbestanden-volumes (dat wil zeggen-bestanden). Dit betekent dat SAP MaxDB gegevensvolumes (dat wil zeggen-bestanden) moeten worden geïnstalleerd op één logisch station en SAP MaxDB logboekvolumes (dat wil zeggen-bestanden) moeten worden geïnstalleerd op een andere logische schijf.
* Instellen van het juiste cachebewerkingen type voor elke schijf, afhankelijk van of u deze gebruiken voor SAP MaxDB gegevensbestand of logboekbestand volumes (dat wil zeggen-bestanden) en of het gebruiken van Azure Standard of Premium-opslag voor Azure, zoals beschreven in hoofdstuk [in cache opslaan voor virtuele machines en gegevensschijven] [dbms-guide-2.1].
* Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor alle gegevensvolumes opslaan op een enkele gekoppelde schijf en ook alle logboekvolumes voor database opslaan op een andere enkele gekoppelde schijf.
* Als meer IOPS en/of ruimte nodig zijn, is het raadzaam om Microsoft Windows-striping voor Microsoft Windows 2008 R2 of Microsoft venster opslaggroepen (alleen beschikbaar in Microsoft Windows Server 2012 en hoger) te maken van één grote logisch apparaat via meerdere gekoppelde schijven. Zie ook hoofdstuk [Software RAID] [ dbms-guide-2.2] van dit document. Deze aanpak vereenvoudigt de beheer-overhead voor het beheren van de schijfruimte en voorkomt de moeite van de bestanden handmatig te verdelen over meerdere gekoppelde schijven.
* Voor de hoogste vereisten voor IOPS, kunt u Azure Premium-opslag gebruiken die beschikbaar zijn op DS-serie en GS-serie virtuele machines is.

![Configuratie van Azure IaaS VM voor SAP MaxDB DBMS verwijzing][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Back-up en herstel
Bij het SAP MaxDB implementeren in Azure, moet u uw back-upmethode doornemen. Zelfs als het systeem niet een productieve systeem is, de SAP-database gehost door SAP MaxDB moet een back-up regelmatig. Omdat Azure Storage drie afbeeldingen houdt, is nu een back-up minder belangrijk in termen van bescherming van uw systeem tegen storingen van opslag en belangrijker operationele of administratieve fouten. De belangrijkste reden voor het onderhouden van een goede back-up en herstel plannen is zodat u logische of handmatige fouten compenseren kunt dankzij de punt in tijd herstelfuncties. Het doel is daarom gebruikmaken van back-ups van de database herstellen naar een bepaald punt in tijd of de back-ups in Azure met een ander systeem seed door te kopiëren van de bestaande database. Bijvoorbeeld, kan u overbrengen van een laag 2 SAP-configuratie naar een 3-laagse system-installatie van hetzelfde systeem door een back-up herstellen.

Een back-up en herstellen van een database in Azure werkt op dezelfde manier als voor on-premises systemen, zodat u kunt standaard SAP MaxDB back-up/herstel hulpmiddelen die worden beschreven in een van de SAP MaxDB documentatie documenten die worden vermeld in SAP-notitie [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Prestatie-overwegingen voor back-up en herstel
Prestaties van back-up en herstel is net zoals in de bare-metal implementaties, afhankelijk van hoeveel volumes in parallelle instructies en de doorvoer van deze volumes kunnen worden gelezen. Het CPU-verbruik gebruikt door de back-compressie kan een belangrijke rol bovendien afspelen op virtuele machines met maximaal acht CPU-threads. Daarom kan een aannemen:

* De minder het aantal schijven gebruikt voor het opslaan van de database-apparaten, des te lager de totale doorvoer lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de invloed van back-compressie
* De minder doelen (Stripe mappen, schijven) de back-up naar de laagste de doorvoer schrijven

Er zijn twee opties die u, mogelijk in combinatie, afhankelijk van uw behoeften gebruiken kunt voor een verhoging van het aantal doelen te schrijven naar:

* Afzonderlijke volumes dat voor back-up
* Het volume voor back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOPS op dat schijfvolume striped
* Apparaten voor afzonderlijke specifieke logische schijf hebben:
  * SAP-MaxDB back-upvolumes (dat wil zeggen-bestanden)
  * SAP-MaxDB gegevensvolumes (dat wil zeggen-bestanden)
  * SAP-MaxDB logboekvolumes (dat wil zeggen-bestanden)

Een volume striping over meerdere gekoppelde schijven zijn besproken eerder in hoofdstuk [Software RAID] [ dbms-guide-2.2] van dit document. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Andere
Andere algemene onderwerpen, zoals Beschikbaarheidssets van Azure of SAP controle zijn ook van toepassing zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de SAP MaxDB-database.
Andere SAP MaxDB-specifieke instellingen transparant virtuele Azure-machines zijn en worden beschreven in verschillende documenten die worden vermeld in SAP-notitie [767598] en in deze SAP-opmerkingen:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Specifieke informatie voor SAP liveCache in Windows
### <a name="sap-livecache-version-support"></a>SAP liveCache versie-ondersteuning
Minimale versie van SAP liveCache ondersteund in Azure Virtual Machines **SAP LC/LCAPPS 10.0 SP 25** inclusief **liveCache 7.9.08.31** en **LCA-Build 25**, vrijgegeven voor **EhP 2 voor SAP SCM 7.0** en hoger.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Microsoft Windows-versies en de Azure VM-typen ondersteund voor SAP liveCache DBMS
Zie informatie over de ondersteunde versie van Microsoft Windows voor SAP liveCache op Azure:

* [SAP Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van het besturingssysteem Microsoft Windows Server gebruiken. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache configuratie-instructies voor SAP-installaties in Azure VM 's
#### <a name="recommended-azure-vm-types"></a>Aanbevolen Azure VM-typen
Heeft een grote invloed op de prestaties van SAP liveCache zoals SAP liveCache is een toepassing met grote berekeningen, de hoeveelheid en de snelheid van het RAM-geheugen en CPU. 

Voor de virtuele machine van Azure die worden ondersteund door SAP (SAP-notitie [1928533]), alle virtuele CPU-resources toegewezen aan de virtuele machine worden ondersteund door toegewijde fysieke CPU-bronnen van de hypervisor. Er is geen overmatige inrichting (en dus concurrentie voor CPU-bronnen) plaatsvindt.

Op deze manier voor alle virtuele Azure-machine exemplaar typen ondersteund door SAP, is het geheugen van de VM 100% toegewezen aan het fysieke geheugen – (te veel streven), bijvoorbeeld overmatige inrichting wordt niet gebruikt.

Vanuit dit perspectief verdient het nieuwe D-reeks of virtuele machine van Azure Active Directory-serie (in combinatie met Azure Premium Storage)-type gebruiken als ze 60% snellere processors dan de A-serie hebben. Voor de grootste RAM-geheugen en CPU-belasting, kunt u G-serie en GS-serie (in combinatie met Azure Premium-opslag) virtuele machines met de meest recente Intel Xeon® processor E5 v3-familie die twee keer de hoeveelheid geheugen en vier keer de Solid-State station opslag (SSD's) van de D/DS-serie.

#### <a name="storage-configuration"></a>Opslagconfiguratie
Zoals SAP liveCache is gebaseerd op MaxDB SAP-technologie, alle Azure-opslag aanbevolen werkwijzen besproken in hoofdstuk wordt vermeld voor SAP MaxDB [opslagconfiguratie] [ dbms-guide-8.4.1] zijn ook geldig voor SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Toegewezen virtuele machine van Azure voor liveCache
Zoals SAP liveCache wordt intensief verwerkingskracht gebruikt, voor productief gebruik is het raadzaam om te implementeren op een specifieke virtuele Machine van Azure. 

![Virtuele machine van Azure voor liveCache voor productief gebruiksvoorbeeld Dedicated][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Back-up en herstel
Back-up en herstel, met inbegrip van prestatie-overwegingen al worden beschreven in de desbetreffende SAP MaxDB hoofdstukken [back-up en herstel] [ dbms-guide-8.4.2] en [prestatie-overwegingen voor back-up maken en terugzetten][dbms-guide-8.4.3]. 

#### <a name="other"></a>Overige
Andere algemene onderwerpen al worden beschreven in de relevante SAP-MaxDB [dit] [ dbms-guide-8.4.4] hoofdstuk. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Details voor de inhoud SAP-Server in Windows
Inhoud SAP-Server is een afzonderlijke, op basis van server-onderdeel voor het opslaan van inhoud zoals elektronische documenten in verschillende indelingen. De inhoudsserver SAP wordt geleverd door de ontwikkeling van technologie en moet worden gebruikt tussen verschillende toepassingen voor alle SAP-toepassingen. Het is geïnstalleerd op een afzonderlijk systeem. Typische inhoud is trainingsmateriaal en documentatie uit Knowledge datawarehouse of technische tekeningen die afkomstig zijn van de mySAP PLM Document Management-systeem. 

### <a name="sap-content-server-version-support"></a>Ondersteuning voor SAP inhoudsserver versie
SAP momenteel ondersteunt:

* **SAP-inhoudsserver** met versie **6.50 (en hoger)**
* **SAP MaxDB versie 7,9**
* **Microsoft IIS (Internet Information Server) versie 8.0 (en hoger)**

Het is raadzaam de nieuwste versie van de Content Server SAP, dat op het moment van publicatie van dit document **6.50 SP4**, en de nieuwste versie van **Microsoft IIS 8.5**. 

Controleer de meest recente ondersteunde versies van inhoud SAP-Server en Microsoft IIS in de [SAP Product beschikbaarheid Matrix (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Microsoft Windows en de Azure VM-typen ondersteund voor SAP-inhoudsserver
Voor meer informatie over ondersteunde Windows-versie voor Content SAP-Server op Azure, Zie:

* [SAP Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van Microsoft Windows Server gebruiken.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP inhoudsserver configuratie-instructies voor SAP-installaties in virtuele machines in Azure
#### <a name="storage-configuration"></a>Opslagconfiguratie
Als u inhoud SAP-Server voor het opslaan van bestanden in de database SAP MaxDB configureert, alle Azure-opslag best practices aanbeveling in hoofdstuk wordt vermeld voor SAP MaxDB [opslagconfiguratie] [ dbms-guide-8.4.1] zijn ook geldig voor het scenario SAP-inhoudsserver. 

Als u inhoud SAP-Server voor het opslaan van bestanden in het bestandssysteem is geconfigureerd, wordt het aanbevolen bij gebruik van een specifieke logische schijf. Met Windows Storage Spaces kunt u ook vergroten de grootte van de logische schijf en de doorvoer IOP's, zoals beschreven in hoofdstuk [Software RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>SAP inhoudsserver locatie
SAP-inhoudsserver moet worden geïmplementeerd in de dezelfde Azure-regio en Azure VNET waarop het SAP-systeem wordt geïmplementeerd. U bent vrij om te bepalen of u wilt implementeren de inhoudsserver SAP-onderdelen op een specifieke virtuele machine in Azure of op dezelfde virtuele machine waarop het SAP-systeem wordt uitgevoerd. 

![Speciale Azure VM voor SAP-inhoudsserver][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP-cachelocatie Server
De SAP-Cache-Server is een extra onderdeel op de server voor toegang tot documenten lokaal (cache). SAP-Cache-Server slaat de documenten van een inhoudsserver van SAP. Hiermee wordt netwerkverkeer optimaliseren als u documenten hebben meer dan één keer worden opgehaald vanaf verschillende locaties. De regel is dat de SAP-cacheserver moet fysiek dicht bij de client die toegang heeft tot de SAP-cacheserver. 

Hier hebt u twee opties:

1. **Client is een back-end SAP-systeem** als een back-end SAP-systeem is geconfigureerd voor toegang tot inhoud SAP-Server, dat SAP-systeem is een client. Als zowel SAP-systeem en de inhoudsserver SAP zijn geïmplementeerd in dezelfde Azure-regio – in hetzelfde Azure datacenter – gebruikers die zich fysiek dicht bij elkaar. Er is daarom niet nodig hebben een speciale Server voor SAP-Cache. SAP-UI-clients (SAP-GUI of web browser) rechtstreeks toegang hebben tot het SAP-systeem en het SAP-systeem documenten opgehaald uit de SAP-inhoudsserver.
2. **Client is een webbrowser die lokale** de Content SAP-Server kan worden geconfigureerd voor het rechtstreeks door de webbrowser worden geopend. In dit geval is een webbrowser op het lokale met een client van de SAP-inhoudsserver. On-premises datacentrum en Azure-datacenter worden geplaatst op verschillende fysieke locaties (in het ideale geval dicht bij elkaar). Uw on-premises datacentrum is verbonden met Azure via Azure Site-naar-Site VPN- of ExpressRoute. Hoewel beide opties veilige VPN-netwerkverbinding naar Azure bieden, site-naar-site-netwerkverbinding een SLA netwerk bandbreedte en de latentie tussen de on-premises datacentrum en de Azure-datacenter niet aangeboden. Als u wilt versnellen toegang tot documenten, kunt u het volgende doen:
   1. SAP-cacheserver on-premises installeert, dicht bij de on-premises webbrowser (optie op [dit] [ dbms-guide-900-sap-cache-server-on-premises] afbeelding)
   2. Configureer Azure ExpressRoute, met een hoge en lage latentie speciaal netwerkverbinding tussen on-premises datacentrum en Azure-datacenter.

![Optie voor SAP-cacheserver on-premises installeert][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Back-up en herstellen
Als u de SAP-inhoudsserver bestanden moeten worden opgeslagen in de database SAP MaxDB configureert, de procedure- en Prestatieoverwegingen voor back-up/herstel al worden beschreven in SAP MaxDB hoofdstuk [back-up en herstel] [ dbms-guide-8.4.2]en hoofdstuk [Prestatieoverwegingen voor back-up en herstel][dbms-guide-8.4.3]. 

Als u de inhoud SAP-Server voor het opslaan van bestanden in het bestandssysteem configureren, één optie moet worden uitgevoerd handmatige back-up/herstel van de structuur van het hele bestand waarin de documenten bevinden. Net als bij SAP MaxDB back-up/herstel, het beste een aparte schijfvolume voor back-doel hebben. 

#### <a name="other"></a>Overige
Andere inhoudsserver SAP-specifieke instellingen NLB is zichtbaar voor virtuele Azure-machines en in verschillende documenten en -opmerkingen bij de SAP worden beschreven:

* <https://service.SAP.com/contentserver> 
* SAP-notitie [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Foutopsporingsgegevens naar IBM DB2 voor LUW in Windows
U kunt eenvoudig uw bestaande SAP-toepassing uitgevoerd op de IBM DB2 voor Linux, UNIX- en Windows (LUW) virtuele machines in Azure met Microsoft Azure migreren. Met SAP op IBM DB2 voor LUW, kunnen beheerders en ontwikkelaars nog steeds gebruikt de dezelfde ontwikkeling en beheerprogramma's, beschikbare on-premises.
Algemene informatie over het uitvoeren van SAP Business Suite op IBM DB2 voor LUW vindt u in de SAP Community netwerk (SCN) op <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Zie voor meer informatie en updates over SAP op DB2 voor LUW op Azure, SAP-notitie [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 voor Linux, UNIX- en ondersteuning voor Windows-versie
SAP op IBM DB2 voor LUW op Services van Microsoft Azure-virtuele Machine wordt ondersteund vanaf DB2 versie 10.5.

Raadpleeg voor informatie over ondersteunde SAP-producten en typen van de virtuele machine in Azure, SAP-notitie [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 voor Linux, UNIX- en Windows-configuratie-instructies voor SAP-installaties in virtuele machines in Azure
#### <a name="storage-configuration"></a>Opslagconfiguratie
Alle databasebestanden moeten worden opgeslagen op het NTFS-bestandssysteem op basis van rechtstreeks gekoppelde schijven. Deze schijven zijn gekoppeld aan de virtuele machine van Azure en zijn gebaseerd in Azure pagina-BLOB-opslag (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of beheerde schijven (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Elk soort netwerkstations of externe shares zijn van de volgende Azure Bestandsservices **niet** ondersteund voor databasebestanden: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-connections-to-Microsoft-Azure-Files.aspx>

Als u van schijven op basis van de pagina Azure BLOB-opslag of beheerd schijven gebruikmaakt, de instructies in dit document in hoofdstuk aangebracht [structuur van de implementatie van een RDBMS] [ dbms-guide-2] ook van toepassing op implementaties met de IBM DB2 voor LUW De database. 

Zoals eerder in het algemene gedeelte van het document wordt uitgelegd, bestaan de quota voor IOPS-doorvoer voor schijven. De exacte quota, is afhankelijk van de VM-type gebruikt. Een lijst met VM-typen met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Als het huidige quotum IOP's per schijf voldoende is, is het mogelijk voor het opslaan van de databasebestanden op één enkele gekoppelde schijf. 

Voor prestaties verwijzen overwegingen ook naar hoofdstuk 'Veiligheid en prestaties overwegingen voor Database gegevensmappen' in de gidsen voor SAP-installatie.

U kunt ook kunt u Windows Storage Pools (alleen beschikbaar in Windows Server 2012 en hoger) of Windows striping voor Windows 2008 R2 als beschreven in hoofdstuk [Software RAID] [ dbms-guide-2.2] van dit document voor Maak één grote logisch apparaat via meerdere schijven.
Voor de schijven met de DB2-opslagpaden voor uw mappen sapdata en saptmp, moet u een fysieke Schijfsectorgrootte van 512 KB. Wanneer u Windows-opslaggroepen, moet u de opslaggroepen handmatig via de opdrachtregelinterface met de parameter '-LogicalSectorSizeDefault '. Zie voor meer informatie <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Back-up maken/terugzetten
De back-up/herstel functionaliteit voor IBM DB2 voor LUW wordt op dezelfde manier als op de standaard Windows-serverbesturingssystemen en Hyper-V ondersteund.

U moet ervoor zorgen dat u beschikken over een geldige database back-upstrategie. 

Net zoals in de bare-metal implementaties afhankelijk back-up/herstel prestaties van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes zijn. Bovendien speelt de CPU-verbruik door back-compressie gebruikt een belangrijke rol op virtuele machines met slechts maximaal acht CPU-threads. Daarom kan een aannemen:

* De minder het aantal schijven gebruikt voor het opslaan van de database-apparaten, hoe kleiner totale doorvoer in lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de invloed van back-compressie
* De minder doelen (Stripe mappen, schijven) de back-up naar de laagste de doorvoer schrijven

Voor een verhoging van het aantal doelen te schrijven naar zijn twee opties afhankelijk van uw behoeften gebruikt/gecombineerd:

* Het volume voor back-updoel striping over meerdere schijven om te verbeteren van de doorvoer IOPS op dat volume striped
* Met behulp van meer dan één doelmap schrijven van de back-up

#### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen
Microsoft Cluster Server (MSCS) wordt niet ondersteund.

DB2 herstel na noodgevallen voor hoge beschikbaarheid (HADR) wordt ondersteund. Als de virtuele machines van de HA-configuratie naamomzetting werkt hebt, verschillen de instellingen in Azure niet al van elke instelling die lokaal wordt uitgevoerd. Het verdient niet afhankelijk zijn van alleen IP-adresomzetting.

Gebruik geen Geo-replicatie voor de storage-accounts waarmee de schijven van de database worden opgeslagen. Raadpleeg voor meer informatie naar hoofdstuk [Microsoft Azure Storage] [ dbms-guide-2.3] en hoofdstuk [hoge beschikbaarheid en herstel na noodgevallen met Azure Virtual machines] [ dbms-guide-3].

#### <a name="other"></a>Overige
Andere algemene onderwerpen, zoals Beschikbaarheidssets van Azure of SAP bewaking van toepassing zoals beschreven in de eerste drie hoofdstukken van dit document voor andere implementaties van virtuele machines met IBM DB2 voor LUW. 

Ook verwijzen naar hoofdstuk [algemene SQL Server voor SAP op Azure samenvatting][dbms-guide-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Foutopsporingsgegevens naar IBM DB2 voor LUW op Linux
U kunt eenvoudig uw bestaande SAP-toepassing uitgevoerd op de IBM DB2 voor Linux, UNIX- en Windows (LUW) virtuele machines in Azure met Microsoft Azure migreren. Met SAP op IBM DB2 voor LUW, kunnen beheerders en ontwikkelaars nog steeds gebruikt de dezelfde ontwikkeling en beheerprogramma's, beschikbare on-premises. Algemene informatie over het uitvoeren van SAP Business Suite op IBM DB2 voor LUW vindt u in de SAP Community netwerk (SCN) op <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Zie voor meer informatie en updates over SAP op DB2 voor LUW op Azure, SAP-notitie [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 voor Linux, UNIX- en ondersteuning voor Windows-versie
SAP op IBM DB2 voor LUW op Services van Microsoft Azure-virtuele Machine wordt ondersteund vanaf DB2 versie 10.5.

Raadpleeg voor informatie over ondersteunde SAP-producten en typen van de virtuele machine in Azure, SAP-notitie [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 voor Linux, UNIX- en Windows-configuratie-instructies voor SAP-installaties in virtuele machines in Azure
#### <a name="storage-configuration"></a>Opslagconfiguratie
Alle databasebestanden moeten worden opgeslagen op een bestandssysteem dat is gebaseerd op rechtstreeks gekoppelde schijven. Deze schijven zijn gekoppeld aan de virtuele machine van Azure en zijn gebaseerd in Azure pagina-BLOB-opslag (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of beheerde schijven (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Elk soort netwerkstations of externe shares zijn van de volgende Azure Bestandsservices **niet** ondersteund voor databasebestanden:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-connections-to-Microsoft-Azure-Files.aspx>

Als u schijven op basis van Azure pagina BLOB Storage, de instructies die zijn aangebracht in dit document in hoofdstuk [structuur van de implementatie van een RDBMS] [ dbms-guide-2] ook van toepassing op implementaties met de IBM DB2 voor LUW-Database.

Zoals eerder in het algemene gedeelte van het document wordt uitgelegd, bestaan de quota voor IOPS-doorvoer voor schijven. De exacte quota, is afhankelijk van de VM-type gebruikt. Een lijst met VM-typen met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Als het huidige quotum IOP's per schijf voldoende is, is het mogelijk voor het opslaan van de databasebestanden op één enkele schijf.

Voor prestaties verwijzen overwegingen ook naar hoofdstuk 'Veiligheid en prestaties overwegingen voor Database gegevensmappen' in de gidsen voor SAP-installatie.

U kunt ook kunt u LVM (beheer van logische volumes) of MDADM zoals beschreven in hoofdstuk [Software RAID] [ dbms-guide-2.2] van dit document te maken van één grote logisch apparaat via meerdere schijven.
Voor de schijven met de DB2-opslagpaden voor uw mappen sapdata en saptmp, moet u een fysieke Schijfsectorgrootte van 512 KB.

#### <a name="backuprestore"></a>Back-up maken/terugzetten
De back-up/herstel functionaliteit voor IBM DB2 voor LUW wordt ondersteund op dezelfde manier als op standaard Linux installatie on-premises.

U moet ervoor zorgen dat u beschikken over een geldige database back-upstrategie.

Net zoals in de bare-metal implementaties afhankelijk back-up/herstel prestaties van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes zijn. Bovendien speelt de CPU-verbruik door back-compressie gebruikt een belangrijke rol op virtuele machines met slechts maximaal acht CPU-threads. Daarom kan een aannemen:

* De minder het aantal schijven gebruikt voor het opslaan van de database-apparaten, hoe kleiner totale doorvoer in lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de invloed van back-compressie
* De minder doelen (Stripe mappen, schijven) de back-up naar de laagste de doorvoer schrijven

Voor een verhoging van het aantal doelen te schrijven naar zijn twee opties afhankelijk van uw behoeften gebruikt/gecombineerd:

* Het volume voor back-updoel striping over meerdere schijven om te verbeteren van de doorvoer IOPS op dat volume striped
* Met behulp van meer dan één doelmap schrijven van de back-up

#### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen
DB2 herstel na noodgevallen voor hoge beschikbaarheid (HADR) wordt ondersteund. Als de virtuele machines van de HA-configuratie naamomzetting werkt hebt, verschillen de instellingen in Azure niet al van elke instelling die lokaal wordt uitgevoerd. Het verdient niet afhankelijk zijn van alleen IP-adresomzetting.

Gebruik geen Geo-replicatie voor de storage-accounts waarmee de schijven van de database worden opgeslagen. Raadpleeg voor meer informatie naar hoofdstuk [Microsoft Azure Storage] [ dbms-guide-2.3] en hoofdstuk [hoge beschikbaarheid en herstel na noodgevallen met Azure Virtual machines] [ dbms-guide-3].

#### <a name="other"></a>Overige
Andere algemene onderwerpen, zoals Beschikbaarheidssets van Azure of SAP bewaking van toepassing zoals beschreven in de eerste drie hoofdstukken van dit document voor andere implementaties van virtuele machines met IBM DB2 voor LUW.

Ook verwijzen naar hoofdstuk [algemene SQL Server voor SAP op Azure samenvatting][dbms-guide-5.8].

