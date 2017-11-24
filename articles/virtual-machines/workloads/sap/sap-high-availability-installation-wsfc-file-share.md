---
title: SAP NetWeaver maximaal beschikbare installatie op een Windows failover cluster en bestandsshare voor SAP ASC's / SCS-exemplaren in Azure | Microsoft Docs
description: SAP NetWeaver maximaal beschikbare installatie op een Windows failover cluster en bestandsshare voor SAP ASC's / SCS exemplaren
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 71296618-673b-4093-ab17-b7a80df6e9ac
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc957ece0250d233db9cec4f1fdd8b063c13a136
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="install-sap-netweaver-high-availability-on-a-windows-failover-cluster-and-file-share-for-sap-ascsscs-instances-on-azure"></a>SAP NetWeaver hoge beschikbaarheid installeren op een Windows failover cluster en bestandsshare voor SAP ASC's / SCS-exemplaren op Azure

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1596496]:https://launchpad.support.sap.com/#/notes/1596496

[sap-installation-guides]:http://service.sap.com/instguides

[sap-powershell-scrips]:https://github.com/Azure-Samples/sap-powershell

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[sofs-overview]:https://technet.microsoft.com/library/hh831349(v=ws.11).aspx
[new-in-win-2016-storage]:https://docs.microsoft.com/windows-server/storage/whats-new-in-storage

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[sap-blog-new-sap-cluster-resource-dll]:https://blogs.sap.com/2017/08/28/new-sap-cluster-resource-dll-is-available/
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host]:sap-high-availability-installation-wsfc-shared-disk.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-high-availability-installation-wsfc-shared-disk-add-probe-port]:sap-high-availability-installation-wsfc-shared-disk.md#10822f4f-32e7-4871-b63a-9b86c76ce761

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-official-ha-file-share-document]:https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


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


[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

In dit artikel wordt beschreven hoe installeren en configureren van een hoge beschikbaarheid SAP-systeem op Azure, met Windows Server Failover Cluster (WSFC) en Scale-Out File Server als een optie voor clustering SAP ASC's / SCS exemplaren.

## <a name="prerequisites"></a>Vereisten

Voordat u de installatie start, controleert u de volgende artikelen:

* [Handleiding voor de referentiearchitectuur: Cluster een SAP ASC's / SCS-exemplaar op een Windows-failovercluster met behulp van de bestandsshare][sap-high-availability-guide-wsfc-file-share]

* [Azure-infrastructuur SAP hoge beschikbaarheid met behulp van een Windows failover cluster en de bestandsshare voor SAP ASC's / SCS exemplaren voorbereiden][sap-high-availability-infrastructure-wsfc-file-share]

U moet de volgende uitvoerbare programma's en dll-bestanden van het SAP:
* SAP Software inrichting Manager (SWPM) installatie tool versie SPS21 of hoger.
* Download de meest recente NTCLUST. SAR-archief met nieuwe SAP-clusterbron-DLL. De nieuwe SAP-cluster-dll's ondersteunen SAP ASC's / SCS hoge beschikbaarheid met bestandsshare voor Windows Server Failover Cluster.

  Zie voor meer informatie over de nieuwe SAP-clusterbron-DLL, deze blog: [nieuwe SAP-clusterbron-DLL is beschikbaar!] [sap-blog-new-sap-cluster-resource-dll].

We beschrijven de installatie van de Database Management System (DBMS) niet omdat de instellingen, is afhankelijk van de DBMS die u gebruikt. Echter, gaan we ervan uit dat hoge beschikbaarheid weergegeven met de DBMS worden aangepakt met de functies die verschillende DBMS leveranciers ondersteuning voor Azure bieden. Deze functies omvatten AlwaysOn of database mirroring voor SQL Server en Oracle Data Guard voor Oracle-databases. In het scenario dat in dit artikel we gebruiken toevoegen niet we meer beveiliging aan de DBMS.

Er zijn geen speciale overwegingen bij verschillende DBMS services met dit soort clusterconfiguratie SAP ASC's / SCS in Azure communiceren.

> [!NOTE]
> De installatieprocedures van SAP NetWeaver ABAP systemen, Java-systemen en ABAP + Java systemen zijn bijna identiek. Het belangrijkste verschil is dat een systeem SAP ABAP één exemplaar van ASC's heeft. Het systeem SAP Java heeft één SCS-exemplaar. Het systeem SAP ABAP + Java heeft één exemplaar van ASC's en één SCS-exemplaar in de groep dezelfde Microsoft failover-cluster. Eventuele verschillen installatie voor elke installatie SAP NetWeaver stack worden expliciet vermeld. Alle andere onderdelen zijn hetzelfde, kunt u aannemen.  
>
>

## <a name="install-an-ascsscs-instance-on-an-ascsscs-cluster"></a>Een exemplaar ASC's / SCS installeert op een cluster ASC's / SCS

> [!IMPORTANT]
>
> Een hoge beschikbaarheid-instelling met de configuratie van een share wordt momenteel niet ondersteund door het hulpprogramma SWPM SAP-installatie. Daarom is een aantal handmatige goedkeuring nodig voor het installeren van een SAP-systeem (bijvoorbeeld, installeren en het cluster een SAP ASC's / SCS-exemplaar en het configureren van een afzonderlijke SAP globale host).  
>
> Er is geen wijziging in andere installatiestappen installeren (en cluster) een DBMS-exemplaar en SAP-toepassingsservers.
>

### <a name="install-an-ascsscs-instance-on-your-local-drive"></a>Een exemplaar ASC's / SCS installeert op uw lokale schijf

Installeer een SAP ASC's / SCS-exemplaar op *beide* knooppunten van het cluster ASC's / SCS. Deze installeren op de lokale schijf. In ons voorbeeld is het lokale station C:\\, maar u kunt een lokaal station.  

Voor het installeren van het exemplaar in het hulpprogramma SAP SWPM installation, gaat u naar:

**\<Product >** > **\<DBMS >** > **installatie** > **toepassingsserver ABAP**(of **Java**) > **Distributed System** > **ASC's / SCS-exemplaar**

> [!IMPORTANT]
> Het scenario voor bestandsdeling wordt op dit moment niet ondersteund door het hulpprogramma SWPM SAP-installatie. U *kan niet worden gebruikt* het volgende installatiepad:
>
> **\<Product >** > **\<DBMS >** > **installatie** > **toepassingsserver ABAP**(of **Java**) > **hoge beschikbaarheid System** >...
>

### <a name="remove-sapmnt-and-create-an-saploc-file-share"></a>SAPMNT verwijderen en een bestandsshare SAPLOC maken

SWMP een lokale share SAPMNT gemaakt in de C:\\usr\\sap-map.

Verwijder de bestandsshare SAPMNT op *beide* ASC's / SCS clusterknooppunten.

Het volgende PowerShell-script uitvoeren:

```PowerShell
Remove-SmbShare sapmnt -ScopeName * -Force
 ```

Als de SAPLOC-share niet bestaat, maakt u een op *beide* ASC's / SCS clusterknooppunten.

Het volgende PowerShell-script uitvoeren:

```PowerShell
#Create SAPLOC share and set security
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"
$HostName = $env:computername
$SAPLocalAdminGroupName = "$HostName\SAP_LocalAdmin"
$SAPDisk = "C:"
$SAPusrSapPath = "$SAPDisk\usr\sap"

New-SmbShare -Name saploc -Path c:\usr\sap -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName , $SAPLocalAdminGroupName  
 ```

## <a name="prepare-an-sap-global-host-on-the-sofs-cluster"></a>Een globale SAP-host op het cluster SOFS voorbereiden

De volgende volume en bestandsshare maken op het cluster SOFS:

* SAP GLOBALHOST bestand C:\ClusterStorage\Volume1\usr\sap\\<SID>\SYS\ structuur op SOFS cluster gedeeld clustervolume (CSV)

* SAPMNT bestandsshare

* Beveiliging instellen voor de bestandsshare SAPMNT en de map met volledig beheer voor:
    * De \<DOMAIN > \SAP_\<SID > _GlobalAdmin-gebruikersgroep
    * De SAP ASC's / SCS cluster knooppunt computerobjecten \<DOMAIN > \ClusterNode1$ en \<DOMAIN > \ClusterNode2$

Als een CSV-volume maken met mirrortolerantie, uitvoeren van de volgende PowerShell-cmdlet op een van de clusterknooppunten SOFS:


```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR1 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```
Uitvoeren voor het maken van SAPMNT en instellen van de map en -share beveiliging, de volgende PowerShell-script op een van de clusterknooppunten SOFS:

```PowerShell
# Create SAPMNT on file share
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP ASCS/SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP ASCS/SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Create a SAPMNT file share and set share security
New-SmbShare -Name sapmnt -Path $UsrSAPFolder -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName, $ASCSClusterObjectNode1, $ASCSClusterObjectNode2 -ContinuouslyAvailable $false -CachingMode None -Verbose

# Get SAPMNT file share security settings
Get-SmbShareAccess sapmnt

# Set file and folder security
$Acl = Get-Acl $UsrSAPFolder

# Add a file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add  a security object of the clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add a security object of the clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
 ```
## <a name="stop-ascsscs-instances-and-sap-services"></a>ASC's / SCS-exemplaren en SAP-services stoppen

De volgende stappen uitvoeren:
1. Stop de SAP ASC's / SCS-exemplaren op beide clusterknooppunten ASC's / SCS.
2. SAP ASC's / SCS Windows-services stoppen **SAP\<SID > _\<InstanceNumber >** op beide clusterknooppunten.

## <a name="move-the-sys-folder-to-the-sofs-cluster"></a>Verplaats de \SYS\... map aan het cluster SOFS

De volgende stappen uitvoeren:
1. Kopieer de map SYS (bijvoorbeeld C:\usr\sap\\<SID>\SYS) van een van de ASC's / SCS clusterknooppunten aan het cluster SOFS (bijvoorbeeld naar C:\ClusterStorage\Volume1\usr\sap\\<SID>\SYS).
2. Verwijderen van de C:\usr\sap\\<SID>\SYS map vanaf beide clusterknooppunten ASC's / SCS.

## <a name="update-the-cluster-security-setting-on-the-sap-ascsscs-cluster"></a>De cluster-beveiligingsinstelling in het cluster SAP ASC's / SCS bijwerken

Het volgende PowerShell-script uitvoeren op een van de clusterknooppunten SAP ASC's / SCS:

```PowerShell
# Grant <DOMAIN>\SAP_<SID>_GlobalAdmin group access to the cluster

$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# Set full access for the <DOMAIN>\SAP_<SID>_GlobalAdmin group
Grant-ClusterAccess -User $SAPSIDGlobalAdminGroupName -Full

#Check security settings
Get-ClusterAccess
```

## <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance"></a>De naam van een virtuele host op voor het geclusterde exemplaar van de SAP ASC's / SCS maken

Maken van een SAP ASC's / SCS clusternetwerknaam (bijvoorbeeld **pr1-ASC's [10.0.6.7]**), zoals beschreven in [maken van een virtuele host-naam voor het geclusterde exemplaar van de SAP ASC's / SCS] [ sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host] .

## <a name="update-the-default-and-sap-ascsscs-instance-profile"></a>De standaard- en SAP ASC's / SCS exemplaar profiel bijwerken

Als u wilt gebruikmaken van de nieuwe virtuele SAP ASC's / SCS-hostnaam en SAP globale hostnaam, moet u de standaard- en SAP ASC's / SCS exemplaar profiel bijwerken \<SID >_ASC's / SCS\<Nr >_<Host>.


| Oude waarden |  |
| --- | --- |
| SAP ASC's / SCS hostnaam = globale SAP-host | ASC's-1 |
| Profielnaam SAP ASC's / SCS-exemplaar | PR1_ASCS00_ascs-1 |

| Nieuwe waarden |  |
| --- | --- |
| SAP ASC's / SCS-hostnaam | **PR1-ASC 's** |
| Globale SAP-host | **sapglobal** |
| Profielnaam SAP ASC's / SCS-exemplaar | PR1\_ASCS00\_**pr1-ASC's** |

### <a name="update-sap-default-profile"></a>SAP standaardprofiel bijwerken


| Parameternaam | Waarde van parameter |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| rdisp/mshost | **PR1-ASC 's** |
| CLR niet/serverhost | **PR1-ASC 's** |

### <a name="update-the-sap-ascsscs-instance-profile"></a>Het exemplaar SAP ASC's / SCS profiel bijwerken

| Parameternaam | Waarde van parameter |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| DIR_PROFILE | \\\sapglobal\sapmnt\PR1\SYS\profile |
| _PF | $(DIR_PROFILE) \PR1\_ASCS00_ pr1-ASC's |
| Restart_Program_02 local$(_MS) pf=$(_PF) = | **Start**_Program_02 local$(_MS) pf=$(_PF) = |
| SAPLOCALHOST | **PR1-ASC 's** |
| Restart_Program_03 local$(_EN) pf=$(_PF) = | **Start**_Program_03 local$(_EN) pf=$(_PF) = |
| GW/netstat_once | **0** |
| encni-CLR niet/set_so_keepalive  | **True** |
| Service/ha_check_node | **1** |

> [!IMPORTANT]
>U kunt de **Update SAPASCSSCSProfile** PowerShell-cmdlet voor het automatiseren van de Profielupdate.
>
>De PowerShell-cmdlet ondersteunt de SAP ABAP ASC's en de SAP Java SCS exemplaren.
>

Kopiëren [ **SAPScripts.psm1** ] [ sap-powershell-scrips] aan uw lokale station C:\tmp en voer de volgende PowerShell-cmdlet:

```PowerShell
Import-Module C:\tmp\SAPScripts.psm1

Update-SAPASCSSCSProfile -PathToAscsScsInstanceProfile \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_ascs-1 -NewASCSHostName pr1-ascs -NewSAPGlobalHostName sapglobal -Verbose  
```

![Afbeelding 1: SAPScripts.psm1 uitvoer][sap-ha-guide-figure-8012]

_**Afbeelding 1**: SAPScripts.psm1 uitvoer_

## <a name="update-the-sidadm-user-environment-variable"></a>Update de \<sid > omgevingsvariabele adm-gebruiker

1. Update de \<sid > adm gebruiker omgeving nieuwe GLOBALHOST UNC-pad op *beide* ASC's / SCS clusterknooppunten.
2. Meld u aan als \<sid > adm-gebruiker, en start het hulpprogramma Regedit.exe.
3. Ga naar **HKEY_CURRENT_USER** > **omgeving**, en werk vervolgens de variabelen voor de nieuwe waarde:

| Variabele | Waarde |
| --- | --- |
| RSEC_SSFS_DATAPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\data |
| RSEC_SSFS_KEYPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\key |
| SAPEXE | \\\\**sapglobal**\sapmnt\PR1\SYS\exe\uc\NTAMD64 |
| SAPLOCALHOST  | **PR1-ASC 's** |


## <a name="install-a-new-saprcdll-file"></a>Een nieuw bestand met saprc.dll installeren

1. Installeer een nieuwe versie van de SAP-clusterbron die ondersteuning biedt voor het scenario voor bestandsdeling.

2. Download de meest recente NTCLUST. SAR pakket van de Marketplace SAP-Service.

3. NTCLUS uitpakken. SAR op een van de ASC's / SCS clusterknooppunten, en voer de volgende opdracht vanaf de opdrachtprompt om de nieuwe saprc.dll-bestand te installeren:

```
.\NTCLUST\insaprct.exe -yes -install
```

Het nieuwe saprc.dll-bestand is op beide clusterknooppunten ASC's / SCS geïnstalleerd.

Zie voor meer informatie [1596496 SAP-notitie - SAP brontype dll's bijwerken voor Cluster Resource Monitor][1596496].

## <a name="create-a-sap-sid-cluster-group-network-name-and-ip"></a>Maken van een SAP <SID> groep, de netwerknaam van het en IP-cluster

Maken van een SAP \<SID > clustergroep, een ASC's / SCS netwerknaam en een bijbehorende IP-adres, voer de volgende PowerShell-cmdlet:

```PowerShell
# Create SAP Cluster Group
$SAPSID = "PR1"
$SAPClusterGroupName = "SAP $SAPSID"
$SAPIPClusterResourceName = "SAP $SAPSID IP"
$SAPASCSNetworkName = "pr1-ascs"
$SAPASCSIPAddress = "10.0.6.7"
$SAPASCSSubnetMask = "255.255.255.0"

# Create an SAP ASCS instance virtual IP cluster resource
Add-ClusterGroup -Name $SAPClusterGroupName -Verbose

#Create an SAP ASCS virtual IP address
$SAPIPClusterResource = Add-ClusterResource -Name $SAPIPClusterResourceName -ResourceType "IP Address" -Group $SAPClusterGroupName -Verbose

# Set a static IP address
$param1 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,Address,$SAPASCSIPAddress
$param2 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,SubnetMask,$SAPASCSSubnetMask
$params = $param1,$param2
$params | Set-ClusterParameter

# Create a corresponding network name
$SAPNetworkNameClusterResourceName = $SAPASCSNetworkName
Add-ClusterResource -Name $SAPNetworkNameClusterResourceName -ResourceType "Network Name" -Group $SAPClusterGroupName -Verbose

# Set a network DNS name
$SAPNetworkNameClusterResource = Get-ClusterResource $SAPNetworkNameClusterResourceName
$SAPNetworkNameClusterResource | Set-ClusterParameter -Name Name -Value $SAPASCSNetworkName

#Check the updated values
$SAPNetworkNameClusterResource | Get-ClusterParameter

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPNetworkNameClusterResourceName -Dependency "[$SAPIPClusterResourceName]" -Verbose

#Start an SAP <SID> cluster group
Start-ClusterGroup -Name $SAPClusterGroupName -Verbose
```

## <a name="register-the-sap-start-service-on-both-nodes"></a>De SAP start-service op beide knooppunten registreren

De SAP ASC's / SCS start-service om te verwijzen naar het nieuwe profiel en profielpad opnieuw te registreren.

Deze herregistratie moet worden uitgevoerd op *beide* ASC's / SCS clusterknooppunten.

Voer de volgende opdracht in de opdrachtprompt met verhoogde bevoegdheid:

```
C:\usr\sap\PR1\ASCS00\exe\sapstartsrv.exe -r -p \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_pr1-ascs -s PR1 -n 00 -U SAPCLUSTER\SAPServicePR1 -P mypasswd12 -e SAPCLUSTER\pr1adm
```

![Afbeelding 2: SAP-service opnieuw installeren][sap-ha-guide-figure-8013]

_**Afbeelding 2**: Installeer SAP-service_

Zorg ervoor dat de parameters juist zijn en selecteer vervolgens **handmatige** als de **opstarttype**.

## <a name="stop-the-ascsscs-service"></a>Stop de service ASC's / SCS

Stop de service SAP ASC's / SCS SAP\<SID > _\<InstanceNumber > op beide ASC's / SCS clusterknooppunten.

## <a name="create-a-new-sap-service-and-sap-instance-resources"></a>Maak een nieuwe SAP-service en SAP instance-bronnen

Voor het voltooien van het maken van resources van het SAP SAP\<SID > de clustergroep, maakt u de volgende bronnen:

* SAP \<SID > \<InstanceNumber > service
* SAP \<SID > \<InstanceNumber > exemplaar

Voer de volgende PowerShell-cmdlet:

```PowerShell
$SAPSID = "PR1"
$SAPInstanceNumber = "00"
$SAPNetworkNameClusterResourceName = "pr1-ascs"

$SAPServiceName = "SAP$SAPSID"+ "_" + $SAPInstanceNumber

$SAPClusterGroupName = "SAP $SAPSID"
$SAPServiceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Service"

$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPServiceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Service" -SeparateMonitor -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name ServiceName -Value $SAPServiceName

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPNetworkNameClusterResourceName]" -Verbose

$SAPInstanceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Instance"

# Create SAP instance cluster resource
$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPInstanceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Resource" -SeparateMonitor -Verbose

#Set SAP instance cluster resource parameters
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystemName -Value $SAPSID -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystem -Value $SAPInstanceNumber -Verbose

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPServiceClusterResourceName]" -Verbose
```

## <a name="add-a-probe-port"></a>Een testpoort toevoegen

Een SAP-clusterbron de test SAP-SID-IP-poort configureren met behulp van PowerShell. Deze configuratie niet uitvoeren op een van de clusterknooppunten SAP ASC's / SCS zoals beschreven [in dit artikel][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].

## <a name="install-an-ers-instance-on-both-cluster-nodes"></a>Een exemplaar Ebruikers installeren op beide clusterknooppunten

Installatie van een wachtrij plaatsen replicatie Server (gebruikers)-exemplaar op *beide* knooppunten van het cluster ASC's / SCS. Ga als volgt dit installatiepad in het menu SWPM:

**\<Product >** > **\<DBMS >** > **installatie** > **extra SAP-systeem exemplaren**  >  **Replicatie Server-exemplaar in de wachtrij plaatsen**

## <a name="install-a-dbms-instance-and-sap-application-servers"></a>Een exemplaar DBMS en SAP-toepassingsservers installeren

De installatie van het systeem SAP voltooien door te installeren:
* Een DBMS-exemplaar.
* Een primaire SAP-toepassingsserver.
* Een extra SAP-toepassingsserver.

## <a name="next-steps"></a>Volgende stappen

* [Een exemplaar ASC's / SCS installeert op een failovercluster zonder gedeelde schijven - officiële SAP-richtlijnen voor hoge beschikbaarheid bestandsshare][sap-official-ha-file-share-document]

* [Opslagruimten Direct in WindowsServer 2016][s2d-in-win-2016]

* [Scale-Out File Server for application data overview][sofs-overview]

* [Wat is er nieuw in de opslag in Windows Server 2016][new-in-win-2016-storage]
