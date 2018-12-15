---
title: Oracle Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP | Microsoft Docs
description: Oracle Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP
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
ms.date: 12/14/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8686130e3b10ece605a6e648badf9aa1dae5e071
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435681"
---
# <a name="oracle-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Oracle Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP

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
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/resources/templates/sql-server-2014-alwayson-existing-vnet-and-ad/
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


Na de algemene ondersteuning is ook het specifieke scenario aan SAP-toepassingen gebruik te maken van Oracle-Databases ondersteund. Details zijn met de naam in dit document. In dit document bevat informatie over de verschillende onderdelen om te overwegen bij het implementeren van Oracle-Database voor SAP-workloads in Azure IaaS. Als een voorwaarde in dit document hebt lees het document [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) evenals andere handleidingen in de [SAP-workloads op Azure-documentatie](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

Oracle-versies en de bijbehorende versies van het besturingssysteem, die worden ondersteund voor het uitvoeren van SAP op Oracle op Azure Virtual Machines gevonden in de SAP-notitie [2039619].

Algemene informatie over het uitvoeren van SAP Business Suite op Oracle vindt u <https://www.sap.com/community/topic/oracle.html> Oracle-software wordt ondersteund door Oracle worden uitgevoerd op Microsoft Azure. Raadpleeg voor meer informatie over de algemene ondersteuning van Windows Hyper-V en Azure: <http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html> 

SAP-opmerkingen die relevant zijn voor Oracle, SAP en Azure-lijst, zoals:

De volgende SAP-opmerkingen zijn gerelateerd aan SAP op Azure met betrekking tot het gebied in dit document behandeld:

| Houd er rekening mee getal | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen op Azure: Ondersteunde producten en typen Azure VM's |
| [2015553] |SAP op Microsoft Azure: Vereisten voor ondersteuning |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [2191498] |SAP op Linux met Azure: Uitgebreide bewaking |
| [2039619] |SAP-toepassingen op Microsoft Azure met behulp van de Oracle-Database: Ondersteunde producten en versies |
| [2243692] |Linux op Microsoft Azure (IaaS) virtuele machine: Problemen met SAP-licentie |
| [2069760] |Oracle Linux 7.x SAP-installatie en Upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |
| [2171857] |Oracle Database 12c - ondersteuning van het bestandssysteem in Linux |
| [1114181] |Oracle Database 11g - ondersteuning van het bestandssysteem in Linux |

Exacte configuraties en functionaliteit die worden ondersteund door Oracle en SAP op Azure worden beschreven in de SAP-notitie [#2039619](https://launchpad.support.sap.com/#/notes/2039619)

Windows- en Oracle Linux zijn de enige besturingssystemen ondersteund door Oracle en SAP op Azure. De gebruikte SLES en RHEL Linux-distributies worden niet ondersteund voor het implementeren van Oracle-onderdelen in Azure. Oracle-onderdelen zijn de Oracle-database-client, die wordt gebruikt door de SAP-toepassingen om op basis van de Oracle-DBMS verbinding te maken. Uitzonderingen op basis van SAP-notitie [#2039619](https://launchpad.support.sap.com/#/notes/2039619) SAP-endcomponenten, die niet met behulp van de Oracle-database-client zijn. Dergelijke SAP-onderdelen zijn van SAP zelfstandige in de wachtrij plaatsen, -berichtenserver, replicatie-services in de wachtrij plaatsen, WebDispatcher en SAP-Gateway.  Ondanks dat uw Oracle DBMS en instanties van SAP-toepassingen uitgevoerd op Oracle Linux, kan u uw SAP Central Services worden uitgevoerd op SLES of RHEL en beveiligen met een cluster op basis van Pacemaker. Pacemaker als hoge beschikbaarheid framework wordt niet ondersteund voor Oracle Linux.

## <a name="specifics-to-oracle-database-on-windows"></a>Specifieke met Oracle-Database op Windows-instellingen
### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms-on-windows"></a>De richtlijnen voor Oracle voor SAP-installaties in Azure VM's op Windows

In overeenstemming met SAP-installatie handmatig, moeten alle bestanden met betrekking tot Oracle niet worden geïnstalleerd of bevindt zich in het stuurprogramma van het bestandssysteem voor de besturingssysteemschijf van de virtuele machine (station c:). Verschillende groottes van virtuele machines ondersteunen een verschillend aantal schijven moeten worden gekoppeld. Kleinere virtuele machine-typen toestaan een kleiner aantal schijven moeten worden gekoppeld. In geval van dergelijke kleinere VM's, wordt u aangeraden Oracle thuis, fase, "saptrace", "saparch", "sapbackup", "sapcheck", "sapreorg" in de besturingssysteemschijf installeren/zoeken. Deze onderdelen van Oracle DBMS-onderdelen zijn niet intensief i/o-en i/o doorvoer. De besturingssysteemschijf kan daarom de i/o-vereisten te verwerken. De standaardgrootte van de besturingssysteemschijf is 127GB. Als de vrije ruimte beschikbaar niet voldoende is, de schijf kan worden [formaat](https://docs.microsoft.com/azure/virtual-machines/windows/expand-os-disk) tot 2048 GB. Oracle database en opnieuw aanmelden bestanden moeten worden opgeslagen op afzonderlijke gegevensschijven. Er is een uitzondering met de Oracle tijdelijke tabelruimte. Tempfiles kunnen worden gemaakt op D: / (niet-persistend station). De niet-permanente D:\ station biedt ook hogere i/o-latentie en doorvoer (met uitzondering van de A-serie VM's). Om te bepalen van de juiste tempfiles ruimte, kunt u de grootten tempfiles op bestaande systemen te controleren.

### <a name="storage-configuration"></a>Opslagconfiguratie
Slechts één exemplaar met behulp van schijven met NTFS geformatteerd Oracle wordt ondersteund. Alle databasebestanden moeten worden opgeslagen op het NTFS-bestandssysteem op Managed Disks (aanbevolen) of VHD's. Deze schijven zijn gekoppeld aan de Azure VM en zijn gebaseerd op de pagina-blobopslag van Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of [Azure beheerde schijven](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview). 

Het is raadzaam om te gebruiken [Azure beheerde schijven](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview). Ook is het raadzaam met behulp van [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) voor uw Oracle-Database-implementaties

Alle soorten netwerkstations of externe bestandsshares, zoals Azure file-services:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

zijn **niet** ondersteund voor Oracle database-bestanden.

Schijven op basis van pagina-blobopslag van Azure of beheerde schijven, de instructies die zijn aangebracht [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) zijn van toepassing op implementaties met de Oracle-Database.

Zoals uitgelegd [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md), quota's voor de doorvoer van de IOPS voor Azure-schijven zijn. De exacte quota zijn afhankelijk van het type virtuele machine die wordt gebruikt. Een lijst met typen VM's met hun quota vindt [hier (Windows)][virtual-machines-sizes-windows].

Voor het identificeren van de ondersteunde typen Azure VM's, verwijzen naar SAP-notitie [1928533].

Minimale configuratie:
| Onderdeel | Schijf | Caching | Opslaggroep |
| --- | ---| --- | --- |
| \oracle\<SID > \origlogaA & mirrlogB | Premium | Geen | Niet nodig |
| \oracle\<SID > \origlogaB & mirrlogA | Premium | Geen | Niet nodig |
| \oracle\<SID > \sapdata1...n | Premium | Alleen-lezen | Kan worden gebruikt |
| \oracle\<SID > \oraarch | Standard | Geen | Niet nodig |
| Oracle thuis, saptrace... | Besturingssysteemschijf | | Niet nodig |


Schijven selecteren voor het hosten van Logboeken van de online opnieuw moet worden aangestuurd door IOPs-vereisten. Het is mogelijk voor het opslaan van alle sapdata1... n (tabelruimten) op één enkele gekoppelde schijf zolang grootte, IOPS en doorvoer voldoet aan de vereisten. 

Prestaties configureren:
| Onderdeel | Schijf | Caching | Opslaggroep |
| --- | ---| --- | --- |
| \oracle\<SID > \origlogaA | Premium | Geen | Kan worden gebruikt  |
| \oracle\<SID > \origlogaB | Premium | Geen | Kan worden gebruikt |
| \oracle\<SID > \mirrlogAB | Premium | Geen | Kan worden gebruikt |
| \oracle\<SID > \mirrlogBA | Premium | Geen | Kan worden gebruikt |
| \oracle\<SID > \sapdata1...n | Premium | Alleen-lezen | Aanbevolen  |
| \oracle\SID\sapdata(n+1) * | Premium | Geen | Kan worden gebruikt |
| \oracle\<SID > \oraarch* | Premium | Geen | Niet nodig |
| Oracle thuis, saptrace... | Besturingssysteemschijf | Niet nodig |

*(n+1) - systeem, TEMP en ongedaan maken tabelruimten hosten. De i/o-patroon van systeem en ongedaan maken tabelruimten zijn anders dan andere tabelruimten die als host fungeert voor toepassingsgegevens. Geen caching, is de beste optie voor prestaties van de tabelruimten systeem- en ongedaan maken.
* oraarch - opslaggroep is niet nodig van de Prestatieweergave. Het kan worden gebruikt om meer ruimte

Als meer IOPS vereist zijn, is het raadzaam venster opslaggroepen (alleen beschikbaar in Windows Server 2012 en hoger) gebruiken om te maken van een grote logisch apparaat via meerdere gekoppelde schijven. Deze aanpak vereenvoudigt het beheer-overhead voor het beheren van de schijfruimte en vermijdt u de inspanningen om de bestanden handmatig te verdelen over meerdere gekoppelde schijven.


#### <a name="write-accelerator"></a>Write Accelerator
Voor Azure-M-serie VM's, kan de latentie schrijven in de logboeken online opnieuw worden teruggebracht door factoren, vergeleken met Azure Premium Storage-prestaties bij het gebruik van Azure Write Accelerator. Azure Write Accelerator voor de schijven (VHD's) op basis van Azure Premium Storage die worden gebruikt voor de logboekbestanden van online opnieuw inschakelen. Meer informatie vindt u in het document [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).


### <a name="backup--restore"></a>Back-up en herstellen
Voor de functionaliteit van back-up/herstel, de SAP-BR * hulpprogramma's voor Oracle worden ondersteund op dezelfde manier als op standaard Windows Server-besturingssystemen. Oracle Recovery Manager (RMAN) wordt ook ondersteund voor back-ups naar schijf en herstellen van de schijf.

U kunt ook Azure Backup-Service gebruiken voor het uitvoeren van een toepassing consistente VM back-up. Het artikel [plannen van uw VM-back-upinfrastructuur in Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction), legt uit de Azure Backup-Service maakt gebruik van de Windows VSS-functionaliteit voor het uitvoeren van een consistente back-ups van toepassing. Het Oracle DBMS-versies die op Azure worden ondersteund door SAP kunnen gebruikmaken van de VSS-functionaliteit voor back-ups. Meer informatie vindt u in de documentatie van Oracle [fundamentele concepten van databaseback-up en herstel met VSS](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/ntqrf/basic-concepts-of-database-backup-and-recovery-with-vss.html#GUID-C085101B-237F-4773-A2BF-1C8FD040C701).


### <a name="high-availability"></a>Hoge beschikbaarheid
Oracle Data Guard wordt ondersteund voor hoge beschikbaarheid en noodherstel. Als u wilt een automatische failover in Data Guard, moet snel starten-Failover (FSFA) worden gebruikt. De waarnemer (FSFA) is de failover wordt geactiveerd. FSFA gebruikt, is de configuratie van een handmatige failover mogelijk.

Aspecten van herstel na noodgevallen voor Oracle-databases in Azure worden weergegeven in het artikel [herstel na noodgevallen voor een Oracle Database 12c-database in een Azure-omgeving](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-disaster-recovery).

### <a name="accelerated-networking"></a>Versneld netwerken
Voor Oracle-implementaties op Windows, is het raadzaam de Azure-functionaliteit van versnelde netwerken gebruiken zoals wordt beschreven in het document [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Houd ook rekening met de aanbevelingen [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md). 

### <a name="other"></a>Overige
Andere algemene onderdelen zoals Beschikbaarheidssets van Azure of SAP bewaking toepassen, zoals beschreven in het document [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) voor implementaties van virtuele machines met de Oracle-Database als goed.

## <a name="specifics-to-oracle-database-on-oracle-linux"></a>Specifieke informatie met Oracle-Database voor Oracle Linux
Oracle-software wordt ondersteund door Oracle uit te voeren op Microsoft Azure met Oracle Linux als gastbesturingssysteem. Raadpleeg voor meer informatie over de algemene ondersteuning van Windows Hyper-V en Azure: <http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html> 

Na de algemene ondersteuning is ook het specifieke scenario aan SAP-toepassingen gebruik te maken van Oracle-Databases ondersteund. Meer informatie worden in dit gedeelte van het document besproken.

### <a name="oracle-version-support"></a>Ondersteuning voor Oracle-versie
Oracle-versies en de bijbehorende versies van het besturingssysteem, die worden ondersteund voor het uitvoeren van SAP op Oracle op Azure Virtual Machines gevonden in de SAP-notitie [2039619].

Algemene informatie over het uitvoeren van SAP Business Suite op Oracle vindt u in <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms-on-linux"></a>De richtlijnen voor Oracle voor SAP-installaties in Azure VM's op Linux

In overeenstemming met SAP-installatie handleidingen, moeten Oracle-gerelateerde bestanden niet worden geïnstalleerd of bevindt zich in het stuurprogramma van het bestandssysteem voor de opstartschijf van de virtuele machine. Verschillende groottes van virtuele machines ondersteunen een verschillend aantal schijven moeten worden gekoppeld. Kleinere virtuele machine-typen toestaan een kleiner aantal schijven moeten worden gekoppeld. In dit geval wordt aangeraden installeren/Zoek Oracle thuis, fase, saptrace, saparch, sapbackup, sapcheck, sapreorg naar opstartschijf. Deze onderdelen van Oracle DBMS-onderdelen zijn niet intensief i/o-en i/o doorvoer. De besturingssysteemschijf kan daarom de i/o-vereisten te verwerken. De standaardgrootte van de besturingssysteemschijf is 30GB. U kunt de opstartschijf uitbreiden met behulp van Azure Portal/PowerShell/CLI. Zodra de opstartschijf is uitgebreid, kunt u een extra partitie voor Oracle-binaire bestanden toevoegen.


### <a name="storage-configuration"></a>Opslagconfiguratie

De bestandssystemen van ext4, of xfs of Oracle ASM worden ondersteund voor bestanden van de Oracle-database in Azure. Alle databasebestanden moeten worden opgeslagen op deze bestandssystemen op basis van VHD's of Managed Disks. Deze schijven zijn gekoppeld aan de Azure VM en zijn gebaseerd op de pagina-blobopslag van Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of [Azure beheerde schijven](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview). 

Voor Oracle Linux UEK kernels, het minimum van UEK versie 4 is vereist voor ondersteuning [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage#premium-storage-for-linux-vms).

Het is raadzaam om te gebruiken [Azure beheerde schijven](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview). Ook is het raadzaam met behulp van [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) voor uw Oracle-Database-implementaties.

Alle soorten netwerkstations of externe bestandsshares, zoals Azure file-services:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

zijn **niet** ondersteund voor Oracle database-bestanden.

Met behulp van schijven op basis van Azure Storage voor Page-BLOB of Managed Disks, de instructies in aangebracht [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) zijn van toepassing op implementaties met de Oracle-Database.

Zoals uitgelegd in document [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md), quota's voor de doorvoer van de IOPS voor Azure-schijven zijn. De exacte quota zijn afhankelijk van het type virtuele machine die wordt gebruikt. Een lijst met typen VM's met hun quota vindt [hier (Linux)][virtual-machines-sizes-linux].

Voor het identificeren van de ondersteunde typen Azure VM's, verwijzen naar SAP-notitie [1928533]

Minimale configuratie:
| Onderdeel | Schijf | Caching | Verwijderen van oude * |
| --- | ---| --- | --- |
| /Oracle/<SID>/origlogaA & mirrlogB | Premium | Geen | Niet nodig |
| /Oracle/<SID>/origlogaB & mirrlogA | Premium | Geen | Niet nodig |
| /Oracle/<SID>/sapdata1...n | Premium | Alleen-lezen | Kan worden gebruikt |
| /Oracle/<SID>/oraarch | Standard | Geen | Niet nodig |
| Oracle thuis, saptrace... | Besturingssysteemschijf | | Niet nodig |

* Verwijdering: LVM stripe of MDADM met behulp van RAID 0

De schijfselectie voor het hosten van Logboeken voor de Oracle-online opnieuw moet worden aangestuurd door IOPS-vereisten. Het is mogelijk voor het opslaan van alle sapdata1... n (tabelruimten) op één enkele gekoppelde schijf zolang volume, IOPS en doorvoer voldoet aan de vereisten. 

Prestaties configureren:
| Onderdeel | Schijf | Caching | Verwijderen van oude * |
| --- | ---| --- | --- |
| /Oracle/<SID>/origlogaA | Premium | Geen | Kan worden gebruikt  |
| /Oracle/<SID>/origlogaB | Premium | Geen | Kan worden gebruikt |
| /Oracle/<SID>/mirrlogAB | Premium | Geen | Kan worden gebruikt |
| /Oracle/<SID>/mirrlogBA | Premium | Geen | Kan worden gebruikt |
| /Oracle/<SID>/sapdata1...n | Premium | Alleen-lezen | Aanbevolen  |
| /Oracle/SID/sapdata(n+1)* | Premium | Geen | Kan worden gebruikt |
| /Oracle/<SID>/oraarch* | Premium | Geen | Niet nodig |
| Oracle thuis, saptrace... | Besturingssysteemschijf | Niet nodig |

* Verwijdering: LVM stripe of MDADM met behulp van RAID 0 *(n+1) - systeem, TEMP en ongedaan maken tabelruimten hosten. hij i/o-patroon van systeem en ongedaan maken tabelruimten zijn anders dan andere tabelruimten die als host fungeert voor toepassingsgegevens. Geen caching, is de beste optie voor prestaties van de tabelruimten systeem- en ongedaan maken.
* oraarch - opslaggroep is niet nodig van de Prestatieweergave. Kan worden gebruikt om meer ruimte.


Als meer IOPS vereist zijn, is het raadzaam LVM (Logical Volume Manager) of MDADM gebruiken om te maken van een logische groot via meerdere gekoppelde schijven. Zie ook het document [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) met betrekking tot richtlijnen en tips over hoe u kunt gebruikmaken van LVM of MDADM. Deze aanpak vereenvoudigt het beheer-overhead voor het beheren van de schijfruimte en vermijdt u de inspanningen om de bestanden handmatig te verdelen over meerdere gekoppelde schijven.


#### <a name="write-accelerator"></a>Write Accelerator:
Voor Azure-M-serie VM's, kan de latentie schrijven in de logboeken online opnieuw worden teruggebracht door factoren, vergeleken met Azure Premium Storage-prestaties bij het gebruik van Azure Write Accelerator. Azure Write Accelerator voor de schijven (VHD's) op basis van Azure Premium Storage die worden gebruikt voor de logboekbestanden van online opnieuw inschakelen. Meer informatie vindt u in het document [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).


### <a name="backup--restore"></a>Back-up en herstellen
Voor de functionaliteit van back-up/herstel, de SAP-BR *-hulpprogramma's voor Oracle op dezelfde manier als op bare-metalcomputers en Hyper-V worden ondersteund. Oracle Recovery Manager (RMAN) wordt ook ondersteund voor back-ups naar schijf en herstellen van de schijf.

Meer informatie over hoe u Azure Backup en Recovery-service kunt voor het back-ups maken en herstellen van Oracle-databases kunnen worden gevonden in het artikel [Back-up en herstellen van een Oracle Database 12 c-database op een virtuele Azure Linux-machine](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-backup-recovery)

### <a name="high-availability"></a>Hoge beschikbaarheid
Oracle Data Guard wordt ondersteund voor hoge beschikbaarheid en noodherstel. Als u wilt een automatische failover in Data Guard, moet snel starten-Failover (FSFA) worden gebruikt. De waarnemer-functionaliteit (FSFA) is de failover wordt geactiveerd. FSFA gebruikt, is de configuratie van een handmatige failover mogelijk.  Meer informatie vindt u in het artikel [implementeren Oracle Data Guard op een Azure Linux-machine](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard).


Aspecten van herstel na noodgevallen voor Oracle-databases in Azure worden weergegeven in het artikel [herstel na noodgevallen voor een Oracle Database 12c-database in een Azure-omgeving](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-disaster-recovery).

### <a name="accelerated-networking"></a>Versneld netwerken
Ondersteuning voor Azure Accelerated Networking in Oracle Linux wordt geleverd met Oracle Linux 7 Update 5 (Oracle Linux 7.5). Als u niet naar de meest recente versie van de Oracle Linux 7.5 upgraden, kan dit worden veroorzaakt door een tijdelijke oplossing met behulp van de Red Hat compatibele Kernel (RHCK) in plaats van de kernel Oracle UEK. Gebruik van de RHEL-kernel binnen Oracle Linux wordt ondersteund op basis van SAP-notitie [#1565179](https://launchpad.support.sap.com/#/notes/1565179). Voor de minimale RHCKL door Azure Accelerated Networking moet kernel release 3.10.0-862.13.1.el7. Voor het gebruik van de kernel UEK in Oracle Linux in combinatie met [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/), moet u Oracle-UEK kernelversie 5 gebruiken.

Als u geen virtuele machines uit die niet is gebaseerd op Azure Marketplace-installatiekopie implementeert, moet u extra configuratiebestanden die moeten worden gekopieerd naar de virtuele machine door uit te voeren: 
<pre><code># Copy settings from github to correct place in VM
sudo curl -so /etc/udev/rules.d/68-azure-sriov-nm-unmanaged.rules https://raw.githubusercontent.com/LIS/lis-next/master/hv-rhel7.x/hv/tools/68-azure-sriov-nm-unmanaged.rules 
</code></pre>


### <a name="other"></a>Overige
Andere algemene onderdelen zoals Beschikbaarheidssets van Azure of SAP bewaking toepassen zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de Oracle-Database.
