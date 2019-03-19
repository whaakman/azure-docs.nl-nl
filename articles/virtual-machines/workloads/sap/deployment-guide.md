---
title: Azure Virtual Machines-implementatie voor SAP NetWeaver | Microsoft Docs
description: Informatie over het implementeren van SAP-software op Linux-machines in Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: sedusch
ms.openlocfilehash: 2d296281f6865030bcdfec33d8c69cc313a358a5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58011905"
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>Azure Virtual Machines-implementatie voor SAP NetWeaver

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
[azure-cli-2]:https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (Azure virtuele Machines DBMS-implementatie voor SAP)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (In cache opslaan voor virtuele machines en VHD 's)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software-RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structuur van een RDBMS-implementatie)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (Hoge beschikbaarheid en herstel na noodgevallen met Azure-VM 's)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 of hoger)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 en oudere releases)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Met behulp van een installatiekopie van SQL Server op Azure Marketplace)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Algemene SQL Server voor SAP op Azure samenvatting)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Specifieke informatie naar SQL Server RDBMS)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Opslagconfiguratie)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Back-up en herstel)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Prestatie-overwegingen voor back-up en herstel)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Andere)
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

[deployment-guide]:deployment-guide.md (Azure Virtual Machines-implementatie voor SAP)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resources)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Een virtuele machine implementeren met behulp van een aangepaste installatiekopie)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Een virtuele machine in Azure Marketplace implementeren voor SAP)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Een virtuele machine met een aangepaste installatiekopie implementeren voor SAP)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Een virtuele machine verplaatsen van on-premises met behulp van een Azure niet gegeneraliseerde VHD met SAP)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Scenario's voor implementatie van virtuele machines voor SAP op Microsoft Azure)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Implementatie van Azure PowerShell-cmdlets)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Downloaden en importeren van SAP-relevante PowerShell-cmdlets)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Een virtuele machine toevoegen aan een on-premises domein - alleen Windows)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Downloaden, installeren en de Azure VM-Agent inschakelen)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Configureren van de Azure uitgebreide controle-extensie voor SAP)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (De gereedheidscontrole voor Azure uitgebreide bewaking voor SAP)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Statuscontrole voor de Azure monitoring-infrastructuur)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Oplossen van problemen met Azure monitoring voor SAP)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Bewaking configureren)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configureer de proxy)
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Controles en probleemoplossing voor end-to-end controle instellen)

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

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

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmaemextension

[planning-guide]:planning-guide.md (Azure virtuele Machines, planning en implementatie van SAP)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (Hoge beschikbaarheid en herstel na noodgevallen voor SAP NetWeaver op Azure Virtual machines)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (Hoge beschikbaarheid voor SAP-toepassingsservers)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Met behulp van automatisch starten voor SAP-exemplaren)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Alleen cloud - implementaties van virtuele machines in Azure zonder afhankelijkheden van de klant on-premises netwerk)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - implementatie van één of meerdere SAP-virtuele machines in Azure is volledig geïntegreerd met de on-premises netwerk)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure-regio 's)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Domeinen met fouten)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade-domeinen)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure-beschikbaarheidssets)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtual machines concept)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Een virtuele machine met een specifieke klant-installatiekopie implementeren)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Voorbereiding voor een virtuele machine verplaatsen van on-premises naar Azure met een schijf niet gegeneraliseerd)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Voorbereiding voor het implementeren van een virtuele machine met een specifieke installatiekopie van de klant voor SAP)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Virtuele machines met SAP voorbereiden voor Azure)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Verschil tussen een Azure-schijf en een afbeelding van Azure)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Uploaden van een VHD van on-premises naar Azure)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Kopiëren van schijven tussen Azure Storage-accounts)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD-structuur voor SAP-implementaties)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Automount voor gekoppelde schijven instellen)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Één virtuele machine met SAP NetWeaver demo/training scenario)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepten van Cloud-Only-implementatie van SAP-instanties)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure-oplossing voor de controle voor SAP)
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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure-netwerken)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Opslag: Microsoft Azure Storage- en gegevensschijven)

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../networking/network-overview.md
[sap-pam]:https://support.sap.com/pam (SAP-Product beschikbaarheid Matrix)
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
[storage-premium-storage-preview-portal]:../../windows/disks-types.md
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
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Virtuele machines beheren met behulp van Azure Resource Manager en PowerShell)
[virtual-machines-windows-agent-user-guide]:../../extensions/agent-windows.md
[virtual-machines-linux-agent-user-guide]:../../extensions/agent-linux.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../extensions/agent-linux.md#command-line-options
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

Virtuele Machines van Azure is de oplossing voor organisaties die reken- en opslagresources, in een minimale tijd en zonder langdurige cycli moeten. Azure Virtual Machines kunt u klassieke toepassingen, zoals SAP NetWeaver gebaseerde toepassingen in Azure implementeren. Breid de betrouwbaarheid en beschikbaarheid zonder extra on-premises resources van een toepassing. Virtuele Machines van Azure biedt ondersteuning voor cross-premises-connectiviteit, zodat u Azure Virtual Machines kunt integreren in uw organisatie on-premises domeinen, privéclouds en SAP-landschap dat systeem.

In dit artikel gaan we de stappen voor het implementeren van SAP-toepassingen op virtuele machines (VM's) in Azure, met inbegrip van alternatieve implementatie-opties en probleemoplossing. Dit artikel is gebaseerd op de informatie in [Azure Virtual Machines planning en implementatie van SAP NetWeaver][planning-guide]. Deze ook is een aanvulling op documentatie voor installatie van SAP en SAP-opmerkingen, de primaire bronnen zijn voor het installeren en implementeren van SAP-software.

## <a name="prerequisites"></a>Vereisten

Instellen van een virtuele machine van Azure voor SAP-software-implementatie bestaat uit meerdere stappen en bronnen. Voordat u begint, controleert u dat u voldoet aan de vereisten voor het SAP-software installeren op virtuele machines in Azure.

### <a name="local-computer"></a>Lokale computer

Voor het beheren van Windows of Linux-machines, kunt u een PowerShell-script en de Azure-portal. Voor beide hulpprogramma's moet u een lokale computer met Windows 7 of een latere versie van Windows. Als u wilt beheren alleen Linux-VM's en u wilt een Linux-computer voor deze taak gebruiken, kunt u Azure CLI.

### <a name="internet-connection"></a>Verbinding met Internet

Als u wilt downloaden en uitvoeren van de hulpprogramma's en scripts die vereist voor SAP-software-implementatie zijn, moet u verbonden met Internet. De Azure-VM met de Azure uitgebreide controle-extensie voor SAP moet ook toegang tot Internet. Als de virtuele Azure-machine deel uit van een virtueel Azure-netwerk of een on-premises domein maakt, zorg ervoor dat de relevante proxy-instellingen zijn ingesteld, zoals beschreven in [configureert de proxy][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure-abonnement

U moet een actief Azure-account.

### <a name="topology-and-networking"></a>Topologie en netwerken

U moet voor het definiëren van de topologie en de architectuur van de SAP-implementatie in Azure:

* Azure storage-accounts moet worden gebruikt
* Virtueel netwerk waar u de SAP-systeem implementeren
* Resourcegroep die u wilt de SAP-systeem implementeren
* Azure-regio waar u de SAP-systeem implementeren
* SAP-configuratie (twee lagen of drie lagen)
* VM-grootten en het aantal extra gegevensschijven worden gekoppeld aan de virtuele machines
* Configuratie van SAP correctie en Transport System (CTS)

Maken en configureren van Azure storage-accounts (indien nodig) of virtuele netwerken van Azure voordat u begint met het implementatieproces van de SAP-software. Zie voor meer informatie over het maken en configureren van deze resources [Azure Virtual Machines planning en implementatie van SAP NetWeaver][planning-guide].

### <a name="sap-sizing"></a>SAP-schaling

Kent de volgende informatie, de grootte van de SAP instelt:

* Bijvoorbeeld, verwachte SAP-workloads, met behulp van het hulpprogramma snel SAP-Sizer en de SAP-toepassing prestaties Standard (SAP's) getal
* Vereiste CPU-resources en het geheugen verbruik van de SAP-systeem
* Vereiste invoer/uitvoer (I/O)-bewerkingen per seconde
* Vereiste netwerkbandbreedte van uiteindelijke communicatie tussen virtuele machines in Azure
* Vereiste netwerkbandbreedte tussen on-premises assets en de Azure-geïmplementeerd SAP-systeem

### <a name="resource-groups"></a>Resourcegroepen

In Azure Resource Manager, u kunt resourcegroepen gebruiken voor het beheren van alle toepassingsresources in uw Azure-abonnement. Zie voor meer informatie, [overzicht van Azure Resource Manager][resource-group-overview].

## <a name="resources"></a>Resources

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-resources

Wanneer u uw SAP-software-implementatie instelt, moet u de volgende bronnen voor SAP:

* SAP-notitie [1928533], heeft:
  * Lijst met Azure-VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van database
  * Vereiste kernel van SAP-versie voor Windows en Linux op Microsoft Azure

* SAP-notitie [2015553] bevat vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2178632] vindt u meer informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [1409604] heeft de vereiste versie van de SAP-Host-Agent voor Windows in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1984787] bevat algemene informatie over het SUSE Linux Enterprise Server 12.
* SAP-notitie [2002167] bevat algemene informatie over Red Hat Enterprise Linux 7.x.
* SAP-notitie [2069760] bevat algemene informatie over Oracle Linux 7.x.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* SAP-notitie [1597355] bevat algemene informatie over wisselruimte voor Linux.
* [SAP op Azure SCN pagina](https://wiki.scn.sap.com/wiki/x/Pia7Gg) nieuws en een verzameling van nuttige bronnen heeft.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) bevat alle SAP-opmerkingen voor Linux vereist.
* SAP-specifieke PowerShell-cmdlets die deel van uitmaken [Azure PowerShell][azure-ps].
* SAP-specifieke Azure CLI-opdrachten die deel van uitmaken [Azure CLI][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows-resources

Deze artikelen Microsoft hebben betrekking op SAP-oplossingen in Azure:

* [Azure virtuele Machines, planning en implementatie van SAP NetWeaver][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP NetWeaver (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Implementatiescenario's voor SAP-software op Azure Virtual machines

U hebt meerdere opties voor het implementeren van VM's en bijbehorende schijven in Azure. Het is belangrijk te weten wat de verschillen tussen de opties voor implementatie, omdat u mogelijk verschillende stappen voor het voorbereiden van uw VM's voor implementatie op basis van het implementatietype dat u kiest.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Een virtuele machine in Azure Marketplace implementeren voor SAP

U kunt een door Microsoft of door een derde partij in de Azure Marketplace-installatiekopie gebruiken om uw VM te implementeren. De Marketplace biedt enkele standard installatiekopieën van het besturingssysteem van Windows Server en andere Linux-distributies. U kunt een installatiekopie met database management ook implementeren system (DBMS) SKU's, bijvoorbeeld Microsoft SQL Server. Zie voor meer informatie over het gebruik van installatiekopieën met DBMS-SKU's [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide].

Het volgende stroomdiagram ziet u de SAP-specifieke volgorde van stappen voor het implementeren van een virtuele machine van Azure Marketplace:

![Stroomdiagram van VM-implementatie voor SAP-systemen met behulp van een VM-installatiekopie uit de Azure Marketplace][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Een virtuele machine maken met behulp van Azure portal

De eenvoudigste manier om een nieuwe virtuele machine maken met een installatiekopie van de Azure Marketplace is met behulp van de Azure-portal.

1.  Ga naar <https://portal.azure.com/#create/hub>.  Of Selecteer in het menu van Azure portal **+ nieuw**.
1.  Selecteer **Compute**, en selecteer vervolgens het type besturingssysteem u wilt implementeren. Bijvoorbeeld, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) of Oracle Linux 7.2. De standaardweergave van de lijst wordt niet weergegeven voor dat alle ondersteunde besturingssystemen. Selecteer **alle** voor een volledige lijst. Zie voor meer informatie over ondersteunde besturingssystemen voor SAP-software-implementatie, SAP-notitie [1928533].
1.  Controleer voorwaarden en bepalingen op de volgende pagina.
1.  In de **een implementatiemodel selecteren** Selecteer **Resource Manager**.
1.  Selecteer **Maken**.

De wizard begeleidt u bij het instellen van de vereiste parameters voor het maken van de virtuele machine, naast alle vereiste resources, zoals netwerkinterfaces en storage-accounts. Sommige van deze parameters zijn:

1. **Grondbeginselen van**:
   * **Naam**: De naam van de resource (de naam van de virtuele machine).
   * **Type VM-schijf**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruikmaken van Premium Storage voor de gegevensschijven van uw wilt, raden wij met Premium Storage voor de OS-schijf.
   * **Gebruikersnaam en wachtwoord** of **openbare SSH-sleutel**: Voer de gebruikersnaam en wachtwoord van de gebruiker die is gemaakt tijdens het inrichten. Voor een virtuele Linux-machine, kunt u de openbare sleutel voor Secure Shell (SSH) waarmee u zich aanmeldt bij de machine.
   * **Abonnement**: Selecteer het abonnement dat u gebruiken wilt voor het inrichten van de nieuwe virtuele machine.
   * **Resourcegroep**: De naam van de resourcegroep voor de virtuele machine. U kunt u de naam van een nieuwe resourcegroep of de naam van een resourcegroep die al bestaat.
   * **Locatie**: Waar u de nieuwe virtuele machine implementeren. Als u wilt dat de virtuele machine verbinden met uw on-premises netwerk, zorg ervoor dat u de locatie van het virtuele netwerk dat maakt Azure verbinding met uw on-premises netwerk selecteert. Zie voor meer informatie, [Microsoft Azure-netwerken] [ planning-guide-microsoft-azure-networking] in [Azure Virtual Machines planning en implementatie van SAP NetWeaver] [ planning-guide].
1. **Grootte**:

     Zie voor een lijst van ondersteunde VM-typen, SAP-notitie [1928533]. Zorg ervoor dat u het juiste VM-type selecteren als u wilt gebruiken van Azure Premium Storage. Niet alle VM-typen bieden ondersteuning voor Premium-opslag. Zie voor meer informatie, [opslag: Microsoft Azure Storage- en gegevensschijven] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] en [Azure Premium Storage] [ planning-guide-azure-premium-storage] in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide].

1. **Instellingen voor**:
   * **Storage**
     * **Schijftype**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruikmaken van Premium Storage voor de gegevensschijven van uw wilt, raden wij met Premium Storage voor de OS-schijf.
     * **Beheerde schijven gebruiken**: Selecteer Ja als u gebruikmaken van beheerde schijven wilt. Zie voor meer informatie over Managed Disks, hoofdstuk [Managed Disks] [ planning-guide-managed-disks] in de handleiding voor capaciteitsplanning.
     * **Opslagaccount**: Selecteer een bestaand opslagaccount of maak een nieuwe. Niet alle opslagtypen werken voor het uitvoeren van SAP-toepassingen. Zie voor meer informatie over opslagtypen [opslagstructuur van een virtuele machine voor RDBMS implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64).
   * **Netwerk**
     * **Virtueel netwerk** en **Subnet**: Voor het integreren van de virtuele machine met het intranet, selecteer het virtuele netwerk dat is verbonden met uw on-premises netwerk.
     * **Openbaar IP-adres**: Selecteer het openbare IP-adres dat u wilt gebruiken, of parameters voor het maken van een nieuw openbaar IP-adres invoeren. U kunt een openbaar IP-adres gebruiken voor toegang tot uw virtuele machine via Internet. Zorg ervoor dat u ook een netwerkbeveiligingsgroep voor beveiligde toegang tot uw virtuele machine te maken.
     * **Netwerkbeveiligingsgroep**: Zie voor meer informatie, [netwerkverkeer met netwerkbeveiligingsgroepen beheren][virtual-networks-nsg].
   * **Extensies**: U kunt extensies voor virtuele machines installeren door ze toe te voegen aan de implementatie. U hoeft geen uitbreidingen toevoegen in deze stap. De extensies vereist voor ondersteuning van SAP zijn later geïnstalleerd. Zie hoofdstuk [configureren van de Azure uitgebreide controle-extensie voor SAP] [ deployment-guide-4.5] in deze handleiding.
   * **Hoge beschikbaarheid**: Selecteer een beschikbaarheidsset of geef de parameters voor het maken van een nieuwe beschikbaarheidsset. Zie voor meer informatie, [Azure-beschikbaarheidssets][planning-guide-3.2.3].
   * **Controle**
     * **Diagnostische gegevens over opstarten**: U kunt selecteren **uitschakelen** voor diagnostische gegevens over opstarten.
     * **Diagnostische gegevens van Guest OS**: U kunt selecteren **uitschakelen** voor het bewaken van diagnostische gegevens.

1. **Summary**:

   Controleer uw selecties en selecteer vervolgens **OK**.

Uw virtuele machine wordt geïmplementeerd in de resourcegroep die u hebt geselecteerd.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Een virtuele machine maken met behulp van een sjabloon

U kunt een virtuele machine maken met behulp van een van de SAP-sjablonen gepubliceerd in de [GitHub-opslagplaats voor azure-quickstart-templates][azure-quickstart-templates-github]. U kunt ook handmatig maken een virtuele machine met behulp van de [Azure-portal][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], of [Azure CLI] [virtual-machines-linux-tutorial].

* [**Met twee lagen (slechts één virtuele machine)-configuratiesjabloon** (sap-2-laag-marketplace-installatiekopie)][sap-templates-2-tier-marketplace-image]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine, moet u deze sjabloon gebruiken.
* [**Met twee lagen (slechts één virtuele machine)-configuratiesjabloon: Managed Disks** (sap-2-tier-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine en Managed Disks, moet u deze sjabloon gebruiken.
* [**Drie lagen configuratiesjabloon (meerdere virtuele machines)** (sap-3-tier-marketplace-installatiekopie)][sap-templates-3-tier-marketplace-image]

  Voor het maken van een systeem met drie lagen met behulp van meerdere virtuele machines, moet u deze sjabloon gebruiken.
* [**Van de drie lagen (meerdere virtuele machines)-configuratiesjabloon: Managed Disks** (sap-3-tier-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  Voor het maken van een systeem met drie lagen met behulp van meerdere virtuele machines en Managed Disks, moet u deze sjabloon gebruiken.

Voer de volgende parameters voor de sjabloon in de Azure-portal:

1. **Grondbeginselen van**:
   * **Abonnement**: Het abonnement te gebruiken om de sjabloon te implementeren.
   * **Resourcegroep**: De resourcegroep te gebruiken om de sjabloon te implementeren. U kunt een nieuwe resourcegroep maken of u kunt een bestaande resourcegroep selecteren in het abonnement.
   * **Locatie**: Waar om de sjabloon te implementeren. Als u een bestaande resourcegroep hebt geselecteerd, wordt de locatie van deze resourcegroep wordt gebruikt.

1. **Instellingen voor**:
   * **De ID van de SAP-systeem**: De SAP-systeem-ID (SID).
   * **Type besturingssysteem**: Het besturingssysteem die u implementeren wilt, bijvoorbeeld, Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) of Oracle Linux 7.2.

     De lijstweergave wordt niet weergegeven voor dat alle ondersteunde besturingssystemen. Zie voor meer informatie over ondersteunde besturingssystemen voor SAP-software-implementatie, SAP-notitie [1928533].
   * **Grootte van het SAP**: De grootte van de SAP-systeem.

     Het aantal SAP's vindt u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
   * **Beschikbaarheid van het systeem** (alleen in drie lagen sjabloon): De beschikbaarheid van het systeem.

     Selecteer **HA** voor een configuratie die geschikt is voor de installatie van een hoge beschikbaarheid. Twee databaseservers en twee servers voor ABAP SAP Central Services (ASCS) worden gemaakt.
   * **Opslagtype** (alleen in twee lagen sjabloon): Het type opslag te gebruiken.

     Voor grotere systemen raden wij met behulp van Azure Premium Storage. Zie de volgende bronnen voor meer informatie over soorten opslag:
      * [Gebruik van Azure Premium-SSD-opslag voor SAP DBMS-instantie][2367194]
      * [Opslagstructuur van een virtuele machine voor RDBMS-implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Storage: Opslag met hoge prestaties voor werkbelastingen van de virtuele Machine van Azure][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
   * **Gebruikersnaam van beheerder** en **beheerderswachtwoord**: Een gebruikersnaam en wachtwoord.
     Een nieuwe gebruiker is gemaakt, voor aanmelding bij de virtuele machine.
   * **Nieuw of bestaand subnet**: Bepaalt of een nieuw virtueel netwerk en subnet worden gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
   * **Subnet-ID**: Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID meestal er als volgt uitzien: /subscriptions/&lt;abonnements-id > /resourceGroups/&lt;groepsnaam voor accountresources > /providers/Microsoft.Network/virtualNetworks/&lt;virtuele-netwerknaam > /subnets/&lt;subnetnaam >

1. **Voorwaarden en bepalingen**:  
    Lees en accepteer de juridische bepalingen.

1. Selecteer **Aankoop**.

De Azure VM-Agent is standaard geïmplementeerd wanneer u een installatiekopie van de Azure Marketplace.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Afhankelijk van hoe uw on-premises-netwerk is geconfigureerd, moet u mogelijk de proxy op de virtuele machine niet instellen. Als uw virtuele machine is verbonden met uw on-premises netwerk via VPN of ExpressRoute, de virtuele machine mogelijk geen toegang tot Internet en is niet mogelijk om te downloaden van de vereiste extensies of verzamelen van gegevens. Zie voor meer informatie, [configureert de proxy][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>Toevoegen aan een domein (alleen Windows)

Als uw Azure-implementatie is verbonden met een on-premises Active Directory of DNS-exemplaar via een Azure-site-naar-site VPN-verbinding of ExpressRoute (dit heet *cross-premises* in [Azure Virtual Machines plannen en implementatie van SAP NetWeaver][planning-guide]), is het waarschijnlijk dat de virtuele machine lid van een on-premises domein worden. Zie voor meer informatie over overwegingen voor deze taak [een VM toevoegen aan een on-premises domein (alleen Windows)][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Bewaking configureren

Om er zeker van te zijn dat SAP biedt ondersteuning voor uw omgeving, instellen van de Azure Monitoring-extensie voor SAP, zoals beschreven in [configureren van de Azure uitgebreide controle-extensie voor SAP][deployment-guide-4.5]. Controleer de vereisten voor het bewaken van SAP en vereist minimaal vereiste versies van de Kernel van SAP en SAP Host-Agent in de resources die worden vermeld in [SAP resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Bewaking van selectievakje

Controleer of controle werkt, zoals beschreven in [controles en probleemoplossing voor het instellen van de end-to-end bewaking][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Stappen na de implementatie

Nadat u de virtuele machine maken en de virtuele machine is geïmplementeerd, moet u de vereiste software-onderdelen installeren op de virtuele machine. Vanwege de volgorde van de installatie van software voor implementatie/in dit type VM-implementatie moet de software worden geïnstalleerd al beschikbaar zijn, in Azure, op een andere virtuele machine of als een schijf die kan worden gekoppeld. Of Overweeg het gebruik van een scenario voor cross-premises, in welke verbinding met de on-premises activa (installatie-shares) is opgegeven.

Nadat u de virtuele machine in Azure implementeert, volgt u de dezelfde richtlijnen en hulpprogramma's voor het installeren van de SAP-software op de virtuele machine zoals u zou in een on-premises omgeving doen. Raadzaam dat u uploaden en opslaan van het installatiemedium van SAP op Azure-VHD's of beheerde schijven, of dat u een Azure-VM die maakt fungeert als een bestandsserver met alle vereiste SAP-installatiemedia voor het SAP-software installeren op een Azure-VM, zowel Microsoft als SAP.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Een virtuele machine met een aangepaste installatiekopie implementeren voor SAP

Omdat verschillende versies van een besturingssysteem of DBMS patch voor verschillende vereisten hebben, kunnen de installatiekopieën die u in de Azure Marketplace vinden niet voldoen aan uw behoeften. Mogelijk wilt u in plaats daarvan een virtuele machine maken met behulp van uw eigen OS/DBMS-VM-installatiekopie, die u het later opnieuw kunt implementeren.
U verschillende stappen gebruiken om een persoonlijke installatiekopie voor Linux dan voor het maken van een voor Windows te maken.

- - -
> ![Windows][Logo_Windows] Windows
>
> Om voor te bereiden op een Windows-installatiekopie die u gebruiken kunt om meerdere virtuele machines te implementeren, moeten de Windows-instellingen (zoals Windows SID en hostnaam) worden geabstraheerd of op de on-premises VM is gegeneraliseerd. U kunt [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) om dit te doen.
>
> ![Linux][Logo_Linux] Linux
>
> Om voor te bereiden op een Linux-installatiekopie die u gebruiken kunt om meerdere virtuele machines te implementeren, moeten sommige Linux-instellingen worden geabstraheerd of op de on-premises VM is gegeneraliseerd. U kunt `waagent -deprovision` om dit te doen. Zie voor meer informatie, [vastleggen van een virtuele Linux-machine die wordt uitgevoerd op Azure] [ virtual-machines-linux-capture-image] en de [gebruikershandleiding voor Azure Linux agent][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
U kunt voorbereiden en een aangepaste installatiekopie maken en vervolgens gebruiken om meerdere nieuwe virtuele machines. Dit wordt beschreven in [Azure Virtual Machines planning en implementatie van SAP NetWeaver][planning-guide]. Instellen van de inhoud van uw database met behulp van SAP-Software wordt ingericht Manager voor het installeren van een nieuwe SAP-systeem (u herstelt een databaseback-up van een schijf die gekoppeld aan de virtuele machine) of door rechtstreeks een databaseback-up terugzetten vanuit Azure-opslag, als uw DBMS-systemen ondersteuning voor biedt. Zie voor meer informatie, [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]. Als u al een SAP-systeem op uw on-premises virtuele machine (met name voor twee lagen systemen) hebt geïnstalleerd, kunt u de instellingen van het SAP na de implementatie van de Azure-VM aanpassen met behulp van de procedure Wijzig de naam van systeem ondersteund door SAP Software inrichting Manager (SAP Houd er rekening mee [1619720]). U kunt anders de SAP-software installeren nadat u de Azure-VM hebt geïmplementeerd.

Het volgende stroomdiagram ziet u de SAP-specifieke volgorde van stappen voor het implementeren van een virtuele machine van een aangepaste installatiekopie:

![Stroomdiagram van VM-implementatie voor SAP-systemen met behulp van een VM-installatiekopie in persoonlijke Marketplace][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-the-azure-portal"></a>Een virtuele machine maken met behulp van Azure portal

De eenvoudigste manier om een nieuwe virtuele machine maken van een installatiekopie van een beheerde schijf is met behulp van de Azure-portal. Lees voor meer informatie over het maken van een Schijfinstallatiekopie beheren [een beheerde installatiekopie maken van een gegeneraliseerde VM in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)

1.  Ga naar <https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages>. Of Selecteer in het menu van Azure portal **installatiekopieën**.
1.  Selecteer de installatiekopie van de beheerde schijf die u wilt implementeren en klik op **VM maken**

De wizard begeleidt u bij het instellen van de vereiste parameters voor het maken van de virtuele machine, naast alle vereiste resources, zoals netwerkinterfaces en storage-accounts. Sommige van deze parameters zijn:

1. **Grondbeginselen van**:
   * **Naam**: De naam van de resource (de naam van de virtuele machine).
   * **Type VM-schijf**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruikmaken van Premium Storage voor de gegevensschijven van uw wilt, raden wij met Premium Storage voor de OS-schijf.
   * **Gebruikersnaam en wachtwoord** of **openbare SSH-sleutel**: Voer de gebruikersnaam en wachtwoord van de gebruiker die is gemaakt tijdens het inrichten. Voor een virtuele Linux-machine, kunt u de openbare sleutel voor Secure Shell (SSH) waarmee u zich aanmeldt bij de machine.
   * **Abonnement**: Selecteer het abonnement dat u gebruiken wilt voor het inrichten van de nieuwe virtuele machine.
   * **Resourcegroep**: De naam van de resourcegroep voor de virtuele machine. U kunt u de naam van een nieuwe resourcegroep of de naam van een resourcegroep die al bestaat.
   * **Locatie**: Waar u de nieuwe virtuele machine implementeren. Als u wilt dat de virtuele machine verbinden met uw on-premises netwerk, zorg ervoor dat u de locatie van het virtuele netwerk dat maakt Azure verbinding met uw on-premises netwerk selecteert. Zie voor meer informatie, [Microsoft Azure-netwerken] [ planning-guide-microsoft-azure-networking] in [Azure Virtual Machines planning en implementatie van SAP NetWeaver] [ planning-guide].
1. **Grootte**:

     Zie voor een lijst van ondersteunde VM-typen, SAP-notitie [1928533]. Zorg ervoor dat u het juiste VM-type selecteren als u wilt gebruiken van Azure Premium Storage. Niet alle VM-typen bieden ondersteuning voor Premium-opslag. Zie voor meer informatie, [opslag: Microsoft Azure Storage- en gegevensschijven] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] en [Azure Premium Storage] [ planning-guide-azure-premium-storage] in [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide].

1. **Instellingen voor**:
   * **Storage**
     * **Schijftype**: Selecteer het schijftype van de besturingssysteemschijf. Als u gebruikmaken van Premium Storage voor de gegevensschijven van uw wilt, raden wij met Premium Storage voor de OS-schijf.
     * **Beheerde schijven gebruiken**: Selecteer Ja als u gebruikmaken van beheerde schijven wilt. Zie voor meer informatie over Managed Disks, hoofdstuk [Managed Disks] [ planning-guide-managed-disks] in de handleiding voor capaciteitsplanning.
   * **Netwerk**
     * **Virtueel netwerk** en **Subnet**: Voor het integreren van de virtuele machine met het intranet, selecteer het virtuele netwerk dat is verbonden met uw on-premises netwerk.
     * **Openbaar IP-adres**: Selecteer het openbare IP-adres dat u wilt gebruiken, of parameters voor het maken van een nieuw openbaar IP-adres invoeren. U kunt een openbaar IP-adres gebruiken voor toegang tot uw virtuele machine via Internet. Zorg ervoor dat u ook een netwerkbeveiligingsgroep voor beveiligde toegang tot uw virtuele machine te maken.
     * **Netwerkbeveiligingsgroep**: Zie voor meer informatie, [netwerkverkeer met netwerkbeveiligingsgroepen beheren][virtual-networks-nsg].
   * **Extensies**: U kunt extensies voor virtuele machines installeren door ze toe te voegen aan de implementatie. U hoeft niet de extensie toevoegen in deze stap. De extensies vereist voor ondersteuning van SAP zijn later geïnstalleerd. Zie hoofdstuk [configureren van de Azure uitgebreide controle-extensie voor SAP] [ deployment-guide-4.5] in deze handleiding.
   * **Hoge beschikbaarheid**: Selecteer een beschikbaarheidsset of geef de parameters voor het maken van een nieuwe beschikbaarheidsset. Zie voor meer informatie, [Azure-beschikbaarheidssets][planning-guide-3.2.3].
   * **Controle**
     * **Diagnostische gegevens over opstarten**: U kunt selecteren **uitschakelen** voor diagnostische gegevens over opstarten.
     * **Diagnostische gegevens van Guest OS**: U kunt selecteren **uitschakelen** voor het bewaken van diagnostische gegevens.

1. **Summary**:

   Controleer uw selecties en selecteer vervolgens **OK**.

Uw virtuele machine wordt geïmplementeerd in de resourcegroep die u hebt geselecteerd.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Een virtuele machine maken met behulp van een sjabloon

Voor het maken van een implementatie met behulp van een persoonlijke installatiekopie van het besturingssysteem van de Azure-portal, gebruikt u een van de volgende SAP-sjablonen. Deze sjablonen zijn gepubliceerd in de [GitHub-opslagplaats voor azure-quickstart-templates][azure-quickstart-templates-github]. U kunt ook handmatig maken een virtuele machine, met behulp van [PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**Met twee lagen (slechts één virtuele machine)-configuratiesjabloon** (sap-2-laag-gebruiker-installatiekopie)][sap-templates-2-tier-user-image]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine, moet u deze sjabloon gebruiken.
* [**Met twee lagen (slechts één virtuele machine)-configuratiesjabloon - beheerde schijf afbeelding** (sap-2-tier-user-image-md)][sap-templates-2-tier-user-image-md]

  Gebruik deze sjabloon voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine en de installatiekopie van een beheerde schijf.
* [**Drie lagen configuratiesjabloon (meerdere virtuele machines)** (sap-3-tier-gebruiker-installatiekopie)][sap-templates-3-tier-user-image]

  Voor het maken van een systeem met drie lagen met behulp van meerdere virtuele machines of uw eigen installatiekopie van het besturingssysteem, moet u deze sjabloon gebruiken.
* [**Drie lagen (meerdere virtuele machines) configuratiesjabloon - beheerde schijf afbeelding** (sap-3-tier-user-image-md)][sap-templates-3-tier-user-image-md]

  Gebruik deze sjabloon voor het maken van een systeem met drie lagen met behulp van meerdere virtuele machines of uw eigen installatiekopie van het besturingssysteem en de installatiekopie van een beheerde schijf.

Voer de volgende parameters voor de sjabloon in de Azure-portal:

1. **Grondbeginselen van**:
   * **Abonnement**: Het abonnement te gebruiken om de sjabloon te implementeren.
   * **Resourcegroep**: De resourcegroep te gebruiken om de sjabloon te implementeren. U kunt maken van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep in het abonnement.
   * **Locatie**: Waar om de sjabloon te implementeren. Als u een bestaande resourcegroep hebt geselecteerd, wordt de locatie van deze resourcegroep wordt gebruikt.
1. **Instellingen voor**:
   * **De ID van de SAP-systeem**: De id van de SAP-systeem.
   * **Type besturingssysteem**: Het besturingssysteemtype dat u implementeren wilt (Windows of Linux).
   * **Grootte van het SAP**: De grootte van de SAP-systeem.

     Het aantal SAP's vindt u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
   * **Beschikbaarheid van het systeem** (alleen in drie lagen sjabloon): De beschikbaarheid van het systeem.

     Selecteer **HA** voor een configuratie die geschikt is voor de installatie van een hoge beschikbaarheid. Twee databaseservers en twee servers voor ASCS worden gemaakt.
   * **Opslagtype** (alleen in twee lagen sjabloon): Het type opslag te gebruiken.

     Voor grotere systemen raden wij met behulp van Azure Premium Storage. Zie de volgende bronnen voor meer informatie over soorten opslag:
      * [Gebruik van Azure Premium-SSD-opslag voor SAP DBMS-instantie][2367194]
      * [Opslagstructuur van een virtuele machine voor RDBMS-implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Storage: Opslag met hoge prestaties voor werkbelastingen van de virtuele machine van Azure][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
   * **VHD-URI van de gebruikersinstallatiekopie** (alleen voor niet-beheerde schijf afbeelding sjabloon): De URI van de persoonlijke OS VHD, bijvoorbeeld: https:// image&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
   * **Storage-account van gebruiker installatiekopie** (alleen voor niet-beheerde schijf afbeelding sjabloon): De naam van het opslagaccount waar de persoonlijke installatiekopie van het besturingssysteem wordt opgeslagen, bijvoorbeeld &lt;accountname > in https://&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
   * **userImageId** (alleen voor beheerde schijf afbeelding sjabloon): Id van de installatiekopie van de beheerde schijf die u wilt gebruiken
   * **Gebruikersnaam van beheerder** en **beheerderswachtwoord**: De gebruikersnaam en wachtwoord.

     Een nieuwe gebruiker is gemaakt, voor aanmelding bij de virtuele machine.
   * **Nieuw of bestaand subnet**: Bepaalt of een nieuw virtueel netwerk en een subnet wordt gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
   * **Subnet-ID**: Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID meestal er als volgt uitzien: /subscriptions/&lt;abonnements-id > /resourceGroups/&lt;groepsnaam voor accountresources > /providers/Microsoft.Network/virtualNetworks/&lt;virtuele-netwerknaam > /subnets/&lt;subnetnaam >

1. **Voorwaarden en bepalingen**:  
    Lees en accepteer de juridische bepalingen.

1. Selecteer **Aankoop**.

#### <a name="install-the-vm-agent-linux-only"></a>Installeer de VM-Agent (alleen Linux)

Voor het gebruik van de sjablonen die zijn beschreven in de vorige sectie, moet de Linux-Agent al zijn geïnstalleerd in de afbeelding van de gebruiker of de implementatie mislukken. Download en installeer de VM-Agent in de afbeelding van de gebruiker, zoals beschreven in [downloaden, installeren en inschakelen van de Azure VM-Agent][deployment-guide-4.4]. Als u de sjablonen niet gebruikt, kunt u ook installeren de VM-Agent later opnieuw.

#### <a name="join-a-domain-windows-only"></a>Toevoegen aan een domein (alleen Windows)

Als uw Azure-implementatie is verbonden met een on-premises Active Directory of DNS-exemplaar via een Azure-site-naar-site VPN-verbinding of Azure ExpressRoute (dit heet *cross-premises* in [Azure Virtual Machines planning en implementatie van SAP NetWeaver][planning-guide]), is het waarschijnlijk dat de virtuele machine lid van een on-premises domein worden. Zie voor meer informatie over overwegingen voor deze stap [een VM toevoegen aan een on-premises domein (alleen Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Afhankelijk van hoe uw on-premises-netwerk is geconfigureerd, moet u mogelijk de proxy op de virtuele machine niet instellen. Als uw virtuele machine is verbonden met uw on-premises netwerk via VPN of ExpressRoute, de virtuele machine mogelijk geen toegang tot Internet en is niet mogelijk om te downloaden van de vereiste extensies of verzamelen van gegevens. Zie voor meer informatie, [configureert de proxy][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Bewaking configureren

Om er zeker van te zijn dat SAP biedt ondersteuning voor uw omgeving, instellen van de Azure Monitoring-extensie voor SAP, zoals beschreven in [configureren van de Azure uitgebreide controle-extensie voor SAP][deployment-guide-4.5]. Controleer de vereisten voor het bewaken van SAP en vereist minimaal vereiste versies van de Kernel van SAP en SAP Host-Agent in de resources die worden vermeld in [SAP resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Bewaking van selectievakje

Controleer of controle werkt, zoals beschreven in [controles en probleemoplossing voor het instellen van de end-to-end bewaking][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Het verplaatsen van een on-premises VM met behulp van een Azure niet gegeneraliseerde VHD met SAP

In dit scenario plant u een specifieke SAP-systeem van een on-premises-omgeving naar Azure verplaatsen. U kunt dit doen door het uploaden van de VHD met het besturingssysteem, de SAP-binaire bestanden en uiteindelijk de binaire bestanden voor de DBMS-systemen, plus de VHD's met de gegevens en logboekbestanden bestanden van het DBMS, naar Azure. In tegenstelling tot het scenario wordt beschreven in [Scenario 2: Een virtuele machine met een aangepaste installatiekopie implementeren voor SAP][deployment-guide-3.3], in dit geval u de hostnaam, SAP-SID, houden en de SAP-gebruiker-accounts in de Azure-VM, omdat ze zijn geconfigureerd in de on-premises omgeving. U hoeft niet te generaliseren van het besturingssysteem. In dit scenario geldt meestal voor cross-premises scenario's waarbij deel uit van de SAP-landschap dat on-premises wordt uitgevoerd en deel ervan wordt uitgevoerd op Azure.

In dit scenario de VM-Agent is **niet** automatisch geïnstalleerd tijdens de implementatie. Omdat de VM-Agent en de Azure uitgebreide controle-extensie voor SAP zijn vereist voor het uitvoeren van SAP NetWeaver op Azure, die u wilt downloaden, installeren en schakel beide onderdelen handmatig nadat u de virtuele machine hebt gemaakt.

Zie de volgende bronnen voor meer informatie over de Azure VM-Agent.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Overzicht van Azure Virtual Machine Agent][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Gebruikershandleiding voor Azure Linux Agent][virtual-machines-linux-agent-user-guide]
>
>

- - -

Het volgende stroomdiagram toont de volgorde van stappen voor het verplaatsen van een on-premises VM met behulp van een Azure niet gegeneraliseerde VHD:

![Stroomdiagram van VM-implementatie voor SAP-systemen met behulp van een VM-schijf][deployment-guide-figure-400]

Als de schijf al is geüpload en gedefinieerd in Azure (Zie [Azure Virtual Machines planning en implementatie van SAP NetWeaver][planning-guide]), voer de taken die worden beschreven in de volgende gedeelten.

#### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Voor het maken van een implementatie met behulp van de schijf met een persoonlijke besturingssysteem via Azure portal, gebruikt u de SAP-sjabloon is gepubliceerd in de [GitHub-opslagplaats voor azure-quickstart-templates][azure-quickstart-templates-github]. U kunt ook handmatig maken een virtuele machine, met behulp van PowerShell.

* [**Met twee lagen (slechts één virtuele machine)-configuratiesjabloon** (sap-2-laag-gebruiker-schijf)][sap-templates-2-tier-os-disk]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine, moet u deze sjabloon gebruiken.
* [**Met twee lagen (slechts één virtuele machine)-configuratiesjabloon - beheerde schijf** (sap-2-tier-user-disk-md)][sap-templates-2-tier-os-disk-md]

  Voor het maken van een systeem met twee lagen met behulp van slechts één virtuele machine en een beheerde schijf maken, moet u deze sjabloon gebruiken.

Voer de volgende parameters voor de sjabloon in de Azure-portal:

1. **Grondbeginselen van**:
   * **Abonnement**: Het abonnement te gebruiken om de sjabloon te implementeren.
   * **Resourcegroep**: De resourcegroep te gebruiken om de sjabloon te implementeren. U kunt maken van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep in het abonnement.
   * **Locatie**: Waar om de sjabloon te implementeren. Als u een bestaande resourcegroep hebt geselecteerd, wordt de locatie van deze resourcegroep wordt gebruikt.
1. **Instellingen voor**:
   * **De ID van de SAP-systeem**: De id van de SAP-systeem.
   * **Type besturingssysteem**: Het besturingssysteemtype dat u implementeren wilt (Windows of Linux).
   * **Grootte van het SAP**: De grootte van de SAP-systeem.

     Het aantal SAP's vindt u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
   * **Opslagtype** (alleen in twee lagen sjabloon): Het type opslag te gebruiken.

     Voor grotere systemen raden wij met behulp van Azure Premium Storage. Zie de volgende bronnen voor meer informatie over soorten opslag:
      * [Gebruik van Azure Premium-SSD-opslag voor SAP DBMS-instantie][2367194]
      * [Opslagstructuur van een virtuele machine voor RDBMS-implementaties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#65fa79d6-a85f-47ee-890b-22e794f51a64)
      * [Premium Storage: Opslag met hoge prestaties voor werkbelastingen van de virtuele Machine van Azure][storage-premium-storage-preview-portal]
      * [Inleiding tot Microsoft Azure Storage][storage-introduction]
   * **OS-schijf VHD-URI** (alleen voor niet-beheerde schijf-sjabloon): De URI van de persoonlijke besturingssysteemschijf, bijvoorbeeld: https://&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
   * **Beheerde schijf-Id van de besturingssysteemschijf** (alleen voor beheerde schijf-sjabloon): De Id van de beheerde schijf besturingssysteemschijf, /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
   * **Nieuw of bestaand subnet**: Bepaalt of een nieuw virtueel netwerk en subnet worden gemaakt of een bestaand subnet wordt gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
   * **Subnet-ID**: Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID meestal er als volgt uitzien: /subscriptions/&lt;abonnements-id > /resourceGroups/&lt;groepsnaam voor accountresources > /providers/Microsoft.Network/virtualNetworks/&lt;virtuele-netwerknaam > /subnets/&lt;subnetnaam >

1. **Voorwaarden en bepalingen**:  
    Lees en accepteer de juridische bepalingen.

1. Selecteer **Aankoop**.

#### <a name="install-the-vm-agent"></a>De VM-Agent installeren

Voor het gebruik van de sjablonen die zijn beschreven in de vorige sectie, de VM-Agent moet worden geïnstalleerd op de schijf met het besturingssysteem of de implementatie mislukken. Download en installeer de VM-Agent op de virtuele machine, zoals beschreven in [downloaden, installeren en inschakelen van de Azure VM-Agent][deployment-guide-4.4].

Als u de sjablonen die worden beschreven in de voorgaande sectie niet gebruikt, kunt u de VM-Agent ook later installeren.

#### <a name="join-a-domain-windows-only"></a>Toevoegen aan een domein (alleen Windows)

Als uw Azure-implementatie is verbonden met een on-premises Active Directory of DNS-exemplaar via een Azure-site-naar-site VPN-verbinding of ExpressRoute (dit heet *cross-premises* in [Azure Virtual Machines plannen en implementatie van SAP NetWeaver][planning-guide]), is het waarschijnlijk dat de virtuele machine lid van een on-premises domein worden. Zie voor meer informatie over overwegingen voor deze taak [een VM toevoegen aan een on-premises domein (alleen Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Afhankelijk van hoe uw on-premises-netwerk is geconfigureerd, moet u mogelijk de proxy op de virtuele machine niet instellen. Als uw virtuele machine is verbonden met uw on-premises netwerk via VPN of ExpressRoute, de virtuele machine mogelijk geen toegang tot Internet en is niet mogelijk om te downloaden van de vereiste extensies of verzamelen van gegevens. Zie voor meer informatie, [configureert de proxy][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Bewaking configureren

Om er zeker van te zijn dat SAP biedt ondersteuning voor uw omgeving, instellen van de Azure Monitoring-extensie voor SAP, zoals beschreven in [configureren van de Azure uitgebreide controle-extensie voor SAP][deployment-guide-4.5]. Controleer de vereisten voor het bewaken van SAP en vereist minimaal vereiste versies van de Kernel van SAP en SAP Host-Agent in de resources die worden vermeld in [SAP resources][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Bewaking van selectievakje

Controleer of controle werkt, zoals beschreven in [controles en probleemoplossing voor het instellen van de end-to-end bewaking][deployment-guide-troubleshooting-chapter].

## <a name="update-the-monitoring-configuration-for-sap"></a>Bijwerken van de configuratie van de bewaking voor SAP

Werk de configuratie van de SAP bewaking in een van de volgende scenario's:
* Het gezamenlijke Microsoft/SAP-team breidt de mogelijkheden voor bewaking en vraagt meer of minder items.
* Microsoft introduceert een nieuwe versie van de onderliggende Azure-infrastructuur die voorziet in de bewakingsgegevens en de Azure uitgebreide controle-extensie voor SAP moet aan deze wijzigingen worden aangepast.
* U extra gegevensschijven koppelen aan uw Azure-VM of u een gegevensschijf verwijderen. In dit scenario werkt het verzamelen van gegevens met betrekking tot opslag. De configuratie wijzigen door het toevoegen of verwijderen van eindpunten of door het IP-adressen toewijzen aan een virtuele machine heeft geen invloed op de configuratie van de bewaking.
* U wijzigen de grootte van uw Azure-VM, bijvoorbeeld van grootte A5 naar elke andere VM-grootte.
* U toevoegen nieuwe netwerkinterfaces aan uw Azure-VM.

Voor het bijwerken van instellingen voor controle bijwerken van de bewakingsinfrastructuur met de volgende stappen in [configureren van de Azure uitgebreide controle-extensie voor SAP][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment"></a>Gedetailleerde taken voor SAP-software-implementatie

In deze sectie bevat gedetailleerde stappen voor het uitvoeren van specifieke taken in de configuratie en implementatie-proces.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Implementeren van Azure PowerShell-cmdlets

1. Ga naar [Downloads van Microsoft Azure](https://azure.microsoft.com/downloads/).
1. Onder **opdrachtregelprogramma's**onder **PowerShell**, selecteer **Windows installeren**.
1. Selecteer in het dialoogvenster Microsoft Download Manager voor het gedownloade bestand (bijvoorbeeld WindowsAzurePowershellGet.3f.3f.3fnew.exe), **uitvoeren**.
1. Als u wilt uitvoeren van Microsoft Web Platform Installer (Microsoft Web PI), selecteer **Ja**.
1. Een pagina die het lijkt erop dat deze wordt weergegeven:

   ![Installatiepagina voor Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

1. Selecteer **installeren**, en accepteer de licentievoorwaarden voor Microsoft-Software.
1. PowerShell is geïnstalleerd. Selecteer **voltooien** om de installatiewizard te sluiten.

Controleer regelmatig op updates voor de PowerShell-cmdlets, die meestal elke maand worden bijgewerkt. De eenvoudigste manier om te controleren op updates is voorgaande installatie stappen, tot aan de installatiepagina wordt weergegeven in stap 5. Het versienummer voor de datum- en release van de cmdlets zijn opgenomen op de pagina wordt weergegeven in stap 5. Tenzij anders vermeld in de SAP-notitie [1928533] of SAP-notitie [2015553], het is raadzaam dat u met de nieuwste versie van Azure PowerShell-cmdlets werkt.

Om te controleren of de versie van de Azure PowerShell-cmdlets die zijn geïnstalleerd op uw computer, moet u deze PowerShell-opdracht uitvoeren:
```powershell
(Get-Module AzureRm.Compute).Version
```
Het resultaat ziet er zo uit:

![Resultaat van de controle van Azure PowerShell-cmdlet-versie][deployment-guide-figure-600]
<a name="figure-6"></a>

Als de versie Azure cmdlet is geïnstalleerd op uw computer de huidige versie is, de eerste pagina van de installatiewizard aangeeft dat ze door toe te voegen **(geïnstalleerd)** aan de Producttitel (Zie de volgende schermafbeelding). Uw PowerShell Azure-cmdlets zijn bijgewerkt. Sluit de installatiewizard, selecteert u **afsluiten**.

![Installatiepagina voor Azure PowerShell-cmdlets waarmee wordt aangegeven dat de meest recente versie van Azure PowerShell-cmdlets zijn geïnstalleerd][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI implementeren

1. Ga naar [Downloads van Microsoft Azure](https://azure.microsoft.com/downloads/).
1. Onder **opdrachtregelprogramma's**onder **Azure-opdrachtregelinterface**, selecteer de **installeren** koppeling voor uw besturingssysteem.
1. Selecteer in het dialoogvenster Microsoft Download Manager voor het gedownloade bestand (bijvoorbeeld WindowsAzureXPlatCLI.3f.3f.3fnew.exe), **uitvoeren**.
1. Als u wilt uitvoeren van Microsoft Web Platform Installer (Microsoft Web PI), selecteer **Ja**.
1. Een pagina die het lijkt erop dat deze wordt weergegeven:

   ![Installatiepagina voor Azure PowerShell-cmdlets][deployment-guide-figure-500]<a name="figure-5"></a>

1. Selecteer **installeren**, en accepteer de licentievoorwaarden voor Microsoft-Software.
1. Azure CLI is geïnstalleerd. Selecteer **voltooien** om de installatiewizard te sluiten.

Controleer regelmatig op updates voor Azure CLI, die meestal wordt maandelijks bijgewerkt. De eenvoudigste manier om te controleren op updates is voorgaande installatie stappen, tot aan de installatiepagina wordt weergegeven in stap 5.

Om te controleren of de versie van Azure CLI die op uw computer is geïnstalleerd, moet u deze opdracht uitvoeren:
```
azure --version
```

Het resultaat ziet er zo uit:

![Resultaat van de controle van de Azure CLI-versie][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Een virtuele machine toevoegen aan een on-premises domein (alleen Windows)

Als u SAP-VM's in een cross-premises-scenario implementeren, waarbij on-premises Active Directory en DNS zijn uitgebreid in Azure, is het waarschijnlijk dat de virtuele machines zijn worden gebruikt voor het toevoegen van een on-premises domein. De gedetailleerde stappen naar een virtuele machine toevoegen aan een on-premises domein en de aanvullende software vereist voor het lid zijn van een on-premises domein, is afhankelijk van de klant. Als u wilt deelnemen aan een virtuele machine naar een on-premises domein, moet u normaal gesproken extra software, zoals antimalware-software, en de back-up of controle-software installeren.

In dit scenario moet u ook om ervoor te zorgen dat als proxy-instellingen van Internet worden gedwongen wanneer een virtuele machine lid van een domein in uw omgeving, het Windows lokale systeemaccount (S-1-5-18) in de Gast-VM heeft de dezelfde proxyinstellingen. De eenvoudigste manier is om af te dwingen de proxy met behulp van Groepsbeleid, dat van toepassing op systemen in het domein van een domein.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Downloaden, installeren en de Azure VM-Agent inschakelen

Voor virtuele machines die zijn geïmplementeerd vanuit een installatiekopie van het besturingssysteem dat niet is gegeneraliseerd (bijvoorbeeld een installatiekopie die niet afkomstig is van het hulpprogramma Windows System Preparation of sysprep), moet u handmatig downloaden, installeren en inschakelen van de Azure VM-Agent.

Als u een virtuele machine van Azure Marketplace implementeert, is deze stap niet vereist. Installatiekopieën van Azure Marketplace beschikt al over de Azure VM-Agent.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows

1. Download de Azure VM-Agent:
   1.  Download de [Azure VM-Agent-installatiepakket](https://go.microsoft.com/fwlink/?LinkId=394789).
   1.  Het VM-Agent-MSI-pakket lokaal op een pc of server Store.
1. De Azure VM-Agent installeren:
   1.  Verbinding maken met de geïmplementeerde virtuele machine van Azure met behulp van Remote Desktop Protocol (RDP).
   1.  Open een Windows Explorer-venster op de virtuele machine en selecteer de doeldirectory voor het MSI-bestand van de VM-Agent.
   1.  Sleep de Azure VM Agent-installatieprogramma MSI-bestand van uw lokale computer/server naar de doelmap van de VM-Agent op de virtuele machine.
   1.  Dubbelklik op het MSI-bestand op de virtuele machine.
1. Voor virtuele machines die zijn gekoppeld aan de on-premises domeinen, zorg ervoor dat eventuele Internet proxy-instellingen ook van toepassing op het lokale Windows-systeem-account (S-1-5-18) in de virtuele machine, zoals beschreven in [configureert de proxy][deployment-guide-configure-proxy]. De VM-Agent wordt uitgevoerd in deze context en moet kunnen verbinding maken met Azure.

Er is geen gebruikersinteractie is vereist voor het bijwerken van de Azure VM-Agent. De VM-Agent wordt automatisch bijgewerkt, en een virtuele machine opnieuw opstarten niet vereist.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux

Gebruik de volgende opdrachten voor het installeren van de VM-Agent voor Linux:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) of Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Als de agent al is geïnstalleerd, voor het bijwerken van de Azure Linux Agent, voer de stappen in [bijwerken van de Azure Linux Agent op een virtuele machine naar de nieuwste versie van GitHub][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Configureer de proxy

De stappen die u moet het configureren van de proxy in Windows verschillen van de wijze waarop die u de proxy in Linux configureert.

#### <a name="windows"></a>Windows

Proxy-instellingen moeten correct worden ingesteld voor het lokale systeemaccount gebruikt voor toegang tot het Internet. Als de proxy-instellingen zijn niet ingesteld met Groepsbeleid, kunt u de instellingen voor het lokale systeemaccount configureren.

1. Ga naar **Start**, voer **gpedit.msc**, en selecteer vervolgens **Enter**.
1. Selecteer **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Internet Explorer**. Zorg ervoor dat de instelling **proxy maken instellingen per computer (in plaats per gebruiker)** is uitgeschakeld of niet is geconfigureerd.
1. In **Configuratiescherm**, gaat u naar **Netwerkcentrum** > **Internetopties**.
1. Op de **verbindingen** tabblad de **LAN-instellingen** knop.
1. Schakel de **-instellingen automatisch detecteren** selectievakje.
1. Selecteer de **een proxyserver gebruiken voor uw LAN** selectievakje en voer vervolgens de proxy-adres en poort.
1. Selecteer de **Geavanceerd** knop.
1. In de **uitzonderingen** voert u het IP-adres **168.63.129.16**. Selecteer **OK**.

#### <a name="linux"></a>Linux

Configureer de juiste proxy in het configuratiebestand van de Microsoft Azure-Gastagent, bevindt zich onder \\enzovoort\\waagent.conf.

De volgende parameters instellen:

1. **HTTP-proxyhost**. Bijvoorbeeld, ingesteld op **proxy.corp.local**.
   ```
   HttpProxy.Host=<proxy host>

   ```
1. **HTTP-proxypoort**. Bijvoorbeeld, ingesteld op **80**.
   ```
   HttpProxy.Port=<port of the proxy host>

   ```
1. De agent opnieuw starten.

   ```
   sudo service waagent restart
   ```

De proxy-instellingen in \\enzovoort\\waagent.conf ook van toepassing op het vereiste VM-extensies. Als u wilt gebruiken de Azure-opslagplaatsen, zorg ervoor dat het verkeer naar deze opslagplaatsen gaat niet via het intranet van uw on-premises. Als u de gebruiker gedefinieerde routes om te geforceerde tunneling inschakelen, moet u een route toevoegen hebt gemaakt routeert die verkeer naar de opslagplaatsen die rechtstreeks met Internet en niet via de site-naar-site VPN-verbinding.

* **SLES**

  U moet ook routes toevoegen voor de IP-adressen die worden vermeld \\enzovoort\\regionserverclnt.cfg. De volgende afbeelding toont een voorbeeld:

  ![Geforceerde tunneling][deployment-guide-figure-50]


* **RHEL**

  U moet ook routes toevoegen voor de IP-adressen van de hosts die worden vermeld in \\enzovoort\\yum.repos.d\\rhui load balancers. Zie de voorgaande afbeelding voor een voorbeeld.

* **Oracle Linux**

  Er zijn geen opslagplaatsen voor Oracle Linux op Azure. U moet uw eigen opslagplaatsen voor Oracle Linux configureren of gebruiken van de openbare opslagplaatsen.

Zie voor meer informatie over de gebruiker gedefinieerde routes, [gebruiker gedefinieerde routes en doorsturen via IP][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Configureren van de Azure uitgebreide controle-extensie voor SAP

Wanneer u de virtuele machine hebt voorbereid, zoals beschreven in [implementatiescenario's van virtuele machines voor SAP op Azure][deployment-guide-3], de Azure VM-Agent is geïnstalleerd op de virtuele machine. De volgende stap is het implementeren van de Azure uitgebreide controle-extensie voor SAP, die beschikbaar in de opslagplaats van de Azure-extensie in de globale Azure-datacenters is. Zie voor meer informatie, [Azure Virtual Machines planning en implementatie van SAP NetWeaver][planning-guide-9.1].

U kunt PowerShell of Azure CLI installeren en configureren van de Azure uitgebreide controle-extensie voor SAP. Zie de extensie installeren op een Windows- of Linux-VM met behulp van een Windows-machine [Azure PowerShell][deployment-guide-4.5.1]. Zie de extensie installeren op een Linux-VM te maken met een Linux-bureaublad [Azure CLI][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell voor Linux en Windows-VM 's

De Azure uitgebreide controle-extensie voor SAP installeren met behulp van PowerShell:

1. Zorg ervoor dat u de nieuwste versie van de Azure PowerShell-cmdlet hebt geïnstalleerd. Zie voor meer informatie, [implementeren van Azure PowerShell-cmdlets][deployment-guide-4.1].  
1. Voer de volgende PowerShell-cmdlet uit.
    Voor een lijst met beschikbare omgevingen, voert u `commandlet Get-AzureRmEnvironment`. Als u wilt het gebruik van global Azure, uw omgeving is **AzureCloud**. Selecteer voor Azure in China, **AzureChinaCloud**.

    ```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Connect-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>

    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Nadat u de accountgegevens van uw invoeren en identificeren van de virtuele machine van Azure, wordt het script implementeert u de vereiste extensies en schakelt u de vereiste functies. Dit kan enkele minuten duren.
Voor meer informatie over `Set-AzureRmVMAEMExtension`, Zie [Set-AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].

![Geslaagde uitvoering van SAP-specifieke Azure-cmdlet Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

De `Set-AzureRmVMAEMExtension` configuratie heeft alle de stappen voor het configureren van host bewaking voor SAP.

De uitvoer van het script bevat de volgende informatie:

* Bevestiging dat bewaking voor de schijf met het besturingssysteem en alle extra gegevensschijven is geconfigureerd.
* De volgende twee berichten Bevestig de configuratie van metrische opslaggegevens voor een specifiek opslagaccount.
* Een line-of-uitvoer geeft de status van de werkelijke update van de configuratie van de bewaking.
* Een andere line-of uitvoer bevestigt dat de configuratie is geïmplementeerd of bijgewerkt.
* De laatste regel van de uitvoer worden ter informatie. Het bevat uw opties voor het testen van de configuratie van de bewaking.
* Als u wilt dat alle stappen van Azure verbeterde controle met succes zijn uitgevoerd en dat de infrastructuur van Azure de benodigde gegevens biedt controleren, doorgaan met de gereedheidscontrole uit voor de Azure uitgebreide controle-extensie voor SAP, zoals beschreven in [Gereedheidscontrole voor Azure uitgebreide bewaking voor SAP][deployment-guide-5.1].
* Wacht 15 tot 30 minuten voor Azure Diagnostics om de relevante gegevens te verzamelen.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI voor Linux-VM 's

De Azure uitgebreide controle-extensie voor SAP installeren met behulp van Azure CLI:

   1. Klassieke Azure-CLI, installeert, zoals beschreven in [de klassieke Azure-CLI installeren][azure-cli].
   1. Meld u aan met uw Azure-account:

      ```
      azure login
      ```

   1. Schakel over naar modus Azure Resource Manager:

      ```
      azure config mode arm
      ```

   1. Schakel de bewaking in Azure uitgebreid:

      ```
      azure vm enable-aem <resource-group-name> <vm-name>
      ```

1. Installeren met behulp van Azure CLI 2.0

   1. Azure CLI 2.0 installeren zoals beschreven in [Azure CLI 2.0 installeren][azure-cli-2].
   1. Meld u aan met uw Azure-account:

      ```
      az login
      ```

   1. Azure CLI-AEM-extensie installeren
  
      ```
      az extension add --name aem
      ```
  
   1. Installeer de extensie met
  
      ```
      az vm aem set -g <resource-group-name> -n <vm name>
      ```

1. Controleer of de extensie Azure uitgebreid Monitoring actief is op de virtuele Azure Linux-machine. Controleer of het bestand \\var\\lib\\AzureEnhancedMonitor\\PerfCounters bestaat. Als deze bestaat, bij een opdrachtprompt, voer deze opdracht uit om gegevens die worden verzameld door de Azure uitgebreid Monitor weer te geven:

   ```
   cat /var/lib/AzureEnhancedMonitor/PerfCounters
   ```

   De uitvoer ziet er als volgt uit:
   ```
   ...
   2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
   2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
   ...
   ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Controles en probleemoplossing voor het bewaken van end-to-end

Nadat u hebt uw Azure-VM geïmplementeerd en de relevante Azure monitoring-infrastructuur instellen, moet u controleren of alle onderdelen van de extensie Azure uitgebreid Monitoring werkt zoals verwacht.

De gereedheidscontrole voor de Azure uitgebreide controle-extensie voor SAP uitvoeren, zoals beschreven in [gereedheidscontrole uit voor de Azure uitgebreide controle-extensie voor SAP][deployment-guide-5.1]. Als alle resultaten van de gereedheid van de positieve en alle relevante prestatiemeteritems OK worden weergegeven, is Azure-bewaking ingesteld met succes. U kunt doorgaan met de installatie van de Hostagent SAP, zoals beschreven in de SAP-opmerkingen in [SAP resources][deployment-guide-2.2]. Als de gereedheidscontrole tellers ontbreken geeft, voert u de statuscontrole voor de Azure monitoring-infrastructuur, zoals beschreven in [Statuscontrole voor Azure infrastructuur bewakingsconfiguratie] [ deployment-guide-5.2]. Zie voor meer opties voor het oplossen van problemen, [probleemoplossing voor Azure-bewaking voor SAP][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>De gereedheidscontrole voor de Azure uitgebreide controle-extensie voor SAP

Deze controle zorgt ervoor dat alle maatstaven voor prestaties die worden weergegeven in de SAP-toepassing worden geleverd door de onderliggende infrastructuur van Azure controleren.

#### <a name="run-the-readiness-check-on-a-windows-vm"></a>Voer de gereedheidscontrole uit op een Windows-VM

1. Aanmelden bij de virtuele machine van Azure (met een beheerdersaccount is niet nodig).
1. Open een opdrachtpromptvenster.
1. Wijzig bij de opdrachtprompt de map naar de installatiemap van de Azure uitgebreide controle-extensie voor SAP: C:\\Packages\\Plugins\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;version>\\drop

   De *versie* in het pad naar de controle-extensie kan variëren. Als u mappen voor meerdere versies van de controle-extensie in de installatiemap ziet, Controleer de configuratie van de AzureEnhancedMonitoring Windows-service en ga vervolgens naar de map die is aangeduid als *pad naar uitvoerbaar bestand* .

   ![Eigenschappen van de Azure uitgebreide controle-extensie voor SAP-service][deployment-guide-figure-1000]

1. Voer bij de opdrachtprompt **azperflib.exe** zonder parameters.

   > [!NOTE]
   > Azperflib.exe wordt in een lus wordt uitgevoerd en werkt de verzamelde tellers elke 60 seconden. Als u wilt beëindigen van de lus, sluit het opdrachtpromptvenster.
   >
   >

Als de Azure uitgebreide controle-extensie is niet geïnstalleerd of de AzureEnhancedMonitoring-service wordt niet uitgevoerd, heeft de extensie niet correct is geconfigureerd. Zie voor gedetailleerde informatie over het implementeren van de extensie [probleemoplossing van de Azure monitoring-infrastructuur voor SAP][deployment-guide-5.3].

> [!NOTE]
> De Azperflib.exe is een onderdeel dat kan niet worden gebruikt voor eigen doeleinden. Het is een onderdeel die voorziet in een Azure-bewakingsgegevens die betrekking hebben op de virtuele machine voor de SAP-Host-Agent.
> 

##### <a name="check-the-output-of-azperflibexe"></a>Controleer de uitvoer van azperflib.exe

Azperflib.exe uitvoer ziet u dat alle Azure-prestatiemeteritems voor SAP wordt gevuld. Een indicator samenvatting en de status weergegeven onder aan de lijst met items die worden verzameld, de status van de Azure-bewaking.

![Uitvoer van de statuscontrole van de door het uitvoeren van azperflib.exe, waarmee wordt aangegeven dat er geen problemen aanwezig][deployment-guide-figure-1100]
<a name="figure-11"></a>

Controleer het resultaat geretourneerd voor de **tellers totaal** -uitvoer, die wordt gerapporteerd als leeg, en voor **integriteitsstatus**, zoals weergegeven in de afbeelding hierboven.

De resulterende waarden als volgt interpreteren:

| Resultaatwaarden Azperflib.exe | Bewaking van status van Azure |
| --- | --- |
| **API-aanroepen - niet beschikbaar** | Items die niet beschikbaar kunnen zijn niet van toepassing op de virtuele-machineconfiguratie of fouten zijn. Zie **integriteitsstatus**. |
| **Prestatiemeteritems van de totale - leeg** |De volgende twee items voor Azure-opslag kunnen niet leeg zijn: <ul><li>Opslag Op Leeslatentie Server msec</li><li>Opslag Op Leeslatentie E2E msec</li></ul>Alle andere tellers moeten waarden hebben. |
| **De integriteitsstatus van de** |Alleen OK als resultaat de status geeft **OK**. |
| **Diagnostics** |Gedetailleerde informatie over de integriteitsstatus van de. |

Als de **integriteitsstatus** waarde is geen **OK**, volg de instructies in [Statuscontrole voor Azure infrastructuur bewakingsconfiguratie] [ deployment-guide-5.2].

#### <a name="run-the-readiness-check-on-a-linux-vm"></a>Voer de gereedheidscontrole uit op een Linux VM

1. Verbinding maken met de virtuele Machine in Azure met behulp van SSH.

1. Controleer de uitvoer van de Azure uitgebreide controle-extensie.

   a.  Voer `more /var/lib/AzureEnhancedMonitor/PerfCounters` uit.

   **Verwacht resultaat**: Retourneert de lijst met prestatiemeteritems. Het bestand mag niet leeg zijn.

   b. Voer `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` uit.

   **Verwacht resultaat**: Retourneert één regel waar de fout is **geen**, bijvoorbeeld **3; config; Fout; 0, 0; geen; 0; 1456416792; tst-servercs;**

   c. Voer `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` uit.

   **Verwacht resultaat**: Geeft als resultaat als leeg of bestaat niet.

Als de voorgaande controle niet geslaagd is, moet u deze extra controles uitvoeren:

1. Zorg ervoor dat de waagent is geïnstalleerd en ingeschakeld.

   a.  Voer `sudo ls -al /var/lib/waagent/` uit.

     **Verwacht resultaat**: Geeft een lijst van de inhoud van de map waagent.

   b.  Voer `ps -ax | grep waagent` uit.

   **Verwacht resultaat**: Een vermelding die vergelijkbaar is met weergegeven: `python /usr/sbin/waagent -daemon`

1. Zorg ervoor dat de Azure uitgebreide controle-extensie geïnstalleerd en actief is.

   a.  Voer `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` uit.

   **Verwacht resultaat**: Geeft een lijst van de inhoud van de extensie voor Azure uitgebreid Monitoring-map.

   b. Voer `ps -ax | grep AzureEnhanced` uit.

   **Verwacht resultaat**: Een vermelding die vergelijkbaar is met weergegeven: `python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

1. SAP-Host-Agent installeren zoals beschreven in de SAP-notitie [1031096], en controleer de uitvoer van `saposcol`.

   a.  Voer `/usr/sap/hostctrl/exe/saposcol -d` uit.

   b.  Voer `dump ccm` uit.

   c.  Controleer of de **Virtualization_Configuration\Enhanced bewaking toegang** meetwaarde is **waar**.

Als u al een SAP NetWeaver ABAP-toepassingsserver geïnstalleerd hebt, transactie ST06 openen en controleer of de verbeterde controle is ingeschakeld.

Als een van deze controle mislukt, en Zie voor gedetailleerde informatie over het implementeren van de extensie [probleemoplossing van de Azure monitoring-infrastructuur voor SAP][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>De configuratie voor de Azure monitoring infrastructuur status controleren

Als een deel van de bewaking correct zoals aangegeven door de test wordt beschreven in gegevens niet worden afgeleverd [gereedheidscontrole voor Azure uitgebreide bewaking voor SAP][deployment-guide-5.1], voert de `Test-AzureRmVMAEMExtension` cmdlet om te controleren of de Azure-infrastructuur en de controle-extensie bewaking voor SAP zijn correct geconfigureerd.

1. Zorg ervoor dat u de nieuwste versie van de Azure PowerShell-cmdlet hebt geïnstalleerd, zoals beschreven in [implementeren van Azure PowerShell-cmdlets][deployment-guide-4.1].
1. Voer de volgende PowerShell-cmdlet uit. Voor een lijst met beschikbare omgevingen, voert u de cmdlet `Get-AzureRmEnvironment`. Voor het gebruik van global Azure, selecteer de **AzureCloud** omgeving. Selecteer voor Azure in China, **AzureChinaCloud**.
   ```powershell
   $env = Get-AzureRmEnvironment -Name <name of the environment>
   Connect-AzureRmAccount -Environment $env
   Set-AzureRmContext -SubscriptionName <subscription name>
   Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
   ```

1. Voer uw accountgegevens en identificeren van de virtuele machine van Azure.

   ![Invoer pagina van de SAP-specifieke Azure cmdlet Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

1. Het script test de configuratie van de virtuele machine die u selecteert.

   ![Uitvoer van geslaagde test van de Azure monitoring-infrastructuur voor SAP][deployment-guide-figure-1300]

Zorg ervoor dat elk resultaat van de gezondheid van **OK**. Als het aantal controles worden niet weergegeven **OK**, voert u de cmdlet update zoals beschreven in [configureren van de Azure uitgebreide controle-extensie voor SAP][deployment-guide-4.5]. Wacht 15 minuten en Herhaal de controles die worden beschreven in [gereedheidscontrole voor Azure uitgebreide bewaking voor SAP] [ deployment-guide-5.1] en [Statuscontrole voor Azure-infrastructuur configuratie van de bewaking] [deployment-guide-5.2]. Als de controles aangeven dat er zich een probleem met enkele of alle items, Zie [probleemoplossing van de Azure monitoring-infrastructuur voor SAP][deployment-guide-5.3].

> [!Note]
> U kunt een aantal waarschuwingen in gevallen waarin het gebruik van beheerde standaardschijven Azure ervaren. Waarschuwingen worden weergegeven in plaats van de tests 'OK' retourneren. Dit is normaal en beoogde in het geval van dat schijftype. Zie ook [probleemoplossing van de Azure monitoring-infrastructuur voor SAP][deployment-guide-5.3]
> 

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Het oplossen van de Azure monitoring-infrastructuur voor SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure-prestatiemeteritems, worden niet weergegeven op alle

De AzureEnhancedMonitoring Windows-service verzamelt metrische gegevens voor prestaties in Azure. Als de service is niet juist geïnstalleerd of als deze niet in uw virtuele machine wordt uitgevoerd, kunnen geen prestatiegegevens worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de Azure uitgebreide controle-extensie is leeg

###### <a name="issue"></a>Probleem

De installatiemap C:\\pakketten\\invoegtoepassingen\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;versie >\\vervolgkeuzelijst is leeg.

###### <a name="solution"></a>Oplossing

De extensie is niet geïnstalleerd. Bepalen of dit een probleem met de proxy is (zoals eerder beschreven). U moet mogelijk de machine opnieuw starten of opnieuw uitvoeren de `Set-AzureRmVMAEMExtension` configuratiescript.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Service voor Azure verbeterde controle bestaat niet

###### <a name="issue"></a>Probleem

De AzureEnhancedMonitoring Windows-service bestaat niet.

Azperflib.exe uitvoer genereert een fout opgetreden:

![Uitvoering van azperflib.exe geeft aan dat de service van de extensie Azure uitgebreide bewaking voor SAP wordt niet uitgevoerd][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Oplossing

Als de service niet bestaat, is de Azure uitgebreide controle-extensie voor SAP niet correct is geïnstalleerd. De extensie implementeren met behulp van de stappen beschreven voor het implementatiescenario in [implementatiescenario's van virtuele machines voor SAP in Azure][deployment-guide-3].

Nadat u de extensie geïmplementeerd na een uur, opnieuw controleren of de Azure-prestatiemeteritems u in de Azure-VM vindt.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Service voor Azure verbeterde controle bestaat, maar niet kan worden gestart

###### <a name="issue"></a>Probleem

De service Windows AzureEnhancedMonitoring bestaat en is ingeschakeld, maar niet kan worden gestart. Controleer het logboek voor toepassingsgebeurtenissen voor meer informatie.

###### <a name="solution"></a>Oplossing

De configuratie is onjuist. Opnieuw opstarten van de controle-extensie voor de virtuele machine, zoals beschreven in [configureren van de Azure uitgebreide controle-extensie voor SAP][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Er ontbreken enkele Azure-prestatiemeteritems

De AzureEnhancedMonitoring Windows-service verzamelt metrische gegevens voor prestaties in Azure. Gegevens van de service worden opgehaald uit verschillende bronnen. Sommige configuratiegegevens lokaal worden verzameld, en sommige metrische gegevens voor prestaties van Azure Diagnostics worden gelezen. Tellers voor opslag worden kunnen uw aanmelden op het abonnementsniveau opslag gebruikt.

Als u problemen oplossen met behulp van SAP-notitie [1999351] niet los het probleem, opnieuw de `Set-AzureRmVMAEMExtension` configuratiescript. Mogelijk moet u wacht een uur omdat storage analytics of diagnostische gegevens van prestatiemeteritems kunnen niet worden gemaakt, onmiddellijk nadat deze zijn ingeschakeld. Als het probleem zich blijft voordoen, opent u een bericht SAP klant ondersteuning op de component BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een virtuele Linux-machine.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure-prestatiemeteritems, worden niet weergegeven op alle

Metrische gegevens voor prestaties in Azure worden door een daemon verzameld. Als de daemon niet actief is, kunnen geen prestatiegegevens worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de extensie voor Azure verbeterde controle is leeg

###### <a name="issue"></a>Probleem

De map \\var\\lib\\waagent\\ heeft geen een submap voor de extensie voor Azure verbeterde controle.

###### <a name="solution"></a>Oplossing

De extensie is niet geïnstalleerd. Bepalen of dit een probleem met de proxy is (zoals eerder beschreven). Mogelijk moet u start de computer opnieuw op en/of opnieuw de `Set-AzureRmVMAEMExtension` configuratiescript.

##### <a name="the-execution-of-set-azurermvmaemextension-and-test-azurermvmaemextension-show-warning-messages-stating-that-standard-managed-disks-are-not-supported"></a>De uitvoering van Set-AzureRmVMAEMExtension en Test-AzureRmVMAEMExtension waarschuwingsberichten Standard Managed Disks worden niet ondersteund met de mededeling weergeven

###### <a name="issue"></a>Probleem

Wanneer uitvoeren Set-AzureRmVMAEMExtension of Test-AzureRmVMAEMExtension zoals deze berichten worden weergegeven:

<pre><code>
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
WARNING: [WARN] Standard Managed Disks are not supported. Extension will be installed but no disk metrics will be available.
</code></pre>

U kunt een resultaat dat is die een niet-status in orde aangeeft azperfli.exe uitvoeren, zoals eerder beschreven ophalen. 

###### <a name="solution"></a>Oplossing

De berichten worden veroorzaakt door het feit dat de API's gebruikt door de controle-extensie om te controleren op statistieken van de standaard Azure Storage-Accounts zijn niet leveren van Standard Managed Disks. Dit is niet een kwestie van belang. Reden voor de introductie van de bewaking voor Standard Disk-opslag-accounts is beperking van i/o's die vaak zijn opgetreden. Beheerde schijven zal voorkomen dat dergelijke beperking door het aantal schijven in een storage-account te beperken. Omdat u niet hoeft dit type gegevens te controleren is daarom niet kritiek.


#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Er ontbreken enkele Azure-prestatiemeteritems

Metrische gegevens voor prestaties in Azure worden door een daemon Hiermee haalt u gegevens uit verschillende bronnen verzameld. Sommige configuratiegegevens lokaal worden verzameld, en sommige metrische gegevens voor prestaties van Azure Diagnostics worden gelezen. Tellers voor opslag zijn afkomstig van de logboeken in uw storage-abonnement.

Zie voor een volledige en bijgewerkte lijst van bekende problemen, SAP-notitie [1999351], die bevat aanvullende informatie over probleemoplossing voor uitgebreide Azure-bewaking voor SAP.

Als u problemen oplossen met behulp van SAP-notitie [1999351] niet los het probleem, opnieuw de `Set-AzureRmVMAEMExtension` configuratiescript zoals beschreven in [configureren van de Azure uitgebreide controle-extensie voor SAP] [deployment-guide-4.5]. Mogelijk moet u wachten op een uur omdat storage analytics of diagnostische gegevens van prestatiemeteritems kunnen niet worden gemaakt, onmiddellijk nadat deze zijn ingeschakeld. Als het probleem zich blijft voordoen, opent u een bericht SAP klant ondersteuning op de component BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een virtuele Linux-machine.
