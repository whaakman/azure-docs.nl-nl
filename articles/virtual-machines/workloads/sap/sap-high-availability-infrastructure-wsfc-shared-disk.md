---
title: Voorbereiden van de Azure-infrastructuur voor SAP HA met een Windows-failovercluster en een gedeelde schijf voor SAP ASCS/SCS | Microsoft Docs
description: Informatie over het voorbereiden van de Azure-infrastructuur voor SAP HA met behulp van een Windows-failovercluster en een gedeelde schijf voor een SAP ASCS/SCS-exemplaar.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ec976257-396b-42a0-8ea1-01c97f820fa6
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 43e4cf27d9a57db58c0f90b269e7a52622508ee1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57998760"
---
# <a name="prepare-the-azure-infrastructure-for-sap-ha-by-using-a-windows-failover-cluster-and-shared-disk-for-sap-ascsscs"></a>Voorbereiden van de Azure-infrastructuur voor SAP HA met behulp van een Windows-failovercluster en een gedeelde schijf voor SAP ASCS/SCS

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

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
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

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (Configuratie voor hoge beschikbaarheid van SAP multi-SID)

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

In dit artikel wordt beschreven hoe u de Azure-infrastructuur voorbereiden voor het installeren en configureren van een SAP-systeem voor hoge beschikbaarheid op een Windows-failovercluster met behulp van een *gedeelde clusterschijf* als een optie voor clustering een SAP ASCS-exemplaar.

## <a name="prerequisites"></a>Vereisten

Voordat u de installatie begint, controleert u in dit artikel:

* [Architectuurhandleiding voor: Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een gedeelde clusterschijf][sap-high-availability-guide-wsfc-shared-disk]

## <a name="prepare-the-infrastructure-for-architectural-template-1"></a>De infrastructuur voorbereiden voor architectuur sjabloon 1
Azure Resource Manager-sjablonen voor SAP te vereenvoudigen van de implementatie van de vereiste resources.

De sjablonen met drie lagen in Azure Resource Manager ondersteunen ook scenario's voor hoge beschikbaarheid. Architectuur sjabloon 1 heeft bijvoorbeeld twee clusters. Elk cluster is een SAP enkel storingspunt voor SAP ASCS/SCS en DBMS-systemen.

Dit is waar u Azure Resource Manager-sjablonen kunt ophalen voor het voorbeeldscenario dat in dit artikel wordt beschreven:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Azure Marketplace-installatiekopie met behulp van Azure Managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Aangepaste installatiekopie met behulp van Managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

De infrastructuur voorbereiden voor architectuur sjabloon 1:

- In de Azure-portal in de **Parameters** deelvenster in de **SYSTEMAVAILABILITY** Schakel **HA**.

  ![Afbeelding 1: SAP hoge beschikbaarheid Azure Resource Manager-parameters instellen][sap-ha-guide-figure-3000]

_**Afbeelding 1:** SAP hoge beschikbaarheid Azure Resource Manager-parameters instellen_


  De sjablonen worden gemaakt:

  * **Virtuele machines**:
    * SAP-toepassingsserver virtuele machines: \<SAPSystemSID\>- di -\<getal\>
    * ASCS/SCS cluster virtuele machines: \<SAPSystemSID\>- ascs-\<getal\>
    * DBMS-cluster: \<SAPSystemSID\>- db-\<getal\>

  * **Netwerkkaarten voor alle virtuele machines, met bijbehorende IP-adressen**:
    * \<SAPSystemSID\>-nic-di-\<Number\>
    * \<SAPSystemSID\>-nic-ascs-\<Number\>
    * \<SAPSystemSID\>- nic-db -\<getal\>

  * **Azure storage-accounts (alleen voor niet-beheerde schijven)**:

  * **Beschikbaarheidsgroepen** voor:
    * SAP-toepassingsserver virtuele machines: \<SAPSystemSID\>-avset-di
    * SAP ASCS/SCS cluster virtuele machines: \<SAPSystemSID\>-avset-ascs
    * DBMS-cluster virtuele machines: \<SAPSystemSID\>-avset-db

  * **Interne Azure load balancer**:
    * Met alle poorten voor de ASCS/SCS-exemplaar en het IP-adres \<SAPSystemSID\>- lb-ascs
    * Met alle poorten voor de SQL Server DBMS en IP-adres \<SAPSystemSID\>- lb-db

  * **Netwerkbeveiligingsgroep**: \<SAPSystemSID\>-nsg-ascs-0  
    * Met een open externe Remote Desktop Protocol (RDP)-poort op de \<SAPSystemSID\>- ascs-0 virtuele machine

> [!NOTE]
> Alle IP-adressen van de netwerkkaarten en interne Azure load balancers zijn standaard dynamisch. Wijzig deze in statische IP-adressen. Hoe u dit doet later in dit artikel wordt beschreven.
>
>

## <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Virtuele machines met de zakelijke netwerkverbinding (cross-premises) te gebruiken in de productieomgeving implementeren
Implementeren voor productie SAP-systemen, Azure virtual machines met [zakelijke netwerkverbinding (cross-premises)] [ planning-guide-2.2] met behulp van Azure VPN-Gateway of Azure ExpressRoute.

> [!NOTE]
> U kunt uw Azure Virtual Network-exemplaar gebruiken. Het virtuele netwerk en subnet zijn al gemaakt en voorbereid.
>
>

1. In de Azure-portal in de **Parameters** deelvenster in de **NEWOREXISTINGSUBNET** Schakel **bestaande**.
2. In de **SUBNETID** vak, voegt u de volledige tekenreeks van uw subnet-ID van het voorbereide Azure-netwerk waar u van plan bent uw Azure-machines implementeren.
3. Als u een lijst met alle Azure-netwerksubnetten, moet u deze PowerShell-opdracht uitvoeren:

   ```PowerShell
   (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
   ```

   De **ID** veld ziet u de waarde voor de subnet-ID.
4. Als u een lijst van alle waarden van de subnet-id's, moet u deze PowerShell-opdracht uitvoeren:

   ```PowerShell
   (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
   ```

   De subnet-ID ziet er zo uit:

   ```
   /subscriptions/<subscription ID>/resourceGroups/<VPN name>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<subnet name>
   ```

## <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> SAP-instanties voor alleen-cloud voor testen en demo implementeren
U kunt uw SAP-systeem voor hoge beschikbaarheid in een alleen-cloud implementatiemodel implementeren. Dit type implementatie is vooral handig voor demo's en test gebruiksvoorbeelden. Het niet geschikt voor productiegebruikssituaties.

- In de Azure-portal in de **Parameters** deelvenster in de **NEWOREXISTINGSUBNET** Schakel **nieuwe**. Laat de **SUBNETID** veld leeg.

  De SAP Azure Resource Manager-sjabloon maakt automatisch de Azure-netwerk en subnet.

> [!NOTE]
> Ook moet u ten minste één toegewezen virtuele machine voor Active Directory en DNS-service in dezelfde instantie van Azure Virtual Network implementeren. De sjabloon maken niet deze virtuele machines.
>
>


## <a name="prepare-the-infrastructure-for-architectural-template-2"></a>De infrastructuur voorbereiden voor architectuur sjabloon 2

U kunt deze Azure Resource Manager-sjabloon voor SAP gebruiken om te vereenvoudigen, de implementatie van resources van de vereiste infrastructuur voor SAP architectuur sjabloon 2.

Dit is waar u Azure Resource Manager-sjablonen voor deze implementatiescenario kunt krijgen:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Azure Marketplace-installatiekopie met behulp van Managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Aangepaste installatiekopie met behulp van Managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


## <a name="prepare-the-infrastructure-for-architectural-template-3"></a>De infrastructuur voorbereiden voor architectuur sjabloon 3

U kunt de infrastructuur voorbereiden en configureren van SAP voor multi-SID. U kunt bijvoorbeeld een extra SAP ASCS/SCS-exemplaar in toevoegen een *bestaande* clusterconfiguratie. Zie voor meer informatie, [configureren van een extra SAP ASCS/SCS-exemplaar voor een bestaande configuratie van het cluster voor het maken van een SAP-multi-SID-configuratie in Azure Resource Manager][sap-ha-multi-sid-guide].

Als u een nieuw multi-SID-cluster maakt wilt, kunt u de multi-SID [quickstart-sjablonen op GitHub](https://github.com/Azure/azure-quickstart-templates).

Voor het maken van een nieuw cluster met multi-SID, moet u de volgende drie sjablonen implementeren:

* [ASCS/SCS-sjabloon](#ASCS-SCS-template)
* [Database-sjabloon](#database-template)
* [Toepassingssjabloon van servers](#application-servers-template)

De volgende secties hebben meer informatie over de sjablonen en de parameters die u moet opgeven in de sjablonen.

### <a name="ASCS-SCS-template"></a> ASCS/SCS-sjabloon

De ASCS/SCS-sjabloon implementeert twee virtuele machines die u gebruiken kunt om te maken van een Windows Server-failovercluster dat als host fungeert voor meerdere ASCS/SCS-exemplaren.

In de ASCS/SCS multi-SID-sjabloon instellen de [ASCS/SCS multi-SID sjabloon] [ sap-templates-3-tier-multisid-xscs-marketplace-image] of [ASCS/SCS multi-SID-sjabloon met behulp van Managed Disks] [ sap-templates-3-tier-multisid-xscs-marketplace-image-md], voer waarden in voor de volgende parameters:

- **Resource-voorvoegsel**:  Stel de resource-voorvoegsel dat wordt gebruikt als voorvoegsel voor alle resources die zijn gemaakt tijdens de implementatie. Omdat de resources niet tot slechts één SAP-systeem behoren, wordt het voorvoegsel van de resource is niet de SID van een SAP-systeem.  Het voorvoegsel moet tussen de drie tot zes tekens lang zijn.
- **Stack-Type**: Selecteer de stack-type van de SAP-systeem. Azure Load Balancer zijn afhankelijk van het type stack, een (ABAP of alleen Java) of twee (ABAP + Java) privé IP-adressen per SAP-systeem.
- **Type besturingssysteem**: Selecteer het besturingssysteem van de virtuele machines.
- **Telling van SAP-systeem**: Selecteer het aantal SAP-systemen die u wilt installeren in dit cluster.
- **Beschikbaarheid van het systeem**: Selecteer **HA**.
- **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**: Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
- **Nieuw of bestaand Subnet**: Instellen of een nieuw virtueel netwerk en een subnet maken of gebruiken van een bestaand subnet. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
- **Subnet-Id**: Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID ziet er meestal als volgt uit:

  /Subscriptions/\<abonnements-id\>/resourceGroups/\<groepsnaam voor accountresources\>/providers/Microsoft.Network/virtualNetworks/\<virtuele-netwerknaam\>/subnets/ \<subnetnaam\>

De sjabloon implementeert één exemplaar voor Azure Load Balancer, die ondersteuning biedt voor meerdere SAP-systemen:

- De ASCS-exemplaren zijn geconfigureerd voor exemplaarnummer 00, 10, 20...
- De SCS-exemplaren zijn geconfigureerd voor het exemplaar, getal, 01, 11, 21...
- De exemplaren ASCS in de wachtrij plaatsen replicatie Server (gebruikers) (alleen Linux) zijn geconfigureerd voor het exemplaar, getal, 02, 12, 22...
- De SCS ERS (alleen Linux)-exemplaren zijn geconfigureerd voor exemplaarnummer 03, 13, 23...

De load balancer bevat 1 VIP(s) (2 voor Linux), 1 x-VIP voor ASCS/SCS en 1 x-VIP voor gebruikers (alleen Linux).

#### <a name="0f3ee255-b31e-4b8a-a95a-d9ed6200468b"></a> SAP ASCS/SCS-poorten
De volgende lijst bevat alle load balancer-regels (waarbij x staat voor het nummer van de SAP-systeem, bijvoorbeeld 1, 2, 3...):
- Windows-specifieke poorten voor elk SAP-systeem: 445, 5985
- ASCS-poorten (exemplaarnummer x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016
- SCS-poorten (exemplaarnummer x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116
- ASCS ERS poorten op Linux (exemplaarnummer x2): 33x2, 5x213, 5x214, 5x216
- SCS ERS poorten op Linux (exemplaarnummer x3): 33x3, 5x313, 5x314, 5x316

De load balancer is geconfigureerd voor het gebruik van de volgende test-poorten (waarbij x staat voor het nummer van de SAP-systeem, bijvoorbeeld 1, 2, 3...):
- ASCS/SCS interne load balancer-test-poort: 620x0
- UIT interne load balancer-test-poort (alleen Linux): 621x2

### <a name="database-template"></a> Database-sjabloon

De databasesjabloon implementeert een of twee virtuele machines die u gebruiken kunt voor het installeren van het relationele databasebeheersysteem (RDBMS) voor een SAP-systeem. Bijvoorbeeld, als u een ASCS/SCS-sjabloon voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van de sjabloon van de database-multi-SID, in de [database multi-SID sjabloon] [ sap-templates-3-tier-multisid-db-marketplace-image] of [multi-SID-sjabloon voor database met behulp van Managed Disks] [ sap-templates-3-tier-multisid-db-marketplace-image-md], voer waarden in voor de volgende parameters:

- **SAP-systeem-Id**: Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
- **Type besturingssysteem**: Selecteer het besturingssysteem van de virtuele machines.
- **Dbtype**: Selecteer het type van de database die u wilt installeren op het cluster. Selecteer **SQL** als u wilt installeren van Microsoft SQL Server. Selecteer **HANA** als u van plan bent voor het installeren van SAP HANA op de virtuele machines. Zorg ervoor dat u het juiste besturingssysteemtype selecteert. Selecteer **Windows** voor SQL, en selecteer een Linux-distributie voor HANA. Azure Load Balancer die is verbonden met de virtuele machines is geconfigureerd voor ondersteuning van het type van de geselecteerde database:
  * **SQL**: De load balancer verdelen poort 1433. Zorg ervoor dat u deze poort voor de installatie van SQL Server AlwaysOn.
  * **HANA**: De load balancer verdelen-poorten 35015 en 35017. Zorg ervoor dat u het installeren van SAP HANA met exemplaarnummer **50**.
  Testpoort 62550 maakt gebruik van de load balancer.
- **SAP-systeem grootte**: Stel het aantal SAP's vindt u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
- **Beschikbaarheid van het systeem**: Selecteer **HA**.
- **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**: Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
- **Subnet-Id**: Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de ASCS/SCS-sjabloon of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASCS/SCS.

### <a name="application-servers-template"></a> Toepassingssjabloon van servers

De toepassing servers-sjabloon implementeert twee of meer virtuele machines die kan worden gebruikt als instanties van SAP-toepassingsserver voor een SAP-systeem. Bijvoorbeeld, als u een ASCS/SCS-sjabloon voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van de toepassing servers multi-SID-sjabloon in de [toepassingssjabloon servers multi-SID] [ sap-templates-3-tier-multisid-apps-marketplace-image] of [servers multi-SID toepassingssjabloon met behulp van Managed Disks] [ sap-templates-3-tier-multisid-apps-marketplace-image-md], voer waarden in voor de volgende parameters:

  -  **SAP-systeem-Id**: Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**: Selecteer het besturingssysteem van de virtuele machines.
  -  **SAP-systeem grootte**: Het aantal SAP's vindt u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
  -  **Beschikbaarheid van het systeem**: Selecteer **HA**.
  -  **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**: Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**: Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de ASCS/SCS-sjabloon of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASCS/SCS.


## <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure-netwerk
In ons voorbeeld is de adresruimte van het exemplaar van Azure Virtual Network 10.0.0.0/16. Er is één subnet met de naam Subnet, met een adresbereik van 10.0.0.0/24. Alle virtuele machines en interne load balancers worden geïmplementeerd in dit virtuele netwerk.

> [!IMPORTANT]
> Breng geen wijzigingen in de netwerkinstellingen van het in het gastbesturingssysteem. Dit omvat IP-adressen, DNS-servers en -subnet. De netwerkinstellingen configureren in Azure. De Dynamic Host Configuration Protocol (DHCP)-service geeft de instellingen.
>
>

## <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS-IP-adressen

Als u wilt de vereiste DNS IP-adressen instellen, voert u de volgende stappen uit:

1. In de Azure-portal in de **DNS-servers** deelvenster, zorg ervoor dat uw virtuele netwerk **DNS-servers** optie is ingesteld op **aangepaste DNS**.
2. Selecteer uw instellingen op basis van het type netwerk die u hebben. Zie de volgende bronnen voor meer informatie:
   * [Verbinding met het hoofdkantoor netwerk (cross-premises)][planning-guide-2.2]: De IP-adressen van de lokale DNS-servers toevoegen.  
   U kunt on-premises DNS-servers aan de virtuele machines die worden uitgevoerd in Azure kunt uitbreiden. U kunt in dit scenario is het IP-adressen van de virtuele machines van Azure waar u de DNS-service uitgevoerd toevoegen.
   * Voor VM-implementaties die zijn geïsoleerd in Azure: Implementeer een extra virtuele machine in hetzelfde Virtueelnetwerk-exemplaar dat als een DNS-server fungeert. Toevoegen het IP-adressen van de Azure virtuele machines die u hebt ingesteld tot de DNS-service wordt uitgevoerd.

   ![Afbeelding 2: DNS-servers configureren voor Azure Virtual Network][sap-ha-guide-figure-3001]

   _**Afbeelding 2:** DNS-servers configureren voor Azure Virtual Network_

   > [!NOTE]
   > Als u de IP-adressen van de DNS-servers wijzigt, moet u de virtuele machines van Azure om de wijziging toepassen en het doorgeven van de nieuwe DNS-servers opnieuw opstarten.
   >
   >

In ons voorbeeld is de DNS-service geïnstalleerd en geconfigureerd op deze virtuele machines van Windows:

| Virtuele-machinefunctie | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste DNS-server |domcontr-0 |pr1-nic-domcontr-0 |10.0.0.10 |
| Tweede DNS-server |domcontr-1 |pr1-nic-domcontr-1 |10.0.0.11 |

## <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Hostnamen en statische IP-adressen voor de geclusterde SAP ASCS/SCS-exemplaar en de geclusterde instantie van DBMS-systemen

Voor on-premises implementatie moet u deze gereserveerde hostnamen en IP-adressen:

| Rol van de virtuele host-naam | Naam van virtuele host | Virtuele vaste IP-adres |
| --- | --- | --- |
| SAP ASCS/SCS eerste cluster virtuele host-naam (voor cluster) |pr1-ascs-vir |10.0.0.42 |
| Naam van de virtuele host SAP ASCS/SCS-exemplaar |PR1-ascs-sap |10.0.0.43 |
| SAP DBMS tweede virtuele host clusternaam (Clusterbeheer) |pr1-dbms-vir |10.0.0.32 |

Wanneer u het cluster maakt, maakt u de virtuele host namen pr1-ascs-vir en pr1-dbms-vir en de bijbehorende IP-adressen die het cluster zelf beheren. Zie voor informatie over hoe u dit doet, [verzamelen van de knooppunten in een clusterconfiguratie][sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config].

U kunt handmatig de andere twee virtuele hostnamen, pr1-ascs-sap en pr1-dbms-sap, en de bijbehorende IP-adressen maken op de DNS-server. De geclusterde SAP ASCS/SCS-exemplaar en de geclusterde instantie op DBMS-systemen kunt u deze resources gebruiken. Zie voor informatie over hoe u dit doet, [maken van een virtuele host-naam voor een geclusterde SAP ASCS/SCS-exemplaar][sap-ha-guide-9.1.1].

## <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Statische IP-adressen voor de SAP-virtuele machines instellen
Nadat u de virtuele machines te gebruiken in uw cluster implementeert, moet u statische IP-adressen voor alle virtuele machines instellen. Dit doen in de configuratie van Azure Virtual Network, en niet in het gastbesturingssysteem.

1. Selecteer in de Azure portal, **resourcegroep** > **netwerkkaart** > **instellingen** > **IP-adres**.
2. In de **IP-adressen** deelvenster onder **toewijzing**, selecteer **statische**. In de **IP-adres** voert u de IP-adres dat u wilt gebruiken.

   > [!NOTE]
   > Als u het IP-adres van de netwerkkaart wijzigt, moet u opnieuw opstarten van de virtuele machines van Azure om toe te passen van de wijziging.  
   >
   >

   ![Afbeelding 3: Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen][sap-ha-guide-figure-3002]

   _**Afbeelding 3:** Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen_

   Herhaal deze stap voor alle netwerkinterfaces die inclusief is, voor alle virtuele machines, virtuele machines die u wilt gebruiken voor uw Active Directory of DNS-service.

In ons voorbeeld hebben we deze virtuele machines en statische IP-adressen:

| Virtuele-machinefunctie | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste SAP application server-exemplaar |pr1-di-0 |pr1-nic-di-0 |10.0.0.50 |
| Tweede instantie van SAP-toepassingsserver |pr1-di-1 |pr1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Laatste SAP application server-exemplaar |pr1-di-5 |pr1-nic-di-5 |10.0.0.55 |
| Eerste clusterknooppunt voor ASCS/SCS-exemplaar |pr1-ascs-0 |pr1-nic-ascs-0 |10.0.0.40 |
| Tweede clusterknooppunt voor ASCS/SCS-exemplaar |pr1-ascs-1 |pr1-nic-ascs-1 |10.0.0.41 |
| Eerste clusterknooppunt voor DBMS-instantie |pr1-db-0 |pr1-nic-db-0 |10.0.0.30 |
| Tweede clusterknooppunt voor DBMS-instantie |pr1-db-1 |pr1-nic-db-1 |10.0.0.31 |

## <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Een statisch IP-adres voor de interne Azure load balancer instellen

De SAP Azure Resource Manager-sjabloon maakt u een interne Azure load balancer die wordt gebruikt voor de SAP ASCS/SCS-exemplaar als het cluster DBMS-systemen.

> [!IMPORTANT]
> Het IP-adres van de virtuele host-naam van de SAP ASCS/SCS is hetzelfde als het IP-adres van de SAP ASCS/SCS interne load balancer: pr1-lb-ascs.
> Het IP-adres van de virtuele naam van de DBMS-systemen is hetzelfde als het IP-adres van de DBMS interne load balancer: pr1-lb-DBMS-systemen.
>
>

Een statisch IP-adres voor de interne Azure load balancer instellen:

1. De eerste implementatie van het IP-adres van interne load balancer wordt ingesteld op **dynamische**. In de Azure-portal op de **IP-adressen** deelvenster onder **toewijzing**, selecteer **statische**.
2. Stel het IP-adres van de interne load balancer **pr1-lb-ascs** naar het IP-adres van de virtuele host-naam van de SAP ASCS/SCS-exemplaar.
3. Stel het IP-adres van de interne load balancer **pr1-lb-dbms** naar het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

   ![Afbeelding 4: Statische IP-adressen instellen voor de interne load balancer voor de SAP ASCS/SCS-exemplaar][sap-ha-guide-figure-3003]

   _**Afbeelding 4:** Statische IP-adressen instellen voor de interne load balancer voor de SAP ASCS/SCS-exemplaar_

In ons voorbeeld hebben we twee interne Azure load balancers die deze statische IP-adressen:

| Interne Azure load balancer-rol | Naam van de interne Azure load balancer | Statisch IP-adres |
| --- | --- | --- |
| SAP ASCS/SCS-exemplaar van interne load balancer |pr1-lb-ascs |10.0.0.43 |
| SAP DBMS interne load balancer |pr1-lb-dbms |10.0.0.33 |


## <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Standaard ASCS/SCS load balancer-regels voor de interne Azure load balancer

De SAP Azure Resource Manager-sjabloon maakt u de poorten die u nodig hebt:
* Een ABAP ASCS-instantie met de standaard instantienummer 00
* Een Java-SCS-exemplaar, met het standaardaantal exemplaar 01

Wanneer u uw SAP ASCS/SCS-exemplaar installeert, moet u de standaard instantienummer 00 gebruiken voor uw ABAP ASCS-exemplaar en het exemplaarnummer standaard 01 voor uw Java-SCS-exemplaar.

Maak vervolgens de vereiste interne load balancer-eindpunten voor de SAP NetWeaver-poorten.

Als u wilt maken vereist interne eindpunten voor load balancer, maakt u eerst deze eindpunten voor load balancer voor de SAP NetWeaver ABAP ASCS-poorten:

| Service/load balancing regelnaam | Standaardpoortnummers | Concrete poorten voor (ASCS-exemplaar met exemplaarnummer 00) (INGEN met 10) |
| --- | --- | --- |
| In de wachtrij plaatsen server / *lbrule3200* |32\<InstanceNumber\> |3200 |
| ABAP-berichtenserver / *lbrule3600* |36\<InstanceNumber\> |3600 |
| Interne ABAP-bericht / *lbrule3900* |39\<InstanceNumber\> |3900 |
| Message server HTTP / *Lbrule8100* |81\<InstanceNumber\> |8100 |
| SAP ASCS-HTTP-service starten / *Lbrule50013* |5\<InstanceNumber\>13 |50013 |
| SAP ASCS HTTPS-service starten / *Lbrule50014* |5\<InstanceNumber\>14 |50014 |
| In de wachtrij plaatsen replicatie / *Lbrule50016* |5\<InstanceNumber\>16 |50016 |
| SAP INGEN HTTP-service starten *Lbrule51013* |5\<InstanceNumber\>13 |51013 |
| SAP INGEN HTTP-service starten *Lbrule51014* |5\<InstanceNumber\>14 |51014 |
| Windows Remote Management (WinRM) *Lbrule5985* | |5985 |
| Bestandsshare *Lbrule445* | |445 |

**Tabel 1:** Poortnummers van de SAP NetWeaver ABAP ASCS-instanties

Vervolgens maakt u deze eindpunten voor load balancer voor de SAP NetWeaver Java SCS-poorten:

| Service/load balancing regelnaam | Standaardpoortnummers | Concrete poorten voor (SCS-exemplaar met exemplaarnummer 01) (INGEN met 11) |
| --- | --- | --- |
| In de wachtrij plaatsen server / *lbrule3201* |32\<InstanceNumber\> |3201 |
| Gateway-server / *lbrule3301* |33\<InstanceNumber\> |3301 |
| Java-berichtenserver / *lbrule3900* |39\<InstanceNumber\> |3901 |
| Message server HTTP / *Lbrule8101* |81\<InstanceNumber\> |8101 |
| SAP SCS-HTTP-service starten / *Lbrule50113* |5\<InstanceNumber\>13 |50113 |
| SAP SCS HTTPS-service starten / *Lbrule50114* |5\<InstanceNumber\>14 |50114 |
| In de wachtrij plaatsen replicatie / *Lbrule50116* |5\<InstanceNumber\>16 |50116 |
| SAP INGEN HTTP-service starten *Lbrule51113* |5\<InstanceNumber\>13 |51113 |
| SAP INGEN HTTP-service starten *Lbrule51114* |5\<InstanceNumber\>14 |51114 |
| WinRM *Lbrule5985* | |5985 |
| Bestandsshare *Lbrule445* | |445 |

**Tabel 2:** Poortnummers van de SAP NetWeaver Java SCS-instanties

![Afbeelding 5: Standaard ASCS/SCS load balancer-regels voor de interne Azure load balancer][sap-ha-guide-figure-3004]

_**Afbeelding 5:** Standaard ASCS/SCS load balancer-regels voor de interne Azure load balancer_

Het IP-adres van de load balancer pr1-lb-dbms ingesteld op het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> De ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer wijzigen

Als u gebruiken van verschillende aantallen voor de SAP ASCS of SCS wilt, moet u de namen en waarden van hun poorten wijzigen van de standaardwaarden.

1. Selecteer in de Azure portal,  **\<SID\>-lb-ascs netwerktaakverdeler** > **regels voor Load Balancing**.
2. Voor alle load balancer-regels die deel uitmaken van het SAP ASCS of SCS-exemplaar, kunt u deze waarden wijzigen:

   * Name
   * Poort
   * Back-end-poort

   Bijvoorbeeld, als u het standaardnummer van de ASCS-instantie wijzigen van 00 tot en met 31 wilt, moet u de aanbrengen voor alle poorten die worden vermeld in tabel 1.

   Hier volgt een voorbeeld van een update voor poort *lbrule3200*.

   ![Afbeelding 6: De ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer wijzigen][sap-ha-guide-figure-3005]

   _**Afbeelding 6:** De ASCS/SCS standaard load balancer-regels voor de interne Azure load balancer wijzigen_

## <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Windows virtuele machines toevoegen aan het domein

Nadat u een statisch IP-adres aan de virtuele machines toegewezen, moet u de virtuele machines toevoegen aan het domein.

![Afbeelding 7: Een virtuele machine toevoegen aan een domein][sap-ha-guide-figure-3006]

_**Afbeelding 7:** Een virtuele machine toevoegen aan een domein_

## <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Registervermeldingen op beide clusterknooppunten van de SAP ASCS/SCS-exemplaar toevoegen

Azure Load Balancer heeft een interne load balancer die wordt gesloten verbindingen als de verbindingen niet actief voor een bepaalde zijn tijd (een time-out voor inactiviteit). SAP-werkprocessen in dialoogvenster exemplaren open verbindingen met de SAP in de wachtrij plaatsen verwerken zodra het eerste in de wachtrij plaatsen/uit de wachtrij verwijderen aanvragen moet worden verzonden. Deze verbindingen meestal blijven tot stand gebrachte tot het werkproces of het proces in de wachtrij plaatsen opnieuw wordt opgestart. Als de verbinding niet actief gedurende een bepaalde tijd wordt opgelost is, sluit de interne Azure load balancer de verbindingen. Dit is een probleem niet omdat het werkproces SAP herstelt u de verbinding met het proces in de wachtrij plaatsen als deze niet meer bestaat. Deze activiteiten worden beschreven in de developer-traceringen van SAP-processen, maar ze een grote hoeveelheid extra inhoud maken in deze traceringen. Is het een goed idee om te wijzigen van de TCP/IP `KeepAliveTime` en `KeepAliveInterval` op beide knooppunten van het cluster. Combineer deze wijzigingen in de TCP/IP-parameters en de SAP-profiel parameters die later in dit artikel wordt beschreven.

Als u wilt toevoegen de registervermeldingen op beide clusterknooppunten van de SAP ASCS/SCS-exemplaar, eerst deze Windows registervermeldingen toevoegen op beide clusterknooppunten Windows voor SAP ASCS/SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam van de variabele |`KeepAliveTime` |
| Type variabele |REG_DWORD (decimaal) |
| Value |120000 |
| Koppeling naar documentatie |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

**Tabel 3:** Wijzig de eerste parameter van de TCP/IP

Voegt u deze Windows-register-item op beide clusterknooppunten Windows voor SAP ASCS/SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam van de variabele |`KeepAliveInterval` |
| Type variabele |REG_DWORD (decimaal) |
| Value |120000 |
| Koppeling naar documentatie |[https://technet.microsoft.com/library/cc957548.aspx](https://technet.microsoft.com/library/cc957548.aspx) |

**Tabel 4:** Wijzig de tweede parameter van de TCP/IP

Start opnieuw op beide clusterknooppunten om de wijzigingen van kracht.

## <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Instellen van een Windows Server-failovercluster voor een SAP ASCS/SCS-exemplaar

Instellen van een Windows Server failover-cluster voor een SAP ASCS/SCS-exemplaar, moet deze taken uitvoeren:

- Verzamel de clusterknooppunten in een clusterconfiguratie.
- Een cluster bestandsshare-witness configureren.

### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Verzamelen van de clusterknooppunten in een clusterconfiguratie

1. In de functie toevoegen en Wizard Functies toevoegen voor failover clustering op beide clusterknooppunten.
2. Het failover-cluster instellen met behulp van Failoverclusterbeheer. Selecteer in Failoverclusterbeheer **Cluster maken**, en voegt u alleen de naam van het eerste cluster (een knooppunt). Het tweede knooppunt niet nog; toevoegen u kunt het tweede knooppunt toevoegen in een latere stap.

   ![Afbeelding 8: De naam van de server of virtuele machine van het eerste clusterknooppunt toevoegen][sap-ha-guide-figure-3007]

   _**Afbeelding 8:** De naam van de server of virtuele machine van het eerste clusterknooppunt toevoegen_

3. Voer de netwerknaam (virtuele host-naam) van het cluster.

   ![Afbeelding 9: Voer de naam van het cluster][sap-ha-guide-figure-3008]

   _**Afbeelding 9:** Voer de naam van het cluster_

4. Nadat u het cluster hebt gemaakt, voert u een clustervalidatietest.

   ![Afbeelding 10: De controle van de cluster-validatie uitvoeren][sap-ha-guide-figure-3009]

   _**Afbeelding 10:** De controle van de cluster-validatie uitvoeren_

   U kunt waarschuwingen over schijven op dit moment in het proces negeren. U voegt dat een bestandsshare-witness en de SIOS gedeelde schijven later opnieuw. In deze fase hoeft u niet bang dat een quorum.

   ![Afbeelding 11: Er is geen quorumschijf is gevonden][sap-ha-guide-figure-3010]

   _**Afbeelding 11:** Er is geen quorumschijf is gevonden_

   ![Afbeelding 12: Een core cluster-bron moet een nieuw IP-adres][sap-ha-guide-figure-3011]

   _**Afbeelding 12:** Een core cluster-bron moet een nieuw IP-adres_

5. Wijzig het IP-adres van de core cluster-service. Het cluster kan totdat u het IP-adres van de core cluster-service niet starten omdat het IP-adres van de server naar een van de virtuele machine-knooppunten verwijst. Dit doen op de **eigenschappen** pagina van de core-clusterservice IP-resource.

   Bijvoorbeeld, moeten we een IP-adres (in ons voorbeeld 10.0.0.42) toewijzen voor het cluster virtuele host naam pr1-ascs-vir.

   ![Afbeelding 13: Klik in het dialoogvenster Eigenschappen van de IP-adres wijzigen][sap-ha-guide-figure-3012]

   _**Afbeelding 13:** In de **eigenschappen** dialoogvenster vak, wijzigt u het IP-adres_

   ![Afbeelding 14: Het IP-adres dat is gereserveerd voor het cluster toewijzen][sap-ha-guide-figure-3013]

   _**Afbeelding 14:** Het IP-adres dat is gereserveerd voor het cluster toewijzen_

6. Virtuele hostnaam van het cluster online brengen.

   ![Afbeelding 15: De cluster-core-service is actief, met de juiste IP-adres][sap-ha-guide-figure-3014]

   _**Afbeelding 15:** De cluster-core-service is actief, met de juiste IP-adres_

7. Het tweede clusterknooppunt toevoegen.

   De core cluster-service is actief en werkend, kunt u het tweede clusterknooppunt toevoegen.

   ![Afbeelding 16 toevoegen het tweede clusterknooppunt][sap-ha-guide-figure-3015]

   _**Afbeelding 16:** Het tweede clusterknooppunt toevoegen_

8. Voer een naam voor de tweede host van de cluster-knooppunt.

   ![Afbeelding 17: Voer de tweede hostnaam van de cluster-knooppunt][sap-ha-guide-figure-3016]

   _**Afbeelding 17:** Voer de tweede hostnaam van de cluster-knooppunt_

   > [!IMPORTANT]
   > Controleer of de **alle in aanmerking komende opslag toevoegen aan het cluster** selectievakje *niet* geselecteerde.  
   >
   >

   ![Afbeelding 18: Schakel het selectievakje niet][sap-ha-guide-figure-3017]

   _**Afbeelding 18:** Voer *niet* Schakel het selectievakje_

   Waarschuwingen over quorum en schijven, kunt u negeren. U stelt het quorum en delen van de schijf later, zoals beschreven in [SIOS DataKeeper Cluster Edition installeren voor het hulpprogramma voor het delen van een SAP ASCS/SCS-clusterschijf][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].

   ![Afbeelding 19: Negeren van waarschuwingen met betrekking tot de schijf quorum][sap-ha-guide-figure-3018]

   _**Afbeelding 19:** Negeren van waarschuwingen met betrekking tot de schijf quorum_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Een cluster bestandsshare-witness configureren

Een cluster bestandsshare-witness configureren, moet deze taken uitvoeren:

- Een bestandsshare maken.
- Stel het quorum file share witness in Failoverclusterbeheer.

#### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Een bestandsshare maken

1. Selecteer een bestandsshare-witness in plaats van een quorumschijf. SIOS DataKeeper biedt ondersteuning voor deze optie.

   In de voorbeelden in dit artikel wordt de bestandsshare-witness is op de Active Directory of DNS-server die wordt uitgevoerd in Azure. De bestandsshare-witness wordt domcontr-0 genoemd. Omdat u zou een VPN-verbinding naar Azure (via VPN-Gateway of Azure ExpressRoute) hebt geconfigureerd, wordt uw Active Directory of DNS-service wordt on-premises en is niet geschikt is voor het uitvoeren van een bestandsshare-witness.

   > [!NOTE]
   > Als uw Active Directory of DNS-service wordt uitgevoerd alleen on-premises, niet uw bestandsshare-witness configureren op de Active Directory of DNS-Windows-besturingssysteem die on-premises wordt uitgevoerd. De netwerklatentie tussen clusterknooppunten die worden uitgevoerd in Azure en Active Directory of DNS-on-premises mogelijk te groot zijn en leiden tot problemen met de netwerkverbinding. Zorg ervoor dat de bestandsshare-witness configureren op een Azure-machine die wordt uitgevoerd dicht bij het clusterknooppunt.  
   >
   >

   Het quorumstation moet ten minste 1024 MB aan vrije ruimte. Het wordt aangeraden de 2048 MB aan vrije ruimte voor de schijf quorum.

2. Het clusternaamobject toevoegen.

   ![Afbeelding 20: De machtigingen voor de share voor het cluster name object toewijzen][sap-ha-guide-figure-3019]

   _**Afbeelding 20:** De machtigingen voor de share voor het cluster name object toewijzen_

   Zorg ervoor dat de machtigingen voor de instantie voor het wijzigen van gegevens in de share voor het clusternaamobject (in ons voorbeeld pr1-ascs-vir$) zijn.

3. Selecteer om de clusternaamobject toe aan de lijst met **toevoegen**. Wijzig het filter om te controleren op computerobjecten, naast die wordt weergegeven in afbeelding 22.

   ![Afbeelding 21: Objecttypen om op te nemen van computers][sap-ha-guide-figure-3020]

   _**Afbeelding 21:** Wijziging **objecttypen** om op te nemen van computers_

   ![Afbeelding 22: Schakel het selectievakje voor Computers][sap-ha-guide-figure-3021]

   _**Afbeelding 22:** Selecteer de **Computers** selectievakje_

4. Voer het clusternaamobject zoals wordt weergegeven in afbeelding 21. Omdat de record is al is gemaakt, kunt u de machtigingen wijzigen, zoals wordt weergegeven in afbeelding 20.

5. Selecteer de **Security** tabblad van de share en stel vervolgens meer gedetailleerde machtigingen voor het clusternaamobject.

   ![Afbeelding 23: De beveiligingskenmerken van voor de clusternaamobject instellen op het bestandsshare quorum][sap-ha-guide-figure-3022]

   _**Afbeelding 23:** De beveiligingskenmerken van voor de clusternaamobject instellen op het bestandsshare quorum_

#### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Stel het quorum file share witness in Failoverclusterbeheer

1. Open de instelling-Wizard clusterquorum configureren.

   ![Afbeelding 24 uur per dag: Start de Wizard configureren clusterquorum-instelling][sap-ha-guide-figure-3023]

   _**Afbeelding 24 uur per dag:** Start de Wizard configureren clusterquorum-instelling_

2. Op de **optie voor quorumconfiguratie** pagina, selecteert u **selecteert u de quorumwitness**.

   ![Afbeelding 25: U kunt kiezen uit Quorumconfiguraties][sap-ha-guide-figure-3024]

   _**Afbeelding 25:** U kunt kiezen uit Quorumconfiguraties_

3. Op de **Quorumwitness selecteren** pagina, selecteert u **een bestandsshare-witness configureren**.

   ![Afbeelding 26: Selecteer de bestandsshare-witness][sap-ha-guide-figure-3025]

   _**Afbeelding 26:** Selecteer de bestandsshare-witness_

4. Geef het UNC-pad naar de bestandsshare (in ons voorbeeld \\domcontr 0\FSW). Voor een overzicht van de wijzigingen die u kunt maken, selecteert u **volgende**.

   ![Afbeelding 27: De bestandslocatie voor de share voor de share witness definiëren][sap-ha-guide-figure-3026]

   _**Afbeelding 27:** De bestandslocatie voor de share voor de share witness definiëren_

5. Selecteer de gewenste wijzigingen aan, en selecteer vervolgens **volgende**. U moet de configuratie van het cluster is opnieuw te configureren zoals wordt weergegeven in afbeelding 28:  

   ![Afbeelding 28: Bevestigen dat u het cluster hebt geconfigureerd][sap-ha-guide-figure-3027]

   _**Afbeelding 28:** Bevestigen dat u het cluster hebt geconfigureerd_

Nadat u het Windows-failovercluster hebt geïnstalleerd, moet u bepaalde drempelwaarden wijzigen zodat ze detectie van failover naar een in Azure aan te passen. De parameters moeten worden gewijzigd, worden beschreven in [afstemmen failover cluster netwerk drempelwaarden][tuning-failover-cluster-network-thresholds]. Ervan uitgaande dat de twee virtuele machines die gezamenlijk de configuratie van het Windows-cluster voor ASCS/SCS zich in hetzelfde subnet bevinden, wijzig de volgende parameters voor deze waarden:

- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Deze instellingen zijn getest met klanten en bieden een goed compromis. Ze zijn flexibel genoeg, maar ze bieden ook failover die snel genoeg in reële fouten in een SAP-software of in een knooppunt of de VM mislukken.

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> SIOS DataKeeper Cluster Edition voor het hulpprogramma voor het delen van de SAP ASCS/SCS-clusterschijf installeren

U hebt nu een werkende configuratie van Windows Server failover clustering in Azure. Als u wilt een SAP ASCS/SCS-exemplaar installeert, moet u een gedeelde schijf-resource. U kunt de gedeelde schijfresources, u moet maken in Azure. SIOS DataKeeper Cluster Edition is een oplossing van derden die u gebruiken kunt om resources van de gedeelde schijf te maken.

SIOS DataKeeper Cluster Edition voor het hulpprogramma voor het delen van de SAP ASCS/SCS-clusterschijf installeren, moet deze taken uitvoeren:

- Microsoft .NET Framework 3.5 toevoegen.
- SIOS DataKeeper installeren.
- SIOS DataKeeper instellen.

### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> .NET Framework 3.5 toevoegen
.NET framework 3.5 is niet automatisch geactiveerd of geïnstalleerd op Windows Server 2012 R2. Omdat SIOS DataKeeper .NET vereist op alle knooppunten waarop u DataKeeper installeert, moet u .NET Framework 3.5 installeren op het gastbesturingssysteem van alle virtuele machines in het cluster.

Er zijn twee manieren om toe te voegen van .NET Framework 3.5:

- Gebruik de toevoegen Wizard functies en onderdelen in Windows, zoals wordt weergegeven in afbeelding 29:

  ![Afbeelding 29: .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen][sap-ha-guide-figure-3028]

  _**Afbeelding 29:** .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen_

  ![Afbeelding 30: Voortgang van de installatie van de balk als u .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen][sap-ha-guide-figure-3029]

  _**Afbeelding 30:** Voortgang van de installatie van de balk als u .NET Framework 3.5 installeren met behulp van de toevoegen Wizard functies en onderdelen_

- Gebruik het opdrachtregelhulpprogramma dism.exe. Voor dit type installatie moet u toegang tot de SxS-map op de installatiemedia van Windows. Voer de volgende opdracht bij een opdrachtprompt met verhoogde bevoegdheid:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

### <a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper

SIOS DataKeeper Cluster Edition installeren op elk knooppunt in het cluster. Voor het maken van virtuele gedeelde opslag met SIOS DataKeeper, maakt een mirror gesynchroniseerd en vervolgens simuleren gedeelde clusteropslag.

Voordat u de SIOS software installeert, maakt u de domeingebruiker DataKeeperSvc.

> [!NOTE]
> De domeingebruiker DataKeeperSvc toevoegen aan de groep lokale Administrators op beide knooppunten van het cluster.
>
>

SIOS DataKeeper installeren:

1. De SIOS software installeren op beide knooppunten van het cluster.

   ![SIOS installatieprogramma][sap-ha-guide-figure-3030]

   ![Afbeelding 31: Eerste pagina van de SIOS DataKeeper-installatie][sap-ha-guide-figure-3031]

   _**Afbeelding 31:** Eerste pagina van de SIOS DataKeeper-installatie_

2. Selecteer in het dialoogvenster **Ja**.

   ![Afbeelding 32: DataKeeper wordt u geïnformeerd dat een service wordt uitgeschakeld][sap-ha-guide-figure-3032]

   _**Afbeelding 32:** DataKeeper wordt u geïnformeerd dat een service wordt uitgeschakeld_

3. In het dialoogvenster wordt aangeraden dat u selecteert **domein-of Server**.

   ![Afbeelding 33: Selectie van de gebruiker voor SIOS DataKeeper][sap-ha-guide-figure-3033]

   _**Afbeelding 33:** Selectie van de gebruiker voor SIOS DataKeeper_

4. Voer de account domeingebruikersnaam en wachtwoord die u hebt gemaakt voor SIOS DataKeeper.

   ![Afbeelding 34: De domeingebruikersnaam en wachtwoord invoeren voor de installatie van de SIOS DataKeeper][sap-ha-guide-figure-3034]

   _**Afbeelding 34:** De domeingebruikersnaam en wachtwoord invoeren voor de installatie van de SIOS DataKeeper_

5. Installeer de licentiesleutel voor uw SIOS DataKeeper-exemplaar, zoals wordt weergegeven in afbeelding 35.

   ![Afbeelding 35: Voer uw sleutel SIOS DataKeeper-licentie][sap-ha-guide-figure-3035]

   _**Afbeelding 35:** Voer uw sleutel SIOS DataKeeper-licentie_

6. Wanneer u hierom wordt gevraagd, start u de virtuele machine.

### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> SIOS DataKeeper instellen

Nadat u SIOS DataKeeper op beide knooppunten hebt geïnstalleerd, start u de configuratie. Het doel van de configuratie is dat synchrone gegevensreplicatie tussen de extra schijven die zijn gekoppeld aan elk van de virtuele machines.

1. Start het configuratiehulpprogramma DataKeeper-beheer en de configuratie en selecteer vervolgens **verbinding maken met Server**.

   ![Afbeelding 36: SIOS DataKeeper beheer en de configuratie-hulpprogramma][sap-ha-guide-figure-3036]

   _**Afbeelding 36:** SIOS DataKeeper beheer en de configuratie-hulpprogramma_

2. Voer de naam of de TCP/IP-adres van het eerste knooppunt dat het hulpprogramma voor beheer en de configuratie verbinding met, en, in een tweede stap van het tweede knooppunt maken moet.

   ![Afbeelding 37: De naam van de invoegen of TCP/IP-adres van het eerste knooppunt van het beheer en het hulpprogramma voor serverconfiguratie verbinding moet maken, en met een tweede stap van het tweede knooppunt][sap-ha-guide-figure-3037]

   _**Afbeelding 37:** De naam van de invoegen of TCP/IP-adres van het eerste knooppunt van het beheer en het hulpprogramma voor serverconfiguratie verbinding moet maken, en met een tweede stap van het tweede knooppunt_

3. De replicatietaak tussen de twee knooppunten maken.

   ![Afbeelding 38: Een replicatietaak maken][sap-ha-guide-figure-3038]

   _**Afbeelding 38:** Een replicatietaak maken_

   Een wizard begeleidt u bij het proces voor het maken van een replicatietaak.

4. De naam van de replicatietaak definiëren.

   ![Afbeelding 39: De naam van de replicatietaak definiëren][sap-ha-guide-figure-3039]

   _**Afbeelding 39:** De naam van de replicatietaak definiëren_

   ![Afbeelding 40: De basisgegevens van het knooppunt, het huidige knooppunt voor de gegevensbron moet definiëren][sap-ha-guide-figure-3040]

   _**Afbeelding 40:** De basisgegevens van het knooppunt, het huidige knooppunt voor de gegevensbron moet definiëren_

5. Definieer de naam, de TCP/IP-adres en het schijfvolume van het doelknooppunt.

   ![Afbeelding 41: De naam, de TCP/IP-adres en het schijfvolume van het huidige doelknooppunt definiëren][sap-ha-guide-figure-3041]

   _**Afbeelding 41:** De naam, de TCP/IP-adres en het schijfvolume van het huidige doelknooppunt definiëren_

6. Definieer de compressie-algoritmen. In ons voorbeeld is het raadzaam dat u de replicatie-stream comprimeren. Met name in situaties opnieuw synchroniseren minder de compressie van de stroom van de replicatie veel tijd opnieuw synchroniseren. Compressie maakt gebruik van de resources CPU en RAM-geheugen van een virtuele machine. Als de snelheid met compressie toeneemt, loopt u het volume van de CPU-resources die worden gebruikt. U kunt aanpassen om deze instelling later opnieuw.

7. Een andere instelling die u nodig hebt om te controleren is of de replicatie synchroon of asynchroon. Wanneer u SAP ASCS/SCS-configuraties beveiligen, moet u synchrone replicatie.  

   ![Afbeelding 42: Replicatiedetails definiëren][sap-ha-guide-figure-3042]

   _**Afbeelding 42:** Replicatiedetails definiëren_

8. Definiëren of het volume dat door de replicatietaak voor wordt gerepliceerd naar een Windows Server failover cluster-configuratie als een gedeelde schijf moet worden weergegeven. Selecteer voor de configuratie SAP ASCS/SCS **Ja** zodat het Windows-cluster ziet de gerepliceerd volume als een gedeelde schijf die kan worden gebruikt als een clustervolume.

   ![Afbeelding 43: Selecteer Ja als u wilt het gerepliceerde volume instellen als een clustervolume van][sap-ha-guide-figure-3043]

   _**Afbeelding 43:** Selecteer **Ja** gerepliceerd volume instellen als een clustervolume_

   Nadat het volume is gemaakt, ziet u het hulpprogramma DataKeeper-beheer en de configuratie of de replicatietaak actief is.

   ![Afbeelding 44: Synchroon spiegelen van DataKeeper voor de schijf van de share SAP ASCS/SCS is actief][sap-ha-guide-figure-3044]

   _**Afbeelding 44:** Synchroon spiegelen van DataKeeper voor de schijf van de share SAP ASCS/SCS is actief_

   Failover Cluster Manager bevat nu de schijf als een schijf DataKeeper, zoals wordt weergegeven in afbeelding 45:

   ![Afbeelding 45: Failover-clusterbeheer ziet u de schijf dat DataKeeper gerepliceerd][sap-ha-guide-figure-3045]

   _**Afbeelding 45:** Failover-clusterbeheer ziet u de schijf dat DataKeeper gerepliceerd_

## <a name="next-steps"></a>Volgende stappen

* [Installatie van SAP NetWeaver hoge beschikbaarheid met behulp van een Windows-failovercluster en een gedeelde schijf voor een SAP ASCS/SCS-exemplaar][sap-high-availability-installation-wsfc-shared-disk]
