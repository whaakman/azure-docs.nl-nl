---
title: Azure virtuele Machines, planning en implementatie van SAP NetWeaver | Microsoft Docs
description: Azure virtuele Machines, planning en implementatie van SAP NetWeaver
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f8b7d2620b5b972c9476332f2b662a891a0b3765
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430337"
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure virtuele Machines, planning en implementatie van SAP NetWeaver

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
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-windows.md#command-line-options
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
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/template-samples.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/manage-virtual-network.md#create-a-virtual-network
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Microsoft Azure kunnen bedrijven aan te schaffen reken- en opslagresources in een minimale tijd zonder langdurige cycli. Azure Virtual Machine-service kan bedrijven klassieke toepassingen implementeren, zoals SAP NetWeaver-toepassingen in Azure en uitbreiden van hun betrouwbaarheid en beschikbaarheid zonder verdere resources on-premises beschikbaar. Azure Virtual Machine-Services biedt ook ondersteuning voor cross-premises-connectiviteit, waardoor bedrijven die actief Azure Virtual Machines integreren in hun on-premises domeinen, hun Privéclouds en hun SAP-landschap dat systeem.
Dit technische document beschrijft de basisprincipes van Microsoft Azure-Machine en biedt een overzicht van de planning en implementatie-informatie voor SAP NetWeaver-installaties in Azure en daarom moet het document te lezen voordat u begint met daadwerkelijke implementaties van SAP NetWeaver op Azure.
Het document is een aanvulling op de documentatie voor installatie van SAP en SAP-opmerkingen, die staan voor de primaire bronnen voor installaties en -implementaties van SAP-software op de opgegeven platforms.

## <a name="summary"></a>Samenvatting
Cloud Computing is een veelgebruikte term die wordt steeds vaker meer belang binnen de IT-sector voor kleine bedrijven tot grote en multinationale ondernemingen.

Microsoft Azure is het Cloudserviceplatform van Microsoft, biedt een breed scala aan nieuwe mogelijkheden. Klanten zijn nu kunnen snel inrichten en ongedaan maken inrichting toepassingen als een service in de cloud, zodat ze niet beperkt tot technische of financiële beperkingen zijn. In plaats van de tijd en geld investeren in hardware-infrastructuur, bedrijven zich kunnen richten op de toepassing, bedrijfsprocessen en de voordelen voor klanten en gebruikers.

Met Microsoft Azure-services voor virtuele machines biedt Microsoft een uitgebreid IaaS-platform (Infrastructure as a Service). SAP NetWeaver-toepassingen worden ondersteund op virtuele Azure-machines (IaaS). In dit technische document wordt beschreven hoe u plannen en implementeren van SAP NetWeaver gebaseerde toepassingen binnen Microsoft Azure-platform naar keuze.

Het document zelf richt zich op twee belangrijke aspecten:

* Het eerste deel hierin worden de twee ondersteunde implementaties voor SAP NetWeaver-toepassingen in Azure. Het beschrijft ook algemene verwerking van Azure met SAP-implementaties in gedachten.
* De tweede onderdeel details uitvoering van de twee verschillende scenario's beschreven in het eerste deel.

Zie voor aanvullende bronnen hoofdstuk [Resources] [ planning-guide-1.2] in dit document.

### <a name="definitions-upfront"></a>Definities kosten vooraf
In het hele document gebruiken we de volgende voorwaarden:

* IaaS: Infrastructure as a Service
* PaaS: Platform as a Service
* SaaS: Software as a Service
* SAP-onderdeel: een afzonderlijke SAP-toepassing, zoals ECC, BW, Manager van de oplossing of EP  SAP-onderdelen kunnen worden gebaseerd op de traditionele ABAP- of Java-technologieën of een toepassing NetWeaver op basis van zoals zakelijke objecten.
* SAP-omgeving: een of meer onderdelen van SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke-functie, zoals ontwikkeling, QAS, Training, herstel na Noodgevallen of productie.
* SAP-landschap: Deze term heeft betrekking op de gehele SAP-elementen in een klant IT landschap. De SAP-landschap bevat alle productie- en niet-productieomgevingen.
* SAP-systeem: De combinatie van DBMS-laag en niveau van de toepassing van bijvoorbeeld een ontwikkelsysteem SAP ERP, SAP BW-testsysteem, SAP CRM productiesysteem, enzovoort. In de Azure-implementaties, is het niet ondersteund voor het delen van deze twee lagen tussen on-premises en Azure. On-premises betekent dat die een SAP-systeem is geïmplementeerd of deze is geïmplementeerd in Azure. U kunt echter de verschillende systemen van SAP-landschap dat in Azure of on-premises implementeren. U kunt bijvoorbeeld implementeert de SAP CRM-ontwikkeling en testen van systemen in Azure, maar de SAP CRM productie system on-premises.
* Alleen-cloud-implementatie: een implementatie waarbij het Azure-abonnement niet is verbonden via een site-naar-site of een ExpressRoute-verbinding met de on-premises netwerkinfrastructuur. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als 'Cloud-Only'-implementaties. Virtuele Machines die worden geïmplementeerd met deze methode zijn toegankelijk via internet en een openbaar IP-adres en/of een openbare DNS-naam toegewezen aan de virtuele machines in Azure. Voor Microsoft Windows, de on-premises Active Directory (AD) en DNS is niet uitgebreid naar Azure in deze typen implementaties. De virtuele machines zijn daarom geen onderdeel van de on-premises Active Directory. Hetzelfde geldt voor Linux-implementaties met behulp van bijvoorbeeld OpenLDAP + Kerberos.

> [!NOTE]
> Alleen-cloud-implementatie in dit document wordt gedefinieerd als complete SAP-landschappen worden uitgevoerd in Azure zonder de extensie van Active Directory uitsluitend / OpenLDAP of naamomzetting van on-premises naar openbare cloud. Alleen-cloud-configuraties worden niet ondersteund voor productie SAP-systemen of configuraties waarbij SAP stm of andere on-premises bronnen worden gebruikt tussen die worden gehost op Azure en bronnen die zich moeten on-premises SAP-systemen.
>
>

* Cross-premises: Beschrijving van een scenario waarbij virtuele machines worden geïmplementeerd naar een Azure-abonnement dat site-naar-site, meerdere locaties of ExpressRoute-connectiviteit tussen de on-premises clientresources en Azure. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als cross-premises scenario's. De reden voor de verbinding is om de on-premises domeinen, on-premises Active Directory/OpenLDAP en on-premises DNS uitbreiden naar Azure. Het on-premises-landschap is uitgebreid naar de Azure-assets van het abonnement. Met deze extensie, kunnen de virtuele machines deel uitmaken van de on-premises domein. Gebruikers van een domein van de on-premises domein toegang tot de servers en services kunnen worden uitgevoerd op deze VM's (zoals DBMS-services). Communicatie en naamomzetting tussen de geïmplementeerde virtuele machines on-premises en Azure geïmplementeerde VM's is het mogelijk. Dit is de meest voorkomende en bijna exclusief case activa SAP implementeren in Azure. Zie voor meer informatie, [dit] [ vpn-gateway-cross-premises-options] artikel en [dit][vpn-gateway-site-to-site-create].

> [!NOTE]
> Cross-premises implementaties van SAP-systemen waarbij Azure Virtual Machines met SAP-systemen lid van een on-premises domein zijn worden ondersteund voor productie SAP-systemen. Cross-premises configuraties worden ondersteund voor het implementeren van onderdelen of uitvoeren van SAP-landschappen in Azure. De volledige SAP-landschap dat zelfs uitvoeren in Azure vereist dat deze virtuele machines die deel uit van de on-premises domein en ADVERTENTIES/OpenLDAP. In vorige versies van de documentatie, we hebben het gehad over hybride IT-scenario's waarin de term *hybride* verankerd ligt in het feit dat er een cross-premises-connectiviteit tussen on-premises en Azure. Bovendien is het feit dat de virtuele machines in Azure deel uit van de on-premises Active Directory maken / OpenLDAP.
>
>

Sommige Microsoft-documentatie worden cross-premises scenario's iets anders, met name voor DBMS HA configuraties beschreven. In het geval van de SAP-gerelateerde documenten, het cross-premises scenario alleen komt meestal neer op een site-naar-site of privé (ExpressRoute)-connectiviteit en het feit dat de SAP-landschap dat wordt gedistribueerd tussen on-premises en Azure in te stellen.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Resources
De volgende aanvullende handleidingen zijn beschikbaar voor het onderwerp van de SAP-oplossingen op Azure:

* [Azure virtuele Machines, planning en implementatie van SAP NetWeaver (in dit document)][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP NetWeaver][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]

> [!IMPORTANT]
> Waar mogelijk een koppeling naar de verwijzende SAP-installatiehandleiding wordt gebruikt (verwijzing InstGuide-01, Zie <http://service.sap.com/instguides>). Als het gaat om de vereisten en installatie, moet de SAP NetWeaver-installatiehandleidingen altijd worden gelezen zorgvuldig, zoals in dit document heeft alleen betrekking op specifieke taken voor SAP NetWeaver-systemen die zijn geïnstalleerd in een Microsoft Azure-Machine.
>
>

De volgende SAP-opmerkingen zijn gerelateerd aan het onderwerp van SAP op Azure:

| Houd er rekening mee getal | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen op Azure: ondersteunde producten en grootte |
| [2015553] |SAP op Microsoft Azure: vereisten voor ondersteuning |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [1409604] |Virtualisatie in Windows: uitgebreide bewaking |
| [2191498] |SAP op Linux met Azure: uitgebreide bewaking |
| [2243692] |Linux op Microsoft Azure (IaaS) virtuele machine: problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [2002167] |Red Hat Enterprise Linux 7.x: installatie en Upgrade |
| [2069760] |Oracle Linux 7.x SAP-installatie en Upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |

Lees ook de [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) die bevat alle SAP-opmerkingen voor Linux.

Algemene standaardbeperkingen en maximale beperkingen van Azure-abonnementen kunnen worden gevonden in [in dit artikel][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>Mogelijke scenario 's
SAP wordt vaak gebruikt als een van de meest essentiële toepassingen binnen ondernemingen. De architectuur en bewerkingen van deze toepassingen is voornamelijk complex en het is belangrijk ervoor te zorgen dat u voldoet aan de vereisten voor beschikbaarheid en prestaties.

Ondernemingen hebben dus om na te denken zorgvuldig over welke toepassingen kunnen worden uitgevoerd in een openbare cloudomgeving, onafhankelijk van de gekozen cloudprovider.

Mogelijke systeemtypes voor het implementeren van SAP NetWeaver-toepassingen in de openbare cloud omgevingen worden hieronder vermeld:

1. Middelgrote productiesystemen
2. Ontwikkeling van systemen
3. Testen van systemen
4. Prototypesystemen
5. Learning / demonstratie-systemen

Als u wilt implementeren SAP-systemen in Azure IaaS of IaaS in het algemeen, is het belangrijk om te begrijpen van de belangrijke verschillen tussen de aanbiedingen van traditionele uitbesteders of hosters en IaaS-producten. Terwijl de traditionele hoster of outsourcer past zich aan de infrastructuur (type netwerk, opslag en server) voor de werkbelasting die een klant wil hosten, maar het is in plaats daarvan de verantwoordelijkheid van de klant de juiste werkbelasting voor IaaS-implementaties te kiezen.

Klanten moeten als een eerste stap om te controleren of de volgende items:

* De SAP ondersteunde typen van de virtuele machine van Azure
* De SAP ondersteund-producten/versies op Azure
* De ondersteunde OS- en DBMS-versies voor de specifieke SAP-releases in Azure
* SAP's doorvoer is geleverd door verschillende Azure-SKU 's

De antwoorden op deze vragen kunnen worden gelezen in SAP-notitie [1928533].

Azure-resource en de bandbreedte beperkingen moeten worden vergeleken met het werkelijke resourceverbruik van on-premises systemen als tweede stap. Klanten moeten daarom om vertrouwd te raken met de verschillende mogelijkheden van de Azure die worden ondersteund met SAP op het gebied van:

* CPU en geheugen resources van verschillende VM-typen en
* IOPS-bandbreedte van verschillende VM-typen en
* Mogelijkheden van verschillende typen van de VM-netwerk.

De meeste van deze gegevens vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Houd er rekening mee dat de limieten die worden vermeld in de bovenstaande koppeling zijn bovenste limieten. Dit betekent niet dat de limieten voor het gebruik van de resources, kan bijvoorbeeld IOP's worden opgegeven onder alle omstandigheden. De uitzonderingen zijn echter de resources CPU en geheugen van een gekozen VM-type. Voor de virtuele machine die worden ondersteund door SAP, zijn de resources CPU en geheugen gereserveerd en als zodanig beschikbaar zijn op elk gewenst moment in de tijd voor gebruik binnen de virtuele machine.

De Microsoft Azure-platform, zoals andere IaaS-platformen is een multitenant-platform. Als gevolg hiervan worden opslag, netwerk en andere resources gedeeld tussen tenants. Intelligente beperken en quota logica wordt gebruikt om te voorkomen dat een tenant die invloed hebben op de prestaties van een andere tenant (ruis) op ingrijpende wijze. Hoewel logica in Azure probeert te houden van afwijkingen in de bandbreedte is vaak kleine, maximaal gedeelde platforms groter afwijkingen in de beschikbaarheid van de resource/bandbreedte dan veel klanten in hun on-premises implementaties worden gebruikt om te introduceren. Als gevolg hiervan kunt u ervaren verschillende niveaus van bandbreedte met betrekking tot het netwerk of opslag i/o (het volume, evenals latentie) minuut minuut. De kans dat een SAP-systeem op Azure grotere afwijkingen dan in een on-premises systeem optreden kan moet worden gehouden.

Een laatste stap is het evalueren van de vereisten voor beschikbaarheid. Dit kan gebeuren dat de onderliggende Azure-infrastructuur moet worden bijgewerkt en dat of de hosts met virtuele machines opnieuw worden opgestart. In dergelijke gevallen zou VM's op deze hosts worden uitgeschakeld en opnieuw opgestart ook. De timing van dergelijke onderhoud wordt uitgevoerd tijdens niet-core tijdens kantooruren voor een bepaalde regio, maar het mogelijke venster van een paar uur gedurende welke een wordt opnieuw opgestart relatief groot is. Er zijn verschillende technologieën in het Azure-platform die kunnen worden geconfigureerd om te beperken van sommige of alle van de impact van dergelijke updates. Toekomstige verbeteringen van de Azure-platform, DBMS-systemen en SAP toepassing zijn ontworpen voor het minimaliseren van de impact van dergelijke opnieuw wordt opgestart.

Als u wilt implementeren naar Azure een SAP-systeem, de on-premises SAP-systemen besturingssysteem, Database, en SAP-toepassingen moeten worden weergegeven op de ondersteuningsmatrix voor Azure voor SAP, past binnen de Azure-resources infrastructuur kunt bieden en die kunnen werken met de beschikbaarheid van SLA's Microsoft Azure-aanbiedingen. Als deze systemen worden geïdentificeerd, moet u besluiten op een van de volgende twee implementatiescenario's.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Cloud-Only - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk
![Één virtuele machine met SAP-demo's of trainingen scenario geïmplementeerd in Azure][planning-guide-figure-100]

In dit scenario is normaal voor trainingen of demo-systemen, waarop alle onderdelen van SAP en niet-SAP-software in een enkele virtuele machine zijn geïnstalleerd. Productie SAP-systemen worden niet ondersteund in dit implementatiescenario. In dit scenario wordt in het algemeen de volgende vereisten voldoet:

* De VM's zelf zijn toegankelijk via het openbare netwerk. Rechtstreekse netwerkverbinding voor de toepassingen die binnen de virtuele machines worden uitgevoerd op de on-premises netwerk van een van beide het bedrijf die eigenaar is van de demo's of trainingen inhoud of de klant is niet nodig.
* In het geval van meerdere virtuele machines die de trainingen of een demo-scenario, moet netwerk communicatie en naamomzetting werkt tussen de virtuele machines. Maar communicatie tussen de set van virtuele machines moeten worden geïsoleerd, zodat verschillende sets virtuele machines kunnen naast elkaar worden geïmplementeerd zonder tussenkomst.  
* Verbinding met Internet is vereist voor de eindgebruiker moet externe Meld u aan bij de VM's die worden gehost in Azure. Afhankelijk van de Gast wordt OS, Terminal Services/extern bureaublad-services of VNC/ssh gebruikt voor toegang tot de virtuele machine om te voldoen aan de training-taken of uitvoeren van de demo's. Als SAP zoals 3200, 3300 & 3600 kan poorten ook worden blootgesteld exemplaar van de SAP-toepassing kan worden benaderd vanaf elke Internet verbonden desktop.
* De SAP-systemen (en VM(s)) vertegenwoordigen een scenario voor zelfstandige in Azure, die alleen openbare internetverbinding vereist voor toegang van eindgebruikers en vereist geen een verbinding met andere virtuele machines in Azure.
* SAPGUI en een browser worden geïnstalleerd en uitgevoerd rechtstreeks op de virtuele machine.
* Snel opnieuw instellen van een virtuele machine naar de oorspronkelijke status en de nieuwe implementatie van deze oorspronkelijke status opnieuw is vereist.
* In het geval van demo's en scenario's voor training, dat zijn gerealiseerde in meerdere virtuele machines, een Active Directory / OpenLDAP en/of de DNS-service is vereist voor elke set van virtuele machines.

![Groep van de virtuele machine voor een demonstratie of scenario training in een Azure Cloud Service][planning-guide-figure-200]

Het is belangrijk dat u er rekening mee dat de VM (s) in elk van de sets worden geïmplementeerd in parallelle moeten, waar de namen van de virtuele machine in elk van de set hetzelfde zijn.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Cross-Premises - implementatie van één of meerdere SAP-virtuele machines naar Azure met de vereiste van volledig geïntegreerd in de on-premises netwerk
![VPN met Site-naar-Site-connectiviteit (cross-premises)][planning-guide-figure-300]

Dit scenario is een cross-premises scenario met vele mogelijke implementaties. Dit kan worden aangeduid met het uitvoeren van sommige onderdelen van de SAP-landschap on-premises en andere onderdelen van de SAP-landschap dat op Azure. Alle aspecten van het feit dat onderdeel van de SAP-onderdelen worden uitgevoerd op Azure moeten worden transparant voor eindgebruikers. Daarom werken de SAP Transport correctie System (STM), RFC communicatie, afdrukken, beveiliging (zoals SSO), enzovoort naadloos voor de SAP-systemen die worden uitgevoerd op Azure. Maar de cross-premises-scenario wordt ook beschreven voor een scenario waarin de complete SAP-landschap dat wordt uitgevoerd in Azure met het domein van de klant en DNS uitgebreid naar Azure.

> [!NOTE]
> Dit is het implementatiescenario dat wordt ondersteund voor het uitvoeren van productief SAP-systemen.
>
>

Lezen [in dit artikel] [ vpn-gateway-create-site-to-site-rm-powershell] voor meer informatie over hoe u uw on-premises netwerk verbinden met Microsoft Azure

> [!IMPORTANT]
> Wanneer we over cross-premises scenario's tussen Azure en on-premises implementaties van klanten praten, zijn we de granulatie van de gehele SAP-systemen ziet. Scenario's die *niet ondersteund* voor cross-premises scenario's:
>
> * Verschillende lagen van SAP-toepassingen in verschillende implementatiemethoden uitgevoerd. De DBMS-laag on-premises, maar de SAP-toepassingslaag bijvoorbeeld uitgevoerd in virtuele machines die worden geïmplementeerd als virtuele Azure-machines of vice versa.
> * Sommige onderdelen van een SAP-laag in Azure en bepaalde on-premises. Bijvoorbeeld splitsen exemplaren van de SAP-toepassingslaag tussen on-premises en Azure-VM's.
> * Distributie van virtuele machines met SAP-instanties van een systeem over meerdere Azure-regio's wordt niet ondersteund.
>
> De reden voor deze beperkingen is de vereiste voor een netwerk van de hoge prestaties met lage latentie binnen één SAP-systeem, met name tussen de exemplaren van de toepassing en de DBMS-laag van een SAP-systeem.
>
>

### <a name="supported-os-and-database-releases"></a>Ondersteund besturingssysteem en versies van de Database
* Microsoft-serversoftware ondersteund voor Azure-Services voor virtuele Machine wordt vermeld in dit artikel: <http://support.microsoft.com/kb/2721672>.
* Ondersteund besturingssysteem system releases, database system releases in combinatie met SAP-software wordt ondersteund door Azure Virtual Machine-Services worden beschreven in de SAP-notitie [1928533].
* SAP-toepassingen en versies die worden ondersteund op Azure Virtual Machine-Services worden beschreven in de SAP-notitie [1928533].
* Alleen 64-bits-afbeeldingen worden ondersteund voor het uitvoeren als gast-VM's in Azure voor SAP-scenario's. Als gevolg hiervan, worden alleen 64-bits SAP-toepassingen en databases ondersteund.

## <a name="microsoft-azure-virtual-machine-services"></a>Microsoft Azure-Services voor virtuele Machine
De Microsoft Azure-platform is een platform internetschaal cloud services die worden gehost en beheerd in Microsoft-datacenters. Het platform bevat de Microsoft Azure Virtual Machine-Services (infrastructuur als een Service of IaaS) en een set uitgebreid Platform als een Service (PaaS)-mogelijkheden.

Het Azure-platform vermindert de noodzaak van vooraf-technologie en infrastructuur-aankopen. Het vereenvoudigt onderhouden en toepassingen werken door te geven van on-demand rekenkracht en opslag te hosten, schalen en beheren van web-App en verbonden toepassingen. Beheer van infrastructuur is geautomatiseerd met een platform dat is ontworpen voor hoge beschikbaarheid en dynamisch schalen zodat deze overeenkomt met de behoeften voor gebruik met de optie van een prijsmodel voor betalen per gebruik.

![Plaatsing van Microsoft Azure-Services voor virtuele Machine][planning-guide-figure-400]

Met Azure Virtual Machine-Services zodat Microsoft u kunt aangepaste server-installatiekopieën implementeren in Azure als IaaS-instanties (Zie afbeelding 4). De virtuele Machines in Azure zijn gebaseerd op Hyper-V virtuele harde schijven (VHD) en kunnen verschillende besturingssystemen worden uitgevoerd als Gastbesturingssysteem.

Vanuit een operationele perspectief biedt de Azure-Service voor virtuele Machine vergelijkbare ervaringen als virtuele machines die on-premises worden geïmplementeerd. Dit heeft echter het grote voordeel dat u niet wilt aanschaffen, beheren en beheren van de infrastructuur. Ontwikkelaars en beheerders hebben volledig beheer van de installatiekopie van besturingssysteem in deze virtuele machines. Beheerders kunnen zich aanmelden op afstand op die virtuele machines om uit te voeren voor onderhoud en probleemoplossing van taken, evenals de taken van software-implementatie. De enige beperkingen zijn voor de implementatie van de grootte en de mogelijkheden van Azure-VM's. Met deze grootten zijn mogelijk niet zo goed gedetailleerde in configuratie, zoals het on-premises kan worden uitgevoerd. Er is een keuze van VM-typen die een combinatie van vertegenwoordigen:

* Het aantal vcpu 's
* Geheugen
* Aantal virtuele harde schijven die kunnen worden gekoppeld
* Netwerk- en bandbreedten

De grootte en de beperkingen van verschillende grootten voor verschillende virtuele machines die worden aangeboden in een tabel in de weergegeven [in dit artikel (Linux)] [ virtual-machines-sizes-linux] en [in dit artikel (Windows)] [virtual-machines-sizes-windows].

Niet alle andere VM-reeks wel wordt aangeboden in elk van de Azure-regio's (voor Azure-regio's Zie volgende hoofdstuk). Bedenk ook dat niet alle virtuele machines of virtuele machine-serie zijn gecertificeerd voor SAP.

> [!IMPORTANT]
> Voor het gebruik van SAP NetWeaver-toepassingen, alleen de subset met VM-typen en configuraties die worden vermeld in de SAP-notitie [1928533] worden ondersteund.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-regio 's
Virtuele Machines worden geïmplementeerd in zogenaamde *Azure-regio's*. Een Azure-regio is mogelijk een of meer datacenters die zich in de nabijheid bevinden. Voor het merendeel van de geopolitieke regio's in de hele wereld heeft Microsoft ten minste twee Azure-regio's. Bijvoorbeeld in Europa er is een Azure-regio van *Noord-Europa* en een van de *West-Europa*. Deze twee Azure-regio's binnen een geopolitieke regio worden gescheiden door aanzienlijke genoeg afstand zodat natuurlijke of technische rampen hebben geen invloed op zowel Azure-regio's in dezelfde geopolitieke regio. Omdat Microsoft is gestaag het bouwen van nieuwe Azure-regio's in verschillende geopolitieke regio's wereldwijd, wordt het nummer van deze regio's is gestaag groeiende en het aantal 20 Azure-regio's met meer regio's aangekondigd al bereikt vanaf december 2015. SAP-systemen kunt u als een klant implementeren in alle deze gebieden, met inbegrip van de twee Azure-regio's in China. Zie deze website voor de huidige actuele informatie over Azure-regio's: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Het Concept van de virtuele Machine Microsoft Azure
Microsoft Azure biedt een infrastructuur als een Service (IaaS)-oplossing naar host virtuele Machines met vergelijkbare functionaliteit als een on-premises-oplossing voor netwerkvirtualisatie. U bent kunt maken van virtuele Machines in Azure portal, PowerShell of CLI, die ook beheermogelijkheden en de implementatie bieden.

Met Azure Resource Manager kunt u uw toepassingen inrichten aan de hand van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U kunt dezelfde sjabloon gebruiken om uw toepassing tijdens elke fase van de levenscyclus van de toepassing herhaaldelijk te implementeren.

Meer informatie over het gebruik van Resource Manager-sjablonen vindt u hier:

* [Implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en Azure CLI](../../linux/create-ssh-secured-vm-from-template.md)
* [Virtuele machines beheren met Azure Resource Manager en PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://azure.microsoft.com/documentation/templates/>

Een andere interessante functie is de mogelijkheid om installatiekopieën te maken van virtuele Machines, zodat u kunt het voorbereiden van bepaalde opslagplaatsen van waaruit u meer exemplaren van de virtuele machine, die voldoen aan uw vereisten snel implementeren zijn.

Meer informatie over het maken van installatiekopieën van virtuele Machines kan worden gevonden [in dit artikel (Linux)] [ virtual-machines-linux-capture-image-resource-manager] en [in dit artikel (Windows)] [ virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Domeinen met fouten
Domeinen met fouten vertegenwoordigen een fysieke eenheid van de fout, nauw verwant aan de fysieke infrastructuur die is opgenomen in datacenters, en een fysieke blade of -rack kan worden beschouwd als een Foutdomein, maar er is geen rechtstreekse-op-een koppeling tussen de twee.

Wanneer u meerdere virtuele Machines als onderdeel van een SAP-systeem in Microsoft Azure-Services voor virtuele Machine implementeert, kunt u het implementeren van uw toepassing in verschillende domeinen met fouten, waardoor voldoen aan de vereisten van de Azure-Infrastructuurcontroller bepalen de Microsoft Azure SLA. De distributie van domeinen met fouten over een Azure-schaaleenheid (verzameling van honderden rekenknooppunten of Storage-knooppunten en netwerken) of de toewijzing van VM's naar een specifieke Foutdomein is echter iets waarover u geen directe controle. Om te kunnen direct de Azure-infrastructuurcontroller een set van virtuele machines via verschillende Foutdomeinen te implementeren, moet u een Azure-Beschikbaarheidsset toewijzen aan de virtuele machines tijdens de implementatie. Zie voor meer informatie over Beschikbaarheidssets van Azure, hoofdstuk [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] in dit document.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Upgrade-domeinen
Upgradedomeinen vertegenwoordigen een logische eenheid die helpt bij het bepalen hoe een virtuele machine binnen een SAP-systeem, die uit de SAP-exemplaren die in meerdere virtuele machines worden uitgevoerd bestaat, wordt bijgewerkt. Wanneer er een upgrade optreedt, gaat u met Microsoft Azure doorloopt het proces voor het bijwerken van deze Upgradedomeinen één voor één. Door te spreiden VM's tijdens de implementatie via verschillende domeinen met een upgrade uitvoert, kunt u uw SAP-systeem beveiligen gedeeltelijk van potentiële downtime. Om af te dwingen Azure voor het implementeren van de virtuele machines van een SAP-systeem verdeeld over verschillende domeinen upgraden, moet u een specifiek kenmerk ingesteld tijdens de implementatie van elke virtuele machine. Net als bij domeinen met fouten, een Azure-schaaleenheid is onderverdeeld in meerdere domeinen upgraden. Om te kunnen direct de Azure-infrastructuurcontroller een set van virtuele machines implementeren via verschillende domeinen upgraden, moet u een Azure-Beschikbaarheidsset toewijzen aan de virtuele machines tijdens de implementatie. Zie voor meer informatie over Beschikbaarheidssets van Azure, hoofdstuk [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] hieronder.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Azure-Beschikbaarheidssets
Azure virtuele Machines binnen een Azure-Beschikbaarheidsset worden door de Azure-Infrastructuurcontroller verdeeld over verschillende Foutdomeinen en Upgrade-domeinen. Het doel van de verdeling over verschillende Foutdomeinen en Upgrade-domeinen is om te voorkomen dat alle virtuele machines van een SAP-systeem in het geval van onderhoud aan de infrastructuur of een storing in één Foutdomein wordt afgesloten. VM's zijn standaard niet deel uit van een Beschikbaarheidsset. De deelname van een virtuele machine in een Beschikbaarheidsset is gedefinieerd tijdens de implementatie of hoger op door een nieuwe configuratie en opnieuw implementeren van een virtuele machine.

Lees voor meer informatie over het concept van Beschikbaarheidssets van Azure en de wijze waarop Beschikbaarheidssets hebben betrekking op fouten en Upgrade-domeinen, [in dit artikel][virtual-machines-manage-availability]

Zie voor het definiëren van beschikbaarheidssets voor Azure Resource Manager via een json-sjabloon [de rest-api-specificaties](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) en zoek naar 'beschikbaarheid'.

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Opslag: De Microsoft Azure Storage en de gegevensschijven
Microsoft Azure Virtual Machines gebruikmaken van verschillende opslagtypen. Bij het implementeren van SAP op Azure Virtual Machine-Services, is het belangrijk te weten wat de verschillen tussen deze twee soorten opslag:

* Niet-permanente, vluchtige opslag.
* Permanente opslag.

Azure-VM's bieden een niet-permanente schijven na de implementatie van een virtuele machine. In het geval van een VM opnieuw wordt opgestart, wordt alle inhoud op deze schijven worden gewist. Het is dus een gezien het feit dat de gegevensbestanden en log/opnieuw bestanden van databases onder geen enkele omstandigheid zich op deze schijven niet-persistente bevinden moeten. Mogelijk zijn er uitzonderingen voor enkele van de databases, waar deze stations niet-persistente mogelijk geschikt voor tempdb en tijdelijke tabelruimten. Echter Vermijd het gebruik van deze schijven voor virtuele machines uit de A-serie, omdat deze niet-persistente schijven zijn beperkt in doorvoer met deze VM-reeks. Lees voor meer informatie het artikel [inzicht krijgen in het tijdelijke station op Windows-VM's in Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

- - -
> ![Windows][Logo_Windows] Windows
> 
> Station D:\ in een Azure VM is een niet-persistente-station, die wordt ondersteund door sommige lokale schijven op de Azure compute-knooppunt. Omdat het niet-persistente, betekent dit dat alle wijzigingen aan de inhoud op het station D:\ gaat verloren wanneer de virtuele machine opnieuw wordt opgestart. "De wijzigingen', zoals bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure-VM's automatisch een station koppelen aan /mnt/resource die is een niet-persistente station ondersteund door lokale schijven op de Azure compute-knooppunt. Omdat het niet-persistente, betekent dit dat alle wijzigingen aan de inhoud in /mnt/resource gaan verloren wanneer de virtuele machine opnieuw wordt opgestart. Door wijzigingen, zoals bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> 

- - -

Microsoft Azure Storage biedt permanente opslag en de normale niveaus van beveiliging en redundantie gezien op SAN-opslag. Op basis van Azure Storage-schijven zijn virtuele harde schijf (VHD's) zich in de Azure Storage-Services. De lokale OS-schijf (C: Windows\, Linux/dev/sda1) is opgeslagen op de Azure-opslag, en extra Volumes/schijven die zijn gekoppeld aan de virtuele machine ophalen die zijn opgeslagen, te.

Het is mogelijk om te uploaden van een bestaande VHD van on-premises of lege mappen uit in Azure maken en koppelen van de geïmplementeerde VM's.

Na het maken of een VHD uploaden naar Azure Storage, is het mogelijk om te koppelen en koppel deze aan een bestaande virtuele Machine en om te kopiëren van de bestaande (ontkoppeld) VHD.

Als deze VHD's zijn opgeslagen, zijn gegevens en wijzigingen in die veilig als opnieuw opstarten en opnieuw maken van een VM-exemplaar. Zelfs als een exemplaar is verwijderd, deze VHD's veilig en kunnen worden geïmplementeerd of in het geval van niet-besturingssysteemschijven kunnen worden gekoppeld aan andere virtuele machines.

Meer informatie over Azure Storage vindt u hier:

* <https://azure.microsoft.com/documentation/services/storage/>
* <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>
* <https://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview.aspx>

#### <a name="azure-standard-storage"></a>Azure Standard Storage
Azure Standard-opslag is het type opslag beschikbaar bij Azure IaaS is uitgebracht. Er zijn IOPS quota afgedwongen per één schijf. Latentie die wordt ervaren heeft niet dezelfde klasse als SAN/NAS-apparaten doorgaans geïmplementeerd voor geavanceerde SAP-systemen on-premises gehost. Evenwel het Azure Standard-opslag in de praktijk bleek voldoende is voor veel honderden SAP-systemen in de tussentijd zorgen geïmplementeerd in Azure.

Schijven die zijn opgeslagen op Azure Storage-Accounts worden in rekening gebracht op basis van de werkelijke hoeveelheid gegevens die zijn opgeslagen, het volume van de opslagtransacties en uitgaande gegevensoverdracht gekozen optie voor redundantie. Veel schijven kunnen worden gemaakt op de maximaal 1TB groot, maar als deze leeg blijven er zijn geen kosten verbonden. Als u een VHD met 100GB elk vervolgens invult, betaalt u voor het opslaan van 100GB en niet voor de nominale grootte van die de VHD is gemaakt met.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Azure Premium Storage
Azure Premium Storage is geïntroduceerd in het doel om te bieden:

* Betere i/o-latentie.
* Betere doorvoer.
* Minder variabiliteit bij i/o-latentie.

Voor dit doel, zijn veel wijzigingen van de twee belangrijkste zijn geïntroduceerd:

* Gebruik van SSD-schijven in de Azure Storage-knooppunten
* Een nieuwe lezen cache die wordt ondersteund door de lokale SSD van een Azure compute-knooppunt

In in de Standard-opslag waar mogelijkheden niet hebt gewijzigd afhankelijk van de grootte van de schijf (of VHD) Premium-opslag heeft momenteel drie categorieën voor andere schijf, in dit artikel worden weergegeven: <https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/>

U ziet dat de schijf- en IOPS-schijf/doorvoer/schijf zijn afhankelijk van de categorie van de grootte van de schijven

Kosten uit te voeren in het geval van Premium-opslag is niet het aantal daadwerkelijke gegevens die zijn opgeslagen in deze schijven, maar de categorie van de grootte van dergelijke een schijf, onafhankelijk van de hoeveelheid gegevens die zijn opgeslagen in de schijf.

U kunt ook schijven maken op Premium-opslag die niet rechtstreeks wilt toewijzen in de grootte van categorieën die worden weergegeven. Kan dit het geval is, met name bij het kopiëren van schijven van standaardopslag naar Premium Storage. In dergelijke gevallen wordt een toewijzing aan de volgende grootste Premium opslagschijfoptie uitgevoerd.

De meeste van de Azure-VM-families gecertificeerd met SAP kunnen werken met Premium Storage en/of een combinatie tussen Azure standard en Premium-opslag.

Als u het gedeelte van de DS-serie VM's in zijn uitgecheckt [in dit artikel (Linux)] [ virtual-machines-sizes-linux] en [in dit artikel (Windows)][virtual-machines-sizes-windows], denk eraan dat Er zijn gegevens volume beperkingen voor Premium Storage-schijven op de granulatie van het niveau van de virtuele machine. Andere DS-reeks of GS-serie VM's hebt ook andere beperkingen met betrekking tot het aantal gegevensschijven die kunnen worden gekoppeld. Deze limieten zijn gedocumenteerd in het artikel ook hierboven genoemde. Maar in feite betekent dit dat u, bijvoorbeeld: 32 x P30 schijven aan een enkele DS14-virtuele machine koppelen kunt u 32 x de maximale doorvoer van een P30-schijf niet ophalen. De maximale doorvoer op VM-niveau, zoals beschreven in het artikel beperkt in plaats daarvan de doorvoer van gegevens.

Meer informatie over Premium Storage vindt u hier: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure Storage Accounts

Bij het implementeren van services of virtuele machines in Azure, kan de implementatie van VHD's en VM-installatiekopieën worden georganiseerd in eenheden die Azure Storage-Accounts genoemd. Wanneer u een Azure-implementatie plant, moet u zorgvuldig rekening houden met de beperkingen van Azure. Op de een-zijde is er een beperkt aantal Storage-Accounts per Azure-abonnement. Hoewel elke Azure Storage-Account een groot aantal VHD-bestanden bevatten kan, moet u er een vaste limiet is op de totale IOP's per Opslagaccount. Bij het implementeren van honderden VM's SAP DBMS-systemen aanzienlijke i/o-aanroepen maakt, is het aanbevolen voor het distribueren van hoge IOPS DBMS-VM's tussen meerdere Azure-Opslagaccounts. Wees voorzichtig niet aan de huidige limiet van Azure Storage-Accounts per abonnement. Omdat opslag een essentieel onderdeel van de implementatie van de database voor een SAP-systeem, dit concept wordt besproken in de al waarnaar wordt verwezen in meer detail [DBMS-Implementatiehandleiding][dbms-guide].

Meer informatie over Azure Storage-Accounts kan worden gevonden [in dit artikel][storage-scalability-targets]. Dit artikel leest, ontdekt u dat er verschillen in de beperkingen tussen Azure Storage-Accounts en Premium Storage-Accounts zijn. Belangrijke verschillen, zijn de hoeveelheid gegevens die kunnen worden opgeslagen in een Opslagaccount. Het volume is in de Standard-opslag een grootte die groter zijn dan met Premium Storage. Aan de andere kant, het standaard Opslagaccount ernstige problemen is beperkt in IOPS (Zie kolom **totale snelheid van aanvragen voor**), terwijl de Azure Premium Storage-Account geen dergelijke beperking heeft. Wanneer u de implementaties van SAP-systemen, met name de DBMS-servers worden details en de resultaten van deze verschillen besproken.

U hebt de mogelijkheid om verschillende containers voor het categoriseren van verschillende VHD's te organiseren en te maken binnen een Opslagaccount. Deze containers worden gebruikt om, bijvoorbeeld: afzonderlijke VHD's van verschillende virtuele machines. Er zijn geen gevolgen voor de prestaties bij het gebruik van slechts één container of meerdere containers onder een enkele Azure Storage-Account.

In Azure volgt de naam van een VHD in de volgende naamgevingsconventie verbinding die u moet een unieke naam voor de VHD in Azure bieden:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

De bovenstaande tekenreeks moet voor het aanduiden van de VHD die is opgeslagen in Azure Storage.

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Beheerde schijven

Managed Disks is een nieuwe brontype in Azure Resource Manager die kunnen worden gebruikt in plaats van VHD's die zijn opgeslagen in Azure Storage-Accounts. Beheerde schijven automatisch uitgelijnd met de Beschikbaarheidsset van de virtuele machine die zijn gekoppeld aan en dus de beschikbaarheid van uw virtuele machine en de services die worden uitgevoerd op de virtuele machine te verhogen. Lees voor meer informatie de [overzichtsartikel](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Wij aanbevelen voor het gebruik van beheerde schijf, omdat ze de implementatie en beheer van uw virtuele machines vereenvoudigt.
SAP ondersteunt momenteel alleen Premium Managed Disks. Lees voor meer informatie, SAP-notitie [1928533].

#### <a name="microsoft-azure-storage-resiliency"></a>Microsoft Azure Storage tolerantie

Microsoft Azure Storage slaat de basis-VHD (met OS) en de gekoppelde schijven of de BLOBs op ten minste drie afzonderlijke opslagknooppunten. Dit wordt lokaal redundante opslag (LRS) genoemd. LRS is de standaardwaarde voor alle typen opslag in Azure. 

Er zijn verschillende meer redundantie methoden, die alle beschreven in het artikel worden [Azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Vanaf Azure Premium Storage, het aanbevolen type opslag voor DBMS-VM's en schijven die de database en logboekbestanden/opnieuw bestanden worden opgeslagen is, is de enige beschikbare methode LRS. Als gevolg hiervan moet u database-methoden, zoals SQL Server Always On, Oracle Data Guard of HANA System Replication om in te schakelen van databasereplicatie voor gegevens in een andere Azure-regio of een andere Azure-Beschikbaarheidszone configureren.


> [!NOTE]
> Het gebruik van geografisch redundante opslag met Azure Standard-opslag beschikbaar is voor DBMS-implementaties, niet aanbevolen omdat deze is van invloed op de ernstige prestaties en biedt niet voldoen aan de order schrijven in verschillende virtuele harde schijven die zijn gekoppeld aan een virtuele machine. Het feit dat de volgorde schrijven niet naleven via verschillende VHD's draagt een grote kans om te beëindigen in inconsistente databases aan de replicatie als de database en logboekbestanden/opnieuw worden verdeeld over meerdere VHD's (zoals meestal het geval) op de bron-VM aan clientzijde.


### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Microsoft Azure-netwerken

Microsoft Azure biedt een netwerkinfrastructuur voor, waardoor de toewijzing van alle scenario's, die we willen realiseren met SAP-software. De mogelijkheden zijn:

* Toegang van buiten en rechtstreeks naar de virtuele machines via Windows Terminal Services of ssh/VNC
* Toegang tot services en specifieke poorten die worden gebruikt door toepassingen binnen de virtuele machines
* Interne communicatie en naamomzetting tussen een groep virtuele machines die zijn geïmplementeerd als virtuele Azure-machines
* Cross-premises-connectiviteit tussen de klant on-premises netwerk en het Azure-netwerk
* Over Azure-regio of data center connectiviteit tussen Azure websites

Meer informatie vindt u hier: <https://azure.microsoft.com/documentation/services/virtual-network/>

Er zijn veel verschillende mogelijkheden voor het configureren van de naam en IP-oplossing in Azure. In dit document, is alleen-Cloud-scenario's zijn afhankelijk van de standaardwaarde van het gebruik van Azure DNS (in tegenstelling tot het definiëren van een eigen DNS-service). Er is ook een nieuwe Azure DNS-service, die kan worden gebruikt in plaats van het instellen van uw eigen DNS-server. Meer informatie vindt u [in dit artikel] [ virtual-networks-manage-dns-in-vnet] en klik op [deze pagina](https://azure.microsoft.com/services/dns/).

Voor cross-premises scenario's, zijn we vertrouwen op het feit dat de on-premises naar Azure AD/OpenLDAP/DNS is uitgebreid via VPN of particuliere verbinding. Voor bepaalde scenario's zoals hier wordt beschreven, kan het nodig zijn om de replica van een AD/OpenLDAP geïnstalleerd in Azure.

Omdat de toegang en naamomzetting is een essentieel onderdeel van de implementatie van de database voor een SAP-systeem, dit concept wordt besproken in meer detail in de [DBMS-Implementatiehandleiding][dbms-guide].

##### <a name="azure-virtual-networks"></a>Virtuele netwerken van Azure

Het opbouwen van een Azure-netwerk, kunt u het adresbereik van het particuliere IP-adressen toegewezen door de Azure DHCP-functionaliteit. In de cross-premises scenario's, is het IP-adresbereik gedefinieerd nog steeds toegewezen met behulp van DHCP door Azure. Echter Domain Name resolution on-premises (ervan uitgaande dat de VM's deel uitmaken van een on-premises domein) wordt uitgevoerd en kan daarom adressen buiten verschillende Azure-Cloudservices kunt oplossen.

Elke virtuele Machine in Azure moet worden verbonden met een Virtueelnetwerk.

Meer informatie vindt u [in dit artikel] [ resource-groups-networking] en klik op [deze pagina](https://azure.microsoft.com/documentation/services/virtual-network/).


> [!NOTE]
> Wanneer een virtuele machine is geïmplementeerd wijzigen niet u standaard de configuratie van het Virtueelnetwerk. De TCP/IP-instellingen moeten worden links naar de Azure DHCP-server. Standaardgedrag is dynamische IP-Adressen toewijzen.
>
>

Het MAC-adres van de kaart van het virtuele netwerk kan wijzigen, bijvoorbeeld na het formaat en de Windows- of Linux Gast OS neemt de nieuwe netwerkkaart en automatisch DHCP wordt gebruikt in dit geval de IP-adres en DNS-adressen toewijzen.

##### <a name="static-ip-assignment"></a>Toewijzing van statische IP-adres
Het is mogelijk opgelost of gereserveerde IP-adressen toewijzen aan virtuele machines binnen een virtueel Azure-netwerk. De virtuele machines uitvoeren in een Azure Virtual Network openen een uitstekende mogelijkheid om deze functionaliteit gebruik als nodig zijn of vereist zijn voor sommige scenario's te De IP-adrestoewijzing blijft geldig in de gehele sprake is van de VM, ongeacht of de virtuele machine actief of afsluiten. Als gevolg hiervan moet u het totale aantal virtuele machines (actieve en gestopte VM's) in rekening te houden bij het definiëren van het bereik van IP-adressen voor het Virtueelnetwerk. Het IP-adres blijft toegewezen totdat de virtuele machine en de netwerkinterface wordt verwijderd of totdat het IP-adres krijgt ongedaan maken opnieuw toegewezen. Lees voor meer informatie, [in dit artikel][virtual-networks-static-private-ip-arm-pportal].

> [!NOTE]
> Moet u statische IP-adressen via Azure middelen toewijzen aan afzonderlijke vnic's. U moet statische IP-adressen binnen het gastbesturingssysteem niet toewijzen aan een vNIC. Sommige Azure-services zoals Azure Backup-Service is afhankelijk van het feit dat op minimaal de primaire vNIC is ingesteld op DHCP- en niet op de vaste IP-adressen. Zie ook het document [los problemen met Azure virtuele machine back-up](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking).
>
>

##### <a name="multiple-nics-per-vm"></a>Meerdere NIC's per VM

U kunt meerdere virtuele netwerkinterfacekaarten (vNIC) definiëren voor een virtuele Machine van Azure. Met de mogelijkheid om meerdere vnic's die u beginnen kunt met het instellen van het netwerkverkeer worden gescheiden, bijvoorbeeld clientverkeer wordt doorgestuurd via één vNIC en back-end-verkeer wordt doorgestuurd via een tweede vNIC. Afhankelijk van het type van de virtuele machine er zijn andere beperkingen met betrekking tot het aantal vnic's. Meer informatie over, functionaliteit en beperkingen kunnen vindt u in deze artikelen:

* [Een Windows-VM maken met meerdere NIC 's][virtual-networks-multiple-nics-windows]
* [Een Linux-VM maken met meerdere NIC 's][virtual-networks-multiple-nics-linux]
* [Meerdere NIC VM's met behulp van een sjabloon implementeren][virtual-network-deploy-multinic-arm-template]
* [Meerdere NIC VM's implementeren met PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Meerdere NIC VM's implementeren met de Azure CLI][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Site-naar-Site-connectiviteit

Cross-premises is Azure VM's en On-Premises die is gekoppeld met een transparante en permanente VPN-verbinding. Naar verwachting worden de meest voorkomende patroon van SAP-implementatie in Azure. De veronderstelling is operationele procedures en processen met SAP-instanties in Azure transparant moeten werken. Dit betekent dat die u kunt afdrukken buiten deze systemen moet worden, evenals de SAP Transport Management System (TMS) voor het transporteren wijzigt van een systeem voor de ontwikkeling in Azure in een testsysteem, die is gebruikt on-premises geïmplementeerd. Meer documentatie over site-naar-site kunt u vinden in [in dit artikel][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-Tunnel-apparaat

Als u wilt maken van een site-naar-site-verbinding (on-premises Datacenter naar Azure-Datacenter), die u wilt downloaden en configureren van een VPN-apparaat of Routing and Remote Access Service (RRAS) dat werd geïntroduceerd als een onderdeel van de software met Windows Server 2012 gebruiken.

* [Een virtueel netwerk maken met een site-naar-site VPN-verbinding met behulp van PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [Over VPN-apparaten voor Site-naar-Site VPN-gatewayverbindingen][vpn-gateway-about-vpn-devices]
* [Veelgestelde vragen over VPN-Gateway][vpn-gateway-vpn-faq]

![Site-naar-site-verbinding tussen on-premises en Azure][planning-guide-figure-600]

De bovenstaande afbeelding ziet u twee Azure-abonnementen zijn gereserveerd voor gebruik van IP-adres subbereiken in virtuele netwerken in Azure. De verbinding van de on-premises netwerk met Azure tot stand is gebracht via VPN.

#### <a name="point-to-site-vpn"></a>Punt-naar-Site-VPN

Punt-naar-site VPN moet elke client-computer om te verbinden met een eigen VPN naar Azure. We kijken, punt-naar-site-connectiviteit is het niet praktisch voor de SAP-scenario's. Daarom worden er geen verdere verwijzingen krijgen tot punt-naar-site VPN-verbinding.

Meer informatie vindt u hier
* [Een punt-naar-site-verbinding met een VNet configureren met Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Een punt-naar-site-verbinding met een VNet configureren met behulp van PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

#### <a name="multi-site-vpn"></a>Meerdere Site-VPN

Azure biedt ook tegenwoordig de mogelijkheid voor het maken van meerdere Site-VPN-connectiviteit voor één Azure-abonnement. Eén abonnement was eerder beperkt tot één site-naar-site VPN-verbinding. Deze beperking is onmiddellijk een fout met multi-site-VPN-verbindingen voor één abonnement. Dit maakt het mogelijk gebruikmaken van meer dan één Azure-regio voor een specifiek abonnement via cross-premises configuraties.

Zie voor meer documentatie [in dit artikel][vpn-gateway-create-site-to-site-rm-powershell]

#### <a name="vnet-to-vnet-connection"></a>VNet-naar-VNet-verbinding

Meerdere Site-VPN gebruiken, moet u een afzonderlijke Azure-netwerk configureren in elk van de regio's. U hebt echter vaak de vereiste dat de software-onderdelen in de verschillende regio's met elkaar moeten communiceren. In het ideale geval moet u deze communicatie niet doorgestuurd van de ene Azure-regio naar on-premises en van daaruit naar de andere Azure-regio. Op de snelkoppeling biedt Azure de mogelijkheid om een verbinding van een virtueel Azure-netwerk in een regio met een andere Azure-netwerk configureren die worden gehost in een andere regio. Deze functionaliteit wordt VNet-naar-VNet-verbinding genoemd. Meer informatie over deze functie vindt u hier: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure-expressroute"></a>Particuliere verbinding met Azure ExpressRoute

Microsoft Azure ExpressRoute kunt het maken van privéverbindingen tussen Azure-datacenters en van de klant on-premises infrastructuur of in een CO-locatie-omgeving. ExpressRoute wordt aangeboden door verschillende MPLS VPN-providers (geschakeld pakket) of andere serviceproviders netwerk. ExpressRoute-verbindingen gaan niet via het openbare internet. ExpressRoute-verbindingen bieden een betere beveiliging, meer betrouwbaarheid via meerdere parallelle circuits, hogere snelheden en kortere wachttijden dan gebruikelijke verbindingen via Internet.

Meer informatie over Azure ExpressRoute en aanbiedingen hier vinden:

* <https://azure.microsoft.com/documentation/services/expressroute/>
* <https://azure.microsoft.com/pricing/details/expressroute/>
* <https://azure.microsoft.com/documentation/articles/expressroute-faqs/>

Express Route kan meerdere Azure-abonnementen via een ExpressRoute-circuit, zoals hier wordt beschreven

* <https://azure.microsoft.com/documentation/articles/expressroute-howto-linkvnet-arm/>
* <https://azure.microsoft.com/documentation/articles/expressroute-howto-circuit-arm/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>Geforceerde tunneling in het geval van cross-premises
Voor virtuele machines on-premises domeinen via site-naar-site-, punt-naar-site- of ExpressRoute toevoegen, moet u om ervoor te zorgen dat de Internet-proxy-instellingen voor alle gebruikers in deze virtuele machines ook zijn ophalen geïmplementeerd. Standaard software die wordt uitgevoerd in deze virtuele machines of gebruikers via een browser voor toegang tot het internet niet via de proxy-bedrijf wilt gaan, maar rechtstreeks via Azure met het internet verbinding te maken. Maar zelfs de proxy-instellingen is niet een 100%-oplossing om te leiden van het verkeer via de proxy-bedrijf omdat het is de verantwoordelijkheid van de software en services om te controleren voor de proxy. Als software die wordt uitgevoerd in de virtuele machine die niet actief of een beheerder de instellingen wordt bewerkt, verkeer naar Internet opnieuw kunt detoured rechtstreeks via Azure met het Internet.

Om te voorkomen dat deze een directe verbinding met internet, kunt u geforceerde Tunneling configureren met site-naar-site-connectiviteit tussen on-premises en Azure. De gedetailleerde beschrijving van de functie geforceerde Tunneling is hier gepubliceerd <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Geforceerde Tunneling met ExpressRoute wordt ingeschakeld door klanten kondigt een standaardroute via de ExpressRoute-BGP-peeringsessies.

#### <a name="summary-of-azure-networking"></a>Overzicht van Azure-netwerken

In dit hoofdstuk bevat veel belangrijke punten met betrekking tot Azure Networking. Hier volgt een samenvatting van de belangrijkste punten:

* Virtuele netwerken van Azure kunt u de netwerkstructuur van een in uw Azure-implementatie wordt geplaatst. VNets kunnen worden geïsoleerd ten opzichte van elkaar of met behulp van Netwerkbeveiligingsgroepen verkeer tussen VNets kunnen worden beheerd.
* Virtuele netwerken van Azure kunnen worden gebruikt om IP-adresbereiken toewijzen aan virtuele machines of vaste IP-adressen toewijzen aan virtuele machines
* Voor het instellen van een Site-naar-Site of een punt-naar-Site-verbinding moet u eerst een Azure-netwerk maken
* Als een virtuele machine eenmaal is geïmplementeerd, is het niet meer mogelijk om te wijzigen van het Virtueelnetwerk dat is toegewezen aan de virtuele machine

### <a name="quotas-in-azure-virtual-machine-services"></a>Quota's in de virtuele Machine van Azure-Services
We moeten duidelijk over het feit dat de infrastructuur voor opslag en netwerk wordt gedeeld tussen virtuele machines met verschillende services in de Azure-infrastructuur. En net zoals in de datacenters van klant capaciteit van een aantal van de infrastructuurresources in te richten, vindt plaats in een mate. Het Microsoft Azure-Platform gebruikt, CPU, netwerk, en andere quota te beperken van het Resourcegebruik en te behouden van consistente en deterministisch prestaties.  De verschillende VM-typen (A5, A6, enzovoort) hebben verschillende quota's voor het aantal schijven, CPU, RAM, en het netwerk.

> [!NOTE]
> CPU en geheugen resources van de virtuele machine die worden ondersteund door SAP zijn vooraf toegewezen op de hostknooppunten. Dit betekent dat wanneer de virtuele machine is geïmplementeerd, de resources op de host beschikbaar zoals gedefinieerd door de VM-type zijn.
>
>

Bij het plannen en het formaat van SAP op Azure-oplossingen, kan de quota voor de grootte van elke virtuele machine moeten worden beschouwd. De VM-quota zijn beschreven [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

De quota's die worden beschreven, vertegenwoordigen de theoretische maximale waarden.  De limiet van IOP's per schijf kan worden bereikt met kleine IOs (8kb), maar mogelijk niet kan worden bereikt met grote IOs (1Mb).  De IOPS-limiet wordt afgedwongen op de granulariteit van één schijf.

Als een ruwe beslissingsstructuur om te bepalen of een SAP-systeem in Azure-Services voor virtuele Machine en de mogelijkheden of of een bestaand systeem moet anders worden geconfigureerd om te implementeren van het systeem op Azure past bij kan de onderstaande beslissingsstructuur worden gebruikt:

![Beslissingsstructuur om te bepalen van de mogelijkheid tot het implementeren van SAP op Azure][planning-guide-figure-700]

**Stap 1**: de belangrijkste informatie is te beginnen met de vereiste SAP's voor een bepaald SAP-systeem. De vereisten voor SAP's moeten worden onderverdeeld in het gedeelte DBMS en de SAP-toepassing-onderdeel, zelfs als de SAP-systeem al on-premises geïmplementeerd in een tier-2-configuratie is. Voor bestaande systemen, kunnen de SAP's vaak met betrekking tot de hardware gebruikt worden bepaald of geschat op basis van bestaande SAP benchmarks. De resultaten vindt u hier: <https://sap.com/about/benchmark.html>.
Voor de zojuist geïmplementeerde SAP-systemen, moet u hebben doorlopen een oefening grootte, die de vereisten voor SAP's van het systeem te bepalen.
Zie ook deze blog en bijgevoegde document voor schaling voor SAP op Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Stap 2**: voor bestaande systemen, de i/o-volume en de i/o-bewerkingen per seconde op de DBMS-server moeten worden gemeten. Voor nieuwe geplande systemen geeft de grootte oefening voor het nieuwe systeem ook ruwe ideeën van de i/o-vereisten aan de DBMS-systemen. Als u niet zeker, moet u uw voor het uitvoeren van een Proof of Concept.

**Stap 3**: de vereiste SAP's voor de DBMS-server met de SAP's u de verschillende typen van de virtuele machine van Azure krijgt vergelijken. De gegevens van SAP's van de verschillende typen Azure VM's wordt beschreven in de SAP-notitie [1928533]. De focus moet eerst op de DBMS-VM worden omdat de databaselaag is de laag in een SAP NetWeaver-systeem dat komt niet in de meeste implementaties worden uitgebreid. Daarentegen kan aan de SAP-toepassingslaag worden uitgebreid. Als geen van de SAP wordt ondersteund typen Azure VM's kunnen de vereiste SAP's leveren, de werkbelasting van de geplande SAP-systeem kan niet worden uitgevoerd op Azure. U moet een implementatie van het systeem on-premises of u wilt wijzigen van het volume van de werkbelasting voor het systeem.

**Stap 4**: zoals wordt beschreven [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows], Azure dwingt een quotum IOP's per schijf onafhankelijke of u Standard Storage of Premium-opslag gebruiken. Afhankelijk van het type virtuele machine, verschilt van het aantal gegevensschijven, die kunnen worden gekoppeld. Als gevolg hiervan kunt u het maximum aantal IOP's die kan worden bereikt met elk van de verschillende VM-typen berekenen. Afhankelijk van de database-bestandsindeling, u kunt stripe-schijven om te worden van een volume in het gastbesturingssysteem te installeren. Echter, als het huidige volume IOPS van een geïmplementeerde SAP-systeem de berekende grenzen van de grootste VM-type van Azure en overschrijdt als er is geen kans om te compenseren met meer geheugen, de werkbelasting van de SAP-systeem kan worden beïnvloed ernstige problemen. In dergelijke gevallen kunt u een punt waar u moet het systeem op Azure niet implementeren bereikt.

**Stap 5**: in SAP-systemen, die on-premises geïmplementeerd in Tier-2-configuraties, met name de kans is dat het systeem mogelijk moeten worden geconfigureerd in Azure in een configuratie met 3-Laagse. In deze stap moet u controleren of er een onderdeel van de SAP-toepassingslaag is, die niet worden uitgebreid en die niet zou passen in de CPU en geheugen resources die de verschillende typen Azure VM's bieden. Als er inderdaad een dergelijke component, kunnen niet de SAP-systeem en de belasting worden geïmplementeerd in Azure. Maar als u de SAP-toepassingsonderdelen naar meerdere Azure-VM's uitschalen kunt, het systeem kan worden geïmplementeerd in Azure.

**Stap 6**: als de DBMS-systemen en SAP laag toepassingsonderdelen kunnen worden uitgevoerd in Azure VM's, de configuratie moet worden gedefinieerd met betrekking tot:

* Aantal virtuele machines in Azure
* VM-typen voor de afzonderlijke onderdelen
* Aantal VHD's in DBMS-VM voor voldoende IOPS

## <a name="managing-azure-assets"></a>Azure-Assets beheren

### <a name="azure-portal"></a>Azure Portal

De Azure-portal is een van drie interfaces voor het beheren van Azure VM-implementaties. De algemene beheertaken, zoals de implementatie van virtuele machines van afbeeldingen, kunnen worden gedaan via de Azure-portal. Het maken van Opslagaccounts, virtuele netwerken en andere Azure-onderdelen zijn bovendien ook taken kan ook worden verwerkt in de Azure-portal. Functionaliteit, zoals VHD's van on-premises uploaden naar Azure of kopiëren van een VHD binnen Azure zijn echter taken, die hulpprogramma's van derden of beheer via PowerShell of CLI vereist.

![Microsoft Azure portal - overzicht van virtuele machines][planning-guide-figure-800]

[comment]: <> (MSSedusch * <https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * <https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Beheer- en configuratietaken voor de virtuele Machine-instantie zijn mogelijk in Azure portal.

Naast het opnieuw opstarten of afsluiten van een virtuele Machine kunt u ook koppelen, loskoppelen, en maken gegevensschijven voor het exemplaar van de virtuele Machine, om vast te leggen van de instantie voor de voorbereiding van de installatiekopie, en configureert u de grootte van de virtuele Machine-instantie.

De Azure portal biedt basisfuncties voor het implementeren en configureren van virtuele machines en vele andere Azure-services. Maar niet alle beschikbare functionaliteit wordt gedekt door de Azure-portal. In de Azure-portal is het niet mogelijk om uit te voeren taken, zoals:

* VHD's uploaden naar Azure
* Kopiëren van virtuele machines

[comment]: <> (MShermannd TODO hoe zit het met automation-service voor SAP-VM's? )
[comment]: <> (Implementatie van meerdere virtuele machines besturingssysteem in de tussentijd zorgen mogelijk MSSedusch)
[comment]: <> (Elk type automation met betrekking tot de implementatie is MSSedusch ook niet mogelijk is met de Azure-portal. Taken, zoals de procedure voor implementatie van meerdere virtuele machines is niet mogelijk via de Azure-portal.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Beheer via Microsoft Azure PowerShell-cmdlets

Windows PowerShell is een krachtig en uitbreidbaar framework dat de breed is vastgesteld door klanten groot aantal systemen in Azure implementeren. Na de installatie van PowerShell-cmdlets op een bureaublad, laptop of toegewezen management station, kan de PowerShell-cmdlets op afstand worden uitgevoerd.

Het proces voor het inschakelen van een lokale bureaublad/laptop voor het gebruik van Azure PowerShell-cmdlets en hoe u deze configureert voor gebruik met de Azure-abonnementen wordt beschreven in [in dit artikel][powershell-install-configure].

Meer gedetailleerde instructies over het installeren, bijwerken en configureren van de Azure PowerShell cmdlets kunt u vinden in [in dit hoofdstuk van de Implementatiehandleiding][deployment-guide-4.1].

Klantervaring is tot nu toe PowerShell (PS) is zeker de krachtige hulpprogramma voor virtuele machines te implementeren en te maken van aangepaste stappen in de implementatie van virtuele machines. Alle klanten die SAP-exemplaren worden uitgevoerd in Azure PS-cmdlets gebruiken om te voorzien in beheertaken zij in Azure portal of zelfs PS-cmdlets gebruikt uitsluitend bedoeld voor het beheren van implementaties in Azure. Omdat de Azure-specifieke cmdlets de dezelfde naamgevingsregel als de meer dan 2000 met betrekking tot Windows-cmdlets delen, is het een eenvoudige taak voor Windows-beheerders gebruikmaken van deze cmdlets.

In dit voorbeeld Zie: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd TODO beschrijven nieuwe CLI-opdracht wanneer getest )
Implementatie van de Azure Monitoring-extensie voor SAP (Zie hoofdstuk [Azure Monitoring oplossing voor SAP] [ planning-guide-9.1] in dit document) is alleen mogelijk via PowerShell of CLI. Daarom is het verplicht te installeren en configureren van PowerShell of CLI bij het implementeren of beheren van een SAP NetWeaver-systeem in Azure.  

Azure biedt meer functionaliteit, gaan nieuwe PS-cmdlets waarvoor een update van de cmdlets worden toegevoegd. Daarom is het zinvol om te controleren of de Azure-downloadsite ten minste één keer de maand <https://azure.microsoft.com/downloads/> voor een nieuwe versie van de cmdlets. De nieuwe versie is geïnstalleerd op de oudere versie.

Voor een algemeen overzicht van PowerShell voor Azure-gerelateerde opdrachten kom hier: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>Beheer via Microsoft Azure CLI-opdrachten

Voor klanten die gebruikmaken van Linux en voor het beheren van Azure Powershell resources mogelijk niet een optie. Microsoft biedt Azure CLI als alternatief.
De Azure CLI bevat een set open-source, platformoverschrijdende opdrachten om te werken met het Azure-Platform. De Opdrachtregelinterface van Azure biedt veel van dezelfde functionaliteit te vinden in de Azure portal.

Zie voor meer informatie over installatie, configuratie en het gebruik van CLI opdrachten voor het uitvoeren van taken op Azure

* [De klassieke Azure-CLI installeren][xplat-cli]
* [Implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en de Azure CLI] [../../linux/create-ssh-secured-vm-from-template.md]
* [Gebruik de klassieke Azure CLI voor Mac, Linux en Windows met Azure Resource Manager][xplat-cli-azure-resource-manager]

Lees ook hoofdstuk [Azure CLI voor Linux-VM's] [ deployment-guide-4.5.2] in de [Deployment Guide] [ planning-guide] over hoe u Azure CLI gebruiken voor het implementeren van de Azure Monitoring De extensie voor SAP.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Verschillende manieren voor het implementeren van virtuele machines voor SAP in Azure

In dit hoofdstuk leert u de verschillende manieren om een VM in Azure te implementeren. Extra voorbereiding procedures, evenals de verwerking van VHD's en virtuele machines in Azure worden besproken in dit hoofdstuk.

### <a name="deployment-of-vms-for-sap"></a>Implementatie van virtuele machines voor SAP

Microsoft Azure biedt verschillende manieren om virtuele machines en gekoppelde schijven te implementeren. Het is dus belangrijk om te weten wat de verschillen sinds de voorbereiding van de virtuele machines kunnen variëren, afhankelijk van de methode van de implementatie. In het algemeen nemen we een overzicht van de volgende scenario's:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd

U van plan bent een specifieke SAP-systeem van on-premises naar Azure te verplaatsen. Dit kan worden gedaan door het uploaden van de VHD, die het besturingssysteem, de SAP-binaire bestanden en binaire bestanden voor DBMS bevat plus de VHD's met de gegevens en logboekbestanden bestanden van de DBMS-systemen naar Azure. In tegenstelling tot [scenario #2 hieronder][planning-guide-5.1.2], houdt u de hostnaam, SAP-SID en SAP-gebruiker-accounts in de Azure-VM zoals ze zijn geconfigureerd in de on-premises omgeving. Generaliseren van de installatiekopie is daarom niet nodig. Zie de hoofdstukken [voorbereiding voor een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd] [ planning-guide-5.2.1] van dit document voor voorbereidingsstappen voor on-premises en uploaden van niet-gegeneraliseerde VM's of VHD's naar Azure. Lezen hoofdstuk [Scenario 3: een virtuele machine verplaatsen van on-premises met behulp van een Azure niet gegeneraliseerde VHD met SAP] [ deployment-guide-3.4] in de [Deployment Guide] [ deployment-guide] voor gedetailleerde procedure voor het implementeren van dergelijke een installatiekopie in Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Een virtuele machine met een installatiekopie klantspecifieke

De opgegeven in de Azure Marketplace-installatiekopieën mogelijk vanwege een patch voor specifieke vereisten van uw besturingssysteem of de DBMS-versie, niet aan uw behoeften voldoen. Daarom moet u mogelijk een virtuele machine maken met uw eigen persoonlijke OS/DBMS-VM-installatiekopie, die kan worden geïmplementeerd als meerdere keren daarna. Als u wilt een persoonlijke installatiekopie voorbereiden voor het kopiëren, moeten de volgende items worden beschouwd als:

- - -
> ![Windows][Logo_Windows] Windows
>
> Lees hier meer informatie: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> de Windows-instellingen (zoals Windows SID en hostnaam) moet op de on-premises VM via de opdracht sysprep geabstraheerd/gegeneraliseerd.
>
>
> ![Linux][Logo_Linux] Linux
>
> Volg de stappen in deze artikelen voor [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], of [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], het voorbereiden van een VHD moet worden geüpload naar Azure.
>
>

- - -
Als u al SAP-inhoud in uw on-premises VM (met name voor Tier-2-systemen) hebt geïnstalleerd, kunt u de instellingen van het SAP aanpassen nadat de implementatie van de virtuele Azure-machine via het exemplaar procedure ondersteund door het SAP Software inrichting Manager (SAP wijzigen Houd er rekening mee [1619720]). Zie de hoofdstukken [voorbereiding voor het implementeren van een virtuele machine met een installatiekopie klantspecifieke voor SAP] [ planning-guide-5.2.2] en [uploaden van een VHD van on-premises naar Azure] [ planning-guide-5.3.2]van dit document voor voorbereidingsstappen voor on-premises en uploaden van een gegeneraliseerde VM in Azure. Lezen hoofdstuk [Scenario 2: een virtuele machine met een aangepaste installatiekopie implementeren voor SAP] [ deployment-guide-3.3] in de [Deployment Guide] [ deployment-guide] voor gedetailleerde stappen van de implementatie van dergelijke een installatiekopie in Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Een virtuele machine buiten de Azure Marketplace

U wilt gebruikmaken van een Microsoft of derden opgegeven VM-installatiekopie uit de Azure Marketplace om uw VM te implementeren. Nadat u uw virtuele machine in Azure geïmplementeerd, volgt u dezelfde richtlijnen en hulpprogramma's voor het installeren van de SAP-software en/of de DBMS-systemen binnen uw virtuele machine net zoals u in een on-premises-omgeving doen zou. Zie voor meer beschrijving van implementatie gedetailleerde, hoofdstuk [Scenario 1: een virtuele machine buiten de Azure Marketplace voor SAP] [ deployment-guide-3.2] in de [Deployment Guide] [ deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Virtuele machines met SAP voorbereiden voor Azure

Voordat u virtuele machines uploadt naar Azure, moet u controleren of de virtuele machines en VHD's te voldoen aan bepaalde vereisten. Er zijn kleine verschillen, afhankelijk van de implementatiemethode die wordt gebruikt.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Voorbereiding voor een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd

Er is een algemene implementatiemethode te verplaatsen van een bestaande virtuele machine, die wordt uitgevoerd van een SAP-systeem van on-premises naar Azure. Deze virtuele machine en de SAP-systeem in de virtuele machine alleen moeten worden uitgevoerd in Azure met behulp van de dezelfde hostnaam en waarschijnlijk de dezelfde SAP-SID. In dit geval moet de Gast-besturingssysteem van de VM niet worden gebruikt voor meerdere implementaties. Als de on-premises netwerk hebt uitgebreid naar Azure (Zie hoofdstuk [cross-premises - implementatie van één of meerdere SAP-virtuele machines naar Azure met de vereiste van volledig geïntegreerd in de on-premises netwerk] [ planning-guide-2.2] in dit document), en zelfs de dezelfde domeinaccounts kunnen worden gebruikt binnen de virtuele machine, zoals die zijn gebruikt voordat u on-premises.

Vereisten bij het voorbereiden van uw eigen Azure-VM-schijf zijn:

* De VHD met het besturingssysteem kan oorspronkelijk alleen een maximale grootte van 127GB zijn. Deze beperking is aan het einde van maart 2015 verwijderd. Nu de VHD met het besturingssysteem maximaal 1TB groot zijn kan als een andere Azure-opslag VHD ook gehoste.
* Het moet zich in de vaste VHD-indeling. Dynamische virtuele harde schijven of VHD's in de VHDx-indeling zijn nog niet ondersteund op Azure. Dynamische VHD's worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met de PowerShell-commandlets of CLI
* VHD's, die zijn gekoppeld aan de virtuele machine en moeten worden gekoppeld opnieuw in Azure worden in een vaste VHD-indeling ook aan de virtuele machine. Lezen [in dit artikel (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) en [in dit artikel (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) voor de maximale grootte van gegevensschijven. Dynamische VHD's worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met de PowerShell-commandlets of CLI
* Voeg een andere lokale account met beheerdersbevoegdheden, die kan worden gebruikt door Microsoft ondersteuning of die kunnen worden toegewezen als context voor services en toepassingen worden uitgevoerd in totdat de virtuele machine is geïmplementeerd en meer gebruikers kan worden gebruikt.
* Voor het geval van het gebruik van een Cloud-Only implementatiescenario (Zie hoofdstuk [Cloud-Only - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk] [ planning-guide-2.1] van dit document) in de combinatie met deze implementatiemethode, domeinaccounts werkt mogelijk niet wanneer de Azure-schijf is geïmplementeerd in Azure. Dit geldt met name voor accounts die worden gebruikt om uit te voeren, zoals de DBMS-systemen of SAP-toepassingen. Daarom moet u dergelijke domeinaccounts vervangen door de lokale accounts VM en de on-premises domein-accounts in de virtuele machine verwijderen. On-premises domeingebruikers blijven in de VM-installatiekopie is niet een probleem wanneer de virtuele machine is geïmplementeerd in het scenario voor cross-premises, zoals beschreven in het hoofdstuk [Cross-Premises - implementatie van één of meerdere SAP-virtuele machines naar Azure met de vereiste worden volledig geïntegreerd in de on-premises netwerk] [ planning-guide-2.2] in dit document.
* Als u domeinaccounts gebruikt als DBMS aanmeldingen of gebruikers bij het uitvoeren van het systeem on-premises en deze virtuele machines moeten worden geïmplementeerd in scenario's alleen in de Cloud, moeten de gebruikers van een domein worden verwijderd. U moet ervoor zorgen dat de lokale beheerder plus een andere virtuele machine lokale gebruiker is toegevoegd als een aanmelding/gebruiker die in het DBMS-Administrators.
* Andere lokale accounts toevoegen als die welke nodig zijn voor het specifieke scenario.

- - -
> ![Windows][Logo_Windows] Windows
>
> In dit scenario is geen generalisatie (sysprep) van de virtuele machine vereist om te uploaden en implementeren van de virtuele machine op Azure.
> Zorg ervoor dat station die d:\ wordt niet gebruikt.
> Instellen van schijf automount voor gekoppelde schijven, zoals beschreven in het hoofdstuk [instellen automount voor gekoppelde schijven] [ planning-guide-5.5.3] in dit document.
>
> ![Linux][Logo_Linux] Linux
>
> In dit scenario geen generalisatie (waagent-inrichting) van de virtuele machine is vereist om te uploaden en implementeren van de virtuele machine op Azure.
> Zorg ervoor dat/mnt/resource niet wordt gebruikt en dat alle schijven zijn gekoppeld via uuid. Zorg ervoor dat de bootloader post ook het koppelpunt uuid op basis van weerspiegelt de besturingssysteemschijf.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Voorbereiding voor het implementeren van een virtuele machine met een installatiekopie klantspecifieke voor SAP

VHD-bestanden met een gegeneraliseerde OS worden opgeslagen in containers op Azure Storage-Accounts of als beheerde schijf-installatiekopieën. U kunt een nieuwe virtuele machine uit een dergelijke afbeelding implementeren die verwijst naar de installatiekopie van het VHD- of beheerde schijf als een bron in uw implementatie-sjabloonbestanden zoals beschreven in het hoofdstuk [Scenario 2: een virtuele machine met een aangepaste installatiekopie implementeren voor SAP] [ deployment-guide-3.3]van de [Implementatiehandleiding][deployment-guide].

Vereisten bij het voorbereiden van uw eigen Azure-VM-installatiekopie zijn:

* De VHD met het besturingssysteem kan oorspronkelijk alleen een maximale grootte van 127GB zijn. Deze beperking is aan het einde van maart 2015 verwijderd. Nu de VHD met het besturingssysteem maximaal 1TB groot zijn kan als een andere Azure-opslag VHD ook gehoste.
* Het moet zich in de vaste VHD-indeling. Dynamische virtuele harde schijven of VHD's in de VHDx-indeling zijn nog niet ondersteund op Azure. Dynamische VHD's worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met de PowerShell-commandlets of CLI
* VHD's, die zijn gekoppeld aan de virtuele machine en moeten worden gekoppeld opnieuw in Azure worden in een vaste VHD-indeling ook aan de virtuele machine. Lees [in dit artikel (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) en [in dit artikel (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) voor de maximale grootte van gegevensschijven. Dynamische VHD's worden geconverteerd naar vaste VHD's tijdens het uploaden van de VHD met de PowerShell-commandlets of CLI
* Omdat alle domeingebruikers geregistreerd als gebruikers in de virtuele machine niet in een Cloud-Only-scenario bestaat (Zie hoofdstuk [Cloud-Only - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk] [ planning-guide-2.1] van dit document), met behulp van dergelijke domain accounts niet werken mogelijk wanneer de installatiekopie is geïmplementeerd in Azure-services. Dit geldt met name voor accounts die worden gebruikt voor services zoals DBMS-systemen of SAP-toepassingen worden uitgevoerd. Daarom moet u dergelijke domeinaccounts vervangen door de lokale accounts VM en de on-premises domein-accounts in de virtuele machine verwijderen. On-premises domeingebruikers blijven in de VM-installatiekopie niet mogelijk een probleem wanneer de virtuele machine wordt geïmplementeerd in het scenario voor cross-premises, zoals beschreven in het hoofdstuk [Cross-Premises - implementatie van één of meerdere SAP-virtuele machines naar Azure met de vereiste van wordt volledig geïntegreerd in de on-premises netwerk] [ planning-guide-2.2] in dit document.
* Voeg een andere lokale account met beheerdersbevoegdheden, die kan worden gebruikt door Microsoft-ondersteuning in het probleem onderzoeken of die kunnen worden toegewezen als context voor services en toepassingen worden uitgevoerd in totdat de virtuele machine is geïmplementeerd en geschikter gebruikers kan worden gebruikt.
* In de alleen-implementaties en waar domeinaccounts als DBMS aanmeldingen of gebruikers zijn gebruikt bij het uitvoeren van de system on-premises, moeten de gebruikers van een domein worden verwijderd. U moet ervoor zorgen dat de lokale beheerder plus een andere virtuele machine lokale gebruiker is toegevoegd als een aanmelding/gebruiker van de DBMS-Administrators.
* Andere lokale accounts toevoegen als die welke nodig zijn voor het specifieke scenario.
* Als de afbeelding bevat een installatie van SAP NetWeaver en wijzigen van de naam van de hostnaam van de oorspronkelijke naam op het moment van de Azure-implementatie is waarschijnlijk dat het beste de nieuwste versies van de SAP-Software wordt ingericht Manager DVD kopiëren naar de sjabloon. Hierdoor kunt u eenvoudig de functionaliteit voor het wijzigen van SAP opgegeven om te gebruiken om aan te passen de hostnaam van de gewijzigde en/of de SID van de SAP-systeem in de geïmplementeerde VM-installatiekopie wijzigen zodra een nieuw exemplaar wordt gestart.

- - -
> ![Windows][Logo_Windows] Windows
>
> Zorg ervoor dat station D:\ niet is ingesteld schijf automount voor de gekoppelde schijven gebruikt zoals beschreven in het hoofdstuk [instellen automount voor gekoppelde schijven] [ planning-guide-5.5.3] in dit document.
>
> ![Linux][Logo_Linux] Linux
>
> Zorg ervoor dat/mnt/resource niet wordt gebruikt en dat alle schijven zijn gekoppeld via uuid. Voor de schijf van het besturingssysteem, zorg ervoor dat de bootloader vermelding geeft ook de koppeling op basis van een uuid.
>
>

- - -
* SAP-GUI (voor administratieve en doelen instellen) vooraf kan worden geïnstalleerd in een sjabloon.
* Andere software die nodig is voor een geslaagde uitvoering van de virtuele machines in een cross-premises scenario's kan worden geïnstalleerd, zolang deze software met de naam van de virtuele machine werken kunt.

Als de virtuele machine is voldoende voorbereid algemene en uiteindelijk onafhankelijk van accounts/gebruikers niet beschikbaar in de betreffende Azure deployment-scenario, wordt de laatste stap van de voorbereiding van het generaliseren van dergelijke een installatiekopie uitgevoerd.

##### <a name="generalizing-a-vm"></a>Een virtuele machine generaliseren
- - -
> ![Windows][Logo_Windows] Windows
>
> De laatste stap is het aanmelden bij een virtuele machine met een Administrator-account. Open een Windows-opdrachtvenster als *beheerder*. Ga naar %windir%\windows\system32\sysprep en sysprep.exe uitvoeren.
> Een klein venster wordt weergegeven. Het is belangrijk om te controleren of de **Generalize** optie (de standaardwaarde is uitgeschakeld) en wijzig de optie afsluiten van de standaardwaarde van 'Opnieuw opstarten' naar 'afsluiten'. Deze procedure wordt ervan uitgegaan dat de sysprep-proces uitgevoerd on-premises beschikken in het Gastbesturingssysteem van een virtuele machine.
> Als u uitvoeren van de procedure met een virtuele machine al wordt uitgevoerd in Azure wilt, volgt u de stappen in [in dit artikel](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Vastleggen van een virtuele Linux-machine om te gebruiken als een Resource Manager-sjabloon][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Virtuele machines en VHD's overbrengen tussen on-premises naar Azure
Aangezien VM-installatiekopieën en schijven uploaden naar Azure niet mogelijk via de Azure-portal is, moet u Azure PowerShell-cmdlets of de CLI gebruiken. Een andere mogelijkheid is het gebruik van het hulpprogramma 'AzCopy'. Het hulpprogramma kunt VHD's kopiëren tussen on-premises en Azure (in beide richtingen). Het kan ook VHD's tussen Azure-regio's kopiëren. Neem contact op met [deze documentatie] [ storage-use-azcopy] downloaden en het gebruik van AzCopy.

Een derde alternatief worden verschillende grafische gebruikersinterface gebaseerde hulpprogramma's van derden gebruikt. Echter, ervoor zorgen dat deze hulpprogramma's ondersteuning voor Azure-pagina-Blobs bieden. Voor ons doel, moeten we gebruiken Azure-pagina-Blob-archief (de verschillen worden hier beschreven: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). De hulpprogramma's van Azure zijn ook efficiënt voor het comprimeren van de virtuele machines en VHD's die moeten worden geüpload. Dit is belangrijk omdat deze efficiëntie van de compressie de tijd vermindert (dit is toch afhankelijk van de uploadkoppeling met het internet van de on-premises-faciliteit en de implementatie van Azure-regio gericht). Het is een geoorloofd veronderstelling dat het uploaden van een virtuele machine of VHD van de Europese locatie voor de VS gebaseerde Azure data centers langer duurt dan de dezelfde VM's / VHD's uploaden naar de Europese Azure datacenters.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Uploaden van een VHD van on-premises naar Azure
Een bestaande virtuele machine of een VHD uploaden vanuit de on-premises netwerk die een virtuele machine of VHD moet voldoen aan de vereisten zoals vermeld in het hoofdstuk [voorbereiding voor een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd] [ planning-guide-5.2.1]van dit document.

Een virtuele machine hoeft niet te worden gegeneraliseerd en in de status en vorm na afsluiten aan de on-premises heeft kan worden geüpload. Hetzelfde geldt voor extra virtuele harde schijven, die elk besturingssysteem niet bevatten.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Een VHD uploaden en het maken van een Azure-schijf
In dit geval willen we een VHD, met of zonder een besturingssysteem, uploaden en koppelen aan een virtuele machine als gegevensschijf of die als besturingssysteemschijf. Dit is een proces met meerdere stappen

**Powershell**

* Meld u aan bij uw abonnement met *Connect-AzureRmAccount*
* Instellen van het abonnement van uw context met *Set-AzureRmContext* en parameter abonnements-id of SubscriptionName - bekijken <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Uploaden van de VHD met *Add-AzureRmVhd* naar een Azure Storage-Account - Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Optioneel) Maak een beheerde schijf van de VHD met *New-AzureRmDisk* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* De besturingssysteemschijf van een nieuwe VM-configuratie ingesteld op de VHD- of beheerde schijf met *Set-azurermvmosdisk,* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* Een nieuwe virtuele machine maken van de configuratie van de virtuele machine met *New-AzureRmVM* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Een gegevensschijf toevoegen aan een nieuwe virtuele machine met *Add-azurermvmdatadisk en* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**Azure-CLI**

* Meld u aan bij uw abonnement met *az login*
* Selecteer uw abonnement met *az account set--abonnement `<subscription name or id`>*
* Uploaden van de VHD met *az storage blob upload* -Zie [met de Azure CLI met Azure Storage][storage-azure-cli]
* (Optioneel) Maak een beheerde schijf van de VHD met *az schijf maken* -Zie https://docs.microsoft.com/cli/azure/disk#az_disk_create
* Maak een nieuwe virtuele machine op te geven de geüploade VHD of beheerde schijf als besturingssysteemschijf met *az vm maken* en de parameter *--koppelen-os-schijf*
* Een gegevensschijf toevoegen aan een nieuwe virtuele machine met *az vm disk attach* en de parameter *--nieuwe*

**Sjabloon**

* Uploaden van de VHD met Powershell of Azure CLI
* (Optioneel) Maak een beheerde schijf van de VHD met Powershell, Azure CLI of Azure portal
* Implementeer de virtuele machine met een JSON-sjabloon die verwijst naar de VHD, zoals wordt weergegeven in [deze voorbeeld-JSON-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) of met behulp van Managed Disks, zoals wordt weergegeven in [deze voorbeeld-JSON-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Implementatie van een VM-installatiekopie
Een bestaande virtuele machine of een VHD uploaden van de on-premises netwerk om het te gebruiken als een Azure VM-installatiekopie die een virtuele machine of VHD moet voldoen aan de vereisten die worden vermeld in het hoofdstuk [voorbereiding voor het implementeren van een virtuele machine met een installatiekopie klantspecifieke voor SAP] [ planning-guide-5.2.2] van dit document.

* Gebruik *sysprep* op Windows of *waagent-inrichting* op Linux om te generaliseren van uw virtuele machine - Zie [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx) voor Windows of [vastleggen een Virtuele Linux-machine om te gebruiken als een Resource Manager-sjabloon] [ capture-image-linux-step-2-create-vm-image] voor Linux
* Meld u aan bij uw abonnement met *Connect-AzureRmAccount*
* Instellen van het abonnement van uw context met *Set-AzureRmContext* en parameter abonnements-id of SubscriptionName - bekijken <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Uploaden van de VHD met *Add-AzureRmVhd* naar een Azure Storage-Account - Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Optioneel) De installatiekopie van een beheerde schijf maken van de VHD met *New-AzureRmImage* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Instellen van de besturingssysteemschijf van een nieuwe VM-configuratie op de
  * VHD met *Set-azurermvmosdisk, - SourceImageUri - CreateOption fromImage* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Beheerde schijf afbeelding *Set AzureRmVMSourceImage* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* Een nieuwe virtuele machine maken van de configuratie van de virtuele machine met *New-AzureRmVM* -Zie <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**Azure-CLI**

* Gebruik *sysprep* op Windows of *waagent-inrichting* op Linux om te generaliseren van uw virtuele machine - Zie [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx) voor Windows of [vastleggen een Virtuele Linux-machine om te gebruiken als een Resource Manager-sjabloon] [ capture-image-linux-step-2-create-vm-image] voor Linux
* Meld u aan bij uw abonnement met *az login*
* Selecteer uw abonnement met *az account set--abonnement `<subscription name or id`>*
* Uploaden van de VHD met *az storage blob upload* -Zie [met de Azure CLI met Azure Storage][storage-azure-cli]
* (Optioneel) De installatiekopie van een beheerde schijf maken van de VHD met *az afbeelding maken* -Zie https://docs.microsoft.com/cli/azure/image#az_image_create
* Maak een nieuwe virtuele machine op te geven de geüploade VHD of installatiekopie voor beheerde schijf als besturingssysteemschijf met *az vm maken* en de parameter *--installatiekopie*

**Sjabloon**

* Gebruik *sysprep* op Windows of *waagent-inrichting* op Linux om te generaliseren van uw virtuele machine - Zie [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx) voor Windows of [vastleggen een Virtuele Linux-machine om te gebruiken als een Resource Manager-sjabloon] [ capture-image-linux-step-2-create-vm-image] voor Linux
* Uploaden van de VHD met Powershell of Azure CLI
* (Optioneel) De installatiekopie van een beheerde schijf van de VHD met Powershell, Azure CLI of Azure portal maken
* Implementeer de virtuele machine met een JSON-sjabloon die verwijst naar de VHD-installatiekopie, zoals wordt weergegeven in [deze voorbeeld-JSON-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json) of met behulp van de beheerde schijf zoals wordt weergegeven in [deze voorbeeld-JSON-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/sap-2-tier-user-image-md/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-to-on-premises"></a>Virtuele harde schijven of beheerde schijven downloaden naar on-premises
Azure-infrastructuur als een Service is niet een eenzijdige straat alleen te kunnen uploaden VHD's en SAP systemen. U kunt verplaatsen SAP-systemen vanaf Azure back-ups maken in de on-premises wereld.

Tijdens het ophalen van de download niet kan de virtuele harde schijven of Managed Disks actief zijn. Zelfs bij het downloaden van de schijven die zijn gekoppeld aan virtuele machines, moet de virtuele machine worden afgesloten en toewijzing ongedaan gemaakt. Als u wilt dat alleen voor het downloaden van de database-inhoud die moet vervolgens worden gebruikt voor het instellen van een nieuw systeem on-premises en, indien gewenst die tijdens de tijd van het downloaden en de instellingen van het nieuwe systeem dat het systeem in Azure kunt nog steeds operationeel zijn , u kunt dan een lang uitvaltijd te voorkomen door het uitvoeren van een gecomprimeerde databaseback-up naar een schijf en alleen downloaden die schijf in plaats van de virtuele machine voor de basis van OS ook downloaden.

#### <a name="powershell"></a>PowerShell

  * Een beheerde schijf downloaden  
  U moet eerst toegang krijgen tot de onderliggende blob van de beheerde schijf. Vervolgens kunt u de onderliggende blob kopiëren naar een nieuw opslagaccount en de blob downloaden uit dit storage-account.

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

  * Een VHD downloaden  
  Zodra de SAP-systeem is gestopt en de virtuele machine wordt afgesloten, kunt u de PowerShell-cmdlet opslaan-AzureRmVhd op de on-premises doel voor het downloaden van de VHD-schijven terug naar de on-premises wereld. Als u wilt doen, moet u de URL van de VHD, u in de 'opslag sectie vinden kunt' van de Azure portal (nodig om te navigeren naar het Opslagaccount en de storage-container waar de VHD is gemaakt) en u moet weten waar de VHD moet worden gekopieerd.

  Vervolgens kunt u gebruikmaken van de opdracht door de parameter SourceUri als de URL van de VHD te downloaden en de LocalFilePath als de fysieke locatie van de VHD (inclusief de naam) definiëren. De opdracht kan er als volgt uitzien:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Kom hier voor meer informatie van de cmdlet opslaan-AzureRmVhd <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="azure-cli"></a>Azure-CLI
  * Een beheerde schijf downloaden  
  U moet eerst toegang krijgen tot de onderliggende blob van de beheerde schijf. Vervolgens kunt u de onderliggende blob kopiëren naar een nieuw opslagaccount en de blob downloaden uit dit storage-account.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Een VHD downloaden   
  Zodra de SAP-systeem is gestopt en de virtuele machine wordt afgesloten, kunt u de Azure CLI-opdracht _azure storage blob download_ op de on-premises doel voor het downloaden van de VHD back-schijven op de on-premises wereld. Als u wilt doen, moet u de naam en de container van de VHD die u kunt vinden in de sectie van het opslag van Azure portal (nodig om te navigeren naar het Opslagaccount en de storage-container waar de VHD is gemaakt) en u wilt weten waar de VHD moet kopiëren kopieerd aan.

  Vervolgens kunt u gebruikmaken van de opdracht door het definiëren van de parameters-blobs en containers van de VHD te downloaden en het doel als de fysieke locatie van de VHD (inclusief de naam). De opdracht kan er als volgt uitzien:

  ```
  az storage blob download --name <name of the VHD to download> --container-name <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --file <destination of the VHD to download>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Overdracht van virtuele machines en schijven in Azure

#### <a name="copying-sap-systems-within-azure"></a>Kopiëren van SAP-systemen in Azure

Een SAP-systeem of zelfs een speciale DBMS-server ondersteunt een SAP-toepassingslaag zal waarschijnlijk bestaan uit meerdere schijven, die het besturingssysteem met de binaire bestanden of de gegevens en logboekbestanden bestanden van de SAP-database bevatten. De functionaliteit van Azure kopiëren van schijven noch de functionaliteit van Azure schijven op te slaan op een lokale schijf heeft een mechanisme voor synchronisatie, welke momentopnamen meerdere schijven in een consistente manier. Daarom is de status van de schijven die zijn gekopieerd of opgeslagen, zelfs als die zijn gekoppeld op basis van dezelfde virtuele machine anders. Dit betekent dat in het geval concrete van verschillende gegevens en logfile(s) die deel uitmaken van de verschillende schijven met de database in de end inconsistent zijn zou.

**Conclusie: Als u wilt kopiëren of opslaan van schijven die deel van de configuratie van een SAP-systeem uitmaken moet u stoppen van de SAP-systeem en moet ook de geïmplementeerde virtuele machine af. U kunt alleen vervolgens kopiëren of downloaden van de set van schijven kunt u een kopie van de SAP-systeem maken in Azure of on-premises.**

Gegevensschijven kunnen worden opgeslagen als VHD-bestanden in een Azure Storage-Account en rechtstreeks kunnen worden gekoppeld aan een virtuele machine of worden gebruikt als een afbeelding. In dit geval wordt de VHD gekopieerd naar een andere locatie voordat het wordt gekoppeld aan de virtuele machine. De volledige naam van het VHD-bestand in Azure moet uniek zijn binnen Azure. Zoals al eerder vermeld, is de naam van de soort in het geval van een driedelige-naam die lijkt op:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Gegevensschijven mogen ook Managed Disks. In dit geval wordt de beheerde schijf gebruikt om te maken van een nieuwe beheerde schijf voordat het wordt gekoppeld aan de virtuele machine. De naam van de beheerde schijf moet uniek zijn binnen een resourcegroep.

##### <a name="powershell"></a>PowerShell

U kunt Azure PowerShell-cmdlets gebruiken om te kopiëren van een VHD, zoals wordt weergegeven in [in dit artikel][storage-powershell-guide-full-copy-vhd]. Voor het maken van een nieuwe beheerde schijf gebruikt u New-AzureRmDiskConfig en New-AzureRmDisk zoals wordt weergegeven in het volgende voorbeeld.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="azure-cli"></a>Azure-CLI

U kunt Azure CLI gebruiken om te kopiëren van een VHD, zoals wordt weergegeven in [in dit artikel][storage-azure-cli-copy-blobs]. Gebruik voor het maken van een nieuwe beheerde schijf *az schijf maken* zoals wordt weergegeven in het volgende voorbeeld.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Azure Storage-hulpprogramma 's

* <http://storageexplorer.com/>

Professionele edities van Azure Storage Explorers vindt u hier:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

De kopie van een VHD zelf in een storage-account is een proces waarbij slechts een paar seconden (vergelijkbaar met het maken van momentopnamen met langzaam exemplaar en exemplaar bij schrijfbewerking in de SAN-hardware). Nadat u een kopie van het VHD-bestand hebt, kunt u deze koppelen aan een virtuele machine of gebruiken als een installatiekopie van een kopie van de VHD koppelen aan virtuele machines.

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
##### <a name="azure-cli"></a>Azure-CLI

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
Deze taak kan niet worden uitgevoerd op de Azure-portal. U kunt Azure PowerShell-cmdlets, Azure CLI of de browser van een externe opslag gebruiken. De PowerShell-cmdlets of de CLI-opdrachten kunnen maken en beheren van blobs, waaronder de mogelijkheid om te blobs asynchroon kopiëren tussen Opslagaccounts en regio's binnen het Azure-abonnement.

##### <a name="powershell"></a>PowerShell
U kunt ook de VHD's kopiëren tussen abonnementen. Lees voor meer informatie, [in dit artikel][storage-powershell-guide-full-copy-vhd].

De basisprincipes van de logica van de cmdlet PS ziet er als volgt:

* Maken van de context van een opslagaccount voor de **bron** storage-account met *New-AzureStorageContext* -Zie <https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext>
* Maken van de context van een opslagaccount voor de **doel** storage-account met *New-AzureStorageContext* -Zie <https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext>
* Het exemplaar bij starten

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Controleer de status van de kopie in een lus met

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* De nieuwe VHD koppelen aan een virtuele machine, zoals hierboven is beschreven.

Voor Zie voor voorbeelden [in dit artikel][storage-powershell-guide-full-copy-vhd].

##### <a name="azure-cli"></a>Azure-CLI
* Het exemplaar bij starten

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Controleer de status als het exemplaar nog steeds in een lus met is

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* De nieuwe VHD koppelen aan een virtuele machine, zoals hierboven is beschreven.

Voor Zie voor voorbeelden [in dit artikel][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Afhandeling van schijf

#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Structuur van een virtuele machine/schijf voor SAP-implementaties

De verwerking van de structuur van een virtuele machine en de bijbehorende schijven moet in het ideale geval eenvoudig. In on-premises installaties ontwikkeld klanten veel manieren van het structureren van een server-installatie.

* Een basisschijf, die het besturingssysteem en alle binaire bestanden van de DBMS-systemen en/of SAP bevat. Sinds maart 2015, kan deze schijf tot 1TB in grootte in plaats van eerdere beperkingen die maximaal 127 GB zijn.
* Een of meerdere schijven, waarin de DBMS-logboekbestand van de SAP-database en het logboekbestand van het gebied van de tijdelijke opslag DBMS-systemen (als de DBMS-systemen wordt ondersteund). Als de vereisten voor de database log IOP's hoog zijn, moet u meerdere schijven stripe om te kunnen bereiken van het IOPS-volume dat is vereist.
* Een getal van schijven met een of twee databasebestanden van de SAP-database en de DBMS tijdelijke bestanden ook (als de DBMS-systemen wordt ondersteund).

![Naslaginformatie over configuratie van Azure IaaS-VM voor SAP][planning-guide-figure-1300]


- - -
> ![Windows][Logo_Windows] Windows
>
> Met veel klanten zagen we configuraties, bijvoorbeeld SAP en DBMS binaire bestanden zijn niet geïnstalleerd op het station c:\ waar het besturingssysteem is geïnstalleerd. Er zijn diverse redenen hiervoor, maar als we teruggaan naar de hoofdmap, het meestal was dat de stations kleine zijn en upgrades voor het besturingssysteem nodig extra ruimte 10-15 jaar geleden. Beide voorwaarden niet te vaak meer tegenwoordig van toepassing. Het station c:\ kan vandaag nog op schijven met grote volumes of VM's worden toegewezen. Als u wilt behouden implementaties eenvoudig in de structuur ervan, verdient het volgen van de volgende implementatie-patroon voor SAP NetWeaver-systemen in Azure
>
> Het wisselbestand voor Windows-besturingssysteem moet zich op de D:-schijf (niet-permanente schijf)
>
> ![Linux][Logo_Linux] Linux
>
> De Linux-wisselbestand onder mnt/mnt/resource plaats op Linux, zoals beschreven in [in dit artikel][virtual-machines-linux-agent-user-guide]. Het wisselbestand kan worden geconfigureerd in het configuratiebestand van de /etc/waagent.conf Linux-Agent. Toevoegen of wijzigen van de volgende instellingen:
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

Lees de SAP-notitie [1597355] voor meer informatie over de grootte van de aanbevolen wisselbestand

- - -
Het aantal schijven dat wordt gebruikt voor de DBMS-gegevensbestanden en het type van deze schijven worden gehost op Azure-opslag moet worden bepaald door de IOPS-vereisten en de latentie is vereist. Exacte quota zijn beschreven in [in dit artikel (Linux)] [ virtual-machines-sizes-linux] en [in dit artikel (Windows)][virtual-machines-sizes-windows].

Ervaring van de SAP-implementaties in de afgelopen twee jaar ons sommige lessen die kunnen worden samengevat als veel geleerd:

* IOPS-verkeer naar verschillende gegevensbestanden is niet altijd hetzelfde omdat bestaande systemen kunnen hebben anders grote gegevensbestanden die hun SAP-database (s) vertegenwoordigt. Als gevolg hiervan blijkt te worden beter met behulp van een RAID-configuratie via meerdere schijven te plaatsen van de LUN's oppervlaktevariaties buiten de gegevensbestanden. Er zijn situaties, met name met Azure Standard-opslag waar een IOPS-snelheid bereikt voor het quotum van één schijf op basis van het transactielogboek DBMS-systemen. Het gebruik van Premium Storage wordt aanbevolen in dergelijke scenario's of schijven met een software-stripe ook aggregeren van meerdere Standard-opslag.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [Aanbevolen procedures voor prestaties voor SQL Server in Azure Virtual Machines][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Software-RAID op Linux configureren][virtual-machines-linux-configure-raid]
> * [LVM configureren op een virtuele Linux-machine in Azure][virtual-machines-linux-configure-lvm]
> * [Geheimen met Azure Storage- en Linux i/o-optimalisatie](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Premium Storage is aanzienlijk betere prestaties, met name voor kritieke transaction logboekschrijfbewerkingen worden weergegeven. Voor SAP-scenario's die naar verwachting productie-achtige prestaties leveren, is het raadzaam gebruik van VM-serie die gebruikmaken van Azure Premium Storage.

Houd er rekening mee dat de schijf die het besturingssysteem bevat, en wanneer is het raadzaam, de binaire bestanden van SAP en de database (basis-VM), niet meer beperkt tot 127GB is. Het kan nu tot 1TB in grootte hebben. Dit moet voldoende ruimte voor het bewaren van het vereiste bestand bijvoorbeeld, waaronder SAP-logboeken voor batch-taak.

Voor nog meer suggesties en meer details, specifiek voor de DBMS-VM's, moet u de [DBMS-Implementatiehandleiding][dbms-guide]

#### <a name="disk-handling"></a>Afhandeling van schijf

In de meeste gevallen moet u extra schijven maken als u wilt implementeren de SAP-database in de virtuele machine. We hebben het gehad over de overwegingen voor het aantal schijven in het hoofdstuk [structuur van een virtuele machine/schijf voor SAP-implementaties] [ planning-guide-5.5.1] van dit document. De Azure-portal kunt koppelen en loskoppelen van schijven, wanneer een basis-VM is geïmplementeerd. De schijven kunnen worden gekoppeld/losgekoppeld wanneer de virtuele machine actief is en wordt uitgevoerd en wanneer deze is gestopt. Bij het toevoegen van een schijf, wordt de Azure-portal biedt een lege schijf of een bestaande schijf, die op dit moment is niet gekoppeld aan een andere virtuele machine koppelen.

**Houd er rekening mee**: schijven kunnen alleen worden gekoppeld aan één virtuele machine op een bepaald moment.

![Koppelen / loskoppelen van schijven met Azure Standard-opslag][planning-guide-figure-1400]

Tijdens de implementatie van een nieuwe virtuele machine, kunt u beslissen of u wilt gebruikmaken van beheerde schijven uit of plaats de schijven op Azure Storage-Accounts. Als u gebruikmaken van Premium Storage wilt, wordt u aangeraden Managed Disks.

Vervolgens moet u beslissen of u wilt maken van een nieuwe en lege schijf of dat u wilt selecteren van een bestaande schijf die eerder is geüpload en nu met de virtuele machine moet worden gekoppeld.

**BELANGRIJKE**: U **niet** wilt Host opslaan in cache gebruiken met Azure Standard-opslag. U moet de Host Cache preference laat op het standaardaantal op None. Met Azure Premium Storage, moet u inschakelen Caching van lezen als de i/o-kenmerk voornamelijk, zoals typische i/o-verkeer op basis van de gegevensbestanden van de database lezen is. In het geval van logboekbestand voor database-transactie, wordt geen caching aanbevolen.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Hoe u een gegevensschijf koppelen in Azure portal][virtual-machines-linux-attach-disk-portal]
>
> Als de schijven zijn gekoppeld, moet u zich aanmeldt bij de virtuele machine te openen van de Windows-Disk Manager. Als automount niet is ingeschakeld zoals aanbevolen in hoofdstuk [instellen automount voor gekoppelde schijven][planning-guide-5.5.3], de zojuist gekoppelde volume moet worden ondernomen online en geïnitialiseerd.
>
> ![Linux][Logo_Linux] Linux
>
> Als de schijven zijn gekoppeld, moet u zich aanmelden bij de virtuele machine en het initialiseren van de schijven, zoals beschreven in [in dit artikel][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Als de nieuwe schijf een lege schijf is, moet u ook de schijf wilt formatteren. Voor de opmaak, met name voor DBMS gegevens en logboekbestanden de dezelfde als voor bare-metal implementaties van de DBMS gelden de aanbevelingen.

Zoals vermeld in het hoofdstuk [het Concept van Microsoft Azure Virtual Machine][planning-guide-3.2], een Azure Storage-account biedt geen oneindige resources in termen van i/o-volume, IOPS en gegevensvolume. DBMS-VM's worden meestal meest beïnvloed door dit. Het is mogelijk beter gebruik van een afzonderlijk Opslagaccount voor elke virtuele machine hebt u enkele hoge i/o-volume virtuele machines te implementeren om te kunnen blijven binnen de grenzen van het volume van de Azure Storage-Account. Anders moet u om te zien hoe u deze virtuele machines tussen verschillende opslagaccounts zonder te maken met de limiet van elk één Opslagaccount kunt verdelen. Meer details worden beschreven de [DBMS-Implementatiehandleiding][dbms-guide]. U moet ook rekening houden deze beperkingen voor pure SAP-toepassing server-VM's of andere virtuele machines, die uiteindelijk meer VHD's vereisen kunnen. Deze beperkingen niet van toepassing als u een beheerde schijf gebruiken. Als u van plan bent te gebruiken Premium-opslag, raden wij met behulp van de beheerde schijf.

Een ander onderwerp, dat relevant is voor Storage-Accounts is of de VHD's in een Storage-Account krijgen Geo-replicatie. Geo-replicatie is ingeschakeld of uitgeschakeld op het niveau van de Storage-Account en niet op het niveau van de virtuele machine. Als geo-replicatie is ingeschakeld, zou de VHD's binnen het Opslagaccount in een andere Azure-datacenter in dezelfde regio worden gerepliceerd. Voordat u besluit op deze, moet u doen met de volgende beperkingen:

Azure Geo-replicatie werkt lokaal op elke VHD in een virtuele machine en wordt de IOs in chronologische volgorde niet gerepliceerd tussen meerdere VHD's in een virtuele machine. Daarom worden de VHD die de basis-VM vertegenwoordigt, evenals de extra virtuele harde schijven die zijn gekoppeld aan de virtuele machine gerepliceerd onafhankelijk van elkaar. Dit betekent dat er geen synchronisatie tussen de wijzigingen in de verschillende VHD's. Het feit dat het bestand IOs onafhankelijk van de volgorde waarin deze manier worden geschreven worden gerepliceerd die geo-replicatie is niet van de waarde voor de database-servers waarop hun databases die zijn verdeeld over meerdere VHD's. Naast het DBMS, er mogelijk ook andere toepassingen waar processen schrijven of gegevens in verschillende VHD's en waar het is belangrijk dat u de volgorde van de wijzigingen te manipuleren. Als dat is vereist, moet geo-replicatie in Azure niet worden ingeschakeld. Afhankelijk van of u nodig hebt of wilt geo-replicatie voor een set van virtuele machines, maar niet voor een andere set, kunt u al categoriseren virtuele machines en hun verwante VHD's in verschillende Opslagaccounts waarvoor geo-replicatie ingeschakeld of uitgeschakeld.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Automount voor gekoppelde schijven instellen
- - -
> ![Windows][Logo_Windows] Windows
>
> Voor virtuele machines die worden gemaakt uit eigen installatiekopieën of schijven, is het nodig zijn om te controleren en stel de parameter automount mogelijk in. Als u deze parameter kunt de virtuele machine na een herstart of het opnieuw implementeren in Azure opnieuw automatisch koppelen van de stations die zijn gekoppeld/gekoppeld.
> De parameter is ingesteld voor de installatiekopieën die worden aangeboden door Microsoft in de Azure Marketplace.
>
> Raadpleeg de documentatie van hier het opdrachtregelprogramma uitvoerbare diskpart.exe om in te stellen de automount:
>
> * [DiskPart opdrachtregelopties](https://technet.microsoft.com/library/bb490893.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> Het opdrachtregelvenster Windows moet worden geopend als beheerder.
>
> Als de schijven zijn gekoppeld, moet u zich aanmeldt bij de virtuele machine te openen van de Windows-Disk Manager. Als automount niet is ingeschakeld zoals aanbevolen in hoofdstuk [instellen automount voor gekoppelde schijven][planning-guide-5.5.3], wordt de zojuist gekoppelde volume > moet worden ondernomen online en geïnitialiseerd.
>
> ![Linux][Logo_Linux] Linux
>
> U moet een zojuist gekoppelde lege schijf initialiseren, zoals beschreven in [in dit artikel][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> U moet ook het toevoegen van nieuwe schijven aan de/etc/fstab.
>
>

- - -
### <a name="final-deployment"></a>Laatste implementatie

Voor de laatste implementatie en de exacte stappen, met name met betrekking tot de implementatie van SAP uitgebreide controle, naar verwijzen de [Deployment Guide][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Toegang tot de SAP-systemen binnen Azure-VM 's

Voor scenario's alleen in de Cloud, kunt u verbinding maken met de SAP-systemen via het openbare internet met SAP-gebruikersinterface. Voor deze gevallen moeten de volgende procedures worden toegepast.

Verderop in dit document bespreken we de andere belangrijke scenario's, verbinding maken met SAP-systemen in cross-premises implementaties waarvoor een site-naar-site-verbinding (VPN-tunnel) of Azure ExpressRoute-verbinding tussen de on-premises systemen en Azure-systemen.

### <a name="remote-access-to-sap-systems"></a>Externe toegang tot de SAP-systemen

Met Azure Resource Manager, zijn er geen Standaardeindpunten meer, zoals in het vorige klassieke model. Alle poorten van een Azure Resource Manager VM zijn geopend als:

1. Er is geen Netwerkbeveiligingsgroep is gedefinieerd voor het subnet of de netwerkinterface. Netwerkverkeer naar virtuele Azure-machines kan worden beveiligd via zogeheten 'Network Security Groups'. Zie voor meer informatie, [wat is er een Netwerkbeveiligingsgroep (NSG)?][virtual-networks-nsg]
2. Er is geen Azure Load Balancer is gedefinieerd voor de netwerkinterface   

Zie de architectuur verschil tussen het klassieke model en ARM, zoals beschreven in [in dit artikel][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Configuratie van de SAP-systeem- en SAP-GUI-connectiviteit voor Cloud-Only-scenario

Raadpleeg dit artikel voor meer informatie in dit onderwerp wordt beschreven: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Firewall-instellingen in de virtuele machine wijzigen

Kan het nodig zijn de firewall te configureren op uw virtuele machines waarmee inkomend verkeer naar uw SAP-systeem.

- - -
> ![Windows][Logo_Windows] Windows
>
> De Windows Firewall in een Azure geïmplementeerde VM is standaard ingeschakeld. U nu wilt toestaan dat de SAP-poort moet worden geopend, anders de SAP-gebruikersinterface is niet mogelijk om verbinding te maken.
> Om dit te doen:
>
> * Configuratiescherm\systeem en beveiliging\windows Firewall te openen **geavanceerde instellingen**.
> * Nu met de rechtermuisknop op de regels voor binnenkomende verbindingen en ervoor kiest **nieuwe regel**.
> * In de volgende Wizard hebt gekozen voor het maken een nieuwe **poort** regel.
> * In de volgende stap van de wizard, laat u de instelling op TCP- en typ in het poortnummer dat u wilt openen. Sinds onze SAP-exemplaar-ID 00 is, werd 3200. Als uw exemplaar van een ander exemplaar heeft, kan de poort die u hebt gedefinieerd eerder op basis van het exemplaar dat moet worden geopend.
> * In het volgende gedeelte van de wizard, moet u het item laten **verbinding toestaan** gecontroleerd.
> * In de volgende stap van de wizard moet u bepalen of de regel van toepassing voor Domain, Private en openbare netwerk is. Wijzig deze indien nodig aan uw behoeften. Echter, verbinding maakt met SAP GUI van buitenaf via het openbare netwerk, moet u de regel toegepast op het openbare netwerk.
> * In de laatste stap van de wizard, geef de regel een naam en sla door te drukken **voltooien**.
>
> De regel wordt onmiddellijk van kracht.
>
> ![De regeldefinitie poort][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> De Linux-installatiekopieën in de Azure Marketplace niet de firewall iptables standaard inschakelt en de verbinding met uw SAP-systeem werkt. Als u iptables of een andere firewall hebt ingeschakeld, raadpleegt u de documentatie van iptables of de firewall gebruikt om toe te staan van inkomende tcp-verkeer naar poort 32xx (xx is het systeemnummer van uw SAP-systeem).
>
>

- - -
#### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

De SAP-gebruikersinterface verbinding geen direct met een van de SAP-instanties (poort 32xx) die worden uitgevoerd, maar eerst verbinding maakt via de poort geopend op het serverproces van de SAP-bericht (poort 36xx). In het verleden is dezelfde poort door de berichtenserver gebruikt voor de interne communicatie met de exemplaren van de toepassing. Om te voorkomen dat on-premises-toepassingsservers per ongeluk communiceert met een berichtenserver in Azure, kunnen de interne communicatie-poorten worden gewijzigd. Het is raadzaam om te wijzigen van de interne communicatie tussen de SAP-berichtenserver en de exemplaren van een toepassing naar een ander poortnummer op systemen die zijn is gekloond vanuit on-premises systemen, zoals een kloon van de ontwikkeling van het project testen enzovoort. Dit kan worden gedaan met de standaardparameter-profiel:

> rdisp/msserv_internal
>
>

zoals beschreven in [beveiligingsinstellingen voor de SAP-berichtenserver ](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Concepten van Cloud-Only-implementatie van SAP-instanties

### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Één virtuele machine met SAP NetWeaver demo/training scenario

![Met één VM-SAP-demo-systemen met dezelfde namen van virtuele machine, geïsoleerd in Azure Cloud Services][planning-guide-figure-1700]

In dit scenario (Zie hoofdstuk [alleen in de Cloud] [ planning-guide-2.1] van dit document) we bij het implementeren van een typische training/demo system-scenario waarbij het volledige training/demo-scenario is opgenomen in een enkele virtuele machine. Gaan we ervan uit dat de implementatie vindt plaats via VM image-sjablonen. We ook wordt ervan uitgegaan dat meerdere van deze demo/u kunt virtuele machines moeten worden geïmplementeerd met de virtuele machines met dezelfde naam.

De veronderstelling is dat u een VM-installatiekopie gemaakt zoals beschreven in bepaalde secties van hoofdstuk [virtuele machines met SAP voor Azure voorbereiden] [ planning-guide-5.2] in dit document.

De volgorde van gebeurtenissen voor het implementeren van het scenario ziet er zo uit:

##### <a name="powershell"></a>PowerShell

* Maak een nieuwe resourcegroep voor elke liggend training/demo

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Een nieuw opslagaccount maken als u niet wilt gebruikmaken van beheerde schijven

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Maak een nieuw virtueel netwerk voor elke liggend training/demo van het gebruik van dezelfde hostnaam en IP-adressen in te schakelen. Het virtuele netwerk wordt beveiligd door een Netwerkbeveiligingsgroep waarmee alleen verkeer op poort 3389 voor extern bureaublad-toegang en poort 22 voor SSH.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Maak een nieuw openbaar IP-adres dat kan worden gebruikt voor toegang tot de virtuele machine via internet

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Een nieuwe netwerkinterface voor de virtuele machine maken

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Hiermee maakt u een virtuele machine. Elke virtuele machine hebben dezelfde naam voor het scenario met alleen-Cloud. De SAP-SID van de SAP NetWeaver-exemplaren in deze virtuele machines zijn hetzelfde als goed. De naam van de virtuele machine moet uniek zijn binnen de Azure-resourcegroep, maar in verschillende Azure-resourcegroepen kunt u virtuele machines uitvoeren met dezelfde naam. Het standaardaccount voor 'Administrator' van het Windows- of 'root' voor Linux zijn niet geldig. Een nieuwe gebruikersnaam van beheerder moet daarom, samen met een wachtwoord worden gedefinieerd. De grootte van de virtuele machine moet ook worden gedefinieerd.

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

* (Optioneel) Voeg meer schijven toe en herstellen van benodigde inhoud. Alle blob-namen (URL's voor de blobs) moeten uniek zijn binnen Azure.

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

De volgende voorbeeldcode kan worden gebruikt in Linux. Voor Windows, met behulp van PowerShell, zoals hierboven beschreven of aan te passen in het voorbeeld voor het gebruik van % rgName % in plaats van $rgName en stel de omgevingsvariabele met de opdracht Windows *ingesteld*.

* Maak een nieuwe resourcegroep voor elke liggend training/demo

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Een nieuw opslagaccount maken

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Maak een nieuw virtueel netwerk voor elke liggend training/demo van het gebruik van dezelfde hostnaam en IP-adressen in te schakelen. Het virtuele netwerk wordt beveiligd door een Netwerkbeveiligingsgroep waarmee alleen verkeer op poort 3389 voor extern bureaublad-toegang en poort 22 voor SSH.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Maak een nieuw openbaar IP-adres dat kan worden gebruikt voor toegang tot de virtuele machine via internet

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Een nieuwe netwerkinterface voor de virtuele machine maken

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Hiermee maakt u een virtuele machine. Elke virtuele machine hebben dezelfde naam voor het scenario met alleen-Cloud. De SAP-SID van de SAP NetWeaver-exemplaren in deze virtuele machines zijn hetzelfde als goed. De naam van de virtuele machine moet uniek zijn binnen de Azure-resourcegroep, maar in verschillende Azure-resourcegroepen kunt u virtuele machines uitvoeren met dezelfde naam. Het standaardaccount voor 'Administrator' van het Windows- of 'root' voor Linux zijn niet geldig. Een nieuwe gebruikersnaam van beheerder moet daarom, samen met een wachtwoord worden gedefinieerd. De grootte van de virtuele machine moet ook worden gedefinieerd.

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

* (Optioneel) Voeg meer schijven toe en herstellen van benodigde inhoud. Alle blob-namen (URL's voor de blobs) moeten uniek zijn binnen Azure.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
```

##### <a name="template"></a>Template

U kunt de voorbeeldsjablonen gebruiken in de azure-quickstart-templates opslagplaats op github.

* [Eenvoudige Linux-VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Eenvoudige Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [Virtuele machine uit de afbeelding](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-that-communicate-within-azure"></a>Een set van virtuele machines die in Azure communiceren implementeren

In dit scenario alleen in de Cloud een typisch scenario voor training en demo is bedoeld waar de software voor de demo/training scenario is verspreid over meerdere virtuele machines. De verschillende onderdelen geïnstalleerd in de verschillende VM's nodig hebt om te communiceren met elkaar. Nogmaals, in dit scenario geen on-premises netwerkcommunicatie of cross-premises scenario nodig is.

In dit scenario is een uitbreiding van de installatie wordt beschreven in het hoofdstuk [één virtuele machine met SAP NetWeaver demo/training scenario] [ planning-guide-7.1] van dit document. In dit geval wordt meer virtuele machines worden toegevoegd aan een bestaande resourcegroep. In het volgende voorbeeld wordt het landschap van training bestaat uit een SAP ASCS/SCS-VM, een virtuele machine met een DBMS-systemen, en een exemplaar van SAP-toepassingsserver VM.

Voordat u dit scenario maakt, moet u om na te denken over de basisinstellingen als al in het scenario voor uitgeoefend.

#### <a name="resource-group-and-virtual-machine-naming"></a>Naamgeving van virtuele Machine en resourcegroep

Alle namen van resourcegroepen moeten uniek zijn. Uw eigen schema voor naamgeving van uw resources, zoals ontwikkelen `<rg-name`>-achtervoegsel.

De naam van de virtuele machine moet uniek zijn binnen de resourcegroep.

#### <a name="set-up-network-for-communication-between-the-different-vms"></a>Netwerk instellen voor de communicatie tussen de verschillende virtuele machines

![Set van virtuele machines binnen een virtueel Azure-netwerk][planning-guide-figure-1900]

Om te voorkomen dat naamconflicten met klonen van de dezelfde opleiding/demo-landschappen, die u wilt maken van een Azure-netwerk voor elke Liggend. DNS-naamomzetting wordt geleverd door Azure of u kunt uw eigen DNS-server buiten Azure (niet te worden verder hier besproken) configureren. In dit scenario, doen we onze eigen DNS niet configureren. Voor alle virtuele machines binnen één Azure Virtual Network, worden communicatie via hostnamen ingeschakeld.

De volgende redenen voor het scheiden van training of demo landschappen door virtuele netwerken en niet alleen resourcegroepen kunnen zijn:

* De SAP-landschap zoals ingesteld moet een eigen AD/OpenLDAP en een domein-Server moet deel uitmaken van elk van de landschappen.  
* De SAP-landschap zoals ingesteld bestaat uit onderdelen die nodig hebt om te werken met vaste IP-adressen.

Meer informatie over Azure Virtual Networks en hoe u ze definiëren in vindt [in dit artikel][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Implementatie van SAP-VM's met de zakelijke netwerkverbinding (Cross-Premises)

U uitvoeren van een SAP-landschap en wilt verdelen van de implementatie tussen bare-metal voor geavanceerde DBMS-servers, on-premises gevirtualiseerde omgevingen voor toepassingslagen en kleinere Tier-2 geconfigureerd SAP-systemen en Azure IaaS. De base veronderstelling is dat SAP-systemen binnen één SAP-landschap nodig om te communiceren met elkaar en met veel andere softwareonderdelen geïmplementeerd in het bedrijf, onafhankelijk van de vorm van hun implementatie. Ook moet er worden geen geïntroduceerd door het formulier implementatie voor de eindgebruiker verbinding te maken met SAP-GUI of andere interfaces verschillen. Deze voorwaarden kunnen alleen worden voldaan wanneer we de on-premises Active Directory/OpenLDAP en DNS-services uitgebreid naar de Azure-systemen via site-naar-site/meerdere-on-site verbindings- of particuliere verbindingen, zoals Azure ExpressRoute.

Om meer achtergrondinformatie over de implementatiedetails van SAP op Azure, raden we u aan het lezen van hoofdstuk [concepten van Cloud-Only implementatie van SAP instanties] [ planning-guide-7] van dit document, waarin wordt uitgelegd dat sommige van de concepten van de basisbeginselen van Azure en hoe deze moeten worden gebruikt met SAP-toepassingen in Azure.

### <a name="scenario-of-an-sap-landscape"></a>Scenario van een SAP-landschap

De cross-premises scenario kan grofweg worden beschreven, zoals in de onderstaande afbeeldingen:

![Site-naar-Site-connectiviteit tussen on-premises en Azure-assets][planning-guide-figure-2100]

Het bovenstaande scenario wordt een scenario beschreven waarin de on-premises AD/OpenLDAP en DNS zijn uitgebreid naar Azure. Aan de on-premises, een bepaalde IP-adresbereik is gereserveerd per Azure-abonnement. Het IP-adresbereik wordt toegewezen aan een Azure-netwerk op de Azure-kant.

#### <a name="security-considerations"></a>Beveiligingsoverwegingen

De minimale vereiste is het gebruik van protocollen, zoals SSL/TLS beveiligde communicatie voor toegang via de browser of op basis van een VPN-verbindingen voor toegang tot het systeem tot de Azure-services. Verondersteld wordt dat bedrijven anders de VPN-verbinding tussen hun bedrijfsnetwerk en Azure afgehandeld. Sommige bedrijven mogelijk blankly alle poorten geopend. Sommige andere bedrijven mogelijk nauwkeurig wilt in welke poorten die ze nodig hebben om te openen, enzovoort.

In de tabel onder normale SAP worden communicatiepoorten weergegeven. Het is in feite voldoende om de SAP-gateway-poort te openen.

| Service | Poortnaam | Voorbeeld `<nn`> = 01 | Standaardbereik (min-max.) | Opmerking |
| --- | --- | --- | --- | --- |
| Functie voor berichtverzending |sapdp`<nn>` Zie * |3201 |3200 - 3299 |SAP-Webdispatcher, die worden gebruikt door SAP GUI voor Windows en Java |
| -Berichtenserver |sapms`<sid`> Zie ** |3600 |gratis sapms`<anySID`> |beveiligings-id = SAP-systeem-ID |
| Gateway |sapgw`<nn`> Zie * |3301 |gratis |SAP-gateway, die wordt gebruikt voor CPIC en RFC communicatie |
| SAP-router |sapdp99 |3299 |gratis |Namen van alleen CI (centrale exemplaar)-Service kunnen opnieuw worden toegewezen in /etc/services op een willekeurige waarde na de installatie. |

*) nn = getal voor SAP-exemplaar

*) beveiligings-id = SAP-systeem-ID

Meer gedetailleerde informatie over de poorten die nodig zijn voor verschillende SAP-producten of services van SAP-producten vindt u hier <http://scn.sap.com/docs/DOC-17124>.
Met dit document moet u het volgende kunnen toegewezen poorten openen in de VPN-apparaat die nodig zijn voor specifieke SAP-producten en -scenario's.

Andere beveiligingsmaatregelen bij het implementeren van VM's in een dergelijk scenario mogelijk maken een [Network Security Group] [ virtual-networks-nsg] toegangsregels definiëren.

### <a name="dealing-with-different-virtual-machine-series"></a>Omgaan met andere virtuele Machine-serie

Microsoft toegevoegd veel meer typen van de virtuele machine die in aantal Vcpu, geheugen verschillen of belangrijker op hardware waarop het wordt uitgevoerd. Niet alle deze virtuele machines worden ondersteund met SAP (Zie ondersteunde VM-typen in SAP-notitie [1928533]). Sommige van deze virtuele machines worden uitgevoerd op host verschillende hardwaregeneraties. Deze host hardwaregeneraties worden ophalen geïmplementeerd in de granulatie van een Azure-schaaleenheid. Gevallen ontstaan waarin de verschillende VM-typen die u hebt gekozen op de dezelfde schaaleenheid kunnen niet worden uitgevoerd. Een Beschikbaarheidsset is beperkt in de mogelijkheid om te omvatten schaaleenheden op basis van andere hardware.  Bijvoorbeeld als u de SAP DBMS-laag op een E64s_v3-virtuele machine die zich in een Beschikbaarheidsset, samen met de virtuele machine met het secundaire DBMS-exemplaar in de configuratie van een HA, u kunt geen gewoon stoppen en start opnieuw op de secundaire virtuele machine als de M-serie VM omdat u upg wilt rede de virtuele machine. Reden is dat uit de M-serie VM's en Ev3-serie VM's worden uitgevoerd op andere hardware en met die in verschillende schaaleenheden. U moet een nieuwe Beschikbaarheidsset maken, verwijderen van de secundaire virtuele machine uit de Ev3-serie zonder te verwijderen van de opslag en de virtuele machine opnieuw te implementeren als virtuele machine uit M-serie in de nieuwe Beschikbaarheidsset.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Afdrukken op een lokale netwerkprinter van SAP-instantie in Azure

##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Afdrukken via TCP/IP in Cross-Premises scenario

Instellen van uw on-premises TCP/IP op basis van netwerkprinters in een Azure VM is de algemene hetzelfde als in uw bedrijfsnetwerk, ervan uitgaande dat u hebt een Site-naar-Site VPN-tunnel of ExpressRoute-verbinding tot stand gebracht.

- - -
> ![Windows][Logo_Windows] Windows
>
> Om dit te doen:
>
> * Sommige netwerkprinters worden geleverd met een configuratiewizard Hierdoor is het eenvoudig om in te stellen de printer in een Azure-VM. Als er geen wizardsoftware met de printer is gedistribueerd, wordt de handmatige manier voor het instellen van de printer is het maken van een nieuwe TCP/IP-printerpoort.
> * Open het Configuratiescherm -> apparaten en Printers > een printer toevoegen
> * Kiest u een printer die met behulp van een TCP/IP-adres of de hostnaam toevoegen
> * Typ in het IP-adres van de printer
> * Poort 9100 standard
> * Indien nodig handmatig het juiste stuurprogramma installeren.
>
> ![Linux][Logo_Linux] Linux
>
> * Als voor Windows gewoon de standaardprocedure voor het installeren van een netwerkprinter
> * Volg de openbare Linux-handleidingen voor [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) of [Red Hat en Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) over het toevoegen van een printer.
>
>

- - -
![Afdrukken via het netwerk][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Printer op basis van een host via SMB (gedeelde printer) in de Cross-Premises scenario

Host-gebaseerde printers zijn niet netwerk compatibel standaard. Maar een printer op de host kan worden gedeeld tussen computers in een netwerk, zolang de printer is aangesloten op een computer ingeschakeld. Verbinding maken met uw zakelijke network of ExpressRoute als Site-naar-Site en de lokale printer te delen. Het SMB-protocol maakt gebruik van NetBIOS in plaats van DNS als naamservice. De NetBIOS-naam kan afwijken van de DNS-hostnaam. De standaardsituatie is dat de NetBIOS-hostnaam en de DNS-hostnaam identiek zijn. De DNS-domein is niet verstandig in de NetBIOS-naam-ruimte. De volledig gekwalificeerde DNS-hostnaam van de DNS-hostnaam en DNS-domein moet daarom niet worden gebruikt in de NetBIOS-naam-ruimte.

De share wordt geïdentificeerd door een unieke naam in het netwerk:

* De hostnaam van de SMB-host (altijd nodig).
* De naam van de share (altijd nodig).
* Naam van het domein is als de printer delen in hetzelfde domein als de SAP-systeem niet.
* Bovendien een gebruikersnaam en een wachtwoord vereist voor toegang tot de share.

Procedure:

- - -
> ![Windows][Logo_Windows] Windows
>
> De lokale printer delen.
> Open de Windows Explorer en typt u de sharenaam van de printer in de Azure-VM.
> Een printer-installatiewizard leidt u door het installatieproces.
>
> ![Linux][Logo_Linux] Linux
>
> Hier volgen enkele voorbeelden van documentatie over het configureren van netwerkprinters in Linux of een hoofdstuk waaronder met betrekking tot afdrukken in Linux. Het wordt in een Azure Linux-VM op dezelfde manier werken als de virtuele machine deel uit van een VPN-verbinding maakt:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_(Samba)_Share_or_Windows_Share>
> * RHEL of Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB-Printer (printer doorsturen)

De mogelijkheid van de extern bureaublad-Services voor gebruikers de toegang tot hun lokale printer-apparaten in een externe sessie is niet beschikbaar in Azure.

- - -
> ![Windows][Logo_Windows] Windows
>
> Meer informatie over het afdrukken met Windows vindt u hier: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integratie van SAP-systemen Azure in correctie en -Transport systeem (TMS) in de Cross-Premises

De SAP-wijzigings- en Transport System (TMS) moet worden geconfigureerd om te exporteren en importeren van transport aanvraag alle systemen in de liggende modus. Gaan we ervan uit dat de exemplaren van de ontwikkeling van een SAP-systeem (DEV) bevinden zich in Azure terwijl de kwaliteit te waarborgen (QA) en productieve systemen (PRD) zich on-premises. Bovendien veronderstellen we dat er een centrale transportmap is.

##### <a name="configuring-the-transport-domain"></a>Het Transport-domein configureren

Uw domein Transport configureren op het systeem die u hebt opgegeven als de domeincontroller Transport, zoals beschreven in [configureren van de domeincontroller Transport](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Een systeemgebruiker die TMSADM wordt gemaakt en de vereiste RFC bestemming worden gegenereerd. U kunt deze RFC-verbindingen met behulp van de transactie SM59 controleren. Hostnaamomzetting moet zijn ingeschakeld in uw domein transport.

Procedure:

* In ons scenario besloten we dat de QAS on-premises systeem is de CTS-domeincontroller. Transactie stm aanroepen. Het dialoogvenster TMS wordt weergegeven. Een Transport-domein configureren-dialoogvenster wordt weergegeven. (Dit dialoogvenster wordt alleen weergegeven als u een transport-domein nog niet hebt geconfigureerd.)
* Zorg ervoor dat de automatisch gemaakte TMSADM-gebruiker is gemachtigd (SM59 -> verbinding ABAP -> TMSADM@E61.DOMAIN_E61 -> Details -> Utilities(M) autorisatie Test ->). Het eerste scherm van de transactie STM moet weergeven of deze SAP-systeem nu als de domeincontroller van het domein transport functioneert zoals hier wordt weergegeven:

![Eerste scherm van de transactie stm op de domeincontroller][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Met inbegrip van SAP-systemen in het domein Transport

De volgorde van de met inbegrip van een SAP-systeem in een domein transport ziet er als volgt uit:

* Op het systeem ontwikkelen in Azure, gaat u naar het transport-systeem (Client 000) en roep transactie stm. Kies andere configuratie in het dialoogvenster en doorgaan met het systeem in domein opgenomen. Geef op de domeincontroller als de doelhost ([met inbegrip van SAP-systemen in het domein Transport](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Het systeem is nu nog moeten worden opgenomen in het domein transport.
* Uit veiligheidsoverwegingen hebt u vervolgens gaat u terug naar de domeincontroller om te bevestigen van uw aanvraag. Kies Systeemoverzicht en goedkeuren van het systeem wacht. Bevestig op de opdrachtprompt en de configuratie worden gedistribueerd.

Deze SAP-systeem bevat nu de nodige informatie over alle de andere SAP-systemen in het domein transport. De gegevens van het nieuwe SAP-systeem voor het adres is verzonden naar alle andere SAP-systemen op hetzelfde moment, en de SAP-systeem is ingevoerd in het profiel van het transport van het programma voor het beheer van transport. Controleer of RFC's en toegang tot de transportmap van het domein werken.

Doorgaan met de configuratie van uw systeem transport zoals gebruikelijk zoals beschreven in de documentatie [wijzigings- en Transport System](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Procedure:

* Zorg ervoor dat uw stm on-premises correct is geconfigureerd.
* Zorg ervoor dat de hostnaam van de domeincontroller Transport kan worden opgelost door uw virtuele machine op Azure en andersom visa.
* Aanroep transactie stm -> andere configuratie -> systeem in domein opgenomen.
* Controleer of de verbinding in het on premises TMS-systeem.
* Zoals gewoonlijk transport routes, groepen en lagen configureren.

In de site-naar-site verbonden cross-premises kunnen scenario's, de vertraging tussen het on-premises en Azure nog steeds behoorlijk oplopen. Als we u de volgorde van het vervoer van objecten via ontwikkelings- en systemen voor productie of denken over het toepassen van transporten of van ondersteuningspakketten naar de andere systemen, u Denk eraan dat, afhankelijk van de locatie van de transportmap centrale enkele van de systemen optreden hoge latentie lezen of schrijven van gegevens in de transportmap centrale. De situatie is vergelijkbaar met de configuraties van SAP-landschap dat is waar de verschillende systemen worden verspreid via verschillende datacenters met aanzienlijke afstand tussen de datacentra.

Als u wilt deze latentie omzeilen en de systemen werken snel bij het lezen of schrijven naar of uit de transportmap hebt, kunt u twee stm transport domeinen instellen (één voor on-premises en één met de systemen in Azure en de transport-domeinen te koppelen. Controleer of deze documentatie wordt uitgelegd van de beginselen van dit concept in de SAP-TMS: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>.

Procedure:

* Een transport-domein op elke locatie (on-premises en Azure) instellen met behulp van transactie stm <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* De domeinen te koppelen met de koppeling van een domein en controleer of de koppeling tussen de twee domeinen.
  <http://help.sap.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/content.htm>
* De configuratie van de gekoppelde system distribueren.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC-verkeer tussen SAP-instanties die zich in Azure en on-premises (Cross-Premises)

RFC-verkeer tussen systemen, die zich on-premises en in Azure nodig om te werken. Instellen van een verbinding aanroep transactie SM59 in een bronsysteem waar u nodig hebt voor het definiëren van een RFC-verbinding op het doelsysteem. De configuratie is vergelijkbaar met de standaardinstellingen van een RFC-verbinding.

Er wordt ervan uitgegaan dat in het scenario voor cross-premises, de virtuele machines, die uitvoeren SAP-systemen die nodig hebt om te communiceren met elkaar in hetzelfde domein. De installatie van een RFC-verbinding tussen de SAP-systemen daarom niet verschillen van de installatiestappen uit en de invoer in on-premises scenario's.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Toegang tot lokale bestandsshares van SAP-instanties in Azure of omgekeerd

SAP-instanties die zich in Azure nodig voor toegang tot bestandsshares die zich binnen het bedrijf premises. Bovendien moeten de on-premises SAP-instanties voor toegang tot bestandsshares die in Azure bevinden zich. Als u wilt de bestandsshares hebt ingeschakeld, moet u de machtigingen en delen van de opties op het lokale systeem configureren. Zorg ervoor dat de poorten op de VPN of ExpressRoute-verbinding tussen Azure en uw datacenter te openen.

## <a name="supportability"></a>Ondersteuning

### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure-oplossing voor de controle voor SAP

Als u wilt inschakelen, de bewaking van essentiële SAP bedrijfssystemen op Azure de SAP controlehulpprogramma's SAPOSCOL of SAP Host-Agent gegevens uit de Azure-virtuele Machine ServiceHost via een Azure Monitoring-extensie voor SAP ophalen. Omdat de eisen van SAP specifiek voor SAP-toepassingen zijn, besloten Microsoft niet algemeen implementeren van de vereiste functionaliteit in Azure, maar laat u het voor klanten aan de vereiste onderdelen voor bewaking en configuraties implementeren in hun virtuele De machines die worden uitgevoerd in Azure. Implementatie en levenscyclus van beheer van de onderdelen van de bewaking wordt echter meestal automatisch door Azure.

#### <a name="solution-design"></a>Het ontwerp van oplossing

De oplossing ontwikkeld voor SAP-bewaking is gebaseerd op de architectuur van Azure VM-Agent en Extension framework. Het idee van de Azure VM-Agent en -extensie-framework is zodat de installatie van software-toepassingen die beschikbaar zijn in de galerie met Azure VM-extensie binnen een virtuele machine. Het principe idee achter dit concept is om toe te staan (in gevallen, zoals de Azure Monitoring-extensie voor SAP), de implementatie van speciale functionaliteit in een virtuele machine en de configuratie van dergelijke software tijdens de implementatie.

De 'Azure VM-Agent' waarmee de verwerking van specifieke Azure-VM-extensies in de VM is opgenomen in Windows-VM's standaard bij het maken van virtuele machine in Azure portal. In het geval van SUSE, Red Hat of Oracle Linux is de VM-agent al lid van de Azure Marketplace-installatiekopie. In het geval een van een Linux-VM van on-premises naar Azure uploaden zou is de VM-agent handmatig worden geïnstalleerd.

De elementaire bouwstenen van de oplossing voor bewaking in Azure voor SAP ziet er als volgt:

![Onderdelen van Microsoft Azure-extensie][planning-guide-figure-2400]

Zoals weergegeven in het bovenstaande blokdiagram, wordt een deel van de oplossing voor bewaking voor SAP wordt gehost in de Azure VM-installatiekopie en de Azure-extensie-galerie, dit is een wereldwijd gerepliceerde opslagplaats die wordt beheerd door Azure-bewerkingen. Het is de verantwoordelijkheid van de gezamenlijke SAP/MS-team werkt de Azure-implementatie van SAP om te werken met Azure-bewerkingen voor het publiceren van nieuwe versies van de Azure Monitoring-extensie voor SAP.

Wanneer u een nieuwe Windows-virtuele machine implementeert, wordt de Azure VM-Agent automatisch toegevoegd aan de virtuele machine. De functie van deze agent is voor de coördinatie van het laden en de configuratie van de Azure-extensies voor de bewaking van SAP NetWeaver-systemen. Voor virtuele Linux-machines is de Azure VM-Agent al deel uit van de Azure Marketplace-installatiekopie.

Er is echter een stap die u moet nog steeds worden uitgevoerd door de klant. Dit is de activering en configuratie van de verzameling van prestatiegebeurtenissen. Het proces met betrekking tot de configuratie is geautomatiseerd door een PowerShell-script of de CLI-opdracht. Het PowerShell-script kan worden gedownload in de Microsoft Azure Script Center, zoals beschreven in de [Deployment Guide][deployment-guide].

Lijkt op de algehele architectuur van de Azure monitoring-oplossing voor SAP:

![Azure-oplossing voor de controle voor SAP NetWeaver][planning-guide-figure-2500]

**Voor de exacte instructies en voor gedetailleerde stappen van het gebruik van deze PowerShell-cmdlets of de CLI-opdracht tijdens implementaties, volg de instructies in de [Deployment Guide][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integratie van Azure SAP-instantie in SAProuter zich bevindt

SAP-exemplaren die worden uitgevoerd in Azure moeten toegankelijk zijn vanaf SAProuter ook.

![SAP-Router-netwerkverbinding][planning-guide-figure-2600]

Een SAProuter kunt het TCP/IP-communicatie tussen de deelnemende systemen als er geen directe IP-verbinding. Dit biedt het voordeel dat er geen end-to-end-verbinding tussen de partners voor communicatie nodig op netwerkniveau is. De SAProuter luistert 3299 standaard poort.
Als u wilt verbinding maken met instanties van SAP via een SAProuter moet u de SAProuter tekenreeks en host-naam met elke poging om verbinding te maken.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver-AS-Java

Heeft de focus van het document SAP NetWeaver tot nu toe in het algemeen zijn of de SAP NetWeaver ABAP-stack. In deze sectie klein zijn specifieke aandachtspunten voor de SAP-Java-stack worden weergegeven. Een van de belangrijkste uitsluitend op basis van SAP NetWeaver Java-toepassingen is de SAP-Enterprise-Portal. Andere SAP NetWeaver-toepassingen zoals SAP PI en SAP-oplossing Manager de SAP NetWeaver ABAP en Java-stacks. Er natuurlijk is daarom moet rekening houden met specifieke aspecten met betrekking tot de SAP NetWeaver-Java-stack.

### <a name="sap-enterprise-portal"></a>SAP Enterprise Portal

De installatie van een SAP-Portal in een Azure Virtual machines verschilt niet van een on premises installatie als u in een cross-premises scenario's implementeert. Omdat de DNS-server wordt uitgevoerd door on-premises, kunnen de poortinstellingen van de afzonderlijke instanties zoals geconfigureerde on-premises worden uitgevoerd. De aanbevelingen en de beperkingen die tot nu toe zijn beschreven in dit document voor een toepassing, zoals SAP Enterprise Portal of de SAP NetWeaver-Java-stack in het algemeen gelden.

![Beschikbaar gemaakte SAP-Portal][planning-guide-figure-2700]

Een speciale implementatiescenario door sommige klanten is de rechtstreekse blootstelling van de SAP-Enterprise-Portal met het Internet, terwijl de virtuele-machinehost is verbonden met het bedrijfsnetwerk via site-naar-site VPN-tunnel of ExpressRoute. U hebt voor een scenario om ervoor te zorgen dat bepaalde poorten geopend en niet wordt geblokkeerd door firewall of netwerk beveiligingsgroep zijn. De dezelfde mechanics zou moeten worden toegepast wanneer u verbinding maken met een SAP-Java-exemplaar van wilt on-premises in een scenario alleen in de Cloud.

De eerste portal URI is http (s):`<Portalserver`>: 5XX00/irj waar de poort wordt gevormd door 50000 plus (Systemnumber? 100). Standaard portal URI van de SAP-systeem 00 is `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. Voor meer informatie, hebt u een overzicht van <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Eindpuntconfiguratie][planning-guide-figure-2800]

Als u aanpassen van de URL en/of de poorten van uw SAP-Enterprise-Portal wilt, schakelt u deze documentatie:

* [De URL van de Portal wijzigen](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Standaardpoortnummers, Portal-poortnummers wijzigen](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Hoge beschikbaarheid (HA) en Disaster Recovery (DR) voor SAP NetWeaver op Azure Virtual machines

### <a name="definition-of-terminologies"></a>Definitie van de terminologie

De term **hoge beschikbaarheid (HA)** in het algemeen is gerelateerd aan een reeks technologieën die IT onderbrekingen door te geven van zakelijke continuïteit van IT-services via redundante, fouttolerante, of failover beveiligde onderdelen in de **dezelfde** Datacenter. In ons geval binnen één Azure-regio.

**Herstel na noodgeval (DR)** is ook die gericht is op Minimalisatie van het aantal onderbreking van de IT-services en het herstel, maar meerdere **verschillende** datacenters, die meestal zich honderden kilometers verwijderd. In ons geval meestal tussen verschillende Azure-regio's binnen dezelfde geopolitieke regio of als tot stand gebrachte door u als een klant.

### <a name="overview-of-high-availability"></a>Overzicht van hoge beschikbaarheid

We kunnen scheiden van de discussie over de SAP hoge beschikbaarheid in Azure uit twee delen:

* **Hoge beschikbaarheid van Azure-infrastructuur**, bijvoorbeeld HA van Reken-(VM's), netwerk, opslag enz. en de voordelen voor het verhogen van de beschikbaarheid van SAP-toepassing.
* **Hoge beschikbaarheid van SAP**, zoals hoge beschikbaarheid van SAP-software-onderdelen:
  * SAP-toepassingsservers
  * SAP ASCS/SCS-exemplaar
  * DB-server

en hoe deze kan worden gecombineerd met Azure-infrastructuur HA.

Hoge beschikbaarheid van SAP in Azure heeft enkele verschillen in vergelijking met hoge beschikbaarheid van SAP in een on-premises fysieke of virtuele omgeving. Het volgende document uit SAP worden standaard hoge beschikbaarheid van SAP-configuraties in gevirtualiseerde omgevingen op Windows beschreven: <http://scn.sap.com/docs/DOC-44415>. Er is geen sapinst geïntegreerd SAP-HA configuratie voor Linux als deze voor Windows bestaat. Met betrekking tot SAP HA on-premises voor Linux vindt hier meer informatie: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Hoge beschikbaarheid van Azure-infrastructuur

Er is momenteel een één-VM-SLA van 99,9%. Als u een idee hoe de beschikbaarheid van een enkele virtuele machine als volgt uitzien, kunt u het product van de verschillende beschikbare Sla's voor Azure maken: <https://azure.microsoft.com/support/legal/sla/>.

De basis voor de berekening is 30 dagen per maand of 43200 minuten. Daarom 0,05% downtime komt overeen met 21,6 minuten. Zoals gewoonlijk wordt de beschikbaarheid van de verschillende services vermenigvuldigt u in de volgende manier:

(Beschikbaarheidservice #1/100) * (beschikbaarheidservice #2/100) * (beschikbaarheidservice #3/100) 

Zoals:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 of een algemene beschikbaarheid van 99.75%.

#### <a name="virtual-machine-vm-high-availability"></a>Hoge beschikbaarheid van virtuele Machine (VM)

Er zijn twee soorten gebeurtenissen voor Azure-platform die invloed kunnen hebben op de beschikbaarheid van uw virtuele machines: gepland onderhoud en niet-gepland onderhoud.

* Geplande onderhoudsgebeurtenissen zijn periodieke updates die zijn aangebracht door Microsoft in de onderliggende Azure-platform voor het verbeteren van de algemene betrouwbaarheid, prestaties en beveiliging van de platform-infrastructuur waarop uw virtuele machines worden uitgevoerd.
* Niet-gepland onderhoud optreden wanneer de hardware of de fysieke infrastructuur die uw virtuele machine is mislukt op een bepaalde manier. Voorbeelden hiervan zijn lokale netwerkproblemen, lokale schijfdefecten of andere defecten op rack-niveau. Wanneer een dergelijke fout wordt gedetecteerd, wordt het Azure-platform uw virtuele machine automatisch migreren van de slechte fysieke server die als host fungeert voor uw virtuele machine naar een gezonde fysieke server. Dergelijke gebeurtenissen zijn zeldzaam, maar kunnen er ook toe leiden dat uw virtuele machine opnieuw moet opstarten.

Meer informatie vindt u in deze documentatie: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Azure Storage-redundantie

De gegevens in uw Microsoft Azure Storage-Account worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid, voldoen aan de SLA voor de Azure-opslag zelfs bij tijdelijke hardwarefouten.

Omdat Azure Storage is het houden van drie kopieën van de gegevens standaard, zijn RAID 5 of RAID1 over meerdere Azure-schijven niet nodig.

Meer informatie vindt u in dit artikel: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Met Azure-infrastructuur opnieuw opstarten van virtuele machine voor hogere beschikbaarheid van SAP-toepassingen

Als u besluit geen gebruik van functies, zoals Windows Server Failover Clustering (WSFC) of Pacemaker op Linux te (momenteel alleen ondersteund voor SLES 12 en hoger), Azure-VM opnieuw wordt gebruikt voor het beveiligen van een SAP-systeem op basis van geplande en ongeplande uitval van Azure fysieke server-infrastructuur en algemene onderliggende Azure-platform.

> [!NOTE]
> Het is belangrijk om te vermelden dat voornamelijk Azure virtuele machine opnieuw VM's en niet de toepassingen beschermt. Virtuele machine opnieuw opstarten biedt geen hoge beschikbaarheid voor SAP-toepassingen, maar het biedt een zekere mate van beschikbaarheid van de infrastructuur en daarom indirect hogere beschikbaarheid van SAP-systemen. Er is ook geen SLA voor de tijd die het duurt om opnieuw te starten van een virtuele machine na een storing gepland of ongepland host. Deze methode van maximale beschikbaarheid is daarom niet geschikt voor essentiële onderdelen van een SAP-systeem, zoals (A) SCS- of DBMS-systemen.
>
>

Een ander belangrijk infrastructuur-element voor hoge beschikbaarheid is opslag. SLA voor Azure-opslag is bijvoorbeeld beschikbaarheid van 99,9%. Als er een implementeert alle virtuele machines met de schijven in een enkele Azure Storage-Account, potentiële Azure Storage niet beschikbaar zijn, zullen niet beschikbaar zijn voor alle virtuele machines die worden geplaatst in het Azure Storage-Account en ook alle SAP onderdelen die op deze virtuele machines worden uitgevoerd.  

In plaats van dat alle virtuele machines in één enkele Azure Storage-Account, kunt u ook toegewezen opslag gebruiken accounts voor elke virtuele machine en op deze manier Verhoog de algehele beschikbaarheid voor virtuele machine en SAP-toepassing met behulp van meerdere onafhankelijke Azure Storage-Accounts.

Azure Managed Disks worden automatisch opgenomen in het Foutdomein van de virtuele machine die ze zijn gekoppeld. Als u twee virtuele machines in een beschikbaarheidsset instellen en gebruiken van beheerde schijven, het platform zorgt voor het distribueren van de beheerde schijven in verschillende domeinen met fouten ook. Als u gebruikmaken van Premium Storage wilt, raadzaam zeer schijven beheren en gebruiken.

De voorbeeldarchitectuur van een van een SAP NetWeaver-systeem dat gebruikmaakt van Azure-infrastructuur HA en storage-accounts kan er als volgt:

![Met behulp van Azure-infrastructuur HA voor hogere beschikbaarheid van SAP-toepassing][planning-guide-figure-2900]

De voorbeeldarchitectuur van een van een SAP NetWeaver-systeem die gebruikmaakt van Azure-infrastructuur HA en Managed Disks kan er als volgt:

![Met behulp van Azure-infrastructuur HA voor hogere beschikbaarheid van SAP-toepassing][planning-guide-figure-2901]

Voor essentiële SAP-onderdelen bereikt we het volgende tot nu toe:

* Hoge beschikbaarheid van SAP-toepassingsservers (AS)

  SAP-toepassing server-exemplaren zijn redundante onderdelen. Elk SAP-exemplaar is geïmplementeerd op een eigen virtuele machine, die wordt uitgevoerd in een andere Azure-Foutdomeinen en Upgrade Domain (Zie de hoofdstukken [Foutdomeinen] [ planning-guide-3.2.1] en [domeinen upgraden] [ planning-guide-3.2.2]). Hierdoor wordt ervoor gezorgd dat met behulp van Beschikbaarheidssets van Azure (Zie hoofdstuk [Beschikbaarheidssets van Azure][planning-guide-3.2.3]). Mogelijke gepland of ongepland onbeschikbaarheid van een Azure-probleem of het upgraden van domein zorgt ervoor dat de onbeschikbaarheid van een beperkt aantal VM's met hun SAP-AS exemplaren.

  Elk SAP-als het exemplaar wordt geplaatst in een eigen Azure Storage-account: mogelijke onbeschikbaarheid van een Azure Storage-Account zorgt ervoor dat de onbeschikbaarheid van slechts één virtuele machine met de SAP-AS exemplaar. Let echter dat er een limiet van Azure Storage-Accounts binnen één Azure-abonnement geldt. Om ervoor te zorgen (A) SCS-exemplaar automatisch wordt gestart nadat de virtuele machine opnieuw is opgestart, zorg ervoor dat de parameter automatisch starten om in te stellen (A) SCS-exemplaar dat wordt beschreven in het hoofdstuk profiel starten [Autostart voor SAP-exemplaren met behulp van][planning-guide-11.5].
  Lees ook hoofdstuk [hoge beschikbaarheid voor SAP-toepassingsservers] [ planning-guide-11.4.1] voor meer informatie.

  Zelfs als u Managed Disks gebruikt, worden deze schijven worden ook opgeslagen in een Azure Storage-Account en mag niet beschikbaar in een gebeurtenis van een storing in de opslag.

* *Hogere* beschikbaarheid van SAP (A) SCS-exemplaar

  Hier maken we gebruik van Azure virtuele machine opnieuw beveiligen van de virtuele machine met geïnstalleerde SAP (A) SCS-exemplaar. In het geval van een geplande of niet-geplande uitvaltijd van Azure servers, virtuele machines op een andere beschikbare server opnieuw wordt gestart. Zoals eerder vermeld, beveiligt Azure-VM opnieuw voornamelijk virtuele machines en niet-toepassingen, in dit geval de (A) SCS-exemplaar. Via de virtuele machine opnieuw hebt opgestart vragen we indirect hogere beschikbaarheid van SAP (A) SCS-exemplaar. Om te zorgen dat (A) SCS-exemplaar automatisch wordt gestart nadat de virtuele machine opnieuw is opgestart, zorg ervoor dat de parameter automatisch starten om in te stellen (A) SCS-exemplaar starten profiel dat wordt beschreven in het hoofdstuk [Autostart voor SAP-exemplaren met behulp van][planning-guide-11.5]. Dit betekent dat de (A) SCS-exemplaar als één punt van de fout (SPOF) die worden uitgevoerd in een enkele virtuele machine is de doorslaggevend factor voor de beschikbaarheid van de gehele SAP-landschap.

* *Hogere* beschikbaarheid van DBMS-Server

  Hier, net als bij de SAP (A) SCS-exemplaar use-case, maken we gebruik van Azure virtuele machine opnieuw opstarten om de VM met geïnstalleerde DBMS-software te beveiligen, en we bereiken hogere beschikbaarheid van de software DBMS-systemen via virtuele machine opnieuw opstarten.
  DBMS-systemen die worden uitgevoerd in een enkele virtuele machine is ook een SPOF en is de doorslaggevend factor voor de beschikbaarheid van de gehele SAP-landschap.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP-toepassing voor hoge beschikbaarheid op Azure IaaS

Als u wilt bereiken volledige SAP hoge beschikbaarheid van het systeem, moeten we alle essentiële SAP-systeemonderdelen, voor het voorbeeld redundante SAP-toepassingsservers en unieke onderdelen (bijvoorbeeld één punt van de fout), zoals SAP (A) SCS-exemplaar en DBMS-systemen te beschermen.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Hoge beschikbaarheid voor SAP-toepassingsservers

Voor de SAP-servers/dialoogvenster toepassingsexemplaren is het niet nodig om na te denken over een oplossing voor specifieke hoge beschikbaarheid. Hoge beschikbaarheid wordt bereikt door redundantie en op die manier met voldoende hiervan in verschillende virtuele machines. Ze moeten allemaal worden geplaatst in de dezelfde Azure-Beschikbaarheidsset om te voorkomen dat de virtuele machines op hetzelfde moment tijdens gepland onderhoud uitval kunnen worden bijgewerkt. De basisfunctionaliteit, die is gebaseerd op verschillende Upgrade en domeinen met fouten binnen een Azure-schaaleenheid al werd geïntroduceerd in hoofdstuk [domeinen upgraden][planning-guide-3.2.2]. Azure-Beschikbaarheidssets zijn die zijn gepresenteerd in hoofdstuk [Beschikbaarheidssets van Azure] [ planning-guide-3.2.3] van dit document.

Er is geen oneindig aantal Foutdomeinen en Upgrade-domeinen die kunnen worden gebruikt door een Azure-Beschikbaarheidsset binnen een Azure-schaaleenheid. Dit betekent dat als een aantal virtuele machines in een Beschikbaarheidsset, eerder of later meer dan één die VM-ends van in dezelfde fout of het upgraden van domein.

Enkele SAP application server-exemplaren in hun toegewezen virtuele machines implementeert en ervan uitgaande dat er vijf Upgrade-domeinen is, verschijnt de volgende afbeelding aan het einde. De werkelijke maximale aantal fout- en updatedomeinen in een beschikbaarheidsset in de toekomst mogelijk wijzigen:

![Hoge beschikbaarheid van SAP-toepassingsservers in Azure][planning-guide-figure-3000]

Meer informatie vindt u in deze documentatie: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-sap-central-services-on-azure"></a>Hoge beschikbaarheid voor SAP Central Services op Azure

Raadpleeg het artikel voor de architectuur voor hoge beschikbaarheid van SAP Central Services op Azure, [architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) als informatie invoeren. Het artikel verwijst naar meer gedetailleerde beschrijvingen voor de specifieke besturingssystemen.

#### <a name="high-availability-for-the-sap-database-instance"></a>Hoge beschikbaarheid voor de SAP-database-instantie

De typische SAP DBMS maximaal beschikbare installatie is gebaseerd op twee DBMS-VM's, waar de DBMS-functionaliteit voor hoge beschikbaarheid wordt gebruikt om gegevens te repliceren van de actieve DBMS-instantie met de tweede virtuele machine in een passieve DBMS-instantie.

Herstel-functionaliteit voor hoge beschikbaarheid en noodherstel voor DBMS in het algemeen ook zo specifiek DBMS-systemen worden beschreven in de [DBMS-Implementatiehandleiding][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Hoge beschikbaarheid van de end-to-End voor het Complete SAP-systeem

Hier volgen twee voorbeelden van een complete SAP NetWeaver HA-architectuur in Azure - één voor Windows en één voor Linux.

Niet-beheerde schijven alleen: de concepten zoals hieronder wordt uitgelegd mogelijk iets is wanneer u veel SAP-systemen implementeert en het aantal virtuele machines geïmplementeerd de maximumlimiet van Storage-Accounts per abonnement overschrijden. In dergelijke gevallen moet de VHD's van virtuele machines kunnen worden gecombineerd in één Opslagaccount. Doorgaans wilt u dat doen door een combinatie van VHD's van de SAP-toepassingslaag virtuele machines van verschillende SAP-systemen.  We ook samengevoegd verschillende VHD's van verschillende DBMS-virtuele machines van verschillende SAP-systemen in één Azure Storage-Account. De IOPS-limieten van Azure Storage-Accounts waardoor waarmee u rekening moet houden (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] Hoge beschikbaarheid in Windows

![SAP NetWeaver-toepassing HA-architectuur met SQL Server in Azure IaaS][planning-guide-figure-3200]

De volgende Azure-constructies worden gebruikt voor de SAP NetWeaver-systeem, effect minimaliseren door problemen van de infrastructuur en hosten patches:

* Het volledige systeem wordt geïmplementeerd op Azure (vereist - DBMS-laag, (A) SCS-exemplaar en de volledige toepassingslaag wilt uitvoeren op dezelfde locatie).
* Het volledige systeem wordt uitgevoerd binnen een Azure-abonnement (vereist).
* Het volledige systeem wordt uitgevoerd binnen een Azure Virtual Network (vereist).
* De scheiding van de virtuele machines van één SAP-systeem in drie Beschikbaarheidssets is het mogelijk zelfs met alle virtuele machines die behoren tot hetzelfde Virtueelnetwerk.
* Elke laag (bijvoorbeeld DBMS-systemen, ASCS, toepassingsservers) moet gebruiken een specifieke Beschikbaarheidsset.
* Alle virtuele machines DBMS-instanties van een SAP-systeem zich in een Beschikbaarheidsset. We gaan ervan uit dat er meer dan één VM met het DBMS-exemplaren per systeem sinds hoge beschikbaarheid van systeemeigen DBMS functies worden gebruikt, zoals SQL Server AlwaysOn of Oracle Data Guard.
* Alle virtuele machines DBMS-instanties gebruiken hun eigen opslagaccount. DBMS-gegevens en logboekbestanden bestanden worden gerepliceerd van één opslagaccount naar een ander opslagaccount met behulp van DBMS hoge beschikbaarheid functies waarmee de gegevens worden gesynchroniseerd. Niet beschikbaar zijn van één opslagaccount zorgt ervoor dat de onbeschikbaarheid van één SQL-Windows-clusterknooppunt, maar niet de volledige SQL Server-service.
* Alle virtuele machines met (A) SCS-exemplaar van een SAP-systeem zich in een Beschikbaarheidsset. Een Windows Server Failover Cluster (WSFC) is geconfigureerd op de desbetreffende VM's te beschermen van de (A) SCS-exemplaar.
* Alle virtuele machines (A) SCS-instanties gebruiken hun eigen opslagaccount. (A) SCS-exemplaarbestanden en globale SAP-map worden gerepliceerd van één opslagaccount naar een ander opslagaccount met behulp van SIOS DataKeeper-replicatie. Niet beschikbaar zijn van een storage-account, zullen niet beschikbaar zijn van een (A) SCS-Windows-clusterknooppunt, maar niet de gehele (A) SCS-service.
* ALLE virtuele machines voor de SAP-toepassingslaag server zijn in een andere Beschikbaarheidsset.
* ALLE virtuele machines met SAP-toepassingsservers gebruiken hun eigen opslagaccount. Niet beschikbaar zijn van één opslagaccount zorgt ervoor dat de onbeschikbaarheid van één SAP-toepassingsserver, waar andere SAP-toepassingsservers worden uitgevoerd.

De volgende afbeelding weergegeven de dezelfde Liggend met behulp van Managed Disks.

![SAP NetWeaver-toepassing HA-architectuur met SQL Server in Azure IaaS][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] Hoge beschikbaarheid in Linux

De architectuur voor SAP HA op Linux op Azure is in feite hetzelfde als voor Windows, zoals hierboven beschreven. Verwijzen naar SAP-notitie [1928533] voor een lijst met ondersteunde hoge beschikbaarheidsoplossingen.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Met behulp van automatisch starten voor SAP-exemplaren

SAP aangeboden de functionaliteit voor het starten van SAP instanties onmiddellijk na het begin van het besturingssysteem in de virtuele machine. De exacte stappen zijn beschreven in Knowledge Base-artikel SAP [1909114]. SAP is echter niet aangeraden de instelling te gebruiken niet meer omdat er geen controle in volgorde van de sessie opnieuw wordt opgestart, ervan uitgaande dat meer dan één virtuele machine is aangetast of meerdere exemplaren per virtuele machine uitgevoerd. Ervan uitgaande dat een typische Azure scenario van één SAP application server-exemplaar in een virtuele machine en het geval van een enkele virtuele machine uiteindelijk ophalen opnieuw wordt opgestart, het automatisch starten is niet kritiek en kan worden ingeschakeld door deze parameter toe te voegen:

    Autostart = 1

In het profiel van de start van het SAP ABAP en/of Java-exemplaar.

> [!NOTE]
> De parameter automatisch starten kan ook bepaalde downfalls hebben. De parameter wordt het begin van een SAP ABAP- of Java-exemplaar in meer detail geactiveerd wanneer de bijbehorende Windows/Linux-service van het exemplaar wordt gestart. Zeker is dat het geval wanneer het besturingssysteem wordt opgestart. Echter, opnieuw opstarten van de SAP-services zijn ook een algemene ding voor levenscyclusbeheer van SAP-Software-functionaliteit, zoals som of andere updates of upgrades. Deze functies verwacht een exemplaar automatisch opnieuw opgestart helemaal niet. De parameter Autostart moet daarom worden uitgeschakeld voordat u deze taken uitvoert. De parameter Autostart moet ook niet worden gebruikt voor SAP-instanties die zijn geclusterd, zoals ASCS/SCS/CI.
>
>

Zie aanvullende informatie met betrekking tot automatisch starten voor SAP-exemplaren hier:

* [Starten/stoppen SAP samen met uw Unix-Server starten/stoppen](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Starten en stoppen van SAP NetWeaver-Beheeragents](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Het inschakelen van automatisch starten van HANA-Database](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Grotere 3-Laagse SAP-systemen
Hoge beschikbaarheid aspecten van 3-Laagse SAP-configuraties hebt u al in eerdere secties besproken. Maar hoe zit waar de DBMS-server-vereisten te groot is zijn dat deze zich in Azure, maar de SAP-toepassingslaag systemen kunnen worden geïmplementeerd in Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Locatie van 3-Laagse SAP-configuraties
Het wordt niet ondersteund voor de toepassingslaag splitsen zelf of de toepassing en DBMS laag tussen on-premises en Azure. Een SAP-systeem is volledig geïmplementeerde on-premises of in Azure. Het is ook niet ondersteund als u wilt dat enkele van de toepassingsservers on-premises en andere uitvoeren in Azure. Dat is het beginpunt van de discussie. We ook geen ondersteuning bieden voor als u wilt dat de DBMS-onderdelen van een SAP-systeem en de SAP-toepassing server laag in twee verschillende Azure-regio's geïmplementeerd. Bijvoorbeeld, DBMS in VS-West en SAP-toepassingslaag in VS-midden. Reden voor de ondersteuning van deze configuraties niet is de gevoeligheid van de latentie van de SAP NetWeaver-architectuur.

Echter, in de loop van vorig jaar data center partners ontwikkelde CO locaties naar de Azure-regio's. Deze CO-locaties zijn vaak dicht in de Azure-gegevens van de fysieke datacenters binnen een Azure-regio. De korte afstand en de verbinding van de activa in de CO-locatie via ExpressRoute in Azure kunnen resulteren in een latentie van minder dan 2 MS. In dergelijke gevallen, als u wilt de DBMS-laag (inclusief opslag, SAN/NAS) Zoek in die een CO-locatie en het SAP-is toepassingslaag in Azure mogelijk. [HANA grote instanties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture). 

### <a name="offline-backup-of-sap-systems"></a>Offline back-up van SAP-systemen
Afhankelijk van de SAP-configuratie (Tier-2 of 3-Laagse) er gekozen kan back-up moet worden. De inhoud van de virtuele machine zelf plusteken om een back-up van de database. Back-ups met betrekking tot DBMS-systemen worden worden uitgevoerd met database-methoden verwacht. Een gedetailleerde beschrijving voor de verschillende databases, kunt u vinden in [DBMS-gids][dbms-guide]. Aan de andere kant kunnen de SAP-gegevens worden gemaakt op een offline manier (met inbegrip van de database-inhoud) zoals beschreven in deze sectie of online zoals beschreven in de volgende sectie.

De offline back-up vereist in feite een afsluiten van de virtuele machine via de Azure-portal en een kopie van de virtuele machine basisschijf plus alle gekoppelde schijven aan de virtuele machine. Dit zou een punt in tijd afbeelding van de virtuele machine en de bijbehorende schijf behouden. Het verdient aanbeveling om te kopiëren van de back-ups naar een andere Azure Storage-Account. Daarom de procedure beschreven in het hoofdstuk [kopiëren van schijven tussen Azure-Opslagaccounts] [ planning-guide-5.4.2] van dit document wilt toepassen.
Naast het afsluiten met behulp van de Azure portal een kunt ook dit doen via Powershell of CLI zoals hier wordt beschreven: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Terugzetten van deze status kan bestaan uit van het verwijderen van de basis-VM, evenals de oorspronkelijke schijven van de basis-VM en gekoppelde schijven, weer de schijven opgeslagen in de oorspronkelijke Storage-Account of resource-groep voor beheerde schijven kopiëren en vervolgens opnieuw het systeem te implementeren.
In dit artikel ziet u een voorbeeld hoe u dit proces in Powershell-script: <http://www.westerndevs.com/azure-snapshots/>

Zorg ervoor dat u een nieuwe licentie voor SAP installeren omdat het herstellen van een virtuele machine back-up, zoals hierboven beschreven maakt u een nieuwe hardwaresleutel.

### <a name="online-backup-of-an-sap-system"></a>Online back-up van een SAP-systeem

Back-up van de DBMS-systemen wordt uitgevoerd met DBMS-specifieke methoden, zoals beschreven in de [DBMS-gids][dbms-guide].

Andere virtuele machines binnen de SAP-systeem kunnen worden back-ups met behulp van Azure VM Backup-functie. Azure virtuele Machine back-up is een standaard-methode voor het back-up van een volledige virtuele machine in Azure. Azure Backup de back-ups opslaat in Azure en een voor het herstellen mogelijk van een virtuele machine opnieuw.

> [!NOTE]
> Vanaf december 2015 met behulp van back-up VM houdt geen unieke ID van de VM die wordt gebruikt voor SAP-licentieverlening. Dit betekent dat installatie van een nieuwe sleutel van de SAP-licentie voor een herstelbewerking voor een virtuele machine back-up is vereist als de herstelde virtuele machine wordt beschouwd als een nieuwe virtuele machine en niet als vervanging van de vorige versie die is opgeslagen.
>
> ![Windows][Logo_Windows] Windows
>
> In theorie, de virtuele machines die uitvoeren databases kunnen een back-up op een consistente manier ook als de DBMS-systeem de Windows VSS ondersteunt (Volume Shadow Copy Service <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx>), bijvoorbeeld SQL Server heeft.
> Wel rekening mee dat op basis van Azure VM-back-ups point-in-time worden hersteld van de databases zijn niet mogelijk. Daarom is de aanbeveling om uit te voeren van back-ups van databases met DBMS-functionaliteit in plaats van Azure VM Backup.
>
> Om vertrouwd te raken met Azure VM Backup Begin hier: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Andere mogelijkheden zijn om een combinatie van Microsoft Data Protection Manager geïnstalleerd in een virtuele machine van Azure en Azure Backup back-up/herstel databases te gebruiken. Meer informatie vindt u hier: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Er is geen equivalent voor Windows VSS in Linux. Alleen bestandsconsistente back-ups zijn daarom mogelijk, maar geen toepassingsconsistente back-ups. De SAP DBMS back-up moet worden uitgevoerd met behulp van DBMS-functionaliteit. Het bestandssysteem waarin de SAP-gerelateerde gegevens kan worden opgeslagen, bijvoorbeeld worden tar zoals beschreven hier gebruiken: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure als locatie voor herstel na Noodgevallen voor productie SAP-landschap

Sinds Mid 2014 inschakelen uitbreidingen voor verschillende onderdelen om Hyper-V, System Center en Azure het gebruik van Azure als DR-site voor on-premises uitgevoerd op basis van Hyper-V-VM's.

Een blog met gedetailleerde informatie over het implementeren van deze oplossing wordt hier beschreven: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Samenvatting

De belangrijkste punten van hoge beschikbaarheid voor SAP-systemen in Azure zijn:

* Op dit moment, kan niet de SAP enkel storingspunt worden beveiligd, precies dezelfde manier zoals deze kan worden uitgevoerd in de on-premises implementaties. De reden is dat gedeelde schijf clusters nog in Azure kunnen niet worden gemaakt zonder het gebruik van 3e software van derden.
* Voor de DBMS-laag die u wilt gebruiken de DBMS-functionaliteit die niet afhankelijk is van een gedeelde schijf-clustertechnologie. Details worden beschreven in de [DBMS-gids][dbms-guide].
* Om te beperken de gevolgen van problemen in domeinen met fouten in de Azure-infrastructuur of de host-onderhoud, moet u Beschikbaarheidssets van Azure:
  * Het wordt aanbevolen dat één Beschikbaarheidsset voor de SAP-toepassingslaag.
  * Het wordt aanbevolen dat u een afzonderlijke Beschikbaarheidsset voor de SAP DBMS-laag.
  * Het wordt niet aanbevolen om toe te passen van dezelfde Beschikbaarheidsset voor virtuele machines van verschillende SAP-systemen.
  * Het verdient aanbeveling gebruik van Premium Managed Disks.
* Voor de doeleinden van de back-up van de SAP DBMS-laag, Controleer de [DBMS-gids][dbms-guide].
* Back-ups van instanties van SAP-dialoogvenster zinvol weinig omdat het is meestal gemakkelijker is om eenvoudig dialoogvenster exemplaren opnieuw te implementeren.
* Back-ups van de virtuele machine waarvan de algemene map van de SAP-systeem en het bevat de profielen van de andere exemplaren, het zinvol zijn en moet worden uitgevoerd met Windows back-up of, bijvoorbeeld tar op Linux. Aangezien er zijn verschillen tussen Windows Server 2008 (R2) en Windows Server 2012 (R2), waarmee u gemakkelijk kunt back-up met behulp van de meest recente Windows-Server-versies, het beste uitvoeren van Windows Server 2012 (R2) als gastbesturingssysteem voor Windows.

## <a name="next-steps"></a>Volgende stappen
Lees de artikelen:

- [Azure Virtual Machines-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)
- [Configuraties van SAP HANA-infrastructuur en bewerkingen op Azure] (https://docs.microsoft.com/
- Azure/virtuele-machines/workloads/sap/hana-vm-bewerkingen)
