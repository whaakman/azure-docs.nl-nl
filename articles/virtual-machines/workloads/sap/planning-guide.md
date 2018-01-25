---
title: Azure virtuele Machines, planning en implementatie voor SAP NetWeaver | Microsoft Docs
description: Azure virtuele Machines, planning en implementatie voor SAP NetWeaver
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bf9f676b48f25ae2d8949dbdba8b4792b05c67f0
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure virtuele Machines, planning en implementatie voor SAP NetWeaver
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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
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
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md
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
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
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
[virtual-network-deploy-multinic-arm-cli]:../../linux/multiple-nics.md
[virtual-network-deploy-multinic-arm-ps]:../../windows/multiple-nics.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure kunnen bedrijven berekenings- en bronnen in de minimale tijd zonder langdurige inkoop cycli te verkrijgen. Virtuele Machines in Azure kunnen bedrijven klassieke om toepassingen te implementeren, zoals SAP NetWeaver toepassingen in Azure gebaseerde en de betrouwbaarheid en beschikbaarheid zonder verdere beschikbaar lokale uitbreiden. Virtuele Machine van Azure-Services biedt ook ondersteuning voor cross-premises connectiviteit, waardoor bedrijven actief Azure Virtual Machines integreren in hun lokale domein, hun Privéclouds en hun liggend SAP-systeem.
Dit document beschrijft de basisprincipes van Microsoft Azure virtuele Machine en biedt een overzicht van de overwegingen voor planning en implementatie voor SAP NetWeaver installaties in Azure en als zodanig moet het document te lezen voordat u begint de werkelijke implementaties van SAP NetWeaver op Azure.
Het artikel is een aanvulling op de SAP installatie documentatie en SAP-opmerkingen primaire resources voor installaties en -implementaties van SAP-software op de opgegeven platforms vertegenwoordigen.

## <a name="summary"></a>Samenvatting
Cloudcomputing is een veelgebruikte term die meer belang binnen de IT-branche van kleine bedrijven tot grote en meertalige bedrijven winnen wordt.

Microsoft Azure is het Cloudplatform van de Services van Microsoft, die een breed scala aan nieuwe mogelijkheden biedt. Nu kunnen klanten snel leveren en intrekken van toepassingen als een service in de cloud, zodat ze niet beperkt tot technische of budget beperkingen zijn. In plaats van de tijd en geld investeren in hardware-infrastructuur, bedrijven kunnen zich concentreren op de toepassing, bedrijfsprocessen en de voordelen voor klanten en gebruikers.

Met Microsoft Azure-services voor virtuele machines biedt Microsoft een uitgebreid IaaS-platform (Infrastructure as a Service). SAP NetWeaver-toepassingen worden ondersteund op virtuele Azure-machines (IaaS). Dit technisch document wordt beschreven hoe plannen en implementeren van SAP NetWeaver gebaseerde toepassingen binnen Microsoft Azure als het platform van keuze.

Het artikel zelf is gericht op twee belangrijke aspecten:

* Het eerste gedeelte beschrijft de twee ondersteunde implementaties voor SAP NetWeaver op basis van toepassingen in Azure. Hierin worden ook algemene verwerking van Azure met SAP-implementaties in gedachten.
* De tweede onderdeel details uitvoering van de twee verschillende scenario's beschreven in het eerste deel.

Voor aanvullende bronnen Zie hoofdstuk [Resources] [ planning-guide-1.2] in dit document.

### <a name="definitions-upfront"></a>Definities van tevoren
In het hele document gebruiken we de volgende termen:

* IaaS: Infrastructuur als een Service
* PaaS: Platform as a Service
* SaaS: Software als een Service
* ARM: Azure Resource Manager
* SAP-onderdeel: een afzonderlijke SAP toepassing zoals ECC, BW, oplossing Manager of EP is geplaatst.  SAP-onderdelen kunnen worden gebaseerd op traditionele ABAP of Java-technologieën of een NetWeaver op basis van toepassing zoals zakelijke objecten.
* SAP-omgeving: een of meer onderdelen voor SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke functie zoals ontwikkeling, QAS, Training, DR of productie.
* SAP liggend: Dit verwijst naar de gehele SAP-elementen in een klant IT Liggend. De SAP-liggend bevat alle productie en niet-productieve omgevingen.
* SAP-systeem: De combinatie van laag DBMS en toepassingslaag van bijvoorbeeld een SAP ERP-ontwikkelsysteem, SAP BW testsysteem, productiesysteem SAP CRM, enzovoort.. In implementaties van Azure, is het niet ondersteund voor het delen van deze twee lagen tussen on-premises en Azure. Dit betekent dat er ofwel een SAP-systeem lokale geïmplementeerd of deze is geïmplementeerd in Azure. U kunt echter de verschillende systemen van een liggend SAP in Azure of on-premises implementeren. U kan bijvoorbeeld de CRM SAP-ontwikkeling implementeren en testen van systemen in Azure, maar de SAP CRM productie system on-premises.
* Cloud-implementatie: een implementatie waarbij het Azure-abonnement niet is verbonden via een site-naar-site of een ExpressRoute-verbinding met de on-premises netwerk-infrastructuur. Gemeenschappelijk Azure-documentatie dergelijke implementaties worden ook beschreven als 'Alleen in de Cloud' implementaties. Virtuele Machines die worden geïmplementeerd met deze methode zijn toegankelijk via het internet en een openbare IP-adres en/of een openbare DNS-naam toegewezen aan de virtuele machines in Azure. Voor Microsoft Windows, de lokale Active Directory (AD) en DNS is niet uitgebreid naar Azure in dergelijke implementaties. Daarom is de virtuele machines maken geen deel uit van de lokale Active Directory. Hetzelfde geldt voor Linux-implementaties gebruikt, bijvoorbeeld OpenLDAP + Kerberos.

> [!NOTE]
> Cloud-implementatie in dit document wordt gedefinieerd als volledige SAP landschappen uitsluitend in Azure zonder extensie van Active Directory worden uitgevoerd / OpenLDAP of naamomzetting van de van on-premises in openbare cloud. Alleen in de cloud-configuraties worden niet ondersteund voor productie SAP-systemen of -configuraties waar SAP stm of andere lokale bronnen moeten worden gebruikt tussen SAP-systemen die worden gehost op Azure en bronnen die zich lokaal.
>
>

* Cross-premises: Beschrijft een scenario waarin virtuele machines zijn geïmplementeerd met een Azure-abonnement met site-naar-site meerdere locaties of ExpressRoute-verbinding tussen de lokale clientresources en Azure. Gemeenschappelijk Azure-documentatie, dit soort implementaties worden ook beschreven als cross-premises scenario's. De reden voor de verbinding is om lokale domeinen, lokale Active Directory/OpenLDAP en lokale DNS-uitbreiden naar Azure. De lokale Liggend wordt uitgebreid naar de Azure activa van het abonnement. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van het lokale domein. Domeingebruikers van het lokale domein hebben toegang tot de servers en services kunnen worden uitgevoerd op deze virtuele machines (zoals DBMS services). Communicatie en naamomzetting tussen de geïmplementeerde virtuele machines on-premises en geïmplementeerde virtuele machines in Azure is mogelijk. Dit is het scenario dat we verwachten dat de meeste SAP activa te worden geïmplementeerd in. Zie voor meer informatie [dit] [ vpn-gateway-cross-premises-options] artikel en [dit][vpn-gateway-site-to-site-create].

> [!NOTE]
> Cross-premises implementaties van waar Azure Virtual Machines SAP computers lid van een lokaal domein zijn SAP-systemen worden ondersteund voor productie SAP-systemen. Cross-premises configuraties worden ondersteund voor het implementeren van onderdelen of SAP landschappen in Azure te voltooien. De volledige SAP liggend zelfs worden uitgevoerd in Azure vereist dat deze VMs deel van het lokale domein en ADVERTENTIES/OpenLDAP. In eerdere versies van de documentatie die eerder genoemde over hybride IT-scenario's, waarbij de term *hybride* verankerd ligt in het feit dat er een cross-premises-connectiviteit tussen on-premises en Azure is. Plus het feit dat de virtuele machines in Azure deel van de lokale Active Directory uitmaken / OpenLDAP.
>
>

Sommige Microsoft-documentatie worden scenario's voor cross-premises iets anders, met name voor DBMS HA configuraties beschreven. In het geval van de documenten SAP-gerelateerde het scenario voor cross-premises net komt meestal neer op een site-naar-site of privé (ExpressRoute)-connectiviteit en het feit dat de SAP-Liggend wordt verdeeld tussen on-premises en Azure.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Resources
De volgende aanvullende handleidingen zijn beschikbaar voor het onderwerp van SAP-implementaties op Azure:

* [Azure virtuele Machines, planning en implementatie voor SAP NetWeaver (in dit document)][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP NetWeaver][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]

> [!IMPORTANT]
> Waar mogelijk een koppeling naar de verwijzende SAP-installatiehandleiding wordt gebruikt (verwijzing InstGuide-01, Zie <http://service.sap.com/instguides>). Als het gaat om de vereisten en installatie, moet de installatiehandleidingen voor SAP NetWeaver altijd worden gelezen zorgvuldig, als dit document alleen specifieke taken voor SAP NetWeaver systemen in een Microsoft Azure virtuele Machine geïnstalleerd bevat.
>
>

De volgende opmerkingen bij de SAP zijn gerelateerd aan het onderwerp van SAP op Azure:

| Het nummer | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen in Azure: ondersteunde producten en schaling |
| [2015553] |SAP op Microsoft Azure: ondersteuning voor vereisten |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel bewaking van metrische gegevens voor SAP op Microsoft Azure |
| [1409604] |Virtualisatie in Windows: uitgebreide bewaking |
| [2191498] |SAP op Linux met Azure: uitgebreide bewaking |
| [2243692] |Linux op Microsoft Azure (IaaS) VM: problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [2002167] |Red Hat Enterprise Linux 7.x: installatie en Upgrade |
| [2069760] |Oracle Linux 7.x SAP-installatie en Upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |

Lees ook de [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) die alle SAP notities voor Linux bevat.

Algemene standaardbeperkingen en de maximale beperkingen van de Azure-abonnementen kunnen worden gevonden [in dit artikel][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Mogelijke scenario 's
SAP wordt vaak gebruikt als een van de meest cruciale toepassingen binnen ondernemingen. De architectuur en bewerkingen van deze toepassingen is voornamelijk zeer complex en het is belangrijk ervoor te zorgen dat u voldoet aan de vereisten voor de beschikbaarheid en prestaties.

Bedrijven hebben dus zorgvuldig over welke toepassingen kunnen worden uitgevoerd in een openbare cloud-omgeving, onafhankelijk van de gekozen cloudprovider denken.

Mogelijke systeemtypes voor het implementeren van SAP NetWeaver gebaseerde toepassingen binnen de openbare cloud omgevingen worden hieronder weergegeven:

1. Middelgrote productiesystemen
2. Ontwikkeling van systemen
3. Testen van systemen
4. Prototypesystemen
5. Learning / demonstratie systemen

Als u wilt implementeren SAP-systemen in Azure IaaS of IaaS in het algemeen, is het belangrijk om te begrijpen van de belangrijke verschillen tussen de offerings van traditionele uitbesteders of hosters en IaaS-aanbod. Terwijl de traditionele hoster of outsourcer infrastructuur (type netwerk, opslag en server) voor de werkbelasting die van de klant wil hosten aanpast, is het in plaats daarvan de verantwoordelijkheid van de klant de juiste werkbelasting voor IaaS-implementaties te kiezen.

Klanten moeten als eerste stap om te controleren of de volgende items:

* De SAP ondersteunde typen van de virtuele machine van Azure
* De SAP ondersteunde producten/versies in Azure
* Ondersteunde besturingssystemen en DBMS versies voor de specifieke SAP-versies in Azure
* SAP's doorvoer geleverd door verschillende Azure-SKU 's

De antwoorden op deze vragen kunnen worden gelezen in SAP-notitie [1928533].

Als een tweede stap van de nodig beperkingen voor Azure resource en bandbreedte moet worden vergeleken met de werkelijke brongebruik van on-premises systemen. Daarom moeten klanten bekend zijn met de verschillende mogelijkheden van de Azure-typen ondersteund met SAP in het gebied van:

* CPU en geheugenbronnen kost van verschillende typen van de virtuele machine en
* IOPS bandbreedte van verschillende typen van de virtuele machine en
* Mogelijkheden van verschillende typen van de VM-netwerk.

De meeste van deze gegevens vindt u [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Houd er rekening mee dat de grenzen die worden vermeld in de bovenstaande koppeling bovengrenzen zijn. Het betekent niet dat de limieten voor de bronnen, kan bijvoorbeeld IOP's worden opgegeven onder alle omstandigheden. De uitzonderingen zijn echter de CPU en geheugen resources van een gekozen VM-type. Voor de virtuele machine die worden ondersteund door SAP, zijn de CPU en geheugenbronnen kost gereserveerd en als zodanig beschikbaar zijn op elk gewenst moment in de tijd voor consumptie in de virtuele machine.

Het Microsoft Azure-platform, zoals andere platforms IaaS is een multitenant-platform. Dit betekent dat de opslag-, netwerk- en andere resources worden gedeeld tussen de tenants. Intelligent bandbreedtebeperking en quota logica wordt gebruikt om te voorkomen dat een tenant die invloed hebben op de prestaties van een andere tenant (ruis neighbor) in een ingrijpende manier. Hoewel de logica in Azure probeert te houden afwijkingen bandbreedte heeft ondergaan vaak kleine, maximaal gedeelde platforms groter afwijkingen in de beschikbaarheid van de resource/bandbreedte dan veel klanten in hun on-premises-implementaties worden gebruikt om te introduceren. Als gevolg hiervan kunnen er verschillende niveaus van bandbreedte met betrekking tot het netwerk of de opslag-i/o (het volume, alsmede latentie) minuut minuut. De kans dat een SAP-systeem op Azure groter afwijkingen dan in een on-premises systeem optreden kan moet rekening worden gehouden.

Een laatste stap is het evalueren van de van beschikbaarheidsvereisten. Dit kan gebeuren, dat de onderliggende Azure-infrastructuur moet worden bijgewerkt en vereist dat de hosts met virtuele machines opnieuw worden opgestart. In deze gevallen zou VM's worden uitgevoerd op deze hosts worden uitgeschakeld en opnieuw opgestart ook. De timing van dergelijke onderhoud wordt uitgevoerd tijdens de kantooruren core-voor een bepaalde regio, maar het potentiële venster van een paar uur waarover een wordt opnieuw opgestart is relatief groot. Er zijn verschillende technologieën binnen de Azure-platform die kunnen worden geconfigureerd om te beperken sommige of alle van de impact van dergelijke updates. Toekomstige verbeteringen van de Azure-platform, DBMS en SAP toepassing zijn ontworpen om de impact van dergelijke opnieuw wordt opgestart.

Als u wilt implementeren naar Azure een SAP-systeem, de on-premises SAP-systemen besturingssysteem, Database, en SAP-toepassingen moeten worden weergegeven op de ondersteuningsmatrix SAP Azure passen binnen de Azure-resources infrastructuur kunt bieden en die kunnen werken met de beschikbaarheid serviceovereenkomsten Microsoft Azure-aanbiedingen. Deze systemen worden aangeduid, moet u beslissen op een van de volgende twee implementatiescenario's.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Alleen in de cloud - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk
![Één virtuele machine met SAP-demo of training scenario geïmplementeerd in Azure][planning-guide-figure-100]

Dit scenario is gebruikelijk om trainingen of demo-systemen, waarop alle onderdelen van SAP en niet-SAP-software in een enkele virtuele machine zijn geïnstalleerd. Productie SAP-systemen worden niet ondersteund in dit implementatiescenario. Dit scenario aan in het algemeen de volgende vereisten voldoet:

* De VM's zelf zijn toegankelijk via het openbare netwerk. Rechtstreekse netwerkverbinding voor de toepassingen die worden uitgevoerd vanuit de virtuele machines naar de on-premises netwerk van ofwel het bedrijf die eigenaar is van de demo's of trainingen inhoud of de klant is niet nodig.
* In geval van meerdere virtuele machines voor de trainingen of demo scenario moet netwerk communicatie en naamomzetting tussen de virtuele machines werken. Maar communicatie tussen de set van virtuele machines moeten worden geïsoleerd, zodat verschillende sets van virtuele machines naast elkaar kunnen worden geïmplementeerd zonder tussenkomst.  
* Verbinding met Internet is vereist voor de eindgebruiker extern aanmelden in op de virtuele machines die worden gehost in Azure. Afhankelijk van de Gast wordt OS, Terminal RDS-Services of VNC/ssh gebruikt voor toegang tot de virtuele machine voor het voltooien van de taken training of de demo's uitvoeren. Als SAP zoals 3200, 3300 & 3600 kan poorten ook worden blootgesteld het toepassingsexemplaar SAP toegankelijk zijn vanaf elke Internet verbonden desktop.
* De SAP-systemen (en VM(s)) vertegenwoordigen een zelfstandige scenario in Azure, dit alleen openbare verbinding met internet vereist voor toegang door eindgebruikers en vereist geen verbinding met andere virtuele machines in Azure.
* SAPGUI en een browser zijn geïnstalleerd en uitgevoerd rechtstreeks op de virtuele machine.
* Snel opnieuw instellen van een virtuele machine naar de oorspronkelijke status en de nieuwe implementatie van die oorspronkelijke status opnieuw is vereist.
* In het geval van demo en training scenario's die zijn gerealiseerde in meerdere virtuele machines, een Active Directory-OpenLDAP en/of de DNS-service is vereist voor elke set van virtuele machines.

![Groep van de virtuele machine die een demo of training scenario in een Azure Cloud Service][planning-guide-figure-200]

Het is belangrijk dat er rekening mee dat de VM('s) in elk van de sets moeten worden geïmplementeerd parallel waar de VM-namen in elk van de set hetzelfde zijn.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Cross-Premises - implementatie van één of meerdere SAP-virtuele machines in Azure met de vereiste van volledig geïntegreerd in de on-premises netwerk
![VPN met Site-naar-Site-connectiviteit (cross-premises)][planning-guide-figure-300]

Dit scenario is een scenario voor cross-premises met veel mogelijke implementaties. Het kan zo eenvoudig als lokale uitgevoerd sommige onderdelen van het SAP liggend en andere onderdelen van het SAP Liggend in Azure worden beschreven. Alle aspecten van het feit dat deel uitmaakt van de SAP-onderdelen worden uitgevoerd op Azure moet transparant voor eindgebruikers. Daarom werken de SAP Transport correctie System (STM's), RFC communicatie, afdrukken, beveiliging, (zoals SSO), enz. naadloos voor de SAP-systemen uitgevoerd op Azure. Maar het cross-premises-scenario wordt ook beschreven voor een scenario waarbij de volledige SAP Liggend wordt uitgevoerd in Azure met de klant domein en DNS uitgebreid naar Azure.

> [!NOTE]
> Dit is het implementatiescenario voor het uitvoeren van productief SAP-systemen wordt ondersteund.
>
>

Lees [in dit artikel] [ vpn-gateway-create-site-to-site-rm-powershell] voor meer informatie over verbinding maken tussen uw on-premises netwerk en Microsoft Azure

> [!IMPORTANT]
> Wanneer we wordt gesproken over cross-premises-scenario's tussen Azure en on-premises implementaties van klanten, kijkt we de granulatie van de gehele SAP-systemen. Scenario's worden *niet ondersteund* voor cross-premises scenario's:
>
> * Verschillende lagen van SAP-toepassingen in verschillende implementatiemethoden uitgevoerd. De DBMS laag on-premises, maar de toepassingslaag SAP bijvoorbeeld uitgevoerd in de virtuele machines die worden geïmplementeerd als Azure virtuele machines of vice versa.
> * Sommige onderdelen van een laag SAP in Azure en een on-premises. Bijvoorbeeld splitsen exemplaren van de toepassingslaag SAP tussen on-premises en Azure VM's.
> * Distributie van virtuele machines met SAP-exemplaren van een systeem over meerdere Azure-regio's wordt niet ondersteund.
>
> De reden voor deze beperkingen is de vereiste voor een zeer lage latentie krachtige netwerk binnen een SAP-systeem, met name tussen de toepassingsexemplaren en de DBMS-laag van een SAP-systeem.
>
>

### <a name="supported-os-and-database-releases"></a>Ondersteund besturingssysteem en versies van de Database
* Microsoft-serversoftware ondersteund voor Services van Azure virtuele Machine wordt vermeld in dit artikel: <http://support.microsoft.com/kb/2721672>.
* Ondersteund besturingssysteem system releases, database system releases in combinatie met SAP-software wordt ondersteund door Azure virtuele Machine Services zijn gedocumenteerd in SAP-notitie [1928533].
* SAP-toepassingen en versies die worden ondersteund op Azure Virtual Machine-Services zijn gedocumenteerd in SAP-notitie [1928533].
* Alleen 64-bits installatiekopieën worden ondersteund voor uitvoeren als gast-VM's in Azure voor SAP-scenario's. Dit betekent ook dat alleen 64-bits SAP-toepassingen en databases worden ondersteund.

## <a name="microsoft-azure-virtual-machine-services"></a>Services van Microsoft Azure-virtuele Machine
Het Microsoft Azure-platform is een internet-scale cloud services-platform gehost en beheerd in Microsoft-datacenters. Het platform omvat de Services van Microsoft Azure virtuele Machine (infrastructuur als een Service of IaaS) en een aantal uitgebreide Platform als een Service (PaaS)-mogelijkheden.

De Azure-platform vermindert de noodzaak om vooraf technologie en infrastructuur koopt. Het vereenvoudigt onderhouden en toepassingen door op te geven op aanvraag berekeningen en opslag hosten, het schalen en het beheren van de webtoepassing en verbonden toepassingen functioneren. Beheer van infrastructuur is geautomatiseerde met een platform dat is ontworpen voor hoge beschikbaarheid en dynamische schalen zodat deze overeenkomen met de moeten gebruiken met de optie om een betalen naar gebruik prijsmodel.

![Plaatsing van de Services van Microsoft Azure-virtuele Machine][planning-guide-figure-400]

Met Azure Virtual Machine-Services zodat Microsoft u kunt aangepaste installatiekopieën implementeren op Azure als IaaS-instanties (Zie afbeelding 4). De virtuele Machines in Azure zijn gebaseerd op Hyper-V virtuele harde schijven (VHD) en kunnen worden uitgevoerd van verschillende besturingssystemen worden uitgevoerd als het Gastbesturingssysteem.

Van een operationeel perspectief biedt de virtuele Machine Azure Service vergelijkbare ervaring als virtuele machines die on-premises worden geïmplementeerd. Dit heeft echter aanzienlijke voordeel dat u hoeft niet te schaffen, te beheren en de infrastructuur te beheren. Ontwikkelaars en beheerders hebben volledig beheer van de besturingssysteemkopie binnen deze virtuele machines. Beheerders kunnen zich aanmelden op afstand op die virtuele machines uitvoeren voor onderhoud en probleemoplossing van taken, evenals de taken van software-implementatie. De enige beperkingen zijn met betrekking tot implementatie van de grootte en de mogelijkheden van Azure Virtual machines. Deze mogelijk niet als fijn gedetailleerde in configuratie, zoals dit on-premises kan worden uitgevoerd. Er is een keuze uit de VM-typen die een combinatie van vertegenwoordigen:

* Aantal Vcpu,
* Geheugen,
* Aantal virtuele harde schijven die kunnen worden gekoppeld,
* Netwerk en opslag bandbreedten.

De grootte en de beperkingen van verschillende grootten voor verschillende virtuele machines die worden aangeboden kunnen worden weergegeven in een tabel in [in dit artikel (Linux)] [ virtual-machines-sizes-linux] en [in dit artikel (Windows)] [virtual-machines-sizes-windows].

Als u beseft, zijn er verschillende families of reeks van virtuele machines. U kunt de volgende families van virtuele machines kunt onderscheiden:

* A0 A7 VM typen: niet alle die zijn gecertificeerd voor SAP. Eerste VM-reeks die Azure IaaS is geïntroduceerd in.
* A8-A11-VM-typen: High Performance computing-exemplaren. Wordt uitgevoerd voor het uitvoeren van verschillende betere compute hosts dan andere virtuele machines van A-serie.
* Dv2-D-reeks VM typen: beter dan A0 A7 uitvoeren. Niet alle van de VM-typen zijn gecertificeerd met SAP.
* DS-DSv2-serie VM typen: vergelijkbaar met Dv2-D-reeks, maar zijn geen verbinding maken met Azure Premium-opslag (Zie hoofdstuk [Azure Premium-opslag] [ planning-guide-3.3.2] van dit document). Opnieuw niet alle VM-typen zijn gecertificeerd met SAP.
* G-serie VM typen: veel geheugen VM typen.
* Typen GS-serie VM: G-serie, zoals maar met inbegrip van de optie voor het gebruik van Azure Premium-opslag (Zie hoofdstuk [Azure Premium-opslag] [ planning-guide-3.3.2] van dit document). Wanneer u virtuele machines GS-serie als databaseservers, is dit verplicht met Premium-opslag voor DB gegevens en transactie-logboekbestanden

U vindt de dezelfde CPU en geheugenconfiguraties in andere VM-reeks. Evenwel bij het opzoeken van de prestaties van de doorvoer van deze virtuele machines buiten de verschillende reeks verschillen ze aanzienlijk. Ondanks dat de dezelfde configuratie van de CPU en geheugen. Reden is dat de onderliggende host-serverhardware op de introductie van de verschillende typen van de VM verschillende doorvoer heeft.  Meestal wordt het verschil in prestaties van de netwerkdoorvoer weergegeven ook doorgevoerd in de prijs van de andere virtuele machines.

Niet alle andere VM-reeks kan worden aangeboden in elk van de Azure-regio's (voor Azure-gebieden Zie volgende hoofdstuk). Zich bewust te zijn dat niet alle virtuele machines of VM-serie zijn gecertificeerd voor SAP.

> [!IMPORTANT]
> Voor het gebruik van SAP NetWeaver op basis van toepassingen, alleen de subset van de VM-typen en configuraties die worden vermeld in SAP-notitie [1928533] worden ondersteund.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-regio 's
Microsoft kan voor het implementeren van virtuele Machines in zogenaamde *Azure-gebieden*. Een Azure-regio is mogelijk een of meerdere datacenters die zich dicht bevinden. Microsoft heeft voor de meeste van de geopolitieke regio's in de wereld ten minste twee Azure-regio's. Bijvoorbeeld, in Europa er is een Azure-regio van *Noord-Europa* en een van *West-Europa*. Deze twee Azure-regio's binnen een geopolitieke regio worden gescheiden door aanzienlijke genoeg afstand zodat natuurlijke of technische noodsituaties hebben geen invloed op zowel Azure-gebieden in dezelfde geopolitieke regio. Aangezien Microsoft gestaag uit nieuwe Azure-regio's in andere geopolitieke regio's globaal bouwt, wordt het nummer van deze gebieden gestaag groeit en het aantal 20 Azure-regio's met extra gebieden aangekondigd al bereikt vanaf december 2015. In alle deze gebieden, met inbegrip van de twee Azure-regio's in China, kunt u als klant SAP-systemen implementeren. Zie voor de huidige actuele informatie over Azure-regio's deze website: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Het Concept van Microsoft Azure virtuele Machine
Microsoft Azure biedt een infrastructuur als een Service (IaaS) naar host voor virtuele Machines met vergelijkbare functionaliteit als een on-premises-oplossing voor netwerkvirtualisatie. U bent kunnen maken van virtuele Machines van binnen de Azure-portal, PowerShell of CLI, die ook implementatie en beheermogelijkheden bieden.

Met Azure Resource Manager kunt u uw toepassingen inrichten aan de hand van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. Dezelfde sjabloon kunt u uw toepassing tijdens elke fase van de levenscyclus van de toepassing herhaaldelijk te implementeren.

Meer informatie over het gebruik van Resource Manager-sjablonen vindt u hier:

* [Implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en de Azure CLI] [../../linux/create-ssh-secured-vm-from-template.md]
* [Virtuele machines beheren met Azure Resource Manager en PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.Microsoft.com/Documentation/templates/>

Een andere interessante functie is de mogelijkheid om u te maken van installatiekopieën van virtuele Machines, waarmee u bepaalde opslagplaatsen van waaruit u kunt snel implementeren instanties van de virtuele machine, die voldoen aan uw vereisten zijn voorbereiden.

Meer informatie over het maken van installatiekopieën van virtuele Machines kunt u vinden [in dit artikel (Linux)] [ virtual-machines-linux-capture-image-resource-manager] en [in dit artikel (Windows)] [ virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Domeinen met fouten
Domeinen met fouten vertegenwoordigen een fysieke eenheid mislukt, zeer nauw verband houden met de fysieke infrastructuur die is opgenomen in datacenters en een fysieke blade of rack kan weliswaar worden beschouwd als een domein met fouten, is er geen rechtstreekse-op-een koppeling tussen de twee.

Wanneer u meerdere virtuele Machines als onderdeel van een SAP-systeem in de Services van Microsoft Azure-virtuele Machine implementeert, kunt u de Azure-Infrastructuurcontroller voor het implementeren van uw toepassing in verschillende domeinen met fouten, waardoor aan de eisen van de Microsoft Azure SLA bepalen. De distributie van domeinen met fouten over een Azure-schaaleenheid (verzameling van honderden rekenknooppunten of opslagknooppunten en netwerken) of de toewijzing van virtuele machines naar een specifieke Foutdomein is echter iets waarover u geen directe controle hebt. U moet de controller Azure-infrastructuur voor het implementeren van een set van virtuele machines via verschillende domeinen met fouten direct, een Azure-Beschikbaarheidsset toewijzen aan de virtuele machines tijdens de implementatie. Zie voor meer informatie over Azure Beschikbaarheidssets hoofdstuk [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] in dit document.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Upgradedomeinen
Upgradedomeinen vertegenwoordigen een logische eenheid die u helpen te bepalen hoe een virtuele machine binnen een SAP-systeem, die uit een SAP-instanties die actief zijn in meerdere virtuele machines bestaat, wordt bijgewerkt. Wanneer een upgrade optreedt, wordt Microsoft Azure gaat door het proces voor het bijwerken van deze Upgradedomeinen één voor één. Door virtuele machines tijdens de implementatie spreiden over verschillende domeinen upgraden, kunt u uw systeem SAP beveiligen gedeeltelijk uit de mogelijke downtime. Om af te dwingen Azure voor het implementeren van de virtuele machines van een SAP-systeem verdeeld over verschillende domeinen upgraden, moet u een specifiek kenmerk ingesteld tijdens de implementatie van elke virtuele machine. Net als bij domeinen met fouten, een Azure-schaaleenheid is onderverdeeld in meerdere domeinen upgraden. Om te leiden de controller Azure-infrastructuur voor het implementeren van een set van virtuele machines via verschillende domeinen upgraden, moet u een Azure-Beschikbaarheidsset toewijzen aan de virtuele machines tijdens de implementatie. Zie voor meer informatie over Azure Beschikbaarheidssets hoofdstuk [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] hieronder.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure Beschikbaarheidssets
Azure virtuele Machines in de Beschikbaarheidsset van een Azure worden door de Azure-Infrastructuurcontroller verdeeld over verschillende domeinen met fouten en domeinen upgraden. Het doel van de verdeling over de verschillende domeinen met fouten en domeinen upgraden is om te voorkomen dat alle virtuele machines van een SAP-systeem wordt afgesloten in het geval van onderhoud van infrastructuur of een storing in een domein met fouten. Virtuele machines zijn standaard niet deel uit van een Beschikbaarheidsset. De deelname van een virtuele machine in een Beschikbaarheidsset is gedefinieerd tijdens de implementatie of hoger op door een herconfiguratie en een nieuwe implementatie van een virtuele machine.

Om het concept van Beschikbaarheidssets van Azure en de manier waarop Beschikbaarheidssets hebben betrekking op fouten en Upgrade domeinen te begrijpen, lezen [in dit artikel][virtual-machines-manage-availability]

Voor het definiëren van beschikbaarheidssets voor ARM via een json-sjabloon Zie [rest-api-specificaties](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) en zoek naar 'beschikbaarheid'.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Opslag: De Microsoft Azure Storage en de gegevensschijven
Microsoft Azure Virtual Machines gebruikmaken van verschillende opslagtypen. Bij het implementeren van SAP op Azure Virtual Machine-Services, is het belangrijk te begrijpen van de verschillen tussen deze twee typen opslag:

* Niet-permanente, vluchtige opslag.
* Permanente opslag.

De niet-permanente opslag rechtstreeks is gekoppeld aan de virtuele Machines en bevindt zich op de rekenknooppunten zelf, de opslag lokaal exemplaar (tijdelijke opslag). De grootte is afhankelijk van de grootte van de virtuele Machine gekozen wanneer de implementatie is gestart. Dit opslagtype is vluchtige en daarom de schijf wordt geïnitialiseerd wanneer een exemplaar van de virtuele Machine opnieuw wordt opgestart. Het wisselbestand voor het besturingssysteem bevindt zich doorgaans op deze tijdelijke schijf.

- - -
> ![Windows][Logo_Windows] Windows
>
> Op Windows-VM's is het tijdelijke station gekoppeld als station D:\ in een geïmplementeerde virtuele machine.
>
> ![Linux][Logo_Linux] Linux
>
> Het gekoppeld op Linux virtuele machines, als /mnt/resource of mnt. Zie hier voor meer informatie:
>
> * [Hoe een gegevensschijf koppelen aan een virtuele Linux-Machine][virtual-machines-linux-how-to-attach-disk]
> * <https://docs.Microsoft.com/Azure/Storage/Storage-About-Disks-and-vhds-Linux#Temporary-Disk>
>
>

- - -
Het feitelijke station is vluchtige omdat deze ophalen op de hostserver zelf opgeslagen is. Als de virtuele machine in een nieuwe installatie verplaatst (bijvoorbeeld door onderhoud op de host of afsluiten en opnieuw opstarten) de inhoud van het station is verbroken. Het is daarom niet een optie voor het opslaan van alle belangrijke gegevens op dit station. Het type media dat wordt gebruikt voor dit type opslag verschilt voor andere VM-reeks met zeer verschillende prestatiekenmerken die vanaf juni 2015 er als volgt uitzien:

* A5-A7: Zeer beperkt prestaties. Niet aanbevolen voor alles afgezien van wisselbestand
* A8-A11: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde.
* D-reeks: Kenmerken, zeer goede prestaties met sommige vervolgens duizenden IOPS en > doorvoer van 1GB per seconde.
* DS-serie: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde.
* G-serie: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde.
* GS-serie: Kenmerken met een aantal IOPS tienduizend zeer goede prestaties en > doorvoer van 1GB per seconde.

Instructies hierboven toepast op de VM-typen die zijn gecertificeerd met SAP. De VM-reeks met uitstekende IOPS en doorvoerlimieten in aanmerking voor gebruik door sommige DBMS-functies. Zie voor meer informatie de [DBMS Deployment Guide][dbms-guide].

Microsoft Azure Storage biedt permanente opslag en de normale niveaus van beveiliging en redundantie weergegeven op de SAN-opslag. Schijven op basis van Azure Storage zijn virtuele harde schijf (VHD's) zich in de Azure Storage-Services. De lokale OS-schijf (C: Windows\, Linux/dev/sda1) wordt opgeslagen op de Azure-opslag, en extra Volumes/schijven die zijn gekoppeld aan de virtuele machine ophalen, opgeslagen te.

Het is mogelijk uploaden van een bestaande VHD on-premises of lege mappen uit in Azure maken en koppelen van de geïmplementeerde virtuele machines.

Na het maken of een VHD uploaden naar Azure Storage, is het mogelijk om te koppelen en deze naar een bestaande virtuele Machine te koppelen en bestaande (ontkoppeld) VHD kopiëren.

Als deze VHD's zijn persistent hebt gemaakt, zijn gegevens en wijzigingen in de veilige wanneer opnieuw opstarten en opnieuw maken van een exemplaar van de virtuele Machine. Zelfs als een exemplaar wordt verwijderd, deze VHD's veilig kunnen worden geïmplementeerd en in geval van een niet-OS schijven kunnen worden gekoppeld aan andere virtuele machines.

In het netwerk van Azure Storage kunnen redundantie van de verschillende niveaus worden geconfigureerd:

* Minimaal niveau hebben die kan worden geselecteerd is *lokale redundantie*, hetgeen gelijk is aan drie-replica van de gegevens binnen hetzelfde datacenter van een Azure-regio (Zie hoofdstuk [Azure-gebieden] [ planning-guide-3.1]).
* Zone redundante opslag, waarmee de drie afbeeldingen verspreid over verschillende data centers binnen dezelfde Azure-regio.
* Redundantie standaardniveau is geografische redundantie, wat de inhoud asynchroon gerepliceerd in een andere drie afbeeldingen van de gegevens in een andere Azure-regio, die wordt gehost in dezelfde geopolitieke regio.

Zie ook de tabel boven in dit artikel met betrekking tot de van de verschillende redundantieopties op: <https://azure.microsoft.com/pricing/details/storage/>

Meer informatie over Azure Storage vindt u hier:

* <https://Azure.Microsoft.com/Documentation/Services/Storage/>
* <https://Azure.Microsoft.com/Services/site-Recovery>
* <https://docs.Microsoft.com/rest/API/storageservices/Understanding-Block-blobs--Append-blobs--and-Page-blobs>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-Disk-Encryption-for-Linux-and-Windows-Virtual-machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Azure Standard-opslag
Azure Standard-opslag is het type opslag beschikbaar bij Azure IaaS werd uitgebracht. Er zijn IOPS quota per één schijf worden afgedwongen. Latentie heeft ondergaan is niet in dezelfde klasse als SAN/NAS-apparaten die doorgaans geïmplementeerd voor geavanceerde SAP-systemen lokale gehoste. Evenwel de Standard-Azure-opslag voldoende voor veel honderden bewezen SAP-systemen ondertussen geïmplementeerd in Azure.

Schijven die zijn opgeslagen op Azure Storage-Accounts worden in rekening gebracht op basis van de werkelijke hoeveelheid gegevens die zijn opgeslagen, de hoeveelheid opslagtransacties en uitgaande gegevensoverdracht redundantie-optie gekozen. Veel schijven kunnen worden gemaakt op de maximaal 1TB groot, maar als deze leeg blijft er zijn geen kosten. Als u een VHD met 100GB vervolgens vult, u in rekening worden gebracht voor het opslaan van 100GB en niet voor de grootte van de nominaal met de VHD gemaakt.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium-opslag
In April 2015 geïntroduceerd Microsoft Azure Premium-opslag. Premium-opslag is geïntroduceerd met het doel om te bieden:

* Betere i/o-latentie.
* Betere doorvoer.
* Minder variabiliteit in i/o-latentie.

Daartoe zijn veel wijzigingen van de twee belangrijkste zijn geïntroduceerd:

* Informatie over het gebruik van SSD-schijven in de Azure Storage-knooppunten
* Een nieuwe lezen cache dat wordt ondersteund door de lokale SSD van een Azure rekenknooppunt

In in de Standard-opslag waar mogelijkheden is niet veranderd afhankelijk van de grootte van de schijf (of VHD) Premium-opslag heeft momenteel drie andere schijf categorieën, die worden weergegeven in dit artikel: <https://azure.microsoft.com/pricing/ Details / / zonder begeleiding-opslagschijven />

U ziet dat IOPS/en de schijf doorvoer/schijf zijn afhankelijk van de categorie van de grootte van de schijven

Kosten basis in het geval van Premium-opslag is niet de werkelijke gegevensvolume dat is opgeslagen in een dergelijke disks, maar de categorie van de grootte van dergelijke een schijf, onafhankelijk van de hoeveelheid gegevens die zijn opgeslagen in de schijf.

U kunt schijven maken op Premium-opslag die niet rechtstreeks in de weergegeven grootte-categorieën wilt toewijzen. Kan dit het geval is, vooral wanneer u schijven uit de Standard-opslag kopiëren naar de Premium-opslag. In dergelijke gevallen wordt een toewijzing naar de volgende grootste Premium-opslag-optie voor schijf uitgevoerd.

Let erop dat alleen bepaalde VM-reeks van de Azure Premium-opslag profiteren kunt. Dit zijn de DS - en GS-serie vanaf december 2015. De DS-serie is in wezen hetzelfde als de D-reeks met de uitzondering dat DS-serie de mogelijkheid om te koppelen Premium-opslag heeft op basis van virtuele machines ook op schijven die worden gehost op Azure Standard-opslag. Hetzelfde geldt voor G-serie vergeleken met GS-serie.

Als u het deel van de DS-serie VM's in zijn uitgecheckt [in dit artikel (Linux)] [ virtual-machines-sizes-linux] en [in dit artikel (Windows)][virtual-machines-sizes-windows], zult u ontdekken dat Er zijn gegevens volume beperkingen voor Premium-opslag-schijven op de granulatie van de VM-niveau. Andere DS-serie- of GS-serie virtuele machines hebt ook andere beperkingen met betrekking tot het aantal gegevensschijven dat kan worden gekoppeld. Deze limieten worden beschreven in het artikel ook hierboven vermeld. Maar in wezen betekent dit dat u, bijvoorbeeld 32 x P30 schijven aan een enkele DS14 VM koppelen kunt u 32 x de maximale doorvoer van een schijf P30 niet ophalen. De maximale doorvoer op VM-niveau, zoals beschreven in het artikel beperkt in plaats daarvan de doorvoer van gegevens.

Meer informatie over het Premium-opslag vindt u hier: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Beheerde schijven
Beheerde schijven zijn een nieuwe brontype in Azure Resource Manager die kunnen worden gebruikt in plaats van VHD's die zijn opgeslagen in Azure Storage-Accounts. Beheerde schijven automatisch zijn afgestemd op de Beschikbaarheidsset van deze zijn gekoppeld aan virtuele machine en daarom verhoogt de beschikbaarheid van uw virtuele machine en de services die worden uitgevoerd op de virtuele machine. Lees voor meer informatie de [overzichtsartikel](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

We raden je aan u beheerde schijf gebruiken, omdat ze de implementatie en beheer van uw virtuele machines vereenvoudigt.
SAP ondersteunt momenteel alleen Premium-schijven worden beheerd. Lees voor meer informatie, SAP-notitie [1928533].

#### <a name="azure-storage-accounts"></a>Azure Storage Accounts
Bij het implementeren van services of virtuele machines in Azure, kan de implementatie van virtuele harde schijven en VM-installatiekopieën worden ingedeeld in eenheden Azure Storage-Accounts die worden genoemd. Bij het plannen van een Azure-implementatie, moet u zorgvuldig overwegen de beperkingen van Azure. De een-zijde is er een beperkt aantal Storage-Accounts per Azure-abonnement. Hoewel elke Azure Storage-Account een groot aantal VHD-bestanden bevatten kan, moet u er een vaste limiet is op de totale IOP's per Storage-Account. Bij het implementeren van honderden SAP virtuele machines met DBMS-systemen voor het maken van belangrijke i/o-aanroepen, wordt het aanbevolen voor het distribueren van hoge IOPS DBMS VMs tussen meerdere Azure Storage-Accounts. Wees voorzichtig niet te overschrijden de huidige limiet van Azure Storage-Accounts per abonnement. Omdat opslag een essentieel onderdeel van de implementatie van de database voor een SAP-systeem, dit concept wordt besproken in al waarnaar wordt verwezen nader [DBMS Deployment Guide][dbms-guide].

Meer informatie over Azure Storage-Accounts kunt u vinden [in dit artikel][storage-scalability-targets]. Dit artikel leest, zult u ontdekken dat er verschillen in de beperkingen tussen Azure Storage-Accounts en Premium Storage-Accounts zijn. Belangrijke verschillen zijn de hoeveelheid gegevens die kunnen worden opgeslagen in een Opslagaccount. Het volume is in de Standard-opslag een grootte die groter is dan met Premium-opslag. Aan de andere kant het standaard Opslagaccount ernstig beperkt is in IOPS (Zie kolom **snelheid van totaal aantal aanvragen voor**), terwijl het Azure Premium Storage-Account beperking bestaat niet is. Behandeld details en de resultaten van deze verschillen bij het bespreken van de implementaties van SAP-systemen, met name de DBMS-servers.

U hebt de mogelijkheid om verschillende containers voor het ordenen en categoriseren van andere virtuele harde schijven maken binnen een Opslagaccount. Deze containers worden meestal gebruikt om, bijvoorbeeld afzonderlijke VHD's van andere virtuele machines. Er zijn geen gevolgen voor prestaties bij het gebruik van een container of meerdere containers onder één Azure Storage-Account.

In Azure volgt de naam van een VHD in de volgende naamgevingsconventie verbinding die u moet een unieke naam opgeven voor de VHD in Azure:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Zoals vermeld dat de bovenstaande tekenreeks moet unieke identificatie van de VHD die is opgeslagen op Azure Storage.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure-netwerken
Microsoft Azure biedt een infrastructuur van het netwerk, waardoor de toewijzing van alle scenario's die we willen realiseren met SAP-software. De mogelijkheden zijn:

* Toegang van buiten en rechtstreeks naar de virtuele machines via Windows Terminal Services of ssh/VNC
* Toegang tot services en bepaalde poorten gebruikt door toepassingen vanuit de virtuele machines
* Interne communicatie en naamomzetting tussen een groep van virtuele machines die worden geïmplementeerd als Azure virtuele machines
* Cross-premises-connectiviteit tussen een klant on-premises netwerk en het Azure-netwerk
* Over Azure-regio of data center-verbindingen tussen Azure websites

Meer informatie vindt u hier: <https://azure.microsoft.com/documentation/services/virtual-network/>

Er zijn veel verschillende mogelijkheden voor het configureren van naam en IP-oplossing in Azure. In dit document is alleen-scenario's zijn afhankelijk van de standaardwaarde van het gebruik van Azure DNS (in tegenstelling tot het definiëren van een eigen DNS-service). Er is ook een nieuwe Azure DNS-service, die kan worden gebruikt in plaats van het instellen van uw eigen DNS-server. Meer informatie vindt u in [in dit artikel] [ virtual-networks-manage-dns-in-vnet] en klik op [deze pagina](https://azure.microsoft.com/services/dns/).

Voor cross-premises scenario's, zijn er vertrouwen op het feit dat de on-premises naar Azure AD/OpenLDAP/DNS-via VPN of een particuliere verbinding is uitgebreid. Voor bepaalde scenario's zoals hier wordt beschreven, kan het nodig zijn om een geïnstalleerd in Azure AD/OpenLDAP-replica.

Omdat de netwerken en naamomzetting is een essentieel onderdeel van de implementatie van de database voor een SAP-systeem, dit concept wordt besproken in meer detail in de [DBMS Deployment Guide][dbms-guide].

##### <a name="azure-virtual-networks"></a>Virtuele netwerken van Azure
Opbouw van een Azure-netwerk, kunt u het adresbereik van de privé IP-adressen toegewezen door Azure DHCP-functionaliteit. In scenario's voor cross-premises, het IP-adresbereik gedefinieerd nog is toegewezen via DHCP door Azure. Echter domein naamomzetting lokale (ervan uitgaande dat de virtuele machines deel uitmaken van een lokaal domein) wordt uitgevoerd en daarom adressen afgezien van andere Azure-Cloud-Services kunt oplossen.

Elke virtuele Machine in Azure moet worden verbonden met een virtueel netwerk.

Meer informatie vindt u in [in dit artikel] [ resource-groups-networking] en klik op [deze pagina](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (Een artikel waaronder de OpenLDAP onderwerp + ARM; vinden MShermannd TODO niet)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Zodra een virtuele machine is geïmplementeerd wijzigen niet u standaard de configuratie van het virtueel netwerk. De TCP/IP-instellingen moeten worden links naar de Azure-DHCP-server. Standaard wordt de dynamische IP-toewijzing.
>
>

Het MAC-adres van de kaart van het virtuele netwerk kan worden gewijzigd, bijvoorbeeld na het aanpassen en de Windows- of Linux Gast OS neemt over de nieuwe netwerkkaart en automatisch de IP-adres en DNS-adressen in dit geval toewijzen via DHCP.

##### <a name="static-ip-assignment"></a>Toewijzing van vaste IP-adres
Het is mogelijk vaste of gereserveerde IP-adressen toewijzen aan virtuele machines binnen een Azure-netwerk. De VM's worden uitgevoerd in een Azure-netwerk wordt geopend kans om deze functionaliteit gebruik als nodig is of voor sommige scenario's vereist te groot. De toewijzing van IP-adres blijft in de gehele het bestaan van de VM, ongeacht of de virtuele machine actief of afgesloten geldig. Als gevolg hiervan moet u het totale aantal virtuele machines (actieve en gestopte VM's) in aanmerking nemen bij het definiëren van het bereik van IP-adressen voor het virtuele netwerk. Het IP-adres blijft toegewezen tot de virtuele machine en de netwerkinterface is verwijderd of totdat het IP-adres opgehaald ongedaan opnieuw toegewezen. Lees voor meer informatie [in dit artikel][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Meerdere NIC's per VM
U kunt meerdere virtuele netwerkinterfacekaarten (vNIC) definiëren voor een virtuele Machine van Azure. Scheiding waar, bijvoorbeeld clientverkeer wordt doorgestuurd via één vNIC- en back-end-verkeer wordt met de mogelijkheid om meerdere vNICs die u beginnen kunt met het instellen van netwerkverkeer via een tweede vNIC doorgestuurd. Afhankelijk van het type van de virtuele machine er zijn verschillende beperkingen met betrekking tot het aantal vNICs. Exacte details, functionaliteit en beperkingen vindt in deze artikelen:

* [Maak een Windows-VM met meerdere NIC 's][virtual-networks-multiple-nics-windows]
* [Maak een Linux-VM met meerdere NIC 's][virtual-networks-multiple-nics-linux]
* [Meerdere NIC VMs met een sjabloon implementeren][virtual-network-deploy-multinic-arm-template]
* [Implementeren van meerdere NIC virtuele machines met behulp van PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Implementeren van meerdere NIC VM's met de Azure CLI][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Site-naar-Site-connectiviteit
Cross-premises is Azure VM's en On-Premises gekoppeld met een transparante en permanente VPN-verbinding. Naar verwachting worden de meest voorkomende SAP-implementatie patroon in Azure. De veronderstelling is operationele procedures en processen met SAP-exemplaren in Azure transparant moeten werken. Dit betekent dat die u kunt afdrukken buiten deze systemen moet evenals gebruik de SAP Transport Management System (TMS) aan transport wordt gewijzigd van een ontwikkelsysteem in Azure met een testsysteem is geïmplementeerd op de lokale. Meer documentatie om site-naar-site vindt u in [in dit artikel][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-Tunnel-apparaat
Om te kunnen maken van een site-naar-site-verbinding (Datacenter lokale naar Azure-Datacenter), moet u verkrijgen en configureren van een VPN-apparaat of Routing and Remote Access Service (RRAS) dat werd geïntroduceerd als softwareonderdeel met Windows Server 2012 gebruiken.

* [Een virtueel netwerk maken met een site-naar-site VPN-verbinding met behulp van PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Informatie over VPN-apparaten voor Site-naar-Site-VPN-gatewayverbindingen][vpn-gateway-about-vpn-devices]
* [VPN-Gateway Veelgestelde vragen][vpn-gateway-vpn-faq]

![Site-naar-site-verbinding tussen on-premises en Azure][planning-guide-figure-600]

De bovenstaande afbeelding ziet twee Azure-abonnementen zijn gereserveerd voor gebruik van IP-adres subbereiken in virtuele netwerken in Azure. De verbinding tussen de on-premises netwerk en Azure tot stand is gebracht via VPN.

#### <a name="point-to-site-vpn"></a>Punt-naar-Site VPN
Punt-naar-site VPN moet elke client-computer verbinding maken met een eigen VPN in Azure. Punt-naar-site-connectiviteit is voor de SAP scenario's die we kijken, niet praktisch. Daarom geen verdere verwijzingen krijgen tot het punt-naar-site VPN-verbinding.

Meer informatie vindt u hier
* [Een punt-naar-site-verbinding met een VNet configureren met Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Een punt-naar-site-verbinding met een VNet configureren met behulp van PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Multi-site-VPN
Azure biedt ook tegenwoordig de mogelijkheid voor het maken van meerdere Site-VPN-verbinding voor een Azure-abonnement. Een abonnement van één was eerder beperkt tot één site-naar-site VPN-verbinding. Deze beperking verdwenen met multi-site-VPN-verbindingen voor één abonnement. Hierdoor kunnen gebruikmaken van meer dan één Azure-regio voor een specifiek abonnement via cross-premises configuraties.

Zie voor meer documentatie [in dit artikel][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO geen ARM docu koppeling gevonden)

#### <a name="vnet-to-vnet-connection"></a>VNet-naar-VNet-verbinding
Met multi-site-VPN, moet u een afzonderlijke Azure Virtual Network configureren in elke regio. U hebt echter vaak de vereiste dat de softwareonderdelen in de verschillende regio's met elkaar moeten communiceren. Deze communicatie moet in het ideale geval niet uit een Azure-gebied met on-premises en van daaruit naar de andere Azure-regio worden gerouteerd. Op de snelkoppeling biedt Azure de mogelijkheid om een verbinding van een Azure Virtual Network in een regio om een ander virtueel netwerk van Azure te configureren die worden gehost in een andere regio. Deze functionaliteit wordt VNet-naar-VNet-verbinding genoemd. Meer informatie over deze functie vindt u hier: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure-expressroute"></a>Persoonlijke verbinding met Azure ExpressRoute
Microsoft Azure ExpressRoute kunt het maken van een particuliere verbindingen tussen Azure-datacenters en van de klant on-premises infrastructuur of in een omgeving met CO-locatie. ExpressRoute wordt aangeboden door verschillende MPLS (geschakeld pakket) VPN-providers of andere serviceproviders netwerk. ExpressRoute-verbindingen gaan niet via het openbare internet. ExpressRoute-verbindingen bieden betere beveiliging, meer betrouwbaarheid via meerdere parallelle circuits, hogere snelheden en lagere latenties dan gewone verbindingen via Internet.

Meer informatie over Azure ExpressRoute en de offerings hier vinden:

* <https://Azure.Microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.Microsoft.com/Pricing/details/expressroute/>
* <https://Azure.Microsoft.com/Documentation/articles/expressroute-faqs/>

Express Route kan meerdere Azure-abonnementen via één ExpressRoute-circuit, zoals hier wordt beschreven

* <https://Azure.Microsoft.com/Documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://Azure.Microsoft.com/Documentation/articles/expressroute-howto-circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Geforceerde tunneling in geval van een cross-premises
Voor virtuele machines het lidmaatschap van lokale domeinen via site-naar-site, punt-naar-site of ExpressRoute, moet u ervoor zorgen dat de proxy-instellingen van Internet zijn ophalen geïmplementeerd voor alle gebruikers in deze VMs ook. Standaard-software in deze virtuele machines of gebruikers via een browser toegang tot het internet met zou niet Ga via de proxy-bedrijf, maar meteen via Azure met het internet verbinding te maken. Maar zelfs de proxy-instelling is niet een oplossing 100% om het verkeer via de proxy bedrijf leiden omdat het is de verantwoordelijkheid van de software en services om te controleren voor de proxy. Als software die wordt uitgevoerd in de virtuele machine die niet actief of een beheerder de instellingen bewerkt, verkeer naar Internet opnieuw kunt detoured rechtstreeks via Azure met Internet.

Om te voorkomen dat dit, kunt u geforceerde Tunneling configureren met site-naar-site-connectiviteit tussen on-premises en Azure. De gedetailleerde beschrijving van de functie geforceerde Tunneling is hier gepubliceerd <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Geforceerde Tunneling met ExpressRoute wordt ingeschakeld door klanten kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies.

#### <a name="summary-of-azure-networking"></a>Overzicht van Azure-netwerken
In dit hoofdstuk bevat veel belangrijke punten over Azure-netwerken. Hier volgt een samenvatting van de belangrijkste punten:

* Virtuele netwerken van Azure kunt voor het instellen van het netwerk aan uw eigen wensen
* Virtuele netwerken van Azure kan worden gebruikt voor het toewijzen van IP-adresbereiken aan virtuele machines of vaste IP-adressen toewijzen aan virtuele machines
* Als u een Site-naar-Site of een punt-naar-Site-verbinding wilt instellen moet u eerst een Azure-netwerk maken
* Wanneer een virtuele machine is geïmplementeerd, is het niet meer mogelijk om te wijzigen van het virtuele netwerk toegewezen aan de virtuele machine

### <a name="quotas-in-azure-virtual-machine-services"></a>Quota's in de virtuele Machine van Azure Services
We moeten wissen over het feit dat de opslag en netwerk-infrastructuur wordt gedeeld tussen VM's met verschillende services in de Azure-infrastructuur. En net zoals in de datacenters van klant overprovisioning van sommige van de infrastructuur-bronnen vindt plaats mate. Het Microsoft Azure-Platform gebruikt, CPU, netwerk, en andere quota wilt beperken, het brongebruik en behouden van consistente en deterministisch prestaties.  De verschillende typen VM (A5, A6, enz.) hebben verschillende quota's voor het aantal schijven, CPU, RAM, en het netwerk.

> [!NOTE]
> CPU en geheugen van de resources van de VM-typen ondersteund door SAP zijn vooraf toegewezen op de hostknooppunten. Dit betekent dat zodra de virtuele machine is geïmplementeerd, de bronnen op de host beschikbaar, zoals gedefinieerd door het VM-type zijn.
>
>

Bij het plannen en SAP formaat op Azure-oplossingen kunnen de quota voor de grootte van elke virtuele machine moeten worden overwogen. De VM-quota worden beschreven [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

De quota die worden beschreven, vertegenwoordigen de theoretische maximale waarden.  De limiet van IOP's per schijf kan worden bereikt met kleine IOs (8kb), maar mogelijk niet kan worden bereikt met grote IOs (1Mb).  De maximale IOPS wordt afgedwongen voor de granulatie van één schijf.

Als een ruwe beslissingsstructuur om te bepalen of een SAP-systeem in Azure Services op virtuele Machine en de mogelijkheden of of een bestaand systeem moet anders worden geconfigureerd past voor het implementeren van het systeem op Azure, kan de onderstaande beslissingsstructuur worden gebruikt:

![Beslissingsstructuur om te bepalen van de mogelijkheid tot het implementeren van SAP op Azure][planning-guide-figure-700]

**Stap 1**: de belangrijkste informatie is beginnen met de vereiste SAP's voor een bepaald SAP-systeem. De vereisten voor SAP's moeten worden gescheiden in het gedeelte DBMS en het deel van de toepassing SAP zelfs als het SAP-systeem al geïmplementeerde lokale in een configuratie met 2-laag. Voor bestaande systemen, kunnen de SAP's vaak betrekking hebben op de hardware gebruikt worden bepaald of op basis van bestaande SAP benchmarks geschatte. De resultaten vindt u hier: <http://global.sap.com/campaigns/benchmark/index.epx>.
Voor geïmplementeerde SAP-systemen, moet u hebben doorlopen een oefening sizing, die de vereisten SAP's van het systeem moet bepalen.
Zie ook deze blog en bijgevoegde document voor SAP sizing op Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Stap 2**: voor bestaande systemen, het i/o-volume en de i/o-bewerkingen per seconde op de DBMS-server moeten worden gemeten. Voor nieuwe geplande systemen geeft de sizing oefening voor het nieuwe systeem ook ruwe ideeën van de i/o-vereisten de DBMS-zijde. Als u niet zeker, moet u uiteindelijk een bewijs van Concept uitvoeren.

**Stap 3**: vergelijken van de vereiste SAP's voor de DBMS-server met de SAP's u de verschillende typen van de virtuele machine van Azure krijgt. De informatie van SAP's van de verschillende typen van de virtuele machine in Azure wordt beschreven in SAP-notitie [1928533]. De focus moet eerst op de DBMS-VM niet omdat de databaselaag is de laag in een SAP NetWeaver-systeem komt niet in de meeste implementaties worden uitgebreid. Daarentegen kan aan de toepassingslaag SAP worden uitgebreid. Als geen van de SAP ondersteund typen van de virtuele machine van Azure kunnen de vereiste SAP's bieden, de werklast van het geplande SAP-systeem kan niet worden uitgevoerd op Azure. U moet ofwel de implementatie van het systeem on-premises of moet u het volume van de werkbelasting voor het systeem te wijzigen.

**Stap 4**: zoals beschreven [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows], Azure dwingt een quotum IOP's per schijf onafhankelijke of u Standard-opslag- of Premium-opslag gebruiken. Afhankelijk van het type van de virtuele machine, verschilt van het aantal gegevensschijven dat kan worden gekoppeld. Als gevolg hiervan kunt u het maximum aantal IOPS dat kan worden bereikt met elk van de verschillende typen van de VM berekenen. Afhankelijk van de indeling van de database-bestand, u kunt stripe schijven om te worden van een volume in het gastbesturingssysteem. Echter, als het huidige volume van de IOPS van een geïmplementeerde SAP-systeem de berekende grenzen van het grootste VM-type van Azure en overschrijdt als er is geen kans om te compenseren met meer geheugen, de werklast van het SAP-systeem kan worden beïnvloed ernstige problemen. In dergelijke gevallen kunt u een punt waar u moet het systeem op Azure niet implementeren bereikt.

**Stap 5**: in SAP-systemen die geïmplementeerd op lokale in laag 2-configuraties zijn, met name de kans is dat het systeem moet mogelijk worden geconfigureerd in Azure in een configuratie met 3-Laagse. In deze stap moet u controleren of er een onderdeel van de toepassingslaag SAP is, die niet worden uitgebreid en die niet in de CPU en geheugen resources die de verschillende typen van de virtuele machine van Azure bieden zou passen. Als er inderdaad een dergelijke component, kunnen niet het SAP-systeem en de werkbelasting worden geïmplementeerd in Azure. Maar als u de SAP-toepassingsonderdelen in meerdere virtuele machines van Azure uitbreiden kunt, het systeem kan worden geïmplementeerd in Azure.

**Stap 6**: als de DBMS en SAP laag toepassingsonderdelen kunnen worden uitgevoerd in Azure VM's, de configuratie moet worden gedefinieerd met betrekking tot:

* Aantal virtuele machines in Azure
* VM-typen voor de afzonderlijke onderdelen
* Aantal VHD's in VM DBMS om voldoende IOP 's

## <a name="managing-azure-assets"></a>Azure activa te beheren
### <a name="azure-portal"></a>Azure-portal
De Azure portal is een van drie interfaces voor het beheren van Azure VM-implementaties. De algemene beheertaken, zoals het implementeren van virtuele machines van installatiekopieën, kunnen worden gedaan via de Azure portal. Het maken van Opslagaccounts, virtuele netwerken en andere Azure-onderdelen zijn bovendien ook taken die de Azure-portal uitstekend kan verwerken. Functionaliteit zoals VHD's van on-premises uploaden naar Azure of kopiëren van een VHD in Azure zijn echter taken, die de hulpprogramma's van derden of via PowerShell of CLI beheer vereist.

![Microsoft Azure portal - overzicht van virtuele machines][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Beheer- en configuratietaken voor het exemplaar van de virtuele Machine zijn mogelijke uit binnen de Azure-portal.

Naast het opnieuw opstarten of afsluiten van een virtuele Machine kunt u ook koppelen, loskoppelen en gegevensschijven voor het exemplaar van de virtuele Machine, het exemplaar voor de voorbereiding van de installatiekopie vastleggen en configureer de grootte van het exemplaar van de virtuele Machine maken.

De Azure portal biedt basisfunctionaliteit te implementeren en configureren van virtuele machines en vele andere Azure-services. Maar niet alle beschikbare functionaliteit wordt gedekt door de Azure-portal. In de Azure portal is het niet mogelijk om uit te voeren taken, zoals:

* VHD's uploaden naar Azure
* Kopiëren van virtuele machines

[comment]: <> (MShermannd TODO wat over automatisering in service voor SAP VM's?)
[comment]: <> (Implementatie van meerdere virtuele machines os ondertussen mogelijk MSSedusch)
[comment]: <> (Elk type automatisering met betrekking tot implementatie is MSSedusch ook niet mogelijk met de Azure-portal. Taken, zoals implementatie van meerdere virtuele machines met scripts is niet mogelijk via de Azure-portal.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Beheer via Microsoft Azure PowerShell-cmdlets
Windows PowerShell is een krachtig en uitbreidbaar framework dat veel is vastgesteld door klanten groot aantal systemen in Azure implementeren. Na de installatie van PowerShell-cmdlets op een desktop-, laptop- of toegewezen management station, worden de PowerShell-cmdlets op afstand uitgevoerd.

Het proces voor het inschakelen van een lokale bureaublad/laptop voor het gebruik van Azure PowerShell-cmdlets en het configureren van die voor het gebruik met de Azure-abonnementen wordt beschreven in [in dit artikel][powershell-install-configure].

Meer gedetailleerde stapsgewijze instructies voor het installeren, bijwerken en configureren van Azure PowerShell cmdlets kunnen worden gevonden in [in dit hoofdstuk van de Implementatiehandleiding][deployment-guide-4.1].

Ervaring van de klant is tot nu toe PowerShell (PS) is gewoon de krachtige hulpprogramma voor het implementeren van virtuele machines en voor het maken van aangepaste stappen in de implementatie van virtuele machines. Alle klanten SAP-exemplaren worden uitgevoerd in Azure PS-cmdlets gebruiken om te voorzien in beheertaken zij in de Azure portal of zelfs PS-cmdlets gebruikt uitsluitend voor het beheren van implementaties in Azure. Omdat de Azure-specifieke cmdlets de dezelfde naamgevingsregel als het meer dan 2000 Windows-gerelateerde cmdlets delen, is een eenvoudige taak voor Windows-beheerders gebruikmaken van deze cmdlets.

Zie het voorbeeld hier: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd TODO beschrijven nieuwe CLI-opdracht wanneer getest)
Implementatie van de extensie Azure Monitoring voor SAP (Zie hoofdstuk [Azure Monitoring Solution voor SAP] [ planning-guide-9.1] in dit document) is alleen mogelijk via PowerShell of CLI. Daarom is het verplichte instellen en configureren van PowerShell of CLI bij het implementeren of beheren van een systeem SAP NetWeaver in Azure.  

Als Azure meer functionaliteit biedt, gaan nieuwe PS-cmdlets die moet worden bijgewerkt met de cmdlets die worden toegevoegd. Daarom is het verstandig om te controleren van de site Azure downloaden ten minste eenmaal in de maand <https://azure.microsoft.com/downloads/> voor een nieuwe versie van de cmdlets. De nieuwe versie is geïnstalleerd op de oudere versie.

Voor een algemeen overzicht van Azure-gerelateerde PowerShell opdrachten hier controleren: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Beheer via Microsoft Azure CLI-opdrachten
Powershell resources mogelijk voor klanten die Linux en wilt u Azure beheert niet een optie. Als alternatief biedt Microsoft Azure CLI.
De Azure CLI biedt een set van open-source platformoverschrijdende opdrachten voor het werken met de Azure-Platform. De Azure CLI biedt vrijwel dezelfde functionaliteit gevonden in de Azure-portal.

Zie voor informatie over de installatie, configuratie en het gebruik van de CLI opdrachten Azure taken uitvoeren

* [De Azure CLI installeren][xplat-cli]
* [Implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en de Azure CLI] [../../linux/create-ssh-secured-vm-from-template.md]
* [De Azure CLI voor Mac, Linux en Windows gebruiken met Azure Resource Manager gebruiken][xplat-cli-azure-resource-manager]

Lees ook hoofdstuk [Azure CLI voor virtuele Linux-machines] [ deployment-guide-4.5.2] in de [Deployment Guide] [ planning-guide] over het gebruik van Azure CLI voor het implementeren van de Azure-bewaking De extensie voor SAP.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Verschillende manieren voor het implementeren van virtuele machines voor SAP in Azure
In dit hoofdstuk leert u de verschillende manieren voor het implementeren van een virtuele machine in Azure. Extra voorbereiding procedures, evenals de verwerking van VHD's en virtuele machines in Azure worden besproken in dit hoofdstuk.

### <a name="deployment-of-vms-for-sap"></a>Implementatie van virtuele machines voor SAP
Microsoft Azure biedt verschillende manieren om virtuele machines en gekoppelde schijven te implementeren. Het is dus zeer belangrijk om te begrijpen van de verschillen omdat voorbereidingen van de virtuele machines afhankelijk van de methode van implementatie verschillen. In het algemeen Kijk we eens naar de volgende scenario's:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Een virtuele machine verplaatsen van on-premises naar Azure met een niet-gegeneraliseerd-schijf
U van plan bent te verplaatsen van een specifiek SAP-systeem van on-premises naar Azure. Dit kan worden gedaan door het uploaden van de VHD, waarin het besturingssysteem, de SAP-binaire bestanden en binaire bestanden DBMS plus de VHD's met de bestanden voor gegevens en logboekbestanden van het DBMS naar Azure. In tegenstelling tot [scenario #2 hieronder][planning-guide-5.1.2], houdt u de hostnaam, SAP-SID en SAP gebruikersaccounts in de Azure VM zoals ze zijn geconfigureerd in de on-premises omgeving. Het generaliseren van de afbeelding is daarom niet nodig. Zie de hoofdstukken [voorbereiding voor het verplaatsen van een virtuele machine van on-premises naar Azure met een schijf niet gegeneraliseerd] [ planning-guide-5.2.1] van dit document voor de voorbereidende stappen op lokale en uploaden van niet-gegeneraliseerde virtuele machines of VHD's in Azure. Lees hoofdstuk [Scenario 3: een virtuele machine verplaatsen van on-premises met een VHD Azure niet gegeneraliseerd met SAP] [ deployment-guide-3.4] in de [Deployment Guide] [ deployment-guide] voor gedetailleerde stappen voor het implementeren van dergelijke een installatiekopie in Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Een virtuele machine met een installatiekopie klantspecifieke implementeren
Als gevolg van het patchbestand vereisten van uw besturingssysteem of DBMS-versie, kunnen de opgegeven afbeeldingen in Azure Marketplace niet aansluiten bij uw behoeften. Daarom moet u mogelijk een virtuele machine maken met uw eigen persoonlijke OS/DBMS VM-installatiekopie, die verschillende keren daarna kan worden geïmplementeerd. Als u een persoonlijke installatiekopie voorbereiden voor het kopiëren, moeten de volgende items worden beschouwd:

- - -
> ![Windows][Logo_Windows] Windows
>
> Hier voor meer informatie: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> de Windows-instellingen (zoals Windows SID en de hostnaam) moet op de lokale virtuele machine via gescheiden/gegeneraliseerd de opdracht sysprep.
>
>
> ![Linux][Logo_Linux] Linux
>
> Volg de stappen in deze artikelen voor [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], of [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], voor het voorbereiden van een VHD te uploaden naar Azure.
>
>

- - -
Als u al SAP-inhoud in uw lokale virtuele machine (vooral voor laag 2-systemen) hebt geïnstalleerd, kunt u de instellingen van het SAP aanpassen nadat de implementatie van de virtuele Azure-machine via het exemplaar wordt ondersteund door de SAP Software inrichting Manager procedure wijzigen (SAP-notitie [1619720]). Zie de hoofdstukken [voorbereiding voor het implementeren van een virtuele machine met een installatiekopie klantspecifieke voor SAP] [ planning-guide-5.2.2] en [uploaden van een VHD van on-premises naar Azure] [ planning-guide-5.3.2]van dit document voor de voorbereidende stappen op lokale en uploaden van een gegeneraliseerde virtuele machine in Azure. Lees hoofdstuk [Scenario 2: een virtuele machine met een aangepaste installatiekopie implementeren voor SAP] [ deployment-guide-3.3] in de [Deployment Guide] [ deployment-guide] voor gedetailleerde stappen voor het implementeren van dergelijke een installatiekopie in Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Implementatie van een virtuele machine uit Azure Marketplace
U wilt gebruiken een door Microsoft of derden geleverd VM-installatiekopie uit Azure Marketplace voor het implementeren van uw virtuele machine. Wanneer u uw virtuele machine in Azure hebt geïmplementeerd, volgt u dezelfde richtlijnen en hulpprogramma's voor het installeren van de SAP-software en/of DBMS in uw virtuele machine, zoals u in een on-premises omgeving doen zou. Voor een beschrijving van de implementatie gedetailleerde, Zie hoofdstuk [Scenario 1: implementatie van een virtuele machine uit Azure Marketplace voor SAP] [ deployment-guide-3.2] in de [Deployment Guide][deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Virtuele machines met SAP voorbereiden voor Azure
Voordat u virtuele machines uploadt naar Azure moet u ervoor zorgen dat de virtuele machines en VHD's te voldoen aan bepaalde vereisten. Er zijn kleine verschillen afhankelijk van de implementatiemethode die wordt gebruikt.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Voorbereiding voor het verplaatsen van een virtuele machine van on-premises naar Azure met een niet-gegeneraliseerd-schijf
Er is een algemene implementatiemethode te verplaatsen van een bestaande virtuele machine die wordt uitgevoerd van een SAP-systeem van on-premises naar Azure. Dat virtuele machine en de SAP-systeem in de virtuele machine alleen wordt uitgevoerd in Azure met behulp van de dezelfde hostnaam en zeer waarschijnlijk dezelfde SAP-SID. In dit geval de Gast OS van virtuele machine niet worden gegeneraliseerd voor meerdere implementaties. Als de on-premises netwerk in Azure hebt uitgebreid (Zie hoofdstuk [cross-premises - implementatie van één of meerdere SAP-virtuele machines in Azure met de vereiste van volledig geïntegreerd in de on-premises netwerk] [ planning-guide-2.2] in dit document), en zelfs de dezelfde domeinaccounts kunnen worden gebruikt binnen de virtuele machine, zoals die zijn gebruikt voor lokale.

Vereisten voor het voorbereiden van uw eigen Azure VM-schijf zijn:

* De VHD met het besturingssysteem kan oorspronkelijk alleen een maximale grootte van 127GB hebben. Deze beperking kreeg aan het einde van maart 2015 geëlimineerd. Nu de VHD met het besturingssysteem maximaal 1TB groot zijn kan als andere Azure-Storage VHD ook gehoste.
* Het moet in de vaste VHD-indeling. Dynamische virtuele harde schijven of VHD's in de VHDx-indeling zijn nog niet ondersteund in Azure. Dynamische virtuele harde schijven worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met commandlets PowerShell of CLI
* VHD's die zijn gekoppeld aan de virtuele machine en moeten worden gekoppeld, opnieuw in Azure aan de virtuele machine zich in een vaste VHD-indeling ook. Lees [in dit artikel (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) en [in dit artikel (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) voor de maximale grootte van gegevensschijven. Dynamische virtuele harde schijven worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met commandlets PowerShell of CLI
* Voeg een andere lokale account met administrator-bevoegdheden die kunnen worden gebruikt door de Microsoft-ondersteuning of die kunnen worden toegewezen als context voor services en toepassingen worden uitgevoerd in totdat de virtuele machine is geïmplementeerd en geschiktere gebruikers kan worden gebruikt.
* Voor het geval van het gebruik van een scenario alleen in de Cloud-implementaties (Zie hoofdstuk [alleen in de Cloud - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk] [ planning-guide-2.1] van dit document) in combinatie met deze implementatiemethode domeinaccounts werkt mogelijk niet zodra de Azure-schijf is geïmplementeerd in Azure. Dit geldt met name voor accounts die worden gebruikt voor het uitvoeren van services, zoals de DBMS of SAP-toepassingen. Daarom moet u dergelijke domeinaccounts vervangen door de lokale accounts van virtuele machine en het lokale domeinaccounts in de virtuele machine te verwijderen. On-premises domeingebruikers behouden in de VM-installatiekopie is niet een probleem bij de virtuele machine wordt geïmplementeerd in het scenario voor cross-premises, zoals beschreven in hoofdstuk [Cross-Premises - implementatie van één of meerdere SAP-virtuele machines in Azure met de vereiste volledig is geïntegreerd in de on-premises netwerk] [ planning-guide-2.2] in dit document.
* Als domeinaccounts zijn gebruikt als DBMS aanmeldingen of gebruikers bij het uitvoeren van de system on-premises en deze virtuele machines moeten worden geïmplementeerd in alleen-scenario's, moeten de gebruikers van een domein worden verwijderd. U moet ervoor zorgen dat de lokale beheerder plus een andere VM lokale gebruiker is toegevoegd als een aanmeldingsgebruiker die in de DBMS-Administrators.
* Add andere lokale accounts kunnen die nodig zijn voor het scenario met specifieke implementatie.

- - -
> ![Windows][Logo_Windows] Windows
>
> In dit scenario is geen generaliseren (sysprep) van de virtuele machine vereist om te uploaden en implementeer de virtuele machine in Azure.
> Zorg ervoor dat station die d:\ wordt niet gebruikt.
> Schijf automount voor gekoppelde schijven ingesteld zoals beschreven in hoofdstuk [instelling automount voor gekoppelde schijven] [ planning-guide-5.5.3] in dit document.
>
> ![Linux][Logo_Linux] Linux
>
> In dit scenario geen generaliseren (waagent-deprovision) van de virtuele machine is vereist om te uploaden en implementeren van de virtuele machine in Azure.
> Zorg ervoor dat mnt/resourcegroep niet wordt gebruikt en dat alle schijven zijn gekoppeld via de uuid. Zorg ervoor dat de vermelding bootloader ook het koppelpunt op basis van uuid weerspiegelt de besturingssysteemschijf.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Voorbereiding voor het implementeren van een virtuele machine met een installatiekopie klantspecifieke voor SAP
VHD-bestanden met een gegeneraliseerde OS worden opgeslagen in de containers op Azure Storage-Accounts of als schijfkopieën beheerd. U kunt een nieuwe virtuele machine van een dergelijke installatiekopie implementeren die verwijzen naar de installatiekopie van het VHD- of schijf beheerd als een bron in uw implementatie sjabloonbestanden zoals beschreven in hoofdstuk [Scenario 2: een virtuele machine met een aangepaste installatiekopie implementeren voor SAP] [ deployment-guide-3.3]van de [Implementatiehandleiding][deployment-guide].

Vereisten bij het voorbereiden van uw eigen Azure VM-installatiekopie zijn:

* De VHD met het besturingssysteem kan oorspronkelijk alleen een maximale grootte van 127GB hebben. Deze beperking kreeg aan het einde van maart 2015 geëlimineerd. Nu de VHD met het besturingssysteem maximaal 1TB groot zijn kan als andere Azure-Storage VHD ook gehoste.
* Het moet in de vaste VHD-indeling. Dynamische virtuele harde schijven of VHD's in de VHDx-indeling zijn nog niet ondersteund in Azure. Dynamische virtuele harde schijven worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met commandlets PowerShell of CLI
* VHD's die zijn gekoppeld aan de virtuele machine en moeten worden gekoppeld, opnieuw in Azure aan de virtuele machine zich in een vaste VHD-indeling ook. Lees [in dit artikel (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) en [in dit artikel (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) voor de maximale grootte van gegevensschijven. Dynamische virtuele harde schijven worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met commandlets PowerShell of CLI
* Omdat alle domeingebruikers is geregistreerd als gebruikers in de virtuele machine niet in een scenario alleen in de Cloud bestaat (Zie hoofdstuk [alleen in de Cloud - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk] [ planning-guide-2.1] van dit document), gebruik van dergelijke accounts niet werken mogelijk wanneer de installatiekopie wordt geïmplementeerd in Azure-domein services. Dit geldt met name voor accounts die worden gebruikt voor het uitvoeren van services zoals DBMS of SAP-toepassingen. Daarom moet u dergelijke domeinaccounts vervangen door de lokale accounts van virtuele machine en het lokale domeinaccounts in de virtuele machine te verwijderen. On-premises domeingebruikers behouden in de VM-installatiekopie niet mogelijk een probleem wanneer de virtuele machine wordt geïmplementeerd in het scenario voor cross-premises, zoals beschreven in hoofdstuk [Cross-Premises - implementatie van één of meerdere SAP-virtuele machines in Azure met de vereiste is volledig geïntegreerd in de on-premises netwerk] [ planning-guide-2.2] in dit document.
* Voeg een andere lokale account met administrator-bevoegdheden die kan worden gebruikt door de Microsoft-ondersteuning in probleem onderzoeken of die kunnen worden toegewezen als context voor services en toepassingen worden uitgevoerd in totdat de virtuele machine is geïmplementeerd en geschiktere gebruikers kan worden gebruikt.
* In de Cloud-implementaties en waar domeinaccounts zijn gebruikt als DBMS aanmeldingen of gebruikers bij het uitvoeren van de system on-premises, moeten domeingebruikers worden verwijderd. U moet ervoor zorgen dat de lokale beheerder plus een andere VM lokale gebruiker is toegevoegd als een aanmeldingsgebruiker van de DBMS-Administrators.
* Add andere lokale accounts kunnen die nodig zijn voor het scenario met specifieke implementatie.
* Als de afbeelding bevat een installatie van SAP NetWeaver en het hernoemen van de hostnaam van de oorspronkelijke naam op het moment van de Azure-implementatie is waarschijnlijk dat het beste de nieuwste versies van het SAP Software inrichting Manager dvd-station te kopiëren naar de sjabloon. Hierdoor kunt u eenvoudig de SAP opgegeven rename-functionaliteit om te gebruiken om de gewijzigde hostnaam aanpassen en/of de SID van het SAP-systeem in de geïmplementeerde VM-installatiekopie wijzigen als een nieuw exemplaar wordt gestart.

- - -
> ![Windows][Logo_Windows] Windows
>
> Zorg ervoor dat station D:\ niet is ingesteld schijf automount voor de gekoppelde schijven gebruikt zoals beschreven in hoofdstuk [instelling automount voor gekoppelde schijven] [ planning-guide-5.5.3] in dit document.
>
> ![Linux][Logo_Linux] Linux
>
> Zorg ervoor dat mnt/resourcegroep niet wordt gebruikt en dat alle schijven zijn gekoppeld via de uuid. Zorg dat de vermelding bootloader weerspiegelt ook het koppelpunt op basis van uuid voor de besturingssysteemschijf.
>
>

- - -
* SAP-GUI (voor administratieve en doeleinden setup) vooraf in deze sjabloon kan worden geïnstalleerd.
* Andere software die nodig is voor een geslaagde uitvoering van de virtuele machines in scenario's voor cross-premises kan worden geïnstalleerd, zolang deze software met de nieuwe naam van de virtuele machine werken kunt.

Als de virtuele machine is voldoende voorbereid algemene en uiteindelijk onafhankelijk van accounts/gebruikers niet beschikbaar in het scenario voor specifieke Azure-implementatie, is de laatste stap ter voorbereiding van het generaliseren van dergelijke een afbeelding wordt uitgevoerd.

##### <a name="generalizing-a-vm"></a>Het generaliseren van een virtuele machine
- - -
> ![Windows][Logo_Windows] Windows
>
> De laatste stap is voor aanmelding bij een virtuele machine met een Administrator-account. Open een opdrachtvenster Windows als *beheerder*. Ga naar %windir%\windows\system32\sysprep en sysprep.exe uitvoeren.
> Een klein venster wordt weergegeven. Het is belangrijk om te controleren de **Generalize** optie (de standaardwaarde is uitgeschakeld) en wijzig de optie afsluiten van de standaardwaarde van 'Opnieuw opstarten' 'afsluiten'. Deze procedure wordt ervan uitgegaan dat de sysprep-proces uitgevoerd lokale in het Gastbesturingssysteem van een virtuele machine.
> Als u uitvoeren van de procedure met een VM die al worden uitgevoerd in Azure wilt, volgt u de stappen in [in dit artikel](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Het vastleggen van een virtuele Linux-machine moet worden gebruikt als een Resource Manager-sjabloon][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Virtuele machines en VHD's overbrengen tussen on-premises naar Azure
Omdat de VM-installatiekopieën en schijven uploaden naar Azure is niet mogelijk via de Azure-portal, moet u gebruikmaken van Azure PowerShell-cmdlets of CLI. Een andere mogelijkheid is het gebruik van het hulpprogramma 'AzCopy'. Het hulpprogramma kunt VHD's kopiëren tussen on-premises en Azure (in beide richtingen). Het kan ook VHD's tussen Azure-gebieden kopiëren. Neem contact op met [deze documentatie] [ storage-use-azcopy] voor download en gebruik van AzCopy.

Een derde alternatief kan worden verschillende grafische gebruikersinterface gebaseerde hulpprogramma's van derden gebruiken. Echter, zorg dat deze hulpprogramma's zijn ondersteuning van Azure-pagina-Blobs. Voor onze toepassing moeten we gebruik van Azure-pagina-Blob-archief (de verschillen worden hier beschreven: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). De hulpprogramma's van Azure zijn ook zeer efficiënt voor het comprimeren van de virtuele machines en VHD's die moeten worden geüpload. Dit is belangrijk omdat deze efficiëntie in compressie de tijd voor uploaden vermindert (die variëren toch afhankelijk van de koppeling uploaden met het internet van de lokale-faciliteit en de implementatie van Azure-regio gericht). Het is een evenwichtige veronderstelling dat een virtuele machine of de VHD van de Europese locatie in de VS gebaseerde Azure gegevens centers langer duurt dan dezelfde VM's / VHD uploaden naar de Europese Azure uploaden datacenters.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Uploaden van een VHD van on-premises naar Azure
Voor het uploaden van een bestaande virtuele machine of de VHD van de on-premises netwerk die een virtuele machine of VHD moet voldoen aan de vereisten zoals vermeld in hoofdstuk [voorbereiding voor het verplaatsen van een virtuele machine van on-premises naar Azure met een schijf niet gegeneraliseerd] [ planning-guide-5.2.1] van dit document.

Een virtuele machine hoeft niet te worden gegeneraliseerd en kan worden geüpload in de status en vorm na afsluiten op de lokale kant heeft. Hetzelfde geldt voor extra virtuele harde schijven die niet elk besturingssysteem bevatten.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Een VHD uploaden en waardoor het een Azure-schijf
In dit geval willen we uploaden van een VHD, met of zonder een besturingssysteem, en deze koppelen aan een virtuele machine als een gegevensschijf of gebruiken als de besturingssysteemschijf. Dit is een proces met meerdere stappen

**Powershell**

* Aanmelden bij uw abonnement met *Login-AzureRmAccount*
* Instellen van het abonnement van uw context met *Set-AzureRmContext* en parameter abonnements-id of SubscriptionName - Zie <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Uploaden van de VHD met *toevoegen AzureRmVhd* naar een Azure Storage-Account - Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Optioneel) Een schijf beheerd maken van de VHD met *nieuw AzureRmDisk* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* De schijf met het besturingssysteem van een nieuwe VM-configuratie ingesteld op de VHD- of schijf beheerd met *Set AzureRmVMOSDisk* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* Een nieuwe virtuele machine maken vanuit de VM-configuratie met *New-AzureRmVM* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Een gegevensschijf toevoegen aan een nieuwe virtuele machine met *toevoegen AzureRmVMDataDisk* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**Azure CLI 2.0**

* Aanmelden bij uw abonnement met *az aanmelding*
* Selecteer uw abonnement met *az account set--abonnement`<subscription name or id`>*
* Uploaden van de VHD met *uploaden van blob storage az* -Zie [met de Azure CLI met Azure Storage][storage-azure-cli]
* (Optioneel) Een schijf beheerd maken van de VHD met *az schijf maken* -Zie https://docs.microsoft.com/cli/azure/disk#az_disk_create
* Maak een nieuwe virtuele machine de geüploade VHD of beheerd schijf opgeven als de besturingssysteemschijf met *az vm maken* en parameter *--koppelen-os-schijf*
* Een gegevensschijf toevoegen aan een nieuwe virtuele machine met *az vm schijf koppelen* en parameter *--nieuwe*

**Sjabloon**

* Uploaden van de VHD met Powershell of Azure CLI
* (Optioneel) Een schijf beheerd maken van de VHD met Powershell, Azure CLI of Azure portal
* Implementeer de virtuele machine met een JSON-sjabloon die verwijzen naar de VHD, zoals wordt weergegeven in [in dit voorbeeld JSON-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json) of met behulp van schijven worden beheerd, zoals wordt weergegeven in [in dit voorbeeld JSON-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-disk-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Implementatie van een VM-installatiekopie
Voor het uploaden van een bestaande virtuele machine of de VHD van de on-premises netwerk om het te gebruiken als een installatiekopie van een virtuele machine van Azure die een virtuele machine of VHD moet voldoen aan de vereisten die worden vermeld in hoofdstuk [voorbereiding voor het implementeren van een virtuele machine met een installatiekopie klantspecifieke voor SAP] [ planning-guide-5.2.2] van dit document.

* Gebruik *sysprep* op Windows of *waagent-deprovision* op Linux generaliseren van uw VM - Zie [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx) voor Windows of [het vastleggen van een Virtuele Linux-machine moet worden gebruikt als een Resource Manager-sjabloon] [ capture-image-linux-step-2-create-vm-image] voor Linux
* Aanmelden bij uw abonnement met *Login-AzureRmAccount*
* Instellen van het abonnement van uw context met *Set-AzureRmContext* en parameter abonnements-id of SubscriptionName - Zie <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Uploaden van de VHD met *toevoegen AzureRmVhd* naar een Azure Storage-Account - Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Optioneel) Maken van de installatiekopie van een beheerd schijf van de VHD met *nieuw AzureRmImage* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Instellen van de schijf met het besturingssysteem van een nieuwe VM-configuratie voor de
  * VHD met *Set AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Beheerde schijfimage *Set AzureRmVMSourceImage* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* Een nieuwe virtuele machine maken vanuit de VM-configuratie met *New-AzureRmVM* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**Azure CLI 2.0**

* Gebruik *sysprep* op Windows of *waagent-deprovision* op Linux generaliseren van uw VM - Zie [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx) voor Windows of [het vastleggen van een Virtuele Linux-machine moet worden gebruikt als een Resource Manager-sjabloon] [ capture-image-linux-step-2-create-vm-image] voor Linux
* Aanmelden bij uw abonnement met *az aanmelding*
* Selecteer uw abonnement met *az account set--abonnement`<subscription name or id`>*
* Uploaden van de VHD met *uploaden van blob storage az* -Zie [met de Azure CLI met Azure Storage][storage-azure-cli]
* (Optioneel) Maken van de installatiekopie van een beheerd schijf van de VHD met *az installatiekopie maken* -Zie https://docs.microsoft.com/cli/azure/image#az_image_create
* Maak een nieuwe virtuele machine opgeven van de geüploade VHD- of schijfimage beheerd als besturingssysteemschijf met *az vm maken* en parameter *--installatiekopie*

**Sjabloon**

* Gebruik *sysprep* op Windows of *waagent-deprovision* op Linux generaliseren van uw VM - Zie [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx) voor Windows of [het vastleggen van een Virtuele Linux-machine moet worden gebruikt als een Resource Manager-sjabloon] [ capture-image-linux-step-2-create-vm-image] voor Linux
* Uploaden van de VHD met Powershell of Azure CLI
* (Optioneel) Maken van de installatiekopie van een beheerd schijf van de VHD met Powershell, Azure CLI of Azure portal
* Implementeer de virtuele machine met een JSON-sjabloon die verwijzen naar de VHD-installatiekopie, zoals wordt weergegeven in [in dit voorbeeld JSON-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-image/azuredeploy.json) of met behulp van de installatiekopie van de schijf worden beheerd, zoals weergegeven in [in dit voorbeeld JSON-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-to-on-premises"></a>Virtuele harde schijven of beheerde schijven met on-premises downloaden
Azure-infrastructuur als een Service is niet een eenzijdige straat alleen niet in staat voor het uploaden van VHD's en SAP systemen. U kunt verplaatsen SAP systemen van Azure weer naar de lokale hele wereld.

Tijdens de periode van de download kan niet de VHD's of schijven beheerd actief zijn. Zelfs wanneer het downloaden van schijven die zijn gekoppeld aan virtuele machines, moet de virtuele machine worden afgesloten en de toewijzing ongedaan gemaakt. Als u alleen downloaden van de inhoud die vervolgens moet worden gebruikt wilt voor het instellen van een nieuw systeem on-premises en als het is acceptabel die tijdens de tijd van het downloaden en de installatie van het nieuwe systeem die het systeem in Azure kan nog steeds worden operationele database , u kunt een lange uitvaltijd voorkomen door het uitvoeren van een gecomprimeerde databaseback-up naar een schijf en alleen downloaden die schijf in plaats van de virtuele machine voor OS-base ook gedownload.

#### <a name="powershell"></a>PowerShell

  * Downloaden van een beheerde schijf  
  U moet eerst toegang krijgen tot de onderliggende blob van de schijf worden beheerd. Vervolgens kunt u de onderliggende blob kopiëren naar een nieuw opslagaccount en downloaden van de blob van dit opslagaccount.

  ```powershell
  $access = Grant-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzureRmStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzureStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzureStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy to finish
  Get-AzureStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzureRmVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download to finish
  Revoke-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

  * Downloaden van een VHD  
  Zodra het SAP-systeem wordt gestopt en de virtuele machine wordt afgesloten, kunt u de PowerShell-cmdlet opslaan AzureRmVhd op de lokale doel voor het downloaden van de VHD-schijven terug naar de lokale wereld. Als u wilt doen, moet u de URL van de VHD die u in de 'opslag sectie vinden kunt' van de Azure portal (nodig om te navigeren naar het Opslagaccount en de opslagcontainer waarin de VHD is gemaakt) en u moet weten waar de VHD moet worden gekopieerd.

  Vervolgens kunt u gebruikmaken van de opdracht als u door de parameter SourceUri gewoon definiëren als de URL van de VHD te downloaden en de LocalFilePath als de fysieke locatie van de VHD (inclusief de naam). De opdracht kan eruitzien als:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Voor meer details van de cmdlet opslaan AzureRmVhd Controleer hier <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="cli-20"></a>CLI 2.0
  * Downloaden van een beheerde schijf  
  U moet eerst toegang krijgen tot de onderliggende blob van de schijf worden beheerd. Vervolgens kunt u de onderliggende blob kopiëren naar een nieuw opslagaccount en downloaden van de blob van dit opslagaccount.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Downloaden van een VHD   
  Zodra het SAP-systeem wordt gestopt en de virtuele machine wordt afgesloten, kunt u de Azure CLI-opdracht _downloaden van de azure-opslag-blob_ op de lokale doel voor het downloaden van de VHD back-schijven op de lokale wereld. Als u wilt doen, moet u de naam en de container van de VHD die u kunt zoeken in de 'opslag sectie' van de Azure portal (nodig om te navigeren naar het Opslagaccount en de opslagcontainer waarin de VHD is gemaakt) en u moet weten waar de VHD moet worden copi Ed aan.

  Vervolgens kunt u de opdracht gebruikmaken door alleen het vaststellen van de parameters-blobs en containers van de VHD te downloaden en het doel als de fysieke locatie van de VHD (inclusief de naam). De opdracht kan eruitzien als:

  ```
  az storage blob download --name <name of the VHD to download> --container-name <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --file <destination of the VHD to download>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Overdracht van virtuele machines en de schijven in Azure
#### <a name="copying-sap-systems-within-azure"></a>SAP-systemen binnen Azure kopiëren
Een SAP-systeem of zelfs een speciale DBMS-server een SAP-toepassingslaag ondersteunende waarschijnlijk bestaat uit verschillende schijven die met het besturingssysteem met de binaire bestanden of de bestanden voor gegevens en logboekbestanden van de SAP-database bevatten. De Azure-functionaliteit van het kopiëren van schijven noch de Azure functionaliteit van schijven opslaan op een lokale schijf heeft een mechanisme voor synchronisatie, die momentopname meerdere schijven synchroon. Daarom zou de status van de schijven van de gekopieerde of opgeslagen zelfs als die zijn gekoppeld op basis van dezelfde virtuele machine afwijken. Dit betekent dat in het geval concrete van andere gegevens en logfile(s) opgenomen in de verschillende schijven met de database in de end inconsistent zijn zou.

**Conclusie: Om te kopiëren of opslaan van de schijven die deel van de configuratie van een SAP-systeem uitmaken moet u stoppen van het SAP-systeem en moet ook de geïmplementeerde virtuele machine afgesloten. U kunt alleen dan kopiëren of downloaden van de set met schijven kunt u een kopie van het SAP-systeem maken in Azure of on-premises.**

Gegevensschijven kunnen worden opgeslagen als VHD-bestanden in een Azure Storage-Account en rechtstreeks kunnen worden gekoppeld aan een virtuele machine of worden gebruikt als een afbeelding. In dit geval wordt de VHD gekopieerd naar een andere locatie voordat het wordt gekoppeld aan de virtuele machine. De volledige naam van het VHD-bestand in Azure moet uniek zijn binnen Azure. Zoals al eerder vermeld, is de naam van een naam drie delen dat lijkt op:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Gegevensschijven kunnen ook worden beheerd schijven. In dit geval wordt wordt de schijf beheerd gebruikt voor het maken van een nieuwe schijf met beheerd voordat het wordt gekoppeld aan de virtuele machine. De naam van de schijf beheerd moet uniek zijn binnen een resourcegroep.

##### <a name="powershell"></a>PowerShell
U kunt Azure PowerShell-cmdlets gebruiken om te kopiëren van een VHD, zoals wordt weergegeven in [in dit artikel][storage-powershell-guide-full-copy-vhd]. Gebruik nieuw AzureRmDiskConfig en nieuw AzureRmDisk zoals weergegeven in het volgende voorbeeld voor het maken van een nieuwe beheerde schijf.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="cli-20"></a>CLI 2.0
U kunt Azure CLI gebruiken voor het kopiëren van een VHD, zoals wordt weergegeven in [in dit artikel][storage-azure-cli-copy-blobs]. Gebruik voor het maken van een nieuwe beheerde schijf *az schijf maken* zoals weergegeven in het volgende voorbeeld.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Hulpprogramma's van Azure Storage
* <http://storageexplorer.com/>

Professional editions van Azure Storage Explorers vindt u hier:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

De kopie van een VHD zelf binnen een opslagaccount is een proces waarbij slechts enkele seconden (vergelijkbaar met SAN-hardware bij het maken van momentopnamen met vertraagde kopiëren en een kopie op schrijven). Nadat u een kopie hebt van het VHD-bestand kunt u deze koppelen aan een virtuele machine of deze als een afbeelding kopieën van de VHD koppelen aan virtuele machines.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a managed disk to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzureRmVM

# attach a copy of the managed disk to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzureRmDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzureRmDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzureRmVM
```
##### <a name="cli-20"></a>CLI 2.0
```

# attach a vhd to a vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path to vhd>

# attach a managed disk to a vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.

# attach a copy of a managed disk to a vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopiëren van schijven tussen Azure Storage-Accounts
Deze taak kan niet worden uitgevoerd op de Azure-portal. U kunt Azure PowerShell-cmdlets, Azure CLI of een derde partij opslag browser gebruiken. De PowerShell-cmdlets of de CLI-opdrachten kunnen maken en beheren van blobs, waaronder de mogelijkheid blobs asynchroon kopiëren tussen Opslagaccounts en regio's binnen het Azure-abonnement.

##### <a name="powershell"></a>PowerShell
U kunt ook virtuele harde schijven kopiëren tussen abonnementen. Lees voor meer informatie [in dit artikel][storage-powershell-guide-full-copy-vhd].

De basisprincipes van de logica van de cmdlet PS ziet er als volgt:

* Maken van de context van een opslagruimte voor de **bron** storage-account met *nieuw AzureStorageContext* -Zie <https://msdn.microsoft.com/library/dn806380.aspx>
* Maken van de context van een opslagruimte voor de **doel** storage-account met *nieuw AzureStorageContext* -Zie <https://msdn.microsoft.com/library/dn806380.aspx>
* Start de kopie met

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Controleer de status van de kopie in een lus met

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* De nieuwe VHD koppelen aan een virtuele machine, zoals hierboven is beschreven.

Voor Zie voor voorbeelden [in dit artikel][storage-powershell-guide-full-copy-vhd].

##### <a name="cli-20"></a>CLI 2.0
* Start de kopie met

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Controleer de status als de kopie in een lus met

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* De nieuwe VHD koppelen aan een virtuele machine, zoals hierboven is beschreven.

Voor Zie voor voorbeelden [in dit artikel][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Verwerking van de schijf
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Structuur van een VM/schijf voor SAP-implementaties
De verwerking van de structuur van een virtuele machine en de gekoppelde schijven moet in het ideale geval zeer eenvoudig. Klanten ontwikkeld in on-premises installaties tal van manieren van structureren van een server-installatie.

* Een basisschijf die het besturingssysteem en alle binaire bestanden van de DBMS en/of SAP bevat. Deze schijf kan maximaal 1TB groot in plaats van eerdere beperkingen die maximaal 127 GB zijn sinds maart 2015.
* Een of meerdere schijven met de DBMS-logboekbestand van de SAP-database en het logboekbestand van de DBMS tijdelijke opslaggebied (als de DBMS ondersteunt dit). Als de vereisten voor de database logboek IOPS hoog zijn, moet u meerdere schijven stripe om te kunnen bereiken van de IOPS volume dat vereist is.
* Een aantal schijven met één of twee databasebestanden van de SAP-database en de DBMS tijdelijke bestanden ook (als de DBMS dit ondersteunt).

![Verwijzing configuratie van Azure IaaS VM voor SAP][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO beschrijven Linux-structuur)

- - -
> ![Windows][Logo_Windows] Windows
>
> Met veel klanten zagen we configuraties waar, bijvoorbeeld SAP en DBMS binaire bestanden zijn niet geïnstalleerd op het station c:\ waarin het besturingssysteem is geïnstalleerd. Er zijn diverse redenen hiervoor, maar toen we terug naar de hoofdmap probeerden, dit meestal is dat de stations kort zijn en upgrades voor het besturingssysteem nodig extra ruimte 10-15 jaar geleden. Beide voorwaarden te vaak meer tegenwoordig niet van toepassing. Vandaag de dag kan het station c:\ worden toegewezen op grote volumes schijven of virtuele machines. Als u wilt behouden implementaties eenvoudige in de structuur, wordt u aangeraden te volgen de volgende implementatie-patroon voor SAP NetWeaver systemen in Azure
>
> Het wisselbestand van Windows-besturingssysteem moet zich op het station D: (niet-permanente schijf)
>
> ![Linux][Logo_Linux] Linux
>
> De Linux-swapfile onder mnt/mnt/resource plaats op Linux, zoals beschreven in [in dit artikel][virtual-machines-linux-agent-user-guide]. Het wisselbestand kan worden geconfigureerd in het configuratiebestand van de /etc/waagent.conf Linux-Agent. Toevoegen of wijzigen van de volgende instellingen:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Voor het activeren van de wijzigingen, moet u de Linux-Agent met opnieuw opstarten

```
sudo service waagent restart
```

Lees SAP-notitie [1597355] voor meer informatie over de grootte van het wisselbestand aanbevolen

- - -
Het aantal schijven gebruikt voor de DBMS-gegevensbestanden en het type van deze schijven worden gehost op Azure-opslag moet worden bepaald door de IOPS-vereisten en de latentie is vereist. Exacte quota worden beschreven in [in dit artikel (Linux)] [ virtual-machines-sizes-linux] en [in dit artikel (Windows)][virtual-machines-sizes-windows].

Ervaring van SAP-implementaties in de afgelopen 2 jaar geleerd ons enkele opgedane die kunnen worden samengevat als:

* IOPS-verkeer naar andere gegevensbestanden is niet altijd dezelfde omdat bestaande klantsystemen kunnen hebben anders grote gegevensbestanden die hun SAP-database (s) vertegenwoordigt. Als gevolg hiervan blijkt te worden beter een RAID-configuratie via over meerdere schijven gegevensbestanden van de die LUN 's oppervlaktevariaties buiten die plaatsen. Er zijn situaties, met name Azure Standard-opslag waar de snelheid van een IOPS het quotum van één schijf met het transactielogboek DBMS bereikt. In dergelijke scenario's voor het gebruik van Premium-opslag wordt aanbevolen of schijven met een software-RAID ook aggregeren meerdere Standard-opslag.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Best practices prestaties for SQL Server in Azure Virtual Machines][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Configureren van Software-RAID op Linux][virtual-machines-linux-configure-raid]
> * [LVM configureren op een virtuele Linux-machine in Azure][virtual-machines-linux-configure-lvm]
> * [Azure Storage-geheimen en Linux-i/o-optimalisatie](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Premium-opslag is aanzienlijk betere prestaties, met name voor kritieke transactie logboekschrijfbewerkingen weergegeven. Voor SAP-scenario's die naar verwachting productie-achtige prestaties leveren, is het raadzaam VM-serie, die gebruikmaken van Azure Premium-opslag kunt gebruiken.

Houd rekening met de schijf die het besturingssysteem bevat en als het is raadzaam, de binaire bestanden van SAP en de database (base VM), niet meer beperkt tot 127GB is. Deze kan nu maximaal 1TB in grootte hebben. Dit moet voldoende ruimte nodig bestand inclusief bijvoorbeeld SAP batch taaklogboeken te behouden.

Raadpleeg voor meer suggesties en meer informatie, specifiek voor DBMS virtuele machines, de [DBMS Deployment Guide][dbms-guide]

#### <a name="disk-handling"></a>Verwerking van de schijf
In de meeste gevallen moet u extra schijven maken voor het implementeren van de SAP-database in de virtuele machine. We de aandachtspunten besproken voor het aantal schijven in hoofdstuk [VM/schijf structuur voor SAP-implementaties] [ planning-guide-5.5.1] van dit document. De Azure-portal kunt koppelen en ontkoppelen schijven zodra een base VM is geïmplementeerd. De schijven kunnen worden gekoppeld/losgekoppeld wanneer de virtuele machine actief is en wordt uitgevoerd en wanneer deze wordt gestopt. Bij het toevoegen van een schijf wordt de Azure-portal biedt een lege schijf of een bestaande schijf die op dit moment niet is verbonden met een andere virtuele machine te koppelen.

**Opmerking**: schijven kunnen alleen worden gekoppeld aan één virtuele machine op een bepaald moment.

![Koppelen / loskoppelen van schijven met Azure Standard-opslag][planning-guide-figure-1400]

Tijdens de implementatie van een nieuwe virtuele machine, kunt u bepalen of u wilt gebruiken schijven beheerd of de schijven te plaatsen op Azure Storage-Accounts. Als u gebruiken van Premium-opslag wilt, raden wij u aan met schijven beheerd.

Vervolgens moet u beslissen of u wilt maken van een nieuwe en lege schijf of dat u wilt een bestaande schijf die eerder is geüpload en moet worden gekoppeld aan de virtuele machine nu selecteren.

**BELANGRIJKE**: U **DO NOT** wilt Host opslaan in cache gebruiken met Azure Standard-opslag. De voorkeur hostcache laat u de standaardwaarde. Met Azure Premium-opslag moet u inschakelen Lees-Caching als het kenmerk i/o-voornamelijk zoals typische i/o-verkeer op basis van de gegevensbestanden van de database lezen is. In geval van een database transactielogbestand wordt cache niet aanbevolen.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Het koppelen van een gegevensschijf in de Azure portal][virtual-machines-linux-attach-disk-portal]
>
> Als de schijven zijn gekoppeld, moet u zich aanmelden bij de virtuele machine naar de Windows-Disk Manager openen. Als automount niet is ingeschakeld zoals aanbevolen in hoofdstuk [instelling automount voor gekoppelde schijven][planning-guide-5.5.3], het zojuist gekoppelde volume moet worden ondernomen online en geïnitialiseerd.
>
> ![Linux][Logo_Linux] Linux
>
> Als de schijven zijn gekoppeld, moet u aanmelden bij de virtuele machine en het initialiseren van de schijven, zoals beschreven in [in dit artikel][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Als de nieuwe schijf een lege schijf is, moet u ook de schijf formatteren. Voor opmaak, met name voor DBMS gegevens en logboekbestanden dezelfde aanbevelingen als voor bare-metal implementaties van het DBMS van toepassing.

Zoals vermeld in hoofdstuk [begrip van de Microsoft Azure virtuele Machine][planning-guide-3.2], een Azure Storage-account biedt geen oneindige resources in termen van i/o-volume, volume IOPS en gegevens. Meestal zijn DBMS VMs meest betrokken bij deze. Dit is mogelijk beter gebruik van een afzonderlijke Opslagaccount voor elke virtuele machine hebt u enkele grote volumes in de i/o-virtuele machines te implementeren om te kunnen blijven binnen de grenzen van het volume van Azure Storage-Account. Anders moet u om te zien hoe u balans vinden tussen deze virtuele machines tussen verschillende Storage-accounts zonder de limiet van elk één Opslagaccount op. Meer details worden beschreven de [DBMS Deployment Guide][dbms-guide]. U moet ook rekening houden deze beperkingen voor pure SAP-toepassing server-VM's of andere virtuele machines die uiteindelijk mogelijk extra virtuele harde schijven. Deze beperkingen niet van toepassing als u schijf beheerd. Als u van plan bent een Premium-opslag gebruiken, wordt u aangeraden schijf beheerd.

Een ander onderwerp dat relevant is voor de Storage-Accounts is of de VHD's in een Opslagaccount wel Geo-replicatie. Geo-replicatie is ingeschakeld of uitgeschakeld op het niveau van de Storage-Account en niet op het niveau van de virtuele machine. Als geo-replicatie is ingeschakeld, zou de VHD's binnen de Storage-Account in een andere Azure-Datacenter binnen dezelfde regio worden gerepliceerd. Voordat u besluit dit, moet u nadenken over de volgende beperkingen:

Azure Geo-replicatie werkt lokaal op elke VHD op een virtuele machine en wordt het bestand IOs in chronologische volgorde niet gerepliceerd tussen meerdere VHD's op een virtuele machine. Daarom worden de VHD die de basis-VM vertegenwoordigt, evenals de extra virtuele harde schijven gekoppeld aan de virtuele machine gerepliceerd onafhankelijk van elkaar. Dit betekent dat er vindt geen synchronisatie tussen de wijzigingen in de verschillende VHD's. Het feit dat het bestand IOs onafhankelijk van de volgorde waarin deze manier worden geschreven worden gerepliceerd die geo-replicatie is niet van de waarde voor de databaseservers waarop hun databases verdeeld over meerdere VHD's. Naast de DBMS ook mogelijk zijn er andere toepassingen waarbij processen schrijven of manipuleren van gegevens in verschillende virtuele harde schijven en waar het is belangrijk dat de volgorde van de wijzigingen te behouden. Als dat een vereiste is, moet geo-replicatie in Azure niet worden ingeschakeld. Afhankelijk van of u moet of wilt geo-replicatie voor een set van virtuele machines, maar niet voor een andere set, kunt u al categoriseren virtuele machines en hun verwante VHD's in verschillende Storage-Accounts waarvoor geo-replicatie ingeschakeld of uitgeschakeld.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Automount voor gekoppelde schijven instellen
- - -
> ![Windows][Logo_Windows] Windows
>
> Voor virtuele machines die worden gemaakt uit eigen installatiekopieën of schijven, is het nodig is om te controleren en stel de parameter automount mogelijk in. Als deze parameter kunt de virtuele machine na een herstart of het opnieuw implementeren in Azure opnieuw automatisch koppelen van de stations die zijn gekoppeld/gekoppeld.
> De parameter is ingesteld voor de installatiekopieën die is geleverd door Microsoft in Azure Marketplace.
>
> Controleer voordat u de automount instellen, de documentatie van hier het opdrachtregelprogramma uitvoerbare diskpart.exe:
>
> * [Opdrachtregelopties voor DiskPart](https://technet.microsoft.com/library/bb490893.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> Het venster van Windows-opdrachtregel moet worden geopend als administrator.
>
> Als de schijven zijn gekoppeld, moet u zich aanmelden bij de virtuele machine naar de Windows-Disk Manager openen. Als automount niet is ingeschakeld zoals aanbevolen in hoofdstuk [instelling automount voor gekoppelde schijven][planning-guide-5.5.3], wordt de nieuwe gekoppeld volume > moet worden ondernomen online en geïnitialiseerd.
>
> ![Linux][Logo_Linux] Linux
>
> U moet een zojuist gekoppelde lege schijf initialiseren, zoals beschreven in [in dit artikel][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> U moet ook nieuwe schijven toevoegen aan de /etc/fstab.
>
>

- - -
### <a name="final-deployment"></a>Laatste implementatie
Voor de laatste implementatie en de exacte stappen, met name met betrekking tot de implementatie van het SAP uitgebreide bewaking, verwijzen wij u naar de [Deployment Guide][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Toegang tot SAP-systemen die zijn uitgevoerd in Azure Virtual machines
Voor alleen-scenario's, is het raadzaam verbinding maken met de SAP-systemen via het openbare internet met SAP-gebruikersinterface. Voor dergelijke gevallen moeten de volgende procedures worden toegepast.

Verderop in dit document behandeld de andere belangrijke scenario verbinding met SAP-systemen in de cross-premises implementaties die een site-naar-site-verbinding (VPN-tunnel) of Azure ExpressRoute-verbinding tussen de on-premises systemen en Azure systemen hebben.

### <a name="remote-access-to-sap-systems"></a>Externe toegang tot SAP-systemen
Met Azure Resource Manager zijn er geen Standaardeindpunten meer zoals in het vorige klassieke model. Alle poorten van een Azure ARM VM zijn geopend, zolang:

1. Er is geen Netwerkbeveiligingsgroep is gedefinieerd voor het subnet of de netwerkinterface. Netwerkverkeer virtuele Azure-machines kan worden beveiligd via zogenaamde 'Netwerkbeveiligingsgroepen'. Zie voor meer informatie [wat is er een Netwerkbeveiligingsgroep (NSG)?][virtual-networks-nsg]
2. Er is geen Azure Load Balancer is gedefinieerd voor de netwerkinterface   

Zie het architectuur verschil tussen het klassieke model en ARM in zoals beschreven in [in dit artikel][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Configuratie van de verbinding met SAP-systeem en SAP-GUI voor alleen-scenario
Raadpleeg dit artikel waarin informatie in dit onderwerp wordt beschreven: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Wijzigen van firewallinstellingen in VM
Kan het nodig zijn de firewall te configureren op uw virtuele machines waarmee binnenkomend verkeer naar uw SAP-systeem.

- - -
> ![Windows][Logo_Windows] Windows
>
> De Windows Firewall in een Azure geïmplementeerde VM is standaard ingeschakeld. U nu wilt toestaan dat de SAP-poort moet worden geopend, anders de SAP-gebruikersinterface wordt geen verbinding kunnen maken.
> Om dit te doen:
>
> * Open configuratiescherm\systeem en beveiliging\windows Firewall **geavanceerde instellingen**.
> * Nu met de rechtermuisknop op de regels voor binnenkomende verbindingen en hebt gekozen **nieuwe regel**.
> * In de volgende Wizard hebt gekozen voor het maken van een nieuwe **poort** regel.
> * In de volgende stap van de wizard laat u de instelling op TCP- en typ het poortnummer dat u wilt openen. Omdat de exemplaar-ID van onze SAP 00, duurde we 3200. Als uw exemplaar een ander exemplaar getal heeft, kan de poort die u hebt gedefinieerd eerder op basis van het exemplaarnummer moet worden geopend.
> * In het volgende gedeelte van de wizard, moet u het item laten **verbinding toestaan** gecontroleerd.
> * In de volgende stap van de wizard die u wilt definiëren of de regel voor Domain, Private en openbare netwerk geldt. Wijzig deze indien nodig aan uw behoeften. Echter, een verbinding met SAP-GUI van buitenaf via het openbare netwerk, moet u de regel die wordt toegepast op het openbare netwerk.
> * Naam in de laatste stap van de wizard voor de regel en opslaan door te drukken **voltooien**.
>
> De regel wordt onmiddellijk van kracht.
>
> ![De regeldefinitie poort][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> De Linux-afbeeldingen in Azure Marketplace komen de firewall iptables niet standaard ingeschakeld en de verbinding met SAP-systeem moet werken. Als u iptables of een andere firewall hebt ingeschakeld, raadpleeg dan de documentatie van iptables of de gebruikte firewall waarmee binnenkomende tcp-verkeer op poort 32xx (waarbij xx is voor het systeemnummer van uw SAP-systeem).
>
>

- - -
#### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging
De gebruikersinterface van SAP verbinding niet direct met een van de SAP-exemplaren (poort 32xx) die worden uitgevoerd, maar eerst verbinding maakt via de poort geopend voor het SAP bericht server-proces (poort 36xx). In het verleden is zeer dezelfde poort door de berichtenserver gebruikt voor de interne communicatie met de exemplaren van een toepassing. Om te voorkomen dat lokale toepassingsservers per ongeluk communiceert met een message-server in Azure de poorten voor interne communicatie kunnen worden gewijzigd. Het is raadzaam de interne communicatie tussen de server SAP-bericht en de exemplaren van een toepassing wijzigen in een ander poortnummer op systemen die hebben is gekloond van on-premises systemen, zoals een kloon van ontwikkeld voor het project testen enzovoort. Dit kan worden gedaan met de standaardparameter van profiel:

> rdisp/msserv_internal
>
>

zoals beschreven in [beveiligingsinstellingen voor de Server SAP-bericht](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Concepten van Cloud-implementatie van SAP-exemplaren
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Met SAP NetWeaver demo/training scenario één virtuele machine
![In Azure Cloud Services met één VM SAP demo-systemen met hetzelfde VM-namen, geïsoleerd][planning-guide-figure-1700]

In dit scenario (Zie hoofdstuk [Cloudconfiguratie] [ planning-guide-2.1] van dit document) we bij het implementeren van een typische training/demo-systeem scenario waarbij het volledige training/demo-scenario is opgenomen in een enkele virtuele machine. We gaan ervan uit dat de implementatie wordt gedaan door VM-sjablonen voor de installatiekopie. We ook wordt ervan uitgegaan dat meerdere van deze demo/trainingen VM's moeten worden geïmplementeerd met de virtuele machines met dezelfde naam.

De veronderstelling is dat u een VM-installatiekopie gemaakt zoals beschreven in sommige secties van hoofdstuk [voorbereiden van virtuele machines met SAP voor Azure] [ planning-guide-5.2] in dit document.

De volgorde van gebeurtenissen voor het implementeren van het scenario ziet er als volgt:

##### <a name="powershell"></a>PowerShell
* Maak een nieuwe resourcegroep voor elke training/demo-liggend

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Een nieuw opslagaccount maken als u niet wilt beheerd schijven te gebruiken

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Maak een nieuw virtueel netwerk voor elke liggend training/demo voor het gebruik van dezelfde hostnaam en IP-adressen inschakelen. Het virtuele netwerk wordt beveiligd door een Netwerkbeveiligingsgroep waarmee alleen verkeer op poort 3389 voor extern bureaublad-toegang en -poort 22 voor SSH.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Maak een nieuw openbaar IP-adres die kan worden gebruikt voor toegang tot de virtuele machine vanaf het internet

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Een nieuwe netwerkinterface voor de virtuele machine maken

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Maak een virtuele machine. Elke virtuele machine wordt dezelfde naam hebben voor het scenario alleen in de Cloud. De SAP-SID van de SAP NetWeaver-exemplaren in deze VMs wordt hetzelfde zijn ook. De naam van de virtuele machine moet uniek zijn binnen de Azure-resourcegroep, maar in verschillende Azure-resourcegroepen kunt u virtuele machines uitvoeren met dezelfde naam. De 'Administrator'-account van Windows of Linux uit hoofdmap zijn niet geldig. Daarom moet een nieuwe gebruikersnaam voor de beheerder samen met een wachtwoord worden gedefinieerd. De grootte van de virtuele machine moet ook worden gedefinieerd.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Eventueel extra schijven toevoegt en herstellen van de benodigde inhoud. Let erop dat alle blob-namen (URL's voor de blobs) uniek zijn binnen Azure moet.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM

# Optional: Attach additional Managed Disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzureRmVM
```

##### <a name="cli"></a>CLI
De volgende voorbeeldcode kan worden gebruikt op Linux. Voor Windows, neem Gebruik PowerShell zoals hierboven is beschreven of aanpassen van het voorbeeld voor het % rgName % gebruiken in plaats van $rgName en stel de variabele met de opdracht Windows *ingesteld*.

* Maak een nieuwe resourcegroep voor elke training/demo-liggend

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Een nieuw opslagaccount maken

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Maak een nieuw virtueel netwerk voor elke liggend training/demo voor het gebruik van dezelfde hostnaam en IP-adressen inschakelen. Het virtuele netwerk wordt beveiligd door een Netwerkbeveiligingsgroep waarmee alleen verkeer op poort 3389 voor extern bureaublad-toegang en -poort 22 voor SSH.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Maak een nieuw openbaar IP-adres die kan worden gebruikt voor toegang tot de virtuele machine vanaf het internet

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Een nieuwe netwerkinterface voor de virtuele machine maken

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Maak een virtuele machine. Elke virtuele machine wordt dezelfde naam hebben voor het scenario alleen in de Cloud. De SAP-SID van de SAP NetWeaver-exemplaren in deze VMs wordt hetzelfde zijn ook. De naam van de virtuele machine moet uniek zijn binnen de Azure-resourcegroep, maar in verschillende Azure-resourcegroepen kunt u virtuele machines uitvoeren met dezelfde naam. De 'Administrator'-account van Windows of Linux uit hoofdmap zijn niet geldig. Daarom moet een nieuwe gebruikersnaam voor de beheerder samen met een wachtwoord worden gedefinieerd. De grootte van de virtuele machine moet ook worden gedefinieerd.

```
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path to image vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* Eventueel extra schijven toevoegt en herstellen van de benodigde inhoud. Let erop dat alle blob-namen (URL's voor de blobs) uniek zijn binnen Azure moet.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Template
U kunt de voorbeeldsjablonen gebruiken in de azure-snelstartsjablonen opslagplaats op github.

* [Eenvoudige Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Eenvoudige Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [VM van de installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Implementeren van een set van virtuele machines die nodig zijn om te communiceren in Azure
Dit scenario alleen in de Cloud is een typisch scenario voor trainings- en demo doeleinden waar u de software die de demo-/ training scenario is verspreid over meerdere virtuele machines. De verschillende onderdelen geïnstalleerd in de andere virtuele machines moeten met elkaar communiceren. Opnieuw, in dit scenario geen on-premises netwerkcommunicatie of cross-premises scenario nodig is.

Dit scenario is een uitbreiding van de installatie wordt beschreven in hoofdstuk [één VM met SAP NetWeaver demo/training scenario] [ planning-guide-7.1] van dit document. In dit geval wordt meer virtuele machines worden toegevoegd aan een bestaande resourcegroep. In het volgende voorbeeld wordt de training liggend bestaat uit een SAP ASC's / SCS VM, een virtuele machine met een DBMS en een exemplaar van de toepassingsserver SAP VM.

Voordat u dit scenario maakt, moet u nadenken over de algemene instellingen die al in het scenario voordat u.

#### <a name="resource-group-and-virtual-machine-naming"></a>Naamgeving van de virtuele Machine en resourcegroep
Alle namen van resourcegroepen moet uniek zijn. Uw eigen schematische naam van uw resources, zoals ontwikkelen `<rg-name`>-achtervoegsel.

De naam van de virtuele machine moet uniek zijn binnen de resourcegroep.

#### <a name="set-up-network-for-communication-between-the-different-vms"></a>Netwerk instellen voor de communicatie tussen de verschillende virtuele machines
![Set van virtuele machines binnen een virtuele Azure-netwerk][planning-guide-figure-1900]

Om te voorkomen dat naamconflicten met klonen van de dezelfde training/demo-landschappen, moet u een virtueel Azure-netwerk voor elke liggend maken. DNS-naamomzetting wordt aangeboden door Azure of u kunt uw eigen DNS-server buiten Azure (niet te worden verder hier besproken) configureren. In dit scenario doen we ons eigen DNS niet configureren. Voor alle virtuele machines binnen een Azure Virtual Network worden communicatie via hostnamen ingeschakeld.

De volgende redenen voor het scheiden van training of demo landschappen door virtuele netwerken en niet alleen resourcegroepen kunnen zijn:

* De SAP-liggend zoals ingesteld moet een eigen AD/OpenLDAP en de Server van een domein moet deel uitmaken van elk van de landschappen.  
* De SAP-liggend zoals ingesteld onderdelen die willen werken met vaste IP-adressen heeft.

Meer informatie over virtuele netwerken van Azure en het definiëren van deze vindt u in [in dit artikel][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP virtuele machines implementeren met de zakelijke netwerkverbinding (Cross-Premises)
U een SAP-liggend uitvoeren en wilt delen van de implementatie tussen bare-metal voor geavanceerde DBMS-servers, on-premises gevirtualiseerde omgevingen voor toepassingslagen en kleinere 2-Tier SAP-systemen en Azure IaaS geconfigureerd. De base veronderstelling is dat SAP-systemen binnen één SAP liggend moeten kunnen communiceren met elkaar en met veel andere softwareonderdelen in het bedrijf, onafhankelijk van de implementatie-vorm geïmplementeerd. Ook moet er geen verschillen die zijn geïntroduceerd in de vorm van de implementatie voor de eindgebruiker verbinden met SAP-GUI of andere interfaces. Deze voorwaarden kunnen alleen worden voldaan wanneer we de lokale Active Directory/OpenLDAP en de DNS-services uitgebreid naar de Azure-systemen via site-naar-site/meerdere-on-site verbroken of particuliere verbindingen zoals Azure ExpressRoute.

Om op te halen nader ingegaan op de implementatiegegevens van SAP op Azure, raden we u aan Lees hoofdstuk [implementatie van de concepten van Cloud-Only SAP-exemplaren van] [ planning-guide-7] van dit document waarin wordt uitgelegd enkele van de constructs basisbeginselen van Azure en hoe deze moeten worden gebruikt met SAP-toepassingen in Azure.

### <a name="scenario-of-an-sap-landscape"></a>Scenario met een liggend SAP
Het scenario voor cross-premises kan ongeveer worden beschreven zoals in de onderstaande afbeeldingen:

![Site-naar-Site-connectiviteit tussen on-premises en Azure activa][planning-guide-figure-2100]

Het bovenstaande scenario wordt een scenario beschreven waarin de on-premises AD/OpenLDAP en DNS zijn uitgebreid naar Azure. Een bepaalde IP-adresbereik is aan de lokale gereserveerd per Azure-abonnement. Het IP-adresbereik wordt toegewezen aan een Azure-netwerk op de Azure-zijde.

#### <a name="security-considerations"></a>Beveiligingsoverwegingen
De minimum vereisten is het gebruik van beveiligde communicatie-protocollen, zoals SSL/TLS voor toegang tot de browser of op basis van VPN-verbindingen voor systeemtoegang tot de Azure-services. De veronderstelling is dat bedrijven heel anders de VPN-verbinding tussen hun bedrijfsnetwerk en Azure afgehandeld. Sommige bedrijven mogelijk blankly alle poorten geopend. Sommige andere bedrijven mogelijk zeer nauwkeurig wilt in welke poorten die ze nodig hebben om te openen, enzovoort.

In de tabel hieronder typische SAP worden communicatiepoorten weergegeven. Het is in feite voldoende de SAP-gateway-poort te openen.

| Service | Poortnaam | Voorbeeld `<nn`> = 01 | Standaard-bereik (min-max.) | Opmerking |
| --- | --- | --- | --- | --- |
| Dispatcher |sapdp`<nn>` Zie * |3201 |3200 - 3299 |SAP-Dispatcher, die wordt gebruikt door SAP GUI voor Windows en Java |
| Berichtenserver |sapms`<sid`> Zie ** |3600 |gratis sapms`<anySID`> |SID SAP-systeem-ID = |
| Gateway |sapgw`<nn`> Zie * |3301 |Gratis |SAP-gateway, die wordt gebruikt voor CPIC en RFC communicatie |
| SAP-router |sapdp99 |3299 |Gratis |Namen van alleen CI (centrale exemplaar)-Service kunnen opnieuw worden toegewezen in /etc/services op een willekeurige waarde na de installatie. |

*) nn SAP exemplaarnummer =

*) sid SAP-systeem-ID =

Gedetailleerde informatie over de poorten die nodig zijn voor verschillende SAP-producten of services door SAP producten vindt u hier <http://scn.sap.com/docs/DOC-17124>.
Met dit document moet u het volgende kunnen toegewezen poorten openen in het VPN-apparaat nodig zijn voor specifieke SAP-producten en scenario's.

Andere veiligheidsmaatregelen bij het implementeren van virtuele machines in een dergelijk scenario mogelijk maken een [Netwerkbeveiligingsgroep] [ virtual-networks-nsg] toegangsregels definiëren.

### <a name="dealing-with-different-virtual-machine-series"></a>Omgaan met andere virtuele Machine-reeks
In de loop van de afgelopen 12 maanden toegevoegd Microsoft veel meer typen van de virtuele machine die in aantal vcpu's, geheugen verschillen of belangrijker op hardware wordt uitgevoerd. Niet alle die virtuele machines worden ondersteund met SAP (Zie ondersteund VM typen in SAP-notitie [1928533]). Sommige van deze VMs worden uitgevoerd op een andere host hardware generaties. Deze host hardware generaties worden in de granulatie van een Azure-schaaleenheid ophalen geïmplementeerd. Betekent gevallen kunnen ontstaan waarin de andere VM-grootten die u hebt gekozen kunnen niet worden uitgevoerd op de dezelfde schaaleenheid. De mogelijkheid om te span schaaleenheden op basis van verschillende hardware van wordt een Beschikbaarheidsset beperkt.  Bijvoorbeeld als u uitvoeren van de DBMS op A5 A11-VM's en de toepassingslaag SAP op virtuele machines G-serie wilt, u zou worden afgedwongen om één SAP-systeem of andere SAP-systemen binnen andere Beschikbaarheidssets te implementeren.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Afdrukken op een lokale netwerkprinter uit SAP-exemplaar in Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Afdrukken via TCP/IP in Cross-Premises-scenario
Instellen van uw lokale TCP/IP op basis van netwerkprinters in een Azure VM is de algemene hetzelfde als in uw bedrijfsnetwerk, ervan uitgaande dat u hebt een Site-naar-Site VPN-tunnel of ExpressRoute-verbinding tot stand gebracht.

- - -
> ![Windows][Logo_Windows] Windows
>
> Om dit te doen:
>
> * Sommige netwerkprinters worden geleverd met een configuratiewizard die gemakkelijk voor het instellen van de printer in een Azure VM. Als er geen wizardsoftware heeft met de printer is gedistribueerd, wordt de handmatige manier voor het instellen van de printer is het maken van een nieuwe TCP/IP-printerpoort.
> * Open Configuratiescherm -> apparaten en Printers -> een printer toevoegen
> * Kies toevoegen een printer met een TCP/IP-adres of de hostnaam
> * Typ in het IP-adres van de printer
> * Standaard 9100-printerpoort
> * Indien nodig handmatig het juiste printerstuurprogramma installeren.
>
> ![Linux][Logo_Linux] Linux
>
> * voor Windows alleen opvolgen, zoals de standaardprocedure voor het installeren van een netwerkprinter
> * Volg de openbare Linux handleidingen voor [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) of [Red Hat en Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) over het toevoegen van een printer.
>
>

- - -
![Afdrukken via het netwerk][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Printer op de host via SMB (gedeelde printer) in de Cross-Premises-scenario
Printers op de host zijn niet netwerk compatibel is met opzet. Maar een host gebaseerde printer op de computers in een netwerk kan worden gedeeld, zolang de printer is aangesloten op een computer ingeschakeld op. Verbinding maken met uw bedrijfsnetwerk via Site-naar-Site of ExpressRoute en delen van de lokale printer. Het SMB-protocol maakt gebruik van NetBIOS in plaats van DNS als name-service. De NetBIOS-hostnaam kan afwijken van de DNS-hostnaam. De standaard wordt de NetBIOS-hostnaam en de DNS-hostnaam identiek zijn. Het DNS-domein is niet verstandig in de NetBIOS-naamruimte. De volledig gekwalificeerde DNS-hostnaam die bestaan uit de DNS-hostnaam en DNS-domein moet daarom niet worden gebruikt in de NetBIOS-naamruimte.

De share wordt geïdentificeerd door een unieke naam in het netwerk:

* De hostnaam van de SMB-host (altijd nodig).
* Naam van de share (altijd nodig).
* Naam van het domein zich als printer delen niet in hetzelfde domein als SAP-systeem.
* Bovendien een gebruikersnaam en een wachtwoord vereist voor toegang tot de share.

Procedure:

- - -
> ![Windows][Logo_Windows] Windows
>
> De lokale printer delen.
> Open Windows Verkenner en typt u de sharenaam van de printer in de Azure-VM.
> Een printer-installatiewizard leidt u door het installatieproces.
>
> ![Linux][Logo_Linux] Linux
>
> Hier volgen enkele voorbeelden van documentatie over het configureren van netwerkprinters in Linux of een hoofdstuk waaronder met betrekking tot afdrukken in Linux. Dit wordt in een Azure Linux VM op dezelfde manier werken als de virtuele machine deel van een VPN uitmaakt:
>
> * SLES <_Share_or_Windows_Share https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba)>
> * RHEL- of Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB-Printer (printer doorsturen)
De mogelijkheid van de extern bureaublad-Services te bieden gebruikers de toegang tot hun lokale printer-apparaten in een externe sessie is niet beschikbaar in Azure.

- - -
> ![Windows][Logo_Windows] Windows
>
> Meer informatie over het afdrukken met Windows vindt u hier: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integratie van SAP Azure systemen in correctie en -Transport-systeem (TMS) in de Cross-Premises
De SAP wijzigings- en Transport-systeem (TMS) moet worden geconfigureerd om te exporteren en importeren van transport aanvraag verschillende systemen in de omgeving. We gaan ervan uit dat de exemplaren van de ontwikkeling van een SAP-systeem (DEV) bevinden zich in Azure, terwijl de quality assurance (QA) en productief systemen (PRD) zich on-premises. Bovendien veronderstellen we dat er een centrale transportmap is.

##### <a name="configuring-the-transport-domain"></a>Het Transport-domein configureren
Uw domein Transport configureren op het systeem die u hebt opgegeven als de domeincontroller Transport, zoals beschreven in [configureren van de domeincontroller Transport](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Een systeemgebruiker die TMSADM wordt gemaakt en de vereiste RFC bestemming wordt gegenereerd. U kunt deze RFC-verbindingen met de transactie SM59 kan controleren. Hostnaamomzetting moet zijn ingeschakeld in uw domein transport.

Procedure:

* In ons scenario besloten we dat de QAS on-premises systeem worden de CTS-domeincontroller. Transactie stm-aanroep. Het dialoogvenster TMS wordt weergegeven. Een dialoogvenster Transport-domein configureren wordt weergegeven. (Dit dialoogvenster wordt alleen weergegeven als u een transport-domein nog niet hebt geconfigureerd.)
* Zorg ervoor dat de automatisch gemaakte gebruiker TMSADM is geautoriseerd (SM59 -> ABAP verbinding -> TMSADM@E61.DOMAIN_E61 -> Details -> Utilities(M)-autorisatie Test >). Het eerste scherm van de transactie STM moet blijken dat dit systeem SAP deze als de controller van het domein van het transport functioneert als volgt te werk:

![Startscherm van transactie stm op de domeincontroller][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Inclusief SAP-systemen in het domein Transport
De volgorde van het opnemen van een SAP-systeem in een domein transport ziet er als volgt uit:

* Op het systeem DEV in Azure, Ga naar het transport-systeem (Client 000) en transactie stm aanroepen. Kies andere configuratie in het dialoogvenster en doorgaan met het systeem opnemen in een domein. De domeincontroller opgeven als de doelhost ([SAP-systemen in het domein Transport inclusief](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Het systeem wacht om te worden opgenomen in het domein transport.
* Uit veiligheidsoverwegingen hebt u vervolgens terug te keren naar de domeincontroller om te bevestigen dat de aanvraag. Kies Systeemoverzicht en goedkeuren van het systeem wachten. Bevestig op de opdrachtprompt en de configuratie worden gedistribueerd.

Dit systeem SAP bevat nu de benodigde informatie over alle andere SAP systemen in het domein transport. De gegevens voor het adres van het nieuwe SAP-systeem wordt verzonden naar alle andere SAP systemen en het SAP-systeem wordt ingevoerd in het profiel voor transport van het besturingsprogramma transport op hetzelfde moment. Controleer of RFC's en toegang tot de transportmap van het domein werken.

Ga door met de configuratie van uw systeem transport gewoon zoals beschreven in de documentatie van [wijzigings- en Transport System](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Procedure:

* Zorg ervoor dat uw stm on-premises correct is geconfigureerd.
* Controleer of dat de hostnaam van de domeincontroller Transport kan worden opgelost door de virtuele machine op Azure en andersom visa.
* Aanroep transactie stm -> andere configuratie -> systeem opnemen in een domein.
* Controleer de verbinding in de on premises TMS-systeem.
* Gewoon transport routes, groepen en lagen configureren.

In de site-naar-site verbonden cross-premises scenario's, de latentie tussen on-premises en Azure kunnen nog steeds worden aanzienlijke. Als we overeen met de volgorde van het vervoer van objecten door ontwikkeling en naar productie testsystemen nadenkt over het toepassen van transporten of ondersteuning van pakketten naar de andere systemen, zult u ontdekken dat, afhankelijk van de locatie van de transportmap centrale, systemen met de tegenkomt hoge latentie lezen of schrijven van gegevens in de transportmap centrale. De situatie is vergelijkbaar met SAP liggend configuraties waarin de verschillende systemen via verschillende datacenters met aanzienlijke afstand tussen de datacenters worden verdeeld.

Om dergelijke latentie omzeilen en de systemen werken snel in lezen of schrijven naar of vanuit de transportmap hebt, kunt u twee stm transport domeinen instellen (één voor on-premises en één met de systemen in Azure en koppel de transport-domeinen. Controleer of deze documentatie waarin wordt uitgelegd van de principes achter dit concept in de SAP-TMS: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Procedure:

* Stel een domein transport op elke locatie (on-premises en Azure) met de transactie stm <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Koppelen van de domeinen met een koppeling van het domein en bevestigt u de koppeling tussen de twee domeinen.
  <http://Help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* De configuratie van het gekoppelde systeem distribueren.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC-verkeer tussen SAP-exemplaren die zich in Azure en on-premises (Cross-Premises)
RFC-verkeer tussen de systemen die zich on-premises en in Azure moet werken. Voor het instellen van een verbinding aanroep transactie SM59 in een bronsysteem waarin u wilt definiëren een RFC-verbinding naar het doelsysteem. De configuratie is vergelijkbaar met de standaardinstellingen van een RFC-verbinding.

Er wordt ervan uitgegaan dat in het scenario voor cross-premises, de virtuele machines die uitvoeren SAP-systemen die nodig zijn om te communiceren met elkaar in hetzelfde domein. De installatie van een RFC-verbinding tussen SAP-systemen daarom niet verschillen van de installatiestappen en invoer in scenario's voor lokale.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Toegang tot lokale fileshares van SAP-exemplaren in Azure of andersom
SAP-exemplaren die zich in Azure nodig voor toegang tot bestandsshares die binnen de zakelijke premises. Lokale SAP-exemplaren moeten bovendien toegang tot bestandsshares die in Azure bevinden zich. Om in te schakelen moet u de machtigingen en delen van de opties op het lokale systeem bestandsshares. Zorg ervoor dat de poorten openen op de VPN- of ExpressRoute-verbinding tussen Azure en uw datacenter.

## <a name="supportability"></a>Ondersteuning
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure Bewakingsoplossing voor SAP
Om in te schakelen, de bewaking van bedrijfskritieke SAP systemen op Azure de SAP controlehulpprogramma's SAPOSCOL of de Hostagent SAP gegevens uit de ServiceHost Azure virtuele Machine via een extensie Azure Monitoring voor SAP ophalen. Aangezien de aanvragen door SAP zeer specifiek zijn voor SAP-toepassingen, besloten Microsoft niet algemeen implementeren van de vereiste functionaliteit in Azure, maar laat u het voor klanten voor het implementeren van de vereiste onderdelen voor bewaking en configuraties met hun virtuele Machines in Azure wordt uitgevoerd. Implementatie en de levenscyclus van beheer van de controle-onderdelen wordt echter meestal automatisch door Azure.

#### <a name="solution-design"></a>Ontwerp van de oplossing
De oplossing die is ontwikkeld om te worden SAP-controle inschakelen is gebaseerd op de architectuur van Azure VM-Agent en Extension framework. Het idee van de Azure VM-Agent en de extensie-framework is zodat de installatie van software-toepassingen die beschikbaar zijn in de galerie van Azure VM-extensie binnen een virtuele machine. Het principe idee achter dit concept is (in zulke gevallen de extensie Azure Monitoring voor SAP), dat de implementatie van speciale functionaliteit in een virtuele machine en de configuratie van dergelijke software tijdens de implementatie.

De 'Azure VM-Agent' waarmee de verwerking van specifieke Azure VM-extensies vanuit de virtuele machine is opgenomen in Windows-VM's standaard op de virtuele machine maken in de Azure portal. In geval van een SUSE, Red Hat of Oracle Linux is de VM-agent al deel uit van de Azure Marketplace-installatiekopie. Als een zou een Linux-VM van on-premises naar Azure uploaden beschikt de VM-agent handmatig worden geïnstalleerd.

De bouwstenen van de oplossing Monitoring in Azure voor SAP ziet er als volgt uit:

![Onderdelen van de uitbreiding voor Microsoft Azure][planning-guide-figure-2400]

Zoals u in het bovenstaande blokdiagram, wordt een deel van de oplossing voor controle voor SAP wordt gehost in de Azure VM-installatiekopie en de extensie-galerie van Azure een globaal gerepliceerde opslagplaats die wordt beheerd door de Azure-bewerkingen. Het is de verantwoordelijkheid van de gemeenschappelijke SAP/MS-team werkt op de Azure SAP werken met Azure-bewerkingen voor het publiceren van nieuwe versies van de extensie Azure Monitoring voor SAP-implementatie.

Wanneer u een nieuwe Windows VM implementeert, wordt de Azure VM-Agent automatisch toegevoegd aan de virtuele machine. De functie van deze agent is voor het coördineren van het laden en de configuratie van de Azure-extensies voor de bewaking van SAP NetWeaver-systemen. Voor Linux VM's is de Azure VM-Agent al deel uit van de Azure Marketplace OS-installatiekopie.

Er is echter een stap die nog moet worden uitgevoerd door de klant. Dit is de activering en configuratie van de prestatieverzameling. Het proces dat is gerelateerd aan de configuratie wordt automatisch uitgevoerd door een PowerShell-script of opdracht CLI. Het PowerShell-script kan worden gedownload in de Microsoft Azure Script Center, zoals beschreven in de [Deployment Guide][deployment-guide].

De algehele architectuur van de oplossing voor Azure controle voor SAP ziet eruit als:

![Bewaking van de oplossing voor SAP NetWeaver Azure][planning-guide-figure-2500]

**Voor de exacte instructies en voor gedetailleerde stappen voor het gebruik van deze PowerShell-cmdlets of de CLI opdracht tijdens implementaties van, volg de instructies in de [Deployment Guide][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integratie van Azure bevinden SAP-exemplaar in SAProuter
SAP-exemplaren die worden uitgevoerd in Azure moeten toegankelijk zijn vanuit SAProuter ook.

![SAP-Router-netwerkverbinding][planning-guide-figure-2600]

Een SAProuter kan de TCP/IP-communicatie tussen deelnemende systemen als er geen directe IP-verbinding. Dit biedt het voordeel dat er geen end-to-end-verbinding tussen de partners communicatie nodig op netwerkniveau is. De SAProuter luistert op poort 3299 standaard.
Voor SAP-exemplaren verbinding via een SAProuter moet u de naam van SAProuter tekenreeks en de host met een poging om verbinding te geven.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Tot nu toe de focus van het document is SAP NetWeaver in het algemeen of de SAP NetWeaver ABAP stack. In deze sectie kort worden specifieke aandachtspunten voor de SAP-Java-stack weergegeven. Een van de belangrijkste uitsluitend op basis van SAP NetWeaver Java-toepassingen is de SAP Enterprise Portal. Andere SAP NetWeaver gebaseerde toepassingen, zoals SAP-PI en SAP-oplossing Manager de SAP NetWeaver ABAP en Java-stacks gebruiken. Daarom zeker is er moeten specifieke aspecten met betrekking tot de SAP NetWeaver Java-stack.

### <a name="sap-enterprise-portal"></a>SAP Enterprise Portal
De installatie van een SAP-Portal in een virtuele Machine van Azure niet afwijken van de installatie van een on premises als u in scenario's voor cross-premises implementeert. Omdat de DNS-server wordt gedaan door on-premises, kunnen de poortinstellingen van de afzonderlijke exemplaren worden gedaan als de geconfigureerde lokale. De aanbevelingen en beperkingen tot nu toe die worden beschreven in dit document voor een toepassing zoals SAP Enterprise Portal of de SAP NetWeaver Java-stack in het algemeen gelden.

![Blootgestelde SAP-Portal][planning-guide-figure-2700]

Een speciale implementatiescenario door sommige klanten is de rechtstreekse blootstelling van de SAP Enterprise Portal met het Internet als de virtuele machinehost is verbonden met het bedrijfsnetwerk via site-naar-site VPN-tunnel of ExpressRoute. U hebt voor een scenario om ervoor te zorgen dat bepaalde poorten geopend en niet wordt geblokkeerd door een firewall of netwerk beveiligingsgroep zijn. Het hetzelfde mechanisme moet worden toegepast wanneer u verbinding wilt maken met een SAP-Java-exemplaar van on-premises in een scenario alleen in de Cloud.

De eerste portal URI is http (s):`<Portalserver`>: 5XX00/irj waar de poort wordt gevormd door 50000 plus (Systemnumber veldnamenrij? 100). De portal van SAP-URI van het systeem 00 is `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Voor meer informatie, hebt u kijken <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Endpoint-configuratie][planning-guide-figure-2800]

Als u aanpassen van de URL en/of de poorten van uw SAP Enterprise Portal wilt, controleert u in deze documentatie:

* [De URL van de Portal wijzigen](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Standaardpoortnummers, Portal-poortnummers wijzigen](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Hoge beschikbaarheid (HA) en Disaster Recovery (DR) voor SAP NetWeaver uitgevoerd op Azure Virtual Machines
### <a name="definition-of-terminologies"></a>Definitie van de terminologie
De term **hoge beschikbaarheid (HA)** in het algemeen is gerelateerd aan een reeks technologieën waarmee IT onderbrekingen minimaliseert door zakelijke continuïteit van de IT-services door middel van redundante en fouttolerantie of failover beveiligde onderdelen in de **dezelfde** Datacenter. In ons geval binnen een Azure-regio.

**Herstel na noodgeval (DR)** is ook gericht Minimalisatie van het aantal onderbreking van de IT-services en hun herstel maar meerdere **verschillende** datacenters, die meestal zich honderden kilometers verwijderd. In ons geval meestal tussen verschillende Azure-regio's binnen dezelfde geopolitieke regio of als tot stand gebracht door u als klant.

### <a name="overview-of-high-availability"></a>Overzicht van hoge beschikbaarheid
We kunnen de discussie over SAP hoge beschikbaarheid in Azure uit twee delen gescheiden:

* **Hoge beschikbaarheid van Azure-infrastructuur**, bijvoorbeeld HA van compute (VM's), netwerk, opslag, enz. en de voordelen voor de beschikbaarheid van de SAP verhogen.
* **Hoge beschikbaarheid voor SAP-toepassing**, bijvoorbeeld HA van SAP-software-onderdelen:
  * SAP-toepassingsservers
  * SAP ASC's / SCS-exemplaar
  * Databaseserver

en hoe deze kan worden gecombineerd met de Azure-infrastructuur HA.

SAP hoge beschikbaarheid in Azure heeft enkele verschillen in vergelijking met SAP hoge beschikbaarheid in een on-premises fysieke of virtuele omgeving. Het volgende artikel van het SAP beschrijft standaardconfiguraties SAP hoge beschikbaarheid in gevirtualiseerde omgevingen in Windows: <http://scn.sap.com/docs/DOC-44415>. Er is geen sapinst geïntegreerd SAP-HA configuratie voor Linux zoals deze voor Windows bestaat. Met betrekking tot SAP HA on-premises voor Linux hier voor meer informatie kunt vinden: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Hoge beschikbaarheid van Azure-infrastructuur
Er is een enkel VM SLA van 99,9%. Om te leren hoe de beschikbaarheid van één VM eruit alsof u gewoon het product van de verschillende beschikbare Azure Sla's kunt opbouwen: <https://azure.microsoft.com/support/legal/sla/>.

De basis voor de berekening is 30 dagen per maand of 43200 minuten. Daarom 0,05% uitvaltijd overeenkomt met 21,6 minuten. De beschikbaarheid van de andere services wordt gewoon, Vermenigvuldig in de volgende manier:

(Beschikbaarheidservice #1/100) * (beschikbaarheidservice #2/100) * (beschikbaarheidservice #3/100) 

Zoals:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 of een algemene beschikbaarheid van 99.75%.

#### <a name="virtual-machine-vm-high-availability"></a>Hoge beschikbaarheid van virtuele Machine (VM)
Er zijn twee soorten gebeurtenissen van Azure-platform die kunnen invloed hebben op de beschikbaarheid van uw virtuele machines: gepland onderhoud en niet-gepland onderhoud.

* Gebeurtenissen voor gepland onderhoud zijn periodieke updates die zijn aangebracht door Microsoft in de onderliggende Azure-platform voor het verbeteren van de algehele betrouwbaarheid, prestaties en beveiliging van de platforminfrastructuur die op uw virtuele machines worden uitgevoerd.
* Niet-gepland onderhoud gebeurtenissen treden op wanneer de hardware of fysieke infrastructuur onderliggende uw virtuele machine een fout in een bepaalde manier opgetreden is. Voorbeelden hiervan zijn lokale netwerkproblemen, lokale schijfdefecten of andere defecten op rack-niveau. Wanneer een dergelijke fout wordt gedetecteerd, wordt de Azure-platform automatisch uw virtuele machine migreren van de slechte fysieke server die als host fungeert voor uw virtuele machine naar een gezonde fysieke server. Dergelijke gebeurtenissen zijn zeldzaam, maar kunnen er ook toe leiden dat uw virtuele machine opnieuw moet opstarten.

Meer informatie vindt u in deze documentatie: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure Storage redundantie
De gegevens in uw Microsoft Azure Storage-Account worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid, voldoen aan de SERVICEOVEREENKOMST van Azure Storage zelfs met betrekking tot tijdelijke hardwarefouten.

Omdat Azure Storage is het houden van drie afbeeldingen van de gegevens standaard, zijn RAID 5- of RAID1 over meerdere Azure-schijven niet nodig.

Meer informatie vindt u in dit artikel: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Met behulp van Azure-infrastructuur VM opnieuw opstarten om te zorgen voor hogere beschikbaarheid van SAP-toepassingen
Als u besluit geen gebruik van functies zoals Windows Server Failover Clustering (WSFC)- of pacemaker heeft op Linux te (momenteel alleen ondersteund voor SLES 12 en hoger), Azure virtuele machine opnieuw opstarten wordt gebruikt voor het beveiligen van een SAP-systeem tegen de geplande en ongeplande uitval van de Azure fysieke serverinfrastructuur en algemene onderliggende Azure-platform.

> [!NOTE]
> Het is belangrijk te vermelden dat voornamelijk Azure VM opnieuw virtuele machines en geen toepassingen beveiligt. VM starten biedt geen hoge beschikbaarheid voor SAP-toepassingen, maar het biedt een zekere mate van beschikbaarheid van de infrastructuur en daarom indirect hogere beschikbaarheid van SAP-systemen. Er is ook geen SLA voor de tijd die het duurt om te starten van een virtuele machine na een onderbreking gepland of ongepland host. Deze methode van hoge beschikbaarheid is daarom niet geschikt voor belangrijke onderdelen van een systeem SAP zoals (A) SCS of DBMS.
>
>

Een ander belangrijk infrastructuur element voor hoge beschikbaarheid is opslag. Voorbeeld is Azure Storage SLA 99,9% beschikbaarheid. Als er een implementeert alle virtuele machines met de schijven in één Azure Storage-Account, potentiële Azure Storage is niet beschikbaar zijn, wordt er geen beschikbaar zijn voor alle virtuele machines die in die Azure Storage-Account worden geplaatst en ook alle SAP onderdelen die op deze virtuele machines worden uitgevoerd.  

In plaats van alle virtuele machines in één enkele Azure Storage-Account te stellen, kunt u ook speciale opslag gebruiken accounts voor elke virtuele machine en op deze manier algemene beschikbaarheid van virtuele machine en SAP verhogen met behulp van meerdere onafhankelijke Azure Storage-Accounts.

Azure-schijven die beheerd worden automatisch opgenomen in het Foutdomein van deze zijn gekoppeld aan virtuele machine. Als u twee virtuele machines in een beschikbaarheidsset en beheerd schijven worden gebruikt, zorgt het platform voor het distribueren van de schijven worden beheerd in verschillende domeinen met fouten ook. Als u van plan bent een Premium-opslag gebruiken, maximaal aangeraden schijven beheren en gebruiken.

Een voorbeeldarchitectuur voor een SAP NetWeaver-systeem dat gebruikmaakt van Azure-infrastructuur HA en storage-accounts kan er als volgt uitzien:

![Met behulp van Azure-infrastructuur HA voor hogere beschikbaarheid voor SAP-toepassing][planning-guide-figure-2900]

Een voorbeeldarchitectuur voor een SAP NetWeaver-systeem dat gebruikmaakt van Azure-infrastructuur HA en schijven beheerd kan er als volgt uit:

![Met behulp van Azure-infrastructuur HA voor hogere beschikbaarheid voor SAP-toepassing][planning-guide-figure-2901]

Voor kritieke SAP-onderdelen wordt bereikt het volgende tot nu toe:

* Hoge beschikbaarheid van SAP-toepassingsservers (AS)

  SAP-toepassingsexemplaren server zijn redundante componenten. Elk SAP omdat exemplaar wordt geïmplementeerd op een eigen virtuele machine die wordt uitgevoerd in een andere Azure-fouten en het upgraden van domein (Zie hoofdstukken [Foutdomeinen] [ planning-guide-3.2.1] en [domeinen upgraden][planning-guide-3.2.2]). Dit is gewaarborgd met behulp van Beschikbaarheidssets van Azure (Zie hoofdstuk [Beschikbaarheidssets van Azure][planning-guide-3.2.3]). Mogelijke gepland of ongepland ontbreken van een Azure-fout of het upgraden van domein, wordt niet beschikbaar zijn van een beperkt aantal virtuele machines met hun SAP-AS exemplaren.

  Elk omdat exemplaar wordt geplaatst in een eigen Azure Storage-account - potentiële ontbreken van een Azure Storage-Account wordt niet beschikbaar zijn van slechts één virtuele machine met de SAP-AS SAP-exemplaar. Let echter is er een limiet van Azure Storage-Accounts binnen een Azure-abonnement. Om ervoor te zorgen (A) SCS exemplaar automatisch wordt gestart nadat de virtuele machine opnieuw is opgestart, zorg ervoor dat de parameter Autostart om in te stellen (A) SCS-exemplaar starten profiel beschreven in hoofdstuk [met behulp van Autostart voor SAP-exemplaren][planning-guide-11.5].
  Lees ook hoofdstuk [hoge beschikbaarheid voor SAP-toepassingsservers] [ planning-guide-11.4.1] voor meer informatie.

  Zelfs als u schijven beheerd gebruikt, wordt deze schijven worden ook opgeslagen in een Azure Storage-Account en mag niet beschikbaar in een gebeurtenis van een onderbreking van de opslag.

* *Hogere* SCS beschikbaarheid van SAP (A)-exemplaar

  Hier maken we gebruik van Azure VM start opnieuw op ter bescherming van de virtuele machine met geïnstalleerde SCS SAP (A)-exemplaar. In het geval van een geplande of niet-geplande uitvaltijd van Azure-servers, virtuele machines op een andere beschikbare server wordt opnieuw gestart. Zoals eerder gezegd, beveiligt Azure VM starten voornamelijk virtuele machines en niet de toepassingen, in dit geval de (A) SCS-exemplaar. We je indirect hogere beschikbaarheid van SCS SAP (A)-exemplaar bereiken via de virtuele machine opnieuw opstarten. Om te verzekeren (A) SCS exemplaar automatisch wordt gestart nadat de virtuele machine opnieuw is opgestart, zorg ervoor dat parameter Autostart om in te stellen (A) SCS exemplaar start profiel beschreven in hoofdstuk [met behulp van Autostart voor SAP-exemplaren][planning-guide-11.5]. Dit betekent dat de SCS (A) als een fouttolerantie (SPOF) uitgevoerd in één VM-instantie worden de doorslaggevend factor voor de beschikbaarheid van de gehele SAP-Liggend.

* *Hogere* beschikbaarheid van de DBMS-Server

  Hier, vergelijkbaar met het gebruiksvoorbeeld SCS SAP (A)-exemplaar, maken we gebruik van Azure VM start opnieuw op ter bescherming van de virtuele machine met geïnstalleerde software die DBMS en wij bereiken hogere beschikbaarheid van DBMS software via de virtuele machine opnieuw opstarten.
  DBMS uitgevoerd in een enkele virtuele machine is ook een SPOF en is de doorslaggevend factor voor de beschikbaarheid van de gehele SAP-Liggend.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP-toepassing voor hoge beschikbaarheid op Azure IaaS
Als u wilt bereiken volledige SAP hoge beschikbaarheid van het systeem, moeten we alle essentiële SAP-systeemonderdelen, voor het voorbeeld redundante SAP-toepassingsservers en unieke onderdelen (bijvoorbeeld fouttolerantie), zoals SAP (A) SCS-exemplaar en DBMS te beschermen.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Hoge beschikbaarheid voor SAP-toepassingsservers
De SAP-servers/dialoogvenster toepassingsexemplaren is het niet nodig om na te denken over een oplossing voor specifieke hoge beschikbaarheid. Hoge beschikbaarheid gewoon wordt gerealiseerd door redundantie en zodoende met voldoende hiervan in verschillende virtuele machines. Ze moeten alle worden geplaatst in dezelfde Azure Beschikbaarheidsset om te voorkomen dat de virtuele machines op hetzelfde moment tijdens gepland onderhoud uitval mogelijk worden bijgewerkt. De basisfunctionaliteit die is gebaseerd op andere Upgrade en domeinen met fouten binnen een Azure-schaaleenheid al is geïntroduceerd in hoofdstuk [domeinen upgraden][planning-guide-3.2.2]. Azure Beschikbaarheidssets in hoofdstuk gepresenteerd [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] van dit document.

Er is geen oneindig aantal probleem- en Upgrade-domeinen die kunnen worden gebruikt door een Azure-Beschikbaarheidsset binnen een Azure-schaaleenheid. Dit betekent dat als een aantal virtuele machines in een Beschikbaarheidsset, sneller of later meer dan één die VM belandt in dezelfde fout of het upgraden van domein.

Enkele SAP application server-exemplaren in hun toegewezen virtuele machines implementeert en ervan uitgaande dat wij vijf upgraden domeinen, worden de volgende afbeelding ontstaat aan het einde. De werkelijke maximum aantal fouten en update domeinen binnen een beschikbaarheidsset in de toekomst mogelijk wijzigen:

![HA van SAP-toepassingsservers in Azure][planning-guide-figure-3000]

Meer informatie vindt u in deze documentatie: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Hoge beschikbaarheid voor het exemplaar SCS SAP (A) in Windows
Windows Server Failover Cluster (WSFC) is een veelgebruikte oplossing voor het beveiligen van het exemplaar SCS SAP (A). Het is ook geïntegreerd in sapinst in de vorm van een "HA-installatie. De Azure-infrastructuur is op dit moment niet kunnen bieden de functionaliteit voor het instellen van de vereiste Windows Server Failover Cluster dezelfde manier als lokale is uitgevoerd.

Het Azure-cloud-platform waarop het besturingssysteem Windows biedt vanaf januari 2016 niet de mogelijkheid van het gebruik van een gedeeld clustervolume op een schijf die wordt gedeeld tussen twee virtuele Azure-machines.

Een geldige oplossing is echter het gebruik van derden 3rd software waarmee u een gedeeld volume door synchrone en transparante schijfreplicatie die kan worden geïntegreerd in WSFC. Deze aanpak geeft aan dat het actieve clusterknooppunt toegang kunnen krijgen tot een van de exemplaren van de schijf op een punt in tijd is. Configuratie wordt ondersteund vanaf januari 2016 deze HA ter bescherming van het SAP (A) SCS-exemplaar op Windows gastbesturingssysteem op Azure VM's in combinatie met software van derden 3rd SIOS DataKeeper.

De oplossing SIOS DataKeeper biedt een gedeelde schijfclusterbron voor Windows failover-Clusters door:

* Een extra Azure-VHD die is gekoppeld aan elk van de virtuele machines (VM's) die zich in een Cluster van Windows-configuratie
* SIOS DataKeeper Cluster Edition is uitgevoerd op beide knooppunten VM
* SIOS DataKeeper Cluster Edition zodanig dat deze synchroon komt overeen met de inhoud van de aanvullende VHD geconfigureerd met gekoppelde volume van de virtuele bronmachines naar aanvullende VHD volume van doel VM gekoppeld.
* SIOS DataKeeper is onttrokken van de bron- en lokale volumes en ze te presenteren aan Windows Failover Cluster als één gedeelde schijf.

U vindt alle informatie over het installeren van een Windows-failovercluster met SIOS DataKeeper en SAP in de [Clustering SAP ASC's exemplaar met gebruikmaking van Windows Server Failover Cluster in Azure met SIOS DataKeeper] [ ha-guide-classic] witboek.

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Hoge beschikbaarheid voor het exemplaar SAP (A) SCS op Linux
Vanaf december 2015 is er ook geen equivalent voor gedeelde schijf WSFC voor virtuele Linux-machines in Azure. Alternatieve oplossingen met behulp van software 3rd van derden zoals SIOS voor Windows zijn nog niet gevalideerde voor het uitvoeren van SAP op Linux op Azure.

#### <a name="high-availability-for-the-sap-database-instance"></a>Hoge beschikbaarheid voor de database-instantie voor SAP
De SAP DBMS HA standaardinstallatie is gebaseerd op twee DBMS VMs waar DBMS hoge beschikbaarheid-functionaliteit wordt gebruikt om gegevens te repliceren van de actieve DBMS-instantie voor de tweede VM naar een passieve DBMS-instantie.

Hoge beschikbaarheid en noodherstel herstelfunctionaliteit voor DBMS in het algemeen ook zo specifiek DBMS worden beschreven in de [DBMS Deployment Guide][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Hoge beschikbaarheid van de end-to-End voor de volledige SAP-systeem
Hier vindt u twee voorbeelden van een volledige SAP NetWeaver HA-architectuur in Azure - een voor Windows en een voor Linux.

Alleen schijven zonder begeleiding: de concepten zoals hieronder wordt uitgelegd moet mogelijk iets afnemen als u veel SAP-systemen implementeert en het aantal geïmplementeerde virtuele machines het maximum aantal Opslagaccounts per abonnement overschrijdt. In dergelijke gevallen moet de VHD's van VM's binnen een Opslagaccount worden gecombineerd. Meestal zou u doen door een combinatie van VHD's van SAP toepassingslaag virtuele machines van verschillende SAP-systemen.  We gecombineerd ook andere VHD's van verschillende DBMS-virtuele machines van verschillende SAP-systemen in een Azure Storage-Account. Houd waardoor de grenzen van de IOPS van Azure Storage-Accounts in gedachten (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] HA in Windows
![SAP NetWeaver HA toepassingsarchitectuur met SQL Server in Azure IaaS][planning-guide-figure-3200]

De volgende Azure constructs worden gebruikt voor het systeem SAP NetWeaver impact minimaliseren door problemen met de infrastructuur en patchen hosten:

* Het volledige systeem wordt geïmplementeerd in Azure (vereist - DBMS laag, (A) SCS-exemplaar en de volledige toepassingslaag moet uitvoeren op dezelfde locatie).
* Het volledige systeem wordt uitgevoerd binnen een Azure-abonnement (vereist).
* Het volledige systeem uitgevoerd binnen een Azure Virtual Network (vereist).
* De scheiding van de virtuele machines van één SAP-systeem in drie Beschikbaarheidssets is mogelijk zelfs met alle virtuele machines die behoren tot hetzelfde virtuele netwerk.
* Alle exemplaren van een SAP-systeem DBMS actieve VM's zijn in een Beschikbaarheidsset. Er wordt ervan uitgegaan dat er meer dan één VM DBMS exemplaren per systeem wordt uitgevoerd sinds hoge beschikbaarheid van systeemeigen DBMS functies worden gebruikt, zoals SQL Server AlwaysOn- of Oracle Data Guard.
* Alle actieve exemplaren DBMS VM's gebruiken hun eigen opslagaccount. DBMS gegevens en logboekbestanden bestanden worden gerepliceerd uit één opslagaccount naar een ander opslagaccount met DBMS hoge beschikbaarheid functies waarmee de gegevens worden gesynchroniseerd. Niet beschikbaar zijn van een opslagaccount zorgt ervoor dat er geen beschikbaar zijn van een SQL-Windows-clusterknooppunt, maar niet de volledige SQL Server-service.
* Alle virtuele machines (A) SCS-exemplaar van een SAP-systeem uitgevoerd, zijn in een Beschikbaarheidsset. Een Windows Server Failover Cluster (WSFC) is geconfigureerd binnen deze VMs (A) beveiligen SCS-exemplaar.
* Alle virtuele machines (A) SCS exemplaren met hun eigen opslagaccount gebruiken. (A) SCS exemplaarbestanden en SAP globale map worden gerepliceerd van een opslagaccount naar een andere SIOS DataKeeper replicatie met storage-account. Niet beschikbaar zijn van een opslagaccount, wordt er geen beschikbaar zijn van een (A) SCS Windows-clusterknooppunt, maar niet de gehele (A) SCS-service.
* ALLE virtuele machines die de SAP-toepassingslaag server zijn in een derde Beschikbaarheidsset.
* ALLE virtuele machines SAP-toepassingsservers met hun eigen opslagaccount gebruiken. Niet beschikbaar zijn van een opslagaccount zorgt ervoor dat er geen beschikbaar zijn van een SAP-toepassingsserver, waar andere SAP-AS worden uitgevoerd.

De volgende afbeelding geïllustreerd de dezelfde mate beheerd schijven.

![SAP NetWeaver HA toepassingsarchitectuur met SQL Server in Azure IaaS][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] HA op Linux
De architectuur voor SAP HA op Linux in Azure is in wezen hetzelfde als voor Windows zoals hierboven wordt beschreven. Raadpleeg de SAP-notitie [1928533] voor een lijst met ondersteunde hoge beschikbaarheidsoplossingen.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Met behulp van Autostart voor SAP-exemplaren
SAP aangeboden de functionaliteit voor het starten van SAP-exemplaren onmiddellijk na het begin van het besturingssysteem vanuit de virtuele machine. De exacte stappen zijn beschreven in Knowledge Base-artikel SAP [1909114]. SAP is echter niet aanbeveelt voor het gebruik van de instelling niet meer omdat er geen controle in de volgorde van exemplaar opnieuw wordt opgestart, ervan uitgaande dat meer dan één virtuele machine is van invloed op een of meerdere exemplaren per virtuele machine is uitgevoerd. Ervan uitgaande dat een Azure standaardscenario van één SAP application server-exemplaar in een virtuele machine en het geval van één VM uiteindelijk ophalen opnieuw wordt gestart, het automatisch starten is niet echt kritiek en kan worden ingeschakeld door deze parameter toe te voegen:

    Autostart = 1

In het profiel van de start van het SAP ABAP en/of Java-exemplaar.

> [!NOTE]
> De parameter Autostart kan ook een aantal downfalls hebben. De parameter activeert het begin van een SAP ABAP of Java-exemplaar in meer detail wanneer de bijbehorende Windows of Linux-service van het exemplaar wordt gestart. Dat waarschijnlijk het geval is wanneer het besturingssysteem wordt opgestart. Echter opnieuw opstarten van SAP-services zijn ook een algemene ding voor SAP Software levenscyclusbeheer functionaliteit zoals som of andere updates of upgrades. Deze functies zijn er een exemplaar automatisch opnieuw opgestart helemaal niet wordt verwacht. De parameter Autostart moet daarom worden uitgeschakeld voordat u taken uitvoert. De parameter Autostart mag ook niet worden gebruikt voor SAP-exemplaren die zijn geclusterd, zoals SCS-ASC's / CI.
>
>

Zie voor aanvullende informatie met betrekking tot autostart voor SAP hier exemplaren:

* [SAP samen met uw Unix-Server starten/stoppen, starten/stoppen](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Starten en stoppen SAP NetWeaver Beheeragents](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Het inschakelen van automatisch starten van HANA-Database](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Grotere 3-Laagse SAP-systemen
Aspecten van hoge beschikbaarheid van 3-Laagse SAP-configuraties is al in de vorige secties beschreven. Maar hoe zit systemen waarbij de DBMS-server-vereisten te groot is zijn om deze zich bevinden in Azure, maar de toepassingslaag SAP kunnen worden geïmplementeerd in Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Locatie van 3-Laagse SAP-configuraties
Het wordt niet ondersteund als u wilt de toepassingslaag splitsen zelf of de toepassing en DBMS servicetier tussen on-premises en Azure. Een SAP-systeem is volledig geïmplementeerde on-premises of in Azure. Het is ook niet ondersteund als u wilt dat sommige van de toepassingsservers on-premises en andere uitvoeren in Azure. Dat is het beginpunt van de bespreking. We worden ook niet ondersteund als u wilt dat de DBMS-onderdelen van een SAP-systeem en de SAP application server laag geïmplementeerd in twee verschillende Azure-regio's. Bijvoorbeeld DBMS in VS-West en SAP toepassingslaag in VS-midden. Reden voor ondersteunt geen configuratie is de gevoeligheid latentie van de SAP NetWeaver-architectuur.

Echter, in de loop van vorig jaar data center partners ontwikkeld mede locaties met Azure-regio's. Deze CO-locaties zijn vaak zeer dicht in de fysieke Azure data centers binnen een Azure-regio. De korte afstand en de verbinding van activa in CO-locatie via ExpressRoute in Azure, kunnen resulteren in een latentie van minder dan 2 MS. In dergelijke gevallen, als u wilt de DBMS-laag (inclusief opslag SAN/NAS) vinden in een collocatie en de SAP is toepassingslaag in Azure mogelijk. Vanaf december 2015, er zijn momenteel geen implementaties die. Maar verschillende klanten met implementaties van toepassingen niet SAP dergelijke benaderingen al gebruikt.

### <a name="offline-backup-of-sap-systems"></a>Offline back-up van SAP-systemen
Die afhankelijk zijn van de SAP-configuratie (laag 2 of 3-Laagse) er gekozen kan back-up moet worden. De inhoud van de virtuele machine zelf plus een back-up van de database. De DBMS-gerelateerde back-ups moeten worden uitgevoerd met database-methoden. Een gedetailleerde beschrijving voor de verschillende databases en vindt u in [DBMS handleiding][dbms-guide]. Aan de andere kant de SAP-gegevens kan een back-up op een offline manier (met inbegrip van de database-inhoud) zoals beschreven in deze sectie of online zoals beschreven in de volgende sectie.

De offline back-up vereist in feite een afsluiten van de virtuele machine via de Azure portal en een kopie van de VM basisschijf plus alle gekoppelde schijven aan de virtuele machine. Dit zou een punt in tijd installatiekopie van de virtuele machine en de bijbehorende schijf behouden. Het verdient aanbeveling om te kopiëren van de back-ups naar een andere Azure Storage-Account. Daarom de procedure beschreven in hoofdstuk [kopiëren van schijven tussen Azure Storage-Accounts] [ planning-guide-5.4.2] van dit document wilt toepassen.
Naast het afsluiten met de Azure portal een kunt ook dit doen via Powershell of CLI zoals hier wordt beschreven: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Terugzetten van die status van het verwijderen van de basis-VM als de oorspronkelijke schijf van de basis-VM zou bestaan en de gekoppelde schijven, kopiëren terug van de opgeslagen schijven aan de oorspronkelijke Opslagaccount of de resource-groep voor beheerde schijven en vervolgens opnieuw het systeem te implementeren.
Dit artikel wordt een voorbeeld van hoe dit proces in Powershell met een script: <http://www.westerndevs.com/azure-snapshots/>

Controleer of voor het installeren van een nieuwe SAP-licentie omdat een virtuele machine back-up herstellen als hierboven beschreven een nieuwe hardwaresleutel maakt.

### <a name="online-backup-of-an-sap-system"></a>Online back-up van een SAP-systeem
Back-up van de DBMS wordt uitgevoerd met DBMS-specifieke methoden, zoals beschreven in de [DBMS handleiding][dbms-guide].

Andere virtuele machines binnen het SAP-systeem kunnen back-up met functionaliteit voor Azure virtuele Machine back-up worden gemaakt. Azure virtuele Machine back-up is geïntroduceerd vroeg in 2015 en ondertussen is een standaardmethode voor back-up van een volledige virtuele machine in Azure. Azure Backup de back-ups opslaat in Azure en Hiermee kunt een herstel van een virtuele machine opnieuw.

> [!NOTE]
> Vanaf december 2015 met VM-back-up houdt geen unieke ID van de VM die wordt gebruikt voor SAP-licentieverlening. Dit betekent dat een herstelbewerking voor een VM-back-installatie van een nieuwe sleutel van de SAP-licentie vereist als de herstelde virtuele machine wordt beschouwd als een nieuwe virtuele machine en niet als vervanging van de voormalige die is opgeslagen.
>
> ![Windows][Logo_Windows] Windows
>
> Theorie, de virtuele machines die uitvoeren databases kunnen een back-up op een consistente manier ook als de DBMS-systeem de Windows VSS ondersteunt (Volume Shadow Copy Service <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>) Als bijvoorbeeld komt SQL Server.
> Let echter die op basis van de virtuele machine van Azure back-ups punt in tijd worden hersteld van de databases zijn niet mogelijk. Daarom is de aanbeveling om uit te voeren van back-ups van databases met DBMS-functionaliteit in plaats van te vertrouwen op Azure VM Backup.
>
> Om vertrouwd te raken met Azure virtuele Machine back-up start hier: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Andere mogelijkheden zijn een combinatie van Microsoft Data Protection Manager is geïnstalleerd in een virtuele machine van Azure en Azure Backup voor databases met back-up/herstel gebruiken. Meer informatie vindt u hier: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Er is geen equivalent voor de Windows VSS in Linux. Alleen bestandsconsistente back-ups zijn daarom mogelijk, maar geen toepassingsconsistente back-ups. De SAP DBMS back-up moet worden gedaan met behulp van de DBMS-functionaliteit. Het bestandssysteem, waaronder de SAP-gerelateerde gegevens kan worden opgeslagen, bijvoorbeeld met tar zoals beschreven hier: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure als DR-site voor productie SAP landschappen
Sinds Mid 2014 inschakelen verschillende onderdelen om Hyper-V, System Center en Azure-extensies het gebruik van Azure als DR-site voor VM's die lokaal op basis van Hyper-V wordt uitgevoerd.

Een blog met gedetailleerde informatie over het implementeren van deze oplossing Hier wordt beschreven: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Samenvatting
De belangrijkste punten van hoge beschikbaarheid voor SAP-systemen in Azure zijn:

* Op dit moment, kan niet de SAP storingspunt worden beveiligd exact dezelfde wijze als deze kan worden uitgevoerd in de on-premises implementaties. De reden is dat gedeelde schijf clusters nog in Azure kunnen niet worden gemaakt zonder het gebruik van 3e software van derden.
* Voor de DBMS-laag moet u DBMS functionaliteit gebruiken die niet afhankelijk van de gedeelde schijf cluster technologie. Details worden gedocumenteerd in de [DBMS handleiding][dbms-guide].
* Om te beperken van de impact van problemen binnen domeinen met fouten in de Azure-infrastructuur of host onderhoud, moet u Beschikbaarheidssets van Azure:
  * Het verdient aanbeveling om één Beschikbaarheidsset voor de toepassingslaag SAP.
  * Het wordt aanbevolen een afzonderlijke Beschikbaarheidsset voor de laag SAP DBMS.
  * Het is niet raadzaam om toe te passen van dezelfde Beschikbaarheidsset voor virtuele machines van verschillende SAP-systemen.
  * Het verdient aanbeveling gebruik van Premium-schijven worden beheerd.
* Voor de doeleinden van de back-up van de laag SAP DBMS, Controleer of de [DBMS handleiding][dbms-guide].
* Een back-up SAP dialoogvenster exemplaren zinvol weinig omdat deze meestal sneller te implementeren eenvoudig dialoogvenster exemplaren.
* Back-ups van de virtuele machine die de algemene map van het SAP-systeem en daarmee bevat de profielen van de verschillende exemplaren zinvol zijn en moet worden uitgevoerd met Windows back-up of bijvoorbeeld tar op Linux. Omdat er verschillen tussen Windows Server 2008 (R2) en Windows Server 2012 (R2), die het eenvoudiger back-up maken met de meest recente Windows Server worden vrijgegeven, is het raadzaam om uit te voeren van Windows Server 2012 (R2) als gastbesturingssysteem voor Windows.
