---
title: Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver | Microsoft Docs
description: Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/26/2018
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2caa9a5137edd4e012adf704c01dc5c470e1bb51
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972441"
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

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Deze handleiding is onderdeel van de documentatie over het implementeren en de SAP-software op Microsoft Azure implementeren. Lees voordat u deze handleiding leest, de [Planning and Implementation Guide][planning-guide]. In dit document bevat informatie over de implementatie van verschillende relationele Database Management Systems (RDBMS) en verwante producten in combinatie met SAP op Microsoft Azure Virtual Machines (VM's) met behulp van de Azure-infrastructuur als een Service (IaaS)-mogelijkheden.

Het document is een aanvulling op de documentatie voor installatie van SAP en SAP-opmerkingen, die staan voor de primaire bronnen voor installaties en -implementaties van SAP-software op de opgegeven platforms.

## <a name="general-considerations"></a>Algemene overwegingen
In dit hoofdstuk, zijn overwegingen met betrekking tot SAP DBMS-systemen uitvoeren in de Azure-VM's geïntroduceerd. Er zijn enkele verwijzingen naar specifieke DBMS-systemen in dit hoofdstuk. In plaats daarvan worden de specifieke DBMS-systemen afgehandeld in dit artikel, na dit hoofdstuk.

### <a name="definitions-upfront"></a>Definities kosten vooraf
In het hele document gebruiken we de volgende voorwaarden:

* IaaS: Infrastructure as a Service.
* PaaS: Platform as a Service.
* SaaS: Software as a Service.
* SAP-onderdeel: een afzonderlijke SAP-toepassing, zoals ECC, BW, Manager van de oplossing of EP  SAP-onderdelen kunnen worden gebaseerd op de traditionele ABAP- of Java-technologieën of een toepassing NetWeaver op basis van zoals zakelijke objecten.
* SAP-omgeving: een of meer onderdelen van SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke-functie, zoals ontwikkeling, QAS, Training, herstel na Noodgevallen of productie.
* SAP-landschap: Dit heeft betrekking op de gehele SAP-elementen in een klant IT landschap. De SAP-landschap bevat alle productie- en niet-productieomgevingen.
* SAP-systeem: De combinatie van DBMS-laag en niveau van de toepassing van bijvoorbeeld een ontwikkelsysteem SAP ERP, SAP BW-testsysteem, SAP CRM productiesysteem, enzovoort. In de Azure-implementaties, is het niet ondersteund voor het delen van deze twee lagen tussen on-premises en Azure. Dit betekent dat die een SAP-systeem een is on-premises geïmplementeerd of deze is geïmplementeerd in Azure. U kunt echter de verschillende systemen van SAP-landschap dat in Azure of on-premises implementeren. U kunt bijvoorbeeld implementeert de SAP CRM-ontwikkeling en testen van systemen in Azure, maar de SAP CRM productie system on-premises.
* Alleen-cloud-implementatie: een implementatie waarbij het Azure-abonnement niet is verbonden via een site-naar-site of een ExpressRoute-verbinding met de on-premises netwerkinfrastructuur. In algemene documentatie voor Azure dit soort implementaties worden ook beschreven als 'Cloud-Only'-implementaties. Virtuele Machines die worden geïmplementeerd met deze methode zijn toegankelijk via het Internet en het openbare Internet eindpunten die zijn toegewezen aan de virtuele machines in Azure. De on-premises Active Directory (AD) en DNS is niet uitgebreid naar Azure in deze typen implementaties. De virtuele machines zijn daarom geen onderdeel van de on-premises Active Directory. Opmerking: Alleen-implementaties in dit document worden gedefinieerd als complete SAP-landschappen, die worden uitgevoerd in Azure zonder de extensie van Active Directory of de naamomzetting van on-premises naar openbare cloud. Alleen-cloud-configuraties worden niet ondersteund voor productie SAP-systemen of configuraties waarbij SAP stm of andere on-premises bronnen worden gebruikt tussen die worden gehost op Azure en bronnen die zich moeten on-premises SAP-systemen.
* Cross-Premises: Beschrijving van een scenario waarbij virtuele machines worden geïmplementeerd naar een Azure-abonnement dat site-naar-site, meerdere locaties of ExpressRoute-connectiviteit tussen de on-premises clientresources en Azure. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als Cross-Premises scenario's. De reden voor de verbinding is om de on-premises domeinen, on-premises Active Directory en DNS-on-premises uitbreiden naar Azure. Het on-premises-landschap is uitgebreid naar de Azure-assets van het abonnement. Met deze extensie, kunnen de virtuele machines deel uitmaken van de on-premises domein. Gebruikers van een domein van de on-premises domein toegang tot de servers en services kunnen worden uitgevoerd op deze VM's (zoals DBMS-services). On-premises communicatie en naamomzetting tussen VM's geïmplementeerd en virtuele machines die worden geïmplementeerd in Azure is mogelijk. We verwachten dat dit de meest voorkomende scenario voor het implementeren van SAP-assets op Azure. Zie voor meer informatie, [in dit artikel] [ vpn-gateway-cross-premises-options] en [in dit artikel][vpn-gateway-site-to-site-create].

> [!NOTE]
> Cross-Premises implementaties van SAP-systemen waarbij Azure Virtual Machines met SAP-systemen lid van een on-premises domein zijn worden ondersteund voor productie SAP-systemen. Cross-Premises configuraties worden ondersteund voor het implementeren van onderdelen of uitvoeren van SAP-landschappen in Azure. De volledige SAP-landschap dat zelfs uitvoeren in Azure vereist dat deze virtuele machines die deel uit van de on-premises domein en ADVERTENTIES. In vorige versies van de documentatie, we hebben het gehad over hybride IT-scenario's waarin de term *hybride* verankerd ligt in het feit dat er een cross-premises-connectiviteit tussen on-premises en Azure. In dit geval *hybride* betekent ook dat de virtuele machines in Azure deel uit van de on-premises Active Directory maken.
> 
> 

Sommige Microsoft-documentatie worden Cross-Premises scenario's iets anders, met name voor DBMS HA configuraties beschreven. In het geval van de SAP-gerelateerde documenten, wordt de Cross-Premises scenario koken op een site-naar-site of privé (ExpressRoute)-verbinding en het feit dat de SAP-landschap gedistribueerd tussen on-premises en Azure.

### <a name="resources"></a>Resources
De volgende handleidingen zijn beschikbaar voor implementaties van SAP op Azure:

* [Azure virtuele Machines, planning en implementatie van SAP NetWeaver][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP NetWeaver][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver (in dit document)][dbms-guide]

De volgende SAP-opmerkingen zijn gerelateerd aan SAP op Azure:

| Houd er rekening mee getal | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen op Azure: ondersteunde producten en Azure-VM-typen |
| [2015553] |SAP op Microsoft Azure: vereisten voor ondersteuning |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [1409604] |Virtualisatie in Windows: uitgebreide bewaking |
| [2191498] |SAP op Linux met Azure: uitgebreide bewaking |
| [2039619] |SAP-toepassingen op Microsoft Azure met behulp van de Oracle-Database: ondersteunde producten en versies |
| [2233094] |DB6: SAP-toepassingen op Azure met behulp van IBM DB2 voor Linux, UNIX- en Windows - als u meer informatie |
| [2243692] |Linux op Microsoft Azure (IaaS) virtuele machine: problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [2002167] |Red Hat Enterprise Linux 7.x: installatie en Upgrade |
| [2069760] |Oracle Linux 7.x SAP-installatie en Upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |
| [2171857] |Oracle Database 12c - ondersteuning van het bestandssysteem in Linux |
| [1114181] |Oracle Database 11g - ondersteuning van het bestandssysteem in Linux |


Lees ook de [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) die bevat alle SAP-opmerkingen voor Linux.

U hebt een praktische kennis over de Microsoft Azure-architectuur en hoe Microsoft Azure Virtual Machines zijn geïmplementeerd en worden beheerd. U vindt meer informatie <https://azure.microsoft.com/documentation/>

> [!NOTE]
> We zijn **niet** bespreken van Microsoft Azure-Platform als een Service (PaaS)-aanbiedingen van het Microsoft Azure-Platform. In dit artikel is over het uitvoeren van een databasebeheersysteem (DBMS) in Microsoft Azure Virtual Machines (IaaS) als u wilt de DBMS-systemen worden uitgevoerd in uw on-premises omgeving. Database-functies en functionaliteiten tussen deze twee aanbiedingen zijn zeer verschillend en moeten niet worden verward met elkaar. Zie ook: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Omdat we van IaaS bespreken, zijn in het algemeen de installatie van Windows, Linux en DBMS-systemen en configuratie in wezen hetzelfde als een virtuele machine of bare metal machine hebt, installeert u on-premises. Er zijn echter enkele architectuur en system management beslissingen voor de implementatie, die verschillen bij gebruik van IaaS. Het doel van dit document is om uit te leggen van de specifieke architectuur en de system management verschillen die u moet worden voorbereid voor bij het gebruik van IaaS.

In het algemeen zijn de algemene gebieden van verschil dat in dit artikel wordt beschreven:

* Planning van de juiste virtuele machine/schijf-indeling van de SAP-systemen om ervoor te zorgen dat u de juiste gegevens lay-out bestands- en kunnen maar liefst voldoende IOP's voor uw workload.
* Netwerkoverwegingen bij het gebruik van IaaS.
* Specifieke database-functies gebruiken om u te optimaliseren van de database-indeling.
* Overwegingen voor back-up en herstel in IaaS.
* Met behulp van verschillende soorten afbeeldingen voor implementatie.
* Hoge beschikbaarheid in Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Structuur van de implementatie van een RDBMS
Als u wilt volgen dit hoofdstuk, is het nodig zijn om te begrijpen wat is opgegeven [dit] [ deployment-guide-3] hoofdstuk van de [Deployment Guide][deployment-guide]. Informatie over de verschillende VM-serie en hun verschillen en de verschillen tussen Azure Standard en Premium-opslag moet worden begrepen en bekende voordat dit hoofdstuk wordt gelezen.

Tot en met maart 2015 zijn de schijven, die een besturingssysteem bevatten beperkt tot 127 GB groot. Deze beperking is opgeheven in maart 2015 (voor meer informatie of <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). Van daaruit op schijven die het besturingssysteem bevat, kan het even groot is als een andere schijf hebben. Echter liever we nog steeds een structuur van waar het besturingssysteem, de DBMS-systemen en de uiteindelijke SAP-binaire bestanden staan los van de bestanden van de implementatie. Daarom verwachten we SAP-systemen die zijn uitgevoerd in Azure Virtual Machines hebben de base VM (of de logische schijf), zoals geïnstalleerd met het besturingssysteem, de database management systeem uitvoerbare bestanden en de SAP-uitvoerbare bestanden. De DBMS-gegevens en logboekbestanden bestanden worden opgeslagen in Azure Storage (Standard of Premium Storage) in de afzonderlijke schijven en als logische schijven die zijn gekoppeld aan de oorspronkelijke Azure besturingssysteemkopie VM. 

Afhankelijk van het gebruik van Azure Standard of Premium-Opslagaccount (bijvoorbeeld met behulp van de DS-reeks of GS-serie VM's) er zijn andere quota's in Azure, die worden beschreven [hier (Linux)] [ virtual-machines-sizes-linux] en [ Hier (Windows)][virtual-machines-sizes-windows]. Bij het plannen van de schijfindeling, moet u de beste balans van de quota voor de volgende items zoeken:

* Het aantal gegevensbestanden.
* Het aantal schijven dat de bestanden bevatten.
* De quota IOPS van één schijf.
* De gegevensdoorvoer per schijf.
* Het aantal extra gegevensschijven mogelijk per VM-grootte.
* Een virtuele machine in de algemene opslagdoorvoer kan bieden.

Azure dwingt een quotum IOP's per gegevensschijf. Deze quota zijn verschillend voor schijven die worden gehost op Azure Standard-opslag- en Premium-opslag. I/o-latenties zijn ook zeer verschillende tussen de twee opslagtypen met Premium Storage factoren leveren betere i/o-latenties. Elk van de verschillende typen van de virtuele machine heeft een beperkt aantal gegevensschijven dat u kunt koppelen. Een andere beperking is dat alleen bepaalde typen VM's kunnen gebruikmaken van Azure Premium Storage. Dit betekent dat de oplossing is voor een bepaald type van de virtuele machine kan niet alleen worden aangestuurd door de CPU en geheugen eisen, maar ook door de IOPS-waarde, latentie en schijf doorvoer vereisten die meestal worden geschaald met het aantal schijven of het type Premium Storage-schijven. Met name met Premium Storage kan de grootte van een schijf ook worden bepaald door het aantal IOPS en doorvoer die moet worden bereikt door elke schijf.

Het feit dat de algehele frequentie van IOP's, het aantal schijven gekoppeld en de grootte van de virtuele machine zijn alle aan elkaar gekoppeld, kan leiden tot een Azure-configuratie van een SAP-systeem anders dan de on-premises implementatie. De limieten IOP's per LUN worden doorgaans geconfigureerd in on-premises implementaties. Terwijl deze limieten met Azure Storage vaste of zoals in de Premium Storage afhankelijk van het schijftype zijn. Met on-premises implementaties zien we dus klant configuraties van database-servers die van veel verschillende volumes voor speciale uitvoerbare bestanden, zoals SAP en de DBMS-systemen of speciale volumes voor tijdelijke databases of tabel spaties gebruikmaken. Wanneer deze een on-premises systeem wordt verplaatst naar Azure, kan dit leiden tot een verspilling van mogelijke IOPS bandbreedte door verspilling van een schijf voor uitvoerbare bestanden of databases, met één of geen een groot aantal IOPS niet uitvoert. Daarom in Azure VM's wordt aangeraden dat de DBMS-systemen en SAP uitvoerbare bestanden worden geïnstalleerd op de besturingssysteemschijf indien mogelijk.

De plaatsing van bestanden van de database en logboekbestanden en het type Azure-opslag gebruikt, moeten worden gedefinieerd door IOPS, latentie en doorvoer vereisten. Om voldoende IOPS aan voor het transactielogboek, mogelijk afgedwongen gebruikmaken van meerdere schijven voor het logboekbestand van de transactie of er een grotere Premium-opslag-schijf. In dat geval zou een een software-RAID (voor voorbeeld van de Windows-opslag van toepassingen voor Windows of MDADM en LVM (Logical Volume Manager) voor Linux) met de schijven, het transactielogboek met bouwen.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Station D:\ in een Azure VM is een niet-persistente-station, die wordt ondersteund door sommige lokale schijven op de Azure compute-knooppunt. Omdat het niet-persistente, betekent dit dat alle wijzigingen aan de inhoud op het station D:\ gaat verloren wanneer de virtuele machine opnieuw wordt opgestart. "Wijzigingen" bedoelen we opgeslagen bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure-VM's automatisch een station koppelen aan /mnt/resource die is een niet-persistente station ondersteund door lokale schijven op de Azure compute-knooppunt. Omdat het niet-persistente, betekent dit dat alle wijzigingen aan de inhoud in /mnt/resource gaan verloren wanneer de virtuele machine opnieuw wordt opgestart. De wijzigingen bedoelen we opgeslagen bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> 

- - -
Afhankelijk van de Azure VM-serie, de lokale schijven op het rekenknooppunt weergeven verschillende prestaties, die kan worden ingedeeld, zoals:

* A0-A7: Zeer beperkte prestaties. Kan niet worden gebruikt voor alles wat dan wisselbestand van windows
* A8-A11: Kenmerken van zeer goede prestaties met sommige tienduizend IOPS en > 1GB per seconde doorvoer
* Uit de D-serie: Kenmerken van zeer goede prestaties met sommige tienduizend IOPS en > 1GB per seconde doorvoer
* DS-serie: Kenmerken van zeer goede prestaties met sommige tienduizend IOPS en > 1GB per seconde doorvoer
* G-serie: Kenmerken van zeer goede prestaties met sommige tienduizend IOPS en > 1GB per seconde doorvoer
* GS-serie: Kenmerken van zeer goede prestaties met sommige tienduizend IOPS en > 1GB per seconde doorvoer

Hierboven genoemde instructies zijn toegepast op de VM-typen die zijn gecertificeerd met SAP. De VM-serie met uitstekende IOPS en doorvoer in aanmerking komen voor gebruik door bepaalde DBMS-functies, zoals tempdb of tijdelijke tabelruimte.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>In cache opslaan voor virtuele machines en de gegevensschijven
Wanneer we gegevensschijven via de portal of wanneer we geüploade schijven op virtuele machines gekoppeld maken, kunnen we kiezen of de i/o-verkeer tussen de virtuele machine en deze schijven zich bevinden in Azure-opslag in cache zijn opgeslagen. Azure Standard en Premium Storage gebruik van twee verschillende technologieën voor dit type cache. In beide gevallen zou de cache zelf schijf back-ups op de dezelfde stations die worden gebruikt door de tijdelijke schijf (D:\ in Windows) of /mnt/resource op Linux, van de virtuele machine.

Voor Azure Standard-opslag zijn de mogelijke cache-typen:

* Geen caching
* Opslaan in cache lezen
* Lezen en schrijven in cache opslaan

Om consistente en deterministisch prestaties, moet u instellen de cache op Azure Standard-opslag voor alle schijven met **DBMS-gerelateerde gegevensbestanden, logboekbestanden en ruimte voor tabel 'NONE'**. Het in cache plaatsen van de virtuele machine kunt blijven met de standaardinstellingen.

Azure Premium Storage bestaat de volgende opties voor opslaan in cache:

* Geen caching
* Opslaan in cache lezen

Aanbeveling voor Azure Premium Storage is om gebruik te **Lees-caching voor gegevensbestanden** van de SAP-database en vervolgens **niet in cache opslaan voor de schijven van een of meer logboekbestanden**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Software-RAID
Zoals hierboven aangegeven, moet u het aantal IOP's die nodig zijn voor de databasebestanden voor het aantal schijven die u kunt configureren in balans brengen en de maximale IOPS een virtuele machine van Azure biedt per schijf of schijftype voor Premium Storage. Eenvoudigste manier om op te lossen met de IOPS-belasting over schijven is om een software-RAID over de verschillende schijven te bouwen. Vervolgens plaatst u een aantal gegevensbestanden van de SAP DBMS in de LUN's oppervlaktevariaties buiten de software-RAID. Afhankelijk van de vereisten dat kunt u rekening houden met het gebruik van Premium Storage ook sinds twee van de drie verschillende Premium Storage-schijven bieden hogere IOPS quotum dan schijven op basis van de Standard-opslag. Naast de aanzienlijk betere i/o latentie geleverd door Azure Premium Storage. 

Hetzelfde geldt voor het transactielogboek van de verschillende DBMS-systemen. Met veel van deze meer Tlog bestanden toe te voegen werkt niet omdat de DBMS-systemen voor het schrijven naar een van de bestanden op slechts een keer. Als hogere IOPS tarieven nodig zijn dan één Standard-opslag op basis van schijf kan leveren, kunt u stripe via meerdere Standard Storage-schijven of kunt u een grotere schijftype voor Premium-opslag die buiten de hogere IOPS tarieven zorgt ook voor factoren lagere latentie voor het schrijven ik / Besturingssysteem in het transactielogboek.

Er zijn situaties voorkomen in met Azure-implementaties, die met behulp van een software-RAID zou dit:

* Logboek voor databasetransacties Log/opnieuw vereisen meer IOPS dan Azure biedt voor één schijf. Zoals hierboven vermeld dit kan worden opgelost door het bouwen van een LUN over meerdere schijven met behulp van een software-RAID.
* Ongelijkmatig i/o werkbelasting verdeeld over de bestanden van verschillende gegevens over de SAP-database. In dergelijke gevallen kan een één gegevensbestand te maken met de quota in plaats daarvan vaak optreden. Terwijl andere bestanden niet, zelfs de dicht bij de IOPS-quotum van één schijf. In dergelijke gevallen is de eenvoudigste oplossing een LUN maken via meerdere schijven met behulp van een software-RAID. 
* U weet niet wat de exacte i/o-werkbelasting per gegevensbestand is en alleen ongeveer weet wat de totale IOPS-werkbelasting op basis van de DBMS-systemen is. Eenvoudigste te doen is het bouwen van een LUN met behulp van een software-RAID. De som van de quota van meerdere schijven achter deze LUN moet vervolgens te voldoen aan het bekende IOPS-tarief.

- - -
> ![Windows][Logo_Windows] Windows
> 
> U wordt aangeraden Windows opslagruimten gebruiken als u op Windows Server 2012 of hoger wordt uitgevoerd. Het is efficiënter dan Windows Striping van eerdere versies van Windows. Mogelijk moet u de Windows-opslaggroepen en opslagruimten maken met PowerShell-opdrachten bij het gebruik van Windows Server 2012 als besturingssysteem. De PowerShell-opdrachten vindt u hier <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Alleen MDADM en LVM (Logical Volume Manager) worden ondersteund voor het bouwen van een software-RAID op Linux. Lees de volgende artikelen voor meer informatie:
> 
> * [Software-RAID op Linux configureren] [ virtual-machines-linux-configure-raid] (voor MDADM)
> * [LVM configureren op een virtuele Linux-machine in Azure][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Overwegingen voor het gebruik van VM-serie, die kunnen werken met Azure Premium Storage meestal zijn:

* Vereisten voor i/o-latenties die zich dicht bij wat SAN/NAS-apparaten bieden.
* Aanvraag voor factoren betere i/o latentie dan Azure Standard-opslag kan leveren.
* Hogere IOPS per VM dan wat kan worden bereikt met meerdere Standard Storage-schijven op basis van een bepaald type van de virtuele machine.

Aangezien de onderliggende Azure-opslag elke schijf aan ten minste drie opslagknooppunten, eenvoudige RAID 0 striping kan worden gebruikt repliceert. Er is niet nodig om RAID 5 of RAID1 te implementeren.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage
Microsoft Azure Storage slaat de basis-VM (met OS) en de schijven of de BLOBs aan ten minste drie afzonderlijke opslagknooppunten. Bij het maken van een storage-account of een beheerde schijf, is er een ruime keuze aan beveiliging zoals hier wordt weergegeven:

![Geo-replicatie is ingeschakeld voor Azure Storage-account][dbms-guide-figure-100]

Azure Storage lokale-replicatie (lokaal Redundant) biedt niveaus van beveiliging tegen gegevensverlies vanwege fout bij de infrastructuur die enkele klanten verlenen kunnen om te implementeren. Zoals u hierboven ziet er zijn vier verschillende opties met een vijfde wordt een variant van een van de eerste drie. Die dichter bij ze willen we kunnen onderscheid maken:

* **Premium lokaal redundante opslag (LRS)**: Azure Premium Storage voorziet in ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines waarop I/O-intensieve werkbelastingen worden uitgevoerd. Er zijn drie replica's van de gegevens binnen de dezelfde Azure-datacenter van een Azure-regio. De exemplaren worden in verschillende domeinen en domeinen bijwerken (voor Zie concepten [dit] [ planning-guide-3.2] hoofdstuk in de [Planningshandleiding][planning-guide]). In het geval van een replica van de gegevens die vanuit service vanwege een fout in Opslagknooppunt of schijffout, wordt er een nieuwe replica automatisch gegenereerd.
* **Lokaal redundante opslag (LRS)**: In dit geval zijn er drie replica's van de gegevens binnen de dezelfde Azure-datacenter van een Azure-regio. De exemplaren worden in verschillende domeinen en domeinen bijwerken (voor Zie concepten [dit] [ planning-guide-3.2] hoofdstuk in de [Planningshandleiding][planning-guide]). In het geval van een replica van de gegevens die vanuit service vanwege een fout in Opslagknooppunt of schijffout, wordt er een nieuwe replica automatisch gegenereerd. 
* **Geografisch redundante opslag (GRS)**: In dit geval wordt er is een asynchrone replicatie die een extra drie replica's van de gegevens in een andere Azure-regio, dat zich in de meeste gevallen in hetzelfde geografische gebied (zoals Noord-Europa en West-Europa ). Dit resulteert in drie extra replica's, zodat er zes replica's in sum zijn. Een variant van dit is een aanvulling waar de gegevens in de geografisch gerepliceerde Azure-regio kunnen worden gebruikt voor lezen (Read-Access Geo-Redundant).
* **Zone-redundante opslag (ZRS)**: In dit geval de drie replica's van de gegevens blijven in dezelfde Azure-regio. Zoals uitgelegd in [dit] [ planning-guide-3.1] hoofdstuk van de [Planningshandleiding] [ planning-guide] een Azure-regio is een aantal datacenters in de nabijheid. In het geval van LRS zou de replica's worden verdeeld over de verschillende datacenters die ene Azure-regio maken.

Meer informatie vindt [hier][storage-redundancy].

> [!NOTE]
> DBMS-implementaties, wordt het gebruik van geografisch redundante opslag niet aanbevolen
> 
> Azure Storage-Geo-replicatie is asynchroon. Replicatie van afzonderlijke schijven die zijn gekoppeld aan een enkele virtuele machine zijn niet gesynchroniseerd in de stap vergrendelen. Het is daarom niet geschikt voor het repliceren van DBMS-bestanden die zijn verdeeld over verschillende schijven of geïmplementeerd op basis van een software-RAID op basis van meerdere schijven. DBMS-software is vereist dat de permanente schijfopslag nauwkeurig worden gesynchroniseerd binnen andere LUN's en onderliggende schijven/aandrijfassen. DBMS-software maakt gebruik van verschillende mechanismen waarmee u kunt takenreeksen i/o-schrijfbewerkingen activiteiten en een DBMS rapporteert dat de schijfopslag die de replicatie is gericht als deze zelfs door enkele milliseconden verschillen is beschadigd. Daarom als een wil dat de configuratie van een database met een database gespreid over meerdere schijven geo-replicatie, zoals een replicatie moet worden uitgevoerd met de database weg en functionaliteit. Een verstandig op Azure Storage Geo-replicatie om uit te voeren van deze taak is niet. 
> 
> Het probleem is het het gemakkelijkst om uit te leggen met een voorbeeld-systeem. Stel, dat u hebt een SAP-systeem dat is geüpload naar Azure, met acht schijven gegevensbestanden van de DBMS-systemen met plus één schijf met het transactielogboek. Elk van deze negen schijven een zijn gegevens die naar ze zijn geschreven in een consistente methode op basis van het DBMS, of de gegevens naar de logboekbestanden gegevens of de transactie wordt geschreven.
> 
> Goed hierop geo-replicatie de gegevens en onderhouden van de installatiekopie van een consistente database, de inhoud van alle schijven van de negen zelf zou moeten worden geo-replicatie in de exacte volgorde de i/o-bewerkingen zijn uitgevoerd op basis van de negen verschillende schijven. Azure Storage-geo-replicatie kunnen echter niet op te geven van afhankelijkheden tussen schijven. Dit betekent dat Microsoft Azure Storage geo-replicatie niet weten over het feit dat de inhoud van deze negen verschillende schijven aan elkaar zijn gerelateerd en dat de gewijzigde gegevens consistent zijn alleen wanneer de i/o-bewerkingen te repliceren in de volgorde is er gebeurd voor alle de negen schijven.
> 
> Naast de kans op hoge dat de installatiekopieën van geo-replicatie in het scenario bieden een consistente database-installatiekopie, is er een op de prestaties die wordt weergegeven met geografisch redundante opslag die ernstige problemen kunnen ook worden de prestaties beïnvloeden. Kortom, gebruik geen dit type opslagredundantie voor DBMS type werkbelastingen.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>VHD's wilt toewijzen aan virtuele Machine van Azure Service Storage-Accounts
In dit hoofdstuk is alleen van toepassing op Azure Storage-Accounts. Als u van plan bent om beheerde schijven te gebruiken, zijn de beperkingen die worden vermeld in dit hoofdstuk niet van toepassing. Lees voor meer informatie over Managed Disks, hoofdstuk [Managed Disks] [ dbms-guide-managed-disks] van deze handleiding.

Een Azure Storage-Account is niet alleen een met beheerdersrechten om voor te bereiden, maar ook een onderwerp met beperkingen. Terwijl de beperkingen, is afhankelijk van of we het hebben over een Azure Standard Storage-Account of een Azure Premium Storage-Account. De exacte mogelijkheden en beperkingen worden vermeld [hier][storage-scalability-targets]

Dus voor Azure Standard-opslag is het belangrijk te weten er geldt een limiet op de IOP's per opslagaccount (rij waarin **totale snelheid van aanvragen voor** in [het artikel][storage-scalability-targets]). Er is bovendien een initiële limiet van 100 Opslagaccounts per Azure-abonnement (vanaf juli 2015). Het verdient daarom IOPS van de virtuele machines balans tussen meerdere opslagaccounts bij het gebruik van Azure Standard-opslag. Terwijl een enkele virtuele machine in het ideale geval maakt gebruik van één opslagaccount indien mogelijk. Als we het hebben over DBMS-implementaties waarin elke VHD die wordt gehost op Azure Standard-opslag de quotalimiet kan bereiken, moet u dus alleen 30-40 VHD's per Azure-Opslagaccount die gebruikmaakt van Azure Standard-opslag implementeren. Anderzijds, als u gebruikmaken van Azure Premium Storage en voor het opslaan van grote database volumes, u mogelijk fijn in termen van IOPS. Maar er is een Azure Premium Storage-Account manier meer beperkende in gegevensvolume dan een Standard Storage-Account van Azure. Als gevolg hiervan kunt u slechts een beperkt aantal VHD's binnen een Azure Premium Storage-Account implementeren voordat u de limiet voor het volume van gegevens te maken. Denkt u aan het einde van een Azure Storage-Account als een 'virtueel SAN' die mogelijkheden in IOPS en/of capaciteit beperkte met. Als gevolg hiervan blijft de taak, zoals in de on-premises implementaties voor het definiëren van de indeling van de VHD's van de verschillende SAP-systemen over de verschillende 'denkbeeldige SAN-apparaten' of de Azure Storage-Accounts.

Voor Azure Standard-opslag, wordt dit niet aanbevolen om weer te geven van de opslag uit verschillende opslagaccounts aan een enkele virtuele machine indien mogelijk.

Wanneer u de DS of GS-serie virtuele Azure-machines, is het mogelijk om te koppelen VHD's uit de Azure Storage-Accounts en Premium Storage-Accounts. Use-cases, zoals het schrijven van back-ups in de Standard-opslag ondersteund VHD's en DBMS-gegevens en logboekbestanden op Premium Storage worden geleverd aan waarmee u rekening moet waar deze heterogene opslag kan worden gebruikt. 

Op basis van implementaties van klanten en testen van ongeveer 30 tot 40 VHD's met gegevensbestanden van de database en logboekbestanden kunnen worden ingericht op een enkele Azure Standard Storage-Account met aanvaardbare prestaties. Zoals eerder vermeld, is de beperking van een Azure Premium Storage-Account is waarschijnlijk de gegevenscapaciteit die erin kunnen worden opgenomen en niet IOPS.

Als met SAN-apparaten on-premises, delen is vereist enige controle om te kunnen detecteren uiteindelijk knelpunten in een Azure Storage-Account. De Azure Monitoring-extensie voor SAP en Azure portal zijn hulpprogramma's die kunnen worden gebruikt voor het detecteren van bezet Azure Storage-Accounts die mogelijk worden leveren van suboptimale i/o-prestaties.  Als deze situatie wordt aangetroffen, wordt het aanbevolen bezette VM's verplaatsen naar een andere Azure Storage-Account. Raadpleeg de [Deployment Guide] [ deployment-guide] voor meer informatie over het activeren van de SAP mogelijkheden voor bewaking host.

Een ander artikel aanbevolen procedures voor Azure Standard Storage en Azure Storage-Accounts worden samengevat vindt u hier <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Beheerde schijven
Managed Disks is een nieuwe brontype in Azure Resource Manager die kunnen worden gebruikt in plaats van VHD's die zijn opgeslagen in Azure Storage-Accounts. Beheerde schijven automatisch uitgelijnd met de Beschikbaarheidsset van de virtuele machine die zijn gekoppeld aan en dus de beschikbaarheid van uw virtuele machine en de services die worden uitgevoerd op de virtuele machine te verhogen. Lees voor meer informatie de [overzichtsartikel](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

SAP ondersteunt momenteel alleen Premium Managed Disks. SAP-notitie lezen [1928533] voor meer informatie.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>DBMS virtuele machines van Azure Standard-opslag naar Azure Premium Storage verplaatsen geïmplementeerd
We optreden een aantal scenario's waar u als klant van een geïmplementeerde virtuele machine van Azure Standard-opslag te verplaatsen naar Azure Premium Storage. Als uw schijven zijn opgeslagen in Azure Storage-Accounts, is dit niet mogelijk zonder de gegevens fysiek te verplaatsen. Er zijn verschillende manieren om het doel te bereiken:

* U kunt alle VHD's, basis-VHD, evenals gegevens-VHD's kopiëren naar een nieuwe Azure Premium Storage-Account. Vaak kiest u het aantal VHD's in Azure Standard-opslag niet vanwege het feit dat u het aantal gegevens nodig is. Maar u die nodig zijn dat veel VHD's vanwege de IOPS-waarde. Nu dat u naar Azure Premium Storage dat kan u minder VHD's verplaatsen voor de dezelfde IOP's doorvoer met manier gaat. Gezien het feit dat in Azure Standard Storage u voor de gegevens die worden gebruikt en niet de nominale schijfgrootte betaalt, het aantal VHD's niet van belang in termen van kosten. Echter, zou u met Azure Premium Storage ook betalen voor de nominale schijfgrootte. Daarom probeer de meeste van de klanten te houden het aantal Azure-VHD's in Premium-opslag bij het nummer die nodig zijn voor het bereiken van de IOPS-doorvoer die nodig zijn. Dus de meeste klanten ervoor kiezen de manier van een eenvoudige 1:1 exemplaar.
* Als nog niet is gekoppeld, kunt u één VHD met een databaseback-up van uw SAP-database kunt koppelen. Na de back-up, moet u alle VHD's, met inbegrip van de VHD met de back-up te ontkoppelen en de basis-VHD en de VHD met de back-up kopiëren naar een Azure Premium Storage-account. U zou vervolgens de virtuele machine op basis van de basis-VHD implementeren en de VHD koppelen met de back-up. U kunt nu extra lege Premium Storage-schijven maken voor de virtuele machine die worden gebruikt voor het herstellen van de database in. Hierbij wordt ervan uitgegaan dat de DBMS-systemen kunt u wijzigen van paden naar de gegevens en logboekbestanden bestanden als onderdeel van het herstelproces.
* Een andere mogelijkheid is een variatie van het vorige proces, waar u de back-up VHD kopiëren naar Azure Premium Storage en koppelt deze op basis van een virtuele machine die u zojuist hebt geïmplementeerd en geïnstalleerd.
* De vierde mogelijkheid u kiest wanneer u het nodig zijn om het aantal gegevensbestanden van de database te wijzigen. In dat geval voert u een SAP homogene system kopiëren met behulp van exporteren/importeren. Put die bestanden exporteren naar een VHD die is gekopieerd naar een Azure Premium Storage-Account en deze koppelen aan een virtuele machine die u gebruikt om uit te voeren van de processen voor het importeren. Klanten gebruiken deze mogelijkheid, vooral wanneer ze willen Verminder het aantal gegevensbestanden.

Als u Managed Disks gebruikt, kunt u migreren naar Premium Storage door:

1. Toewijzing van de virtuele machine ongedaan maken
1. Indien nodig, grootte van de virtuele machine een grootte die ondersteuning biedt voor Premium Storage (bijvoorbeeld DS of GS)
1. Het type beheerde schijf wijzigen naar Premium (SSD)
1. Wijzig de caching van de gegevensschijven, zoals aanbevolen in hoofdstuk [in cache opslaan voor virtuele machines en de gegevensschijven][dbms-guide-2.1]
1. Uw virtuele machine starten

### <a name="deployment-of-vms-for-sap-in-azure"></a>Implementatie van virtuele machines voor SAP in Azure
Microsoft Azure biedt verschillende manieren om virtuele machines en gekoppelde schijven te implementeren. Daardoor is het belangrijk om te weten wat de verschillen sinds de voorbereiding van de virtuele machines kunnen verschillen afhankelijk van de manier van implementatie. In het algemeen wordt behandeld in de scenario's beschreven in de volgende hoofdstukken.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Een virtuele machine in Azure Marketplace
U wilt Microsoft of derden opgegeven installatiekopie van de Azure Marketplace om uw VM te implementeren. Nadat u de virtuele machine in Azure geïmplementeerd, volgt u dezelfde richtlijnen en hulpprogramma's voor het installeren van de SAP-software in uw virtuele machine net zoals u in een on-premises-omgeving doen zou. Voor het installeren van de SAP-software in de Azure-VM, SAP en Microsoft aanbevelen uploaden en opslaan van de SAP-installatiemedia in schijven of om te maken van een virtuele machine van Azure werkt als een 'File server', die alle benodigde SAP-installatiemedia bevat.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Een virtuele machine met een klantspecifieke gegeneraliseerde installatiekopie
De opgegeven in de Azure Marketplace-installatiekopieën mogelijk vanwege een patch voor specifieke vereisten met betrekking tot uw besturingssysteem of de DBMS-versie, niet aan uw behoeften voldoen. Daarom moet u mogelijk een virtuele machine maken met uw eigen 'persoonlijke' OS/DBMS-VM-installatiekopie, die kan worden geïmplementeerd als meerdere keren daarna. Om voor te bereiden die een 'persoonlijke' installatiekopie voor het kopiëren, moet het besturingssysteem op de on-premises virtuele machine worden gegeneraliseerd. Raadpleeg de [Deployment Guide] [ deployment-guide] voor meer informatie over hoe u een virtuele machine generaliseren.

Als u al SAP-inhoud in uw on-premises VM (met name voor Tier-2-systemen) hebt geïnstalleerd, kunt u de instellingen van het SAP aanpassen nadat de implementatie van de virtuele Azure-machine via het exemplaar procedure ondersteund door het SAP Software inrichting Manager (SAP wijzigen Houd er rekening mee [1619720]). U kunt anders de SAP-software installeren nadat de implementatie van de Azure-VM.

Als van de database-inhoud die wordt gebruikt door de SAP-toepassing, u de inhoud opnieuw worden door een SAP-installatie genereren kunt of u kunt uw inhoud in Azure importeren kunt met behulp van een VHD met een DBMS databaseback-up of door gebruik te maken van de mogelijkheden van de DBMS rechtstreeks back-up in  Microsoft Azure Storage. In dit geval kan u ook voorbereiden VHD's met de DBMS-systemen voor gegevens en logboekbestanden bestanden on-premises en die als schijven vervolgens importeren in Azure. Maar de overdracht van gegevens van de DBMS-systemen, die wordt geladen vanaf on-premises naar Azure via een VHD-schijven die worden voorbereid moeten on-premises zou moeten werken.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd
U van plan bent om te verplaatsen van een specifieke SAP-systeem van on-premises naar Azure (lift- and -shift). Dit kan worden gedaan door het uploaden van de schijf die het besturingssysteem, de SAP-binaire bestanden, en de uiteindelijke DBMS-binaire bestanden bevat plus de schijven met de gegevens en logboekbestanden bestanden van de DBMS-systemen naar Azure. In in scenario #2 hierboven, u de hostnaam, de SAP-SID en de SAP-gebruikersaccounts in de Azure-VM zoals ze zijn geconfigureerd in de on-premises omgeving. Generaliseren van de installatiekopie is daarom niet nodig. Dit geldt vooral voor Cross-Premises scenario's waarbij een deel van de SAP-landschap dat on-premises en onderdelen wordt uitgevoerd op Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Hoge beschikbaarheid en herstel na noodgeval met Azure-VM 's
Azure biedt de volgende High Availability (HA) en herstel na noodgevallen (DR)-functies die van toepassing op de verschillende onderdelen we voor SAP- en DBMS-implementaties gebruiken

### <a name="vms-deployed-on-azure-nodes"></a>Virtuele machines die worden geïmplementeerd op Azure-knooppunten
Het Azure-Platform biedt functies zoals livemigratie voor geïmplementeerde VM's. Dit betekent dat als er onderhoud nodig op een cluster waarop een virtuele machine is geïmplementeerd is, moet de VM gestopt en opnieuw opgestart. Onderhoud in Azure wordt uitgevoerd met zogenaamde Upgrade-domeinen binnen clusters van servers. Slechts één domein met een upgrade uitvoert op een tijdstip wordt onderhouden. Tijdens deze opnieuw is opgestart is het een onderbreking van de service terwijl de virtuele machine wordt afgesloten, onderhoud wordt uitgevoerd en virtuele machine opnieuw opgestart. De meeste DBMS-leveranciers bieden echter functionaliteit voor hoge beschikbaarheid en herstel na noodgevallen die snel de DBMS-services op een ander knooppunt opnieuw wordt opgestart, als het primaire knooppunt niet beschikbaar is. Het Azure-Platform biedt de functionaliteit voor het distribueren van virtuele machines, opslag- en andere Azure-services in meerdere domeinen bijwerken om ervoor te zorgen dat gepland onderhoud of infrastructuur fouten alleen gevolgen voor een kleine subset van virtuele machines of services hebben zou.  Met een zorgvuldige planning kijken, is het mogelijk om te realiseren beschikbaarheidsniveaus die vergelijkbaar is met on-premises infrastructuren.

Microsoft Azure-Beschikbaarheidssets worden een logische groepering van virtuele machines of Services die ervoor zorgt dat virtuele machines en andere services worden gedistribueerd naar verschillende domeinen en Upgrade-domeinen binnen een cluster dat alleen één knooppunt wordt uitgeschakeld op een bepaald tijdstip wordt ( lezen[deze (Linux)] [ virtual-machines-manage-availability-linux] of [deze (Windows)] [ virtual-machines-manage-availability-windows] artikel voor meer informatie).

Het moet worden geconfigureerd door doel bij het implementeren van virtuele machines, zoals hier wordt weergegeven:

![Definitie van de Beschikbaarheidsset voor DBMS HA-configuraties][dbms-guide-figure-200]

Als we willen maken van maximaal beschikbare configuraties van DBMS-implementaties (onafhankelijk van de afzonderlijke DBMS HA-functionaliteit die wordt gebruikt), moet de DBMS-VM's naar:

* De virtuele machines toevoegen aan hetzelfde Azure-netwerk (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* De virtuele machines van de HA-configuratie moet ook in hetzelfde subnet. Naamomzetting tussen de verschillende subnetten is niet mogelijk in alleen-implementaties, alleen IP-oplossing werkt. Met site-naar-site- of ExpressRoute-connectiviteit voor Cross-Premises implementaties, is een netwerk met ten minste één subnet al gemaakt. Naamomzetting is uitgevoerd op basis van de on-premises AD-beleidsregels en netwerkinfrastructuur. 



#### <a name="ip-addresses"></a>IP-adressen
Het is raadzaam om in te stellen van de virtuele machines voor HA-configuraties in een flexibele manier. Vertrouwen op IP-adressen aan de HA partner (s) in de configuratie van de HA-adres is niet betrouwbaar zijn in Azure, tenzij de statische IP-adressen worden gebruikt. Er zijn twee 'Shutdown' concepten in Azure:

* Afsluiten via Azure portal of Azure PowerShell-cmdlet Stop-AzureRmVM: In dit geval wordt de virtuele Machine wordt afgesloten en toewijzing ongedaan gemaakt. Uw Azure-account wordt niet meer in rekening gebracht voor deze virtuele machine zodat alleen de kosten in rekening worden gebracht voor de opslag die wordt gebruikt. Als het privé IP-adres van de netwerkinterface niet statisch is, het IP-adres wordt vrijgegeven en is er geen garantie dat de netwerkinterface het oude IP-adres toegewezen opnieuw na het opnieuw opstarten van de virtuele machine wordt. Het afsluiten omlaag uitvoeren via de Azure-portal of door het aanroepen van Stop-AzureRmVM automatisch zorgt ervoor dat de toewijzing ongedaan maken. Als u niet wilt dat deze toewijzing ongedaan maken van het gebruik van machine Stop-AzureRmVM - StayProvisioned 
* Als u de virtuele machine van een OS-niveau afsluit, wordt de virtuele machine afsluiten en niet ongedaan maken is toegewezen. Echter, in dit geval uw Azure-account is nog steeds kosten in rekening gebracht voor de virtuele machine, ondanks het feit dat deze afgesloten wordt. In dat geval zijn de toewijzing van het IP-adres aan een gestopte virtuele machine blijft intact. Afsluiten van de virtuele machine uit binnen niet automatisch voor dat toewijzing ongedaan maken.

Zelfs voor Cross-Premises scenario's, standaard afsluiten en de toewijzing ongedaan maken betekent dat de toewijzing ongedaan maken van de IP-adressen van de virtuele machine, zelfs als on-premises-beleid in de DHCP-instellingen zijn verschillend. 

* De uitzondering wordt als een statisch IP-adres aan een netwerkinterface als toewijst beschreven [hier][virtual-networks-reserved-private-ip].
* In dat geval is het IP-adres blijft vast zolang de netwerkinterface is niet verwijderd.

> [!IMPORTANT]
> Zorgen dat de hele implementatie eenvoudig en te beheren, wordt de wissen aanbeveling is het instellen van de virtuele machines samenwerking in een DBMS HA of DR-configuratie in Azure op een manier die er een werkende naamomzetting tussen de verschillende virtuele machines die betrokken zijn is.
> 
> 

## <a name="deployment-of-host-monitoring"></a>Implementatie van de Host controleren
SAP vereist voor productief gebruik van SAP-toepassingen in Azure Virtual Machines, de mogelijkheid om op te halen van de host bewakingsgegevens van de fysieke hosts waarop de virtuele Machines van Azure wordt uitgevoerd. Er is een specifieke Host-Agent voor SAP-patchniveau vereist waarmee deze mogelijkheid in SAPOSCOL en SAP Host-Agent. De exacte patchniveau wordt beschreven in de SAP-notitie [1409604].

Raadpleeg voor de details met betrekking tot implementatie van onderdelen die hostgegevens aan SAPOSCOL en SAP-Hostagent leveren en het beheer van de levenscyclus van deze onderdelen, de [Implementatiehandleiding][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Specifieke informatie naar Microsoft SQL Server
### <a name="sql-server-iaas"></a>SQL Server IaaS
Beginnen met Microsoft Azure, kunt u eenvoudig migreren uw bestaande SQL Server-toepassingen die is gebouwd op Windows Server-platform op Azure Virtual Machines. SQL Server in een virtuele Machine kunt u de totale eigendomskosten van implementatie, beheer en onderhoud van de breedte van bedrijfstoepassingen verminderen door eenvoudig deze toepassingen met Microsoft Azure te migreren. Met SQL Server in een Azure Virtual machines, beheerders en ontwikkelaars kunnen nog steeds gebruiken de dezelfde ontwikkelings- en beheerhulpprogramma's die on-premises beschikbaar zijn. 

> [!IMPORTANT]
> We zijn niet bespreken van Microsoft Azure SQL Database, die is een Platform als een Service-aanbieding van de Microsoft Azure-Platform. De discussie in dit artikel is over het uitvoeren van de SQL Server-product als oplossing staat bekend om on-premises implementaties in Azure Virtual Machines, gebruik te maken van de infrastructuur als een Service-mogelijkheden van Azure. Database-functies en functionaliteiten tussen deze twee aanbiedingen zijn verschillend en moeten niet worden verward met elkaar. Zie ook: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Het verdient aanbeveling om te controleren [dit] [ virtual-machines-sql-server-infrastructure-services] documentatie voordat u doorgaat.

In de volgende secties worden onderdelen van onderdelen van de documentatie onder de bovenstaande koppeling samengevoegd en die worden vermeld. Specifieke informatie over SAP worden ook vermeld en enkele concepten in meer detail worden beschreven. Echter is het raadzaam om te werken door de documentatie boven eerste voordat u de documentatie van SQL Server-specifieke leest.

Er is een SQL-Server in IaaS specifieke gegevens die u voordat u doorgaat weten moet:

* **Virtual Machine SLA**: Er is een SLA voor virtuele Machines die worden uitgevoerd in Azure, die u kunt hier vinden: <https://azure.microsoft.com/support/legal/sla/>  
* **Ondersteuning voor SQL-versie**: voor SAP-klanten, wij ondersteuning voor SQL Server 2008 R2 en hoger op Microsoft Azure-Machine. Eerdere versies worden niet ondersteund. Bekijk deze algemene [ondersteuningsverklaring](https://support.microsoft.com/kb/956893) voor meer informatie. Houd er rekening mee dat in het algemeen SQL Server 2008 wordt ondersteund door Microsoft ook. Maar vanwege een belangrijke functionaliteit voor SAP, die is geïntroduceerd in SQL Server 2008 R2, SQL Server 2008 R2 de minimale versie voor SAP is. Houd er rekening mee dat SQL Server 2012 en 2014 is uitgebreid met diepere integratie in de IaaS-scenario (zoals back-ups van rechtstreeks op basis van Azure Storage). Dus beperken we in dit document tot SQL Server 2012 en 2014 met de meest recente patchniveau voor Azure.
* **SQL-functieondersteuning**: meest SQL Server-functies op Microsoft Azure Virtual Machines met enkele uitzonderingen worden ondersteund. **SQL Server Failover Clustering met gedeelde schijven wordt niet ondersteund**.  Gedistribueerd technologieën zoals databasespiegeling, AlwaysOn Availability Groups, replicatie, upfunctie en Service Broker binnen één Azure-regio worden ondersteund. SQL Server AlwaysOn ook wordt ondersteund tussen verschillende Azure-regio's zoals hier wordt beschreven: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Controleer de [ondersteuningsverklaring](https://support.microsoft.com/kb/956893) voor meer informatie. Een voorbeeld over het implementeren van een AlwaysOn-configuratie wordt weergegeven in [dit] [ virtual-machines-workload-template-sql-alwayson] artikel. Bekijk ook de aanbevolen procedures beschreven [hier][virtual-machines-sql-server-infrastructure-services] 
* **SQL-prestaties**: We zijn ervan overtuigd dat Microsoft Azure gehoste virtuele Machines heel goed in vergelijking met andere openbare cloud virtualization aanbiedingen, maar de afzonderlijke resultaten uitvoeren kunnen variëren. Bekijk [dit] [ virtual-machines-sql-server-performance-best-practices] artikel.
* **Met installatiekopieën uit Azure Marketplace**: de snelste manier om een nieuwe Microsoft Azure-VM implementeren is het gebruik van een installatiekopie van de Azure Marketplace. Er zijn in de Azure Marketplace-installatiekopieën die SQL Server bevatten. De afbeeldingen waarop SQL Server al is geïnstalleerd worden niet onmiddellijk gebruikt voor SAP NetWeaver-toepassingen. De reden is dat de standaardsortering van SQL Server is geïnstalleerd in de afbeeldingen en niet de sortering die zijn vereist voor SAP NetWeaver-systemen. Als u wilt gebruiken die afbeeldingen, controleert u de stappen die zijn beschreven in het hoofdstuk [met behulp van een installatiekopie van SQL Server buiten de Microsoft Azure Marketplace][dbms-guide-5.6]. 
* Bekijk [prijsinformatie](https://azure.microsoft.com/pricing/) voor meer informatie. De [SQL Server 2012-licentiehandleiding](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) en [SQL Server 2014-licentieverlening Guide](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) zijn ook een belangrijke bron.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>De richtlijnen voor SQL Server voor SAP-gerelateerde SQL Server-installaties in Azure VM 's
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Aanbevelingen voor virtuele machine/VHD-structuur voor SAP-gerelateerde SQL Server-implementaties
In overeenstemming met de algemene beschrijving uitvoerbare bestanden van SQL Server moet zich bevindt of in het systeemstation van de besturingssysteemschijf van de virtuele machine geïnstalleerd (station C:\).  De meeste van de databases van SQL Server system zijn normaal gesproken niet gebruikt op hoog niveau voor SAP NetWeaver-werkbelasting. Daarom kunnen de systeemdatabases van SQL Server (master, msdb en model) blijven op het station C:\ ook. Een uitzondering mogelijk tempdb, die in het geval van een enkele SAP ERP- en alle BW werkbelastingen kan vereisen hogere gegevensvolume of i/o-bewerkingen volume, die niet in de oorspronkelijke virtuele machine passen. Voor dergelijke systemen, moeten de volgende stappen worden uitgevoerd:

* De primaire tempdb-gegevensbestanden naar hetzelfde logische station bevinden als de primaire gegevensbestanden van de SAP-database verplaatsen.
* Eventuele aanvullende tempdb-gegevensbestanden toevoegen aan elk van de andere logische schijven met een bestand met de SAP-database.
* De tempdb-logboekbestand toevoegen aan de logische schijf, waarin het logboekbestand van de gebruikersdatabase.
* **Exclusief voor VM-typen die gebruikmaken van de lokale SSD's** op de compute-knooppunt tempdb gegevens en logboekbestanden bestanden op het station D:\ kunnen worden geplaatst. Het kan echter worden aanbevolen om het gebruik van meerdere tempdb-gegevensbestanden. Let erop D:\ stationsvolumes verschillen op basis van het type virtuele machine.

Deze configuraties inschakelen tempdb te verbruiken meer ruimte dan het systeemstation is kunnen bieden. Om te bepalen van de grootte van de juiste tempdb, kunt een controleren of de tempdb-grootten op bestaande systemen, die on-premises uitvoert. Bovendien zou een dergelijke configuratie de IOPS-nummers op basis van tempdb die kan niet worden voorzien van het systeemstation inschakelen. Systemen die on-premises worden uitgevoerd kunnen opnieuw worden gebruikt voor het bewaken van i/o-werkbelasting op basis van tempdb zodat u de IOPS-nummers die u verwacht te zien op uw tempdb kunt afleiden.

Een VM-configuratie, die SQL Server wordt uitgevoerd met een SAP-database en waarin het tempdb-gegevensbestanden en tempdb-logboekbestand worden geplaatst op het station D:\ zou er als volgt uitzien:

![Naslaginformatie over configuratie van Azure IaaS-VM voor SAP][dbms-guide-figure-300]

Let op het station D:\ is afhankelijk van het type virtuele machine van verschillende grootten. Afhankelijk van de vereiste grootte van tempdb u wordt mogelijk afgedwongen voor het paar tempdb gegevens en logboekbestanden bestanden met de SAP-database gegevens en logboekbestanden bestanden in gevallen waarbij D:\ station te klein is.

#### <a name="formatting-the-disks"></a>De schijven formatteren
Voor SQL Server het NTFS blokgrootte voor schijven met SQL Server-gegevens en logboekbestanden moet 64 kB. Er is niet nodig om de schijf D:\ te formatteren. Dit station wordt vooraf ingestelde geleverd.

Om ervoor te zorgen dat het terugzetten of het maken van databases is niet tijdens de initialisatie van de bestanden door het licht van de inhoud van de bestanden, moet een ervoor zorgen dat de context van de gebruiker die de SQL Server-service wordt uitgevoerd in een bepaalde machtiging heeft. Meestal hebben gebruikers in de groep Windows-beheerder deze machtigingen. Als de SQL Server-service wordt uitgevoerd in de gebruikerscontext van niet - Windows-beheerder, moet u het recht voor de gebruiker toewijzen **onderhoudstaken op volume uitvoeren**.  Zie de details in deze Microsoft Knowledge Base-artikel: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Gevolgen van het database-compressie
In configuraties waar i/o-bandbreedte een beperkende factor kan worden, kan elke meting, waardoor IOP's helpen om te vergroten van de werkbelasting een kan worden uitgevoerd in een IaaS-scenario, zoals Azure. Daarom als nog niet gedaan, wordt toepassen van de pagina SQL Server-compressie aanbevolen door zowel Microsoft als SAP voordat u een bestaande SAP-database uploadt naar Azure.

De aanbeveling om uit te voeren van compressie van de Database voordat u uploadt naar Azure wordt gegeven uit twee redenen:

* De hoeveelheid gegevens moet worden geüpload is lager.
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat een sterkere hardware met meer CPU's of hogere i/o-bandbreedte of minder i/o latentie on-premises gebruiken kunt.
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Database compressie werkt ook in een Azure Virtual Machines als on-premises. Kom hier voor meer informatie over het comprimeren van een bestaande SAP SQL Server-database: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014---storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 - opslag databasebestanden rechtstreeks in Azure Blob-opslag
SQL Server 2014 wordt de mogelijkheid voor het opslaan van databasebestanden rechtstreeks in Azure Blob Store zonder de 'wrapper' van een VHD omheen geopend. Hierdoor kunnen scenario's waar u de grenzen van IOPS dat zou worden afgedwongen door een beperkt aantal schijven die kunnen worden gekoppeld aan bepaalde kleinere VM-typen kunt strijden tegen met name bij het gebruik van standaard Azure Storage of kleinere VM-typen. Dit werkt voor gebruikersdatabases echter niet voor systeemdatabases van SQL Server. Het werkt ook voor gegevens en logboekbestanden van SQL Server. Als u wilt implementeren van een SAP SQL Server-database op deze manier in plaats van 'wrapping' laten naar VHD's, het volgende in gedachten:

* Het Opslagaccount gebruikte moet zich in dezelfde Azure-regio bevinden als de naam die wordt gebruikt voor het implementeren van de virtuele machine SQL-Server wordt uitgevoerd in.
* Overwegingen met betrekking tot eerder vermelde met betrekking tot de distributie van VHD's die via andere Azure Storage-Accounts gelden voor deze methode ook implementaties. Betekent dat het aantal i/o-bewerkingen op basis van de grenzen van de Azure Storage-Account.

[comment]: <> (MSSedusch TODO maar Hiermee worden gebruikt. de netwerkbandbreedte en geen opslag bandbreedte, toch?)

Meer informatie over dit type implementatie worden hier weergegeven: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

Als u wilt opslaan in SQL Server-gegevensbestanden rechtstreeks in Azure Premium Storage, moet u beschikken over een minimale SQL Server 2014 patch release, die hier wordt beschreven: <https://support.microsoft.com/kb/3063054>. Opslaan van SQL Server-gegevensbestanden op Azure Standard-opslag werkt in combinatie met de uitgebrachte versie van SQL Server 2014. De dezelfde patches bevat echter een andere reeks oplossingen waarmee u het direct gebruik van Azure Blob-opslag voor SQL Server-gegevensbestanden en back-ups betrouwbaarder. Daarom wordt u aangeraden deze patches in het algemeen.

### <a name="sql-server-2014-buffer-pool-extension"></a>De Buffergroepuitbreiding van SQL Server 2014
SQL Server 2014 geïntroduceerd voor een nieuwe functie, die de Buffergroepuitbreiding wordt genoemd. Deze functionaliteit wordt uitgebreid voor de buffergroep van SQL Server, die wordt opgeslagen in het geheugen met een tweede niveau cache die wordt ondersteund door de lokale SSD van een server of virtuele machine. Hierdoor kunnen houden van een grotere werkset van gegevens 'in het geheugen'. In vergelijking met toegang tot Azure Standard-opslag is de toegang tot de extensie van de buffergroep die is opgeslagen op lokale SSD van een virtuele machine van Azure veel factoren sneller.  Gebruik te maken van het lokale station D:\ van de VM-typen die uitstekende IOPS en doorvoer zijn kan dus een heel redelijke manier om de belasting IOP's op basis van Azure Storage te verminderen en de reactietijd van query's aanzienlijk verbeteren. Dit geldt met name als u niet met behulp van Premium Storage. In het geval van Premium Storage en het gebruik van de Cache Premium Azure lezen in het rekenknooppunt, zoals aanbevolen voor gegevensbestanden, worden er zijn geen belangrijke verschillen verwacht. Reden is dat beide caches (SQL Server-Buffergroepuitbreiding en Premium Storage-lezen Cache) van de lokale schijven van de rekenknooppunten gebruikmaakt.
Controleer deze documentatie voor meer informatie over deze functionaliteit: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Overwegingen voor back-up/herstel voor SQL Server
Bij het implementeren van SQL Server naar Azure moet worden gecontroleerd op uw back-upmethode. Zelfs als het systeem niet een productieve systeem is, moet de SAP-database die wordt gehost door de SQL Server worden back-ups regelmatig. Omdat Azure Storage kan drie afbeeldingen voldoen, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een storage compenserende. De reden van de prioriteit voor het onderhouden van een juiste back-up en recovery-abonnement nog meer dingen die u logische/handmatige fouten compenseren kunt door punt in tijd herstel mogelijkheden te bieden. Zodat het doel om een van beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of is het gebruik van de back-ups in Azure als seeding voor een ander systeem door het kopiëren van de bestaande database. Bijvoorbeeld, u kan de overdracht van een Tier-2-SAP-configuratie naar een 3-Laagse system-installatie van hetzelfde systeem door terug te zetten van een back-up.

Er zijn drie verschillende manieren om back-up van SQL Server naar Azure Storage:

1. SQL Server 2012 CU4 en hoger kunt systeemeigen back-up van databases naar een URL. Dit wordt beschreven in de blog [nieuwe functionaliteit in SQL Server 2014 - deel 5 - back-up/herstellen verbeterde](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Zie hoofdstuk [SQL Server 2012 SP1 CU4 of hoger][dbms-guide-5.5.1].
2. SQL Server-versies vóór SQL 2012 CU4 kunnen een omleidingsfunctionaliteit gebruiken om back-up op een VHD en de stroom schrijven in feite verplaatsen naar een Azure-opslaglocatie die is geconfigureerd. Zie hoofdstuk [SQL Server 2012 SP1 CU3 en oudere releases][dbms-guide-5.5.2].
3. De laatste methode is het uitvoeren van een traditionele SQL Server back-up schijfopdracht op een schijfapparaat. Dit is gelijk aan het patroon van on-premises implementatie en niet in dit document gedetailleerd wordt besproken.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 of hoger
Deze functie kunt u rechtstreeks back-up naar Azure BLOB-opslag. Zonder deze methode, u moet back-up op andere schijven die u voor schijf- en IOPS-capaciteit gebruiken wilt. Het idee is in feite dit:

 ![Met behulp van back-up van SQL Server 2012 naar Microsoft Azure Storage-BLOB][dbms-guide-figure-400]

In dit geval is het voordeel dat een niet hoeft te besteden aan schijven voor het opslaan van back-ups van SQL Server op. Daarom moet u minder schijven die zijn toegewezen en de hele bandbreedte van schijf-IOPS kan worden gebruikt voor gegevens en logboekbestanden. Houd er rekening mee dat de maximale grootte van een back-up beperkt tot maximaal 1 TB, is zoals beschreven in de sectie **beperkingen** in dit artikel: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. Als de back-up, ondanks het gebruik van SQL Server-back-compressie zou groter zijn dan 1 TB in grootte, de functionaliteit die worden beschreven in het hoofdstuk [SQL Server 2012 SP1 CU3 en oudere releases] [ dbms-guide-5.5.2] moet worden in dit document gebruikt.

[Verwante documentatie](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) niet terug te zetten rechtstreeks vanuit Azure BLOB-archief als de back-up met een beschrijving van het terugzetten van databases voor back-ups op basis van Azure Blob Store raden > 25 GB. De aanbeveling in dit artikel is gebaseerd op de prestatie-overwegingen en niet als gevolg van functionele beperkingen. Daarom kunnen verschillende voorwaarden van toepassing op een per geval beoordeeld.

Documentatie over hoe dit type back-up is ingesteld en gebruikt kan worden gevonden in [dit](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) zelfstudie

Een voorbeeld van de volgorde van de stappen kan worden gelezen [hier](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Back-ups automatiseren, is het van het hoogste belang om ervoor te zorgen dat de naam van de BLOBs voor elke back-up anders. Als ze worden overschreven en de restore-keten is verbroken.

Om niet te combineren om dingen tussen de drie verschillende typen back-ups, is het raadzaam om verschillende containers onder het opslagaccount die wordt gebruikt voor back-ups te maken. De containers kunnen worden door virtuele machine alleen of met virtuele machine en back-uptype. Het schema kan er als volgt uitzien:

 ![Met behulp van back-up van SQL Server 2012 naar Microsoft Azure Storage-BLOB - verschillende containers onder afzonderlijke Storage-Account][dbms-guide-figure-500]

In het bovenstaande voorbeeld zou de back-ups niet worden uitgevoerd in hetzelfde opslagaccount waarin de virtuele machines zijn geïmplementeerd. Er is een nieuw opslagaccount, speciaal voor de back-ups. In de storage-accounts, zou er verschillende containers die zijn gemaakt met een matrix van het type back-up en de naam van de virtuele machine. Dergelijke segmentering wordt het gemakkelijker te beheren van de back-ups van de verschillende virtuele machines.

De BLOBs die een rechtstreeks schrijft u de back-ups, zijn geen schijven toe te voegen aan het aantal van de gegevens van een virtuele machine. Daarom een kan het maximum van gegevensschijven gekoppeld van de specifieke VM-SKU voor de gegevens te maximaliseren en transactie-logboekbestand en een back-up op basis van een storage-container nog steeds worden uitgevoerd. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 en oudere releases
De eerste stap die u moet uitvoeren om te realiseren van een back-up rechtstreeks op basis van Azure Storage zou worden voor het downloaden van het MSI-bestand, dat is gekoppeld aan [dit](https://www.microsoft.com/download/details.aspx?id=40740) KBA artikel.

Download de x64-installatiebestand en de documentatie. Het bestand installeert een programma met de naam: **Microsoft SQL Server back-up naar Microsoft Azure-hulpprogramma**. Raadpleeg de documentatie van het product aandachtig door.  Het hulpprogramma werkt in feite in de volgende manier:

* Aan de SQL Server, de locatie van een schijf voor de SQL Server back-up is gedefinieerd (het station D:\ niet gebruiken als locatie).
* Het hulpprogramma kunt u voor het definiëren van regels, die kunnen worden gebruikt om te leiden van verschillende typen back-ups naar andere Azure Storage-containers.
* Zodra de regels voldaan is, leidt het hulpprogramma voor de stroom van het schrijven van de back-up naar een van de VHD's /-schijven naar de Azure-opslaglocatie, die eerder is gedefinieerd.
* Het hulpprogramma verlaat een kleine stub-bestand met een paar KB's op de VHD/de schijf die is gedefinieerd voor de SQL Server back-up. **Dit bestand moet op de opslaglocatie blijven omdat deze opnieuw terug te zetten naar Azure-opslag is vereist.**
  * Als u de stubbestand (bijvoorbeeld via verlies van de opslagmedia die deel uitmaakt van het stubbestand) is verbroken en u ervoor de mogelijkheid om back gekozen hebt-up op een Microsoft Azure Storage-account, kunt u het stubbestand via de Microsoft Azure Storage kan herstellen downloaden in de opslagcontainer waarin deze is geplaatst. Plaats het stubbestand naar een map op de lokale computer waarop het hulpprogramma is geconfigureerd om te detecteren en te uploaden naar de dezelfde container met de dezelfde Versleutelingswachtwoord als versleuteling is gebruikt met de oorspronkelijke regel. 

Dit betekent dat het schema zoals hierboven beschreven voor een meer recente versies van SQL Server in de plaats ook voor SQL Server-versies die niet dat direct adres een Azure-opslaglocatie toestaat kan worden geplaatst.

Deze methode mag niet worden gebruikt bij een meer recente versies van SQL Server, die ondersteuning bieden voor back-ups maken van systeemeigen op basis van Azure Storage. Uitzonderingen zijn beperkingen van de systeemeigen back-up naar Azure blokkeren wanneer systeemeigen back-up kan worden uitgevoerd in Azure.

#### <a name="other-possibilities-to-back-up-sql-server-databases"></a>Andere mogelijkheden om back-up van SQL Server-databases
Andere mogelijkheden om back-up van databases is extra gegevensschijven koppelen aan een virtuele machine die u gebruikt voor het opslaan van back-ups op. In dat geval moet u om ervoor te zorgen dat de schijven worden niet uitgevoerd volledige. Als dit het geval is, moet u de schijven ontkoppelen en dus tot speak deze ' archiveren en vervang deze door een nieuwe lege schijf. Als u inzoomt op dat pad, u wilt voorkomen dat deze VHD's in afzonderlijke Azure Storage-Accounts die die de VHD's met bestanden van de database.

Een tweede mogelijkheid is het gebruik van een grote VM waarvoor veel schijven die zijn gekoppeld, kan bijvoorbeeld een D14 met 32VHDs. Gebruik opslagruimten om een flexibele omgeving waarin u shares die vervolgens worden gebruikt als back-updoelen voor de verschillende DBMS-servers kunt bouwen.

Enkele aanbevolen procedures hebt gedocumenteerd [hier](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) ook. 

#### <a name="performance-considerations-for-backupsrestores"></a>Prestatie-overwegingen voor back-ups/herstellen
Prestaties van back-up/herstel is als in de bare-metal implementaties, afhankelijk van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes mogelijk. Bovendien speelt de CPU-verbruik die worden gebruikt door de back-upcompressie een belangrijke rol op virtuele machines met maximaal 8 CPU-threads. Daarom kunt u ervan uitgaan:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de gegevens van bestanden, de kleinere de algehele doorvoer bij het lezen.
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de gevolgen van het back-compressie.
* De minder doelen (BLOBs, VHD's of schijven) schrijven van de back-up naar de laagste de doorvoer.
* De kleinere de VM-grootte, de kleinere de doorvoer opslaglimiet schrijven en lezen van het Azure Storage. Onafhankelijk van of de back-ups rechtstreeks worden opgeslagen in Azure Blob of of ze worden opgeslagen in de virtuele harde schijven die opnieuw worden opgeslagen in Azure-Blobs.

Wanneer u een Microsoft Azure Storage-BLOB als de back-updoel in recentere versies, bent u beperkt tot het toewijzen van slechts één URL-doel voor elke specifieke back-up.

Maar als u de "Microsoft SQL Server back-up naar Microsoft Azure-hulpprogramma' in oudere versies, kunt u meer dan één doel voor het bestand opgeven. De back-up kan worden geschaald met meer dan één doel, en de doorvoer van de back-up is hoger. Dit zou leiden tot vervolgens meerdere bestanden ook in de Azure Storage-account. In de testen kunt met behulp van meerdere bestemmingen van het bestand u zeker bereiken de doorvoer, wat u met de back-extensies die zijn geïmplementeerd in SQL Server 2012 SP1 CU4 op kan realiseren. U ook niet worden geblokkeerd door de limiet van 1TB zoals in de systeemeigen back-up naar Azure.

Houd er rekening mee echter, de doorvoer is afhankelijk van de locatie van de Azure Storage-Account die u voor de back-up gebruikt. Een idee mogelijk naar het opslagaccount niet vinden in een andere regio dan de virtuele machines worden uitgevoerd in. Bijvoorbeeld gebruikt u de VM-configuratie worden uitgevoerd in West-Europa, maar het Opslagaccount dat u om te gebruiken zetten back-up op basis van in Noord-Europa. Die zeker heeft een invloed op de back-doorvoer en waarschijnlijk niet lijkt te zijn in gevallen waar de doel-opslagaccount en de virtuele machines worden uitgevoerd in hetzelfde regionaal datacenter mogelijk een doorvoer van 150MB per seconde genereren.

#### <a name="managing-backup-blobs"></a>Back-up BLOBs beheren
Er is een vereiste voor het beheren van de back-ups op uw eigen. Aangezien de verwachting is dat veel blobs zijn gemaakt door het uitvoeren van regelmatige transactielogboekback-ups, kunt beheer van deze blobs eenvoudig overbelast Azure portal. Het is daarom recommendable gebruikmaken van een Azure storage explorer. Er zijn enkele goede die beschikbaar is, kunt voor het beheren van een Azure storage-account

* Microsoft Visual Studio met Azure-SDK geïnstalleerd (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure Storage Explorer (<https://azure.microsoft.com/downloads/>)
* Hulpprogramma's van derden

Voor een uitgebreidere bespreking van de back-up en SAP op Azure, raadpleegt u [de SAP-handleiding voor back-up](sap-hana-backup-guide.md) voor meer informatie.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Met behulp van een installatiekopie van SQL Server buiten de Microsoft Azure Marketplace
Microsoft biedt virtuele machines in de Azure Marketplace, die al versies van SQL Server bevatten. Voor SAP-klanten die behoefte hebben aan licenties voor SQL Server en Windows, kan dit een kans om te kunnen de noodzaak van licenties in feite krijgen door virtuele machines met SQL Server is al geïnstalleerd zijn. Als u wilt gebruiken die afbeeldingen voor SAP, moeten de volgende overwegingen worden gemaakt:

* De versies van SQL Server niet kan worden beoordeeld aan te schaffen hogere kosten dan een 'Windows-only' virtuele machine geïmplementeerd vanuit Azure Marketplace. Zie de volgende artikelen om te vergelijken prijzen: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> en <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* U kunt alleen SQL Server-versies die worden ondersteund door SAP, zoals SQL Server 2012 gebruiken.
* De sortering van de SQL Server-exemplaar is geïnstalleerd in de virtuele machines die worden aangeboden in de Azure Marketplace is niet de sortering die SAP NetWeaver vereist de SQL Server-exemplaar om uit te voeren. U kunt de sortering wijzigen met de richtlijnen in de volgende sectie.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Wijzigen van de SQL Server-sortering van een Microsoft Windows/SQL Server-VM
Aangezien de SQL Server-installatiekopieën in de Azure Marketplace zijn niet ingesteld voor het gebruik van de sortering die nodig is voor SAP NetWeaver-toepassingen, moet onmiddellijk na de implementatie niet worden gewijzigd. Voor SQL Server 2012, kunt dit doen met de volgende stappen uit als u de virtuele machine is geïmplementeerd en er is een beheerder aanmelden bij de geïmplementeerde virtuele machine:

* Open een Windows-opdrachtvenster als beheerder.
* Wijzig de map in C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* De opdracht niet uitvoeren: Setup.exe/quiet/Action = REBUILDDATABASE InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION SQL_Latin1_General_Cp850_BIN2 =   
  * `<local_admin_account_name`> is de account die is gedefinieerd als het administrator-account bij het implementeren van de virtuele machine voor het eerst via de galerie.

Het proces duurt slechts enkele minuten. Als u wilt controleren of de stap is het juiste resultaat, moet u de volgende stappen uitvoeren:

* Open SQL Server Management Studio.
* Open een queryvenster.
* Voer de opdracht sp_helpsort in de hoofddatabase van SQL Server.

Het gewenste resultaat er moet uitzien:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Als dit niet het resultaat is, stopt de implementatie van SAP en onderzoeken waarom de setup-opdracht niet werkt zoals verwacht. Implementatie van SAP NetWeaver-toepassingen naar SQL Server-exemplaar met verschillende SQL Server-codepagina's dan de hierboven genoemde is **niet** ondersteund.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server hoge beschikbaarheid voor SAP in Azure
Zoals eerder in dit document is vermeld, is er geen mogelijkheid om te maken van gedeelde opslag die nodig is voor het gebruik van de functionaliteit voor hoge beschikbaarheid van oudste SQL Server. Deze functionaliteit kan twee of meer exemplaren van SQL Server installeren in een Windows Server Failover Cluster (WSFC) met behulp van een gedeelde schijf voor de gebruikersdatabases (en uiteindelijk tempdb). Dit is de methode voor het standaard hoge beschikbaarheid van lange tijd, die ook wordt ondersteund door SAP. Omdat Azure biedt geen ondersteuning voor gedeelde opslag, kunnen niet SQL Server-configuraties voor hoge beschikbaarheid met een configuratie van het cluster gedeelde schijf worden gerealiseerd. Veel andere methoden voor hoge beschikbaarheid zijn echter nog steeds mogelijk en worden beschreven in de volgende secties.

#### <a name="sql-server-log-shipping"></a>De logboekbestanden van SQL Server
Een van de methoden voor hoge beschikbaarheid (HA) is upfunctie van SQL Server. Als de virtuele machines die deel uitmaken van de HA-configuratie naamomzetting werkt hebt, is er geen probleem en de instellingen in Azure verschilt niet van alle instellingen die on-premises wordt uitgevoerd. Het verdient niet afhankelijk zijn van alleen IP-adresomzetting. Met betrekking tot het instellen van upfunctie en de principes met betrekking tot upfunctie, controleert u deze documentatie:

<https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server>

Als u wilt een hoge beschikbaarheid bereiken, moet één implementatie van de virtuele machines, die binnen een dergelijke upfunctie configuratie moet zich binnen de dezelfde Azure-Beschikbaarheidsset.

#### <a name="database-mirroring"></a>Databasespiegeling
Database spiegelen zoals ondersteund door SAP (Zie SAP-notitie [965908]) is afhankelijk van het definiëren van een failover-partner in de SAP-verbindingsreeks. Voor de gevallen Cross-Premises, gaan we ervan uit dat de twee virtuele machines zich in hetzelfde domein en dat de gebruiker context de twee SQL Server-exemplaren worden uitgevoerd onder een domein-gebruiker en beschikken over voldoende bevoegdheden in de twee SQL Server-exemplaren die betrokken zijn. De installatie van het spiegelen van de Database in Azure daarom niet verschillen tussen een typische on-premises setup/configuratie.

Vanaf de alleen-implementaties is de eenvoudigste methode dat setup met een ander domein in Azure om deze DBMS-VM's (en in het ideale geval toegewezen SAP-virtuele machines) binnen één domein.

Als een domein niet mogelijk is, kunt een certificaten ook gebruiken voor het spiegelen van de eindpunten zoals hier wordt beschreven: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Een zelfstudie voor het instellen van het spiegelen van de Database in Azure vindt u hier: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQL Server AlwaysOn
Zoals altijd op voor SAP on-premises wordt ondersteund (Zie SAP-notitie [1772688]), worden gebruikt in combinatie met SAP in Azure wordt ondersteund. Het feit dat u nog niet kunt maken van gedeelde schijven in Azure betekent niet dat een een altijd op Windows Server Failover Cluster (WSFC)-configuratie tussen verschillende virtuele machines kan niet maken. Het betekent alleen dat u de mogelijkheid om een gedeelde schijf gebruiken als een quorum in de configuratie van het cluster geen hebt. U kunt daarom een altijd op de WSFC-configuratie in Azure bouwen en het quorumtype dat gebruikmaakt van gedeelde schijf niet selecteren. De Azure-omgeving die virtuele machines worden geïmplementeerd moet oplossen die aan de virtuele machines met de naam en de virtuele machines moet in hetzelfde domein. Dit geldt voor Azure alleen en Cross-Premises implementaties. Er zijn enkele speciale overwegingen voor het implementeren van de SQL Server-Listener voor beschikbaarheidsgroep (niet te verwarren met de Azure-Beschikbaarheidsset) omdat Azure op dit moment niet toegestaan voor het maken van een AD-en DNS-object omdat deze mogelijk on-premises. Daarom zijn sommige stappen van de andere installatie nodig zijn om te strijden tegen het bepaald gedrag van Azure.

Er zijn enkele overwegingen met behulp van een Listener voor beschikbaarheidsgroep:

* Met behulp van een beschikbaarheidsgroep-Listener is alleen mogelijk met Windows Server 2012 of hoger als gast-OS van de virtuele machine. Voor Windows Server 2012 moet u ervoor zorgen dat deze patch wordt toegepast: <https://support.microsoft.com/kb/2854082> 
* Voor Windows Server 2008 R2, deze patch niet bestaat en altijd op moeten op dezelfde manier als het spiegelen van de Database worden gebruikt door een failoverpartner in de verbindingsreeks op te geven (gedaan via de SAP default.pfl parameter databases/mss/server - Zie SAP-notitie [965908]).
* Wanneer u een Listener voor de beschikbaarheidsgroep gebruikt, moet de Database-VM's zijn verbonden met een specifieke Load Balancer. Naamomzetting in alleen-implementaties, moet u alle virtuele machines van een SAP-systeem (toepassingsservers, DBMS-server en (A) SCS-server) in hetzelfde virtuele netwerk of het onderhoud van het bestand etc\host in volgorde van een SAP-toepassingslaag zou vereist ophalen van de VM-namen van de SQL Server-VM's is opgelost. Om te voorkomen dat nieuwe IP-adressen in gevallen waarin beide VM's incidenteel niet afsluiten is toewijzen van Azure, moet een toewijzen statische IP-adressen aan de netwerkinterfaces van de virtuele machines in de AlwaysOn-configuratie (een statisch IP-adres is beschreven in definiëren[dit] [ virtual-networks-reserved-private-ip] artikel)

[comment]: <> (Oude blogs)
[comment]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Er zijn speciale stappen zijn vereist wanneer het bouwen van de configuratie van het WSFC-cluster waar het cluster een speciale IP-adres moet toegewezen, omdat Azure met de huidige functionaliteit naam van het cluster hetzelfde IP-adres toewijzen zou als op het knooppunt van het cluster is gemaakt. Dit betekent dat een handmatige stap moet worden uitgevoerd als u wilt een ander IP-adres toewijzen aan het cluster.
* De beschikbaarheidsgroep-Listener is gaan in Azure worden gemaakt met de TCP/IP-eindpunten die zijn toegewezen aan de virtuele machines die de primaire en secundaire replica's van de beschikbaarheidsgroep wordt uitgevoerd.
* Er is mogelijk nodig voor het beveiligen van deze eindpunten met ACL's.

[comment]: <> (Oude TODO-blog)
[comment]: <> (De gedetailleerde stappen en de noodzakelijke items moeten meebrengen van het installeren van een AlwaysOn-configuratie in Azure zijn beste optreden bij het doorlopen van de zelfstudie beschikbaar [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[comment]: <> (Vooraf geconfigureerde AlwaysOn-installatie via de Azure gallery <https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Het maken van een beschikbaarheidsgroep-Listener is best wordt beschreven in de zelfstudie voor [this][virtual-machines-windows-classic-ps-sql-int-listener])
[comment]: <> (Beveiligen netwerkeindpunten met ACL's worden uitgelegd beste hier:)
[comment]: <> (* <https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx> )
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* <https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Het is mogelijk een SQL Server AlwaysOn-beschikbaarheidsgroep via andere Azure-regio's ook implementeren. Deze functionaliteit maakt gebruik van de Azure VNet-naar-Vnet-connectiviteit ([meer details][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Oude TODO-blog)
[comment]: <> (De installatie van SQL Server AlwaysOn Availability Groups in een dergelijk scenario wordt hier beschreven: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Overzicht van SQL Server met hoge beschikbaarheid in Azure
Gezien het feit dat de inhoud wordt beveiligd door Azure Storage, is er een minder reden te noemen op een hot stand-by-installatiekopie. Dit betekent dat uw scenario hoge beschikbaarheid moet alleen beveiligen op basis van de volgende gevallen:

* Niet beschikbaar zijn van de virtuele machine als geheel vanwege onderhoud op de server-cluster in Azure of andere redenen
* Softwareproblemen met in het SQL Server-exemplaar
* Beveiligen van handmatige fout waarbij gegevens wordt verwijderd en point-in-time-herstel is vereist

Overeenkomende technologieën een dat de eerste twee gevallen kunnen worden volstaan met databasespiegeling of is altijd ingeschakeld, stellen kan dat de derde geval alleen kan worden volstaan met back-upfunctie kijken.

U moet de complexe installatie van een Always On, vergeleken met het spiegelen van de Database, met de voordelen van het Always On in balans brengen. Die voordelen kunnen worden weergegeven, zoals:

* Leesbare secundaire replica's.
* Back-ups van secundaire replica's.
* Betere schaalbaarheid.
* Meer dan één secundaire replica's.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Algemene SQL Server voor SAP op Azure samenvatting
Er zijn veel aanbevelingen in deze handleiding en het is raadzaam dat u lees deze meer dan één keer voordat uw Azure-implementatie plannen. In het algemeen, moet u volgt de bovenste tien algemene DBMS in Azure specifieke punten:

[comment]: <> (2.3 hogere doorvoer dan wat? Dan één VHD?)
1. Gebruik de nieuwste versie van het DBMS-systemen, zoals SQL Server 2014, die de meeste voordelen in Azure heeft. Dit is voor SQL Server, SQL Server 2012 SP1 CU4, waaronder de functie van de back-ups verschil tussen Azure Storage. Echter, in combinatie met SAP verdient minst SQL Server 2014 SP1 CU1 of SQL Server 2012 SP2 en de meest recente CU gebruiken.
2. Uw SAP-landschap dat systeem in Azure om de gegevens bestand-indeling en de Azure-beperkingen in balans zorgvuldig plannen:
   * Niet te veel schijven hebben, maar hebt u genoeg om te controleren of dat u kunt de vereiste IOPS bereiken.
   * Als u Managed Disks niet gebruikt, vergeet dan niet IOP's zijn ook beperkt per Azure Storage-Account en Storage-Accounts geldt binnen elk Azure-abonnement ([meer details][azure-subscription-service-limits]). 
   * Alleen streep over schijven als u wilt een hogere doorvoer te realiseren.
3. Nooit software te installeren of bestanden waarvoor persistentie op station D:\, omdat het niet-permanente en alles op dit station verloren gaan op een Windows opnieuw opstarten is geplaatst.
4. Gebruik geen opslaan in schijfcache voor Azure Standard-opslag.
5. Gebruik geen Azure-Opslagaccounts van geo-replicatie.  Gebruik lokaal redundante voor DBMS-werkbelastingen.
6. Gebruik van de leverancier van uw DBMS HA-/ DR-oplossing voor het repliceren van gegevens uit een database.
7. Altijd naamomzetting gebruiken, Vertrouw niet alleen op IP-adressen.
8. Gebruik de hoogste compressie op de database mogelijk. Dit is de compressie van de pagina voor SQL Server.
9. Wees voorzichtig met behulp van SQL Server-installatiekopieën op Azure Marketplace. Als u de SQL Server gebruikt, moet u de sortering van het exemplaar voordat u een SAP NetWeaver-systeem installeert op deze wijzigen.
10. Installeren en configureren van de SAP-Host-bewaking voor Azure zoals wordt beschreven in [Deployment Guide][deployment-guide].

## <a name="specifics-to-sap-ase-on-windows"></a>Specifieke informatie naar SAP ASE op Windows
Beginnen met Microsoft Azure, kunt u eenvoudig uw bestaande SAP ASE toepassingen migreren naar Azure Virtual Machines. SAP ASE in een virtuele Machine kunt u de totale eigendomskosten van implementatie, beheer en onderhoud van de breedte van bedrijfstoepassingen verminderen door eenvoudig deze toepassingen met Microsoft Azure te migreren. Met SAP ASE in een Azure Virtual machines, beheerders en ontwikkelaars kunnen nog steeds gebruiken de dezelfde ontwikkelings- en beheerhulpprogramma's die on-premises beschikbaar zijn.

Er is een SLA voor de Azure Virtual Machines, die u kunt hier vinden: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

We zijn ervan overtuigd dat Microsoft Azure gehoste virtuele Machines uitvoert en in vergelijking met andere openbare cloud virtualization aanbiedingen, maar de afzonderlijke resultaten kunnen variëren. SAP formaat SAP's nummers van de verschillende SAP gecertificeerde VM-SKU's vindt u in een afzonderlijk SAP-notitie [1928533].

Instructies en aanbevelingen voor het gebruik van Azure Storage, de implementatie van SAP VM's of de SAP-bewaking van toepassing op implementaties van SAP ASE in combinatie met SAP-toepassingen zoals vermeld in de eerste vier hoofdstukken van dit document.

### <a name="sap-ase-version-support"></a>Ondersteuning voor SAP-versie van de as-omgeving
SAP op dit moment ondersteunt SAP ASE versie 16,0 voor gebruik met SAP Business Suite producten. Alle updates voor SAP ASE-server of JDBC en ODBC-stuurprogramma's voor gebruik met SAP Business Suite producten vindt u uitsluitend via SAP Service Marketplace op: <https://support.sap.com/swdc>.

Als voor installaties downloaden on-premises updates voor de SAP ASE-server, of de JDBC en ODBC-stuurprogramma's rechtstreeks vanuit Sybase websites niet. Voor gedetailleerde informatie over patches, die worden ondersteund voor gebruik met SAP Business Suite producten on-premises en in Azure Virtual Machines, Zie de volgende SAP-opmerkingen:

* [1590719]
* [1973241]

Algemene informatie over het uitvoeren van SAP Business Suite op SAP ASE vindt u de [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Richtlijnen voor SAP ASE voor SAP-gerelateerde SAP ASE installaties in Azure VM 's
#### <a name="structure-of-the-sap-ase-deployment"></a>Structuur van de SAP ASE-implementatie
In overeenstemming met de algemene beschrijving SAP ASE uitvoerbare bestanden moeten zich bevindt of geïnstalleerd in het systeemstation van de besturingssysteemschijf van de virtuele machine (station c:\). Normaal gesproken worden de meeste van de SAP ASE-systeem en hulpprogramma's voor databases niet gebruikt hard door SAP NetWeaver-werkbelasting. Daarom kunnen het systeem en hulpprogramma's voor databases (master, model, saptools, sybmgmtdb, sybsystemdb) blijven op het station C:\ ook. 

Een uitzondering wordt mogelijk de tijdelijke database die alle werktabellen en tijdelijke tabellen die zijn gemaakt door SAP ASE, die in het geval van sommige SAP ERP- en alle BW werkbelastingen vereisen kan hogere gegevensvolume of i/o-bewerkingen volume, die niet in de oorspronkelijke VM-besturingssysteem passen schijf (station c:\).

Afhankelijk van de versie van SAPInst/SWPM gebruikt voor het installeren van het systeem, kan de database bevatten:

* Een enkele SAP ASE tempdb, die wordt gemaakt bij het installeren van SAP ASE
* Een SAP ASE tempdb die zijn gemaakt door SAP ASE en een extra saptempdb die zijn gemaakt door de SAP-installatieprogramma installeren
* Een SAP ASE tempdb die zijn gemaakt door het installeren van SAP ASE en een aanvullende tempdb die handmatig is gemaakt (bijvoorbeeld volgende SAP-notitie [1752266]) om te voldoen aan specifieke tempdb ERP/BW

In het geval van specifieke ERP of alle BW werkbelastingen is het handig, met betrekking tot prestaties, zodat de tempdb-apparaten van de Daarnaast gemaakte tempdb (door SWPM of handmatig) op een ander station dan C:\. Als er geen aanvullende tempdb bestaat, is het raadzaam een te maken (SAP-notitie [1752266]).

Voor dergelijke systemen moeten de volgende stappen worden uitgevoerd voor de tempdb Daarnaast gemaakt:

* Het eerste tempdb-apparaat naar het eerste apparaat van de SAP-database verplaatsen
* Tempdb-apparaten toevoegen aan elk van de VHD's met een apparaat van de SAP-database

Deze configuratie kunt tempdb kunt gebruiken meer ruimte dan het systeemstation is kunnen bieden. Als uitgangspunt kunt een controleren of de tempdb-apparaat-grootten op bestaande systemen, die on-premises uitvoert. Of een dergelijke configuratie zou kunnen IOPS-nummers op basis van tempdb die kan niet worden voorzien van het systeemstation. Systemen die on-premises worden uitgevoerd kunnen opnieuw worden gebruikt voor het bewaken van i/o-werkbelasting op basis van tempdb.

Nooit plaats alle SAP ASE-apparaten op de schijf D:\ van de virtuele machine. Dit geldt ook voor de tempdb, zelfs als de objecten die zijn opgeslagen in de tempdb alleen tijdelijk zijn.

#### <a name="impact-of-database-compression"></a>Gevolgen van het Database-compressie
In configuraties waar i/o-bandbreedte een beperkende factor kan worden, kan elke meting, waardoor IOP's helpen om te vergroten van de werkbelasting een kan worden uitgevoerd in een IaaS-scenario, zoals Azure. Daarom is het raadzaam om ervoor te zorgen dat SAP ASE-compressie wordt gebruikt voor het uploaden van een bestaande SAP-database naar Azure.

De aanbeveling om uit te voeren van compressie voordat u uploadt naar Azure als deze is niet geïmplementeerd wordt verkregen uit verschillende redenen:

* De hoeveelheid gegevens worden geüpload naar Azure is lager
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat een sterkere hardware met meer CPU's of hogere i/o-bandbreedte of minder i/o latentie on-premises gebruiken kunt
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Compressie van gegevens en LOB werken in een virtuele machine die wordt gehost in Azure Virtual Machines, als er een on-premises. Voor meer informatie over hoe u om te controleren als compressie is al in gebruik in een bestaande SAP ASE-database, controleert u SAP-notitie [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit gebruikt voor het bewaken van Database-exemplaren
Voor SAP-systemen, die SAP ASE zijn als databaseplatform worden gebruikt, is de DBACockpit toegankelijk als ingesloten browservensters in transactie DBACockpit of Webdynpro. De volledige functionaliteit voor bewaking en beheer van de database is echter beschikbaar in de implementatie Webdynpro van alleen de DBACockpit.

Als met on-premises systemen zijn verschillende stappen vereist om alle SAP NetWeaver-functionaliteit die worden gebruikt door de Webdynpro uitvoering van de DBACockpit te schakelen. Ga als volgt de SAP-notitie [1245200] voor het gebruik van webdynpros inschakelen en het genereren van de vereiste velden. Wanneer u de instructies in de notities van de bovenstaande configureren u ook de Internet-Communicatiemanager (icm), samen met de poorten die worden gebruikt voor http en https-verbindingen. De standaardinstelling voor HTTP-ziet er zo uit:

> ICM/server_port_0 = b = HTTP, poort 8000, PROCTIMEOUT = = 600, time-out = 600
> 
> ICM/server_port_1 = b = HTTPS, poort 443$ $, PROCTIMEOUT = = 600, time-out = 600
> 
> 

en de koppelingen die zijn gegenereerd in transactie DBACockpit ziet er ongeveer als volgt uit:

> https://`<fullyqualifiedhostname`>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Afhankelijk van of en hoe de Azure-virtuele Machine die als host fungeert voor de SAP-systeem is verbonden via site-naar-site meerdere site- of ExpressRoute (Cross-Premises implementatie), moet u ervoor zorgen dat ICM wordt met behulp van een volledig gekwalificeerde hostnaam die kan worden opgelost op de computer waar u probeert te openen van de DBACockpit uit. Zie SAP-notitie [773830] om te begrijpen hoe ICM bepaalt de volledig gekwalificeerde hostnaam, afhankelijk van het profiel parameters en set parameter icm/host_name_full expliciet indien nodig.

Als u de virtuele machine in een scenario alleen in de Cloud zonder cross-premises-connectiviteit tussen on-premises en Azure hebt geïmplementeerd, moet u een openbaar IP-adres en een domainlabel definiëren. De indeling van de openbare DNS-naam van de virtuele machine er zo uit:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

Meer informatie met betrekking tot de DNS-naam vindt [hier][virtual-machines-azurerm-versus-azuresm].

Als u de SAP-profiel parameter icm/host_name_full op de DNS-naam van de koppeling van de Azure-VM kan er ongeveer als volgt:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

In dit geval moet u ervoor zorgen dat:

* Regels voor binnenkomende verbindingen toevoegen aan de Netwerkbeveiligingsgroep in de Azure-portal voor de TCP/IP-poorten die worden gebruikt om te communiceren met ICM
* Regels voor binnenkomende verbindingen toevoegen aan de Windows Firewall-configuratie voor de TCP/IP-poorten die worden gebruikt om te communiceren met de ICM

Voor een geautomatiseerde beschikbaar voor alle correcties geïmporteerd, wordt aanbevolen om toe te passen periodiek de correctie verzameling SAP-notitie die van toepassing zijn op uw SAP-versie:

* [1558958]
* [1619967]
* [1882376]

Meer informatie over de DBA Cockpit voor SAP ASE vindt u in de volgende SAP-opmerkingen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Overwegingen voor back-up/herstel voor SAP ASE
Wanneer u SAP ASE implementeert in Azure, moet worden gecontroleerd op uw back-upmethode. Zelfs als het systeem niet een productieve systeem is, moet de SAP-database die wordt gehost door SAP ASE worden back-ups regelmatig. Omdat Azure Storage kan drie afbeeldingen voldoen, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een storage compenserende. De belangrijkste reden voor het onderhouden van een juiste back-up en herstel plan nog meer dingen die u logische/handmatige fouten compenseren kunt door punt in tijd herstel mogelijkheden te bieden. Zodat het doel om een van beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of is het gebruik van de back-ups in Azure als seeding voor een ander systeem door het kopiëren van de bestaande database. Bijvoorbeeld, u kan de overdracht van een Tier-2-SAP-configuratie naar een 3-Laagse system-installatie van hetzelfde systeem door terug te zetten van een back-up.

Back-up en herstellen van een database in Azure werkt op dezelfde manier als on-premises. Zie Opmerkingen bij de SAP:

* [1588316]
* [1585981]

voor informatie over het maken van het dump configuraties en planning back-ups. Afhankelijk van uw strategie en behoeften u configureren dat kunt geheugendumps database en logboekbestanden schijf naar een van de bestaande schijven of toevoegen van een extra schijf voor de back-up. Het risico van gegevensverlies in geval van een fout, verminderen het verdient aanbeveling een schijf gebruiken waar geen voor het databaseapparaat zich bevindt.

Naast gegevens- en LOB-biedt compressie SAP ASE ook back-compressie. Als u wilt minder ruimte met de database en logboekbestanden dumpbestanden voor foutopsporing in beslag nemen het verdient aanbeveling gebruik van back-compressie. Zie voor meer informatie, SAP-notitie [1588316]. Comprimeren van de back-up is ook essentieel om te kunnen verminderen de hoeveelheid gegevens die moet worden overgezet als u van plan bent om back-ups of VHD's met back-up dumpbestanden voor foutopsporing van de virtuele Machine van Azure naar on-premises te downloaden.

Gebruik geen station D:\ als bestemming voor database- of logboekpad dump.

#### <a name="performance-considerations-for-backupsrestores"></a>Prestatie-overwegingen voor back-ups/herstellen
Prestaties van back-up/herstel is als in de bare-metal implementaties, afhankelijk van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes mogelijk. Bovendien speelt de CPU-verbruik die worden gebruikt door de back-upcompressie een belangrijke rol op virtuele machines met maximaal 8 CPU-threads. Daarom kan een aannemen:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de database-apparaten, de kleinere totale doorvoer bij het lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de gevolgen van het back-compressie
* De minder doelen (Stripe mappen, schijven) schrijven van de back-up naar de laagste de doorvoer

Vergroten van het aantal doelen te schrijven naar er zijn twee opties die gebruikt/gecombineerd afhankelijk van uw behoeften worden kunnen:

* Het volume back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOP's op het desbetreffende striped volume
* Het maken van een dump-configuratie op het niveau van SAP ASE, die meer dan één doeldirectory gebruikt voor de dump in te schrijven

Een volume striping over meerdere gekoppelde schijven heeft eerder in deze handleiding zijn besproken. Voor meer informatie over het gebruik van meerdere mappen in de configuratie van de dump SAP ASE, Raadpleeg de documentatie van de opgeslagen Procedure sp_config_dump, die wordt gebruikt voor het maken van de configuratie van de dump op de [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Herstel na noodgeval met Azure-VM 's
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replicatie van gegevens met SAP Sybase replicatie-Server
Met de SAP Sybase Replication Server (SRS) SAP ASE biedt een warme stand-by-oplossing voor databasetransacties asynchroon overdragen naar een externe locatie. 

De installatie en het gebruik van SRS werkt ook functioneel in een virtuele machine die wordt gehost in Azure-Services voor virtuele Machine als er een on-premises.

SAP ASE HADR is niet vereist voor een interne Azure Load Balancer en heeft geen afhankelijkheden op OS niveau clustering en werkt op Windows Azure en Linux-VM's. Voor meer informatie over SAP ASE HADR leest de [SAP ASE HADR gebruikershandleiding](https://help.sap.com/viewer/efe56ad3cad0467d837c8ff1ac6ba75c/16.0.3.3/en-US/a6645e28bc2b1014b54b8815a64b87ba.html).

## <a name="specifics-to-sap-ase-on-linux"></a>Specifieke informatie naar SAP ASE op Linux
Beginnen met Microsoft Azure, kunt u eenvoudig uw bestaande SAP ASE toepassingen migreren naar Azure Virtual Machines. SAP ASE in een virtuele Machine kunt u de totale eigendomskosten van implementatie, beheer en onderhoud van de breedte van bedrijfstoepassingen verminderen door eenvoudig deze toepassingen met Microsoft Azure te migreren. Met SAP ASE in een Azure Virtual machines, beheerders en ontwikkelaars kunnen nog steeds gebruiken de dezelfde ontwikkelings- en beheerhulpprogramma's die on-premises beschikbaar zijn.

Voor het implementeren van virtuele Azure-machines is het belangrijk te weten de officiële Sla's, kunnen u hier vinden: <https://azure.microsoft.com/support/legal/sla>

SAP sizinginformatie en een lijst met SAP gecertificeerde VM-SKU's vindt u in de SAP-notitie [1928533]. Aanvullende SAP formaat documenten voor Azure Virtual machines u hier vindt <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> en hier <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Instructies en aanbevelingen voor het gebruik van Azure Storage, de implementatie van SAP VM's of de SAP-bewaking van toepassing op implementaties van SAP ASE in combinatie met SAP-toepassingen zoals vermeld in de eerste vier hoofdstukken van dit document.

De volgende twee SAP-opmerkingen bevatten algemene informatie over het as-omgeving op Linux- en as-omgeving in de Cloud:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Ondersteuning voor SAP-versie van de as-omgeving
SAP op dit moment ondersteunt SAP ASE versie 16,0 voor gebruik met SAP Business Suite producten. Alle updates voor SAP ASE-server of JDBC en ODBC-stuurprogramma's voor gebruik met SAP Business Suite producten vindt u uitsluitend via SAP Service Marketplace op: <https://support.sap.com/swdc>.

Als voor installaties downloaden on-premises updates voor de SAP ASE-server, of de JDBC en ODBC-stuurprogramma's rechtstreeks vanuit Sybase websites niet. Voor gedetailleerde informatie over patches, die worden ondersteund voor gebruik met SAP Business Suite producten on-premises en in Azure Virtual Machines, Zie de volgende SAP-opmerkingen:

* [1590719]
* [1973241]

Algemene informatie over het uitvoeren van SAP Business Suite op SAP ASE vindt u de [SCN](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Richtlijnen voor SAP ASE voor SAP-gerelateerde SAP ASE installaties in Azure VM 's
#### <a name="structure-of-the-sap-ase-deployment"></a>Structuur van de SAP ASE-implementatie
In overeenstemming met de algemene beschrijving moeten SAP ASE uitvoerbare bestanden worden zich bevindt of in het bestandssysteem van de hoofdmap van de virtuele machine (/sybase) is geïnstalleerd. Normaal gesproken worden de meeste van de SAP ASE-systeem en hulpprogramma's voor databases niet gebruikt hard door SAP NetWeaver-werkbelasting. Daarom kunnen de systeem- en hulpprogramma's voor databases (master, model, saptools, sybmgmtdb, sybsystemdb) blijven op het bestandssysteem hoofdmap. 

Een uitzondering wordt mogelijk de tijdelijke database die alle werktabellen en tijdelijke tabellen die zijn gemaakt door SAP ASE, die in het geval van sommige SAP ERP- en alle BW werkbelastingen vereisen kan hogere gegevensvolume of i/o-bewerkingen, volume die niet in de oorspronkelijke VM-besturingssysteem passen schijf.

Afhankelijk van de versie van SAPInst/SWPM gebruikt voor het installeren van het systeem, kan de database bevatten:

* Een enkele SAP ASE tempdb, die wordt gemaakt bij het installeren van SAP ASE
* Een SAP ASE tempdb die zijn gemaakt door SAP ASE en een extra saptempdb die zijn gemaakt door de SAP-installatieprogramma installeren
* Een SAP ASE tempdb die zijn gemaakt door het installeren van SAP ASE en een aanvullende tempdb die handmatig is gemaakt (bijvoorbeeld volgende SAP-notitie [1752266]) om te voldoen aan specifieke tempdb ERP/BW

In het geval van specifieke ERP of alle BW werkbelastingen, is het zinvol, met betrekking tot prestaties, zodat de tempdb-apparaten van de Daarnaast gemaakte tempdb (door SWPM of handmatig) op een afzonderlijk bestand-systeem, die kan worden gerepresenteerd door een enkele Azure-gegevensschijf of een Linux-RAID-spannin g meerdere Azure-gegevensschijven. Als er geen aanvullende tempdb bestaat, is het raadzaam een te maken (SAP-notitie [1752266]).

Voor dergelijke systemen moeten de volgende stappen worden uitgevoerd voor de tempdb Daarnaast gemaakt:

* De eerste tempdb-map verplaatsen naar het eerste bestandssysteem van de SAP-database
* Tempdb-mappen toevoegen aan elk van de schijven met een bestandssysteem van de SAP-database

Deze configuratie kunt tempdb kunt gebruiken meer ruimte dan het systeemstation is kunnen bieden. Als uitgangspunt kunt een controleren of de tempdb-map-grootten op bestaande systemen, die on-premises uitvoert. Of een dergelijke configuratie zou kunnen IOPS-nummers op basis van tempdb die kan niet worden voorzien van het systeemstation. Systemen die on-premises worden uitgevoerd kunnen opnieuw worden gebruikt voor het bewaken van i/o-werkbelasting op basis van tempdb.

Plaats alle mappen SAP ASE nooit naar mnt of /mnt/resource van de virtuele machine. Dit geldt ook voor de tempdb, zelfs als de objecten die zijn opgeslagen in de tempdb alleen tijdelijk zijn omdat mnt of /mnt/resource is een standaard Azure-VM tijdelijke ruimte, dit geen permanente is. Meer informatie over de tijdelijke ruimte van de virtuele machine van Azure kunnen u vinden in [in dit artikel][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Gevolgen van het Database-compressie
In configuraties waar i/o-bandbreedte een beperkende factor kan worden, kan elke meting, waardoor IOP's helpen om te vergroten van de werkbelasting een kan worden uitgevoerd in een IaaS-scenario, zoals Azure. Daarom is het raadzaam om ervoor te zorgen dat SAP ASE-compressie wordt gebruikt voor het uploaden van een bestaande SAP-database naar Azure.

De aanbeveling om uit te voeren van compressie voordat u uploadt naar Azure als deze is niet geïmplementeerd wordt verkregen uit verschillende redenen:

* De hoeveelheid gegevens worden geüpload naar Azure is lager
* De duur van de compressie-uitvoering is korter, ervan uitgaande dat een sterkere hardware met meer CPU's of hogere i/o-bandbreedte of minder i/o latentie on-premises gebruiken kunt
* Kleinere database kunnen leiden tot minder kosten voor schijftoewijzing

Compressie van gegevens en LOB werken in een virtuele machine die wordt gehost in Azure Virtual Machines, als er een on-premises. Voor meer informatie over hoe u om te controleren als compressie is al in gebruik in een bestaande SAP ASE-database, controleert u SAP-notitie [1750510]. Zie voor meer informatie met betrekking tot de database-compressie, SAP-notitie [2121797].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>DBACockpit gebruikt voor het bewaken van Database-exemplaren
Voor SAP-systemen, die SAP ASE zijn als databaseplatform worden gebruikt, is de DBACockpit toegankelijk als ingesloten browservensters in transactie DBACockpit of Webdynpro. De volledige functionaliteit voor bewaking en beheer van de database is echter beschikbaar in de implementatie Webdynpro van alleen de DBACockpit.

Als met on-premises systemen zijn verschillende stappen vereist om alle SAP NetWeaver-functionaliteit die worden gebruikt door de Webdynpro uitvoering van de DBACockpit te schakelen. Ga als volgt de SAP-notitie [1245200] voor het gebruik van webdynpros inschakelen en het genereren van de vereiste velden. Wanneer u de instructies in de notities van de bovenstaande configureren u ook de Internet-Communicatiemanager (icm), samen met de poorten die worden gebruikt voor http en https-verbindingen. De standaardinstelling voor HTTP-ziet er zo uit:

> ICM/server_port_0 = b = HTTP, poort 8000, PROCTIMEOUT = = 600, time-out = 600
> 
> ICM/server_port_1 = b = HTTPS, poort 443$ $, PROCTIMEOUT = = 600, time-out = 600
> 
> 

en de koppelingen die zijn gegenereerd in transactie DBACockpit zal er ongeveer als volgt:

> https://`<fullyqualifiedhostname`>:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Afhankelijk van of en hoe de Azure-virtuele Machine die als host fungeert voor de SAP-systeem is verbonden via site-naar-site meerdere site- of ExpressRoute (Cross-Premises implementatie), moet u ervoor zorgen dat ICM wordt met behulp van een volledig gekwalificeerde hostnaam die kan worden opgelost op de computer waar u probeert te openen van de DBACockpit uit. Zie SAP-notitie [773830] om te begrijpen hoe ICM bepaalt de volledig gekwalificeerde hostnaam, afhankelijk van het profiel parameters en set parameter icm/host_name_full expliciet indien nodig.

Als u de virtuele machine in een scenario alleen in de Cloud zonder cross-premises-connectiviteit tussen on-premises en Azure hebt geïmplementeerd, moet u een openbaar IP-adres en een domainlabel definiëren. De indeling van de openbare DNS-naam van de virtuele machine er zo uit:

> `<custom domainlabel`>.`<azure region`>.cloudapp.azure.com
> 
> 

Meer informatie met betrekking tot de DNS-naam vindt [hier][virtual-machines-azurerm-versus-azuresm].

Als u de SAP-profiel parameter icm/host_name_full op de DNS-naam van de koppeling van de Azure-VM kan er ongeveer als volgt:

> https://mydomainlabel.westeurope.cloudapp.net:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

In dit geval moet u ervoor zorgen dat:

* Regels voor binnenkomende verbindingen toevoegen aan de Netwerkbeveiligingsgroep in de Azure-portal voor de TCP/IP-poorten die worden gebruikt om te communiceren met ICM
* Regels voor binnenkomende verbindingen toevoegen aan de Windows Firewall-configuratie voor de TCP/IP-poorten die worden gebruikt om te communiceren met de ICM

Voor een geautomatiseerde beschikbaar voor alle correcties geïmporteerd, wordt aanbevolen om toe te passen periodiek de correctie verzameling SAP-notitie die van toepassing zijn op uw SAP-versie:

* [1558958]
* [1619967]
* [1882376]

Meer informatie over de DBA Cockpit voor SAP ASE vindt u in de volgende SAP-opmerkingen:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Overwegingen voor back-up/herstel voor SAP ASE
Wanneer u SAP ASE implementeert in Azure moet worden gecontroleerd op uw back-upmethode. Zelfs als het systeem niet een productieve systeem is, moet de SAP-database die wordt gehost door SAP ASE worden back-ups regelmatig. Omdat Azure Storage kan drie afbeeldingen voldoen, is nu een back-up minder belangrijke met betrekking tot het vastlopen van een storage compenserende. De belangrijkste reden voor het onderhouden van een juiste back-up en herstel plan nog meer dingen die u logische/handmatige fouten compenseren kunt door punt in tijd herstel mogelijkheden te bieden. Zodat het doel om een van beide gebruik back-ups van de database herstellen naar een bepaald punt in tijd of is het gebruik van de back-ups in Azure als seeding voor een ander systeem door het kopiëren van de bestaande database. Bijvoorbeeld, u kan de overdracht van een Tier-2-SAP-configuratie naar een 3-Laagse system-installatie van hetzelfde systeem door terug te zetten van een back-up.

Back-up en herstellen van een database in Azure werkt op dezelfde manier als on-premises. Zie Opmerkingen bij de SAP:

* [1588316]
* [1585981]

voor informatie over het maken van het dump configuraties en planning back-ups. Afhankelijk van uw strategie en behoeften u configureren dat kunt geheugendumps database en logboekbestanden schijf naar een van de bestaande schijven of toevoegen van een extra schijf voor de back-up. Het risico van gegevensverlies in geval van een fout, verminderen het verdient aanbeveling een schijf gebruiken waarop geen database map/bestand zich bevindt.

Naast gegevens- en LOB-biedt compressie SAP ASE ook back-compressie. Als u wilt minder ruimte met de database en logboekbestanden dumpbestanden voor foutopsporing in beslag nemen het verdient aanbeveling gebruik van back-compressie. Zie voor meer informatie, SAP-notitie [1588316]. Comprimeren van de back-up is ook essentieel om te kunnen verminderen de hoeveelheid gegevens die moet worden overgezet als u van plan bent om back-ups of VHD's met back-up dumpbestanden voor foutopsporing van de virtuele Machine van Azure naar on-premises te downloaden.

Gebruik de Azure-VM tijdelijke ruimte mnt of /mnt/resource niet als doel voor database- of logboekpad dump.

#### <a name="performance-considerations-for-backupsrestores"></a>Prestatie-overwegingen voor back-ups/herstellen
Prestaties van back-up/herstel is als in de bare-metal implementaties, afhankelijk van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes mogelijk. Bovendien speelt de CPU-verbruik die worden gebruikt door de back-upcompressie een belangrijke rol op virtuele machines met maximaal 8 CPU-threads. Daarom kan een aannemen:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de database-apparaten, de kleinere totale doorvoer bij het lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de gevolgen van het back-compressie
* De minder doelen (Linux software-RAID, schijven) schrijven van de back-up naar de laagste de doorvoer

Vergroten van het aantal doelen te schrijven naar er zijn twee opties die gebruikt/gecombineerd afhankelijk van uw behoeften worden kunnen:

* Het volume back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOP's op het desbetreffende striped volume
* Het maken van een dump-configuratie op het niveau van SAP ASE, die meer dan één doeldirectory gebruikt voor de dump in te schrijven

Een volume striping over meerdere gekoppelde schijven heeft eerder in deze handleiding zijn besproken. Voor meer informatie over het gebruik van meerdere mappen in de configuratie van de dump SAP ASE, Raadpleeg de documentatie van de opgeslagen Procedure sp_config_dump, die wordt gebruikt voor het maken van de configuratie van de dump op de [Sybase Infocenter](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Herstel na noodgeval met Azure-VM 's
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replicatie van gegevens met SAP Sybase replicatie-Server
Met de SAP Sybase Replication Server (SRS) SAP ASE biedt een warme stand-by-oplossing voor databasetransacties asynchroon overdragen naar een externe locatie. 

De installatie en het gebruik van SRS werkt ook functioneel in een virtuele machine die wordt gehost in Azure-Services voor virtuele Machine als er een on-premises.

As-omgeving HADR via SAP replicatie-Server wordt op dit moment niet ondersteund. Het kan worden getest met en die zijn uitgebracht voor Microsoft Azure-platform in de toekomst.

## <a name="specifics-to-oracle-database-on-windows"></a>Specifieke met Oracle-Database op Windows-instellingen
Oracle-software wordt ondersteund door Oracle worden uitgevoerd op Microsoft Windows Hyper-V en Azure. Raadpleeg voor meer informatie over de algemene ondersteuning van Windows Hyper-V en Azure: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Na de algemene ondersteuning is ook het specifieke scenario aan SAP-toepassingen gebruik te maken van Oracle-Databases ondersteund. Details zijn met de naam in dit gedeelte van het document.

### <a name="oracle-version-support"></a>Ondersteuning voor Oracle-versie
Oracle-versies en de bijbehorende versies van het besturingssysteem, die worden ondersteund voor het uitvoeren van SAP op Oracle op Azure Virtual Machines gevonden in de SAP-notitie [2039619].

Algemene informatie over het uitvoeren van SAP Business Suite op Oracle kan worden gevonden in 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>De richtlijnen voor Oracle voor SAP-installaties in Azure VM 's
#### <a name="storage-configuration"></a>Opslagconfiguratie
Slechts één exemplaar met behulp van schijven met NTFS geformatteerd Oracle wordt ondersteund. Alle databasebestanden moeten worden opgeslagen op het NTFS-bestandssysteem op basis van VHD's of Managed Disks. Deze schijven zijn gekoppeld aan de Azure VM en zijn gebaseerd op de pagina-blobopslag van Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alle soorten netwerkstations of externe bestandsshares, zoals Azure file-services:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

zijn **niet** ondersteund voor Oracle database-bestanden.

Schijven op basis van Azure Storage voor Page-BLOB of Managed Disks, de instructies die zijn aangebracht in dit document in hoofdstuk [in cache opslaan voor virtuele machines en de gegevensschijven] [ dbms-guide-2.1] en [Microsoft Azure Storage] [ dbms-guide-2.3] zijn van toepassing op implementaties met de Oracle-Database.

Zoals eerder in het algemene gedeelte van het document is uitgelegd, bestaan de quota voor de doorvoer van de IOPS voor Azure-schijven. De exacte quota zijn afhankelijk van het type virtuele machine die wordt gebruikt. Een lijst met typen VM's met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Voor het identificeren van de ondersteunde typen Azure VM's, verwijzen naar SAP-notitie [1928533].

Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor het opslaan van alle DB-bestanden op één enkele gekoppelde schijf. 

Als meer IOPS vereist zijn, is het aanbevolen gebruik venster opslaggroepen (alleen beschikbaar in Windows Server 2012 en hoger) of Windows 2008 R2 voor Windows wordt striping om te maken van een grote logisch apparaat over meerdere gekoppelde schijven (Zie ook hoofdstuk [ Software-RAID] [ dbms-guide-2.2] van dit document). Deze aanpak vereenvoudigt het beheer-overhead voor het beheren van de schijfruimte en vermijdt u de inspanningen om de bestanden handmatig te verdelen over meerdere gekoppelde schijven.

#### <a name="backup--restore"></a>Back-up en herstellen
Voor back-up / herstellen van de functionaliteit van de SAP-BR *-hulpprogramma's voor Oracle op dezelfde manier als op de standaard Windows Server-besturingssystemen en Hyper-V worden ondersteund. Oracle Recovery Manager (RMAN) wordt ook ondersteund voor back-ups op schijf en herstellen van de schijf.

#### <a name="high-availability"></a>Hoge beschikbaarheid
Oracle Data Guard wordt ondersteund voor hoge beschikbaarheid en noodherstel. Meer informatie vindt u [dit] [ virtual-machines-windows-classic-configure-oracle-data-guard] documentatie.

#### <a name="other"></a>Overige
Andere algemene onderdelen zoals Beschikbaarheidssets van Azure of SAP bewaking toepassen zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de Oracle-Database.

## <a name="specifics-to-oracle-database-on-oracle-linux"></a>Specifieke informatie met Oracle-Database voor Oracle Linux
Oracle-software wordt ondersteund door Oracle worden uitgevoerd op Microsoft Windows Hyper-V en Azure. Raadpleeg voor meer informatie over de algemene ondersteuning van Windows Hyper-V en Azure: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Na de algemene ondersteuning is ook het specifieke scenario aan SAP-toepassingen gebruik te maken van Oracle-Databases ondersteund. Details zijn met de naam in dit gedeelte van het document.

### <a name="oracle-version-support"></a>Ondersteuning voor Oracle-versie
Oracle-versies en de bijbehorende versies van het besturingssysteem, die worden ondersteund voor het uitvoeren van SAP op Oracle op Azure Virtual Machines gevonden in de SAP-notitie [2039619].

Algemene informatie over het uitvoeren van SAP Business Suite op Oracle kan worden gevonden in 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>De richtlijnen voor Oracle voor SAP-installaties in Azure VM 's
#### <a name="storage-configuration"></a>Opslagconfiguratie
Slechts wordt één exemplaar Oracle met behulp van ext3, ext4 en xfs opgemaakt schijven ondersteund. Alle databasebestanden moeten worden opgeslagen op deze bestandssystemen op basis van VHD's of Managed Disks. Deze schijven zijn gekoppeld aan de Azure VM en zijn gebaseerd op de pagina-blobopslag van Azure (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alle soorten netwerkstations of externe bestandsshares, zoals Azure file-services:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

zijn **niet** ondersteund voor Oracle database-bestanden.

Schijven op basis van Azure Storage voor Page-BLOB of Managed Disks, de instructies die zijn aangebracht in dit document in hoofdstuk [in cache opslaan voor virtuele machines en de gegevensschijven] [ dbms-guide-2.1] en [Microsoft Azure Storage] [ dbms-guide-2.3] zijn van toepassing op implementaties met de Oracle-Database.

Zoals eerder in het algemene gedeelte van het document is uitgelegd, bestaan de quota voor de doorvoer van de IOPS voor Azure-schijven. De exacte quota zijn afhankelijk van het type virtuele machine die wordt gebruikt. Een lijst met typen VM's met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Voor het identificeren van de ondersteunde typen Azure VM's, verwijzen naar SAP-notitie [1928533]

Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor het opslaan van alle DB-bestanden op één enkele gekoppelde schijf. 

Als meer IOPS vereist zijn, is het raadzaam LVM (Logical Volume Manager) of MDADM gebruiken om te maken van een logische groot via meerdere gekoppelde schijven. Zie ook hoofdstuk [Software-RAID] [ dbms-guide-2.2] van dit document. Deze aanpak vereenvoudigt het beheer-overhead voor het beheren van de schijfruimte en vermijdt u de inspanningen om de bestanden handmatig te verdelen over meerdere gekoppelde schijven.

#### <a name="backup--restore"></a>Back-up en herstellen
Voor back-up / herstellen van de functionaliteit van de SAP-BR *-hulpprogramma's voor Oracle op dezelfde manier als op bare-metalcomputers en Hyper-V worden ondersteund. Oracle Recovery Manager (RMAN) wordt ook ondersteund voor back-ups op schijf en herstellen van de schijf.

#### <a name="high-availability"></a>Hoge beschikbaarheid
Oracle Data Guard wordt ondersteund voor hoge beschikbaarheid en noodherstel. Meer informatie vindt u [dit] [ virtual-machines-windows-classic-configure-oracle-data-guard] documentatie.

#### <a name="other"></a>Overige
Andere algemene onderdelen zoals Beschikbaarheidssets van Azure of SAP bewaking toepassen zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de Oracle-Database.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Gegevens voor de Database van de MaxDB SAP op Windows
### <a name="sap-maxdb-version-support"></a>Ondersteuning voor SAP-MaxDB versie
SAP op dit moment ondersteunt SAP MaxDB versie 7,9 voor gebruik met SAP NetWeaver gebaseerde producten in Azure. Alle updates voor SAP MaxDB-server of JDBC en ODBC-stuurprogramma's voor gebruik met SAP NetWeaver gebaseerde producten vindt u uitsluitend via SAP Service Marketplace op <https://support.sap.com/swdc>.
Algemene informatie over het uitvoeren van SAP NetWeaver op SAP MaxDB kan worden gevonden op <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Ondersteunde versies van Microsoft Windows en virtuele Azure-machine-typen voor SAP MaxDB DBMS
Als u de ondersteunde versie van Microsoft Windows voor SAP MaxDB DBMS in Azure, bezoekt u het:

* [SAP-Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van het besturingssysteem Microsoft Windows is Microsoft Windows 2012 R2 gebruiken.

### <a name="available-sap-maxdb-documentation"></a>Beschikbare SAP MaxDB-documentatie
U kunt de bijgewerkte lijst van SAP MaxDB documentatie vinden in de volgende SAP-notitie [767598]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Richtlijnen voor SAP MaxDB voor SAP-installaties in Azure VM 's
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Opslagconfiguratie
Aanbevolen procedures van de Azure-opslag voor SAP MaxDB gaat u als volgt de algemene aanbevelingen die worden vermeld in het hoofdstuk [structuur van de implementatie van een RDBMS][dbms-guide-2].

> [!IMPORTANT]
> Net als andere databases heeft SAP MaxDB ook gegevens en logboekbestanden. De juiste term is echter in de SAP-MaxDB terminologie "volume" (niet ' file'). Er zijn bijvoorbeeld SAP MaxDB hoeveelheden gegevens en logboekvolumes. Verwar deze OS schijfvolumes niet. 
> 
> 

Kort gezegd hebt tot:

* Als u Azure Storage-accounts gebruiken, stelt u de Azure-opslagaccount waarin de SAP-MaxDB gegevens en logboekvolumes (dat wil zeggen-bestanden) op **lokaal redundante opslag (LRS)** zoals opgegeven in het hoofdstuk [Microsoft Azure Storage][dbms-guide-2.3].
* De i/o-pad voor SAP MaxDB gegevensvolumes (dat wil zeggen-bestanden) te scheiden van de i/o-pad voor logboekvolumes (dat wil zeggen-bestanden). Dit betekent dat SAP MaxDB gegevensvolumes (dat wil zeggen-bestanden) moeten worden geïnstalleerd op één logisch station en SAP MaxDB logboekvolumes (dat wil zeggen-bestanden) moeten worden geïnstalleerd op een andere logische schijf.
* Stel de juiste cachingtype voor elke schijf, afhankelijk van of u deze gebruiken voor SAP MaxDB gegevensbestand of logboekbestand volumes (dat wil zeggen-bestanden) en of u Azure Standard- of Azure Premium Storage gebruiken zoals wordt beschreven in het hoofdstuk [in cache opslaan voor virtuele machines en de gegevensschijven] [dbms-guide-2.1].
* Als het huidige quotum IOP's per schijf aan de vereisten voldoet, is het mogelijk voor het opslaan van de gegevensvolumes op één gekoppelde schijf en ook alle logboekvolumes voor database opslaan op een andere één gekoppelde schijf.
* Als u meer IOPS en/of ruimte vereist zijn, is het raadzaam Microsoft Windows-striping voor Microsoft Windows 2008 R2 of Microsoft venster opslaggroepen (alleen beschikbaar in Microsoft Windows Server 2012 en hoger) gebruiken om te maken van een grote logisch apparaat via meerdere gekoppelde schijven. Zie ook hoofdstuk [Software-RAID] [ dbms-guide-2.2] van dit document. Deze aanpak vereenvoudigt het beheer-overhead voor het beheren van de schijfruimte en vermijdt u de moeite van het bestanden handmatig te verdelen over meerdere gekoppelde schijven.
* Voor de hoogste IOPS-vereisten, kunt u Azure Premium Storage gebruiken die beschikbaar zijn op uit de DS-serie en GS-serie VM's is.

![Naslaginformatie over configuratie van Azure IaaS-VM voor SAP MaxDB DBMS][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Back-up en herstel
Wanneer u SAP MaxDB implementeert in Azure, moet u uw back-upmethode bekijken. Zelfs als het systeem niet een productieve systeem is, moet de SAP-database die wordt gehost door SAP MaxDB worden back-ups regelmatig. Omdat Azure Storage kan drie afbeeldingen voldoen, is nu een back-up minder belangrijke wat betreft het beveiligen van uw systeem op basis van de fout bij de opslag en de belangrijkste administratieve of operationele fouten. De belangrijkste reden voor het onderhouden van een goede back-up en herstellen-plan is zodat u logische of handmatige fouten compenseren kunt door recovery point-in-time-mogelijkheden te bieden. Zodat het doel is om de back-ups gebruiken voor de database herstellen naar een bepaald punt in tijd of het gebruik van de back-ups in Azure als seeding voor een ander systeem door het kopiëren van de bestaande database. Bijvoorbeeld, u kan de overdracht van een tier-2-SAP-configuratie naar een 3-laagse system-installatie van hetzelfde systeem door terug te zetten van een back-up.

Back-up en herstellen van een database in Azure werkt op dezelfde manier als voor on-premises systemen, zodat u kunt standaardprogramma SAP MaxDB back-up/herstel's, die worden beschreven in een van de SAP-MaxDB documentatie over documenten die worden vermeld in de SAP-notitie [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Prestatie-overwegingen voor back-up en herstel
Prestaties van back-up en herstel is als in de bare-metal implementaties, afhankelijk van hoeveel volumes kunnen worden gelezen in parallelle instructies en de doorvoer van deze volumes. De CPU-verbruik die worden gebruikt door de back-compressie kunt een belangrijke rol bovendien afspelen op virtuele machines met maximaal 8 CPU-threads. Daarom kan een aannemen:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de database-apparaten, des te lager de algemene lezen doorvoer
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de gevolgen van het back-compressie
* De back-up om naar te schrijven, des te lager de doorvoer het minder doelen (Stripe mappen, schijven)

Als u wilt het aantal doelen te schrijven naar verhogen, zijn er twee opties die u, mogelijk in combinatie, afhankelijk van uw behoeften gebruiken kunt:

* Afzonderlijke volumes uitsluitend voor back-up
* Het volume back-updoel striping over meerdere gekoppelde schijven om te verbeteren van de doorvoer IOP's op het desbetreffende schijfvolume striped
* Apparaten van afzonderlijke toegewezen logische schijf voor problemen:
  * SAP-MaxDB back-upvolumes (dat wil zeggen-bestanden)
  * SAP-gegevensvolumes MaxDB (dat wil zeggen-bestanden)
  * SAP-MaxDB logboekvolumes (dat wil zeggen-bestanden)

Een volume striping over meerdere gekoppelde schijven is beschreven eerder in het hoofdstuk [Software-RAID] [ dbms-guide-2.2] van dit document. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Andere
Alle andere algemene gebieden, zoals Azure-Beschikbaarheidssets of SAP controle zijn ook van toepassing zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met de SAP-MaxDB-database.
Andere SAP MaxDB-specifieke instellingen zijn transparant voor de Azure-VM's en worden beschreven in verschillende documenten die worden vermeld in de SAP-notitie [767598] en deze SAP-opmerkingen:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Gegevens voor SAP liveCache op Windows
### <a name="sap-livecache-version-support"></a>SAP liveCache versie-ondersteuning
Minimale versie van SAP-liveCache ondersteund in Azure Virtual Machines is **SAP LC/LCAPPS 10.0 SP 25** inclusief **liveCache 7.9.08.31** en **LCA-Build 25**, vrijgegeven voor **EhP 2 voor SAP SCM 7.0** en hoger.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Microsoft Windows-versies en Azure-VM-typen voor liveCache SAP DBMS-systemen ondersteund
Als u de ondersteunde versie van Microsoft Windows voor liveCache SAP op Azure, bezoekt u het:

* [SAP-Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van het besturingssysteem Microsoft Windows Server gebruiken. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache de richtlijnen voor SAP-installaties in Azure VM 's
#### <a name="recommended-azure-vm-types"></a>Aanbevolen Azure VM-typen
Heeft een grote invloed hebben op de prestaties van SAP-liveCache zoals SAP liveCache is een toepassing die enorme berekeningen uitvoert, de hoeveelheid en de snelheid van het RAM-geheugen en CPU. 

Voor de virtuele machine van Azure die worden ondersteund door SAP (SAP-notitie [1928533]), alle virtuele CPU-resources toegewezen aan de virtuele machine worden ondersteund door specifieke fysieke CPU-resources van de hypervisor. Er is geen overmatige inrichting (en dus geen concurrentie voor CPU-resources) plaatsvindt.

Op dezelfde manier voor alle Azure-VM-exemplaar die worden ondersteund door SAP, is het geheugen van de virtuele machine toegewezen aan het fysieke geheugen - (te veel toezegging), bijvoorbeeld piekmomenten 100-% wordt niet gebruikt.

Vanuit dit perspectief, is het raadzaam gebruik van de nieuwe D-serie of DS-serie (in combinatie met Azure Premium Storage) Azure VM-type, als ze beschikken over 60% snellere processoren dan de A-serie. Voor de hoogste RAM-geheugen en CPU-belasting, kunt u uit de G-serie en GS-serie (in combinatie met Azure Premium Storage) virtuele machines met de nieuwste Intel gebruiken? Xeon? processor E5 v3-familie, waarvoor twee keer de hoeveelheid geheugen en vier keer de solid state drive-opslag (SSD's) van de D/DS-serie.

#### <a name="storage-configuration"></a>Opslagconfiguratie
Als SAP liveCache is gebaseerd op SAP MaxDB technologie, de Azure storage aanbevolen aanbevelingen voor SAP MaxDB vermeld in het hoofdstuk [opslagconfiguratie] [ dbms-guide-8.4.1] zijn ook geldig voor SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Speciale Azure-VM voor liveCache
Omdat SAP liveCache intensief gebruikmaakt van verwerkingskracht, voor productief gebruik is het raadzaam om te implementeren op een specifieke Azure-Machine. 

![Virtuele Azure-machine voor liveCache voor productieve use-case toegewezen][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Back-up en herstel
back-up en herstel, met inbegrip van prestatie-overwegingen, worden al beschreven in de desbetreffende SAP MaxDB hoofdstukken [back-up en herstellen] [ dbms-guide-8.4.2] en [prestatie-overwegingen voor back-up maken en terugzetten][dbms-guide-8.4.3]. 

#### <a name="other"></a>Overige
Alle andere algemene gebieden al worden beschreven in de relevante SAP MaxDB [dit] [ dbms-guide-8.4.4] hoofdstuk. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Gegevens voor de SAP-inhoudsserver op Windows
De Server van de SAP-inhoud is een afzonderlijke, server-gebaseerde onderdeel voor het opslaan van inhoud, zoals elektronische documenten in verschillende indelingen. De SAP-Server voor webinhoud wordt geleverd door het gebied van technologie en moet worden gebruikt verschillende toepassingen voor SAP-toepassingen. Deze is geïnstalleerd op een afzonderlijk systeem. Typische inhoud is trainingsmateriaal en documentatie van kennis datawarehouse of technische tekeningen die afkomstig zijn van de mySAP PLM Document Management-systeem. 

### <a name="sap-content-server-version-support"></a>Ondersteuning voor de versie van de SAP inhoudsserver
SAP op dit moment ondersteunt:

* **SAP-Server voor webinhoud** met versie **6.50 (en hoger)**
* **SAP MaxDB versie 7,9**
* **Microsoft IIS (Internet Information Server) versie 8.0 (en hoger)**

Het is raadzaam gebruik van de nieuwste versie van SAP inhoudsserver, die op het moment van schrijven van dit document is **6.50 SP4**, en de nieuwste versie van **Microsoft IIS 8.5**. 

Controleer de nieuwste ondersteunde versies van de inhoudsserver SAP en Microsoft IIS in de [SAP Product beschikbaarheid Matrix (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Microsoft Windows en virtuele Azure-machine-typen ondersteund voor SAP-Server voor webinhoud
Ondersteunde Windows-versie voor de Server voor webinhoud SAP op Azure, Zie:

* [SAP-Product beschikbaarheid Matrix (PAM)][sap-pam]
* SAP-notitie [1928533]

Het is raadzaam de nieuwste versie van Microsoft Windows Server gebruiken.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Richtlijnen voor Content Server SAP voor SAP-installaties in Azure VM 's
#### <a name="storage-configuration"></a>Opslagconfiguratie
Als u inhoud SAP-Server voor het opslaan van bestanden in de database SAP MaxDB configureert, alle Azure storage aanbevolen procedures voor SAP MaxDB vermeld in het hoofdstuk aanbeveling [opslagconfiguratie] [ dbms-guide-8.4.1] zijn ook geldig voor het scenario met SAP-Server voor webinhoud. 

Als u inhoud SAP-Server voor het opslaan van bestanden in het bestandssysteem is geconfigureerd, wordt het aanbevolen een specifieke logische station wilt gebruiken. Windows Storage Spaces, kunt u zodat ook de grootte van de logische schijf en doorvoer, IOP's, zoals beschreven in het hoofdstuk [Software-RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>Locatie van de SAP-Server voor webinhoud
SAP-inhoudsserver moet worden geïmplementeerd in dezelfde Azure-regio en Azure VNET waar het SAP-systeem wordt geïmplementeerd. U staat op vrije om te bepalen of u wilt implementeren de inhoudsserver SAP-onderdelen op een specifieke virtuele machine van Azure of op dezelfde virtuele machine waarop het SAP-systeem wordt uitgevoerd. 

![Toegewezen virtuele machines van Azure voor SAP-Server voor webinhoud][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP-Server cachelocatie
De cacheserver SAP is een extra onderdeel op de server voor toegang tot documenten lokaal (in het cachegeheugen). De cacheserver SAP slaat de documenten van een SAP-Server voor webinhoud. Dit is het optimaliseren van het netwerkverkeer als documenten hebben meer dan één keer worden opgehaald van de verschillende locaties. De regel is dat de cacheserver SAP worden fysiek dicht bij de client die toegang heeft tot de cacheserver SAP. 

Hier hebt u twee opties:

1. **Client is een SAP-systeem voor back-end** als een back-end SAP-systeem is geconfigureerd voor toegang tot inhoud SAP-Server, die SAP-systeem is een client. Als zowel de SAP-systeem en de SAP-Server voor webinhoud zijn geïmplementeerd in dezelfde Azure-regio, in de dezelfde Azure-datacenter, gebruikers die zich fysiek dicht bij elkaar. Er is daarom niet nodig om een speciale Server voor de SAP-Cache. SAP-UI-clients (SAP-GUI of web browser) rechtstreeks toegang hebben tot de SAP-systeem en de SAP-systeem documenten opgehaald uit de SAP-Server voor webinhoud.
2. **Client is een on-premises-webbrowser** de SAP-inhoudsserver kan worden geconfigureerd voor het rechtstreeks door de webbrowser worden geopend. In dit geval is een webbrowser die on-premises uitgevoerd een client van de SAP-Server voor webinhoud. On-premises datacenter en Azure-datacenter worden geplaatst op verschillende fysieke locaties (in het ideale geval dicht bij elkaar). Uw on-premises datacenter is verbonden met Azure via Azure Site-naar-Site VPN of ExpressRoute. Hoewel beide opties beveiligde VPN-netwerkverbinding naar Azure bieden, biedt site-naar-site-netwerkverbinding een SLA van netwerk bandbreedte en de latentie tussen het on-premises datacenter en de Azure-datacenter. Als u wilt versnellen toegang tot documenten, kunt u het volgende doen:
   1. De cacheserver SAP on-premises installeert, dicht bij de on-premises web browser (op de optie [dit] [ dbms-guide-900-sap-cache-server-on-premises] afbeelding)
   2. Configureer Azure ExpressRoute biedt een hoge en lage latentie toegewezen netwerkverbinding tussen on-premises datacenter en Azure-datacenter.

![Optie voor het installeren van de cacheserver SAP on-premises][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Back-up en herstellen
Als u de SAP-Server voor webinhoud voor het opslaan van bestanden in de database SAP MaxDB configureert, de procedure- en Prestatieoverwegingen voor back-up/herstel al worden beschreven in SAP MaxDB hoofdstuk [back-up en herstellen] [ dbms-guide-8.4.2]en hoofdstuk [prestatie-overwegingen voor back-up en herstel][dbms-guide-8.4.3]. 

Als u de SAP-Server voor webinhoud voor het opslaan van bestanden in het bestandssysteem is geconfigureerd, wordt een optie is voor het uitvoeren van handmatige back-up of herstellen van de structuur van het hele bestand waar de documenten moeten zich bevinden. Net als bij SAP MaxDB back-up/herstel, het is aanbevolen dat een toegewezen schijfvolume voor back-doel. 

#### <a name="other"></a>Overige
Andere inhoud SAP-Server-instellingen zijn transparant voor de Azure-VM's en worden beschreven in verschillende documenten en SAP-opmerkingen:

* <https://service.sap.com/contentserver> 
* SAP-notitie [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Gegevens met IBM DB2 voor LUW op Windows
Met Microsoft Azure, kunt u eenvoudig uw bestaande SAP-toepassing uitvoeren op IBM DB2 voor Linux, UNIX- en Windows (LUW) om virtuele machines van Azure te migreren. Met SAP on IBM DB2 voor LUW, beheerders en ontwikkelaars kunnen nog steeds gebruiken de dezelfde ontwikkelings- en beheerprogramma's, die on-premises beschikbaar zijn.
Algemene informatie over het uitvoeren van SAP Business Suite op IBM DB2 voor LUW gevonden in de SAP Community netwerk (SCN) op <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Zie voor meer informatie en updates voor SAP op DB2 voor LUW op Azure, SAP-notitie [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 voor Linux, UNIX- en ondersteuning voor Windows-versie
SAP op IBM DB2 voor LUW op Microsoft Azure-Services voor virtuele Machine wordt ondersteund vanaf DB2 versie 10,5.

Raadpleeg voor informatie over ondersteunde SAP-producten en typen Azure VM's, SAP-notitie [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 voor Linux, UNIX- en Windows-configuratie-instructies voor SAP-installaties in Azure VM 's
#### <a name="storage-configuration"></a>Opslagconfiguratie
Alle databasebestanden moeten worden opgeslagen op het NTFS-bestandssysteem op basis van rechtstreeks aangesloten schijven. Deze schijven zijn gekoppeld aan de Azure VM en zijn gebaseerd in Azure pagina-BLOB-opslag (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alle soorten netwerkstations of externe shares zijn de volgende Azure-Bestandsservices **niet** ondersteund voor databasebestanden: 

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Als u van schijven op basis van Azure Storage voor Page-BLOB of Managed Disks gebruikmaakt, de instructies in dit document in hoofdstuk aangebracht [structuur van de implementatie van een RDBMS] [ dbms-guide-2] ook van toepassing op implementaties met de IBM DB2 voor LUW De database. 

Zoals eerder in het algemene gedeelte van het document is uitgelegd, bestaan de quota voor IOPS-doorvoer voor schijven. De exacte quota, is afhankelijk van de VM-type dat wordt gebruikt. Een lijst met typen VM's met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Als het huidige quotum IOP's per schijf voldoende is, is het mogelijk voor het opslaan van de databasebestanden op één enkele gekoppelde schijf. 

Voor prestaties verwijzen overwegingen ook naar hoofdstuk 'Veiligheid van gegevens en prestatie-overwegingen voor Database-mappen' in de SAP-installatiehandleidingen.

U kunt ook kunt u opslaggroepen van Windows (alleen beschikbaar in Windows Server 2012 en hoger) of Windows striping voor Windows 2008 R2 als beschreven in het hoofdstuk [Software-RAID] [ dbms-guide-2.2] van dit document om Maak één grote logisch apparaat over meerdere schijven.
Voor de schijven die de DB2-opslagpaden voor uw mappen sapdata en saptmp bevat, moet u een fysieke Schijfsectorgrootte van 512 KB. Wanneer u opslaggroepen van Windows gebruikt, moet u de opslaggroepen handmatig via de opdrachtregelinterface met de parameter `-LogicalSectorSizeDefault`. Zie voor meer informatie, <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Back-up maken/terugzetten
De back-up/herstel-functionaliteit voor IBM DB2 voor LUW wordt ondersteund op dezelfde manier als op de standaard Windows Server-besturingssystemen en Hyper-V.

U moet ervoor zorgen dat er een geldige database back-upstrategie in plaats. 

Zoals in de bare-metal implementaties afhankelijk prestaties van back-up/herstel van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes kan zijn. Bovendien speelt de CPU-verbruik die worden gebruikt door de back-upcompressie een belangrijke rol op virtuele machines met maximaal 8 CPU-threads. Daarom kan een aannemen:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de database-apparaten, de kleinere totale doorvoer bij het lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de gevolgen van het back-compressie
* De back-up om naar te schrijven, des te lager de doorvoer het minder doelen (Stripe mappen, schijven)

Voor het verhogen van het aantal doelen om naar te schrijven, kunnen twee opties zijn afhankelijk van uw behoeften gebruikt/gecombineerd:

* Het volume back-updoel striping over meerdere schijven om te verbeteren van de doorvoer IOP's op het desbetreffende striped volume
* Met behulp van meer dan één doeldirectory het schrijven van de back-up

#### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen
Microsoft Cluster Server (MSCS) wordt niet ondersteund.

Herstel na noodgevallen (HADR) voor hoge beschikbaarheid DB2 wordt ondersteund. Als de virtuele machines van de HA-configuratie naamomzetting werkt hebt, niet de instellingen in Azure afwijken van elke instelling die on-premises wordt uitgevoerd. Het verdient niet afhankelijk zijn van alleen IP-adresomzetting.

Gebruik geen Geo-replicatie voor de opslagaccounts die opslaan van de schijven van de database. Raadpleeg voor meer informatie naar hoofdstuk [Microsoft Azure Storage] [ dbms-guide-2.3] en hoofdstuk [hoge beschikbaarheid en herstel na noodgeval met Azure-VM's] [ dbms-guide-3].

#### <a name="other"></a>Overige
Andere algemene onderdelen zoals Beschikbaarheidssets van Azure of SAP bewaking zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met IBM DB2 voor LUW ook van toepassing. 

Ook verwijzen naar hoofdstuk [algemene SQL Server voor SAP op Azure samenvatting][dbms-guide-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Gegevens met IBM DB2 voor LUW op Linux
Met Microsoft Azure, kunt u eenvoudig uw bestaande SAP-toepassing uitvoeren op IBM DB2 voor Linux, UNIX- en Windows (LUW) om virtuele machines van Azure te migreren. Met SAP on IBM DB2 voor LUW, beheerders en ontwikkelaars kunnen nog steeds gebruiken de dezelfde ontwikkelings- en beheerprogramma's, die on-premises beschikbaar zijn. Algemene informatie over het uitvoeren van SAP Business Suite op IBM DB2 voor LUW gevonden in de SAP Community netwerk (SCN) op <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Zie voor meer informatie en updates voor SAP op DB2 voor LUW op Azure, SAP-notitie [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 voor Linux, UNIX- en ondersteuning voor Windows-versie
SAP op IBM DB2 voor LUW op Microsoft Azure-Services voor virtuele Machine wordt ondersteund vanaf DB2 versie 10,5.

Raadpleeg voor informatie over ondersteunde SAP-producten en typen Azure VM's, SAP-notitie [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 voor Linux, UNIX- en Windows-configuratie-instructies voor SAP-installaties in Azure VM 's
#### <a name="storage-configuration"></a>Opslagconfiguratie
Alle databasebestanden moeten worden opgeslagen op een bestandssysteem op basis van rechtstreeks aangesloten schijven. Deze schijven zijn gekoppeld aan de Azure VM en zijn gebaseerd in Azure pagina-BLOB-opslag (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) of Managed Disks (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Alle soorten netwerkstations of externe shares zijn de volgende Azure-Bestandsservices **niet** ondersteund voor databasebestanden:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx>

Als u schijven op basis van Azure pagina BLOB Storage, de instructies die zijn aangebracht in dit document in hoofdstuk [structuur van de implementatie van een RDBMS] [ dbms-guide-2] ook van toepassing op implementaties met de IBM DB2 voor LUW-Database.

Zoals eerder in het algemene gedeelte van het document is uitgelegd, bestaan de quota voor IOPS-doorvoer voor schijven. De exacte quota, is afhankelijk van de VM-type dat wordt gebruikt. Een lijst met typen VM's met hun quota vindt [hier (Linux)] [ virtual-machines-sizes-linux] en [hier (Windows)][virtual-machines-sizes-windows].

Als het huidige quotum IOP's per schijf voldoende is, is het mogelijk voor het opslaan van de databasebestanden op één enkele schijf.

Voor prestaties verwijzen overwegingen ook naar hoofdstuk 'Veiligheid van gegevens en prestatie-overwegingen voor Database-mappen' in de SAP-installatiehandleidingen.

U kunt ook kunt u LVM (Logical Volume Manager) of MDADM zoals beschreven in het hoofdstuk [Software-RAID] [ dbms-guide-2.2] van dit document om te maken van een grote logisch apparaat over meerdere schijven.
Voor de schijven die de DB2-opslagpaden voor uw mappen sapdata en saptmp bevat, moet u een fysieke Schijfsectorgrootte van 512 KB.

#### <a name="backuprestore"></a>Back-up maken/terugzetten
De back-up/herstel-functionaliteit voor IBM DB2 voor LUW wordt ondersteund op dezelfde manier als in standaard Linux-installatie on-premises.

U moet ervoor zorgen dat er een geldige database back-upstrategie in plaats.

Zoals in de bare-metal implementaties afhankelijk prestaties van back-up/herstel van hoeveel volumes parallel kunnen worden gelezen en wat de doorvoer van deze volumes kan zijn. Bovendien speelt de CPU-verbruik die worden gebruikt door de back-upcompressie een belangrijke rol op virtuele machines met maximaal 8 CPU-threads. Daarom kan een aannemen:

* Minder het nummer van de schijven die worden gebruikt voor het opslaan van de database-apparaten, de kleinere totale doorvoer bij het lezen
* Hoe kleiner dat het aantal CPU-threads in de VM, de strengere de gevolgen van het back-compressie
* De back-up om naar te schrijven, des te lager de doorvoer het minder doelen (Stripe mappen, schijven)

Voor het verhogen van het aantal doelen om naar te schrijven, kunnen twee opties zijn afhankelijk van uw behoeften gebruikt/gecombineerd:

* Het volume back-updoel striping over meerdere schijven om te verbeteren van de doorvoer IOP's op het desbetreffende striped volume
* Met behulp van meer dan één doeldirectory het schrijven van de back-up

#### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen
Herstel na noodgevallen (HADR) voor hoge beschikbaarheid DB2 wordt ondersteund. Als de virtuele machines van de HA-configuratie naamomzetting werkt hebt, niet de instellingen in Azure afwijken van elke instelling die on-premises wordt uitgevoerd. Het verdient niet afhankelijk zijn van alleen IP-adresomzetting.

Gebruik geen Geo-replicatie voor de opslagaccounts die opslaan van de schijven van de database. Raadpleeg voor meer informatie naar hoofdstuk [Microsoft Azure Storage] [ dbms-guide-2.3] en hoofdstuk [hoge beschikbaarheid en herstel na noodgeval met Azure-VM's] [ dbms-guide-3].

#### <a name="other"></a>Overige
Andere algemene onderwerpen, zoals Azure-Beschikbaarheidssets of SAP bewaking zoals beschreven in de eerste drie hoofdstukken van dit document voor implementaties van virtuele machines met IBM DB2 voor LUW ook van toepassing.

Ook verwijzen naar hoofdstuk [algemene SQL Server voor SAP op Azure samenvatting][dbms-guide-5.8].

