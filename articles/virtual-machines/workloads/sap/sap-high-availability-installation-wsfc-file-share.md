---
title: SAP NetWeaver HA-installatie op Windows-failovercluster en de bestandsnaam (A) SCS-exemplaar op Azure delen voor SAP | Microsoft Docs
description: SAP NetWeaver HA-installatie op Windows-failovercluster en bestandsshare voor SAP (A) SCS exemplaar
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
ms.openlocfilehash: ec7888cfb9b0d288b5b25f580c852ee32306684c
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="sap-netweaver-ha-installation-on-windows-failover-cluster-and-file-share-for-sap-ascs-instance-on-azure"></a>SAP NetWeaver HA-installatie op Windows-failovercluster en bestandsshare voor SAP (A) SCS exemplaar op Azure

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1596496]:https://launchpad.support.sap.com/#/notes/1596496

[sap-installation-guides]:http://service.sap.com/instguides

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

Dit document wordt beschreven hoe u installeren en configureren van hoge beschikbaar SAP-systeem in Azure, met **Windows Failover Cluster (WSFC)** en **Scale-Out bestandsshare** als een optie voor SAP (A) SCS clustering exemplaar.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat deze documenten controleren voordat u begint met de installatie:

* [Handleiding voor de referentiearchitectuur - Clustering SAP-(A) SCS-exemplaar op **Windows Failover Cluster** met **bestandsshare**][sap-high-availability-guide-wsfc-file-share]

* [De voorbereiding van het Azure-infrastructuur voor SAP HA met behulp van **Windows Failover Cluster** en **bestand gedeeld** voor SAP (A) SCS exemplaar][sap-high-availability-infrastructure-wsfc-file-share]


U moet volgen uitvoerbare bestanden / dll-bestanden van het SAP:
* SAP **Software inrichting Manager** (**SWPM**) versie van het hulpprogramma installatie **SPS21 (of hoger)**.
* Download de **nieuwste NTCLUST. SAR** archief met nieuwe SAP-clusterbron-DLL. De nieuwe SAP-cluster dll-bestanden ondersteunt SAP (A) SCS hoge beschikbaarheid met bestandsshare op Windows Server Failover-Cluster.

  Raadpleeg dit blog voor meer oprichting op nieuwe SAP-clusterbron-DLL: [nieuwe SAP-clusterbron-DLL beschikbaar is.][sap-blog-new-sap-cluster-resource-dll]

We beschrijven de DBMS-instellingen niet omdat de instellingen, is afhankelijk van de DBMS-systeem die u gebruikt. Echter, gaan we ervan uit dat hoge beschikbaarheid weergegeven met de DBMS worden aangepakt met de functies van die de verschillende DBMS leveranciers ondersteuning voor Azure. Bijvoorbeeld altijd op of database mirroring voor SQL Server en Oracle Data Guard voor Oracle-databases. In het scenario dat in dit artikel we gebruiken toevoegen niet we meer beveiliging aan de DBMS.

Er zijn geen speciale overwegingen bij verschillende DBMS services met dit soort clusterconfiguratie SAP ASC's / SCS in Azure communiceren.

> [!NOTE]
> De installatieprocedures van SAP NetWeaver ABAP systemen, Java-systemen en ABAP + Java systemen zijn bijna identiek. Het belangrijkste verschil is dat een systeem SAP ABAP één exemplaar van ASC's heeft. Het systeem SAP Java heeft één SCS-exemplaar. Het systeem SAP ABAP + Java heeft één exemplaar van ASC's en één SCS-exemplaar in de groep dezelfde Microsoft failover-cluster. Eventuele verschillen installatie voor elke installatie SAP NetWeaver stack worden expliciet vermeld. Alle andere onderdelen zijn hetzelfde, kunt u aannemen.  
>
>

## <a name="install-ascs-instance-on-ascs-cluster"></a>(A) SCS-exemplaar installeren op (A) SCS-Cluster

> [!IMPORTANT]
>
>Een HA-instelling met de configuratie van een share wordt momenteel niet ondersteund door het hulpprogramma voor SAP installatie Software inrichting Manager (SWPM). Daarom is een aantal handmatige goedkeuring nodig zijn voor het installeren van een SAP-systeem, bijvoorbeeld, voor het installeren en het cluster SCS SAP (A)-exemplaar en het configureren van afzonderlijke SAP GLOBALHOST.  
>
>Er is geen wijziging in andere installatiestappen DBMS-exemplaar installeren (en cluster) en SAP-toepassingsservers.
>

### <a name="install-ascs-instance-on-local-drive"></a>(A) SCS-exemplaar installeren op een lokaal station

Installatie SAP (A) SCS exemplaar op **beide** knooppunten van (A) SCS-cluster. Installeer het op **lokale** station. In ons voorbeeld we gekozen lokaal station C: is\\. U kunt een lokaal station.  

Voor het installeren van navigeren in de installatie van de SAP hulpprogramma SWPM naar:

&lt;Product&gt; -> &lt;DBMS&gt; -> installatie -> Application Server ABAP (of Java) -> gedistribueerde systeem -> (A) SCS-exemplaar

> [!IMPORTANT]
>Op dit moment file share scenario is nog niet ondersteund door het installatiehulpprogramma SAP SWPM **kan niet worden gebruikt** installatiepad:
>
>&lt;Product&gt; -> &lt;DBMS&gt; -> installatie -> Application Server ABAP (of Java) -> hoge beschikbaarheid systeem ->...
>

### <a name="remove-sapmnt-and-create-saploc-file-share"></a>Verwijder SAPMNT en SAPLOC bestandsshare maken

SWMP SAPMNT lokale share gemaakt op C:\\usr\\sap-map.

Verwijder de share bestanden SAPMNT op **beide** (A) SCS clusterknooppunten:

Volgende PowerShell-script uitvoeren:

```PowerShell
Remove-SmbShare sapmnt -ScopeName * -Force
 ```

Als SAPLOC share niet bestaat, maken op de clusterknooppunten zowel ASC's.

Volgende PowerShell-script uitvoeren:

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

## <a name="prepare-sap-global-host-on-sofs-cluster"></a>GLOBALE SAP-HOST op SOFS Cluster voorbereiden

In deze stap maakt u de volgende volume en bestandsshare op het cluster SOFS:

* SAP GLOBALHOST bestand C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS\ structuur op SOFS cluster gedeeld clustervolume (CSV)

* SAPMNT bestandsshare maken

* Beveiliging instellen voor SAPMNT bestandsshare en de map met volledig beheer voor:
    * **&lt;DOMEIN&gt;\SAP_&lt;SID&gt;_GlobalAdmin** gebruikersgroep
    * SAP-(A) knooppunten SCS Cluster computer **objecten &lt;domein&gt;\ClusterNode1$ en &lt;domein&gt;\ClusterNode2$**

Maken van CSV-volume met mirrortolerantie zoals gedefinieerd in hoofdstuk **SAP-vereisten voor SOFS in Azure - koppeling toevoegen**, volgende PowerShell-cmdlet op een van de clusterknooppunten SOFS uitvoeren:


```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR1 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```
Uitvoeren voor het maken van SAPMNT en stel het beveiligingsniveau map en de share, volgende PowerShell-script op een van de clusterknooppunten SOFS:

```PowerShell
# Create SAPMNT on file share
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP (A)SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP (A)SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Create SAPMNT file share and set share security
New-SmbShare -Name sapmnt -Path $UsrSAPFolder -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName, $ASCSClusterObjectNode1, $ASCSClusterObjectNode2 -ContinuouslyAvailable $false -CachingMode None -Verbose

# Get SAPMNT file share security settings
Get-SmbShareAccess sapmnt

# Set files & folder security
$Acl = Get-Acl $UsrSAPFolder

# Add file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
 ```
## <a name="stop-ascs-instances-and-sap-services"></a>Stop (A) SCS-exemplaren en SAP-Services

Uitvoeren van volgende stappen uit:
* SAP (A) SCS-exemplaren op beide (A) SCS clusterknooppunten stoppen
* SAP (A) SCS Windows-services stoppen **SAP&lt;SID&gt;_&lt;InstanceNumber&gt;**  op beide clusterknooppunten

## <a name="move-sys-folder-to-sofs-cluster"></a>Verplaatsen van \SYS\.... Map voor SOFS-Cluster

Uitvoeren van volgende stappen uit:
* De map SYS kopiëren (bijvoorbeeld C:\usr\sap\\&lt;SID&gt;\SYS) van een van de SCS (A) clusterknooppunten op SOFS cluster bijvoorbeeld C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS
* Verwijderen van C:\usr\sap\\&lt;SID&gt;\SYS map vanaf beide knooppunten (A) SCS van

## <a name="update-cluster-security-setting-on-sap-ascs-cluster"></a>Cluster-beveiligingsinstelling op SAP (A) SCS Cluster bijwerken

Het volgende PowerShell-script uitvoeren op een van de clusterknooppunten SCS SAP (A):

```PowerShell
# Grant <DOMAIN>\SAP_<SID>_GlobalAdmin group access to cluster

$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# Set full access for <DOMAIn>\SAP_<SID>_GlobalAdmin group
Grant-ClusterAccess -User $SAPSIDGlobalAdminGroupName -Full

#Check security settings
Get-ClusterAccess
```

## <a name="create-a-virtual-host-name-for-the-clustered-sap-ascs-instance"></a>Maak een virtuele Host-naam voor het exemplaar van de SCS geclusterde SAP (A)

Zoals beschreven in hoofdstuk [maken van een virtuele host-naam voor het geclusterde exemplaar van de SAP ASC's / SCS] [ sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host] , zoals SAP (A) SCS clusternetwerknaam maken **pr1-ASC's [10.0.6.7]**

## <a name="update-default-and-sap-ascs-instance-profile"></a>Standaard bijwerken en SAP (A)-exemplaar SCS profiel

U moet de STANDAARDWAARDE bijwerken en het SAP (A) SCS exemplaar profiel &lt;SID&gt;_(A) SCS<Nr>_  <Host> te gebruiken:

* Naam van de virtuele host nieuwe SCS SAP (A)

* Nieuwe SAP globale hostnaam


| Oude waarden |  |
| --- | --- |
| SAP-(A) hostnaam SCS = globale SAP-Host | ASC's-1 |
| SAP-(A) profielnaam SCS-exemplaar | PR1_ASCS00_ascs-1 |

| Nieuwe waarden |  |
| --- | --- |
| SAP-(A) SCS-hostnaam | **PR1-ASC 's** |
| Globale SAP-Host | **sapglobal** |
| SAP-(A) profielnaam SCS-exemplaar | PR1\_ASCS00\_**pr1-ASC's** |

### <a name="update-sap-default-profile"></a>SAP standaardprofiel bijwerken


| Parameternaam | Waarde van parameter |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| rdisp/mshost | **PR1-ASC 's** |
| CLR niet/serverhost | **PR1-ASC 's** |

### <a name="update-sap-ascs-instance-profile"></a>SAP (A) SCS exemplaar profiel bijwerken

| Parameternaam | Waarde van parameter |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| DIR_PROFILE | \\\\**sapglobal**\sapmnt\PR1\SYS\profile |
| _PF | $(DIR_PROFILE) \PR1\_ASCS00_ pr1-ASC's |
| Restart_Program_02 local$(_MS) pf=$(_PF) = | **Start**_Program_02 local$(_MS) pf=$(_PF) = |
| SAPLOCALHOST | **PR1-ASC 's** |
| Restart_Program_03 local$(_EN) pf=$(_PF) = | **Start**_Program_03 local$(_EN) pf=$(_PF) = |
| GW/netstat_once | **0** |
| encni-CLR niet/set_so_keepalive  | **True** |
| Service/ha_check_node | **1** |

> [!IMPORTANT]
>U kunt **Update SAPASCSSCSProfile** PowerShell-cmdlet voor het automatiseren van profiel bijwerken
>
>PowerShell-cmdlet ondersteunt zowel SAP ABAP ASC's en SAP Java SCS-exemplaar.
>

Kopiëren **SAPScripts.ps1** aan lokale station C:\tmp en volgende PowerShell-cmdlet uitvoeren:

```PowerShell
Import-Module C:\tmp\SAPScripts.ps1

Update-SAPASCSSCSProfile -PathToAscsScsInstanceProfile \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_ascs-1 -NewASCSHostName pr1-ascs -NewSAPGlobalHostName sapglobal -Verbose  
```

![Afbeelding 1: SAPScripts.ps1 uitvoer][sap-ha-guide-figure-8012]

_**Afbeelding 1:** SAPScripts.ps1 uitvoer_

## <a name="update-ltsidgtadm-user-environment-variable"></a>Update &lt;sid&gt;adm gebruiker omgevingsvariabele

Update &lt;sid&gt;adm gebruiker omgeving nieuwe GLOBALHOST UNC-pad op de clusterknooppunten SCS zowel (A).
Meld u aan als &lt;sid&gt;adm gebruiker en start Regedit.exe hulpprogramma.
Ga naar **HKEY_CURRENT_USER** -> **omgeving** en variabelen te werken naar de nieuwe waarde:

| Variabele | Waarde |
| --- | --- |
| RSEC_SSFS_DATAPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\data |
| RSEC_SSFS_KEYPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\key |
| SAPEXE | \\\\**sapglobal**\sapmnt\PR1\SYS\exe\uc\NTAMD64 |
| SAPLOCALHOST  | **PR1-ASC 's** |


## <a name="install-new-saprcdll"></a>Nieuwe SAPRC installeren. DLL-BESTAND

U moet een nieuwe versie van de SAP-clusterbron die ondersteuning biedt voor bestandsshare scenario installeren.

Download het meest recente **NTCLUST. SAR** pakket uit SAP Service marktplaats.

NTCLUS uitpakken. SAR op een van de SCS (A) clusterknooppunten en voer de volgende opdracht uit vanaf de opdrachtprompt voor het installeren van nieuwe saprc.dll:

```
.\NTCLUST\insaprct.exe -yes -install
```

De nieuwe saprc.dll wordt geïnstalleerd op beide (A) SCS clusterknooppunten.

Zie voor meer informatie [1596496 SAP-notitie - SAP Resource Type dll-bestanden voor Cluster Broncontrole bijwerken][1596496].

## <a name="create-sap-sid-cluster-group-network-name-and-ip"></a>SAP maken <SID> clustergroep, de netwerknaam van het en IP-

U moet maken:

* SAP &lt;SID&gt; clustergroep
* < (a) SCSNetworkName >
* en bijbehorende IP-adres

Voer de volgende PowerShell-cmdlet:

```PowerShell
# Create SAP Cluster Group
$SAPSID = "PR1"
$SAPClusterGroupName = "SAP $SAPSID"
$SAPIPClusterResourceName = "SAP $SAPSID IP"
$SAPASCSNetworkName = "pr1-ascs"
$SAPASCSIPAddress = "10.0.6.7"
$SAPASCSSubnetMask = "255.255.255.0"

# Create SAP ASCS instance Virtual IP cluster resource
Add-ClusterGroup -Name $SAPClusterGroupName -Verbose

#Create SAP ASCS Virtual IP Address
$SAPIPClusterResource = Add-ClusterResource -Name $SAPIPClusterResourceName -ResourceType "IP Address" -Group $SAPClusterGroupName -Verbose

# Set static IP Address
$param1 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,Address,$SAPASCSIPAddress
$param2 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,SubnetMask,$SAPASCSSubnetMask
$params = $param1,$param2
$params | Set-ClusterParameter

# Create corresponding network name
$SAPNetworkNameClusterResourceName = $SAPASCSNetworkName
Add-ClusterResource -Name $SAPNetworkNameClusterResourceName -ResourceType "Network Name" -Group $SAPClusterGroupName -Verbose

# Set Network DNS Name
$SAPNetworkNameClusterResource = Get-ClusterResource $SAPNetworkNameClusterResourceName
$SAPNetworkNameClusterResource | Set-ClusterParameter -Name Name -Value $SAPASCSNetworkName

#Check the updated values
$SAPNetworkNameClusterResource | Get-ClusterParameter

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPNetworkNameClusterResourceName -Dependency "[$SAPIPClusterResourceName]" -Verbose

#Start SAP <SID> Cluster Group
Start-ClusterGroup -Name $SAPClusterGroupName -Verbose
```

## <a name="register-sap-start-service-on-both-nodes"></a>SAP START-Service op beide knooppunten registreren

U moet pint naar het nieuwe profiel en profielpad SAP (A) SCS sapstart service opnieuw te registreren.

U moet zijn op zowel (A) SCS clusterknooppunten worden uitgevoerd.

Formulier de opdracht met verhoogde bevoegdheid vragen volgende opdracht uitvoeren:

```
C:\usr\sap\PR1\ASCS00\exe\sapstartsrv.exe -r -p \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_pr1-ascs -s PR1 -n 00 -U SAPCLUSTER\SAPServicePR1 -P mypasswd12 -e SAPCLUSTER\pr1adm
```

![Afbeelding 2: SAP-service opnieuw installeren][sap-ha-guide-figure-8013]

_**Afbeelding 2:** Reinstall SAP service_

Zorgt ervoor dat de parameters juist zijn en kies **handmatige** als opstarten Type.

## <a name="stop-ascs-service"></a>(A) SCS-Service stoppen

SAP (A) SCS-service stoppen **SAP&lt;SID&gt;_ &lt;InstanceNumber&gt;**  op beide (A) SCS clusterknooppunten.

## <a name="create-new-sap-service-and-sap-instance-resources"></a>Nieuwe SAP-Service en SAP exemplaar Resources

Nu moet u het maken van resources van SAP SAP voltooien&lt;SID&gt; clustergroep, bijvoorbeeld u nodig hebt om resources te maken:

* **SAP &lt;SID&gt; &lt;InstanceNumber&gt; Service** en
* **SAP &lt;SID&gt; &lt;InstanceNumber&gt; exemplaar**

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

# Create SAP Instance cluster resource
$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPInstanceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Resource" -SeparateMonitor -Verbose

#Set SAP Instance cluster resource parameters
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystemName -Value $SAPSID -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystem -Value $SAPInstanceNumber -Verbose

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPServiceClusterResourceName]" -Verbose
```

## <a name="add-a-probe-port"></a>Een test-poort toevoegen

In deze stap maakt configureert u een SAP-clusterbron SAP-SID-IP-testpoort met behulp van PowerShell. Deze configuratie niet uitvoeren op een van de clusterknooppunten SAP ASC's / SCS zoals beschreven [hier][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].

## <a name="install-ers-instance-on-both-cluster-nodes"></a>Ebruikers-exemplaar installeren op beide clusterknooppunten

U moet in de volgende stap exemplaar Ebruikers (in de wachtrij plaatsen replicatie Server) installeren op beide knooppunten van het (A) SCS-cluster.
De installatieoptie vindt u in het menu SWPM:

&lt;Product&gt; -> &lt;DBMS&gt; -> installatie extra exemplaren -> SAP-systeem -> **replicatie Server-exemplaar in de wachtrij plaatsen**

## <a name="install-dbms-instance-and-sap-application-servers"></a>Installatie DBMS exemplaar en SAP-toepassingsservers

De installatie van het systeem SAP voltooien door te installeren:
* DBMS-exemplaar
* Primaire SAP-toepassingsserver
* Aanvullende SAP-toepassingsserver

## <a name="next-steps"></a>Volgende stappen

* [Installatie van een exemplaar van de SCS (A) op een failovercluster waarop geen gedeelde schijven - officiële SAP-richtlijnen voor de bestandsshare HA][sap-official-ha-file-share-document]:

* [Opslagruimten Direct in WindowsServer 2016][s2d-in-win-2016]

* [Scale-Out File Server for Application Data Overview][sofs-overview]

* [Wat is er nieuw in de opslag in Windows Server 2016][new-in-win-2016-storage]
