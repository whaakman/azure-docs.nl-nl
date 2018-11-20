---
title: IBM Db2 pureScale op Azure
description: In dit artikel laten we zien een architectuur voor het uitvoeren van een IBM Db2 pureScale-omgeving op Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 5a7ae96a3730df2d0dae04dde6d7609ce69cdee5
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977604"
---
# <a name="ibm-db2-purescale-on-azure"></a>IBM Db2 pureScale op Azure

De IBM Db2 pureScale omgeving biedt een oplossing voor database voor Azure met hoge beschikbaarheid en schaalbaarheid op Linux-besturingssystemen. In dit artikel laten we zien een architectuur voor het uitvoeren van Db2 pureScale op Azure.

## <a name="overview"></a>Overzicht

Ondernemingen hebben traditionele relationele database management systeem (RDBMS) platforms lang gebruikt die geschikt zijn voor online behoeften (OLTP) voor transactieverwerking. Deze dagen zijn veel hun databaseomgevingen op basis van mainframe-migreren naar Azure als een manier om de capaciteit, kosten te verlagen en onderhouden van een structuur onveranderlijk operationele kosten.

Migratie is vaak de eerste stap bij het moderniseren van een verouderde platform. Eén zakelijk rehosted bijvoorbeeld onlangs hun IBM Db2-omgeving die worden uitgevoerd op z/OS IBM Db2 pureScale op Azure. Hoewel niet identiek zijn aan de oorspronkelijke omgeving, biedt IBM Db2 pureScale op Linux vergelijkbare hoge beschikbaarheid en schaalbaarheid functies als IBM Db2 z/OS uitvoeren in een parallelle Sysplex-configuratie op de mainframe.

In dit artikel beschrijft de architectuur die wordt gebruikt voor deze Azure-migratie. Red Hat Linux 7.4 is gebruikt voor het testen van de configuratie. Deze versie is beschikbaar in de Azure Marketplace. Voordat u een Linux-distributie, Controleer de momenteel ondersteunde versies. Zie voor meer informatie de documentatie voor [IBM Db2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) en [GlusterFS](https://docs.gluster.org/en/latest/).

Dit artikel is gewoon een beginpunt voor uw implementatieplan Db2. Uw zakelijke vereisten zullen variëren, maar hetzelfde patroon van toepassing. Deze architectuur patroon kan ook worden gebruikt voor online analytical processing (OLAP)-toepassingen op Azure.

In dit artikel omvat niet verschillen en mogelijke migratietaken voor het verplaatsen van een IBM Db2 voor z/OS-database naar IBM Db2 pureScale die worden uitgevoerd op Linux. Noch schattingen van gelijk formaat en analyses van de werkbelasting voor het verplaatsen van Db2 z/OS naar Db2 pureScale. Om te bepalen van de beste Db2 pureScale architectuur voor uw omgeving, is het raadzaam dat u een volledige sizing voltooien schatting van de oefening en een hypothese tot stand brengen. Bij andere factoren op het bronsysteem Zorg ervoor dat rekening houden met Db2 z/OS parallelle Sysplex met delen van Data-architectuur, configuratie van de koppeling faciliteit en DDF gebruiksstatistieken.

> [!NOTE]
> In dit artikel beschrijft een benadering voor de Db2-migratie, maar er zijn andere gebruikers. Db2 pureScale kunt bijvoorbeeld ook uitvoeren in gevirtualiseerde on-premises omgevingen. IBM Db2 op Microsoft Hyper-V ondersteunt in verschillende configuraties. Zie voor meer informatie, [architectuur van virtualisatie voor Db2 pureScale](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) in het midden van de kennis IBM.

## <a name="architecture"></a>Architectuur

Ter ondersteuning van hoge beschikbaarheid en schaalbaarheid op Azure, een scale-out, kan de architectuur van gedeelde gegevens worden gebruikt voor Db2 pureScale. Het volgende voorbeeld-architectuur is gebruikt voor de migratie van onze klanten.

![](media/db2-purescale-on-azure/pureScaleArchitecture.png "Db2-pureScale op Azure virtual machines weergeven van opslag en netwerken")


Architectuurdiagram ziet u de logische lagen die nodig zijn voor een Db2-pureScale-cluster. Hieronder vallen virtuele machines voor een client, voor beheer, voor in cache opslaan, voor de database-engine en voor gedeelde opslag. Het diagram bevat ook twee knooppunten die worden gebruikt voor het wat staat bekend als het cluster caching faciliteiten (CFs) naast de database-engine-knooppunten. Minimaal twee knooppunten worden gebruikt voor de database-engine zelf. een Db2-server die deel uitmaakt van een cluster pureScale heet een lid. Het cluster is verbonden via iSCSI-een cluster met drie knooppunten GlusterFS gedeelde opslag voor scale-out opslag en hoge beschikbaarheid. Db2 pureScale is geïnstalleerd op virtuele Azure-machines waarop Linux wordt uitgevoerd.

Deze aanpak is gewoon een sjabloon die u aan de behoeften van de grootte kunt wijzigen en de schaal die nodig zijn voor uw organisatie en is gebaseerd op het volgende:

-   Twee of meer leden van de database worden gecombineerd met ten minste twee knooppunten voor CF, die een buffergroep in de globale (Engelse) beheren voor gedeeld geheugen en globale vergrendeling manager (GLM)-services om te bepalen gedeelde toegang en vergrendelen conflicten van meerdere actieve leden. Één CF knooppunt fungeert als de primaire en de andere als de secundaire, CF-failoverknooppunt. Om te voorkomen dat het maken van een omgeving die een single point of failure heeft, zijn minimaal vier knooppunten vereist voor een Db2-pureScale-cluster.

-   Gedeelde opslag met hoge prestaties (weergegeven in P30-grootte in afbeelding 1), die wordt gebruikt door elk van de Gluster FS-knooppunten.

-   Hoogwaardige netwerken voor de gegevensleden en gedeelde opslag.

### <a name="compute-considerations"></a>Overwegingen voor COMPUTE

Deze architectuur voert de toepassing, opslag en lagen op Azure virtual machines. De [implementatie installatiescripts](http://aka.ms/db2onazure) maken het volgende:

-   Een Db2-pureScale-cluster. Het type van de compute-resources die u nodig hebt op Azure, is afhankelijk van uw installatie. Er zijn in het algemeen kunt u op twee manieren:

    -   Gebruik een meerdere knooppunten, high performance computing (HPC)-stijl netwerk waar de gedeelde opslag voor het openen van meerdere kleine en middelgrote exemplaren. Voor deze configuratie HPC type Azure geoptimaliseerd voor geheugen E-serie of opslag geoptimaliseerd L-serie [virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) bieden de rekenkracht power die nodig zijn.

    -   Gebruik minder grote virtuele machine-instanties voor de gegevensengines voor. Voor grote instanties, geoptimaliseerd voor het grootste geheugen [M-serie](https://azure.microsoft.com/pricing/details/virtual-machines/series/) virtuele machines zijn ideaal voor zware in-memory workloads, maar een toegewezen exemplaar is mogelijk vereist zijn, afhankelijk van de grootte van de logische partitie (LPAR) die wordt gebruikt om uit te voeren Db2.

-   De Db2-CF maakt gebruik van virtuele machines geoptimaliseerd voor geheugen, zoals E-serie of L-serie.

-   GlusterFS storage maakt gebruik van Standard\_DS4\_v2 virtuele machines waarop Linux wordt uitgevoerd.

-   Een jumpbox GlusterFS is een standaard\_DS2\_v2 virtuele machine waarop Linux wordt uitgevoerd.

-   De client is een standaard\_DS3\_v2 virtuele machine met Windows (gebruikt voor testdoeleinden).

-   Een witness-server is een standaard\_DS3\_v2 virtuele machine waarop Linux (gebruikt voor Db2 pureScale) wordt uitgevoerd.

> [!NOTE]
> Minimaal twee Db2-exemplaren zijn vereist in een Db2-pureScale-cluster. Een Cache-instantie en Vergrendelingsmanager exemplaar zijn ook vereist.

### <a name="storage-considerations"></a>Opslagoverwegingen

Db2 pureScale is, zoals Oracle RAC, een krachtige blok i/o, scale-out-database. Wordt u aangeraden de grootste [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) beschikbaar die aansluit bij uw behoeften. Bijvoorbeeld, kunnen kleinere opslagopties zijn geschikt voor ontwikkelings- en testomgevingen, terwijl productieomgevingen vaak grotere opslagcapaciteit vereisen. Maakt gebruik van de voorbeeld-architectuur [P30](https://azure.microsoft.com/pricing/details/managed-disks/) vanwege de verhouding tussen de IOPS op grootte en prijs. Ongeacht de grootte, Premium Storage te gebruiken voor de beste prestaties.

Db2 pureScale maakt gebruik van een gedeelde alles architectuur, waarbij alle gegevens zijn toegankelijk is vanaf alle clusterknooppunten. Premium-opslag moet worden verdeeld over meerdere exemplaren, of op aanvraag of op specifieke instanties.

Een grote Db2 pureScale cluster kunt 200 terabytes (TB) is vereist of hoger van Premium gedeelde opslag, met IOPS van 100.000. Db2 pureScale ondersteunt een interface voor het blokkeren van iSCSI-die kan worden gebruikt op Azure. De iSCSI-interface is vereist voor een cluster met gedeelde opslag die kan worden geïmplementeerd met GlusterFS, S2D of een ander hulpprogramma. Dit type oplossing maakt een apparaat virtuele storage area network (SAN) in Azure. Db2 pureScale maakt gebruik van het virtuele SAN voor het installeren van de geclusterde bestandsserver-systeem dat wordt gebruikt voor het delen van gegevens tussen meerdere virtuele machines.

Voor deze architectuur, hebben we het GlusterFS file system, een gratis, schaalbare, open-source gedistribueerd bestandssysteem die speciaal is geoptimaliseerd voor opslag in de cloud gebruikt.

### <a name="networking-considerations"></a>Aandachtspunten voor netwerken

IBM raadt InfiniBand-netwerken aan voor alle leden in een Db2-pureScale-cluster. Db2 pureScale gebruikt ook remote direct memory access (RDMA), indien beschikbaar, voor de CFs gebruikt.

Tijdens de installatie van een Azure [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) wordt gemaakt voor alle virtuele machines bevatten. In het algemeen zijn resources gegroepeerd op basis van hun levensduur en die ze gaan beheren. De virtuele machines in deze architectuur [versnelde netwerken](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/), een functie van Azure met consistente en zeer lage netwerklatentie via één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine.

Elke virtuele machine van Azure wordt geïmplementeerd in een virtueel netwerk dat is onderverdeeld in meerdere subnetten: main, Gluster FS-front-end (gfsfe), Gluster FS back-end (bfsbe), Db2 pureScale (db2be) en Db2 purescale front end (db2fe). Het installatiescript maakt ook de primaire [NIC's](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) op de virtuele machines in de belangrijkste subnet.

[Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (nsg's) worden gebruikt om netwerkverkeer binnen het virtuele netwerk te beperken en om te isoleren van de subnetten.

In Azure moet Db2 pureScale TCP/IP gebruiken als de netwerkverbinding voor de opslag.

## <a name="next-steps"></a>Volgende stappen

-   [Deze architectuur op Azure implementeren](deploy-ibm-db2-purescale-azure.md)
