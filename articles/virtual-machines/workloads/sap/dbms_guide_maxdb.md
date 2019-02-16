---
title: SAP-implementatie MaxDB liveCache en Server voor webinhoud op Azure Virtual machines | Microsoft Docs
description: SAP MaxDB liveCache en implementatie van de Server voor webinhoud op Azure
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
ms.date: 07/12/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4d770e091c8786972e3f15a03fc5af9ec9445002
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327796"
---
# <a name="sap-maxdb-livecache-and-content-server-deployment-on-azure-vms"></a>SAP MaxDB liveCache en implementatie van de Server voor webinhoud op Azure Virtual machines

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
[dbms-guide-figure-900]:media/dbms_maxdb_deployment_guide/900-sap-cache-server-on-premises.png

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
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
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




In dit document bevat informatie over de verschillende onderdelen om te overwegen bij het implementeren van MaxDB liveCache en Server voor webinhoud in Azure IaaS. Als een voorwaarde in dit document hebt lees het document [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md) evenals andere handleidingen in de [SAP-workloads op Azure-documentatie](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started). 

## <a name="specifics-for-the-sap-maxdb-deployments-on-windows"></a>Details voor de SAP-MaxDB-implementaties op Windows
### <a name="sap-maxdb-version-support-on-azure"></a>SAP MaxDB versie-ondersteuning op Azure
SAP biedt momenteel ondersteuning voor SAP MaxDB 7,9 of hoger voor gebruik met SAP NetWeaver gebaseerde producten in Azure. Alle updates voor SAP MaxDB-server of JDBC en ODBC-stuurprogramma's voor gebruik met SAP NetWeaver gebaseerde producten vindt u uitsluitend via SAP Service Marketplace op <https://support.sap.com/swdc>.
Algemene informatie over het uitvoeren van SAP NetWeaver op SAP MaxDB kan worden gevonden op <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Ondersteunde versies van Microsoft Windows en virtuele Azure-machine-typen voor SAP MaxDB DBMS
Als u de ondersteunde versie van Microsoft Windows voor SAP MaxDB DBMS in Azure, bezoekt u het:

* [SAP-Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam om te gebruiken van de nieuwste versie van het besturingssysteem Microsoft Windows, Microsoft Windows 2016 is.

### <a name="available-sap-maxdb-documentation-for-maxdb"></a>Beschikbare SAP MaxDB documentatie voor MaxDB
U kunt de bijgewerkte lijst van SAP MaxDB documentatie vinden in de volgende SAP-notitie [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Richtlijnen voor SAP MaxDB voor SAP-installaties in Azure VM 's
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Opslagconfiguratie
Aanbevolen procedures van de Azure-opslag voor SAP MaxDB gaat u als volgt de algemene aanbevelingen die worden vermeld in het hoofdstuk [opslagstructuur van een virtuele machine voor RDBMS implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64).

> [!IMPORTANT]
> Net als andere databases heeft SAP MaxDB ook gegevens en logboekbestanden. De juiste term is echter in de SAP-MaxDB terminologie "volume" (niet ' file'). Er zijn bijvoorbeeld SAP MaxDB hoeveelheden gegevens en logboekvolumes. Verwar deze OS schijfvolumes niet. 
> 
> 

Kort gezegd hebt tot:

* Als u Azure Storage-accounts gebruiken, stelt u de Azure-opslagaccount waarin de SAP-MaxDB gegevens en logboekvolumes (gegevens en logboekbestanden bestanden) op **lokaal redundante opslag (LRS)** zoals opgegeven in [overwegingen voor het virtueel Azure- Implementatie voor de werkbelasting van SAP DBMS machines](dbms_guide_general.md).
* De i/o-pad voor SAP MaxDB gegevensvolumes (gegevensbestanden) te scheiden van de i/o-pad voor logboekvolumes (zoals logboekbestanden). Betekent dit dat SAP MaxDB gegevensvolumes (gegevensbestanden) moeten worden geïnstalleerd op één logisch station en SAP MaxDB logboekvolumes (zoals logboekbestanden) moeten worden geïnstalleerd op een andere logische schijf.
* Stel de juiste cachingtype voor elke schijf, afhankelijk van of u deze gebruiken voor SAP MaxDB gegevensbestand of logboekbestand volumes (gegevens en logboekbestanden bestanden) en of u Azure Standard- of Azure Premium Storage gebruiken zoals wordt beschreven in [overwegingen voor Azure Virtual Machines DBMS implementatie voor de werkbelasting van SAP](dbms_guide_general.md).
* Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor het opslaan van de gegevensvolumes op één gekoppelde schijf en ook alle logboekvolumes voor database opslaan op een andere één gekoppelde schijf.
* Als meer IOPS en/of ruimte vereist zijn, is het aanbevolen Microsoft-venster opslaggroepen (alleen beschikbaar in Microsoft Windows Server 2012 en hoger) gebruiken om te maken van een grote logisch apparaat via meerdere gekoppelde schijven. Zie voor meer informatie ook [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md). Deze aanpak vereenvoudigt het beheer-overhead voor het beheren van de schijfruimte en vermijdt u de moeite van het bestanden handmatig te verdelen over meerdere gekoppelde schijven.
* het is raadzaam Azure Premium Storage gebruiken voor MaxDB implementaties. 

![Naslaginformatie over configuratie van Azure IaaS-VM voor SAP MaxDB DBMS](./media/dbms_maxdb_deployment_guide/Simple_disk_structure_maxdb.PNG)


#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Back-up en herstel
Wanneer u SAP MaxDB implementeert in Azure, moet u uw back-upmethode bekijken. Zelfs als het systeem niet een productieve systeem is, moet de SAP-database die wordt gehost door SAP MaxDB worden back-ups regelmatig. Omdat Azure Storage kan drie afbeeldingen voldoen, is nu een back-up minder belangrijke wat betreft het beveiligen van uw systeem op basis van de fout bij de opslag en de belangrijkste administratieve of operationele fouten. De belangrijkste reden voor het onderhouden van een goede back-up en herstellen-plan is zodat u logische of handmatige fouten compenseren kunt door recovery point-in-time-mogelijkheden te bieden. Zodat het doel is om de back-ups gebruiken voor de database herstellen naar een bepaald punt in tijd of het gebruik van de back-ups in Azure als seeding voor een ander systeem door het kopiëren van de bestaande database. 

Back-up en herstellen van een database in Azure werkt op dezelfde manier als voor on-premises systemen, zodat u kunt standaardprogramma SAP MaxDB back-up/herstel's, die worden beschreven in een van de SAP-MaxDB documentatie over documenten die worden vermeld in de SAP-notitie [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Prestatie-overwegingen voor back-up en herstel
Zoals in de bare-metal implementaties zijn de prestaties van back-up en herstel afhankelijk van hoeveel volumes kunnen worden gelezen in parallelle instructies en de doorvoer van deze volumes. Daarom kan een aannemen:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de database-apparaten, des te lager de algemene lezen doorvoer
* De back-up om naar te schrijven, des te lager de doorvoer het minder doelen (Stripe mappen, schijven)

Als u wilt het aantal doelen te schrijven naar verhogen, zijn er twee opties die u, mogelijk in combinatie, afhankelijk van uw behoeften gebruiken kunt:

* Afzonderlijke volumes uitsluitend voor back-up
* Het volume back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOP's op het desbetreffende schijfvolume striped
* Apparaten van afzonderlijke toegewezen logische schijf voor problemen:
  * SAP-MaxDB back-upvolumes (dat wil zeggen-bestanden)
  * SAP-gegevensvolumes MaxDB (dat wil zeggen-bestanden)
  * SAP-MaxDB logboekvolumes (dat wil zeggen-bestanden)

Een volume striping over meerdere gekoppelde schijven is beschreven in eerder [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md). 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Andere overwegingen
Alle andere algemene gebieden, zoals Azure-Beschikbaarheidssets of SAP controle zijn ook van toepassing zoals beschreven in [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md).  voor implementaties van virtuele machines met de SAP-MaxDB-database.
Andere SAP MaxDB-specifieke instellingen zijn transparant voor de Azure-VM's en worden beschreven in verschillende documenten die worden vermeld in de SAP-notitie [767598] en deze SAP-opmerkingen:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-deployments-on-windows"></a>Details voor SAP liveCache implementaties op Windows
### <a name="sap-livecache-version-support"></a>SAP liveCache versie-ondersteuning
Minimale versie van SAP-liveCache ondersteund in Azure Virtual Machines is **SAP LC/LCAPPS 10.0 SP 25** inclusief **liveCache 7.9.08.31** en **LCA-Build 25**, vrijgegeven voor **EhP 2 voor SAP SCM 7.0** en latere versies.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Microsoft Windows-versies en Azure-VM-typen voor liveCache SAP DBMS-systemen ondersteund
Als u de ondersteunde versie van Microsoft Windows voor liveCache SAP op Azure, bezoekt u het:

* [SAP-Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van het besturingssysteem Microsoft Windows Server gebruiken. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache de richtlijnen voor SAP-installaties in Azure VM 's
#### <a name="recommended-azure-vm-types-for-livecache"></a>Aanbevolen Azure-VM-typen voor liveCache
Heeft een grote invloed hebben op de prestaties van SAP-liveCache zoals SAP liveCache is een toepassing die enorme berekeningen uitvoert, de hoeveelheid en de snelheid van het RAM-geheugen en CPU. 

Voor de virtuele machine van Azure die worden ondersteund door SAP (SAP-notitie [1928533]), alle virtuele CPU-resources toegewezen aan de virtuele machine worden ondersteund door specifieke fysieke CPU-resources van de hypervisor. Er is geen overmatige inrichting (en dus geen concurrentie voor CPU-resources) plaatsvindt.

Op dezelfde manier voor alle Azure-VM-exemplaar die worden ondersteund door SAP, is het geheugen van de virtuele machine 100% is toegewezen aan het fysieke geheugen - overprovisioning (te veel toezegging), bijvoorbeeld wordt niet gebruikt.

Van dit perspectief, is het raadzaam om de meest recente Dv2, Dv3, Ev3 en M-serie VM's te gebruiken. De keuze van de verschillende VM-typen, is afhankelijk van het geheugen die u nodig hebt voor liveCache en de CPU-resources die u nodig hebt. Als met alle andere implementaties van DBMS wordt u aangeraden gebruikmaken van Azure Premium Storage voor prestaties van essentiële volumes.

#### <a name="storage-configuration-for-livecache-in-azure"></a>De opslagconfiguratie voor liveCache in Azure
Alle de Azure storage aanbevolen werkwijzen besproken voor SAP-MaxDB die worden beschreven in dit document vermeld, zijn ook geldig voor SAP liveCache zoals SAP liveCache is gebaseerd op SAP MaxDB technologie. 

#### <a name="dedicated-azure-vm-for-livecache-scenario"></a>Speciale Azure-VM voor liveCache scenario
Omdat SAP liveCache intensief gebruikmaakt van verwerkingskracht, voor productief gebruik is het raadzaam om te implementeren op een specifieke Azure-Machine. 

![Virtuele Azure-machine voor liveCache voor productieve use-case toegewezen](./media/dbms_maxdb_deployment_guide/700-livecach-prod.PNG)


#### <a name="backup-and-restore-for-livecache-in-azure"></a>Back-up en herstel voor liveCache in Azure
back-up en herstel, met inbegrip van prestatie-overwegingen, worden al in de desbetreffende SAP MaxDB hoofdstukken in dit document beschreven. 

#### <a name="other-considerations"></a>Andere overwegingen
Alle andere algemene gebieden worden al in het desbetreffende SAP MaxDB hoofdstuk beschreven. 

## <a name="specifics-for-the-sap-content-server-deployment-on-windows-in-azure"></a>Gegevens voor de implementatie van de SAP-Server voor webinhoud op Windows in Azure
De Server van de SAP-inhoud is een afzonderlijke, server-gebaseerde onderdeel voor het opslaan van inhoud, zoals elektronische documenten in verschillende indelingen. De SAP-Server voor webinhoud wordt geleverd door het gebied van technologie en moet worden gebruikt verschillende toepassingen voor SAP-toepassingen. Deze is geïnstalleerd op een afzonderlijk systeem. Typische inhoud is trainingsmateriaal en documentatie van kennis datawarehouse of technische tekeningen die afkomstig zijn van de mySAP PLM Document Management-systeem. 

### <a name="sap-content-server-version-support-for-azure-vms"></a>Ondersteuning voor versie van SAP-Server voor webinhoud voor Azure VM 's
SAP op dit moment ondersteunt:

* **SAP-Server voor webinhoud** met versie **6.50 (en hoger)**
* **SAP MaxDB versie 7,9**
* **Microsoft IIS (Internet Information Server) versie 8.0 (en hoger)**

Het is raadzaam het gebruik van de nieuwste versie van SAP-Server voor webinhoud en de nieuwste versie van **Microsoft IIS**. 

Controleer de nieuwste ondersteunde versies van de inhoudsserver SAP en Microsoft IIS in de [SAP Product beschikbaarheid Matrix (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Microsoft Windows en virtuele Azure-machine-typen ondersteund voor SAP-Server voor webinhoud
Ondersteunde Windows-versie voor de Server voor webinhoud SAP op Azure, Zie:

* [SAP-Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van Microsoft Windows Server gebruiken.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Richtlijnen voor Content Server SAP voor SAP-installaties in Azure VM 's
#### <a name="storage-configuration-foir-content-server-in-azure"></a>Opslag configuratie foir Server voor webinhoud in Azure
Als u inhoud SAP-Server voor het opslaan van bestanden in de database SAP MaxDB configureert, zijn alle Azure storage aanbevolen procedures voor aanbevelingen voor SAP MaxDB vermeld in dit document ook geldig voor de SAP-Server voor webinhoud-scenario. 

Als u inhoud SAP-Server voor het opslaan van bestanden in het bestandssysteem is geconfigureerd, wordt het aanbevolen een specifieke logische station wilt gebruiken. Windows Storage Spaces, kunt u zodat ook de grootte van de logische schijf en doorvoer, IOP's, zoals beschreven in [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](dbms_guide_general.md). 

#### <a name="sap-content-server-location"></a>Locatie van de SAP-Server voor webinhoud
SAP-inhoudsserver moet worden geïmplementeerd in dezelfde Azure-regio en Azure VNET waar het SAP-systeem wordt geïmplementeerd. U staat op vrije om te bepalen of u wilt implementeren de inhoudsserver SAP-onderdelen op een specifieke virtuele machine van Azure of op dezelfde virtuele machine waarop het SAP-systeem wordt uitgevoerd. 

![Toegewezen virtuele machines van Azure voor SAP-Server voor webinhoud](./media/dbms_maxdb_deployment_guide/800-azure-vm-sap-content-server.png)


#### <a name="sap-cache-server-location"></a>SAP-Server cachelocatie
De cacheserver SAP is een extra onderdeel op de server voor toegang tot documenten lokaal (in het cachegeheugen). De cacheserver SAP slaat de documenten van een SAP-Server voor webinhoud. Dit is het optimaliseren van het netwerkverkeer als documenten hebben meer dan één keer worden opgehaald van de verschillende locaties. De regel is dat de cacheserver SAP worden fysiek dicht bij de client die toegang heeft tot de cacheserver SAP. 

Hier hebt u twee opties:

1. **Client is een SAP-systeem voor back-end** als een back-end SAP-systeem is geconfigureerd voor toegang tot inhoud SAP-Server, die SAP-systeem is een client. Als zowel de SAP-systeem en de SAP-Server voor webinhoud zijn geïmplementeerd in dezelfde Azure-regio, in de dezelfde Azure-datacenter, gebruikers die zich fysiek dicht bij elkaar. Er is daarom niet nodig om een speciale Server voor de SAP-Cache. SAP-UI-clients (SAP-GUI of web browser) rechtstreeks toegang hebben tot de SAP-systeem en de SAP-systeem documenten opgehaald uit de SAP-Server voor webinhoud.
2. **Client is een on-premises-webbrowser** de SAP-inhoudsserver kan worden geconfigureerd voor het rechtstreeks door de webbrowser worden geopend. In dit geval is een webbrowser die on-premises uitgevoerd een client van de SAP-Server voor webinhoud. On-premises datacenter en Azure-datacenter worden geplaatst op verschillende fysieke locaties (in het ideale geval dicht bij elkaar). Uw on-premises datacenter is verbonden met Azure via Azure Site-naar-Site VPN of ExpressRoute. Hoewel beide opties beveiligde VPN-netwerkverbinding naar Azure bieden, biedt site-naar-site-netwerkverbinding een SLA van netwerk bandbreedte en de latentie tussen het on-premises datacenter en de Azure-datacenter. Als u wilt versnellen toegang tot documenten, kunt u het volgende doen:
   1. De cacheserver SAP on-premises installeert, dicht bij de on-premises web browser (optie in de afbeelding hieronder)
   2. Configureer Azure ExpressRoute biedt een hoge en lage latentie toegewezen netwerkverbinding tussen on-premises datacenter en Azure-datacenter.

![Optie voor het installeren van de cacheserver SAP on-premises](./media/dbms_maxdb_deployment_guide/900-sap-cache-server-on-premises.png)
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Back-up en herstellen
Als u de SAP-Server voor webinhoud voor het opslaan van bestanden in de database SAP MaxDB configureert, kan de back-up/herstel procedure- en Prestatieoverwegingen al in SAP MaxDB hoofdstukken van dit document worden beschreven. 

Als u de SAP-Server voor webinhoud voor het opslaan van bestanden in het bestandssysteem is geconfigureerd, wordt een optie is voor het uitvoeren van handmatige back-up of herstellen van de structuur van het hele bestand waar de documenten moeten zich bevinden. Net als bij SAP MaxDB back-up/herstel, het is aanbevolen dat een toegewezen schijfvolume voor back-doel. 

#### <a name="other"></a>Overige
Andere inhoud SAP-Server-instellingen zijn transparant voor de Azure-VM's en worden beschreven in verschillende documenten en SAP-opmerkingen:

* <https://service.sap.com/contentserver> 
* SAP-notitie [1619726]  
