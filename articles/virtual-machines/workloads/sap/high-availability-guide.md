---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver | Microsoft Docs
description: Hoge beschikbaarheid-handleiding voor SAP NetWeaver op Azure Virtual Machines
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
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
ms.date: 01/24/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 29bf430173da706dd6fb8228b02bfe39e6014ec8
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330882"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a>Hoge beschikbaarheid voor SAP NetWeaver op virtuele Azure-machines

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

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
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

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (Configuratie voor hoge beschikbaarheid van SAP multi-SID)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam (SAP-Product beschikbaarheid Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
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
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/resource-group-overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


Virtuele Machines van Azure is de oplossing voor organisaties die compute-, opslag- en netwerkbronnen, in een minimale tijd en zonder langdurige cycli moeten. Azure Virtual Machines kunt u klassieke toepassingen zoals SAP NetWeaver gebaseerde ABAP-, Java- en een ABAP + Java-stack implementeren. Breid betrouwbaarheid en beschikbaarheid zonder extra on-premises resources. Virtuele Machines van Azure biedt ondersteuning voor cross-premises-connectiviteit, zodat u Azure Virtual Machines kunt integreren in uw organisatie on-premises domeinen, privéclouds en SAP-landschap dat systeem.

In dit artikel gaan we de stappen die u ondernemen kunt om hoge beschikbaarheid SAP-systemen in Azure implementeren met behulp van het Azure Resource Manager-implementatiemodel. We helpen u bij deze belangrijke taken:

* Zoek de juiste SAP-opmerkingen en installatiehandleidingen, die worden vermeld in de [Resources] [ sap-ha-guide-2] sectie. In dit artikel is een aanvulling op de documentatie voor installatie van SAP en SAP-opmerkingen, zijn de primaire bronnen waarmee u kunnen installeren en implementeren van SAP-software op specifieke platforms.
* Meer informatie over de verschillen tussen de Azure Resource Manager-implementatiemodel en het model van de klassieke Azure-implementatie.
* Meer informatie over Windows Server Failover Clustering quorum-modi, zodat u het model dat geschikt is voor uw Azure-implementatie kunt selecteren.
* Meer informatie over Windows Server Failover Clustering gedeelde opslag in Azure-services.
* Meer informatie over het beveiligen van één punt van fout-onderdelen, zoals Advanced Business Application Programming (ABAP) SAP Central Services (ASCS) / SAP Central Services (SCS) en databasebeheersystemen (DBMS) en redundante onderdelen, zoals SAP-toepassing Server in Azure.
* Ga als volgt een voorbeeld van een stapsgewijze van een installatie en configuratie van de SAP-systeem van een hoge beschikbaarheid in een Windows Server Failover Clustering-cluster in Azure met behulp van Azure Resource Manager.
* Meer informatie over extra stappen vereist voor het gebruik van Windows Server Failover Clustering in Azure, maar die niet nodig zijn in een on-premises implementatie.

Ter vereenvoudiging van de implementatie en configuratie, in dit artikel gebruiken we de SAP drie lagen hoge beschikbaarheid Resource Manager-sjablonen. De sjablonen automatiseren implementatie van de gehele infrastructuur die u nodig hebt voor een hoge beschikbaarheid SAP-systeem. De infrastructuur biedt ook ondersteuning voor SAP toepassing prestaties Standard (SAP's) formaat van uw SAP-systeem.

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Vereisten
Voordat u begint, controleert u dat u voldoet aan de vereisten die worden beschreven in de volgende secties. Daarnaast moet u om te controleren of alle resources die worden vermeld in de [Resources] [ sap-ha-guide-2] sectie.

In dit artikel gebruiken we Azure Resource Manager-sjablonen voor [drie lagen SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Zie voor een handig overzicht van sjablonen, [SAP Azure Resource Manager-sjablonen](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources
Deze artikelen hebben betrekking op SAP-oplossingen in Azure:

* [Azure virtuele Machines, planning en implementatie van SAP NetWeaver][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP NetWeaver][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]
* [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver (in deze handleiding)][sap-ha-guide]

> [!NOTE]
> Indien mogelijk, geven we u een koppeling naar de verwijzende SAP-installatiehandleiding (Zie de [SAP-installatiehandleidingen][sap-installation-guides]). Voor informatie over het installatieproces en vereisten is het een goed idee om te lezen van de SAP NetWeaver-installatiehandleidingen zorgvuldig. Dit artikel behandelt alleen specifieke taken voor SAP NetWeaver-systemen die u met Azure Virtual Machines gebruiken kunt.
>
>

Deze opmerkingen bij de SAP zijn gerelateerd aan het onderwerp van SAP in Azure:

| Houd er rekening mee getal | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen op Azure: Ondersteunde producten en -grootte |
| [2015553] |SAP op Microsoft Azure: Vereisten voor ondersteuning |
| [1999351] |Uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [1999351] |Virtualisatie in Windows: Uitgebreide bewaking |
| [2243692] |Gebruik van Azure Premium-SSD-opslag voor SAP DBMS-instantie |

Meer informatie over de [beperkingen van Azure-abonnementen][azure-subscription-service-limits-subscription], waaronder algemene standaardbeperkingen en maximale beperkingen.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hoge beschikbaarheid SAP met Azure Resource Manager en het model van de klassieke Azure-implementatie
De Azure Resource Manager en klassieke Azure-implementatiemodellen verschillen in de volgende gebieden:

- Resourcegroepen
- Interne Azure load balancer afhankelijkheid van de Azure-resourcegroep
- Ondersteuning voor SAP multi-SID-scenario 's

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resourcegroepen
In Azure Resource Manager, u kunt resourcegroepen gebruiken voor het beheren van alle toepassingsresources in uw Azure-abonnement. Een geïntegreerde benadering, in een resourcegroep, alle resources hebben dezelfde levenscyclus. Bijvoorbeeld alle resources worden gemaakt op hetzelfde moment en op hetzelfde moment worden verwijderd. Meer informatie over [resourcegroepen](../../../azure-resource-manager/resource-group-overview.md#resource-groups).

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Interne Azure load balancer afhankelijkheid van de Azure-resourcegroep

Er is een afhankelijkheid tussen de interne Azure load balancer (Azure Load Balancer-service) en de groep cloud service in het model klassieke Azure-implementatie. Elke interne load balancer moet een cloud service-groep.

In Azure Resource Manager, hoeft u geen Azure-resourcegroep te gebruiken van Azure Load Balancer. De omgeving is eenvoudiger en meer flexibiliteit.

### <a name="support-for-sap-multi-sid-scenarios"></a>Ondersteuning voor SAP multi-SID-scenario 's

In Azure Resource Manager kunt u meerdere SAP-systeem-id (SID) ASCS/SCS exemplaren in één cluster installeren. Multi-SID-exemplaren zijn mogelijk vanwege ondersteuning voor meerdere IP-adressen voor elke Azure interne load balancer.

Voor het gebruik van het model van de klassieke Azure-implementatie, volgt u de procedures in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS-exemplaren met behulp van Windows Server Failover Clustering in Azure met SIOS DataKeeper](https://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> Het is raadzaam dat u het implementatiemodel Azure Resource Manager voor uw SAP-installaties gebruiken. Het biedt veel voordelen die niet beschikbaar in het klassieke implementatiemodel. Meer informatie over Azure [implementatiemodellen][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Serverfailover Clustering
Windows Server Failover Clustering, vormt de basis van een hoge beschikbaarheid SAP ASCS/SCS-installatie en DBMS in Windows.

Een failover-cluster is een groep 1 + n onafhankelijke servers (knooppunten) die samenwerken om de beschikbaarheid van toepassingen en services te vergroten. Als er een storing op een knooppunt optreedt, berekent Windows Server Failover Clustering het aantal fouten die optreden kunnen tijdens het onderhoud van een gezonde cluster om toepassingen en services te bieden. U kunt kiezen uit verschillende quorum-modi voor failover clustering.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum-modi
U kunt kiezen uit vier quorum-modi wanneer u Windows Server Failover Clustering:

* **Knooppuntmeerderheid**. Elk knooppunt van het cluster kunt stemmen. Het cluster werkt alleen met een meerderheid van stemmen, dat wil zeggen, met meer dan de helft van de stemmen. U wordt aangeraden deze optie voor clusters met een oneven aantal knooppunten. Bijvoorbeeld drie knooppunten in een cluster met zeven knooppunten kunnen mislukken en het cluster stilstaande beelden een meerderheid bereikt en blijft om uit te voeren.  
* **Knooppunt en schijfmeerderheid**. Elk knooppunt en een aangewezen schijf (een schijfwitness) in de clusteropslag kunnen stemmen wanneer deze beschikbaar zijn en in de communicatie. Het cluster werkt alleen met een meerderheid van de stemmen, dat wil zeggen, met meer dan de helft van de stemmen. In deze modus is wel zinvol in een clusteromgeving met een even aantal knooppunten. Als de helft van de knooppunten en de schijf online zijn, blijft het cluster in een foutloze toestand bevindt.
* **Knooppunt en de meeste bestandsshare**. Elk knooppunt plus een aangewezen bestandsshare (een bestandssharewitness) die de beheerder maakt kunt stemmen, ongeacht of de knooppunten en een bestandsshare beschikbaar zijn en in de communicatie. Het cluster werkt alleen met een meerderheid van de stemmen, dat wil zeggen, met meer dan de helft van de stemmen. In deze modus is wel zinvol in een clusteromgeving met een even aantal knooppunten. Het is vergelijkbaar met de modus knooppunt en schijfmeerderheid, maar hierbij een witness-bestandsshare in plaats van een witness-schijf. In deze modus is eenvoudig te implementeren, maar als de bestandsshare zelf is niet maximaal beschikbaar, mogelijk als dat een single point of failure.
* **Geen meerderheid: Alleen schijf**. Het cluster heeft quorum als één knooppunt beschikbaar en in de communicatie met een specifieke schijf in de clusteropslag is. Alleen de knooppunten die ook in de communicatie met de schijf kunnen deelnemen aan het cluster. U wordt aangeraden dat u niet in deze modus gebruikt.

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Serverfailover Clustering van on-premises
Afbeelding 1 ziet u een cluster met twee knooppunten. Als de netwerkverbinding tussen de knooppunten mislukt en zowel blijven van knooppunten en die wordt uitgevoerd, een quorumschijf of een bestand deelt bepaalt welk knooppunt blijft voor de toepassingen en services van het cluster. Het knooppunt dat toegang tot het quorum schijf of de bestandsshare heeft is het knooppunt dat ervoor zorgt dat services kunnen blijven.

Omdat in dit voorbeeld een cluster met twee knooppunten wordt, gebruiken we de quorum-modus knooppunt en bestandssharemeerderheid. Het knooppunt en schijfmeerderheid is ook een geldige optie. In een productieomgeving, raden wij aan dat u een quorumschijf. Netwerk- en system-technologie kunt u maximaal beschikbaar wilt maken.

![Afbeelding 1: Voorbeeld van een Windows Server Failover Clustering-configuratie voor SAP ASCS/SCS in Azure][sap-ha-guide-figure-1000]

_**Afbeelding 1:** Voorbeeld van een Windows Server Failover Clustering-configuratie voor SAP ASCS/SCS in Azure_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Gedeelde opslag
Afbeelding 1 ziet ook een cluster met twee knooppunten gedeelde opslag. In een cluster van de gedeelde opslag on-premises detecteren alle knooppunten in het cluster gedeelde opslag. Een mechanisme voor vergrendeling beveiligt de gegevens tegen beschadiging. Alle knooppunten kunnen worden gedetecteerd als een ander knooppunt mislukt. Als één knooppunt mislukt, wordt het resterende knooppunt wordt eigenaar van de storage-resources en zorgt ervoor dat de beschikbaarheid van services.

> [!NOTE]
> U hoeft geen gedeelde schijven voor hoge beschikbaarheid met sommige toepassingen DBMS-systemen, zoals met SQL Server. SQL Server Always On DBMS-gegevens en logboekbestanden bestanden van de lokale schijf van een clusterknooppunt wordt gerepliceerd naar de lokale schijf van een ander clusterknooppunt. De configuratie van het Windows-cluster, hoeft in dat geval niet een gedeelde schijf.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Netwerk- en naamomzetting
Clientcomputers bereiken het cluster via een virtueel IP-adres en de naam van een virtuele host waarmee de DNS-server. Meerdere IP-adressen kunnen worden verwerkt door de on-premises knooppunten en de DNS-server.

In een typische installatie gebruikt u twee of meer netwerkverbindingen:

* Een speciale verbinding naar de opslag
* Een cluster-interne netwerkverbinding voor de heartbeat
* Een openbaar netwerk die clients gebruiken voor verbinding met het cluster

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Serverfailover Clustering in Azure
Vergeleken met de bare-metal of privécloud implementaties, vereist virtuele Machines van Azure extra stappen voor het configureren van Windows Server Failover Clustering. Wanneer u een gedeelde clusterschijf bouwt, moet u verschillende IP-adressen en namen van de virtuele host voor de SAP ASCS/SCS-exemplaar instellen.

In dit artikel bespreken we de belangrijkste concepten en de extra stappen vereist voor het bouwen van een cluster SAP central services met hoge beschikbaarheid in Azure. Laten we zien u hoe u het hulpprogramma van derden SIOS DataKeeper instelt en het configureren van de interne Azure load balancer. U kunt deze hulpprogramma's gebruiken om te maken van een Windows-failovercluster met een bestandsshare-witness in Azure.

![Afbeelding 2: Windows Server Failover Clustering-configuratie in Azure zonder een gedeelde schijf][sap-ha-guide-figure-1001]

_**Afbeelding 2:** Windows Server Failover Clustering-configuratie in Azure zonder een gedeelde schijf_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Schijf in Azure met SIOS DataKeeper gedeeld
U moet het cluster gedeelde opslag voor een hoge beschikbaarheid SAP ASCS/SCS-exemplaar. Azure biedt geen gedeelde opslag die u gebruiken kunt om te maken van een cluster met gedeelde opslag bieden vanaf September 2016. Software van derden SIOS DataKeeper Cluster Edition kunt u een gespiegelde opslagruimte die gedeelde clusteropslag simuleert maken. De oplossing SIOS biedt realtime synchrone gegevensreplicatie. Dit is hoe u een gedeelde schijfbron voor een cluster kunt maken:

1. Een extra Azure virtuele harde schijf (VHD) koppelen aan elk van de virtuele machines (VM's) in de configuratie van een Windows-cluster.
2. SIOS DataKeeper Cluster Edition worden uitgevoerd op beide knooppunten van de virtuele machine.
3. SIOS DataKeeper Cluster Edition configureren zodat deze overeenkomt met de inhoud van de aanvullende VHD die is gekoppeld volume van de virtuele bronmachine naar de aanvullende VHD die is gekoppeld volume van de virtuele doelmachine. SIOS DataKeeper isoleert de bron- en lokale volumes en geeft deze weer op Windows Server Failover Clustering als één gedeelde schijf.

Meer informatie krijgen over [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Afbeelding 3: Configuratie van Windows Server Failover Clustering in Azure met SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Afbeelding 3:** Configuratie van Windows Server Failover Clustering in Azure met SIOS DataKeeper_

> [!NOTE]
> U hoeft geen gedeelde schijven voor hoge beschikbaarheid met sommige DBMS-producten, zoals SQL Server. SQL Server Always On DBMS-gegevens en logboekbestanden bestanden van de lokale schijf van een clusterknooppunt wordt gerepliceerd naar de lokale schijf van een ander clusterknooppunt. De configuratie van het Windows-cluster, hoeft in dit geval niet een gedeelde schijf.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Naamomzetting in Azure
Het Azure-cloud-platform wordt niet de optie voor het configureren van virtuele IP-adressen, zoals zwevende IP-adressen bieden. U moet een alternatieve oplossing voor het instellen van een virtueel IP-adres de cluster-bron in de cloud te bereiken.
Azure heeft een interne load balancer in de Azure Load Balancer-service. Met de interne load balancer bereiken clients het cluster via het cluster virtuele IP-adres.
U moet de interne load balancer in de resourcegroep met de clusterknooppunten implementeren. Vervolgens configureert u alle benodigde poort regels voor doorsturen met de test poorten van de interne load balancer.
De clients verbinding kunnen maken via de naam van de virtuele host. De DNS-server wordt omgezet de cluster-IP-adres en de interne load balancer-ingangen poort doorsturen naar het actieve knooppunt van het cluster.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver met hoge beschikbaarheid in Azure Infrastructure-as-a-Service (IaaS)
Voor het bereiken van hoge beschikbaarheid voor de SAP-toepassingen, zoals voor SAP-software-onderdelen, moet u de volgende onderdelen beveiligd:

* SAP-toepassingsserver instantie
* SAP ASCS/SCS-exemplaar
* DBMS server

Zie voor meer informatie over het beveiligen van SAP-onderdelen in scenario's voor hoge beschikbaarheid [Azure Virtual Machines planning en implementatie van SAP NetWeaver](planning-guide.md).

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> Hoge beschikbaarheid SAP-toepassingsserver
U kunt een specifieke oplossing voor hoge beschikbaarheid doorgaans niet nodig voor de SAP-toepassingsserver en dialoogvenster-exemplaren. Bereik een hoge beschikbaarheid met redundantie en configureert u meerdere exemplaren van het dialoogvenster in verschillende exemplaren van Azure Virtual Machines. U moet ten minste twee exemplaren met SAP-toepassing in twee exemplaren van Azure Virtual Machines geïnstalleerd hebben.

![Afbeelding 4: Hoge beschikbaarheid SAP-toepassingsserver][sap-ha-guide-figure-2000]

_**Afbeelding 4:** Hoge beschikbaarheid SAP-toepassingsserver_

U moet alle virtuele machines die SAP-toepassingsserver exemplaren hosten in de dezelfde Azure-beschikbaarheidsset plaatsen. Een Azure-beschikbaarheidsset zorgt ervoor dat:

* Alle virtuele machines maken deel uit van de dezelfde upgradedomein. Een upgradedomein bijvoorbeeld ervoor zorgt dat de virtuele machines op hetzelfde moment tijdens de downtime voor gepland onderhoud niet zijn bijgewerkt.
* Alle virtuele machines maken deel uit van hetzelfde foutdomein. Een foutdomein bijvoorbeeld, kunt u ervoor zorgen dat virtuele machines zijn geïmplementeerd, zodat er geen storingspunt is van invloed op de beschikbaarheid van alle virtuele machines.

Meer informatie over het [de beschikbaarheid van virtuele machines beheren][virtual-machines-manage-availability].

Omdat de Azure storage-account een potentieel single point of failure is, is het belangrijk dat u hebt ten minste twee Azure storage-accounts, waarbij ten minste twee virtuele machines worden gedistribueerd. In een ideale instelling, wordt de schijven van elke virtuele machine waarop een exemplaar van SAP dialoogvenster wordt geïmplementeerd in een ander opslagaccount.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> Hoge beschikbaarheid SAP ASCS/SCS-exemplaar
Afbeelding 5 is een voorbeeld van een hoge beschikbaarheid SAP ASCS/SCS-exemplaar.

![Afbeelding 5: Hoge beschikbaarheid SAP ASCS/SCS-exemplaar][sap-ha-guide-figure-2001]

_**Afbeelding 5:** Hoge beschikbaarheid SAP ASCS/SCS-exemplaar_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS-exemplaar een hoge beschikbaarheid met Windows Server Failover Clustering in Azure
Vergeleken met de bare-metal of privécloud implementaties, vereist virtuele Machines van Azure extra stappen voor het configureren van Windows Server Failover Clustering. Voor het bouwen van een Windows-failovercluster, moet u een gedeelde clusterschijf, verschillende IP-adressen, namen van meerdere virtuele host en een interne Azure load balancer voor de clustering van een SAP ASCS/SCS-exemplaar. We bespreken in meer detail later in dit artikel.

![Afbeelding 6: Windows Server Failover Clustering voor een SAP ASCS/SCS-configuratie in Azure met behulp van SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Afbeelding 6:** Windows Server Failover Clustering voor een SAP ASCS/SCS-configuratie in Azure met SIOS DataKeeper_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hoge beschikbaarheid DBMS-exemplaar
De DBMS-systemen is ook een single point of neem contact op met in een SAP-systeem. U moet deze beschermen met behulp van een oplossing voor hoge beschikbaarheid. Afbeelding 7 toont een SQL Server Always On hoge beschikbaarheid-oplossing in Azure, met Windows Server Failover Clustering en de interne Azure load balancer. SQL Server Always On repliceert DBMS-gegevens en logboekbestanden bestanden met behulp van een eigen DBMS-replicatie. In dit geval u niet moet het cluster gedeelde schijven die vereenvoudigt de volledige installatie.

![Afbeelding 7: Voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server Always On][sap-ha-guide-figure-2003]

_**Afbeelding 7:** Voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server Always On_

Zie de volgende artikelen voor meer informatie over clusters van SQL Server in Azure met behulp van het implementatiemodel Azure Resource Manager:

* [AlwaysOn-beschikbaarheidsgroep configureren in Azure Virtual Machines handmatig met behulp van Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Een interne Azure load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end implementatiescenario's voor hoge beschikbaarheid

### <a name="deployment-scenario-using-architectural-template-1"></a>Implementatiescenario architectuur sjabloon 1 gebruikt

Afbeelding 8 toont een voorbeeld van een architectuur voor hoge beschikbaarheid van SAP NetWeaver in Azure voor **één** SAP-systeem. In dit scenario is als volgt instellen:

- Een speciaal cluster wordt gebruikt voor de SAP ASCS/SCS-exemplaar.
- Een speciaal cluster wordt gebruikt voor de DBMS-instantie.
- SAP-toepassingsserver instanties zijn geïmplementeerd in hun eigen toegewezen virtuele machines.

![Afbeelding 8: SAP-architectuur hoge beschikbaarheid-sjabloon 1, met specifieke cluster voor ASCS/SCS en DBMS-systemen][sap-ha-guide-figure-2004]

_**Afbeelding 8:** SAP-architectuur sjabloon 1 hoge beschikbaarheid, toegewezen clusters voor ASCS/SCS en DBMS-systemen_

### <a name="deployment-scenario-using-architectural-template-2"></a>Implementatiescenario met architectuur 2 van de sjabloon

Afbeelding 9 toont een voorbeeld van een architectuur voor hoge beschikbaarheid van SAP NetWeaver in Azure voor **één** SAP-systeem. In dit scenario is als volgt instellen:

- Een speciaal cluster wordt gebruikt voor **beide** de SAP ASCS/SCS-exemplaar en de DBMS-systemen.
- SAP-toepassingsserver instanties zijn geïmplementeerd in een eigen toegewezen virtuele machines.

![Afbeelding 9: SAP-hoge beschikbaarheid architectuur sjabloon 2, met een specifieke cluster voor ASCS/SCS en een specifieke cluster voor DBMS-systemen][sap-ha-guide-figure-2005]

_**Afbeelding 9:** SAP-hoge beschikbaarheid architectuur sjabloon 2, met een specifieke cluster voor ASCS/SCS en een specifieke cluster voor DBMS-systemen_

### <a name="deployment-scenario-using-architectural-template-3"></a>Implementatiescenario met architectuur sjabloon 3

Afbeelding 10 toont een voorbeeld van een architectuur voor hoge beschikbaarheid van SAP NetWeaver in Azure voor **twee** SAP-systemen, met &lt;SID1&gt; en &lt;SID2&gt;. In dit scenario is als volgt instellen:

- Een speciaal cluster wordt gebruikt voor **beide** het exemplaar SAP ASCS/SCS SID1 *en* het SID2 van SAP ASCS/SCS-exemplaar (één cluster).
- Een speciaal cluster wordt gebruikt voor DBMS SID1 en een andere specifieke cluster wordt gebruikt voor DBMS SID2 (twee clusters).
- SAP-toepassingsserver instanties voor de SAP-systeem SID1 hebben hun eigen toegewezen virtuele machines.
- SAP-toepassingsserver instanties voor de SAP-systeem SID2 hebben hun eigen toegewezen virtuele machines.

![Afbeelding 10: SAP-hoge beschikbaarheid architectuur sjabloon 3, met een specifieke cluster voor verschillende ASCS/SCS-exemplaren][sap-ha-guide-figure-6003]

_**Afbeelding 10:** SAP-hoge beschikbaarheid architectuur sjabloon 3, met een specifieke cluster voor verschillende ASCS/SCS-exemplaren_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> De infrastructuur voorbereiden

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a>De infrastructuur voorbereiden voor architectuur sjabloon 1
Azure Resource Manager-sjablonen voor SAP te vereenvoudigen van de implementatie van de vereiste resources.

De sjablonen met drie lagen in Azure Resource Manager ondersteunen ook scenario's voor hoge beschikbaarheid, zoals in de architectuur sjabloon 1, met twee clusters. Elk cluster is een SAP enkel storingspunt voor SAP ASCS/SCS en DBMS-systemen.

Dit is waar u Azure Resource Manager-sjablonen kunt ophalen voor het voorbeeldscenario dat in dit artikel wordt beschreven:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

De infrastructuur voorbereiden voor architectuur sjabloon 1:

- In de Azure-portal op de **Parameters** blade in de **SYSTEMAVAILABILITY** Schakel **HA**.

  ![Afbeelding 11: SAP hoge beschikbaarheid Azure Resource Manager-parameters instellen][sap-ha-guide-figure-3000]

_**Afbeelding 11:** SAP hoge beschikbaarheid Azure Resource Manager-parameters instellen_


  De sjablonen worden gemaakt:

  * **Virtuele machines**:
    * SAP-toepassingsserver virtuele machines: <*SAPSystemSID*> - di - <*getal*>
    * ASCS/SCS cluster virtuele machines: <*SAPSystemSID*> - ascs - <*getal*>
    * DBMS-cluster: <*SAPSystemSID*> - db - <*getal*>

  * **Netwerkkaarten voor alle virtuele machines, met bijbehorende IP-adressen**:
    * <*SAPSystemSID*> - nic - di - <*getal*>
    * <*SAPSystemSID*>-nic-ascs-<*Number*>
    * <*SAPSystemSID*> - nic - db - <*getal*>

  * **Azure storage-accounts**

  * **Beschikbaarheidsgroepen** voor:
    * SAP-toepassingsserver virtuele machines: <*SAPSystemSID*> - avset - detectie
    * SAP ASCS/SCS cluster virtuele machines: <*SAPSystemSID*> - avset - ascs
    * DBMS cluster virtuele machines: <*SAPSystemSID*> - avset - db

  * **Interne Azure load balancer**:
    * Met alle poorten voor de ASCS/SCS-exemplaar en het IP-adres <*SAPSystemSID*> - lb - ascs
    * Met alle poorten voor de SQL Server DBMS en IP-adres <*SAPSystemSID*> - lb - db

  * **Netwerkbeveiligingsgroep**: <*SAPSystemSID*> - nsg - ascs-0  
    * Met een open externe Remote Desktop Protocol (RDP)-poort naar de <*SAPSystemSID*> - ascs - 0 virtuele machine

> [!NOTE]
> Alle IP-adressen van de netwerkkaarten en interne Azure load balancers zijn **dynamische** standaard. Deze te wijzigen in **statische** IP-adressen. Hoe u dit doet later in dit artikel wordt beschreven.
>
>

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Virtuele machines met de zakelijke netwerkverbinding (cross-premises) te gebruiken in de productieomgeving implementeren
Implementeren voor productie SAP-systemen, Azure virtual machines met [zakelijke netwerkverbinding (cross-premises)] [ planning-guide-2.2] met behulp van Azure Site-naar-Site VPN of Azure ExpressRoute.

> [!NOTE]
> U kunt uw Azure Virtual Network-exemplaar gebruiken. Het virtuele netwerk en subnet zijn al gemaakt en voorbereid.
>
>

1.  In de Azure-portal op de **Parameters** blade in de **NEWOREXISTINGSUBNET** Schakel **bestaande**.
2.  In de **SUBNETID** vak, voegt u de volledige tekenreeks van het voorbereide Azure netwerk SubnetID waar u van plan bent uw Azure-machines implementeren.
3.  Als u een lijst met alle Azure-netwerksubnetten, moet u deze PowerShell-opdracht uitvoeren:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  De **ID** veld geeft de **SUBNETID**.
4. Voor een lijst van alle **SUBNETID** waarden, deze PowerShell-opdracht uitvoeren:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  De **SUBNETID** ziet er als volgt:

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> SAP-instanties voor alleen-cloud voor testen en demo implementeren
U kunt uw SAP-systeem voor hoge beschikbaarheid in een alleen-cloud implementatiemodel implementeren. Dit type implementatie is vooral handig voor demo's en test gebruiksvoorbeelden. Het niet geschikt voor productiegebruikssituaties.

- In de Azure-portal op de **Parameters** blade in de **NEWOREXISTINGSUBNET** Schakel **nieuwe**. Laat de **SUBNETID** veld leeg.

  De SAP Azure Resource Manager-sjabloon maakt automatisch de Azure-netwerk en subnet.

> [!NOTE]
> Ook moet u ten minste één toegewezen virtuele machine voor Active Directory en DNS implementeren in dezelfde instantie van Azure Virtual Network. De sjabloon maken niet deze virtuele machines.
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a>De infrastructuur voorbereiden voor architectuur sjabloon 2

U kunt deze Azure Resource Manager-sjabloon voor SAP vereenvoudigt het implementeren van resources van de vereiste infrastructuur voor SAP architectuur sjabloon 2.

Dit is waar u Azure Resource Manager-sjablonen voor deze implementatiescenario kunt krijgen:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a>De infrastructuur voorbereiden voor architectuur sjabloon 3

U kunt de infrastructuur voorbereiden en configureren van SAP voor **multi-SID**. U kunt bijvoorbeeld een extra SAP ASCS/SCS-exemplaar in toevoegen een *bestaande* clusterconfiguratie. Zie voor meer informatie, [configureren van een extra SAP ASCS/SCS-exemplaar naar een bestaande configuratie van het cluster voor het maken van een SAP-multi-SID-configuratie in Azure Resource Manager][sap-ha-multi-sid-guide].

Als u een nieuw multi-SID-cluster maakt wilt, kunt u de multi-SID [quickstart-sjablonen op GitHub](https://github.com/Azure/azure-quickstart-templates).
Voor het maken van een nieuw cluster met multi-SID, moet u de volgende drie sjablonen implementeren:

* [ASCS/SCS-sjabloon](#ASCS-SCS-template)
* [Database-sjabloon](#database-template)
* [Toepassingssjabloon van servers](#application-servers-template)

De volgende secties hebben meer informatie over de sjablonen en de parameters die u moet opgeven in de sjablonen.

#### <a name="ASCS-SCS-template"></a> ASCS/SCS-sjabloon

De ASCS/SCS-sjabloon implementeert twee virtuele machines die u gebruiken kunt om te maken van een Windows Server-failovercluster dat als host fungeert voor meerdere ASCS/SCS-exemplaren.

In de ASCS/SCS multi-SID-sjabloon instellen de [ASCS/SCS multi-SID sjabloon][sap-templates-3-tier-multisid-xscs-marketplace-image], voer waarden in voor de volgende parameters:

  - **Resource-voorvoegsel**.  Stel de resource-voorvoegsel dat wordt gebruikt als voorvoegsel voor alle resources die zijn gemaakt tijdens de implementatie. Omdat de resources niet tot slechts één SAP-systeem behoren, wordt het voorvoegsel van de resource is niet de SID van een SAP-systeem.  Het voorvoegsel moet liggen tussen **drie tot zes tekens**.
  - **Stack-Type**. Selecteer de stack-type van de SAP-systeem. Azure Load Balancer zijn afhankelijk van het type stack, een (ABAP of alleen Java) of twee (ABAP + Java) privé IP-adressen per SAP-systeem.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **Telling van SAP-systeem**. Selecteer het aantal SAP-systemen die u wilt installeren in dit cluster.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Nieuw of bestaand Subnet**. Instellen of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
  -  **Subnet-Id**. Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID meestal er als volgt uitzien: /subscriptions/ <*abonnements-id*> /resourceGroups/ <*groepsnaam voor accountresources*> /providers/Microsoft.Network/virtualNetworks/ < *virtuele-netwerknaam*> /subnets/ <*subnetnaam*>

De sjabloon implementeert één exemplaar voor Azure Load Balancer, die ondersteuning biedt voor meerdere SAP-systemen.

- De ASCS-exemplaren zijn geconfigureerd voor exemplaarnummer 00, 10, 20...
- De SCS-exemplaren zijn geconfigureerd voor het exemplaar, getal, 01, 11, 21...
- De exemplaren ASCS in de wachtrij plaatsen replicatie Server (gebruikers) (alleen Linux) zijn geconfigureerd voor het exemplaar, getal, 02, 12, 22...
- De SCS ERS (alleen Linux)-exemplaren zijn geconfigureerd voor exemplaarnummer 03, 13, 23...

De load balancer bevat 1 (2 voor Linux) VIP(s), 1 x-VIP voor ASCS/SCS en 1 x-VIP voor gebruikers (alleen Linux).

De volgende lijst bevat alle load balancer-regels (waarbij x staat voor het nummer van de SAP-systeem, bijvoorbeeld 1, 2, 3...):
- Windows-specifieke poorten voor elk SAP-systeem: 445, 5985
- ASCS-poorten (exemplaarnummer x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016
- SCS-poorten (exemplaarnummer x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116
- ASCS ERS poorten op Linux (exemplaarnummer x2): 33x2, 5x213, 5x214, 5x216
- SCS ERS poorten op Linux (exemplaarnummer x3): 33x3, 5x313, 5x314, 5x316

De load balancer is geconfigureerd voor het gebruik van de volgende test-poorten (waarbij x staat voor het nummer van de SAP-systeem, bijvoorbeeld 1, 2, 3...):
- ASCS/SCS interne load balancer-test-poort: 620x0
- UIT interne load balancer-test-poort (alleen Linux): 621x2

#### <a name="database-template"></a> Database-sjabloon

De databasesjabloon implementeert een of twee virtuele machines die u gebruiken kunt voor het installeren van het relationele databasebeheersysteem (RDBMS) voor een SAP-systeem. Bijvoorbeeld, als u een ASCS/SCS-sjabloon voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van de sjabloon van de database-multi-SID, in de [database multi-SID sjabloon][sap-templates-3-tier-multisid-db-marketplace-image], voer waarden in voor de volgende parameters:

  -  **SAP-systeem-Id**. Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **Dbtype**. Selecteer het type van de database die u wilt installeren op het cluster. Selecteer **SQL** als u wilt installeren van Microsoft SQL Server. Selecteer **HANA** als u van plan bent voor het installeren van SAP HANA op de virtuele machines. Zorg ervoor dat u het juiste besturingssysteem-type selecteren: Selecteer **Windows** voor SQL, en selecteer een Linux-distributie voor HANA. De Azure Load Balancer die is verbonden met de virtuele machines wordt geconfigureerd ter ondersteuning van het type van de geselecteerde database:
    * **SQL**. De load balancer wordt taakverdeling op poort 1433. Zorg ervoor dat u deze poort voor de SQL Server Always On-configuratie.
    * **HANA**. De load balancer wordt verdelen poorten 35015 en 35017. Zorg ervoor dat u het installeren van SAP HANA met exemplaarnummer **50**.
    De load balancer gebruikt testpoort 62550.
  -  **SAP-systeem grootte**. Stel het aantal SAP's u het nieuwe systeem vindt. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**. Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de ASCS/SCS-sjabloon of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASCS/SCS.

#### <a name="application-servers-template"></a> Toepassingssjabloon van servers

De toepassing servers-sjabloon implementeert twee of meer virtuele machines die kan worden gebruikt als instanties van SAP-toepassingsserver voor een SAP-systeem. Bijvoorbeeld, als u een ASCS/SCS-sjabloon voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van de toepassing servers multi-SID-sjabloon in de [toepassingssjabloon servers multi-SID][sap-templates-3-tier-multisid-apps-marketplace-image], voer waarden in voor de volgende parameters:

  -  **SAP-systeem-Id**. Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **SAP-systeem grootte**. Het aantal SAP's u het nieuwe systeem vindt. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**. Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de ASCS/SCS-sjabloon of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASCS/SCS.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure-netwerk
In ons voorbeeld is de adresruimte van het Azure-netwerk 10.0.0.0/16. Er is één subnet met de naam **Subnet**, met een adresbereik van 10.0.0.0/24. Alle virtuele machines en interne load balancers worden geïmplementeerd in dit virtuele netwerk.

> [!IMPORTANT]
> Breng geen wijzigingen in de netwerkinstellingen van het in het gastbesturingssysteem. Dit omvat IP-adressen, DNS-servers en -subnet. De netwerkinstellingen configureren in Azure. De Dynamic Host Configuration Protocol (DHCP)-service geeft de instellingen.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS-IP-adressen

Als u wilt de vereiste DNS IP-adressen instellen, moet u de volgende stappen uitvoeren.

1.  In de Azure-portal op de **DNS-servers** blade, zorg ervoor dat uw virtuele netwerk **DNS-servers** optie is ingesteld op **aangepaste DNS**.
2.  Selecteer uw instellingen op basis van het type netwerk die u hebben. Zie de volgende bronnen voor meer informatie:
    * [Verbinding met het hoofdkantoor netwerk (cross-premises)][planning-guide-2.2]: De IP-adressen van de lokale DNS-servers toevoegen.  
    U kunt on-premises DNS-servers aan de virtuele machines die worden uitgevoerd in Azure kunt uitbreiden. U kunt in dit scenario is het IP-adressen van de virtuele machines van Azure waar u de DNS-service uitgevoerd toevoegen.
    * Voor implementaties die zijn geïsoleerd in Azure: Implementeer een extra virtuele machine in hetzelfde Virtueelnetwerk-exemplaar dat als een DNS-server fungeert. De IP-adressen van de Azure virtuele machines die u hebt ingesteld om uit te voeren van DNS-service toevoegen.

    ![Afbeelding 12: DNS-servers configureren voor Azure Virtual Network][sap-ha-guide-figure-3001]

    _**Afbeelding 12:** DNS-servers configureren voor Azure Virtual Network_

  > [!NOTE]
  > Als u de IP-adressen van de DNS-servers wijzigt, moet u de virtuele machines van Azure om de wijziging toepassen en het doorgeven van de nieuwe DNS-servers opnieuw opstarten.
  >
  >

In ons voorbeeld is de DNS-service geïnstalleerd en geconfigureerd op deze virtuele machines van Windows:

| Virtuele-machinefunctie | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste DNS-server |domcontr-0 |pr1-nic-domcontr-0 |10.0.0.10 |
| Tweede DNS-server |domcontr-1 |pr1-nic-domcontr-1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Hostnamen en statische IP-adressen voor de geclusterde SAP ASCS/SCS-exemplaar en de geclusterde instantie van DBMS-systemen

Voor on-premises implementatie moet u deze gereserveerde hostnamen en IP-adressen:

| Rol van de virtuele host-naam | Naam van virtuele host | Virtuele vaste IP-adres |
| --- | --- | --- |
| SAP ASCS/SCS eerste cluster virtuele host-naam (voor cluster) |pr1-ascs-vir |10.0.0.42 |
| Naam van de virtuele host SAP ASCS/SCS-exemplaar |PR1-ascs-sap |10.0.0.43 |
| SAP DBMS tweede virtuele host clusternaam (Clusterbeheer) |pr1-dbms-vir |10.0.0.32 |

Wanneer u het cluster maakt, maakt u de virtuele hostnamen **pr1-ascs-vir** en **pr1-dbms-vir** en de bijbehorende IP-adressen die het cluster zelf beheren. Zie voor informatie over hoe u dit doet, [verzamelen van de knooppunten in een clusterconfiguratie][sap-ha-guide-8.12.1].

U kunt handmatig maken met de andere twee virtuele hostnamen, **pr1-ascs-sap** en **pr1-dbms-sap**, en de bijbehorende IP-adressen, op de DNS-server. De geclusterde SAP ASCS/SCS-exemplaar en de geclusterde instantie op DBMS-systemen kunt u deze resources gebruiken. Zie voor informatie over hoe u dit doet, [maken van een virtuele host-naam voor een geclusterde SAP ASCS/SCS-exemplaar][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Statische IP-adressen voor de SAP-virtuele machines instellen
Nadat u de virtuele machines te gebruiken in uw cluster implementeert, moet u statische IP-adressen voor alle virtuele machines instellen. Dit doen in de configuratie van Azure Virtual Network, en niet in het gastbesturingssysteem.

1.  Selecteer in de Azure portal, **resourcegroep** > **netwerkkaart** > **instellingen** > **IP-adres**.
2.  Op de **IP-adressen** blade onder **toewijzing**, selecteer **statische**. In de **IP-adres** voert u de IP-adres dat u wilt gebruiken.

  > [!NOTE]
  > Als u het IP-adres van de netwerkkaart wijzigt, moet u opnieuw opstarten van de virtuele machines van Azure om toe te passen van de wijziging.  
  >
  >

  ![Afbeelding 13: Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen][sap-ha-guide-figure-3002]

  _**Afbeelding 13:** Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen_

  Herhaal deze stap voor alle netwerkinterfaces die inclusief is, voor alle virtuele machines, virtuele machines die u wilt gebruiken voor uw Active Directory/DNS-service.

In ons voorbeeld hebben we deze virtuele machines en statische IP-adressen:

| Virtuele-machinefunctie | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste exemplaar van de SAP-toepassingsserver |pr1-di-0 |pr1-nic-di-0 |10.0.0.50 |
| Tweede instantie van SAP-toepassingsserver |pr1-di-1 |pr1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Laatste instantie van SAP-toepassingsserver |pr1-di-5 |pr1-nic-di-5 |10.0.0.55 |
| Eerste clusterknooppunt voor ASCS/SCS-exemplaar |pr1-ascs-0 |pr1-nic-ascs-0 |10.0.0.40 |
| Tweede clusterknooppunt voor ASCS/SCS-exemplaar |pr1-ascs-1 |pr1-nic-ascs-1 |10.0.0.41 |
| Eerste clusterknooppunt voor DBMS-instantie |pr1-db-0 |pr1-nic-db-0 |10.0.0.30 |
| Tweede clusterknooppunt voor DBMS-instantie |pr1-db-1 |pr1-nic-db-1 |10.0.0.31 |

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Een statisch IP-adres voor de interne Azure load balancer instellen

De SAP Azure Resource Manager-sjabloon maakt u een interne Azure load balancer die wordt gebruikt voor de SAP ASCS/SCS-exemplaar als het cluster DBMS-systemen.

> [!IMPORTANT]
> Het IP-adres van de virtuele host-naam van de SAP ASCS/SCS is hetzelfde als het IP-adres van de SAP ASCS/SCS interne load balancer: **pr1-lb-ascs**.
> Het IP-adres van de virtuele naam van de DBMS-systemen is hetzelfde als het IP-adres van de DBMS interne load balancer: **pr1-lb-dbms**.
>
>

Een statisch IP-adres voor de interne Azure load balancer instellen:

1.  De eerste implementatie van het IP-adres van interne load balancer wordt ingesteld op **dynamische**. In de Azure-portal op de **IP-adressen** blade onder **toewijzing**, selecteer **statische**.
2.  Stel het IP-adres van de interne load balancer **pr1-lb-ascs** naar het IP-adres van de virtuele host-naam van de SAP ASCS/SCS-exemplaar.
3.  Stel het IP-adres van de interne load balancer **pr1-lb-dbms** naar het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

  ![Afbeelding 14: Statische IP-adressen instellen voor de interne load balancer voor de SAP ASCS/SCS-exemplaar][sap-ha-guide-figure-3003]

  _**Afbeelding 14:** Statische IP-adressen instellen voor de interne load balancer voor de SAP ASCS/SCS-exemplaar_

In ons voorbeeld hebben we twee interne Azure load balancers die deze statische IP-adressen:

| Interne Azure load balancer-rol | Naam van de interne Azure load balancer | Statisch IP-adres |
| --- | --- | --- |
| SAP ASCS/SCS-exemplaar van interne load balancer |pr1-lb-ascs |10.0.0.43 |
| SAP DBMS interne load balancer |pr1-lb-dbms |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Standaard ASCS/SCS load balancer-regels voor de interne Azure load balancer

De SAP Azure Resource Manager-sjabloon maakt u de poorten die u nodig hebt:
* Een ABAP ASCS-instantie met het standaardaantal exemplaar **00**
* Een Java-SCS-exemplaar, met het standaardaantal exemplaar **01**

Wanneer u uw SAP ASCS/SCS-exemplaar installeert, moet u de standaard instantienummer **00** voor uw ABAP ASCS-exemplaar en het exemplaarnummer standaard **01** voor uw Java-SCS-exemplaar.

Maak vervolgens vereist interne eindpunten voor load balancer voor de SAP NetWeaver-poorten.

Als u wilt maken vereist interne eindpunten voor load balancer, maakt u eerst deze eindpunten voor load balancer voor de SAP NetWeaver ABAP ASCS-poorten:

| Service/load balancing regelnaam | Standaardpoortnummers | Concrete poorten voor (ASCS-exemplaar met exemplaarnummer 00) (INGEN met 10) |
| --- | --- | --- |
| Enqueue Server / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| ABAP Message Server / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| Interne ABAP bericht / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| Message Server HTTP / *Lbrule8100* |81 <*InstanceNumber*> |8100 |
| SAP Start Service ASCS HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP Start Service ASCS HTTPS / *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| In de wachtrij plaatsen replicatie / *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP Start Service ERS HTTP *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP Start Service ERS HTTP *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| Win RM *Lbrule5985* | |5985 |
| Bestandsshare *Lbrule445* | |445 |

_**Tabel 1:** Poortnummers van de SAP NetWeaver ABAP ASCS-instanties_

Vervolgens maakt u deze eindpunten voor load balancer voor de SAP NetWeaver Java SCS-poorten:

| Service/load balancing regelnaam | Standaardpoortnummers | Concrete poorten voor (SCS-exemplaar met exemplaarnummer 01) (INGEN met 11) |
| --- | --- | --- |
| Enqueue Server / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Gateway Server / *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Java-berichtenserver / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| Message Server HTTP / *Lbrule8101* |81 <*InstanceNumber*> |8101 |
| SAP Start Service SCS HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP Start Service SCS HTTPS / *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| In de wachtrij plaatsen replicatie / *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP Start Service ERS HTTP *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP Start Service ERS HTTP *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| Win RM *Lbrule5985* | |5985 |
| Bestandsshare *Lbrule445* | |445 |

_**Tabel 2:** Poortnummers van de SAP NetWeaver Java SCS-instanties_

![Afbeelding 15: Standaard ASCS/SCS load balancer-regels voor de interne Azure load balancer][sap-ha-guide-figure-3004]

_**Afbeelding 15:** Standaard ASCS/SCS load balancer-regels voor de interne Azure load balancer_

Stel het IP-adres van de load balancer **pr1-lb-dbms** naar het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> De ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer wijzigen

Als u gebruiken van verschillende aantallen voor de SAP ASCS of SCS wilt, moet u de namen en waarden van hun poorten wijzigen van de standaardwaarden.

1.  Selecteer in de Azure portal,  **< *SID*> ascs - lb - load balancer** > **regels voor Load Balancing**.
2.  Voor alle load balancer-regels die deel uitmaken van het SAP ASCS of SCS-exemplaar, kunt u deze waarden wijzigen:

  * Name
  * Poort
  * Back-end-poort

  Bijvoorbeeld, als u het standaardnummer van de ASCS-instantie wijzigen van 00 tot en met 31 wilt, moet u de aanbrengen voor alle poorten die worden vermeld in tabel 1.

  Hier volgt een voorbeeld van een update voor poort *lbrule3200*.

  ![Afbeelding 16: De ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer wijzigen][sap-ha-guide-figure-3005]

  _**Afbeelding 16:** De ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer wijzigen_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Windows virtuele machines toevoegen aan het domein

Nadat u een statisch IP-adres aan de virtuele machines toegewezen, moet u de virtuele machines toevoegen aan het domein.

![Afbeelding 17: Een virtuele machine toevoegen aan een domein][sap-ha-guide-figure-3006]

_**Afbeelding 17:** Een virtuele machine toevoegen aan een domein_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Registervermeldingen op beide clusterknooppunten van de SAP ASCS/SCS-exemplaar toevoegen

Azure Load Balancer heeft een interne load balancer die wordt gesloten verbindingen als de verbindingen niet actief voor een bepaalde zijn tijd (een time-out voor inactiviteit). SAP-werkprocessen in dialoogvenster exemplaren open verbindingen met de SAP in de wachtrij plaatsen verwerken zodra het eerste in de wachtrij plaatsen/uit de wachtrij verwijderen aanvragen moet worden verzonden. Deze verbindingen meestal blijven tot stand gebrachte tot het werkproces of het proces in de wachtrij plaatsen opnieuw wordt opgestart. Als de verbinding niet actief gedurende een bepaalde tijd wordt opgelost is, sluit de interne Azure load balancer de verbindingen. Dit is een probleem niet omdat het werkproces SAP herstelt u de verbinding met het proces in de wachtrij plaatsen als deze niet meer bestaat. Deze activiteiten worden beschreven in de developer-traceringen van SAP-processen, maar ze een grote hoeveelheid extra inhoud maken in deze traceringen. Is het een goed idee om te wijzigen van de TCP/IP `KeepAliveTime` en `KeepAliveInterval` op beide knooppunten van het cluster. Combineer deze wijzigingen in de TCP/IP-parameters en de SAP-profiel parameters die later in dit artikel wordt beschreven.

Als u wilt toevoegen de registervermeldingen op beide clusterknooppunten van de SAP ASCS/SCS-exemplaar, eerst deze Windows registervermeldingen toevoegen op beide clusterknooppunten Windows voor SAP ASCS/SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam van de variabele |`KeepAliveTime` |
| Type variabele |REG_DWORD (decimaal) |
| Value |120000 |
| Koppeling naar documentatie |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

_**Tabel 3:** Wijzig de eerste parameter van de TCP/IP_

Voegt u deze registervermeldingen van Windows op beide clusterknooppunten Windows voor SAP ASCS/SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam van de variabele |`KeepAliveInterval` |
| Type variabele |REG_DWORD (decimaal) |
| Value |120000 |
| Koppeling naar documentatie |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

_**Tabel 4:** Wijzig de tweede parameter van de TCP/IP_

**De wijzigingen wilt toepassen, start opnieuw op beide clusterknooppunten**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Een cluster met Windows Server Failover Clustering voor een SAP ASCS/SCS-exemplaar instellen

Instellen van een cluster met Windows Server Failover Clustering voor een SAP ASCS/SCS-exemplaar, moet deze taken uitvoeren:

- Verzamelen van de clusterknooppunten in een clusterconfiguratie
- Een cluster bestandsshare-witness configureren

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Verzamelen van de clusterknooppunten in een clusterconfiguratie

1.  In de functie toevoegen en Wizard Functies toevoegen voor failover clustering op beide clusterknooppunten.
2.  Het failover-cluster instellen met behulp van Failoverclusterbeheer. Selecteer in Failoverclusterbeheer **Cluster maken**, en voegt u alleen de naam van het eerste cluster, knooppunt A. Het tweede knooppunt niet nog; toevoegen in een latere stap voegt u het tweede knooppunt toe.

  ![Afbeelding 18: De naam van de server of virtuele machine van het eerste clusterknooppunt toevoegen][sap-ha-guide-figure-3007]

  _**Afbeelding 18:** De naam van de server of virtuele machine van het eerste clusterknooppunt toevoegen_

3.  Voer de netwerknaam (virtuele host-naam) van het cluster.

  ![Afbeelding 19: Voer de naam van het cluster][sap-ha-guide-figure-3008]

  _**Afbeelding 19:** Voer de naam van het cluster_

4.  Nadat u het cluster hebt gemaakt, voert u een clustervalidatietest.

  ![Afbeelding 20: De controle van de cluster-validatie uitvoeren][sap-ha-guide-figure-3009]

  _**Afbeelding 20:** De controle van de cluster-validatie uitvoeren_

  U kunt waarschuwingen over schijven op dit moment in het proces negeren. U voegt dat een bestandsshare-witness en de SIOS gedeelde schijven later opnieuw. In deze fase hoeft u niet bang dat een quorum.

  ![Afbeelding 21: Er is geen quorumschijf is gevonden][sap-ha-guide-figure-3010]

  _**Afbeelding 21:** Er is geen quorumschijf is gevonden_

  ![Afbeelding 22: Core-cluster-bron moet een nieuw IP-adres][sap-ha-guide-figure-3011]

  _**Afbeelding 22:** Core-cluster-bron moet een nieuw IP-adres_

5.  Wijzig het IP-adres van de core cluster-service. Het cluster kan totdat u het IP-adres van de core cluster-service niet starten omdat het IP-adres van de server naar een van de virtuele machine-knooppunten verwijst. Dit doen op de **eigenschappen** pagina van de core-clusterservice IP-resource.

  Bijvoorbeeld, moeten we een IP-adres toewijzen (in ons voorbeeld **10.0.0.42**) voor de naam van het cluster virtuele host **pr1-ascs-vir**.

  ![Afbeelding 23: Klik in het dialoogvenster Eigenschappen van de IP-adres wijzigen][sap-ha-guide-figure-3012]

  _**Afbeelding 23:** In de **eigenschappen** dialoogvenster vak, wijzigt u het IP-adres_

  ![Afbeelding 24 uur per dag: Het IP-adres dat is gereserveerd voor het cluster toewijzen][sap-ha-guide-figure-3013]

  _**Afbeelding 24 uur per dag:** Het IP-adres dat is gereserveerd voor het cluster toewijzen_

6.  Virtuele hostnaam van het cluster online brengen.

  ![Afbeelding 25: Cluster core-service actief is en uitgevoerd, en met het juiste IP-adres][sap-ha-guide-figure-3014]

  _**Afbeelding 25:** Cluster core-service actief is en uitgevoerd, en met het juiste IP-adres_

7.  Het tweede clusterknooppunt toevoegen.

  De core cluster-service is actief en werkend, kunt u het tweede clusterknooppunt toevoegen.

  ![Afbeelding 26: Het tweede clusterknooppunt toevoegen][sap-ha-guide-figure-3015]

  _**Afbeelding 26:** Het tweede clusterknooppunt toevoegen_

8.  Voer een naam voor de tweede host van de cluster-knooppunt.

  ![Afbeelding 27: Voer de tweede hostnaam van de cluster-knooppunt][sap-ha-guide-figure-3016]

  _**Afbeelding 27:** Voer de tweede hostnaam van de cluster-knooppunt_

  > [!IMPORTANT]
  > Controleer of de **alle in aanmerking komende opslag toevoegen aan het cluster** selectievakje **niet** geselecteerde.  
  >
  >

  ![Afbeelding 28: Schakel het selectievakje niet][sap-ha-guide-figure-3017]

  _**Afbeelding 28:** Voer **niet** Schakel het selectievakje_

  Waarschuwingen over quorum en schijven, kunt u negeren. U stelt het quorum en delen van de schijf later, zoals beschreven in [SIOS DataKeeper Cluster Edition installeren voor de gedeelde clusterschijf SAP ASCS/SCS][sap-ha-guide-8.12.3].

  ![Afbeelding 29: Negeren van waarschuwingen met betrekking tot de schijf quorum][sap-ha-guide-figure-3018]

  _**Afbeelding 29:** Negeren van waarschuwingen met betrekking tot de schijf quorum_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Een cluster bestandsshare-witness configureren

Een cluster bestandsshare-witness configureren, moet deze taken uitvoeren:

- Het maken van een bestandsshare
- Instellen van het quorum file share witness in Failoverclusterbeheer

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Een bestandsshare maken

1.  Selecteer een bestandsshare-witness in plaats van een quorumschijf. SIOS DataKeeper biedt ondersteuning voor deze optie.

  In de voorbeelden in dit artikel wordt de bestandsshare-witness is op de Active Directory/DNS-server die wordt uitgevoerd in Azure. De bestandsshare-witness wordt aangeroepen **domcontr-0**. Omdat u zou een VPN-verbinding naar Azure (via Site-naar-Site VPN of Azure ExpressRoute) hebt geconfigureerd, wordt in uw Active Directory/DNS-service wordt on-premises en is niet geschikt is voor het uitvoeren van een bestand witness delen.

  > [!NOTE]
  > Als uw Active Directory/DNS-service wordt uitgevoerd alleen on-premises, niet uw bestandsshare-witness configureren op de Active Directory en DNS-Windows-besturingssysteem dat on-premises wordt uitgevoerd. De netwerklatentie tussen clusterknooppunten die worden uitgevoerd in Azure en Active Directory/DNS-on-premises mogelijk te groot zijn en leiden tot problemen met de netwerkverbinding. Zorg ervoor dat de bestandsshare-witness configureren op een Azure-machine die wordt uitgevoerd dicht bij het clusterknooppunt.  
  >
  >

  Het quorumstation moet ten minste 1024 MB aan vrije ruimte. Het wordt aangeraden de 2048 MB aan vrije ruimte voor de schijf quorum.

2.  Het clusternaamobject toevoegen.

  ![Afbeelding 30: De machtigingen voor de share voor het cluster name object toewijzen][sap-ha-guide-figure-3019]

  _**Afbeelding 30:** De machtigingen voor de share voor het cluster name object toewijzen_

  Zorg ervoor dat de machtigingen voor de instantie te wijzigen van gegevens in de share voor het clusternaamobject opneemt (in ons voorbeeld **pr1-ascs-vir$**).

3.  Selecteer om de clusternaamobject toe aan de lijst met **toevoegen**. Het filter om te controleren op computerobjecten, naast die wordt weergegeven in afbeelding 31 wijzigen.

  ![Afbeelding 31: Wijzig de objecttypen die aan zijn onder andere computers][sap-ha-guide-figure-3020]

  _**Afbeelding 31:** Wijzig de objecttypen die aan zijn onder andere computers_

  ![Afbeelding 32: Schakel het selectievakje voor Computers][sap-ha-guide-figure-3021]

  _**Afbeelding 32:** Selecteer de **Computers** selectievakje_

4.  Voer het clusternaamobject zoals wordt weergegeven in afbeelding 31. Omdat de record is al is gemaakt, kunt u de machtigingen wijzigen, zoals wordt weergegeven in afbeelding 30.

5.  Selecteer de **Security** tabblad van de share en stel vervolgens meer gedetailleerde machtigingen voor het clusternaamobject.

  ![Afbeelding 33: De beveiligingskenmerken van voor de clusternaamobject instellen op het bestandsshare quorum][sap-ha-guide-figure-3022]

  _**Afbeelding 33:** De beveiligingskenmerken van voor de clusternaamobject instellen op het bestandsshare quorum_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Stel het quorum file share witness in Failoverclusterbeheer

1.  Open de instelling-Wizard clusterquorum configureren.

  ![Afbeelding 34: Start de Wizard configureren clusterquorum-instelling][sap-ha-guide-figure-3023]

  _**Afbeelding 34:** Start de Wizard configureren clusterquorum-instelling_

2.  Op de **quorumconfiguratie selecteren** pagina, selecteert u **selecteert u de quorumwitness**.

  ![Afbeelding 35: U kunt kiezen uit Quorumconfiguraties][sap-ha-guide-figure-3024]

  _**Afbeelding 35:** U kunt kiezen uit Quorumconfiguraties_

3.  Op de **Quorumwitness selecteren** pagina, selecteert u **een bestandsshare-witness configureren**.

  ![Afbeelding 36: Selecteer de bestandsshare-witness][sap-ha-guide-figure-3025]

  _**Afbeelding 36:** Selecteer de bestandsshare-witness_

4.  Geef het UNC-pad naar de bestandsshare (in ons voorbeeld \\domcontr 0\FSW). Voor een overzicht van de wijzigingen die u kunt maken, selecteert u **volgende**.

  ![Afbeelding 37: De bestandslocatie voor de share voor de share witness definiëren][sap-ha-guide-figure-3026]

  _**Afbeelding 37:** De bestandslocatie voor de share voor de share witness definiëren_

5.  Selecteer de gewenste wijzigingen aan, en selecteer vervolgens **volgende**. U moet de configuratie van het cluster is opnieuw te configureren zoals wordt weergegeven in afbeelding 38.  

  ![Afbeelding 38: Bevestigen dat u het cluster hebt geconfigureerd][sap-ha-guide-figure-3027]

  _**Afbeelding 38:** Bevestigen dat u het cluster hebt geconfigureerd_

Na de installatie van het Windows-failovercluster is, moeten de wijzigingen worden aangebracht aan de sommige drempelwaarden aan te passen van detectie van failover naar een in Azure. De parameters moeten worden gewijzigd, worden beschreven in deze blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ . Ervan uitgaande dat de twee virtuele machines die de configuratie van het Windows-Cluster voor ASCS/SCS bouwen die zich in hetzelfde subnet bevinden, moeten de volgende parameters worden gewijzigd in deze waarden:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Deze instellingen zijn getest met klanten en een goed compromis om robuuste op de een-zijde opgegeven. Aan de andere kant zijn deze instellingen voorzien in snel genoeg failover in reële fouten in SAP-software of knooppunt/VM-fout. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> SIOS DataKeeper Cluster Edition voor het hulpprogramma voor het delen van de SAP ASCS/SCS-clusterschijf installeren

U hebt nu een werkende configuratie van Windows Server Failover Clustering in Azure. Maar voor het installeren van een SAP ASCS/SCS-exemplaar, moet u een gedeelde schijf-resource. U kunt de gedeelde schijfresources, u moet maken in Azure. SIOS DataKeeper Cluster Edition is een oplossing van derden die u gebruiken kunt om resources van de gedeelde schijf te maken.

SIOS DataKeeper Cluster Edition voor het hulpprogramma voor het delen van de SAP ASCS/SCS-clusterschijf installeren, moet deze taken uitvoeren:

- Toevoegen van de .NET Framework 3.5
- SIOS DataKeeper installeren
- SIOS DataKeeper instellen

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> De .NET Framework 3.5 toevoegen
Microsoft .NET Framework 3.5 is niet automatisch geactiveerd of geïnstalleerd op Windows Server 2012 R2. Omdat SIOS DataKeeper .NET Framework vereist op alle knooppunten die u DataKeeper op installeert, moet u .NET Framework 3.5 installeren op het gastbesturingssysteem van alle virtuele machines in het cluster.

Er zijn twee manieren om toe te voegen van de .NET Framework 3.5:

- Gebruik de toevoegen Wizard functies en onderdelen in Windows zoals wordt weergegeven in afbeelding 39.

  ![Afbeelding 39: .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen][sap-ha-guide-figure-3028]

  _**Afbeelding 39:** .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen_

  ![Afbeelding 40: Voortgang van de installatie van de balk wanneer u de .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen][sap-ha-guide-figure-3029]

  _**Afbeelding 40:** Voortgang van de installatie van de balk wanneer u de .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen_

- Gebruik het opdrachtregelprogramma dism.exe. Voor dit type installatie moet u toegang tot de SxS-map op de installatiemedia van Windows. Typ bij een opdrachtprompt met verhoogde bevoegdheid:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper

SIOS DataKeeper Cluster Edition installeren op elk knooppunt in het cluster. Voor het maken van virtuele gedeelde opslag met SIOS DataKeeper, maakt een mirror gesynchroniseerd en vervolgens simuleren gedeelde clusteropslag.

Voordat u de SIOS software installeert, maakt u de domeingebruiker **DataKeeperSvc**.

> [!NOTE]
> Toevoegen de **DataKeeperSvc** gebruiker de **lokale beheerder** groep op beide knooppunten van het cluster.
>
>

SIOS DataKeeper installeren:

1.  De SIOS software installeren op beide knooppunten van het cluster.

  ![SIOS installatieprogramma][sap-ha-guide-figure-3030]

  ![Afbeelding 41: Eerste pagina van de SIOS DataKeeper-installatie][sap-ha-guide-figure-3031]

  _**Afbeelding 41:** Eerste pagina van de SIOS DataKeeper-installatie_

2.  Selecteer in het dialoogvenster wordt weergegeven in afbeelding 42 **Ja**.

  ![Afbeelding 42: DataKeeper wordt u geïnformeerd dat een service wordt uitgeschakeld][sap-ha-guide-figure-3032]

  _**Afbeelding 42:** DataKeeper wordt u geïnformeerd dat een service wordt uitgeschakeld_

3.  In het dialoogvenster wordt weergegeven in afbeelding 43, wordt aangeraden dat u selecteert **domein-of Server**.

  ![Afbeelding 43: Selectie van de gebruiker voor SIOS DataKeeper][sap-ha-guide-figure-3033]

  _**Afbeelding 43:** Selectie van de gebruiker voor SIOS DataKeeper_

4.  Voer de domein-gebruikersnaam en wachtwoorden op dat u hebt gemaakt voor SIOS DataKeeper.

  ![Afbeelding 44: De domeingebruikersnaam en wachtwoord invoeren voor de installatie van de SIOS DataKeeper][sap-ha-guide-figure-3034]

  _**Afbeelding 44:** De domeingebruikersnaam en wachtwoord invoeren voor de installatie van de SIOS DataKeeper_

5.  Installeer de licentiesleutel voor uw exemplaar SIOS DataKeeper zoals wordt weergegeven in afbeelding 45.

  ![Afbeelding 45: Voer uw sleutel SIOS DataKeeper-licentie][sap-ha-guide-figure-3035]

  _**Afbeelding 45:** Voer uw sleutel SIOS DataKeeper-licentie_

6.  Wanneer u hierom wordt gevraagd, start u de virtuele machine.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> SIOS DataKeeper instellen

Nadat u SIOS DataKeeper op beide knooppunten hebt geïnstalleerd, moet u de configuratie te starten. Het doel van de configuratie is dat synchrone gegevensreplicatie tussen de aanvullende VHD's die zijn gekoppeld aan elk van de virtuele machines.

1.  Start het configuratiehulpprogramma DataKeeper-beheer en de configuratie en selecteer vervolgens **verbinding maken met Server**. (In de afbeelding 46, deze optie is met een rode cirkel.)

  ![Afbeelding 46: SIOS DataKeeper beheer en de configuratie-hulpprogramma][sap-ha-guide-figure-3036]

  _**Afbeelding 46:** SIOS DataKeeper beheer en de configuratie-hulpprogramma_

2.  Voer de naam of de TCP/IP-adres van het eerste knooppunt dat het hulpprogramma voor beheer en de configuratie verbinding met, en, in een tweede stap van het tweede knooppunt maken moet.

  ![Afbeelding 47: De naam van de invoegen of TCP/IP-adres van het eerste knooppunt van het beheer en het hulpprogramma voor serverconfiguratie verbinding moet maken, en met een tweede stap van het tweede knooppunt][sap-ha-guide-figure-3037]

  _**Afbeelding 47:** De naam van de invoegen of TCP/IP-adres van het eerste knooppunt van het beheer en het hulpprogramma voor serverconfiguratie verbinding moet maken, en met een tweede stap van het tweede knooppunt_

3.  De replicatietaak tussen de twee knooppunten maken.

  ![Afbeelding 48: Een replicatietaak maken][sap-ha-guide-figure-3038]

  _**Afbeelding 48:** Een replicatietaak maken_

  Een wizard begeleidt u bij het proces voor het maken van een replicatietaak.
4.  De naam, de TCP/IP-adres en het schijfvolume van het bronknooppunt definiëren.

  ![Afbeelding 49: De naam van de replicatietaak definiëren][sap-ha-guide-figure-3039]

  _**Afbeelding 49:** De naam van de replicatietaak definiëren_

  ![Afbeelding 50: De basisgegevens van het knooppunt, het huidige knooppunt voor de gegevensbron moet definiëren][sap-ha-guide-figure-3040]

  _**Afbeelding 50:** De basisgegevens van het knooppunt, het huidige knooppunt voor de gegevensbron moet definiëren_

5.  Definieer de naam, de TCP/IP-adres en het schijfvolume van het doelknooppunt.

  ![Afbeelding 51: De basisgegevens van het knooppunt, het huidige doelknooppunt moet definiëren][sap-ha-guide-figure-3041]

  _**Afbeelding 51:** De basisgegevens van het knooppunt, het huidige doelknooppunt moet definiëren_

6.  Definieer de compressie-algoritmen. In ons voorbeeld is het raadzaam dat u de replicatie-stream comprimeren. Met name in situaties opnieuw synchroniseren minder de compressie van de stroom van de replicatie veel tijd opnieuw synchroniseren. Houd er rekening mee dat de resources CPU en RAM-geheugen van een virtuele machine maakt gebruik van compressie. Als de snelheid met compressie toeneemt, loopt u het volume van de CPU-resources die worden gebruikt. Ook kunt u deze instelling later opnieuw.

7.  Een andere instelling die u nodig hebt om te controleren is of de replicatie synchroon of asynchroon. *Wanneer u SAP ASCS/SCS-configuraties beveiligen, moet u synchrone replicatie*.  

  ![Afbeelding 52: Replicatiedetails definiëren][sap-ha-guide-figure-3042]

  _**Afbeelding 52:** Replicatiedetails definiëren_

8.  Definiëren of het volume dat door de replicatietaak voor wordt gerepliceerd naar een cluster Windows Server Failover Clustering configuratie als een gedeelde schijf moet worden weergegeven. Selecteer voor de configuratie SAP ASCS/SCS **Ja** zodat het Windows-cluster ziet de gerepliceerd volume als een gedeelde schijf die kan worden gebruikt als een clustervolume.

  ![Afbeelding 53: Selecteer Ja als u wilt het gerepliceerde volume instellen als een clustervolume van][sap-ha-guide-figure-3043]

  _**Afbeelding 53:** Selecteer **Ja** gerepliceerd volume instellen als een clustervolume_

  Nadat het volume is gemaakt, ziet u het hulpprogramma DataKeeper-beheer en de configuratie of de replicatietaak actief is.

  ![Afbeelding 54: Synchroon spiegelen van DataKeeper voor de schijf van de share SAP ASCS/SCS is actief][sap-ha-guide-figure-3044]

  _**Afbeelding 54:** Synchroon spiegelen van DataKeeper voor de schijf van de share SAP ASCS/SCS is actief_

  Failover Cluster Manager bevat nu de schijf als een schijf DataKeeper, zoals wordt weergegeven in afbeelding 55.

  ![Afbeelding 55: Failover-clusterbeheer ziet u de schijf dat DataKeeper gerepliceerd][sap-ha-guide-figure-3045]

  _**Afbeelding 55:** Failover-clusterbeheer ziet u de schijf dat DataKeeper gerepliceerd_

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Installatie van de SAP NetWeaver-systeem

De DBMS-installatie wordt niet beschreven omdat instellingen variëren, afhankelijk van de DBMS-systeem die u gebruikt. Echter, gaan we ervan uit dat problemen met de DBMS-systemen voor hoge beschikbaarheid met de functies van die de verschillende DBMS-leveranciers voor Azure Support worden behandeld. Bijvoorbeeld, Always On of database mirroring voor SQL Server en Oracle Data Guard voor Oracle-databases. In het scenario we in dit artikel gebruiken niet hebt we veel meer bescherming in het DBMS toevoegen.

Er zijn geen speciale overwegingen bij verschillende DBMS-services met dit type geclusterde SAP ASCS/SCS-configuratie in Azure communiceren.

> [!NOTE]
> De installatieprocedures van SAP NetWeaver ABAP-systemen, Java-systemen en systemen ABAP + Java zijn bijna identiek. Het belangrijkste verschil is dat een SAP ABAP-systeem één ASCS-exemplaar. Het SAP-Java-systeem heeft een SCS-exemplaar. Het systeem SAP ABAP + Java heeft één ASCS-exemplaar en een SCS-exemplaar die wordt uitgevoerd in de groep dezelfde Microsoft failover-cluster. Eventuele verschillen installatie voor elke installatie van SAP NetWeaver stack worden expliciet vermeld. U kunt ervan uitgaan dat alle andere onderdelen zijn hetzelfde.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> SAP installeren met een hoge beschikbaarheid ASCS/SCS-exemplaar

> [!IMPORTANT]
> Zorg ervoor dat u niet uw wisselbestand op DataKeeper gespiegelde volumes plaatsen. DataKeeper biedt geen ondersteuning voor gespiegelde volumes. U kunt uw wisselbestand op de tijdelijke station D van een Azure-machine laat de standaardinstelling. Als deze nog niet is ingevuld, verplaatst u het wisselbestand van Windows op station D van uw virtuele machine van Azure.
>
>

SAP met een hoge beschikbaarheid ASCS/SCS-exemplaar installeert, moet deze taken uitvoeren:

- Het maken van een virtuele host-naam voor de geclusterde SAP ASCS/SCS-exemplaar
- Het eerste clusterknooppunt van SAP installeren
- Wijzigen van het SAP-profiel van de ASCS/SCS-exemplaar
- Een testpoort toevoegen
- De testpoort van Windows firewall te openen

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> De naam van een virtuele host voor de geclusterde SAP ASCS/SCS-exemplaar maken

1.  Maak een DNS-vermelding voor de virtuele host-naam van de ASCS/SCS-exemplaar in de Windows DNS-beheer.

  > [!IMPORTANT]
  > Het IP-adres dat u aan de virtuele host-naam van de ASCS/SCS-exemplaar toewijst moet hetzelfde zijn als het IP-adres dat u hebt toegewezen aan Azure Load Balancer (**<*SID*> - lb - ascs**).  
  >
  >

  Het IP-adres van de virtuele SAP ASCS/SCS-hostnaam (**pr1-ascs-sap**) hetzelfde is als het IP-adres van Azure Load Balancer (**pr1-lb-ascs**).

  ![Afbeelding 56: De DNS-vermelding voor de virtuele naam van SAP ASCS/SCS cluster en de TCP/IP-adres definiëren][sap-ha-guide-figure-3046]

  _**Afbeelding 56:** De DNS-vermelding voor de virtuele naam van SAP ASCS/SCS cluster en de TCP/IP-adres definiëren_

2.  Voor het definiëren van het IP-adres toegewezen aan de virtuele host-naam, selecteer **DNS-beheer** > **domein**.

  ![Afbeelding 57: Nieuwe naam van de virtuele en TCP/IP-adres voor SAP ASCS/SCS-clusterconfiguratie][sap-ha-guide-figure-3047]

  _**Afbeelding 57:** Nieuwe naam van de virtuele en TCP/IP-adres voor SAP ASCS/SCS-clusterconfiguratie_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Het eerste clusterknooppunt van SAP installeren

1.  Uitvoeren van de eerste optie van cluster-knooppunt op clusterknooppunt A. Bijvoorbeeld: op de **pr1-ascs-0** host.
2.  Om te voorkomen dat de standaard-poorten voor de interne Azure load balancer, selecteren:

  * **ABAP system**: **ASCS** exemplaarnummer **00**
  * **Java-systeem**: **SCS** exemplaarnummer **01**
  * **ABAP + Java system**: **ASCS** exemplaarnummer **00** en **SCS** exemplaarnummer **01**

  Als u wilt exemplaar getallen dan 00 voor de ABAP ASCS-exemplaar en 01 voor de Java-SCS-exemplaar gebruikt, moet u eerst wijzigen van de interne Azure load balancer standaard load balancer-regels, die worden beschreven in [de ASCS/SCS standaard load balancer-regels voor wijzigen de interne Azure load balancer][sap-ha-guide-8.9].

De volgende paar taken worden niet beschreven in de documentatie van de standaard SAP-installatie.

> [!NOTE]
> De SAP-installatie-documentatie wordt beschreven hoe u het eerste ASCS/SCS-clusterknooppunt installeren.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Wijzig het SAP-profiel van de ASCS/SCS-exemplaar

U moet een nieuwe profiel-parameter. De profiel-parameter voorkomt dat verbindingen tussen SAP processen en de server in de wachtrij plaatsen af te sluiten wanneer ze niet actief is te lang zijn. We het probleem scenario in genoemde [registervermeldingen op beide clusterknooppunten van de SAP ASCS/SCS-exemplaar toe te voegen][sap-ha-guide-8.11]. In deze sectie hebben we ook geïntroduceerd twee wijzigingen aan enkele eenvoudige parameters van de TCP/IP-verbinding. In een tweede stap, moet u om in te stellen van de server in de wachtrij plaatsen voor het verzenden van een `keep_alive` geven zodat de verbindingen niet van de interne Azure load balancer niet-actieve drempelwaarde bereikt.

De SAP-profiel van de ASCS/SCS-exemplaar wijzigen:

1.  Deze parameter profiel toevoegen aan het profiel SAP ASCS/SCS-exemplaar:

  ```
  enque/encni/set_so_keepalive = true
  ```
  In ons voorbeeld is het pad:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Bijvoorbeeld, voor de SAP-SCS-exemplaar profiel en het bijbehorende pad:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  De wijzigingen wilt toepassen, door het SAP ASCS /SCS-exemplaar opnieuw te starten.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Een testpoort toevoegen

De interne load balancer-test-functionaliteit gebruiken om de configuratie van het hele cluster werken met Azure Load Balancer. De interne Azure load balancer verdeelt meestal de inkomende belasting gelijk verdelen tussen deelnemende virtuele machines. Echter, dat werkt niet in sommige configuraties van clusters op omdat er slechts één exemplaar actief is. Het andere exemplaar is passieve en een van de werkbelasting kan accepteren. Een test-functionaliteit helpt bij de interne Azure load balancer werken alleen met een actief exemplaar wijst. De interne load balancer kan met de functionaliteit test detecteren welke instanties actief zijn en vervolgens de doelinstantie van alleen de met de werkbelasting.

Een testpoort toevoegen:

1.  Controleer de huidige **ProbePort** instellen door het uitvoeren van de volgende PowerShell-opdracht. Deze binnen een van de virtuele machines worden uitgevoerd in de configuratie van het cluster.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  Een testpoort definiëren. Het standaardpoortnummer voor de test is **0**. In ons voorbeeld gebruiken we de testpoort **62000**.

  ![Afbeelding 58: De testpoort van de cluster-configuratie is 0 standaard][sap-ha-guide-figure-3048]

  _**Afbeelding 58:** De standaardpoort cluster configuration test is 0_

  Het poortnummer dat is gedefinieerd in SAP Azure Resource Manager-sjablonen. U kunt het poortnummer in PowerShell.

  Om in te stellen van een nieuwe ProbePort-waarde voor de **SAP <*SID*> IP** clusterbron, voert u de volgende PowerShell-script. De PowerShell-variabelen voor uw omgeving bijwerken. Nadat het script wordt uitgevoerd, wordt u gevraagd om opnieuw te starten van de SAP-clustergroep voor het activeren van de wijzigingen.

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  Clear-Host
  $SAPClusterRoleName = "SAP $SAPSID"
  $SAPIPresourceName = "SAP $SAPSID IP"
  $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
  $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
  $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
  $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
  $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
  $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
  $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

  $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

  Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

  Write-Host
  Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

  if($ActivateChanges -eq "yes"){
  Write-Host
  Write-Host "Activating changes..." -ForegroundColor Cyan

  Write-Host
  write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
  Stop-ClusterResource -Name $SAPIPresourceName
  sleep 5

  Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
  Start-ClusterGroup -Name $SAPClusterRoleName

  Write-Host "New ProbePort parameter is active." -ForegroundColor Green
  Write-Host

  Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
  Write-Host
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
  }else
  {
  Write-Host "Changes are not activated."
  }
  ```

  Nadat u de **SAP <*SID*>** cluster online rol, Controleer **ProbePort** is ingesteld op de nieuwe waarde.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Afbeelding 59: De cluster testpoort na het instellen van de nieuwe waarde][sap-ha-guide-figure-3049]

  _**Afbeelding 59:** De cluster testpoort na het instellen van de nieuwe waarde_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open de Windows firewall-test-poort

U moet een testpoort van Windows firewall op beide clusterknooppunten te openen. Gebruik het volgende script om een Windows firewall-testpoort te openen. De PowerShell-variabelen voor uw omgeving bijwerken.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

De **ProbePort** is ingesteld op **62000**. Nu u toegang heeft tot de bestandsshare  **\\\ascsha-clsap\sapmnt** met andere hosts, zoals van **ascsha DBA**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Het database-exemplaar installeren

Volg de procedure beschreven in de documentatie van SAP-installatie voor het installeren van de database-instantie.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Het tweede clusterknooppunt installeren

Voor het installeren van het tweede cluster, volg de stappen in de SAP-installatiehandleiding.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Wijzig het starttype van de SAP INGEN Windows service-exemplaar

Wijzig het starttype van de SAP INGEN Windows-service naar **automatisch (vertraagd starten)** op beide knooppunten van het cluster.

![Afbeelding 60: Wijzig het type van de service voor het exemplaar van de SAP-klanten in vertraagd automatisch][sap-ha-guide-figure-3050]

_**Afbeelding 60:** Wijzig het type van de service voor het exemplaar van de SAP-klanten in vertraagd automatisch_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> De primaire SAP-toepassingsserver installeren

Het exemplaar van de primaire toepassing Server (PAS) installeren <*SID*> - di - 0 op de virtuele machine die u hebt opgegeven voor het hosten van de Pa's. Er zijn geen afhankelijkheden op Azure of DataKeeper-specifieke instellingen.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> De aanvullende SAP-toepassingsserver installeren

Installeer een SAP extra Application Server (AAS) op alle virtuele machines die u hebt opgegeven voor het hosten van een exemplaar van de SAP-toepassingsserver. Druk bijvoorbeeld in <*SID*> - di - 1 naar <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> De installatie van een hoge beschikbaarheid SAP NetWeaver-systeem wordt voltooid. Vervolgens gaat u verder met het testen van failover.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> De failover-SAP ASCS/SCS-exemplaar en SIOS replicatie testen
Het is gemakkelijk om te testen en een failover-SAP ASCS/SCS-exemplaar en SIOS schijfreplicatie bewaken met behulp van Failoverclusterbeheer en de SIOS DataKeeper Management en configuratie-hulpprogramma.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS-exemplaar wordt uitgevoerd op een clusterknooppunt A

De **SAP PR1** clustergroep wordt uitgevoerd op een clusterknooppunt A. Bijvoorbeeld: op **pr1-ascs-0**. Toewijzen van de gedeelde schijf S, die deel uitmaakt van de **SAP PR1** clustergroep, en die in de ASCS/SCS-exemplaar gebruikt, om de cluster-knooppunt A.

![Afbeelding 61: Failover Cluster Manager: De clustergroep SAP < SID > wordt uitgevoerd op een clusterknooppunt A][sap-ha-guide-figure-5000]

_**Afbeelding 61:** Failover Cluster Manager: De SAP <*SID*> clustergroep wordt uitgevoerd op een clusterknooppunt A_

In het hulpprogramma SIOS DataKeeper beheer en de configuratie kunt u zien dat de gegevens van de gedeelde schijf is asynchroon worden gerepliceerd vanaf de bron volume station S op clusterknooppunt A naar het doel volume station S op clusterknooppunt B. Bijvoorbeeld: deze worden gerepliceerd van **pr1-ascs-0 [10.0.0.40]** naar **pr1-ascs-1 [10.0.0.41]**.

![Afbeelding 62: Repliceren in SIOS DataKeeper, het lokale volume van clusterknooppunt A naar B-clusterknooppunt][sap-ha-guide-figure-5001]

_**Afbeelding 62:** Repliceren in SIOS DataKeeper, het lokale volume van clusterknooppunt A naar B-clusterknooppunt_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover van knooppunt A naar B-knooppunt

1.  Kies een van deze opties voor het initiëren van een failover van de SAP <*SID*> clustergroep van het clusterknooppunt een clusterknooppunt B:
  - Gebruik Failoverclusterbeheer  
  - Failover-Cluster PowerShell gebruiken

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Start opnieuw op een van de cluster-knooppunt in de Windows-gastbesturingssysteem (Hiermee initieert een automatische failover van de SAP <*SID*> clustergroep van knooppunt A naar B knooppunt).  
3.  Start opnieuw op een van de cluster-knooppunt vanuit Azure portal (Hiermee initieert een automatische failover van de SAP <*SID*> clustergroep van knooppunt A naar B knooppunt).  
4.  Cluster-knooppunt A opnieuw met behulp van Azure PowerShell (Hiermee initieert een automatische failover van de SAP <*SID*> clustergroep van knooppunt A naar B knooppunt).

  Na een failover van de SAP <*SID*> clustergroep wordt uitgevoerd op een clusterknooppunt B. Bijvoorbeeld, deze wordt uitgevoerd op **pr1-ascs-1**.

  ![Afbeelding 63: In Failoverclusterbeheer de clustergroep SAP < SID > wordt uitgevoerd op clusterknooppunt B][sap-ha-guide-figure-5002]

  _**Afbeelding 63**: In Failoverclusterbeheer, de SAP <*SID*> clustergroep wordt uitgevoerd op een clusterknooppunt B_

  De gedeelde schijf is nu gekoppeld op cluster knooppunt B. SIOS DataKeeper is die gegevens van de bron volume station S op clusterknooppunt B naar volume doelstation S op clusterknooppunt A. Bijvoorbeeld, deze wordt gerepliceerd van **pr1-ascs-1 [10.0.0.41]** naar **pr1-ascs-0 [10.0.0.40]**.

  ![Afbeelding 64: SIOS DataKeeper worden gerepliceerd van het lokale volume van het clusterknooppunt B aan het cluster een knooppunt][sap-ha-guide-figure-5003]

  _**Afbeelding 64:** SIOS DataKeeper worden gerepliceerd van het lokale volume van het clusterknooppunt B aan het cluster een knooppunt_
