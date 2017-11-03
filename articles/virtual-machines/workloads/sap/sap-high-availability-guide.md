---
title: Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver | Microsoft Docs
description: Hoge beschikbaarheid-handleiding voor SAP NetWeaver op Azure Virtual Machines
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
ms.openlocfilehash: d00db895ffcf9ba9a51e3df2dae5d33c0277dd6f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver"></a>Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md

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



Virtuele Machines in Azure is de oplossing voor organisaties die berekenings-, opslag- en netwerkbronnen, in de minimale tijd en zonder langdurige inkoop cycli moeten. U kunt Azure Virtual Machines klassieke toepassingen zoals SAP NetWeaver gebaseerde ABAP, Java en een ABAP + Java-stack implementeren. Betrouwbaarheid en beschikbaarheid zonder dat extra on-premises resources uitbreiden. Virtuele Machines in Azure ondersteunt cross-premises connectiviteit, zodat u kunt Azure Virtual Machines integreren in uw organisatie lokale domeinen, privéclouds en SAP system Liggend.

In dit artikel komen we de stappen die u ondernemen kunt om hoge beschikbaarheid SAP-systemen in Azure implementeren met behulp van het Azure Resource Manager-implementatiemodel. We helpt u bij deze belangrijke taken:

* Zoek de juiste SAP-opmerkingen en installatiehandleidingen, die worden vermeld in de [Resources] [ sap-ha-guide-2] sectie. In dit artikel is een aanvulling op de documentatie voor SAP-installatie en SAP-opmerkingen, zijn de primaire resources waarmee u kunnen installeren en SAP software implementeren op specifieke platforms.
* Meer informatie over de verschillen tussen de Azure Resource Manager-implementatiemodel en het klassieke implementatiemodel Azure.
* Meer informatie over Windows Server Failover Clustering quorum-modi, zodat u het model dat geschikt is voor uw Azure-implementatie kunt selecteren.
* Meer informatie over Windows Server Failover Clustering gedeelde opslag in Azure-services.
* Meer informatie over het beveiligen van één punt van fout onderdelen, zoals geavanceerde Business Application Programming (ABAP) SAP centrale Services (ASC's) / SAP centrale Services (SCS) en databasebeheersystemen (DBMS) en redundante componenten zoals SAP-toepassingsserver, in Azure.
* Ga als volgt een voorbeeld van een stapsgewijze van een installatie en configuratie van een SAP-systeem voor hoge beschikbaarheid in een cluster met Windows Server Failover Clustering in Azure met Azure Resource Manager.
* Meer informatie over extra stappen vereist voor het gebruik van Windows Server Failover Clustering in Azure, maar die niet nodig zijn in een on-premises implementatie.

We gebruiken de SAP drie lagen hoge beschikbaarheid Resource Manager-sjablonen om te vereenvoudigen, implementatie en configuratie in dit artikel. De sjablonen voor automatiseren implementatie van de gehele infrastructuur die u nodig hebt voor een hoge beschikbaarheid SAP-systeem. De infrastructuur ondersteunt ook het formaat van uw systeem SAP SAP toepassing prestaties Standard (SAP's).

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Vereisten
Voordat u begint, controleert u dat u voldoet aan de vereisten die worden beschreven in de volgende secties. Bovendien moet u controleren van alle resources in de [Resources] [ sap-ha-guide-2] sectie.

In dit artikel gebruiken we Azure Resource Manager-sjablonen voor [drie lagen SAP NetWeaver schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md/). Zie voor een handig overzicht van sjablonen [SAP Azure Resource Manager-sjablonen](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Resources
Deze artikelen omvatten SAP-implementaties in Azure:

* [Azure virtuele Machines, planning en implementatie voor SAP NetWeaver][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP NetWeaver][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver][dbms-guide]
* [Azure virtuele Machines hoge beschikbaarheid voor SAP NetWeaver (deze handleiding)][sap-ha-guide]

> [!NOTE]
> Waar mogelijk, geven wij u een koppeling naar de verwijzende SAP-installatiehandleiding (Zie de [SAP installatiehandleidingen][sap-installation-guides]). Het is een goed idee om Lees zorgvuldig de SAP NetWeaver installatiehandleidingen voor vereisten en informatie over het installatieproces. Dit artikel behandelt alleen specifieke taken voor SAP NetWeaver-gebaseerde computers die u met Azure Virtual Machines gebruiken kunt.
>
>

Deze opmerkingen SAP zijn gerelateerd aan het onderwerp van SAP in Azure:

| Het nummer | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen in Azure: ondersteunde producten en schaling |
| [2015553] |SAP op Microsoft Azure: ondersteuning voor vereisten |
| [1999351] |Verbeterde Azure bewaking voor SAP |
| [2178632] |Sleutel bewaking van metrische gegevens voor SAP op Microsoft Azure |
| [1999351] |Virtualisatie in Windows: uitgebreide bewaking |
| [2243692] |Gebruik van Azure Premium-SSD-opslag voor SAP DBMS exemplaar |

Meer informatie over de [beperkingen van de Azure-abonnementen][azure-subscription-service-limits-subscription], waaronder algemene standaardbeperkingen en maximale beperkingen.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hoge beschikbaarheid SAP met Azure Resource Manager vergeleken met het klassieke implementatiemodel Azure
De Azure Resource Manager en Azure klassieke implementatiemodellen verschillen in de volgende gebieden:

- Resourcegroepen
- Azure interne load balancer afhankelijkheid van de Azure-resourcegroep
- Ondersteuning voor SAP multi-SID-scenario 's

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Resourcegroepen
In Azure Resource Manager, u kunt resourcegroepen gebruiken voor het beheren van alle toepassingsresources in uw Azure-abonnement. Een geïntegreerde aanpak in een resourcegroep of alle bronnen hebben de dezelfde levenscyclus. Bijvoorbeeld, alle resources worden gemaakt op hetzelfde moment en op hetzelfde moment worden verwijderd. Meer informatie over [resourcegroepen](../../../azure-resource-manager/resource-group-overview.md#resource-groups).

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure interne load balancer afhankelijkheid van de Azure-resourcegroep

Er is een afhankelijkheid tussen de Azure interne load balancer (Azure Load Balancer-service) en de cloudservice in het klassieke implementatiemodel Azure. Elke interne load balancer moet een service in de cloud.

Elke Azure-resource moet worden geplaatst in een Azure-resourcegroep in Azure Resource Manager en dit is ook geldig voor de Load Balancer van Azure. Echter is niet nodig om een Azure-resourcegroep per Azure-load balancer, bijvoorbeeld een Azure-resourcegroep meerdere Azure Load Balancers kan bevatten. De omgeving is eenvoudiger en flexibeler. 

### <a name="support-for-sap-multi-sid-scenarios"></a>Ondersteuning voor SAP multi-SID-scenario 's

In Azure Resource Manager kunt u meerdere SAP systeem-id (SID) ASC's / SCS exemplaren in één cluster installeren. Multi-SID-exemplaren zijn mogelijk vanwege ondersteuning voor meerdere IP-adressen voor elke Azure interne load balancer.

Voer de procedures in het klassieke implementatiemodel Azure met [SAP NetWeaver in Azure: Clustering SAP ASC's / SCS exemplaren met behulp van Windows Server Failover Clustering in Azure met SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> Wij raden u aan het Azure Resource Manager-implementatiemodel te gebruiken voor uw SAP-installaties. Dit biedt veel voordelen die niet beschikbaar in het klassieke implementatiemodel. Meer informatie over Azure [implementatiemodellen][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Serverfailover Clustering
Windows Server Failover Clustering vormt de basis van een hoge beschikbaarheid SAP ASC's / SCS installatie en DBMS in Windows.

Een failovercluster is een groep 1 + n onafhankelijke servers (knooppunten) die samenwerken, zodat de beschikbaarheid van toepassingen en services te verbeteren. Als een knooppuntfout optreedt, berekent Windows Server Failover Clustering het aantal fouten die zich voordoen kunnen tijdens het onderhoud van een gezonde cluster om toepassingen en services te bieden. U kunt kiezen uit andere quorum-modi voor failover clustering.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Quorum-modi
U kunt kiezen uit vier quorum-modi wanneer u Windows Server Failover Clustering:

* **Knooppuntmeerderheid**. Elk knooppunt van het cluster kunt stemmen. Het cluster werkt alleen met een meerderheid van stemmen, dat wil zeggen, met meer dan de helft van de stemmen. U wordt aangeraden deze optie voor clusters met een oneven aantal knooppunten. Bijvoorbeeld drie knooppunten in een cluster met zeven knooppunten kunnen mislukken en het cluster tussen beelden meerderheid bereikt en nog wordt uitgevoerd.  
* **Knooppunt en schijfmeerderheid**. Elk knooppunt en een aangewezen schijf (een schijfwitness) in de clusteropslag kunnen stemmen wanneer ze beschikbaar zijn en in de communicatie. Het cluster werkt alleen met een meerderheid van de stemmen, dat wil zeggen, met meer dan de helft van de stemmen. Deze modus is wel zinvol in een clusteromgeving met een even aantal knooppunten. Als de schijf en de helft van de knooppunten online zijn, blijft het cluster in een foutloze toestand bevindt.
* **Knooppunt en de meeste bestandsshare**. Elk knooppunt plus een aangewezen bestandsshare (een bestandssharewitness) die de beheerder maakt kunt stemmen, ongeacht of de knooppunten en een bestandsshare beschikbaar zijn en bij communicatie. Het cluster werkt alleen met een meerderheid van de stemmen, dat wil zeggen, met meer dan de helft van de stemmen. Deze modus is wel zinvol in een clusteromgeving met een even aantal knooppunten. Het is vergelijkbaar met de modus knooppunt en schijfmeerderheid, maar een witness-bestandsshare wordt gebruikt in plaats van een witness-schijf. Deze modus is eenvoudig te implementeren, maar als de bestandsshare zelf is niet maximaal beschikbaar, het mogelijk een potentieel risico.
* **Geen meerderheid: Alleen schijf**. Het cluster heeft een quorum als een knooppunt beschikbaar is en communicatie met een specifieke schijf in de clusteropslag. Alleen de knooppunten die ook in de communicatie met die schijf kunnen deelnemen aan het cluster. Het is raadzaam dat u deze modus niet gebruiken.
 

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Serverfailover Clustering van on-premises
Afbeelding 1 ziet u een cluster met twee knooppunten. Als de netwerkverbinding tussen de knooppunten mislukt en blijf van knooppunten en wordt uitgevoerd, een quorumschijf of bestand delen bepaalt welk knooppunt blijven bieden de toepassingen en services van het cluster. Het knooppunt dat toegang tot het quorum schijf of de bestandsshare heeft is het knooppunt dat ervoor zorgt dat services kunnen blijven.

Omdat in dit voorbeeld een cluster met twee knooppunten wordt, gebruiken we de knooppunt en bestandssharemeerderheid quorum-modus. Het knooppunt en schijfmeerderheid is ook een geldige optie. U wordt aangeraden dat u een quorumschijf in een productieomgeving. Netwerk- en system-technologie kunt u maximaal beschikbaar wilt maken.

![Afbeelding 1: Voorbeeld van een configuratie voor Windows Server Failover Clustering voor SAP ASC's / SCS in Azure][sap-ha-guide-figure-1000]

_**Afbeelding 1:** voorbeeld van een configuratie voor Windows Server Failover Clustering voor SAP ASC's / SCS in Azure_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Gedeelde opslag
Afbeelding 1 wordt ook een cluster met twee knooppunten gedeelde opslag. Alle knooppunten in het cluster detecteren in een cluster van de gedeelde opslag lokale gedeelde opslag. Een vergrendelingsfout mechanisme beveiligt de gegevens tegen beschadiging. Alle knooppunten kunnen detecteren als een ander knooppunt mislukt. Als een knooppunt uitvalt, wordt het resterende knooppunt eigenaar van de storage-resources en zorgt ervoor dat de beschikbaarheid van services.

> [!NOTE]
> U hoeft geen gedeelde schijven voor hoge beschikbaarheid bij sommige toepassingen DBMS zoals met SQL Server. SQL Server Always On DBMS gegevens en logboekbestanden bestanden van de lokale schijf van één clusterknooppunt worden gerepliceerd naar de lokale schijf van een ander clusterknooppunt. De configuratie van het Windows-cluster, hoeft in dat geval niet een gedeelde schijf.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Netwerk- en naamomzetting
Het cluster bereiken clientcomputers via een virtueel IP-adres en de naam van een virtuele host waarmee de DNS-server. Lokale knooppunten en de DNS-server kunnen meerdere IP-adressen verwerken.

In een typische installatie gebruikt u twee of meer netwerkverbindingen:

* Een speciale verbinding naar de opslag
* Een cluster interne netwerkverbinding voor de heartbeat
* Een openbaar netwerk waarmee clients verbinding maken met het cluster

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Serverfailover Clustering in Azure
Azure Virtual Machines vergeleken met bare metal of privécloud implementaties, zijn aanvullende stappen voor het configureren van Windows Server Failover Clustering vereist. Als u een gedeelde clusterschijf bouwen, moet u verschillende IP-adressen en namen van virtuele hosts voor de SAP ASC's / SCS-instantie ingesteld.

In dit artikel wordt besproken hoofdconcepten en de extra stappen vereist voor het bouwen van een SAP-services met hoge beschikbaarheid centrale cluster in Azure. We zien u hoe u het hulpprogramma van derden SIOS DataKeeper instelt en het configureren van de Azure interne load balancer. U kunt deze hulpprogramma's gebruiken voor het maken van een Windows-failovercluster met een bestandsshare-witness in Azure.

![Afbeelding 2: Windows Server Failover Clustering configuratie in Azure zonder een gedeelde schijf][sap-ha-guide-figure-1001]

_**Afbeelding 2:** configuratie Windows Server Failover Clustering in Azure zonder een gedeelde schijf_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Gedeelde schijf in Azure met SIOS DataKeeper
U moet gedeelde opslag voor een hoge beschikbaarheid SAP ASC's / SCS-exemplaar in de cluster. Vanaf September 2016 biedt geen Azure-aanbieding gedeelde opslag die u gebruiken kunt voor het maken van een cluster met gedeelde opslag. U kunt software van derden SIOS DataKeeper Cluster Edition gebruiken voor het maken van een gespiegelde opslagruimte die gedeelde clusteropslag simuleert. De oplossing SIOS biedt realtime synchrone gegevensreplicatie. Dit is hoe u een gedeelde schijfbron voor een cluster kunt maken:

1. Een extra schijf koppelen aan elk van de virtuele machines (VM's) in de configuratie van een Windows-cluster.
2. SIOS DataKeeper Cluster Edition wordt uitgevoerd op beide knooppunten van de virtuele machine.
3. SIOS DataKeeper Cluster Edition configureren zodat deze overeenkomt met de inhoud van het volume extra schijf die is gekoppeld van de virtuele bronmachine met het extra schijf die is gekoppeld volume van de virtuele doelmachine. SIOS DataKeeper isoleert de bron- en lokale volumes en vervolgens gepresenteerd aan het Windows Server Failover Clustering als één gedeelde schijf.

Vindt u meer informatie over [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![Afbeelding 3: Configuratie van Windows Server Failover Clustering in Azure met SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Afbeelding 3:** configuratie Windows Server Failover Clustering in Azure met SIOS DataKeeper_

> [!NOTE]
> U hoeft geen gedeelde schijven voor hoge beschikbaarheid met sommige DBMS-producten, zoals SQL Server. SQL Server Always On DBMS gegevens en logboekbestanden bestanden van de lokale schijf van één clusterknooppunt worden gerepliceerd naar de lokale schijf van een ander clusterknooppunt. De configuratie van het Windows-cluster, hoeft in dit geval niet een gedeelde schijf.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Naamomzetting in Azure
Het Azure-cloud-platform niet de optie voor het configureren van virtuele IP-adressen, zoals zwevend IP-adressen te bieden. U moet een alternatieve oplossing voor het instellen van een virtueel IP-adres te bereiken van de cluster-bron in de cloud.
Azure heeft een interne load balancer in de service Azure Load Balancer. Met de interne load balancer bereiken clients het cluster via het cluster virtuele IP-adres.
U moet de interne load balancer in de resourcegroep waarin de clusterknooppunten implementeren. Configureer vervolgens alle benodigde poorttoewijzing regels met de test poorten van de interne load balancer.
De clients verbinding kunnen maken via de naam van de virtuele host. De DNS-server worden het IP-adres van het cluster en de interne load balancer-ingangen poort doorsturen naar het actieve knooppunt van het cluster.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver hoge beschikbaarheid in een Azure-infrastructuur-as-a-Service (IaaS)
Om te zorgen voor hoge beschikbaarheid voor de SAP-toepassing, zoals voor SAP-softwareonderdelen, moet u de volgende onderdelen beveiligd:

* SAP Application Server-exemplaar
* SAP ASC's / SCS-exemplaar
* DBMS-server

Zie voor meer informatie over het beveiligen van SAP-onderdelen in scenario's voor hoge beschikbaarheid [Azure Virtual Machines planning en implementatie voor SAP NetWeaver][planning-guide-11].

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Hoge beschikbaarheid SAP-toepassingsserver
Normaal gesproken hoeft u niet een bepaalde oplossing voor hoge beschikbaarheid voor de SAP-toepassingsserver en dialoogvenster exemplaren. Een maximale beschikbaarheid realiseren door redundantie en configureert u meerdere exemplaren van het dialoogvenster in verschillende exemplaren van Azure Virtual Machines. U hebt ten minste twee SAP exemplaren van een toepassing in twee exemplaren van Azure Virtual Machines geïnstalleerd.

![Afbeelding 4: Hoge beschikbaarheid SAP-toepassingsserver][sap-ha-guide-figure-2000]

_**Afbeelding 4:** hoge beschikbaarheid SAP-toepassingsserver_

Alle virtuele machines die host SAP-toepassingsserver exemplaren in de dezelfde Azure beschikbaarheid instellen, moet u plaatsen. Een Azure beschikbaarheidsset zorgt ervoor dat:

* Alle virtuele machines uitmaken deel van hetzelfde domein upgrade. Een upgradedomein bijvoorbeeld ervoor zorgt dat de virtuele machines op hetzelfde moment tijdens gepland onderhoud uitval zijn niet bijgewerkt.
* Alle virtuele machines uitmaken deel van hetzelfde foutdomein. Een foutdomein bijvoorbeeld ervoor zorgt dat virtuele machines zodat er geen storingspunt is van invloed op de beschikbaarheid van alle virtuele machines zijn geïmplementeerd.

Meer informatie over het [de beschikbaarheid van virtuele machines beheren][virtual-machines-manage-availability].

Alleen niet-beheerde schijf: omdat de Azure storage-account een potentieel storingspunt is, het is belangrijk dat u hebt ten minste twee Azure storage-accounts, waarbij ten minste twee virtuele machines worden gedistribueerd. In een ideaal instelling, zou de schijven van elke virtuele machine die wordt uitgevoerd een SAP-dialoogvenster-exemplaar worden geïmplementeerd in een ander opslagaccount.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Hoge beschikbaarheid SAP ASC's / SCS exemplaar
Afbeelding 5 is een voorbeeld van een exemplaar van de SAP ASC's / SCS hoge beschikbaarheid.

![Afbeelding 5: Hoge beschikbaarheid SAP ASC's / SCS exemplaar][sap-ha-guide-figure-2001]

_**Afbeelding 5:** hoge beschikbaarheid SAP ASC's / SCS-exemplaar_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC's / SCS exemplaar hoge beschikbaarheid met Windows Server Failover Clustering in Azure
Azure Virtual Machines vergeleken met bare metal of privécloud implementaties, zijn aanvullende stappen voor het configureren van Windows Server Failover Clustering vereist. Als u wilt maken van een Windows-failovercluster, moet u een gedeelde clusterschijf, verschillende IP-adressen, verschillende namen van de virtuele host en een Azure interne load balancer voor clustering een SAP ASC's / SCS-exemplaar. Dit bespreken we in meer detail later in dit artikel.

![Afbeelding 6: Windows Server Failover Clustering voor een SAP ASC's / SCS-configuratie in Azure met behulp van SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Afbeelding 6:** Windows Server Failover Clustering voor een SAP ASC's / SCS-configuratie in Azure met SIOS DataKeeper_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hoge beschikbaarheid DBMS exemplaar
De DBMS is ook een aanspreekpunt in een SAP-systeem. U moet deze beschermen met behulp van een oplossing voor hoge beschikbaarheid. Afbeelding 7 toont een hoge beschikbaarheid SQL Server Always On oplossing in Azure, met Windows Server Failover Clustering en de Azure interne load balancer. SQL Server Always On repliceert DBMS gegevens en logboekbestanden bestanden met behulp van een eigen DBMS-replicatie. In dit geval nodig u niet clusteren gedeelde schijven die vereenvoudigt de volledige installatie.

![Afbeelding 7: Voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server Always On][sap-ha-guide-figure-2003]

_**Afbeelding 7:** voorbeeld van een hoge beschikbaarheid SAP DBMS, met SQL Server Always On_

Zie voor meer informatie over de clustering van SQL Server in Azure met behulp van het Azure Resource Manager-implementatiemodel, deze artikelen:

* [AlwaysOn-beschikbaarheidsgroep in Azure Virtual Machines handmatig configureren met behulp van Resource Manager] [virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Een Azure interne load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure] [virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>End-to-end hoge beschikbaarheid implementatiescenario 's

### <a name="deployment-scenario-using-architectural-template-1"></a>Implementatiescenario met architectuur sjabloon 1

Afbeelding 8 toont een voorbeeld van een SAP NetWeaver hoge beschikbaarheid-architectuur in Azure voor **één** SAP-systeem. Dit scenario is als volgt instellen:

- Een speciale cluster wordt gebruikt voor het SAP ASC's / SCS-exemplaar.
- Een speciale cluster wordt gebruikt voor de DBMS-exemplaar.
- SAP Application Server-exemplaren zijn in hun eigen toegewezen virtuele machines geïmplementeerd.

![Afbeelding 8: Hoge beschikbaarheid architectuur sjabloon 1, met speciale cluster voor ASC's / SCS en DBMS SAP][sap-ha-guide-figure-2004]

_**Afbeelding 8:** SAP-architectuur sjabloon 1 hoge beschikbaarheid, speciale clusters voor ASC's / SCS en DBMS_

### <a name="deployment-scenario-using-architectural-template-2"></a>Implementatiescenario met behulp van architectuur sjabloon 2

Afbeelding 9 toont een voorbeeld van een SAP NetWeaver hoge beschikbaarheid-architectuur in Azure voor **één** SAP-systeem. Dit scenario is als volgt instellen:

- Een speciale cluster wordt gebruikt voor **beide** het SAP ASC's / SCS-exemplaar en de DBMS.
- SAP Application Server-exemplaren zijn in de eigen toegewezen virtuele machines geïmplementeerd.

![Afbeelding 9: Hoge beschikbaarheid architectuur sjabloon 2, met een speciale voor ASC's / SCS als een cluster toegewezen voor DBMS SAP][sap-ha-guide-figure-2005]

_**Afbeelding 9:** hoge beschikbaarheid architectuur sjabloon 2, met een speciale voor ASC's / SCS als een cluster toegewezen voor DBMS SAP_

### <a name="deployment-scenario-using-architectural-template-3"></a>Implementatiescenario met architectuur sjabloon 3

Afbeelding 10 toont een voorbeeld van een SAP NetWeaver hoge beschikbaarheid-architectuur in Azure voor **twee** SAP-systemen, met &lt;SID1&gt; en &lt;SID2&gt;. Dit scenario is als volgt instellen:

- Een speciale cluster wordt gebruikt voor **beide** het exemplaar SAP ASC's / SCS SID1 *en* het SAP ASC's / SCS SID2-exemplaar (één cluster).
- Een speciale cluster wordt gebruikt voor DBMS SID1 en een ander toegewezen cluster wordt gebruikt voor DBMS SID2 (twee clusters).
- SAP Application Server-exemplaren voor het systeem SAP SID1 hebben hun eigen toegewezen virtuele machines.
- SAP Application Server-exemplaren voor het systeem SAP SID2 hebben hun eigen toegewezen virtuele machines.

![Afbeelding 10: SAP hoge beschikbaarheid architectuur sjabloon 3, met een cluster toegewezen voor verschillende ASC's / SCS-exemplaren][sap-ha-guide-figure-6003]

_**Afbeelding 10:** SAP-hoge beschikbaarheid architectuur sjabloon 3, met een cluster toegewezen voor verschillende ASC's / SCS-exemplaren_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>De infrastructuur voorbereiden

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a>De infrastructuur voorbereiden voor architecturale sjabloon 1
Azure Resource Manager-sjablonen voor SAP vereenvoudigen implementatie van de vereiste resources.

De drie lagen sjablonen in Azure Resource Manager ondersteunen ook scenario's met hoge beschikbaarheid, zoals in de architectuur sjabloon 1, met twee clusters. Elk cluster is een SAP storingspunt voor SAP ASC's / SCS en DBMS.

Dit is waarop u Azure Resource Manager-sjablonen kunt ophalen voor het voorbeeldscenario dat in dit artikel worden beschreven:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Azure Marketplace-installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Aangepaste installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

De infrastructuur voorbereiden voor architecturale sjabloon 1:

- In de Azure-portal op de **Parameters** blade in de **SYSTEMAVAILABILITY** de optie **HA**.

  ![Afbeelding 11: Stel SAP-parameters voor hoge beschikbaarheid Azure Resource Manager][sap-ha-guide-figure-3000]

_**Afbeelding 11:** ingesteld SAP-parameters voor hoge beschikbaarheid Azure Resource Manager_


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

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Virtuele machines implementeren met de zakelijke netwerkverbinding (cross-premises) moet worden gebruikt in productie
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

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Alleen in de cloud SAP-exemplaren voor test- en demo implementeren
U kunt uw SAP-systeem voor hoge beschikbaarheid in een cloudconfiguratie implementatiemodel implementeren. Dit type implementatie is vooral nuttig voor demo en test gebruiksvoorbeelden. Het niet geschikt voor gebruiksvoorbeelden voor productie.

- In de Azure-portal op de **Parameters** blade in de **NEWOREXISTINGSUBNET** de optie **nieuwe**. Laat de **SUBNETID** veld leeg.

  De SAP Azure Resource Manager-sjabloon maakt automatisch virtuele Azure-netwerk en subnet.

> [!NOTE]
> Ook moet u ten minste één toegewezen virtuele machine voor Active Directory en DNS implementeren in hetzelfde exemplaar van Azure Virtual Network. De sjabloon maken niet deze virtuele machines.
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a>De infrastructuur voorbereiden voor architecturale sjabloon 2

U kunt deze Azure Resource Manager-sjabloon voor SAP te vereenvoudigen, de implementatie van resources van de vereiste infrastructuur voor SAP architectuur sjabloon 2.

Dit is waarop u Azure Resource Manager-sjablonen kunt ophalen voor deze implementatiescenario:

* [Azure Marketplace-installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Azure Marketplace-installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Aangepaste installatiekopie](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Aangepaste installatiekopie met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a>De infrastructuur voorbereiden voor architecturale sjabloon 3

U kunt de infrastructuur voorbereiden en configureren van SAP voor **multi-SID**. U kunt bijvoorbeeld een extra exemplaar SAP ASC's / SCS in toevoegen een *bestaande* clusterconfiguratie. Zie voor meer informatie [configureren van een extra exemplaar SAP ASC's / SCS in een bestaande configuratie van het cluster te maken van de configuratie van een SAP-multi-SID in Azure Resource Manager][sap-ha-multi-sid-guide].

Als u maken van een nieuw cluster met meerdere SID wilt, kunt u de multi-SID [Quick Start-sjablonen op GitHub](https://github.com/Azure/azure-quickstart-templates).
Voor het maken van een nieuw cluster met meerdere SID, moet u de volgende drie sjablonen te implementeren:

* [ASC's / SCS-sjabloon](#ASCS-SCS-template)
* [Database-sjabloon](#database-template)
* [Toepassingssjabloon voor servers](#application-servers-template)

De volgende secties hebben meer informatie over de sjablonen en de parameters die u moet opgeven in de sjablonen.

#### <a name="ASCS-SCS-template"></a>ASC's / SCS-sjabloon

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

De volgende lijst bevat alle load-balancingregels (waarbij x staat voor het nummer van het SAP-systeem, bijvoorbeeld 1, 2, 3...):
- Windows-specifieke poorten voor elke SAP-systeem: 445, 5985
- ASC's poorten (exemplaarnummer x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- SCS-poorten (exemplaarnummer x1): 32 x 1 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- ASCS ERS poorten op Linux (exemplaarnummer x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216
- SCS ERS poorten op Linux (exemplaarnummer x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316

De load balancer is geconfigureerd voor gebruik van de volgende test-poorten (waarbij x staat voor het nummer van het SAP-systeem, bijvoorbeeld 1, 2, 3...):
- ASC's / SCS interne load balancer-testpoort: 620 0 x
- Ebruikers interne load balancer-testpoort (alleen voor Linux): 621 x 2

#### <a name="database-template"></a>Database-sjabloon

De databasesjabloon implementeert een of twee virtuele machines die u gebruiken kunt voor het installeren van het relationele databasebeheersysteem (RDBMS) voor een SAP-systeem. Bijvoorbeeld, als u een sjabloon ASC's / SCS voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van het databasesjabloon multi-SID in de [multi-SID databasesjabloon] [ sap-templates-3-tier-multisid-db-marketplace-image] of [multi-SID databasesjabloon beheerd schijven][sap-templates-3-tier-multisid-db-marketplace-image-md], voer waarden in voor de volgende parameters:

  -  **SAP-Id van het systeem**. Voer de SAP-systeem-ID van het SAP-systeem die u wilt installeren. De ID zal worden gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **DbType**. Selecteer het type van de database die u wilt installeren op het cluster. Selecteer **SQL** als u wilt installeren van Microsoft SQL Server. Selecteer **HANA** als u van plan bent een SAP HANA installeren op de virtuele machines. Zorg ervoor dat het juiste besturingssysteem-type selecteren: Selecteer **Windows** voor SQL, en selecteer een Linux-distributiepunt voor HANA. De Load Balancer van Azure die is verbonden met de virtuele machines wordt geconfigureerd ter ondersteuning van het type van de geselecteerde database:
    * **SQL**. De load balancer wordt verdelen poort 1433. Zorg ervoor dat deze poort gebruiken voor uw SQL Server Always On.
    * **HANA**. De load balancer wordt verdelen poorten 35015 en 35017. Zorg dat u SAP HANA met exemplaarnummer **50**.
    De load balancer wordt testpoort 62550 gebruiken.
  -  **Grootte van het SAP**. Het nummer van het nieuwe systeem bieden SAP's ingesteld. Als u niet zeker weet hoeveel SAP's u het systeem moet, vraag uw SAP-technologie Partner of System Integrator.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam en het beheerder beheerderswachtwoord**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**. Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de sjabloon ASC's / SCS of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASC's / SCS.

#### <a name="application-servers-template"></a>Toepassingssjabloon voor servers

De servers toepassingssjabloon implementeert twee of meer virtuele machines die kan worden gebruikt als SAP-toepassingsserver exemplaren voor een SAP-systeem. Bijvoorbeeld, als u een sjabloon ASC's / SCS voor vijf SAP-systemen implementeert, moet u voor het implementeren van deze sjabloon vijf keer.

Voor het instellen van de sjabloon toepassingen servers multi-SID in de [toepassingssjabloon servers multi-SID] [ sap-templates-3-tier-multisid-apps-marketplace-image] of [servers multi-SID toepassingssjabloon beheerd schijven][sap-templates-3-tier-multisid-apps-marketplace-image-md], voer waarden in voor de volgende parameters:

  -  **SAP-Id van het systeem**. Voer de SAP-systeem-ID van het SAP-systeem die u wilt installeren. De ID zal worden gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
  -  **Type besturingssysteem**. Selecteer het besturingssysteem van de virtuele machines.
  -  **Grootte van het SAP**. Het nummer van het nieuwe systeem bieden SAP's. Als u niet zeker weet hoeveel SAP's u het systeem moet, vraag uw SAP-technologie Partner of System Integrator.
  -  **Beschikbaarheid van het systeem**. Selecteer **HA**.
  -  **Gebruikersnaam en het beheerder beheerderswachtwoord**. Maak een nieuwe gebruiker die zich aanmeldt bij de computer kan worden gebruikt.
  -  **Subnet-Id**. Voer de ID van het subnet dat u hebt gebruikt tijdens de implementatie van de sjabloon ASC's / SCS of de ID van het subnet dat is gemaakt als onderdeel van de implementatie van de sjabloon ASC's / SCS.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Virtuele Azure-netwerk
In ons voorbeeld is de adresruimte van het Azure-netwerk 10.0.0.0/16. Er is één subnet met de naam **Subnet**, met een adresbereik van 10.0.0.0/24. Alle virtuele machines en interne load balancers worden geïmplementeerd in dit virtuele netwerk.

> [!IMPORTANT]
> Niet alle wijzigingen aanbrengen in de netwerkinstellingen in het gastbesturingssysteem. Dit omvat het IP-adressen, DNS-servers en subnet. De netwerkinstellingen configureren in Azure. De service Dynamic Host Configuration Protocol (DHCP) geeft uw instellingen.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP-adressen

Om in te stellen de vereiste DNS IP-adressen, moet u de volgende stappen uitvoeren.

1.  In de Azure-portal op de **DNS-servers** blade, zorg ervoor dat het virtuele netwerk **DNS-servers** optie is ingesteld op **aangepaste DNS**.
2.  Selecteer uw instellingen op basis van het type netwerk die u hebben. Zie de volgende bronnen voor meer informatie:
    * [Zakelijke netwerkverbinding (cross-premises)][planning-guide-2.2]: de IP-adressen van de lokale DNS-servers toevoegen.  
    U kunt de lokale DNS-servers aan de virtuele machines die worden uitgevoerd in Azure uitbreiden. In dit scenario, kunt u de IP-adressen van de Azure virtuele machines waarop u de DNS-service uitvoert toevoegen.
    * [Cloud-implementatie][planning-guide-2.1]: een extra virtuele machine implementeren in hetzelfde virtuele netwerk-exemplaar dat als een DNS-server fungeert. Voeg de IP-adressen van de Azure virtuele machines die u hebt ingesteld DNS-service uit te voeren.

    ![Afbeelding 12: DNS-servers configureren voor Azure Virtual Network][sap-ha-guide-figure-3001]

    _**Afbeelding 12:** configureren DNS-servers voor Azure Virtual Network_

  > [!NOTE]
  > Als u de IP-adressen van de DNS-servers wijzigt, moet u de Azure virtuele machines voor het toepassen van de wijziging en het doorgeven van de nieuwe DNS-servers opnieuw opstarten.
  >
  >

In ons voorbeeld is de DNS-service geïnstalleerd en geconfigureerd op deze virtuele machines van Windows:

| De rol virtuele machine | Hostnaam van de virtuele machine | Naam van de netwerk-kaart | Statisch IP-adres |
| --- | --- | --- | --- |
| Eerste DNS-server |domcontr 0 |PR1-nic-domcontr-0 |10.0.0.10 |
| Tweede DNS-server |domcontr-1 |PR1-nic-domcontr-1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Hostnamen en statische IP-adressen voor de geclusterde instantie SAP ASC's / SCS en DBMS geclusterd exemplaar

Voor on-premises implementatie moet u deze gereserveerde hostnamen en IP-adressen:

| Virtuele host naam rol | De naam van de virtuele host | Virtuele vaste IP-adres |
| --- | --- | --- |
| SAP ASC's / SCS eerste virtuele host clusternaam (voor cluster) |PR1-ASC's-vir |10.0.0.42 |
| Naam van de virtuele host op het exemplaar van SAP ASC's / SCS |PR1 ASC's sap |10.0.0.43 |
| SAP DBMS tweede virtuele host clusternaam (cluster management) |PR1-dbms-vir |10.0.0.32 |

Wanneer u het cluster maakt, maakt u de virtuele hostnamen **pr1-ASC's-vir** en **pr1-dbms-vir** en de bijbehorende IP-adressen die het cluster zelf beheren. Zie voor informatie over hoe u dit doet, [verzamelen clusterknooppunten in een clusterconfiguratie][sap-ha-guide-8.12.1].

U kunt handmatig maken met de andere twee virtuele hostnamen, **pr1 ASC's sap** en **pr1 dbms sap**, en de bijbehorende IP-adressen op de DNS-server. Het geclusterde exemplaar van het SAP ASC's / SCS en het geclusterde exemplaar van de DBMS deze resources gebruiken. Zie voor informatie over hoe u dit doet, [maken van een virtuele host-naam voor een geclusterd exemplaar van de SAP ASC's / SCS][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Statische IP-adressen voor de virtuele machines van SAP instellen
Nadat u de virtuele machines te gebruiken in uw cluster implementeert, moet u statische IP-adressen voor alle virtuele machines instellen. Dit doen in de Azure Virtual Network-configuratie en niet in het gastbesturingssysteem.

1.  Selecteer in de Azure-portal **resourcegroep** > **netwerkkaart** > **instellingen** > **IP-adres**.
2.  Op de **IP-adressen** blade onder **toewijzing**, selecteer **statische**. In de **IP-adres** Voer het IP-adres dat u wilt gebruiken.

  > [!NOTE]
  > Als u het IP-adres van de netwerkkaart wijzigt, moet u opnieuw opstarten van de virtuele machines in Azure als de wijziging wilt toepassen.  
  >
  >

  ![Afbeelding 13: Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen][sap-ha-guide-figure-3002]

  _**Afbeelding 13:** statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen_

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

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Instellen van een statisch IP-adres voor de Azure interne load balancer

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

  ![Afbeelding 14: Statische IP-adressen voor de interne load balancer voor de SAP ASC's / SCS-instantie instellen][sap-ha-guide-figure-3003]

  _**Afbeelding 14:** statische IP-adressen voor de interne load balancer voor de SAP ASC's / SCS-instantie instellen_

In ons voorbeeld hebben we twee Azure interne load balancers die deze statische IP-adressen:

| Azure interne load balancer-rol | Naam Azure interne load balancer | Statisch IP-adres |
| --- | --- | --- |
| SAP ASC's / SCS exemplaar interne load balancer |PR1-lb-ASC 's |10.0.0.43 |
| SAP DBMS interne load balancer |PR1-lb-dbms |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Standaard ASC's / SCS taakverdelingsregels voor de Azure interne load balancer

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

_**Tabel 1:** poortnummers van de exemplaren SAP NetWeaver ABAP ASC's_

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

_**Tabel 2:** poortnummers van de SAP NetWeaver Java SCS-exemplaren_

![Afbeelding 15: Standaard ASC's / SCS taakverdelingsregels voor de Azure interne load balancer][sap-ha-guide-figure-3004]

_**Afbeelding 15:** standaard ASC's / SCS load-balancingregels voor de Azure interne load balancer_

Stel het IP-adres van de load balancer **pr1-lb-dbms** het IP-adres van de virtuele host-naam van de DBMS-exemplaar.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>De ASC's / SCS standaard taakverdelingsregels voor de Azure interne load balancer wijzigen

Als u gebruiken verschillende aantallen voor de SAP ASC's of SCS exemplaren wilt, moet u de namen en waarden van de poorten wijzigen van standaardwaarden.

1.  Selecteer in de Azure-portal  **<* SID*> - lb - ASC's load balancer ** > **Load Balancing regels**.
2.  Voor alle load-balancingregels die deel uitmaken van het SAP ASC's of SCS exemplaar, deze waarden te wijzigen:

  * Naam
  * Poort
  * Back-end-poort

  Bijvoorbeeld, als u wijzigen van het standaardaantal ASC's in het exemplaar van 00 tot en met 31 wilt, moet u om de wijzigingen voor alle poorten die worden vermeld in tabel 1.

  Hier volgt een voorbeeld van een update voor poort *lbrule3200*.

  ![Afbeelding 16: De ASC's / SCS standaard taakverdelingsregels voor de Azure interne load balancer wijzigen][sap-ha-guide-figure-3005]

  _**Afbeelding 16:** ASC's / SCS standaard Netwerktaakverdeling regels voor de Azure interne load balancer wijzigen_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuele machines toevoegen aan het domein

Nadat u een statisch IP-adres aan de virtuele machines toewijzen, moet u de virtuele machines toevoegen aan het domein.

![Afbeelding 17: Een virtuele machine toevoegen aan een domein][sap-ha-guide-figure-3006]

_**Afbeelding 17:** een virtuele machine toevoegen aan een domein_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Registervermeldingen op beide knooppunten van het SAP ASC's / SCS-exemplaar toe te voegen

Azure Load Balancer heeft een interne load balancer die wordt gesloten verbindingen als de verbindingen niet actief gedurende een bepaalde zijn time (niet-actieve time-out). SAP processen in het dialoogvenster exemplaren open verbinding naar de wachtrij plaatsen van het SAP verwerken zodra het eerste in de wachtrij plaatsen/wachtrij aanvragen moet worden verzonden. Deze verbindingen meestal blijven tot stand gebrachte totdat het proces of het proces in de wachtrij plaatsen opnieuw wordt opgestart. Als de verbinding actief gedurende een bepaalde tijd is, sluit de Azure interne load balancer de verbindingen. Dit geen probleem omdat het werkproces SAP herstelt u de verbinding met het proces in de wachtrij plaatsen als deze niet meer bestaat. Deze activiteiten worden beschreven in de developer-traces van SAP-processen, maar ze een grote hoeveelheid extra inhoud in deze traceringen maken. Het is een goed idee om te wijzigen van de TCP/IP `KeepAliveTime` en `KeepAliveInterval` op beide clusterknooppunten. Deze wijzigingen in de TCP/IP-parameters met SAP-profiel, beschreven parameters later in dit artikel worden gecombineerd.

Registervermeldingen toegevoegd op beide knooppunten van het SAP ASC's / SCS-exemplaar, Voeg eerst deze registervermeldingen van Windows op beide clusterknooppunten Windows voor SAP ASC's / SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam variabele |`KeepAliveTime` |
| Type variabele |REG_DWORD (decimaal) |
| Waarde |120000 |
| Koppelen aan documentatie |[https://technet.Microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

_**Tabel 3:** wijzigen van de eerste parameter van de TCP/IP_

Voegt u deze Windows-registervermeldingen op beide clusterknooppunten Windows voor SAP ASC's / SCS:

| Pad | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Naam variabele |`KeepAliveInterval` |
| Type variabele |REG_DWORD (decimaal) |
| Waarde |120000 |
| Koppelen aan documentatie |[https://technet.Microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

_**Tabel 4:** wijzigen van de tweede parameter van de TCP/IP_

**De wijzigingen wilt toepassen, start opnieuw op beide clusterknooppunten**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Een cluster met Windows Server Failover Clustering voor een SAP ASC's / SCS-instantie instellen

Instellen van een cluster met Windows Server Failover Clustering voor een SAP ASC's / SCS-exemplaar, moet deze taken uitvoeren:

- Verzamelen van de clusterknooppunten in een clusterconfiguratie
- Een cluster bestandsshare-witness configureren

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Verzamelen van de clusterknooppunten in een clusterconfiguratie

1.  In de functie toevoegen en de Wizard Functies toevoegen Failoverclustering aan beide clusterknooppunten.
2.  Het failover-cluster instellen met behulp van Failoverclusterbeheer. Selecteer in Failoverclusterbeheer **Cluster maken**, en vervolgens alleen de naam van het eerste knooppunt A.-cluster toevoegen Voeg het tweede knooppunt nog; u moet het tweede knooppunt toevoegen in een later stadium.

  ![Afbeelding 18: De server of de virtuele machine-naam van het eerste clusterknooppunt toevoegen][sap-ha-guide-figure-3007]

  _**Afbeelding 18:** de server of de virtuele machine-naam van het eerste clusterknooppunt toevoegen_

3.  Voer de netwerknaam (virtuele host-naam) van het cluster.

  ![Afbeelding 19: Voer de naam van het cluster][sap-ha-guide-figure-3008]

  _**Afbeelding 19:** Voer de naam van het cluster_

4.  Nadat u het cluster hebt gemaakt, voert u een clustervalidatietest.

  ![Afbeelding van 20: De validatiecontrole cluster uitvoeren][sap-ha-guide-figure-3009]

  _**Afbeelding van 20:** de validatiecontrole cluster uitvoeren_

  U kunt waarschuwingen met betrekking tot schijven op dit moment in het proces negeren. U voegt dat een bestandssharewitness en de SIOS gedeelde schijven later. In deze fase hoeft u niet te hoeven maken over het hebben van een quorum.

  ![Afbeelding 21: Er is geen quorumschijf is gevonden.][sap-ha-guide-figure-3010]

  _**Afbeelding 21:** geen quorumschijf is gevonden_

  ![Afbeelding 22: Core cluster-bron moet een nieuw IP-adres][sap-ha-guide-figure-3011]

  _**Afbeelding 22:** Core cluster-bron moet een nieuw IP-adres_

5.  Wijzig het IP-adres van de clusterservice core. Het cluster kan totdat u het IP-adres van de clusterservice core wijzigt niet starten omdat het IP-adres van de server naar een van de knooppunten van de virtuele machine verwijst. Dit doen op de **eigenschappen** pagina van de core-clusterservice IP-resource.

  Bijvoorbeeld, moet een IP-adres toewijzen (in ons voorbeeld **10.0.0.42**) voor de naam van het cluster virtuele host **pr1-ASC's-vir**.

  ![Afbeelding 23: In het dialoogvenster Eigenschappen van de IP-adres wijzigen][sap-ha-guide-figure-3012]

  _**Afbeelding 23:** In de **eigenschappen** dialoogvenster Wijzig het IP-adres_

  ![Afbeelding 24: Wijs het IP-adres dat is gereserveerd voor het cluster][sap-ha-guide-figure-3013]

  _**Afbeelding 24:** toewijzen van het IP-adres dat is gereserveerd voor het cluster_

6.  Virtuele hostnaam van het cluster online brengen.

  ![Afbeelding 25: Core de clusterservice actief is en actief en met het juiste IP-adres][sap-ha-guide-figure-3014]

  _**Afbeelding 25:** core de clusterservice actief is en actief en met het juiste IP-adres_

7.  Het tweede knooppunt toevoegen.

  Nu de core cluster-service actief is, kunt u het tweede knooppunt toevoegen.

  ![Afbeelding 26: Voeg het tweede clusterknooppunt][sap-ha-guide-figure-3015]

  _**Afbeelding 26:** het tweede knooppunt toevoegen_

8.  Voer een naam voor de tweede host van de cluster-knooppunt.

  ![Afbeelding 27: Voer de tweede hostnaam van de cluster-knooppunt][sap-ha-guide-figure-3016]

  _**Afbeelding 27:** invoeren van de tweede hostnaam van de cluster-knooppunt_

  > [!IMPORTANT]
  > Controleer of de **alle in aanmerking komende opslag toevoegen aan het cluster** selectievakje **niet** geselecteerde.  
  >
  >

  ![Afbeelding 28: Schakel het selectievakje niet][sap-ha-guide-figure-3017]

  _**Afbeelding 28:** doen **niet** Schakel het selectievakje_

  U kunt waarschuwingen over quorum en schijven negeren. U stelt het quorum en delen van de schijf later, zoals beschreven in [SIOS DataKeeper Cluster Edition installeren voor de clusterschijf share SAP ASC's / SCS][sap-ha-guide-8.12.3].

  ![Afbeelding 29: Negeren van waarschuwingen over de schijf quorum][sap-ha-guide-figure-3018]

  _**Afbeelding 29:** negeren van waarschuwingen over de schijf quorum_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Een cluster bestandsshare-witness configureren

Een cluster bestandsshare-witness configureren, moet deze taken uitvoeren:

- Een bestandsshare maken
- Instellen van het bestand bestandsshare-witness quorum in Failoverclusterbeheer

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Een bestandsshare maken

1.  Selecteer een bestandssharewitness in plaats van een quorumschijf. SIOS DataKeeper ondersteunt deze optie.

  In de voorbeelden in dit artikel wordt de bestandsshare-witness is op de Active Directory en DNS-server die wordt uitgevoerd in Azure. De bestandsshare-witness wordt aangeroepen **domcontr 0**. Omdat u zou een VPN-verbinding naar Azure (via Site-naar-Site VPN- of Azure ExpressRoute) hebt geconfigureerd, kunt u met uw Active Directory en DNS-service on-premises en is niet geschikt is voor het uitvoeren van een bestand witness delen.

  > [!NOTE]
  > Als uw Active Directory en DNS-service wordt uitgevoerd alleen lokale, niet de bestandsshare-witness configureren op het Active Directory en DNS-Windows-besturingssysteem die lokaal wordt uitgevoerd. Netwerklatentie tussen clusterknooppunten uitgevoerd in Azure en Active Directory en DNS-on-premises mogelijk te groot zijn en leiden tot problemen met de netwerkverbinding. Zorg ervoor dat de bestandsshare-witness configureren op een virtuele machine van Azure waarop dicht bij het clusterknooppunt.  
  >
  >

  De schijf quorum moet ten minste 1024 MB vrije ruimte. We raden 2048 MB aan vrije ruimte voor het quorumstation aan.

2.  De clusternaamobject toevoegen.

  ![Afbeelding 30: De machtigingen op de share voor het clusterobject naam toewijzen][sap-ha-guide-figure-3019]

  _**Afbeelding 30:** de machtigingen op de share voor het clusterobject naam toewijzen_

  Zorg dat de machtigingen voor de autoriteit voor het wijzigen van gegevens in de share voor het clusterobject naam zijn (in ons voorbeeld **pr1-ASC's-vir$**).

3.  Selecteer om de clusternaamobject toe aan de lijst met **toevoegen**. Het filter om computerobjecten, naast die wordt weergegeven in afbeelding 31 te controleren of wijzigen.

  ![Afbeelding 31: De objecttypen die computers bijvoegt wijzigen][sap-ha-guide-figure-3020]

  _**Afbeelding 31:** de objecttypen die computers bijvoegt wijzigen_

  ![Afbeelding 32: Schakel het selectievakje Computers][sap-ha-guide-figure-3021]

  _**Afbeelding 32:** selecteert u de **Computers** selectievakje_

4.  Voer het clusternaamobject zoals in afbeelding 31. Omdat de record al is gemaakt, kunt u de machtigingen kunt wijzigen, zoals wordt weergegeven in afbeelding 30.

5.  Selecteer de **beveiliging** tabblad van de share- en vervolgens moet u meer gedetailleerde machtigingen voor het clusterobject naam.

  ![Afbeelding 33: De kenmerken voor beveiliging voor het clusterobject naam ingesteld op het bestandsshare quorum][sap-ha-guide-figure-3022]

  _**Afbeelding 33:** beveiligingskenmerken van voor de clusternaamobject instellen op het bestandsshare quorum_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Instellen van het bestand bestandsshare-witness quorum in Failoverclusterbeheer

1.  Open de instelling-Wizard clusterquorum configureren.

  ![Afbeelding 34: Start de Wizard configureren clusterquorum-instelling][sap-ha-guide-figure-3023]

  _**Afbeelding 34:** de configureren instelling Wizard clusterquorum starten_

2.  Op de **quorumconfiguratie selecteren** pagina **selecteert u de quorumwitness**.

  ![Afbeelding 35: Quorumconfiguraties u kunt kiezen uit][sap-ha-guide-figure-3024]

  _**Afbeelding 35:** quorumconfiguraties kunt u kiezen uit_

3.  Op de **Quorumwitness selecteren** pagina **een bestandssharewitness configureren**.

  ![Afbeelding 36: Selecteer de bestandsshare-witness][sap-ha-guide-figure-3025]

  _**Afbeelding 36:** selecteert u de bestandsshare-witness_

4.  Geef het UNC-pad naar de bestandsshare (in ons voorbeeld \\domcontr 0\FSW). Een overzicht van de wijzigingen die u kunt maken, selecteer **volgende**.

  ![Afbeelding 37: Definieer de bestandslocatie van de share voor de witness-share][sap-ha-guide-figure-3026]

  _**Afbeelding 37:** definiëren de bestandslocatie van de share voor de witness-share_

5.  Selecteer de gewenste wijzigingen aan, en selecteer vervolgens **volgende**. U moet de configuratie van het cluster is opnieuw te configureren zoals wordt weergegeven in afbeelding 38.  

  ![Afbeelding 38: Bevestiging dat u het cluster opnieuw hebt geconfigureerd][sap-ha-guide-figure-3027]

  _**Afbeelding 38:** bevestiging dat u het cluster opnieuw hebt geconfigureerd_

Nadat het failovercluster van Windows is geïnstalleerd, moeten de wijzigingen worden aangebracht aan sommige drempelwaarden aan te passen failover detectie om voorwaarden in Azure. De parameters worden gewijzigd, worden beschreven in deze blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/. Ervan uitgaande dat uw twee virtuele machines die de configuratie van het Windows-Cluster voor ASC's / SCS bouwen zich in hetzelfde SubNet bevinden, moeten de volgende parameters worden gewijzigd in deze waarden:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Deze instellingen zijn getest met klanten en een goede inbreuk robuuste de een-zijde worden opgegeven. Aan de andere kant zijn die instellingen bieden snel genoeg failover in een echte foutcondities op SAP-software of het knooppunt/VM-fout. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>SIOS DataKeeper Cluster Edition voor de SAP ASC's / SCS share clusterschijf installeren

U hebt nu een werkende configuratie van Windows Server Failover Clustering in Azure. Maar als u wilt installeren een SAP ASC's / SCS-exemplaar, moet u een gedeelde schijfresource. U kunt de gedeelde schijfresources, u moet in Azure maken. SIOS DataKeeper Cluster Edition is een oplossing van derden die u gebruiken kunt om resources van de gedeelde schijf te maken.

SIOS DataKeeper Cluster Edition voor de SAP ASC's / SCS share clusterschijf installeren, moet deze taken uitvoeren:

- Toevoegen van het .NET Framework 3.5
- SIOS DataKeeper installeren
- SIOS DataKeeper instellen

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Het .NET Framework 3.5 toevoegen
Microsoft .NET Framework 3.5 is niet automatisch geactiveerd of Windows Server 2012 R2 is geïnstalleerd. Omdat SIOS DataKeeper .NET Framework vereist op alle knooppunten die u op DataKeeper installeert, moet u .NET Framework 3.5 installeren op het gastbesturingssysteem van alle virtuele machines in het cluster.

Er zijn twee manieren om toe te voegen .NET Framework 3.5:

- Gebruik de toevoegen Wizard functies en onderdelen in Windows zoals in afbeelding 39.

  ![Afbeelding 39: .NET Framework 3.5 installeren met behulp van de functies en onderdelen][sap-ha-guide-figure-3028]

  _**Afbeelding 39:** .NET Framework 3.5 installeren met behulp van de functies en onderdelen_

  ![Afbeelding 40: Installatie van de voortgangsbalk wanneer u de .NET Framework 3.5 installeren met behulp van de functies en onderdelen][sap-ha-guide-figure-3029]

  _**Afbeelding 40:** installatie van de voortgangsbalk wanneer u de .NET Framework 3.5 installeren met behulp van de functies en onderdelen_

- Gebruik het opdrachtregelprogramma dism.exe. Voor dit type installatie moet u toegang tot de SxS-map op de installatiemedia van Windows. Typ bij een opdrachtprompt met verhoogde bevoegdheid:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper installeren

Installeer SIOS DataKeeper Cluster Edition op elk knooppunt in het cluster. Als u wilt maken van virtuele gedeelde opslag met SIOS DataKeeper, maakt u een gesynchroniseerde mirror en vervolgens simuleren cluster gedeelde opslag.

Voordat u de software SIOS installeert, maakt u de domeingebruiker **DataKeeperSvc**.

> [!NOTE]
> Voeg de **DataKeeperSvc** gebruiker de **lokale beheerder** groep op beide clusterknooppunten.
>
>

SIOS DataKeeper installeren:

1.  De SIOS software installeren op beide clusterknooppunten.

  ![SIOS installatieprogramma][sap-ha-guide-figure-3030]

  ![41 afbeelding: Eerste pagina van de installatie SIOS DataKeeper][sap-ha-guide-figure-3031]

  _**Afbeelding 41:** eerste pagina van de installatie SIOS DataKeeper_

2.  Selecteer in het dialoogvenster wordt weergegeven in afbeelding 42 **Ja**.

  ![Afbeelding 42: DataKeeper informeert u dat een service wordt uitgeschakeld][sap-ha-guide-figure-3032]

  _**Afbeelding 42:** DataKeeper informeert u dat een service wordt uitgeschakeld_

3.  In het dialoogvenster wordt weergegeven in afbeelding 43, wordt aangeraden dat u selecteert **domein of de Server account**.

  ![Afbeelding 43: Selectie van de de gebruiker voor SIOS DataKeeper][sap-ha-guide-figure-3033]

  _**Afbeelding 43:** gebruikersselectie voor SIOS DataKeeper_

4.  Geef de accountnaam en domein en de wachtwoorden die u hebt gemaakt voor SIOS DataKeeper.

  ![Afbeelding 44: Voer de gebruikersnaam en het wachtwoord voor de installatie SIOS DataKeeper][sap-ha-guide-figure-3034]

  _**Afbeelding 44:** domeingebruikersnaam en wachtwoord invoeren voor de installatie SIOS DataKeeper_

5.  De licentiesleutel voor uw exemplaar SIOS DataKeeper installeren zoals wordt weergegeven in afbeelding 45.

  ![Afbeelding 45: Voer uw licentiecode SIOS DataKeeper][sap-ha-guide-figure-3035]

  _**Afbeelding 45:** uw licentiesleutel SIOS DataKeeper invoeren_

6.  Wanneer u wordt gevraagd, start u de virtuele machine opnieuw.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper instellen

Nadat u SIOS DataKeeper op beide knooppunten hebt geïnstalleerd, moet u de configuratie te starten. Het doel van de configuratie is dat synchrone replicatie tussen de extra schijven die zijn gekoppeld aan elk van de virtuele machines.

1.  Start het hulpprogramma DataKeeper beheer en de configuratie en selecteer vervolgens **verbinding maken met Server**. (In de afbeelding 46 deze optie is met een rode cirkel.)

  ![46 afbeelding: De SIOS DataKeeper beheer en het hulpprogramma voor serverconfiguratie][sap-ha-guide-figure-3036]

  _**Afbeelding 46:** SIOS DataKeeper beheer en de configuratie-hulpprogramma_

2.  Voer de naam of TCP/IP-adres van het eerste knooppunt dat het hulpprogramma voor beheer en de configuratie verbinding, en, in een tweede stap van het tweede knooppunt maken moet.

  ![Afbeelding 47: Voegt u de naam of TCP/IP-adres van het eerste knooppunt van het beheer en hulpprogramma voor serverconfiguratie verbinding moeten maken, en in een tweede stap van het tweede knooppunt][sap-ha-guide-figure-3037]

  _**Afbeelding 47:** invoegen van de naam of TCP/IP-adres van het eerste knooppunt het hulpprogramma voor beheer en de configuratie verbinding, en in een tweede stap van het tweede knooppunt maken moet_

3.  De taak van de replicatie tussen de twee knooppunten maken.

  ![Afbeelding van 48: Een replicatie-taak maken][sap-ha-guide-figure-3038]

  _**Afbeelding van 48:** een replicatie-taak maken_

  Een wizard leidt u door het proces van het maken van een taak voor replicatie.
4.  Definieer de naam, de TCP/IP-adres en het schijfvolume van het bronknooppunt.

  ![Afbeelding 49: Definieer de naam van de replicatie-taak][sap-ha-guide-figure-3039]

  _**Afbeelding 49:** definiëren van de naam van de replicatie-taak_

  ![Afbeelding 50: Definieer de basisgegevens voor het knooppunt, het huidige bronknooppunt moet][sap-ha-guide-figure-3040]

  _**Afbeelding 50:** de basisgegevens voor het knooppunt, het huidige bronknooppunt moet definiëren_

5.  Definieer de naam, de TCP/IP-adres en het schijfvolume van het doelknooppunt.

  ![Afbeelding 51: Definieer de basisgegevens voor het knooppunt, het huidige doelknooppunt moet][sap-ha-guide-figure-3041]

  _**Afbeelding 51:** de basisgegevens voor het knooppunt, het huidige doelknooppunt moet definiëren_

6.  Definieer de compressie-algoritmen. In ons voorbeeld is het raadzaam dat u de replicatiestroom comprimeren. Met name in situaties hersynchronisatie vermindert de compressie van de replicatiestroom aanzienlijk hersynchronisatie tijd. Houd er rekening mee dat de bronnen van CPU en RAM-geheugen van een virtuele machine maakt gebruik van compressie. Als de snelheid met compressie toeneemt, loopt u het volume van de CPU-resources die worden gebruikt. Ook kunt u aanpassen deze instelling later.

7.  Een andere instelling die u nodig hebt om te controleren is of de replicatie synchroon of asynchroon plaatsvindt. *Wanneer u SAP ASC's / SCS configuraties beveiligt, moet u synchrone replicatie*.  

  ![Afbeelding 52: Replicatiedetails definiëren][sap-ha-guide-figure-3042]

  _**Afbeelding 52:** replicatiedetails definiëren_

8.  Bepalen of het volume dat is gerepliceerd door de taak moet worden weergegeven in een clusterconfiguratie Windows Server Failover Clustering als gedeelde schijf. Selecteer voor de configuratie SAP ASC's / SCS **Ja** zodat het cluster van Windows ziet het volume gerepliceerde als gedeelde schijf die kan worden gebruikt als een clustervolume.

  ![Afbeelding 53: Selecteer Ja als u wilt het gerepliceerde volume als een clustervolume instellen][sap-ha-guide-figure-3043]

  _**Afbeelding 53:** Selecteer **Ja** in te stellen van het volume gerepliceerde als een clustervolume_

  Nadat het volume is gemaakt, ziet u het hulpprogramma DataKeeper beheer en de configuratie of de taak nog actief.

  ![Afbeelding 54: Synchroon spiegelen van DataKeeper voor de schijf van de share SAP ASC's / SCS actief is.][sap-ha-guide-figure-3044]

  _**Afbeelding 54:** DataKeeper synchroon spiegelen voor de SAP ASC's / SCS schijf delen is actief_

  De schijf als een schijf DataKeeper weergegeven Failoverclusterbeheer nu zoals in afbeelding 55.

  ![Afbeelding 55: Failover Cluster Manager ziet u de schijf die DataKeeper gerepliceerd][sap-ha-guide-figure-3045]

  _**Afbeelding 55:** Failoverclusterbeheer ziet u de schijf die is gerepliceerd DataKeeper_

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Het SAP NetWeaver systeem installeert

De installatie van de DBMS won't worden beschreven omdat instellingen variëren, afhankelijk van de DBMS-systeem die u gebruikt. Echter, gaan we ervan uit dat hoge beschikbaarheid weergegeven met de DBMS worden aangepakt met de functies van die de verschillende DBMS leveranciers ondersteuning voor Azure. Bijvoorbeeld altijd op of database mirroring voor SQL Server en Oracle Data Guard voor Oracle-databases. In het scenario dat in dit artikel we gebruiken toevoegen niet we meer beveiliging aan de DBMS.

Er zijn geen speciale overwegingen bij verschillende DBMS services met dit soort clusterconfiguratie SAP ASC's / SCS in Azure communiceren.

> [!NOTE]
> De installatieprocedures van SAP NetWeaver ABAP systemen, Java-systemen en ABAP + Java systemen zijn bijna identiek. Het belangrijkste verschil is dat een systeem SAP ABAP één exemplaar van ASC's heeft. Het systeem SAP Java heeft één SCS-exemplaar. Het systeem SAP ABAP + Java heeft één exemplaar van ASC's en één SCS-exemplaar in de groep dezelfde Microsoft failover-cluster. Eventuele verschillen installatie voor elke installatie SAP NetWeaver stack worden expliciet vermeld. Alle andere onderdelen zijn hetzelfde, kunt u aannemen.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>SAP installeren met een hoge beschikbaarheid ASC's / SCS-exemplaar

> [!IMPORTANT]
> Zorg ervoor dat u niet uw wisselbestand op DataKeeper gespiegelde volumes plaatsen. DataKeeper biedt geen ondersteuning voor gespiegelde volumes. U kunt uw wisselbestand op het tijdelijke station D van een virtuele machine van Azure, laten de standaardinstelling. Als deze nog niet is gebeurd, verplaatst u het wisselbestand van Windows naar station D: van uw Azure-machine.
>
>

SAP installeren met een exemplaar van de ASC's / SCS hoge beschikbaarheid, moet deze taken uitvoeren:

- De naam van een virtuele host op voor het geclusterde exemplaar van de SAP ASC's / SCS maken
- Het eerste SAP-clusterknooppunt installeren
- Wijzigen van het SAP-profiel van het exemplaar ASC's / SCS
- Een testpoort toevoegen
- De Windows firewall-testpoort openen

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>De naam van een virtuele host op voor het geclusterde exemplaar van de SAP ASC's / SCS maken

1.  Maak een DNS-vermelding voor de virtuele host-naam van het exemplaar ASC's / SCS in de Windows-DNS-beheer.

  > [!IMPORTANT]
  > Het IP-adres dat u aan de virtuele host-naam van het exemplaar ASC's / SCS toewijst moet hetzelfde zijn als het IP-adres dat u hebt toegewezen aan de Load Balancer van Azure (**<*SID*> - lb - ASC's **).  
  >
  >

  Het IP-adres van de virtuele SAP ASC's / SCS-hostnaam (**pr1 ASC's sap**) is hetzelfde als het IP-adres van de Load Balancer van Azure (**pr1-lb-ASC's**).

  ![Afbeelding 56: Definieer de DNS-vermelding voor de virtuele clusternaam SAP ASC's / SCS en TCP/IP-adres][sap-ha-guide-figure-3046]

  _**Afbeelding 56:** definiëren van de DNS-vermelding voor de virtuele clusternaam SAP ASC's / SCS en TCP/IP-adres_

2.  Het IP-adres dat is toegewezen aan de naam van de virtuele host definiëren selecteren **DNS-beheer** > **domein**.

  ![57 afbeelding: De nieuwe virtuele naam en het TCP/IP-adres voor de clusterconfiguratie SAP ASC's / SCS][sap-ha-guide-figure-3047]

  _**Afbeelding 57:** nieuwe virtuele naam en het TCP/IP-adres voor SAP ASC's / SCS-clusterconfiguratie_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Het eerste SAP-clusterknooppunt installeren

1.  Uitvoeren van de eerste cluster knooppunt optie op clusterknooppunt A. Bijvoorbeeld: op de **pr1-ASC's-0** host.
2.  Als u wilt behouden de standaard poorten voor de Azure interne load balancer, selecteren:

  * **ABAP system**: **ASC's** exemplaar nummer **00**
  * **Java-systeem**: **SCS** exemplaar nummer **01**
  * **ABAP + Java system**: **ASC's** exemplaar nummer **00** en **SCS** exemplaar nummer **01**

  Als u exemplaar getallen dan 00 in voor het exemplaar ABAP ASCS en 01 voor de Java-SCS-exemplaar, moet u eerst wijzigen van de Azure interne load balancer standaard load-balancingregels beschreven in [ASC's / SCS standaard Netwerktaakverdeling regels voor de Azure interne load balancer wijzigen][sap-ha-guide-8.9].

De volgende enkele taken worden niet beschreven in de documentatie van de standaard SAP-installatie.

> [!NOTE]
> De installatie SAP documentatie wordt beschreven hoe het eerste ASC's / SCS-clusterknooppunt installeren.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Wijzig het SAP-profiel van het exemplaar ASC's / SCS

U moet een nieuwe profiel-parameter toevoegen. De profiel-parameter voorkomt dat de verbindingen tussen processen SAP en de server in de wachtrij plaatsen af te sluiten wanneer ze niet actief is te lang zijn. We het probleem scenario in genoemde [registervermeldingen toe te voegen op beide knooppunten van het exemplaar SAP ASC's / SCS][sap-ha-guide-8.11]. In deze sectie geïntroduceerd we ook twee wijzigingen aan enkele eenvoudige TCP/IP-verbindingsparameters. In een tweede stap moet u de server in de wachtrij plaatsen verzenden instellen een `keep_alive` signaal zodat de verbindingen van de Azure interne load balancer niet-actieve drempelwaarde niet bereikt.

Het SAP-profiel van het exemplaar ASC's / SCS wijzigen:

1.  Deze parameter profiel toevoegen aan het profiel SAP ASC's / SCS-exemplaar:

  ```
  enque/encni/set_so_keepalive = true
  ```
  In ons voorbeeld is het pad:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Bijvoorbeeld: voor het profiel voor SAP SCS-exemplaar en het bijbehorende pad:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  De wijzigingen wilt toepassen, het SAP ASC's /SCS-exemplaar opnieuw te starten.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Een testpoort toevoegen

De interne load balancer-test functionaliteit gebruiken om de volledige clusterconfiguratie werken met Azure Load Balancer. De Azure interne load balancer distribueert meestal de binnenkomende werkbelasting gelijkmatig tussen deelnemende virtuele machines. Echter werkt dat niet in bepaalde clusterconfiguraties omdat er slechts één exemplaar actief is. Het andere exemplaar is passief en een van de werkbelasting kan accepteren. Een test-functionaliteit helpt bij de Azure interne load balancer werk alleen aan een actief exemplaar wijst. Met de functionaliteit van de test worden de interne load balancer kunt detecteren welke exemplaren actief zijn en vervolgens de doelinstantie van alleen de met de werkbelasting.

Een testpoort toevoegen:

1.  Controleer de huidige **ProbePort** instellen met de volgende PowerShell-opdracht. Deze niet binnen één van de virtuele machines worden uitgevoerd in de configuratie van het cluster.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  Definieer een testpoort. Het standaardpoortnummer voor de test is **0**. In ons voorbeeld gebruiken we de testpoort **62000**.

  ![Afbeelding 58: De cluster configuration testpoort is 0 standaard][sap-ha-guide-figure-3048]

  _**Afbeelding 58:** de standaardpoort cluster configuration test is 0_

  Het poortnummer dat is gedefinieerd in SAP Azure Resource Manager-sjablonen. U kunt het poortnummer in PowerShell kunt toewijzen.

  Een nieuwe ProbePort waarde instellen voor de  **SAP <*SID*> IP-** clusterbron, voer het volgende PowerShell-script. Bijwerken van de PowerShell-variabelen voor uw omgeving. Nadat het script wordt uitgevoerd, wordt u gevraagd de clustergroep SAP voor het activeren van de wijzigingen opnieuw opstarten.

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

  Nadat u brengt de  **SAP <*SID*> ** cluster online rol, Controleer **ProbePort** is ingesteld op de nieuwe waarde.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![Afbeelding 59: Test de cluster-poort nadat u de nieuwe waarde instellen][sap-ha-guide-figure-3049]

  _**Afbeelding 59:** test van de cluster-poort nadat u de nieuwe waarde instellen_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Open de poort van Windows firewall-test

U moet een Windows firewall-test poort op beide clusterknooppunten te openen. Gebruik het volgende script in een Windows firewall-testpoort openen. Bijwerken van de PowerShell-variabelen voor uw omgeving.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

De **ProbePort** is ingesteld op **62000**. Nu u toegang hebt tot de bestandsshare  **\\\ascsha-clsap\sapmnt** met andere hosts, zoals als van **ascsha-DBA's**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Het database-exemplaar installeren

Volg de procedure beschreven in de documentatie van de installatie SAP voor het installeren van de database-instantie.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Het tweede clusterknooppunt installeren

Volg de stappen in de SAP-installatiehandleiding voor het installeren van het tweede cluster.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Wijzig het starttype van het SAP Ebruikers Windows service-exemplaar

Het starttype van de SAP Ebruikers Windows-service naar **automatisch (vertraagd starten)** op beide clusterknooppunten.

![Afbeelding 60: Wijzig het type van de service voor het exemplaar SAP Ebruikers in vertraagd automatisch][sap-ha-guide-figure-3050]

_**Afbeelding 60:** type van de service voor het exemplaar SAP Ebruikers voor vertraagd automatisch wijzigen_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>De primaire SAP-toepassingsserver installeren

Het exemplaar van de primaire toepassing Server (PAS) installeren <*SID*> - di - 0 op de virtuele machine die u hebt opgegeven voor het hosten van de Pa's. Er zijn geen afhankelijkheden op Azure of DataKeeper-specifieke instellingen.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>De aanvullende SAP-toepassingsserver installeren

Een SAP aanvullende Application Server (AAS) installeren op alle virtuele machines die u hebt opgegeven voor het hosten van een SAP Application Server-exemplaar. Bijvoorbeeld: op <*SID*> - di - 1 naar <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> Dit is de installatie van een hoge beschikbaarheid SAP NetWeaver systeem voltooid. Vervolgens gaat u verder met het testen van failover.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testen van de SAP ASC's / SCS exemplaar failover en SIOS replicatie
Het is gemakkelijk om te testen en een SAP ASC's / SCS exemplaar failover en SIOS schijfreplicatie bewaken met behulp van Failoverclusterbeheer en de SIOS DataKeeper beheer en de configuratie-hulpprogramma.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASC's / SCS-exemplaar wordt uitgevoerd op een clusterknooppunt A

De **SAP PR1** clustergroep wordt uitgevoerd op een clusterknooppunt A. Bijvoorbeeld: op **pr1-ASC's-0**. Toewijzen van de gedeelde schijf S, die deel uitmaakt van de **SAP PR1** clustergroep en die gebruikmaakt van het exemplaar ASC's / SCS, aan het cluster knooppunt A.

![Afbeelding 61: Failover Cluster Manager: de SAP < SID > clustergroep wordt uitgevoerd op een clusterknooppunt A][sap-ha-guide-figure-5000]

_**Afbeelding 61:** Failoverclusterbeheer: de SAP <*SID*> clustergroep wordt uitgevoerd op een clusterknooppunt A_

In het hulpprogramma SIOS DataKeeper beheer en de configuratie kunt u zien dat de gedeelde schijfgegevens worden synchroon gerepliceerd van bron volume station S op clusterknooppunt A naar het volume doelstation S op clusterknooppunt B. Bijvoorbeeld, worden gerepliceerd van **pr1 ASC's 0 [10.0.0.40]** naar **pr1-ASC's-1 [10.0.0.41]**.

![Afbeelding 62: In SIOS DataKeeper, repliceert de lokaal volume van het clusterknooppunt een clusterknooppunt B][sap-ha-guide-figure-5001]

_**Afbeelding 62:** repliceren In SIOS DataKeeper, het lokale volume van het clusterknooppunt een clusterknooppunt B_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Failover van knooppunt A naar B-knooppunt

1.  Kies een van deze opties voor het initiëren van een failover van de SAP <*SID*> clustergroep van het clusterknooppunt een clusterknooppunt B:
  - Gebruik Failoverclusterbeheer  
  - Failover-Cluster PowerShell gebruiken

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Start opnieuw op een van de cluster knooppunt in de Windows-gastbesturingssysteem (Hiermee initieert u een automatische failover van de SAP <*SID*> clustergroep van knooppunt A naar B knooppunt).  
3.  Opnieuw opstarten cluster knooppunt een van de Azure-portal (Hiermee initieert u een automatische failover van de SAP <*SID*> clustergroep van knooppunt A naar B knooppunt).  
4.  Cluster-knooppunt een starten met Azure PowerShell (Hiermee initieert u een automatische failover van de SAP <*SID*> clustergroep van knooppunt A naar B knooppunt).

  Na een failover de SAP <*SID*> clustergroep wordt uitgevoerd op een clusterknooppunt B. Bijvoorbeeld, deze wordt uitgevoerd op **pr1-ASC's-1**.

  ![Afbeelding 63: In Failoverclusterbeheer de clustergroep SAP < SID > wordt uitgevoerd op het clusterknooppunt B][sap-ha-guide-figure-5002]

  _**Afbeelding 63**: In Failoverclusterbeheer, de SAP <*SID*> clustergroep wordt uitgevoerd op een clusterknooppunt B_

  De gedeelde schijf is nu gekoppeld op cluster knooppunt B. SIOS DataKeeper van bron volume station S op clusterknooppunt B gegevens wordt gerepliceerd naar doel volume station S op clusterknooppunt A. Bijvoorbeeld, van repliceren **pr1-ASC's-1 [10.0.0.41]** naar **pr1 ASC's 0 [10.0.0.40]**.

  ![Afbeelding 64: SIOS DataKeeper repliceert lokaal volume van het clusterknooppunt B aan het cluster een knooppunt][sap-ha-guide-figure-5003]

  _**Afbeelding 64:** SIOS DataKeeper repliceert lokaal volume van het clusterknooppunt B aan het cluster een knooppunt_
