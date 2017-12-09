---
title: Een SAP ASC's / SCS-exemplaar op een failovercluster voor Windows-cluster met behulp van een bestandsshare in Azure | Microsoft Docs
description: Informatie over het cluster een SAP ASC's / SCS-exemplaar op een Windows-failovercluster met behulp van een bestandsshare in Azure.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7fa09de63c6f148064861479138934976f1f3e38
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2492395]:https://launchpad.support.sap.com/#/notes/2492395

[kb4025334]:https://support.microsoft.com/help/4025334/windows-10-update-kb4025334

[dv2-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dv2-series
[ds-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#ds-series

[sap-installation-guides]:http://service.sap.com/instguides

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
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-volumes-s2d-choosing-filesystem]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-filesystem
[choosing-the-size-of-volumes-s2d]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-size-of-volumes
[deploy-sofs-s2d-in-azure]:https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment
[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[deep-dive-volumes-in-s2d]:https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

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

[1869038]:https://launchpad.support.sap.com/#/notes/1869038 

# <a name="cluster-an-sap-ascsscs-instance-on-a-windows-failover-cluster-by-using-a-file-share-in-azure"></a>Een SAP ASC's / SCS-exemplaar op een failovercluster voor Windows-cluster met behulp van een bestandsshare in Azure

> ![Windows][Logo_Windows] Windows
>

Windows Server failover clustering vormt de basis van een hoge beschikbaarheid SAP ASC's / SCS installatie en DBMS in Windows.

Een failovercluster is een groep 1 + n onafhankelijke servers (knooppunten) die samenwerken, zodat de beschikbaarheid van toepassingen en services te verbeteren. Als een knooppuntfout optreedt, wordt het aantal fouten die kunnen optreden en onderhouden nog steeds een gezonde cluster om toepassingen en services te bieden in Windows Server failover clustering berekend. U kunt kiezen uit andere quorum-modi voor failover clustering.

## <a name="prerequisites"></a>Vereisten
Voordat u de taken die worden beschreven in dit artikel, controleert u in dit artikel:

* [Architectuur van de hoge beschikbaarheid Azure virtuele Machines en scenario's voor SAP NetWeaver][sap-high-availability-architecture-scenarios]

> [!IMPORTANT]
> SAP ASC's / SCS exemplaren met behulp van een bestandsshare clustering wordt ondersteund voor SAP NetWeaver 7.40 (en hoger), met SAP Kernel 7.49 (en hoger).
>


## <a name="windows-server-failover-clustering-in-azure"></a>Windows Server failover clustering in Azure

Azure Virtual Machines vergeleken met de bare-metal of privécloud implementaties, zijn aanvullende stappen voor het configureren van Windows Server Failoverclustering vereist. Wanneer u een cluster maakt, moet u verschillende IP-adressen en namen van virtuele hosts voor de SAP ASC's / SCS-instantie ingesteld.

### <a name="name-resolution-in-azure-and-the-cluster-virtual-host-name"></a>Naamomzetting in Azure en de naam van het cluster virtuele host

Het Azure-cloud-platform niet de optie voor het configureren van virtuele IP-adressen, zoals zwevend IP-adressen te bieden. U moet een alternatieve oplossing voor het instellen van een virtueel IP-adres te bereiken van de cluster-bron in de cloud. 

De service Azure Load Balancer biedt een *interne load balancer* voor Azure. Met de interne load balancer bereiken clients het cluster via het cluster virtuele IP-adres. 

De interne load balancer in de resourcegroep waarin de clusterknooppunten implementeren. Configureer vervolgens alle benodigde poorttoewijzing regels met behulp van de test poorten van de interne load balancer. De clients verbinding kunnen maken via de naam van de virtuele host. De DNS-server wordt omgezet IP-adres van het cluster. De interne load balancer verwerkt poort doorsturen naar het actieve knooppunt van het cluster.

![Afbeelding 1: Windows Server Failover Clustering configuratie in Azure zonder een gedeelde schijf][sap-ha-guide-figure-1001]

_**Afbeelding 1:** Windows Server failover clustering configuratie in Azure zonder een gedeelde schijf_

## <a name="sap-ascsscs-ha-with-file-share"></a>SAP ASC's / SCS HA met bestandsshare

SAP ontwikkeld voor een nieuwe benadering en een alternatief voor het cluster gedeelde schijven, voor clustering van een SAP ASC's / SCS-exemplaar op een Windows-failovercluster. In plaats van Gedeelde clusterschijven, kunt u een SMB-bestandsshare SAP globale hostbestanden te implementeren.

> [!NOTE]
> Een SMB-bestandsshare is een alternatief voor het gebruik van Gedeelde clusterschijven voor clustering SAP ASC's / SCS exemplaren.  
>

Deze architectuur is specifiek in de volgende manieren:

* Centrale SAP-services (met een eigen processen van de structuur en het bericht en in de wachtrij plaatsen voor bestand) worden gescheiden van de bestanden van de globale host SAP.
* Centrale SAP-services worden uitgevoerd onder een SAP ASC's / SCS-exemplaar.
* SAP ASC's / SCS-exemplaar is geclusterd en toegankelijk is via de \<naam van de virtuele host ASC's / SCS\> naam virtuele host op.
* Globale SAP-bestanden worden geplaatst op de SMB-bestandsshare en toegankelijk zijn via de \<SAP globale host\> hostnaam: \\ \\ &lt;SAP globale host&gt;\sapmnt\\ &lt;SID&gt;\SYS\....
* Het SAP ASC's / SCS-exemplaar is geïnstalleerd op een lokale schijf op beide clusterknooppunten.
* De \<naam van de virtuele host ASC's / SCS\> netwerknaam verschilt van &lt;SAP globale host&gt;.

![Afbeelding 2: SAP ASC's / SCS HA-architectuur met SMB-bestandsshare][sap-ha-guide-figure-8004]

_**Afbeelding 2:** nieuwe SAP ASC's / SCS HA architectuur met een SMB-bestandsshare_

Vereisten voor een SMB-bestandsshare:

* Protocol van SMB 3.0 (of hoger).
* De mogelijkheid toegang tot Active Directory-toegangsbeheerlijsten (ACL's) voor Active Directory-gebruikersgroepen instellen en de `computer$` computerobject.
* De bestandsshare moet HA ingeschakeld:
    * Schijven die worden gebruikt voor het opslaan van bestanden moet niet een potentieel risico.
    * Server of VM uitvaltijd veroorzaakt geen downtime voor de bestandsshare.

De SAP \<SID\> clusterfunctie bevat geen gedeelde clusterschijven of een share clusterbron voor algemeen bestand.


![Afbeelding 3: SAP \<SID\> rol clusterbronnen voor het gebruik van een bestandsshare][sap-ha-guide-figure-8005]

_**Afbeelding 3:** SAP &lt;SID&gt; rol clusterbronnen voor het gebruik van een bestandsshare_


## <a name="scale-out-file-shares-with-storage-spaces-direct-in-azure-as-an-sapmnt-file-share"></a>Scale-out bestandsshares voor opslagruimten Direct in Azure als een bestandsshare SAPMNT

U kunt een scale-out bestandsshare hosten en SAP globale hostbestanden beveiligen. Een scale-out bestandsshare biedt ook een maximaal beschikbare SAPMNT file share-service.

![Afbeelding 4: Scale-out-bestandsshare gebruikt voor het beveiligen van SAP globale host-bestanden][sap-ha-guide-figure-8006]

_**Afbeelding 4:** een scale-out bestandsshare gebruikt voor het beveiligen van SAP globale host-bestanden_

> [!IMPORTANT]
> Scale-out bestandsshares worden volledig ondersteund in de Microsoft Azure-cloud en on-premises omgevingen.
>

Een scale-out bestandsshare biedt een maximaal beschikbare en horizontaal schaalbare SAPMNT-bestandsshare.

Opslagruimten Direct wordt gebruikt als een gedeelde schijf voor een scale-out bestandsshare. U kunt opslagruimten Direct gebruiken om maximaal beschikbare en schaalbare opslag servers gebruiken met lokale opslag samen te stellen. Gedeelde opslag die wordt gebruikt voor een scale-out bestandsshare, zoals voor SAP globale host-bestanden, is niet een potentieel risico.

> [!IMPORTANT]
>Als u *niet* plan voor het instellen van herstel na noodgevallen, wordt u aangeraden een scale-out bestandsshare als een oplossing voor een maximaal beschikbare bestandsshare in Azure.
>

### <a name="sap-prerequisites-for-scale-out-file-shares-in-azure"></a>SAP-vereisten voor scale-out bestandsshares in Azure

Voor het gebruik van een scale-out bestandsshare, moet uw systeem voldoen aan de volgende vereisten:

* Ten minste twee clusterknooppunten voor een scale-out bestandsshare.
* Elk knooppunt moet ten minste twee lokale schijven hebben.
* Reden van prestaties, moet u *spiegelen tolerantie*:
    * Wederzijdse spiegelen voor een scale-out bestandsshare met twee knooppunten.
    * Drie richtingen spiegelen voor een scale-out bestandsshare met drie (of meer) clusterknooppunten.
* Het wordt aangeraden de clusterknooppunten drie (of meer) voor een scale-out bestandsshare met mirroring in drie richtingen.
    Deze instelling biedt meer schaalbaarheid en tolerantie van meer opslagruimte dan de share-instellingen van de scale-out bestandsserver met twee clusterknooppunten en tweewegmirrors.
* U moet Azure Premium-schijven gebruiken.
* Het is raadzaam dat u beheerde Azure-schijven.
* Het is raadzaam dat u de volumes formatteren met behulp van Resilient File System (ReFS).
    * Zie voor meer informatie [SAP-notitie 1869038 - SAP-ondersteuning voor ReFs-bestandssysteem] [ 1869038] en de [kiezen van het bestandssysteem] [ planning-volumes-s2d-choosing-filesystem] hoofdstuk van de artikel Planning volumes in opslagruimten Direct.
    * Zorg ervoor dat u installeert [cumulatieve update voor Microsoft KB4025334][kb4025334].
* U kunt de DS-serie of DSv2-serie Azure VM-grootte.
* Gebruik voor goede netwerkprestaties tussen virtuele machines, die nodig is voor synchronisatie van opslagruimten Direct schijf, een VM-type dat ten minste een 'hoog' netwerkbandbreedte is.
    Zie voor meer informatie de [DSv2-serie] [ dv2-series] en [DS-serie] [ ds-series] specificaties.
* Het is raadzaam dat u een aantal niet-toegewezen capaciteit in de opslaggroep reserveren. Als u een aantal niet-toegewezen capaciteit in de opslaggroep biedt volumes ruimte voor 'in-place' herstellen als een schijf kapot gaat. Dit verbetert de beveiliging van gegevens en prestaties.  Zie voor meer informatie [volumegrootte kiezen][choosing-the-size-of-volumes-s2d].
* Scale-out bestandsshare Azure Virtual machines moet worden geïmplementeerd in hun eigen Azure beschikbaarheidsset.
* U hoeft niet te configureren van de Azure interne load balancer voor de scale-out bestandsserver netwerk sharenaam zoals voor \<SAP globale host\>. Dit geldt voor de \<naam van de virtuele host ASC's / SCS\> van het SAP ASC's / SCS-exemplaar of voor de DBMS. Een scale-out bestandsshare uitgeschaald de belasting door alle clusterknooppunten. \<SAP globale host\> het lokale IP-adres wordt gebruikt voor alle clusterknooppunten.


> [!IMPORTANT]
> U kunt de bestandsshare SAPMNT die naar wijst niet hernoemen \<SAP globale host\>. SAP ondersteunt alleen de share name 'sapmnt."

> Zie voor meer informatie [SAP-notitie 2492395 - kunnen worden gewijzigd met de naam share sapmnt?][2492395]

### <a name="configure-sap-ascsscs-instances-and-a-scale-out-file-share-in-two-clusters"></a>Configureer SAP ASC's / SCS-exemplaren en een scale-out bestandsshare in twee clusters

U kunt implementeren SAP ASC's / SCS-exemplaren in een cluster met hun eigen SAP \<SID\> cluster-rol. In dit geval configureert u de scale-out bestandsshare op een ander cluster, met een ander cluster-rol.

> [!IMPORTANT]
>In dit scenario wordt het SAP ASC's / SCS-exemplaar is geconfigureerd voor toegang tot de globale SAP-host via een UNC-pad \\ \\ &lt;SAP globale host&gt;\sapmnt\\&lt;SID&gt;\SYS\.
>

![Afbeelding 5: SAP ASC's / SCS-exemplaar en een bestandsshare van scale-out in twee clusters geïmplementeerd][sap-ha-guide-figure-8007]

_**Afbeelding 5:** bestandsshare een SAP ASC's / SCS-exemplaar en een scale-out in twee clusters geïmplementeerd_

> [!IMPORTANT]
> In de Azure-cloud elk cluster dat wordt gebruikt voor SAP en scale-out bestandsserver shares moeten worden geïmplementeerd in een eigen Azure beschikbaarheid instellen. Dit zorgt ervoor gedistribueerde plaatsing van het cluster virtuele machines in de onderliggende Azure-infrastructuur.
>

## <a name="generic-file-share-with-sios-datakeeper-as-cluster-shared-disks"></a>Algemene bestandsshare met SIOS DataKeeper zoals cluster gedeelde schijven


> [!IMPORTANT]
> U wordt aangeraden een oplossing voor het delen van scale-out bestandsserver voor een maximaal beschikbare bestandsshare.
>
> Als u herstel na noodgevallen voor de maximaal beschikbare bestandsshare ook worden ingesteld wilt, moet u een algemene bestandsshare en SISO DataKeeper voor uw gedeelde clusterschijven.
>

Een algemene bestandsshare is een andere optie voor het bereiken van een maximaal beschikbare bestandsshare.

In dit geval kunt u een oplossing van derden SIOS als een cluster gedeelde schijf.

## <a name="next-steps"></a>Volgende stappen

* [De Azure-infrastructuur voorbereiden voor SAP HA met behulp van een Windows failover cluster en de bestandsshare voor een SAP ASC's / SCS-exemplaar][sap-high-availability-infrastructure-wsfc-file-share]
* [Installeer SAP NetWeaver HA op een Windows failover cluster en bestandsshare voor een SAP ASC's / SCS-exemplaar][sap-high-availability-installation-wsfc-shared-disk]
* [Een twee knooppunten opslagruimten Direct scale-out bestandsserver implementeert voor UPD opslag in Azure][deploy-sofs-s2d-in-azure]
* [Opslagruimten Direct in WindowsServer 2016][s2d-in-win-2016]
* [Diepgaand: Volumes in opslagruimten Direct][deep-dive-volumes-in-s2d]
