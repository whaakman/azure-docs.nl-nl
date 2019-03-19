---
title: IBM DB2 pureScale op Azure
description: In dit artikel laten we zien een architectuur voor het uitvoeren van een IBM DB2 pureScale-omgeving op Azure.
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
ms.openlocfilehash: 1622de0cccdbc8fee0681e209e756b30da292d3c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58003505"
---
# <a name="ibm-db2-purescale-on-azure"></a>IBM DB2 pureScale op Azure

De IBM DB2 pureScale omgeving bevat een databasecluster voor Azure met hoge beschikbaarheid en schaalbaarheid op Linux-besturingssystemen. In dit artikel ziet u een architectuur voor het uitvoeren van DB2 pureScale op Azure.

## <a name="overview"></a>Overzicht

Ondernemingen hebben lang relationele database management systeem (RDBMS) platforms gebruikt voor hun online behoeften (OLTP) voor transactieverwerking. Deze dagen zijn veel hun databaseomgevingen op basis van mainframe-migreren naar Azure als een manier om de capaciteit, kosten te verlagen en onderhouden van een structuur onveranderlijk operationele kosten.

Migratie is vaak de eerste stap bij het moderniseren van een oudere platform. Bijvoorbeeld rehosted één grote zakelijke klant onlangs de IBM DB2-omgeving die worden uitgevoerd op z/OS IBM DB2 pureScale op Azure. Hoewel niet identiek zijn aan de oorspronkelijke omgeving, biedt IBM DB2 pureScale op Linux een vergelijkbare hoge beschikbaarheid en schaalbaarheid, functies als IBM DB2 voor z/OS uitvoeren in een parallelle Sysplex-configuratie op de mainframe.

In dit artikel beschrijft de architectuur die wordt gebruikt voor deze Azure-migratie. De klant gebruikt Red Hat Linux 7.4 voor het testen van de configuratie. Deze versie is beschikbaar in de Azure Marketplace. Voordat u een Linux-distributie kiest, moet u controleren of de momenteel ondersteunde versies. Zie voor meer informatie de documentatie voor [IBM DB2 pureScale](https://www.ibm.com/support/knowledgecenter/SSEPGG) en [GlusterFS](https://docs.gluster.org/en/latest/).

Dit artikel is een beginpunt voor uw implementatieplan DB2. Uw zakelijke vereisten zullen variëren, maar hetzelfde patroon van toepassing. U kunt ook dit architectuurpatroon voor online analytical processing (OLAP)-toepassingen op Azure.

In dit artikel wordt niet beschreven verschillen en mogelijke migratietaken voor het verplaatsen van een IBM DB2 voor z/OS-database naar IBM DB2 pureScale die worden uitgevoerd op Linux. En biedt het geen schattingen van de grootte en workload analyses voor het verplaatsen van DB2 z/OS naar DB2 pureScale. 

Voor hulp bij het bepalen van de beste DB2 pureScale architectuur voor uw omgeving, wordt aangeraden dat u volledig schatting van de grootte en een hypothese maken. Zorg ervoor dat rekening houden met DB2 z/OS Parallel op het bronsysteem Sysplex met architectuur delen van gegevens, configuratie van de koppeling faciliteit en gebruiksstatistieken van gedistribueerde gegevens faciliteit (DDF).

> [!NOTE]
> In dit artikel beschrijft een benadering voor de DB2-migratie, maar er zijn andere gebruikers. DB2 pureScale kunt bijvoorbeeld ook uitvoeren in gevirtualiseerde on-premises omgevingen. IBM DB2 op Microsoft Hyper-V ondersteunt in verschillende configuraties. Zie voor meer informatie, [architectuur van virtualisatie voor DB2 pureScale](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/r0061462.html) in het midden van de kennis IBM.

## <a name="architecture"></a>Architectuur

Ter ondersteuning van hoge beschikbaarheid en schaalbaarheid op Azure, kunt u een scale-out, gedeelde data-architectuur voor DB2 pureScale. De migratie van de klant gebruikt het volgende voorbeeld-architectuur.

![DB2-pureScale op Azure virtual machines weergeven van opslag en netwerken](media/db2-purescale-on-azure/pureScaleArchitecture.png "DB2 pureScale op Azure virtual machines weergeven van opslag en netwerken")


Het diagram toont de logische lagen die nodig zijn voor een DB2-pureScale-cluster. Hieronder vallen virtuele machines voor een client, voor beheer, voor in cache opslaan, voor de database-engine en voor gedeelde opslag. 

Naast de knooppunten in database-engine bevat het diagram twee knooppunten die worden gebruikt voor caching faciliteiten (CFs)-cluster. Ten minste twee knooppunten worden gebruikt voor de database-engine zelf. Een DB2-server die deel uitmaakt van een cluster pureScale heet een lid. 

Het cluster is verbonden via iSCSI-een cluster met drie knooppunten GlusterFS gedeelde opslag voor scale-out opslag en hoge beschikbaarheid. DB2 pureScale is geïnstalleerd op virtuele Azure-machines waarop Linux wordt uitgevoerd.

Deze aanpak is een sjabloon die u voor de grootte en de schaal van uw organisatie wijzigen kunt. Deze gebaseerd op het volgende:

-   Twee of meer leden van de database worden gecombineerd met ten minste twee CF knooppunten. De knooppunten beheren een globale buffergroep (Engelse) voor gedeeld geheugen en globale vergrendeling manager (GLM)-services voor het beheren van gedeelde toegang en vergrendelen conflicten van actieve leden. Één CF knooppunt fungeert als de primaire en de andere als de secundaire, CF-failoverknooppunt. Om te voorkomen dat een single point of failure wordt in de omgeving, worden in een DB2 pureScale cluster ten minste vier knooppunten vereist.

-   Gedeelde opslag met hoge prestaties (weergegeven in P30-grootte in het diagram). Elk van de knooppunten Gluster FS maakt gebruik van deze opslag.

-   Hoogwaardige netwerken voor de gegevensleden en gedeelde opslag.

### <a name="compute-considerations"></a>Overwegingen voor COMPUTE

Deze architectuur voert de toepassing, opslag en lagen op Azure virtual machines. De [implementatie installatiescripts](https://aka.ms/db2onazure) maken het volgende:

-   Een DB2-pureScale-cluster. Het type van de compute-resources die u nodig hebt op Azure, is afhankelijk van uw installatie. In het algemeen kunt u op twee manieren gebruiken:

    -   Gebruik een meerdere knooppunten, high performance computing (HPC)-stijl netwerk waar de gedeelde opslag voor het openen van kleine en middelgrote instanties. Voor deze configuratie HPC type Azure geoptimaliseerd voor geheugen E-serie of opslag geoptimaliseerd L-serie [virtuele machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) de benodigde rekenkracht bieden.

    -   Gebruik minder grote virtuele machine-instanties voor de gegevensengines voor. Voor grote instanties, de grootste geoptimaliseerd voor geheugen [M-serie](https://azure.microsoft.com/pricing/details/virtual-machines/series/) virtuele machines zijn ideaal voor zware in-memory workloads. Mogelijk moet u een toegewezen exemplaar, afhankelijk van de grootte van de logische partitie (LPAR) die wordt gebruikt om uit te voeren DB2.

-   De DB2-CF maakt gebruik van virtuele machines geoptimaliseerd voor geheugen, zoals E-serie of L-serie.

-   GlusterFS storage maakt gebruik van Standard\_DS4\_v2 virtuele machines waarop Linux wordt uitgevoerd.

-   Een jumpbox GlusterFS is een standaard\_DS2\_v2 virtuele machine waarop Linux wordt uitgevoerd.

-   De client is een standaard\_DS3\_v2 virtuele machine met Windows (gebruikt voor testdoeleinden).

-   Een witness-server is een standaard\_DS3\_v2 virtuele machine waarop Linux (gebruikt voor DB2 pureScale) wordt uitgevoerd.

> [!NOTE]
> Een cluster met DB2 pureScale vereist ten minste twee instanties van de DB2. Het is ook vereist een cache-exemplaar en een exemplaar van de manager vergrendelen.

### <a name="storage-considerations"></a>Opslagoverwegingen

DB2 pureScale is, zoals Oracle RAC, een krachtige blok i/o, scale-out-database. Wordt u aangeraden de grootste [Azure premium SSD](disks-types.md) -optie die past bij uw behoeften. Kleinere opslagopties mogelijk geschikt voor ontwikkelings- en testomgevingen, terwijl de productie-omgevingen moeten vaak meer opslagcapaciteit. Maakt gebruik van de voorbeeld-architectuur [P30](https://azure.microsoft.com/pricing/details/managed-disks/) vanwege de verhouding tussen de IOPS op grootte en prijs. Ongeacht de grootte, Premium Storage te gebruiken voor de beste prestaties.

DB2 pureScale maakt gebruik van een gedeelde-alles architectuur, waarbij alle gegevens zijn toegankelijk is vanaf alle clusterknooppunten. Premium-opslag moet worden gedeeld tussen instanties, of op aanvraag of op specifieke instanties.

Een grote DB2 pureScale cluster kunt 200 terabytes (TB) is vereist of meer van de premium-opslag, gedeeld met IOPS van 100.000. DB2 pureScale ondersteunt de interface van een iSCSI-blokkeren die u in Azure gebruiken kunt. De iSCSI-interface is vereist voor een cluster met gedeelde opslag die u met GlusterFS, S2D of een ander hulpprogramma implementeren kunt. Dit type oplossing maakt een apparaat virtuele storage area network (SAN) in Azure. DB2 pureScale maakt gebruik van het virtuele SAN voor het installeren van de geclusterde bestandsserver-systeem dat wordt gebruikt voor het delen van gegevens tussen virtuele machines.

De voorbeeld-architectuur maakt gebruik van GlusterFS, een gratis, schaalbare, open-source gedistribueerd bestandssysteem dat geoptimaliseerd voor opslag in de cloud.

### <a name="networking-considerations"></a>Aandachtspunten voor netwerken

IBM raadt InfiniBand-netwerken aan voor alle leden in een DB2-pureScale-cluster. DB2 pureScale gebruikt ook remote direct memory access (RDMA), indien beschikbaar, voor de CFs.

Tijdens de installatie, maakt u een Azure [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) op alle virtuele machines bevatten. U kunt groeperen in het algemeen resources op basis van hun levensduur en die ze gaan beheren. De virtuele machines in deze architectuur [versnelde netwerken](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Het is een Azure-functie waarmee consistente en zeer lage netwerklatentie via één-i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine.

Elke virtuele machine van Azure wordt geïmplementeerd in een virtueel netwerk met subnetten: main, Gluster FS-front-end (gfsfe), Gluster FS back-end (bfsbe), DB2 pureScale (db2be) en DB2 pureScale front end (db2fe). Het installatiescript maakt ook de primaire [NIC's](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics) op de virtuele machines in de belangrijkste subnet.

Gebruik [netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) om netwerkverkeer binnen het virtuele netwerk te beperken en om te isoleren van de subnetten.

In Azure moet DB2 pureScale TCP/IP gebruiken als de netwerkverbinding voor de opslag.

## <a name="next-steps"></a>Volgende stappen

-   [Deze architectuur op Azure implementeren](deploy-ibm-db2-purescale-azure.md)
