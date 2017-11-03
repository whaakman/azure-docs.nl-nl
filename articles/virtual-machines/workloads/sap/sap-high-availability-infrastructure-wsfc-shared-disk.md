---
title: Voorbereiding van het Azure-infrastructuur voor SAP HA met behulp van Windows-failovercluster en de gedeelde schijf voor SAP-(A) exemplaar SCS | Microsoft Docs
description: De voorbereiding van het Azure-infrastructuur voor SAP HA met behulp van gedeelde schijf en Windows Failover Cluster voor SAP (A) SCS exemplaar
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: ec976257-396b-42a0-8ea1-01c97f820fa6
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 16c5a2ccfb27b87ba76aa0390ca6c57d2885e43d
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="azure-infrastructure-preparation-for-sap-ha-using-windows-failover-cluster-and-shared-disk-for-sap-ascs-instance"></a>Voorbereiding van het Azure-infrastructuur voor SAP HA met behulp van Windows-failovercluster en de gedeelde schijf voor SAP (A) SCS exemplaar

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides
[tuning-failover-cluster-network-thresholds]:https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f


[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md


> ![Windows][Logo_Windows] Windows
>

Dit document is met een beschrijving van Azure-infrastructuur voorbereidende stappen die nodig zijn voor het installeren en configureren van hoge beschikbaar SAP-systeem op **Windows Failover Cluster (WSFC)**met **gedeelde clusterschijf** als een optie voor clustering SCS SAP (A)-exemplaar.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat deze documenten controleren voordat u begint met de installatie:

* [Handleiding voor de referentiearchitectuur - Clustering SAP (A) SCS exemplaar op **Windows Failover Cluster** met **gedeelde clusterschijf**][sap-high-availability-guide-wsfc-shared-disk]

* [De voorbereiding van het Azure-infrastructuur voor SAP HA met behulp van **Windows Failover Cluster** en **gedeelde schijf** voor **SAP (A) SCS** exemplaar][sap-high-availability-infrastructure-wsfc-shared-disk]

## <a name="prepare-the-infrastructure-for-architectural-template-1"></a>De infrastructuur voorbereiden voor architecturale sjabloon 1
Azure Resource Manager-sjablonen voor SAP vereenvoudigen implementatie van de vereiste resources.

De drie lagen sjablonen in Azure Resource Manager ondersteunen ook scenario's met hoge beschikbaarheid, zoals in de architectuur sjabloon 1, met twee clusters. Elk cluster is een SAP storingspunt voor SAP ASC's / SCS en DBMS.

Dit is waarop u Azure Resource Manager-sjablonen kunt ophalen voor het voorbeeldscenario dat in dit artikel worden beschreven:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Azure Marketplace-installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Aangepaste installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

De infrastructuur voorbereiden voor architecturale sjabloon 1:

- In de Azure-portal op de **Parameters** blade in de **SYSTEMAVAILABILITY** de optie **HA**.

  ![Afbeelding 1: Stel SAP-parameters voor hoge beschikbaarheid Azure Resource Manager][sap-ha-guide-figure-3000]

_**Afbeelding 1:** ingesteld SAP-parameters voor hoge beschikbaarheid Azure Resource Manager_


  De sjablonen maken:

  * **Virtuele machines**:
    * SAP-toepassingsserver virtuele machines: <*SAPSystemSID*> - di - <*getal*>
    * ASC's / SCS cluster virtuele machines: <*SAPSystemSID*> - ASC - 's <*getal*>
    * DBMS-cluster: <*SAPSystemSID*> - db - <*getal*>

  * **Netwerkkaarten voor alle virtuele machines met bijbehorende IP-adressen**:
    * <*SAPSystemSID*> - nic - di - <*getal*>
    * <*SAPSystemSID*> - nic - ASC's - <*getal*>
    * <*SAPSystemSID*> - nic - db - <*getal*>

  * **Azure storage-accounts (alleen niet-beheerde schijven)**

  * **Beschikbaarheidsgroepen** voor:
    * SAP-toepassingsserver virtuele machines: <*SAPSystemSID*> - avset - di
    * SAP ASC's / SCS cluster virtuele machines: <*SAPSystemSID*> - avset - ASC's
    * DBMS cluster virtuele machines: <*SAPSystemSID*> - avset - db

  * **Azure interne load balancer**:
    * Met alle poorten voor de ASC's / SCS-instance en IP-adres <*SAPSystemSID*> - lb - ASC's
    * Met alle poorten voor het SQL Server-DBMS en IP-adres <*SAPSystemSID*> - lb - db

  * **Netwerkbeveiligingsgroep**: <*SAPSystemSID*> - nsg - ASC's-0  
    * Met een open externe Remote Desktop Protocol (RDP)-poort naar de <*SAPSystemSID*> - ASC's - 0 virtuele machine

> [!NOTE]
> Alle IP-adressen van de netwerkkaarten en Azure interne load balancers zijn **dynamische** standaard. Deze te wijzigen **statische** IP-adressen. Hoe u dit doet later in dit artikel worden beschreven.
>
>

## <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Virtuele Machines implementeren met de zakelijke netwerkverbinding (Cross-Premises) moet worden gebruikt in productie
Voor productiesystemen SAP, Azure virtuele machines implementeren met [bedrijfsnetwerk connectiviteit (cross-premises)] [ planning-guide-2.2] met behulp van Azure Site-naar-Site VPN- of Azure ExpressRoute.

> [!NOTE]
> U kunt uw Azure Virtual Network-exemplaar gebruiken. Het virtuele netwerk en subnet zijn al gemaakt en voorbereid.
>
>

1.  In de Azure-portal op de **Parameters** blade in de **NEWOREXISTINGSUBNET** de optie **bestaande**.
2.  In de **SUBNETID** vak, voegt u de volledige tekenreeks van het voorbereide Azure netwerk SubnetID waaruit u plant voor het implementeren van uw virtuele machines in Azure.
3.  Als u een lijst met alle subnetten die Azure-netwerk, moet u deze PowerShell-opdracht uitvoeren:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  De **ID** veld bevat de **SUBNETID**.
4. Voor een lijst van alle **SUBNETID** waarden, deze PowerShell-opdracht uitvoeren:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  De **SUBNETID** ziet er als volgt:

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

## <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Alleen in de Cloud SAP-instanties implementeert voor testen en Demo
U kunt uw SAP-systeem voor hoge beschikbaarheid in een cloudconfiguratie implementatiemodel implementeren. Dit type implementatie is vooral nuttig voor demo en test gebruiksvoorbeelden. Het niet geschikt voor gebruiksvoorbeelden voor productie.

- In de Azure-portal op de **Parameters** blade in de **NEWOREXISTINGSUBNET** de optie **nieuwe**. Laat de **SUBNETID** veld leeg.

  De SAP Azure Resource Manager-sjabloon maakt automatisch virtuele Azure-netwerk en subnet.

> [!NOTE]
> Ook moet u ten minste één toegewezen virtuele machine voor Active Directory en DNS implementeren in hetzelfde exemplaar van Azure Virtual Network. De sjabloon maken niet deze virtuele machines.
>
>


## <a name="prepare-the-infrastructure-for-architectural-template-2"></a>De infrastructuur voorbereiden voor architecturale sjabloon 2

U kunt deze Azure Resource Manager-sjabloon voor SAP te vereenvoudigen, de implementatie van resources van de vereiste infrastructuur voor SAP architectuur sjabloon 2.

Dit is waarop u Azure Resource Manager-sjablonen kunt ophalen voor deze implementatiescenario:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Azure Marketplace-installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Aangepaste installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


## <a name="prepare-the-infrastructure-for-architectural-template-3"></a>De infrastructuur voorbereiden voor architecturale sjabloon 3

U kunt de infrastructuur voorbereiden en configureren van SAP voor **multi-SID**. U kunt bijvoorbeeld een extra exemplaar SAP ASC's / SCS in toevoegen een *bestaande* clusterconfiguratie. Zie voor meer informatie [configureren van een extra exemplaar SAP ASC's / SCS in een bestaande configuratie van het cluster te maken van de configuratie van een SAP-multi-SID in Azure Resource Manager][sap-ha-multi-sid-guide].

Als u maken van een nieuw cluster met meerdere SID wilt, kunt u de multi-SID [Quick Start-sjablonen op GitHub](https://github.com/Azure/azure-quickstart-templates).

Voor het maken van een nieuw cluster met meerdere SID, moet u de volgende drie sjablonen te implementeren:

* [ASC's / SCS-sjabloon](#ASCS-SCS-template)
* [Database-sjabloon](#database-template)
* [Toepassingssjabloon voor servers](#application-servers-template)

De volgende secties hebben meer informatie over de sjablonen en de parameters die u moet opgeven in de sjablonen.

### <a name="ASCS-SCS-template"></a>ASC's / SCS-sjabloon

De sjabloon ASC's / SCS implementeert twee virtuele machines die u gebruiken kunt voor het maken van een failover-cluster van Windows Server die als host fungeert voor meerdere exemplaren van ASC's / SCS.

Voor het instellen van de sjabloon ASC's / SCS-multi-SID in de [ASC's / SCS multi-SID sjabloon] [ sap-templates-3-tier-multisid-xscs-marketplace-image] of [ASC's / SCS multi-SID-sjabloon met gebruik van schijven beheerd][sap-templates-3-tier-multisid-xscs-marketplace-image-md], voer waarden in voor de volgende parameters:

  - **Resource-voorvoegsel**.  Stel het resource-voorvoegsel dat wordt gebruikt als voorvoegsel voor alle resources die zijn gemaakt tijdens de implementatie. Omdat de resources niet tot slechts één SAP-systeem behoren, wordt het voorvoegsel van de resource is niet de SID van een SAP-systeem.  Het voorvoegsel moet liggen tussen **drie tot zes tekens**.
  - **Stack is Type**. Selecteer het type van de stack van het SAP-systeem. Afhankelijk van het type stack heeft Azure Load Balancer een (ABAP of alleen Java) of twee (ABAP + Java) privé IP-adressen per SAP-systeem.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **SAP System aantal**. Selecteer het aantal SAP-systemen die u wilt installeren in dit cluster.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam en het beheerder beheerderswachtwoord**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Nieuwe of bestaande Subnet**. Instellen of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
  -  **Subnet-Id**. Stel de ID van het subnet waarop de virtuele machines moet worden verbonden. Selecteer het subnet van uw virtueel particulier netwerk (VPN) of ExpressRoute virtueel netwerk verbinding maken met de virtuele machine van uw on-premises netwerk. De ID ziet er meestal als volgt uit:

   /Subscriptions/ <*abonnements-id*> /resourceGroups/ <*Resourcegroepnaam*> /providers/Microsoft.Network/virtualNetworks/ <*virtuele-netwerknaam*> /subnets/ <*subnetnaam*>

De sjabloon implementeert één Load Balancer van Azure-instantie, die ondersteuning biedt voor meerdere SAP-systemen.

- De exemplaren ASC's worden geconfigureerd voor exemplaarnummer 00, 10, 20...
- De SCS-exemplaren zijn geconfigureerd voor exemplaarnummer 01, 11, 21...
- De exemplaren ASC's in de wachtrij plaatsen replicatie Server (gebruikers) (alleen voor Linux) zijn geconfigureerd voor exemplaarnummer 02, 12, 22...
- De exemplaren SCS ERS (alleen voor Linux) zijn geconfigureerd voor exemplaarnummer 03, 13, 23...

De load balancer bevat 1 (2 voor Linux) VIP(s), 1 x-VIP voor ASC's / SCS en 1 x-VIP voor Ebruikers (alleen voor Linux).

#### <a name="0f3ee255-b31e-4b8a-a95a-d9ed6200468b"></a>SAP ASC's / SCS poorten
De volgende lijst bevat alle load-balancingregels (waarbij x staat voor het nummer van het SAP-systeem, bijvoorbeeld 1, 2, 3...):
- Windows-specifieke poorten voor elke SAP-systeem: 445, 5985
- ASC's poorten (exemplaarnummer x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- SCS-poorten (exemplaarnummer x1): 32 x 1 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- ASCS ERS poorten op Linux (exemplaarnummer x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216
- SCS ERS poorten op Linux (exemplaarnummer x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316

De load balancer is geconfigureerd voor gebruik van de volgende test-poorten (waarbij x staat voor het nummer van het SAP-systeem, bijvoorbeeld 1, 2, 3...):
- ASC's / SCS interne load balancer-testpoort: 620 0 x
- Ebruikers interne load balancer-testpoort (alleen voor Linux): 621 x 2

### <a name="database-template"></a>Database-sjabloon

De databasesjabloon implementeert een of twee virtuele machines die u gebruiken kunt voor het installeren van het relationele databasebeheersysteem (RDBMS) voor een SAP-systeem. Bijvoorbeeld, als u een sjabloon ASC's / SCS voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van het databasesjabloon multi-SID in de [multi-SID databasesjabloon] [ sap-templates-3-tier-multisid-db-marketplace-image] of [multi-SID databasesjabloon beheerd schijven][sap-templates-3-tier-multisid-db-marketplace-image-md], voer waarden in voor de volgende parameters:

  -  **SAP-Id van het systeem**. Voer de SAP-systeem-ID van het SAP-systeem die u wilt installeren. De ID is worden gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **DbType**. Selecteer het type van de database die u wilt installeren op het cluster. Selecteer **SQL** als u wilt installeren van Microsoft SQL Server. Selecteer **HANA** als u van plan bent een SAP HANA installeren op de virtuele machines. Zorg ervoor dat het juiste besturingssysteem-type selecteren: Selecteer **Windows** voor SQL, en selecteer een Linux-distributiepunt voor HANA. De Azure Load Balancer die is verbonden met de virtuele machines is worden geconfigureerd ter ondersteuning van het type van de geselecteerde database:
    * **SQL**. De load balancer verdelen poort 1433. Zorg ervoor dat deze poort gebruiken voor uw SQL Server Always On.
    * **HANA**. De load balancer verdelen-poorten 35015 en 35017. Zorg dat u SAP HANA met exemplaarnummer **50**.
    De testpoort 62550 maakt gebruik van de load balancer.
  -  **Grootte van het SAP**. Stel het aantal SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam en het beheerder beheerderswachtwoord**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**. Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de sjabloon ASC's / SCS of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASC's / SCS.

### <a name="application-servers-template"></a>Toepassingssjabloon voor Servers

De servers toepassingssjabloon implementeert twee of meer virtuele machines die kan worden gebruikt als SAP-toepassingsserver exemplaren voor een SAP-systeem. Bijvoorbeeld, als u een sjabloon ASC's / SCS voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van de sjabloon toepassingen servers multi-SID in de [toepassingssjabloon servers multi-SID] [ sap-templates-3-tier-multisid-apps-marketplace-image] of [servers multi-SID toepassingssjabloon beheerd schijven][sap-templates-3-tier-multisid-apps-marketplace-image-md], voer waarden in voor de volgende parameters:

  -  **SAP-Id van het systeem**. Voer de SAP-systeem-ID van het SAP-systeem die u wilt installeren. De ID is worden gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **Grootte van het SAP**. Het aantal SAP's het nieuwe systeem biedt. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam en het beheerder beheerderswachtwoord**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**. Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de sjabloon ASC's / SCS of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASC's / SCS.


## <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuele Azure-netwerk
In ons voorbeeld is de adresruimte van het Azure-netwerk 10.0.0.0/16. Er is één subnet met de naam **Subnet**, met een adresbereik van 10.0.0.0/24. Alle virtuele machines en interne load balancers worden geïmplementeerd in dit virtuele netwerk.

> [!IMPORTANT]
> Niet alle wijzigingen aanbrengen in de netwerkinstellingen in het gastbesturingssysteem. Dit omvat het IP-adressen, DNS-servers en subnet. De netwerkinstellingen configureren in Azure. De service Dynamic Host Configuration Protocol (DHCP) geeft uw instellingen.
>
>

## <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS-IP-adressen

Voer de volgende stappen uit te stellen de vereiste DNS IP adressen:

1.  In de Azure-portal op de **DNS-servers** blade, zorg ervoor dat het virtuele netwerk **DNS-servers** optie is ingesteld op **aangepaste DNS**.
2.  Selecteer uw instellingen op basis van het type netwerk die u hebben. Zie de volgende bronnen voor meer informatie:
    * [Zakelijke netwerkverbinding (cross-premises)][planning-guide-2.2]: de IP-adressen van de lokale DNS-servers toevoegen.  
    U kunt de lokale DNS-servers aan de virtuele machines die worden uitgevoerd in Azure uitbreiden. In dit scenario, kunt u de IP-adressen van de Azure virtuele machines waarop u de DNS-service uitvoert toevoegen.
    * [Cloud-implementatie][planning-guide-2.1]: een extra virtuele machine implementeren in hetzelfde virtuele netwerk-exemplaar dat als een DNS-server fungeert. Voeg de IP-adressen van de Azure virtuele machines die u hebt ingesteld DNS-service uit te voeren.

    ![Afbeelding 2: DNS-servers configureren voor Azure Virtual Network][sap-ha-guide-figure-3001]

    _**Afbeelding 2:** configureren DNS-servers voor Azure Virtual Network_

  > [!NOTE]
  > Als u de IP-adressen van de DNS-servers wijzigt, moet u de Azure virtuele machines voor het toepassen van de wijziging en het doorgeven van de nieuwe DNS-servers opnieuw opstarten.
  >
  >

In ons voorbeeld is de DNS-service geïnstalleerd en geconfigureerd op deze virtuele machines van Windows:

| De rol virtuele machine | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste DNS-server |domcontr 0 |PR1-nic-domcontr-0 |10.0.0.10 |
| Tweede DNS-server |domcontr-1 |PR1-nic-domcontr-1 |10.0.0.11 |

## <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Hostnamen en statische IP-adressen voor de SAP ASC's / SCS geclusterde exemplaar en DBMS geclusterd exemplaar

Voor on-premises implementatie moet u deze gereserveerde hostnamen en IP-adressen:

| Virtuele host naam rol | De naam van de virtuele host | Virtuele vaste IP-adres |
| --- | --- | --- |
| SAP ASC's / SCS eerste virtuele host clusternaam (voor cluster) |PR1-ASC's-vir |10.0.0.42 |
| Naam van de virtuele host op het exemplaar van SAP ASC's / SCS |PR1 ASC's sap |10.0.0.43 |
| SAP DBMS tweede virtuele host clusternaam (cluster management) |PR1-dbms-vir |10.0.0.32 |

Wanneer u het cluster maakt, maakt u de virtuele hostnamen **pr1-ASC's-vir** en **pr1-dbms-vir** en de bijbehorende IP-adressen die het cluster zelf beheren. Zie voor informatie over hoe u dit doet, [verzamelen clusterknooppunten in een clusterconfiguratie][sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config].

U kunt handmatig maken met de andere twee virtuele hostnamen, **pr1 ASC's sap** en **pr1 dbms sap**, en de bijbehorende IP-adressen op de DNS-server. Het geclusterde exemplaar van het SAP ASC's / SCS en het geclusterde exemplaar van de DBMS deze resources gebruiken. Zie voor informatie over hoe u dit doet, [maken een naam van de virtuele host op voor een geclusterd exemplaar van de SAP ASC's / SCS] [sap-ha-handleiding-9.1.1].

## <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Statische IP-adressen instellen voor de SAP virtuele Machines
Nadat u de virtuele machines te gebruiken in uw cluster implementeert, moet u statische IP-adressen voor alle virtuele machines instellen. Dit doen in de Azure Virtual Network-configuratie en niet in het gastbesturingssysteem.

1.  Selecteer in de Azure-portal **resourcegroep** > **netwerkkaart** > **instellingen** > **IP-adres**.
2.  Op de **IP-adressen** blade onder **toewijzing**, selecteer **statische**. In de **IP-adres** Voer het IP-adres dat u wilt gebruiken.

  > [!NOTE]
  > Als u het IP-adres van de netwerkkaart wijzigt, moet u opnieuw opstarten van de virtuele machines in Azure als de wijziging wilt toepassen.  
  >
  >

  ![Afbeelding 3: De statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen][sap-ha-guide-figure-3002]

  _**Afbeelding 3:** statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen_

  Herhaal deze stap voor alle netwerkinterfaces, is voor alle virtuele machines, met inbegrip van virtuele machines die u wilt gebruiken voor uw Active Directory en DNS-service.

In ons voorbeeld hebben we deze virtuele machines en statische IP-adressen:

| De rol virtuele machine | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste SAP Application Server-exemplaar |PR1-di-0 |PR1-nic-di-0 |10.0.0.50 |
| Tweede SAP Application Server-exemplaar |PR1-di-1 |PR1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Laatste SAP Application Server-exemplaar |PR1-di-5 |PR1-nic-di-5 |10.0.0.55 |
| Eerste clusterknooppunt voor ASC's / SCS-exemplaar |PR1-ASC's-0 |PR1-nic-ASC's-0 |10.0.0.40 |
| Tweede clusterknooppunt voor ASC's / SCS-exemplaar |PR1-ASC's-1 |PR1-nic-ASC's-1 |10.0.0.41 |
| Eerste clusterknooppunt voor DBMS-exemplaar |PR1-db-0 |PR1-nic-db-0 |10.0.0.30 |
| Tweede clusterknooppunt voor DBMS-exemplaar |PR1-db-1 |PR1-nic-db-1 |10.0.0.31 |

## <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Instellen van een statisch IP-adres voor de Azure interne Load Balancer

De SAP Azure Resource Manager-sjabloon maakt u een Azure interne load balancer die wordt gebruikt voor het SAP ASC's / SCS exemplaar en de DBMS-cluster.

> [!IMPORTANT]
> Het IP-adres van de virtuele host-naam van de SAP ASC's / SCS is hetzelfde als het IP-adres van de SAP ASC's / SCS interne load balancer: **pr1-lb-ASC's**.
> Het IP-adres van de virtuele naam van de DBMS is hetzelfde als het IP-adres van de DBMS interne load balancer: **pr1-lb-dbms**.
>
>

Een statisch IP-adres voor de Azure interne load balancer instellen:

1.  De eerste implementatie het IP-adres van de interne load balancer wordt ingesteld op **dynamische**. In de Azure-portal op de **IP-adressen** blade onder **toewijzing**, selecteer **statische**.
2.  Stel het IP-adres van de interne load balancer **pr1-lb-ASC's** het IP-adres van de virtuele host-naam van het SAP ASC's / SCS-exemplaar.
3.  Stel het IP-adres van de interne load balancer **pr1-lb-dbms** het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

  ![Afbeelding 4: Statische IP-adressen voor de interne load balancer voor de SAP ASC's / SCS-instantie instellen][sap-ha-guide-figure-3003]

  _**Afbeelding 4:** statische IP-adressen voor de interne load balancer voor de SAP ASC's / SCS-instantie instellen_

In ons voorbeeld hebben we twee Azure interne load balancers die deze statische IP-adressen:

| Azure interne load balancer-rol | Naam Azure interne load balancer | Statisch IP-adres |
| --- | --- | --- |
| SAP ASC's / SCS exemplaar interne load balancer |PR1-lb-ASC 's |10.0.0.43 |
| SAP DBMS interne load balancer |PR1-lb-dbms |10.0.0.33 |


## <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Standaard ASC's / SCS Taakverdelingsregels voor de Azure interne Load Balancer

De SAP Azure Resource Manager-sjabloon maakt u de poorten die u nodig:
* Een exemplaar ABAP ASCS met het standaardaantal exemplaar **00**
* Een Java-SCS-exemplaar, met het standaardaantal exemplaar **01**

Wanneer u uw exemplaar SAP ASC's / SCS installeert, moet u het standaardaantal exemplaar **00** voor uw ABAP ASCS-exemplaar en het exemplaarnummer standaard **01** voor uw Java-SCS-exemplaar.

Maak vervolgens vereist voor de interne load balancer eindpunten voor de SAP NetWeaver-poorten.

Maak eerst deze eindpunten voor de poorten SAP NetWeaver ABAP ASC's voor taakverdeling voor het maken van de vereiste voor de interne load balancer eindpunten:

| Service/load balancing regelnaam | Standaardpoortnummers | Concrete poorten voor (ASC's exemplaar met exemplaarnummer 00) (uit met 10) |
| --- | --- | --- |
| In de wachtrij plaatsen Server / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| ABAP berichtenserver / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| Interne ABAP bericht / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| Server-HTTP-bericht / *Lbrule8100* |81 <*InstanceNumber*> |8100 |
| SAP Start Service ASC's HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP Start Service ASC's HTTPS / *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| Replicatie van de wachtrij plaatsen / *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP Start Service Ebruikers HTTP *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP Start Service Ebruikers HTTP *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| Win RM *Lbrule5985* | |5985 |
| Bestandsshare *Lbrule445* | |445 |

**Tabel 1:** poortnummers van de exemplaren SAP NetWeaver ABAP ASC's

Vervolgens maakt u deze load balancing-eindpunten voor de SAP NetWeaver Java SCS-poorten:

| Service/load balancing regelnaam | Standaardpoortnummers | Concrete poorten voor (exemplaar met exemplaarnummer 01 SCS) (Ebruikers met 11) |
| --- | --- | --- |
| In de wachtrij plaatsen Server / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Gateway-Server / *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Java berichtenserver / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| Server-HTTP-bericht / *Lbrule8101* |81 <*InstanceNumber*> |8101 |
| SAP Start Service SCS HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP Start Service SCS HTTPS / *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| Replicatie van de wachtrij plaatsen / *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP Start Service Ebruikers HTTP *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP Start Service Ebruikers HTTP *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| Win RM *Lbrule5985* | |5985 |
| Bestandsshare *Lbrule445* | |445 |

**Tabel 2:** poortnummers van de SAP NetWeaver Java SCS-exemplaren

![Afbeelding 5: Standaard ASC's / SCS taakverdelingsregels voor de Azure interne load balancer][sap-ha-guide-figure-3004]

_**Afbeelding 5:** standaard ASC's / SCS load-balancingregels voor de Azure interne load balancer_

Stel het IP-adres van de load balancer **pr1-lb-dbms** het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>De regels van ASC's / SCS standaard taakverdeling voor de Azure interne Load Balancer wijzigen

Als u gebruiken verschillende aantallen voor de SAP ASC's of SCS exemplaren wilt, moet u de namen en waarden van de poorten wijzigen van standaardwaarden.

1.  Selecteer in de Azure-portal  **<* SID*> - lb - ASC's load balancer ** > **Load Balancing regels**.
2.  Voor alle load-balancingregels die deel uitmaken van het SAP ASC's of SCS exemplaar, deze waarden te wijzigen:

  * Naam
  * Poort
  * Back-end-poort

  Bijvoorbeeld, als u wijzigen van het standaardaantal ASC's in het exemplaar van 00 tot en met 31 wilt, moet u om de wijzigingen voor alle poorten die worden vermeld in tabel 1.

  Hier volgt een voorbeeld van een update voor poort *lbrule3200*.

  ![Afbeelding 6: De ASC's / SCS standaard taakverdelingsregels voor de Azure interne load balancer wijzigen][sap-ha-guide-figure-3005]

  _**Afbeelding 6:** ASC's / SCS standaard Netwerktaakverdeling regels voor de Azure interne load balancer wijzigen_

## <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuele Machines toevoegen aan het domein

Nadat u een statisch IP-adres aan de virtuele machines toewijzen, moet u de virtuele machines toevoegen aan het domein.

![Afbeelding 7: Een virtuele machine toevoegen aan een domein][sap-ha-guide-figure-3006]

_**Afbeelding 7:** een virtuele machine toevoegen aan een domein_

## <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Registervermeldingen op beide knooppunten van het SAP ASC's / SCS-exemplaar toe te voegen

Azure Load Balancer heeft een interne load balancer die wordt gesloten verbindingen als de verbindingen niet actief gedurende een bepaalde zijn time (niet-actieve time-out). SAP processen in het dialoogvenster exemplaren open verbinding naar de wachtrij plaatsen van het SAP verwerken zodra het eerste in de wachtrij plaatsen/wachtrij aanvragen moet worden verzonden. Deze verbindingen meestal blijven tot stand gebrachte totdat het proces of het proces in de wachtrij plaatsen opnieuw wordt opgestart. Als de verbinding actief gedurende een bepaalde tijd is, sluit de Azure interne load balancer de verbindingen. Dit geen probleem omdat het werkproces SAP herstelt u de verbinding met het proces in de wachtrij plaatsen als deze niet meer bestaat. Deze activiteiten worden beschreven in de developer-traces van SAP-processen, maar ze een grote hoeveelheid extra inhoud in deze traceringen maken. Het is een goed idee om te wijzigen van de TCP/IP `KeepAliveTime` en `KeepAliveInterval` op beide clusterknooppunten. Deze wijzigingen in de TCP/IP-parameters met SAP-profiel, beschreven parameters later in dit artikel worden gecombineerd.

Registervermeldingen toegevoegd op beide knooppunten van het SAP ASC's / SCS-exemplaar, Voeg eerst deze Windows-register-item op beide clusterknooppunten Windows voor SAP ASC's / SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam variabele |`KeepAliveTime` |
| Type variabele |REG_DWORD (decimaal) |
| Waarde |120000 |
| Koppelen aan documentatie |[https://technet.Microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

**Tabel 3:** wijzigen van de eerste parameter van de TCP/IP

Voegt u deze Windows-registervermeldingen op beide clusterknooppunten Windows voor SAP ASC's / SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam variabele |`KeepAliveInterval` |
| Type variabele |REG_DWORD (decimaal) |
| Waarde |120000 |
| Koppelen aan documentatie |[https://technet.Microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

**Tabel 4:** wijzigen van de tweede parameter van de TCP/IP

**De wijzigingen wilt toepassen, start opnieuw op beide clusterknooppunten**.

## <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Een Windows Server Failover Clustering-Cluster instellen voor een SAP ASC's / SCS-exemplaar

Instellen van een cluster met Windows Server Failover Clustering voor een SAP ASC's / SCS-exemplaar, moet deze taken uitvoeren:

- Verzamelen van de clusterknooppunten in een clusterconfiguratie
- Een cluster bestandsshare-witness configureren

### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Verzamelen van de clusterknooppunten in een clusterconfiguratie

1.  In de functie toevoegen en de Wizard Functies toevoegen Failoverclustering aan beide clusterknooppunten.
2.  Het failover-cluster instellen met behulp van Failoverclusterbeheer. Selecteer in Failoverclusterbeheer **Cluster maken**, en vervolgens alleen de naam van het eerste knooppunt A.-cluster toevoegen Voeg het tweede knooppunt nog; u kunt het tweede knooppunt toevoegen in een later stadium.

  ![Afbeelding 8: De server of de virtuele machine-naam van het eerste clusterknooppunt toevoegen][sap-ha-guide-figure-3007]

  _**Afbeelding 8:** de server of de virtuele machine-naam van het eerste clusterknooppunt toevoegen_

3.  Voer de netwerknaam (virtuele host-naam) van het cluster.

  ![Afbeelding 9: Voer de naam van het cluster][sap-ha-guide-figure-3008]

  _**Afbeelding 9:** Voer de naam van het cluster_

4.  Nadat u het cluster hebt gemaakt, voert u een clustervalidatietest.

  ![Afbeelding 10: De validatiecontrole cluster uitvoeren][sap-ha-guide-figure-3009]

  _**Afbeelding 10:** de validatiecontrole cluster uitvoeren_

  U kunt waarschuwingen met betrekking tot schijven op dit moment in het proces negeren. U voegt dat een bestandssharewitness en de SIOS gedeelde schijven later. In deze fase hoeft u niet te hoeven maken over het hebben van een quorum.

  ![Afbeelding 11: Er is geen quorumschijf is gevonden.][sap-ha-guide-figure-3010]

  _**Afbeelding 11:** geen quorumschijf is gevonden_

  ![Afbeelding 12: Core cluster-bron moet een nieuw IP-adres][sap-ha-guide-figure-3011]

  _**Afbeelding 12:** Core cluster-bron moet een nieuw IP-adres_

5.  Wijzig het IP-adres van de clusterservice core. Het cluster kan totdat u het IP-adres van de clusterservice core wijzigt niet starten omdat het IP-adres van de server naar een van de knooppunten van de virtuele machine verwijst. Dit doen op de **eigenschappen** pagina van de core-clusterservice IP-resource.

  Bijvoorbeeld, moet een IP-adres toewijzen (in ons voorbeeld **10.0.0.42**) voor de naam van het cluster virtuele host **pr1-ASC's-vir**.

  ![Afbeelding 13: Wijzig het IP-adres In het dialoogvenster Eigenschappen][sap-ha-guide-figure-3012]

  _**Afbeelding 13:** In de **eigenschappen** dialoogvenster Wijzig het IP-adres_

  ![Afbeelding 14: Wijs het IP-adres dat is gereserveerd voor het cluster][sap-ha-guide-figure-3013]

  _**Afbeelding 14:** toewijzen van het IP-adres dat is gereserveerd voor het cluster_

6.  Virtuele hostnaam van het cluster online brengen.

  ![Afbeelding 15: Core de clusterservice actief is en actief en met het juiste IP-adres][sap-ha-guide-figure-3014]

  _**Afbeelding 15:** core de clusterservice actief is en actief en met het juiste IP-adres_

7.  Het tweede knooppunt toevoegen.

  Nu de core cluster-service actief is, kunt u het tweede knooppunt toevoegen.

  ![Afbeelding van 16 het tweede knooppunt toevoegen][sap-ha-guide-figure-3015]

  _**Afbeelding 16:** het tweede knooppunt toevoegen_

8.  Voer een naam voor de tweede host van de cluster-knooppunt.

  ![Afbeelding 17: Voer de tweede hostnaam van de cluster-knooppunt][sap-ha-guide-figure-3016]

  _**Afbeelding 17:** invoeren van de tweede hostnaam van de cluster-knooppunt_

  > [!IMPORTANT]
  > Controleer of de **alle in aanmerking komende opslag toevoegen aan het cluster** selectievakje **niet** geselecteerde.  
  >
  >

  ![Afbeelding 18: Schakel het selectievakje niet][sap-ha-guide-figure-3017]

  _**Afbeelding 18:** doen **niet** Schakel het selectievakje_

  U kunt waarschuwingen over quorum en schijven negeren. U stelt het quorum en delen van de schijf later, zoals beschreven in [SIOS DataKeeper Cluster Edition installeren voor de clusterschijf share SAP ASC's / SCS][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].

  ![Afbeelding 19: Negeren van waarschuwingen over de schijf quorum][sap-ha-guide-figure-3018]

  _**Afbeelding 19:** negeren van waarschuwingen over de schijf quorum_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Een Cluster bestandsshare-Witness configureren

Een cluster bestandsshare-witness configureren, moet deze taken uitvoeren:

- Een bestandsshare maken
- Instellen van het bestand bestandsshare-witness quorum in Failoverclusterbeheer

#### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Een bestandsshare maken

1.  Selecteer een bestandssharewitness in plaats van een quorumschijf. SIOS DataKeeper ondersteunt deze optie.

  In de voorbeelden in dit artikel wordt de bestandsshare-witness is op de Active Directory en DNS-server die wordt uitgevoerd in Azure. De bestandsshare-witness wordt aangeroepen **domcontr 0**. Omdat u zou een VPN-verbinding naar Azure (via Site-naar-Site VPN- of Azure ExpressRoute) hebt geconfigureerd, kunt u met uw Active Directory en DNS-service on-premises en is niet geschikt is voor het uitvoeren van een bestand witness delen.

  > [!NOTE]
  > Als uw Active Directory en DNS-service wordt uitgevoerd alleen lokale, niet de bestandsshare-witness configureren op het Active Directory en DNS-Windows-besturingssysteem die lokaal wordt uitgevoerd. Netwerklatentie tussen clusterknooppunten uitgevoerd in Azure en Active Directory en DNS-on-premises mogelijk te groot zijn en leiden tot problemen met de netwerkverbinding. Zorg ervoor dat de bestandsshare-witness configureren op een virtuele machine van Azure waarop dicht bij het clusterknooppunt.  
  >
  >

  De schijf quorum moet ten minste 1024 MB vrije ruimte. We raden 2048 MB aan vrije ruimte voor het quorumstation aan.

2.  De clusternaamobject toevoegen.

  ![Afbeelding van 20: De machtigingen op de share voor het clusterobject naam toewijzen][sap-ha-guide-figure-3019]

  _**Afbeelding van 20:** de machtigingen op de share voor het clusterobject naam toewijzen_

  Zorg dat de machtigingen voor de autoriteit voor het wijzigen van gegevens in de share voor het clusterobject naam zijn (in ons voorbeeld **pr1-ASC's-vir$**).

3.  Selecteer om de clusternaamobject toe aan de lijst met **toevoegen**. Het filter om computerobjecten, naast die wordt weergegeven in de bovenste afbeelding te controleren of wijzigen.

  ![Afbeelding 21: De objecttypen die computers bijvoegt wijzigen][sap-ha-guide-figure-3020]

  _**Afbeelding 21:** de objecttypen die computers bijvoegt wijzigen_

  ![Afbeelding 22: Schakel het selectievakje Computers][sap-ha-guide-figure-3021]

  _**Afbeelding 22:** selecteert u de **Computers** selectievakje_

4.  Voer het clusternaamobject zoals in **afbeelding: Wijzig de objecttypen die computers bijvoegt**. Omdat de record al is gemaakt, kunt u de machtigingen, zoals wordt weergegeven in **afbeelding: de machtigingen op de share voor het clusterobject naam toewijzen**.

5.  Selecteer de **beveiliging** tabblad van de share- en vervolgens moet u meer gedetailleerde machtigingen voor het clusterobject naam.

  ![Afbeelding 23: De kenmerken voor beveiliging voor het clusterobject naam ingesteld op het bestandsshare quorum][sap-ha-guide-figure-3022]

  _**Afbeelding 23:** beveiligingskenmerken van voor de clusternaamobject instellen op het bestandsshare quorum_

#### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Het bestand bestandsshare-Witness Quorum in Failover Cluster Manager instellen

1.  Open de instelling-Wizard clusterquorum configureren.

  ![Afbeelding 24: Start de Wizard configureren clusterquorum-instelling][sap-ha-guide-figure-3023]

  _**Afbeelding 24:** de configureren instelling Wizard clusterquorum starten_

2.  Op de **quorumconfiguratie selecteren** pagina **selecteert u de quorumwitness**.

  ![Afbeelding 25: Quorumconfiguraties u kunt kiezen uit][sap-ha-guide-figure-3024]

  _**Afbeelding 25:** quorumconfiguraties kunt u kiezen uit_

3.  Op de **Quorumwitness selecteren** pagina **een bestandssharewitness configureren**.

  ![Afbeelding 26: Selecteer de bestandsshare-witness][sap-ha-guide-figure-3025]

  _**Afbeelding 26:** selecteert u de bestandsshare-witness_

4.  Geef het UNC-pad naar de bestandsshare (in ons voorbeeld \\domcontr 0\FSW). Een overzicht van de wijzigingen die u kunt maken, selecteer **volgende**.

  ![Afbeelding 27: Definieer de bestandslocatie van de share voor de witness-share][sap-ha-guide-figure-3026]

  _**Afbeelding 27:** definiëren de bestandslocatie van de share voor de witness-share_

5.  Selecteer de gewenste wijzigingen aan, en selecteer vervolgens **volgende**. U moet de configuratie van het cluster is configureren zoals wordt weergegeven:  

  ![Afbeelding 28: Bevestiging dat u het cluster opnieuw hebt geconfigureerd][sap-ha-guide-figure-3027]

  _**Afbeelding 28:** bevestiging dat u het cluster opnieuw hebt geconfigureerd_

Nadat het failovercluster van Windows is geïnstalleerd, moeten de wijzigingen worden aangebracht aan sommige drempelwaarden aan te passen failover detectie om voorwaarden in Azure. De parameters worden gewijzigd, worden beschreven in de blog [afstemmen Failover Cluster netwerk drempelwaarden][tuning-failover-cluster-network-thresholds]. Ervan uitgaande dat uw twee virtuele machines die de configuratie van het Windows-Cluster voor ASC's / SCS bouwen zich in hetzelfde SubNet bevinden, moeten de volgende parameters worden gewijzigd in deze waarden:

- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Deze instellingen zijn getest met klanten en een goede inbreuk robuuste de een-zijde worden opgegeven. Aan de andere kant zijn die instellingen bieden snel genoeg failover in een echte foutcondities op SAP-software of het knooppunt/VM-fout.

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>SIOS DataKeeper Cluster Edition voor de SAP ASC's / SCS Share clusterschijf installeren

U hebt nu een werkende configuratie van Windows Server Failover Clustering in Azure. Maar als u wilt installeren een SAP ASC's / SCS-exemplaar, moet u een gedeelde schijfresource. U kunt de gedeelde schijfresources, u moet in Azure maken. SIOS DataKeeper Cluster Edition is een oplossing van derden die u gebruiken kunt om resources van de gedeelde schijf te maken.

SIOS DataKeeper Cluster Edition voor de SAP ASC's / SCS share clusterschijf installeren, moet deze taken uitvoeren:

- Toevoegen van het .NET Framework 3.5
- SIOS DataKeeper installeren
- SIOS DataKeeper instellen

### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Het .NET Framework 3.5 toevoegen
Microsoft .NET Framework 3.5 is niet automatisch geactiveerd of Windows Server 2012 R2 is geïnstalleerd. Omdat SIOS DataKeeper .NET Framework vereist op alle knooppunten die u op DataKeeper installeert, moet u .NET Framework 3.5 installeren op het gastbesturingssysteem van alle virtuele machines in het cluster.

Er zijn twee manieren om toe te voegen .NET Framework 3.5:

- Gebruik de toevoegen Wizard functies en onderdelen in Windows zoals wordt weergegeven:

  ![Afbeelding 29: .NET Framework 3.5 installeren met behulp van de functies en onderdelen][sap-ha-guide-figure-3028]

  _**Afbeelding 29:** .NET Framework 3.5 installeren met behulp van de functies en onderdelen_

  ![Afbeelding 30: Installatie van de voortgangsbalk wanneer u de .NET Framework 3.5 installeren met behulp van de functies en onderdelen][sap-ha-guide-figure-3029]

  _**Afbeelding 30:** installatie van de voortgangsbalk wanneer u de .NET Framework 3.5 installeren met behulp van de functies en onderdelen_

- Gebruik het opdrachtregelprogramma dism.exe. Voor dit type installatie moet u toegang tot de SxS-map op de installatiemedia van Windows. Typ bij een opdrachtprompt met verhoogde bevoegdheid:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper installeren

Installeer SIOS DataKeeper Cluster Edition op elk knooppunt in het cluster. Als u wilt maken van virtuele gedeelde opslag met SIOS DataKeeper, maakt u een gesynchroniseerde mirror en vervolgens simuleren cluster gedeelde opslag.

Voordat u de software SIOS installeert, maakt u de domeingebruiker **DataKeeperSvc**.

> [!NOTE]
> Voeg de **DataKeeperSvc** gebruiker de **lokale beheerder** groep op beide clusterknooppunten.
>
>

SIOS DataKeeper installeren:

1.  De SIOS software installeren op beide clusterknooppunten.

  ![SIOS installatieprogramma][sap-ha-guide-figure-3030]

  ![31 afbeelding: Eerste pagina van de installatie SIOS DataKeeper][sap-ha-guide-figure-3031]

  _**Afbeelding 31:** eerste pagina van de installatie SIOS DataKeeper_

2.  Selecteer in het dialoogvenster **Ja**.

  ![Afbeelding 32: DataKeeper informeert u dat een service wordt uitgeschakeld][sap-ha-guide-figure-3032]

  _**Afbeelding 32:** DataKeeper informeert u dat een service wordt uitgeschakeld_

3.  In het dialoogvenster wordt aangeraden dat u selecteert **domein of de Server account**.

  ![Afbeelding 33: Selectie van de de gebruiker voor SIOS DataKeeper][sap-ha-guide-figure-3033]

  _**Afbeelding 33:** gebruikersselectie voor SIOS DataKeeper_

4.  Geef de accountnaam en domein en de wachtwoorden die u hebt gemaakt voor SIOS DataKeeper.

  ![Afbeelding 34: Voer de gebruikersnaam en het wachtwoord voor de installatie SIOS DataKeeper][sap-ha-guide-figure-3034]

  _**Afbeelding 34:** domeingebruikersnaam en wachtwoord invoeren voor de installatie SIOS DataKeeper_

5.  De licentiesleutel voor uw exemplaar SIOS DataKeeper installeren zoals wordt weergegeven in afbeelding 45.

  ![Afbeelding 35: Voer uw licentiecode SIOS DataKeeper][sap-ha-guide-figure-3035]

  _**Afbeelding 35:** uw licentiesleutel SIOS DataKeeper invoeren_

6.  Wanneer u wordt gevraagd, start u de virtuele machine opnieuw.

### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper instellen

Nadat u SIOS DataKeeper op beide knooppunten hebt geïnstalleerd, moet u de configuratie te starten. Het doel van de configuratie is dat synchrone replicatie tussen de extra schijven die zijn gekoppeld aan elk van de virtuele machines.

1.  Start het hulpprogramma DataKeeper beheer en de configuratie en selecteer vervolgens **verbinding maken met Server**. (deze optie is met een rode cirkel.)

  ![36 afbeelding: De SIOS DataKeeper beheer en het hulpprogramma voor serverconfiguratie][sap-ha-guide-figure-3036]

  _**Afbeelding 36:** SIOS DataKeeper beheer en de configuratie-hulpprogramma_

2.  Voer de naam of TCP/IP-adres van het eerste knooppunt dat het hulpprogramma voor beheer en de configuratie verbinding, en, in een tweede stap van het tweede knooppunt maken moet.

  ![Afbeelding 37: Voegt u de naam of TCP/IP-adres van het eerste knooppunt van het beheer en hulpprogramma voor serverconfiguratie verbinding moeten maken, en in een tweede stap van het tweede knooppunt][sap-ha-guide-figure-3037]

  _**Afbeelding 37:** invoegen van de naam of TCP/IP-adres van het eerste knooppunt het hulpprogramma voor beheer en de configuratie verbinding, en in een tweede stap van het tweede knooppunt maken moet_

3.  De taak van de replicatie tussen de twee knooppunten maken.

  ![Afbeelding 38: Een replicatie-taak maken][sap-ha-guide-figure-3038]

  _**Afbeelding 38:** een replicatie-taak maken_

  Een wizard leidt u door het proces van het maken van een taak voor replicatie.
4.  Definieer de naam, de TCP/IP-adres en het schijfvolume van het bronknooppunt.

  ![Afbeelding 39: Definieer de naam van de replicatie-taak][sap-ha-guide-figure-3039]

  _**Afbeelding 39:** definiëren van de naam van de replicatie-taak_

  ![Afbeelding 40: Definieer de basisgegevens voor het knooppunt, het huidige bronknooppunt moet][sap-ha-guide-figure-3040]

  _**Afbeelding 40:** de basisgegevens voor het knooppunt, het huidige bronknooppunt moet definiëren_

5.  Definieer de naam, de TCP/IP-adres en het schijfvolume van het doelknooppunt.

  ![Afbeelding 41: Definieer de basisgegevens voor het knooppunt, het huidige doelknooppunt moet][sap-ha-guide-figure-3041]

  _**Afbeelding 41:** de basisgegevens voor het knooppunt, het huidige doelknooppunt moet definiëren_

6.  Definieer de compressie-algoritmen. In ons voorbeeld is het raadzaam dat u de replicatiestroom comprimeren. Met name in situaties hersynchronisatie vermindert de compressie van de replicatiestroom aanzienlijk hersynchronisatie tijd. Compressie maakt gebruik van de bronnen van CPU en RAM-geheugen van een virtuele machine. Als de snelheid met compressie toeneemt, loopt u het volume van de CPU-resources die worden gebruikt. Ook kunt u aanpassen deze instelling later.

7.  Een andere instelling die u nodig hebt om te controleren is of de replicatie synchroon of asynchroon plaatsvindt. *Wanneer u SAP ASC's / SCS configuraties beveiligt, moet u synchrone replicatie*.  

  ![Afbeelding 42: Replicatiedetails definiëren][sap-ha-guide-figure-3042]

  _**Afbeelding 42:** replicatiedetails definiëren_

8.  Bepalen of het volume dat is gerepliceerd door de taak moet worden weergegeven in een clusterconfiguratie Windows Server Failover Clustering als gedeelde schijf. Selecteer voor de configuratie SAP ASC's / SCS **Ja** zodat het cluster van Windows ziet het volume gerepliceerde als gedeelde schijf die kan worden gebruikt als een clustervolume.

  ![Afbeelding 43: Selecteer Ja als u wilt het gerepliceerde volume als een clustervolume instellen][sap-ha-guide-figure-3043]

  _**Afbeelding 43:** Selecteer **Ja** in te stellen van het volume gerepliceerde als een clustervolume_

  Nadat het volume is gemaakt, ziet u het hulpprogramma DataKeeper beheer en de configuratie of de taak nog actief.

  ![Afbeelding 44: Synchroon spiegelen van DataKeeper voor de schijf van de share SAP ASC's / SCS actief is.][sap-ha-guide-figure-3044]

  _**Afbeelding 44:** DataKeeper synchroon spiegelen voor de SAP ASC's / SCS schijf delen is actief_

  Failover Cluster Manager ziet nu de schijf als een schijf DataKeeper, zoals wordt weergegeven:

  ![Afbeelding 45: Failover Cluster Manager ziet u de schijf die DataKeeper gerepliceerd][sap-ha-guide-figure-3045]

  _**Afbeelding 45:** Failoverclusterbeheer ziet u de schijf die is gerepliceerd DataKeeper_

## <a name="next-steps"></a>Volgende stappen

* [SAP NetWeaver HA installatie met **Windows Failover Cluster** en **gedeelde schijf** voor SAP (A) SCS exemplaar][sap-high-availability-installation-wsfc-shared-disk]
