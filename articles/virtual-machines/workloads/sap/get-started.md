---
title: Aan de slag met SAP op Azure Virtual machines | Microsoft Docs
description: Meer informatie over SAP oplossingen die worden uitgevoerd op virtuele machines (VM's) in Microsoft Azure
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a0dcb941db5038b7c904c9eaa8599c5a1dc6e83
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a>Met behulp van Azure voor het hosten en uitvoeren van SAP werkbelasting scenario 's
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
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription

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
[deploy-template-cli]:../../../resource-group-template-deploy.md
[deploy-template-portal]:../../../resource-group-template-deploy.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c


[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056
[ha-guide]:sap-high-availability-guide.md
[ha-guide-get-started]:sap-high-availability-guide-start.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
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
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
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
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#capture-the-vm
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
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
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

Als u Microsoft Azure als uw SAP gereed cloud partner kiest, zich u op betrouwbare wijze uw scenario's en bedrijfskritieke SAP taken uitvoeren op een schaalbare, voldoen aan het beleid en enterprise bewezen platform.  Ga voor de schaalbaarheid, flexibiliteit en kostenbesparingen van Azure. De uitgebreide samenwerking tussen Microsoft en SAP, u kunt tegenkomen SAP-toepassingen ontwikkelen en testen en productie scenario's in Azure - en volledig worden ondersteund. Uit SAP NetWeaver SAP S4/HANA, SAP BI, Linux bij Windows, SAP HANA naar SQL hebt u behandeld.

Naast de SAP NetWeaver scenario's met de verschillende DBMS in Azure hosten, kunt u hosten verschillende andere SAP werkbelasting scenario's zoals SAP BI in Azure. Documentatie over SAP NetWeaver implementaties op virtuele Machines in Azure systeemeigen vindt u in de sectie "SAP NetWeaver op virtuele Machines in Azure."

Azure heeft systeemeigen virtuele Machine van Azure-aanbiedingen die ooit in grootte van de CPU en geheugenbronnen kost omvatten SAP-werkbelasting die gebruikmaakt van SAP HANA groeien. Meer informatie over dit onderwerp, zoekt u de documenten in de sectie SAP HANA op Azure Virtual Machines."

De uniekheid van Azure voor SAP HANA is een unieke aanbieding die Azure naast concurrentie ingesteld. Om in te schakelen die als host fungeert meer geheugen en CPU-bronnen veeleisende SAP-scenario's met betrekking tot SAP HANA, Azure biedt het gebruik van de klant toegewezen bare-metal hardware omwille van de SAP HANA-implementaties die tot 20 TB (60 TB scale-out) aan geheugen voor vereisen die worden uitgevoerd S/4HANA of andere SAP HANA-werkbelasting. Deze unieke Azure oplossing van SAP HANA in Azure (grote exemplaren) kunt u SAP HANA uitvoeren op de specifieke bare-metal hardware met de SAP-toepassingslaag of werkbelasting middleware-laag in systeemeigen Azure Virtual Machines die worden gehost. Deze oplossing wordt beschreven in meerdere documenten in de sectie 'SAP HANA in Azure (grote exemplaren)'.   

SAP werkbelasting scenario's in Azure hosten ook eisen van Identity integration en Single-Sign-On met Azure activiteit Directory verschillende SAP-onderdelen en SAP SaaS kunt maken of PaaS biedt. Een lijst van deze integratie en eenmalige aanmelding scenario's met Azure Active Directory (AAD) en SAP entiteiten wordt beschreven en wordt beschreven in de sectie ' AAD SAP Identity Integration en eenmalige aanmelding. "


## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA op SAP HANA in Azure (grote exemplaren)

### <a name="overview-and-architecture-of-sap-hana-on-azure-large-instances"></a>Overzicht en architectuur van SAP HANA in Azure (grote exemplaren)
Titel: Overzicht en architectuur van SAP HANA in Azure (grote exemplaren)

Overzicht: Deze architectuur en technische Implementatiehandleiding bevat informatie over het implementeren van SAP op de nieuwe SAP HANA in Azure (grote exemplaren) in Azure. Het is niet bedoeld als een uitgebreide handleiding die betrekking hebben op specifieke installatie van het SAP-oplossingen, maar in plaats daarvan nuttige informatie in de eerste installatie en voortgaande operaties. Deze moet niet vervangen met SAP documentatie die betrekking heeft op de installatie van de SAP HANA (of veel SAP ondersteuning opmerkingen die betrekking hebben op het onderwerp). Het biedt een overzicht en biedt de extra details SAP HANA op Azure (grote exemplaren) te installeren.

Bijgewerkte: Oktober 2017

[Deze handleiding vindt u hier](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="infrastructure-and-connectivity-to-sap-hana-on-azure-large-instances"></a>Infrastructuur en de verbinding met SAP HANA in Azure (grote exemplaren)
Titel: Infrastructuur en de verbinding met SAP HANA in Azure (grote exemplaren)

Overzicht: Nadat de aankoop van SAP HANA in Azure (grote exemplaren) tussen u en Microsoft enterprise-accountteam is voltooid, verschillende netwerkconfiguraties nodig zijn om de juiste verbinding.  Dit document bevat een overzicht van de gegevens moeten worden gedeeld met de volgende informatie is vereist. Dit document bevat een overzicht van welke gegevens moeten worden verzameld en wat configuratiescripts worden uitgevoerd.

Bijgewerkte: Oktober 2017

[Deze handleiding vindt u hier](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="install-sap-hana-in-sap-hana-on-azure-large-instances"></a>SAP HANA in SAP HANA in Azure (grote exemplaren) installeren
Titel: SAP HANA installeren op SAP HANA in Azure (grote exemplaren)

Overzicht: Dit document bevat de installatieprocedures voor SAP HANA installeren op uw grote exemplaar van Azure.

Bijgewerkte: Juli 2017

[Deze handleiding vindt u hier](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-and-disaster-recovery-of-sap-hana-on-azure-large-instances"></a>Hoge beschikbaarheid en herstel na noodgevallen van SAP HANA in Azure (grote exemplaren)
Titel: Hoge beschikbaarheid en herstel na noodgevallen van SAP HANA in Azure (grote exemplaren)

Overzicht: Hoge beschikbaarheid en herstel na noodgeval (DR) zijn zeer belangrijke aspecten van uw bedrijfskritieke SAP HANA worden uitgevoerd op Azure (grote exemplaren) (s). Deze's importeren in de SAP, uw system integrator en/of Microsoft goed ontwerpen en implementeren van het recht HA-/ DR-strategie voor u samenwerkt. Belangrijke overwegingen zoals Recovery Point Objective (RPO) en herstel tijd Objective (RTO), specifiek voor uw omgeving, moeten worden overwogen.  Dit document wordt uitgelegd uw opties voor het inschakelen van uw voorkeur niveau van de HA en Noodherstel.

Bijgewerkte: Oktober 2017

[Dit document vindt u hier](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="troubleshooting-and-monitoring-of-sap-hana-on-azure-large-instances"></a>Probleemoplossing en bewaking van SAP HANA in Azure (grote exemplaren)
Titel: Probleemoplossing en bewaking van SAP HANA in Azure (grote exemplaren)

Overzicht: Deze gids vindt u informatie die nuttig zijn bij het vaststellen van de bewaking van uw SAP HANA op Azure-omgeving, evenals aanvullende informatie over probleemoplossing.

Bijgewerkte: Oktober 2017

[Dit document vindt u hier](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA in virtuele machines op Azure

### <a name="getting-started-with-sap-hana-on-azure"></a>Aan de slag met SAP HANA op Azure
Titel: Quick Start guide voor handmatige installatie van het SAP HANA op Azure Virtual machines

Overzicht: Deze snelstartgids helpt bij het instellen van een single instance SAP HANA-systeem op Azure VM's door een handmatige installatie van SAP NetWeaver 7.5 en SAP HANA SP12. De handleiding wordt ervan uitgegaan dat de lezer bekend met de basisprincipes van Azure IaaS zoals het implementeren van virtuele machines of virtuele netwerken, hetzij via Azure portal of Powershell/CLI is, inclusief de mogelijkheid om json-sjablonen te gebruiken. Bovendien wordt verwacht dat de lezer bekend met SAP HANA, SAP NetWeaver en het is on-premises installeert.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier](hana-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="s4hana-sap-cal-deployment-on-azure"></a>S/4HANA CAL SAP-implementatie in Azure
Titel: SAP S/4HANA of BW/4HANA in Azure implementeren

Overzicht: Deze handleiding helpt ter illustratie van de implementatie van SAP S/4HANA op Azure met behulp van SAP-Cloudbibliotheek toestel. SAP-Cloudbibliotheek toestel is een service door SAP waarmee SAP-toepassingen in Azure implementeren. De handleiding wordt beschreven stap voor stap in de implementatie.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier](cal-s4h.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-of-sap-hana-in-azure-virtual-machines"></a>Hoge beschikbaarheid van SAP HANA in virtuele Machines in Azure
Titel: Hoge beschikbaarheid van SAP HANA op virtuele Machines in Azure

Overzicht: In deze handleiding helpt u bij de configuratie voor hoge beschikbaarheid van de SUSE 12 OS en SAP HANA HANA System replicatie met automatische failover voor. De handleiding is specifiek voor Azure virtuele Machines en SUSE. De handleiding is niet van toepassing nog voor Red Hat of bare metal of persoonlijke cloud- of andere niet-Azure openbare cloud-implementaties.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier](sap-hana-high-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-backup-overview-on-azure-vms"></a>Back-overzicht van de SAP HANA op Azure Virtual machines
Titel: Back-up-handleiding voor SAP HANA op Azure Virtual Machines

Overzicht: In deze handleiding bevat algemene informatie over back-mogelijkheden voor SAP HANA uitgevoerd op Azure Virtual Machines.

Bijgewerkte: Maart 2017

[Deze handleiding vindt u hier](sap-hana-backup-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-file-level-backup-on-azure-vms"></a>SAP HANA niveau back-up op Azure Virtual machines
Titel: SAP HANA back-up op basis van opslag-momentopnamen

Overzicht: In deze handleiding bevat informatie over het gebruik van back-ups op basis van een momentopname op Azure VM's wanneer SAP HANA wordt uitgevoerd op Azure Virtual Machines.

Bijgewerkte: Maart 2017

[Deze handleiding vindt u hier](sap-hana-backup-file-level.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="sap-hana-snapshot-based-backups-on-azure-vms"></a>SAP HANA momentopname op basis van back-ups op Azure Virtual machines
Titel: SAP HANA Azure back-up op bestandsniveau

Overzicht: Deze handleiding bevat informatie over het gebruik van SAP HANA niveau back-up SAP HANA uitgevoerd op Azure Virtual Machines

Bijgewerkte: Maart 2017

[Deze handleiding vindt u hier](sap-hana-backup-storage-snapshots.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>SAP NetWeaver geïmplementeerd op Azure Virtual Machines

### <a name="deploy-sap-ides-system-on-windows-and-sql-server-through-sap-cal-on-azure"></a>SAP IDE-systeem voor Windows en SQL-Server via SAP CAL in Azure implementeren
Titel: SAP NetWeaver op Microsoft Azure SUSE Linux VM's testen

Overzicht: Dit document beschrijft de implementatie van een op basis van Windows en SQL Server op Azure met behulp van SAP-Cloudbibliotheek toestel SAP IDE-systeem. SAP Cloud toestel bibliotheek is een SAP-service die de implementatie van SAP-producten kan op Azure. Dit document gaat stap voor stap door de implementatie van een SAP IDE-systeem. Het IDE-systeem is slechts een voorbeeld voor verschillende andere dozijn toepassingen die kunnen worden geïmplementeerd via SAP Cloud toestel op Microsoft Azure.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier](cal-ides-erp6-erp7-sp3-sql.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="quickstart-guide-for-netweaver-on-suse-linux-on-azure"></a>Quick Start guide for NetWeaver op SUSE Linux op Azure
Titel: SAP NetWeaver op Microsoft Azure SUSE Linux VM's testen

Overzicht: In dit artikel beschrijft verschillende overwegingen wanneer u SAP NetWeaver op virtuele Microsoft Azure SUSE Linux-machines (VM's) uitvoert. SAP NetWeaver wordt officieel op SUSE Linux virtuele machines in Azure ondersteund. Alle gegevens met betrekking tot Linux-versies, SAP kernel-versies en andere details vindt u in SAP-notitie 1928533 ' SAP-toepassingen in Azure: ondersteunde producten en Azure VM typen '.

Bijgewerkte: September 2016

[Deze handleiding vindt u hier](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planning en implementatie
Titel: Azure virtuele Machines, planning en implementatie voor SAP NetWeaver

Overzicht: Dit document is de handleiding wilt beginnen met als u denkt over SAP NetWeaver in Azure virtuele Machines. Deze handleiding voor planning en implementatie kunt u bepalen of een bestaande of geplande SAP NetWeaver-systeem kan worden geïmplementeerd op een virtuele Machines van Azure-omgeving. Hierin komen verschillende SAP NetWeaver-implementatiescenario’s aan bod en worden specifieke SAP-configuraties voor Azure beschreven. Het artikel bevat en alle vereiste configuratiegegevens moet u aan de kant SAP/Azure uitvoeren van een hybride SAP liggend beschrijft. Daarnaast wordt beschreven welke maatregelen u kunt nemen om een hoge beschikbaarheid van SAP NetWeaver-systemen in IaaS te garanderen.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier][planning-guide]

### <a name="high-availability-configurations-of-sap-netweaver-in-azure-vms"></a>Configuraties voor hoge beschikbaarheid van SAP NetWeaver in Azure VM 's
Titel: Azure virtuele Machines voor hoge beschikbaarheid voor SAP NetWeaver

Overzicht: In dit document, we hebben betrekking op de stappen die u ondernemen kunt om hoge beschikbaarheid SAP-systemen in Azure implementeren met behulp van het Azure Resource Manager-implementatiemodel. We doorlopen deze belangrijke taken. In het document wordt beschreven hoe één punt-van-fouten onderdelen, zoals geavanceerde Business Application Programming (ABAP) SAP centrale Services (ASC's) / SAP centrale Services (SCS) en databasebeheersystemen (DBMS) en redundante onderdelen, zoals SAP-toepassingsserver wilt bij uitvoering in Azure VM's worden beveiligd. Een voorbeeld van een stapsgewijze van een installatie en configuratie van een SAP-systeem voor hoge beschikbaarheid in een cluster met Windows Server Failover Clustering en SUSE Linux Enterprise Server Cluster Framework in Azure is aangetoond en weergegeven in dit document.

Bijgewerkte: Oktober 2017

[Deze handleiding vindt u hier][ha-guide-get-started]

### <a name="realizing-multi-sid-deployments-of-sap-netweaver-in-azure-vms"></a>Het realiseren van Multi-SID-implementaties van SAP NetWeaver in Azure VM 's
Titel: Maak een SAP NetWeaver multi-SID-configuratie

Overzicht: Dit document is een aanvulling op het document hoge beschikbaarheid voor SAP NetWeaver op Azure Virtual machines. Als gevolg van nieuwe functionaliteit in Azure die is geïntroduceerd in September 2016 is het mogelijk om te implementeren meerdere SAP NetWeaver ASC's / SCS-exemplaren in een paar van virtuele Azure-machines. Een configuratie kunt u het aantal VM's nodig voor het implementeren van maximaal beschikbare SAP NetWeaver configuraties mee verlagen. De handleiding beschrijft de installatie van deze configuraties multi-SID.

Bijgewerkte: December 2016

[Deze handleiding vindt u hier](high-availability-multi-sid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Implementatie van SAP NetWeaver in virtuele machines in Azure
Titel: Virtuele Machines in Azure-implementatie voor SAP NetWeaver

Samenvatting: dit document bevat procedurele richtlijnen voor het implementeren van SAP NetWeaver-software op virtuele machines in Azure. In dit document worden drie specifieke implementatiescenario's behandeld, waarbij de nadruk ligt op het inschakelen van de Azure-uitbreidingen voor bewaking voor SAP, inclusief aanbevelingen voor het oplossen van problemen met deze uitbreidingen. Dit artikel wordt ervan uitgegaan dat u de planning en implementatie-handleiding hebt gelezen.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier][deployment-guide]

### <a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>DBMS Implementatiehandleiding
Titel: Azure virtuele Machines DBMS implementatie voor SAP NetWeaver

Samenvatting: dit document bevat plannings- en implementatie-informatie voor de DBMS-systemen die moeten worden uitgevoerd in combinatie met SAP. Het eerste deel bestaat uit algemene overwegingen. De volgende delen van het artikel hebben betrekking op implementaties van verschillende DBMS-systemen in Azure die worden ondersteund door SAP. Andere DBMS gepresenteerd zijn SQL Server, SAP-as-omgeving en Oracle. In die specifieke delen worden overwegingen die u het account moet voor wanneer u SAP-systemen worden uitgevoerd op Azure in combinatie met deze DBMS besproken. Er wordt onder andere beschreven welke back-ups en methoden voor hoge beschikbaarheid door de verschillende DBMS-systemen in Azure worden ondersteund voor gebruik met SAP-toepassingen.

Bijgewerkte: Juni 2017

[Deze handleiding vindt u hier][dbms-guide]

### <a name="using-azure-site-recovery-for-sap-workload"></a>Met behulp van Azure Site Recovery voor SAP werkbelasting
Titel: SAP NetWeaver: bouwen van een noodherstel met Azure Site Recovery

Overzicht: Dit document beschrijft de manier waarop Azure Site Recovery services kunnen worden gebruikt voor de verwerking van herstel na noodgevallen. Gevallen waarbij Azure wordt gebruikt als locatie voor herstel na noodgevallen voor een on-premises SAP Liggend met Azure Site Recovery Services. Een ander scenario wordt beschreven in het document is het geval is herstel na noodgevallen Azure Azure (A2A) en hoe deze wordt beheerd met Azure Site Recovery.  

Bijgewerkte: Augustus 2017

[Deze handleiding vindt u hier](http://aka.ms/asr-sap)
