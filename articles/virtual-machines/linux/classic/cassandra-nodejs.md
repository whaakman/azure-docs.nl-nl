---
title: Een Cassandra-cluster op Linux in Azure uitvoeren met Node.js
description: Hoe u een Cassandra-cluster op Linux in Azure Virtual Machines uitvoert vanuit een Node.js-app
services: virtual-machines-linux
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: ''
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: d99c9732bb1bf494b87d2073ba002264c7a51634
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221244"
---
# <a name="run-a-cassandra-cluster-on-linux-in-azure-with-nodejs"></a>Een Cassandra-cluster worden uitgevoerd op Linux in Azure met behulp van Node.js

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Overzicht van Resource Manager-sjablonen voor [Datastax Enterprise](https://azure.microsoft.com/documentation/templates/datastax) en [Spark-cluster en Cassandra op CentOS](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Overzicht
Microsoft Azure is een open platform dat wordt uitgevoerd zowel Microsoft als niet-Microsoft-software, waaronder besturingssystemen, toepassingsservers, messaging middleware, evenals SQL- en NoSQL-databases van zowel commerciële en open-source-modellen. Het bouwen van flexibele services in openbare clouds, met inbegrip van Azure vereist zorgvuldige planning en doelbewuste architectuur voor beide toepassingsservers als en storage-lagen. De gedistribueerde opslagarchitectuur van Cassandra helpt op natuurlijke wijze in het bouwen van maximaal beschikbare systemen die fouttolerant cluster fouten zijn. Cassandra is een cloudschaal onderhouden door Apache Software Foundation op cassandra.apache.org NoSQL-database. Cassandra is geschreven in Java. Dus deze wordt uitgevoerd op zowel op Windows en Linux-platforms.

De focus van dit artikel is om weer te geven van de implementatie van Cassandra in Ubuntu als een cluster met één of meerdere data center die gebruikmaakt van Azure Virtual Machines en virtuele netwerken. De implementatie van het cluster voor werkbelastingen voor productie geoptimaliseerd valt buiten het bereik van dit artikel omdat hiervoor meerdere schijven knooppuntconfiguratie, het juiste ring topologie ontwerpen en gegevensmodellering ter ondersteuning van de benodigde replicatie, gegevensconsistentie, doorvoer en hoog vereisten voor beschikbaarheid.

In dit artikel wordt een fundamentele benadering om weer te geven wat is betrokken bij het bouwen van de Cassandra-cluster in vergelijking met Docker, Chef of Puppet zodat de infrastructuur veel eenvoudiger.  

## <a name="the-deployment-models"></a>Het implementatiemodel
Microsoft Azure-netwerken kunt de implementatie van geïsoleerde particuliere clusters, de toegang van die beperkt worden kan tot goed korrelig netwerkbeveiliging bereiken.  Omdat in dit artikel over het weergeven van de Cassandra-implementatie op een fundamenteel niveau, niet het zich richten op het consistentieniveau van de en de optimale Opslagontwerp voor doorvoer. Hier volgt de lijst met de netwerkvereisten voor de hypothetische cluster:

* Externe systemen geen toegang tot de Cassandra-database van binnen of buiten Azure
* Cassandra-cluster moet zich achter een load balancer voor thrift-verkeer
* Cassandra-knooppunten in twee groepen in elk Datacenter voor een beschikbaarheid van het verbeterde cluster implementeren
* Vergrendelen het cluster zodanig dat alleen application server-farm toegang tot de database rechtstreeks heeft
* Er zijn geen openbare netwerken eindpunten dan SSH
* Elke Cassandra-knooppunt moet een vaste interne IP-adres

Cassandra kan worden geïmplementeerd naar één Azure-regio of in meerdere regio's op basis van de gedistribueerde aard van de werkbelasting. U kunt een model voor implementatie in meerdere regio's gebruiken om eindgebruikers te verkorten in een bepaalde geo via dezelfde Cassandra-infrastructuur. Cassandra van ingebouwde knooppunt replicatie zorgt voor de synchronisatie van meerdere masters schrijft die afkomstig zijn van meerdere datacenters en biedt een consistente weergave van de gegevens voor toepassingen. Implementatie in meerdere regio's kan ook helpen bij de risico's te beperken van de bredere Azure-service-onderbrekingen. Instelbare consistentie en replicatie-topologie van de Cassandra helpt bij het voldoen aan diverse RPO behoeften van toepassingen.

### <a name="single-region-deployment"></a>Implementatie in één regio 's
Laten we beginnen met een implementatie met één regio en harvest de geleerde lessen bij het maken van een model voor meerdere regio's. Azure-virtuele netwerken wordt gebruikt voor het maken van geïsoleerde subnetten, zodat de netwerkvereisten beveiliging de hierboven genoemde kunnen worden voldaan.  Ubuntu 14.04 LTS en Cassandra 2.08 maakt gebruik van de procedure beschreven in de implementatie van één regio wordt gemaakt. Het proces kan echter eenvoudig worden vastgesteld met de andere varianten van Linux. Hier volgen enkele van de systematische kenmerken van de implementatie van één regio.  

**Hoge beschikbaarheid:** de Cassandra-knooppunten die worden weergegeven in afbeelding 1 op twee beschikbaarheidssets worden geïmplementeerd, zodat de knooppunten worden verdeeld tussen meerdere domeinen met fouten voor hoge beschikbaarheid. Virtuele machines van aantekeningen voorzien met elke beschikbaarheidsset is toegewezen aan 2 foutdomeinen. Azure maakt gebruik van het concept van foutdomein voor het beheren van niet-geplande uitvaltijd (bijvoorbeeld hardware- of hardwarestoringen). Het concept van het upgradedomein (bijvoorbeeld een host of Gast-OS patches of upgrades worden uitgevoerd, upgrades van toepassingen) wordt gebruikt voor het beheer van geplande uitvaltijd. Raadpleeg [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](http://msdn.microsoft.com/library/dn251004.aspx) voor de rol van de fout- en upgradedomeinen in het bereiken van hoge beschikbaarheid.

![Implementatie in één regio 's](./media/cassandra-nodejs/cassandra-linux1.png)

Afbeelding 1: Implementatie van één regio

Houd er rekening mee dat op het moment van schrijven van dit Azure niet toegestaan om de expliciete toewijzing van een groep van virtuele machines naar een specifieke foutdomein; Als gevolg daarvan kunnen zelfs met het implementatiemodel dat wordt weergegeven in afbeelding 1, is het statistisch waarschijnlijk dat alle virtuele machines kunnen worden toegewezen aan twee foutdomeinen in plaats van vier.

**Load Balancing Thrift-verkeer:** Thrift-clientbibliotheken binnen de webserver verbinding met het cluster via een interne load balancer. Hiervoor moet het proces van de interne load balancer toe te voegen aan het subnet "gegevens" (Zie afbeelding 1) in de context van de cloudservice die als host fungeert voor de Cassandra-cluster. Nadat de interne load balancer is gedefinieerd, moet elk knooppunt het eindpunt met gelijke moet worden toegevoegd met de aantekeningen van een set met gelijke met de naam eerder gedefinieerde load balancer. Zie [Azure Internal Load Balancing ](../../../load-balancer/load-balancer-internal-overview.md)voor meer informatie.

**Cluster Seeds:** is het belangrijk om te selecteren van de meeste maximaal beschikbare knooppunten voor seeds omdat de nieuwe knooppunten die met de seed-knooppunten voor het detecteren van de topologie van het cluster communiceren. Eén knooppunt uit elke beschikbaarheidsset is aangewezen als seed-knooppunten om te voorkomen dat één storingspunt is.

**Replicatiefactor en Consistentieniveau:** Cassandra van ingebouwde hoge beschikbaarheid en gegevens duurzaamheid wordt gekenmerkt door de replicatie van meerdere factoren (RF - aantal exemplaren van elke rij die zijn opgeslagen op het cluster) en Consistentieniveau (het aantal replica's worden gelezen of weggeschreven voordat het resultaat wordt geretourneerd voor de oproepende functie). Replicatiefactor wordt opgegeven tijdens het maken van de KEYSPACE (vergelijkbaar met een relationele database), terwijl het consistentieniveau is opgegeven tijdens het uitgeven van de CRUD-query. Zie de documentatie bij Cassandra [configureren voor consistentie](https://docs.datastax.com/en/cassandra/3.0/cassandra/dml/dmlConfigConsistency.html) voor consistentie van gegevens en de formule voor het quorum berekening.

Cassandra ondersteunt twee typen integriteit gegevensmodellen – consistentie en uiteindelijke consistentie; de replicatie van meerdere factoren en Consistentieniveau vaststellen samen of de gegevens consistent als een schrijfbewerking voltooid of uiteindelijk consistent is. Bijvoorbeeld, QUORUM op te geven als het Consistentieniveau altijd zorgt ervoor gegevens consistentie bij elk consistentieniveau, onder het aantal replica's worden geschreven dat naar behoefte te bereiken QUORUM (bijvoorbeeld één) resulteert in gegevens uiteindelijk consistent.

De 8-node cluster hierboven, met een replicatiefactor van 3 en QUORUM (2 knooppunten worden gelezen of geschreven voor consistentie) lezen/schrijven consistentieniveau, kunnen het theoretische verlies van gegevens van maximaal 1 knooppunt per replicatiegroep overleven voordat de toepassing start vatten het is mislukt. Hierbij wordt ervan uitgegaan dat alle de belangrijkste spaties zijn goed met gelijke taakverdeling voor lezen/schrijven aanvragen.  Hier volgen de parameters voor de geïmplementeerde cluster gebruikt:

Configuratie voor één regio Cassandra-cluster:

| Cluster-Parameter | Waarde | Opmerkingen |
| --- | --- | --- |
| Aantal knooppunten (N) |8 |Totale aantal knooppunten in het cluster |
| Replicatiefactor (RF) |3 |Aantal replica's van een bepaalde rij |
| Consistentieniveau (schrijven) |QUORUM[(RF/2) +1) = 2] het resultaat van de formule wordt omlaag afgerond |Maximaal 2 replica's schrijft voordat de reactie wordt verzonden naar de oproepende functie 3e replica is geschreven in een uiteindelijk consistente manier. |
| Consistentieniveau (lezen) |QUORUM [(RF/2) + 1 = 2] het resultaat van de formule wordt omlaag afgerond |Hiermee leest u 2 replica's moet worden verzonden naar de aanroepende functie. |
| Replicatiestrategie |Zie NetworkTopologyStrategy [gegevensreplicatie](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeAbout.html) in Cassandra-documentatie voor meer informatie |De implementatietopologie begrijpt en replica's op knooppunten geplaatst, zodat alle replica's niet op de dezelfde rek eindigt |
| Snitch |Zie GossipingPropertyFileSnitch [Switches](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archSnitchesAbout.html) in Cassandra-documentatie voor meer informatie |Een concept van snitch NetworkTopologyStrategy gebruikt om te begrijpen van de topologie. GossipingPropertyFileSnitch biedt betere controle bij het toewijzen van elk knooppunt naar het datacenter en rack. Het cluster gebruikt vervolgens roddels om deze informatie doorgegeven. Dit is veel eenvoudiger in instelling voor dynamische IP ten opzichte van PropertyFileSnitch |

**Azure overwegingen voor het Cassandra-Cluster:** Microsoft Azure Virtual Machines-functie maakt gebruik van Azure Blob-opslag voor de schijfpersistentie; Azure Storage slaat drie replica's van elke schijf voor hoge duurzaamheid. Dit betekent dat elke rij gegevens in een Cassandra-tabel ingevoegd is al opgeslagen in drie replica's. Dus is de consistentie van gegevens al afgehandeld, zelfs als de replicatie van meerdere factoren (RF) 1 is. Het belangrijkste probleem met Replicatiefactor 1 is dat de toepassing uitvaltijd optreedt, zelfs als een enkel Cassandra-knooppunt is mislukt. Als een knooppunt is niet beschikbaar vanwege de problemen (bijvoorbeeld hardware, software systeemfouten) wordt herkend door de Azure-Infrastructuurcontroller, richt het een nieuw knooppunt in plaats daarvan de dezelfde opslag-schijven. Een nieuw knooppunt ter vervanging van de oude heeft ingericht, kan een paar minuten duren.  Op dezelfde manier voor gepland onderhoudsactiviteiten zoals Gast OS wijzigingen, Cassandra wordt bijgewerkt en wijzigingen in de toepassing Azure-Infrastructuurcontroller voert rolling upgrades van de knooppunten in het cluster.  Rolling upgrades ook kan duren voordat u een paar knooppunten tegelijk en kan daarom korte uitvaltijd voor een aantal partities kan optreden in het cluster. Echter, de gegevens niet verloren vanwege de ingebouwde redundantie voor Azure Storage.  

Voor systemen die zijn geïmplementeerd in Azure die geen hoge beschikbaarheid vereist (bijvoorbeeld ongeveer 99,9 die gelijk is aan 8.76 uur/jaar; Zie [hoge beschikbaarheid](http://en.wikipedia.org/wiki/High_availability) voor meer informatie) kunt u mogelijk om uit te voeren bij RF = 1 en Consistentieniveau = een.  Voor toepassingen met hoge beschikbaarheid vereisen, RF = 3 en Consistentieniveau = QUORUM maximaal wordt toegestaan de tijd van een van de knooppunten van de replica's. RF = 1 in traditionele implementaties (bijvoorbeeld on-premises) kunnen niet worden gebruikt vanwege het mogelijke gegevensverlies resultaat zijn van problemen, zoals schijffouten te voorkomen.   

## <a name="multi-region-deployment"></a>Implementatie in meerdere regio 's
Cassandra van data-center-bewuste replicatie en consistent model die hierboven worden beschreven, helpt bij de implementatie voor meerdere regio's zonder de noodzaak van een externe hulpprogramma's. Dit wijkt af van de traditionele relationele databases waarin de instellingen voor het spiegelen van de database voor meerdere masters schrijfbewerkingen mogelijk complex. Cassandra in een configuratie voor meerdere regio's kan helpen bij het gebruik van scenario's met inbegrip van de scenario's:

**Implementatie op basis van de service:** toepassingen met meerdere tenants, met de toewijzing van tenantgebruikers wissen-naar-regio, kan worden geprofiteerd van de lage latentie van het cluster meerdere regio's. Bijvoorbeeld, een learning beheersystemen voor onderwijsinstellingen kunnen een gedistribueerde cluster implementeren in VS-Oost en VS-West-regio's voor het bieden van de respectieve campussen voor transactionele en analytics. De gegevens lokaal consistent kunnen zijn op de tijd lees- en schrijfbewerkingen en uiteindelijk consistent kunt worden zowel de regio's. Er zijn andere voorbeelden zoals mediadistributie, e-commerce en codes en alles wat u geconcentreerd geo-gebruiker basis fungeert een goede use-case voor dit implementatiemodel is.

**Hoge beschikbaarheid:** redundantie is een belangrijke factor bij het bereiken van hoge beschikbaarheid van de software en hardware; Zie de betrouwbare Cloud-systemen bouwen op Microsoft Azure voor meer informatie. Op Microsoft Azure is het alleen betrouwbare manier van echte redundantie bereiken door het implementeren van een cluster met meerdere regio's. Toepassingen kunnen worden geïmplementeerd in de modus actief-actief of actief-passief en als een van de regio's niet actief is, Azure Traffic Manager kunt omleiden van verkeer naar de actieve regio.  Met de implementatie van één regio als de beschikbaarheid van 99,9, is de implementatie van een twee-regio's kan bereiken een beschikbaarheid van 99,9999 berekend door de formule: (1-(1-0.999) * (1-0,999)) * 100); Raadpleeg het bovenstaande artikel voor meer informatie.

**Herstel na noodgevallen:** meerdere regio's Cassandra-cluster, als het goed is ontworpen, bestand zijn tegen catastrofale data center storingen. Als één regio is niet actief is, kan de toepassing geïmplementeerd in andere regio's kunt starten voor de eindgebruikers. Net als elke andere zakelijke continuïteit-implementaties heeft de toepassing moet fouttolerante voor gegevens verloren gaan die voortvloeien uit de gegevens in de asynchrone pijplijn. Cassandra is echter het herstel veel sneller dan de tijd die door processen voor het herstellen van traditionele database. Afbeelding 2 ziet u het model van de normale implementatie voor meerdere regio's met acht knooppunten in elke regio. Beide regio's zijn kopieën van de mirror van elkaar voor dezelfde van symmetrie; echte wereld mogelijk te maken, is afhankelijk van het type werkbelasting (zoals transactionele of analytische), RPO, RTO, gegevensconsistentie en beschikbaarheidsvereisten.

![Implementatie in meerdere regio 's](./media/cassandra-nodejs/cassandra-linux2.png)

Afbeelding2: Implementatie van Cassandra in meerdere regio 's

### <a name="network-integration"></a>Netwerkintegratie
Sets van virtuele machines met particuliere netwerken die zich op twee regio's bevinden die communiceert met elkaar met behulp van een VPN-tunnel. De VPN-tunnel maakt verbinding met twee software-gateways ingericht tijdens het implementatieproces van het netwerk. Beide regio's hebben vergelijkbare netwerkarchitectuur in termen van de subnetten 'web' en 'gegevens'; Azure-netwerken kunt het maken van zo veel subnetten indien nodig en ACL's toepassen, zoals die nodig zijn voor netwerkbeveiliging. Inter tijdens het ontwerpen van de clustertopologie, data center communicatie latentie en de economische impact van verkeer netwerk hoeft te worden beschouwd.

### <a name="data-consistency-for-multi-data-center-deployment"></a>De consistentie van gegevens voor de implementatie van meerdere Data Center
Gedistribueerde implementaties hoeven te zijn op de hoogte van de cluster-topologie-impact op de doorvoer en een hoge beschikbaarheid. De RF en Consistentieniveau moeten zodanig dat het quorum niet afhankelijk van de beschikbaarheid van de datacenters worden geselecteerd.
Voor een systeem dat hoge consistentie moet ervoor een LOCAL_QUORUM voor consistentieniveau (voor lees- en schrijfbewerkingen) zorgt dat de lokale lees- en schrijfbewerkingen wordt voldaan van de lokale knooppunten terwijl gegevens asynchroon worden gerepliceerd naar de externe datacenters.  Tabel 2 bevat een overzicht van de configuratiegegevens voor het cluster meerdere regio's is die later in het schrijven van worden beschreven.

**Cassandra-clusterconfiguratie twee regio 's**

| Cluster-Parameter | Waarde | Opmerkingen |
| --- | --- | --- |
| Aantal knooppunten (N) |8 + 8 |Totale aantal knooppunten in het cluster |
| Replicatiefactor (RF) |3 |Aantal replica's van een bepaalde rij |
| Consistentieniveau (schrijven) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] het resultaat van de formule wordt omlaag afgerond |2 knooppunten wordt geschreven naar het eerste Datacenter synchroon; de extra 2 knooppunten die nodig zijn voor het quorum wordt asynchroon geschreven naar het 2e Datacenter. |
| Consistentieniveau (lezen) |LOCAL_QUORUM ((RF/2) + 1) = 2 is het resultaat van de formule wordt omlaag afgerond |Leesaanvragen is van slechts één regio, voldaan 2 knooppunten worden gelezen voordat het antwoord terug naar de client wordt verzonden. |
| Replicatiestrategie |Zie NetworkTopologyStrategy [gegevensreplicatie](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeAbout.html) in Cassandra-documentatie voor meer informatie |De implementatietopologie begrijpt en replica's op knooppunten geplaatst, zodat alle replica's niet op de dezelfde rek eindigt |
| Snitch |Zie GossipingPropertyFileSnitch [Snitches](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archSnitchesAbout.html) in Cassandra-documentatie voor meer informatie |Een concept van snitch NetworkTopologyStrategy gebruikt om te begrijpen van de topologie. GossipingPropertyFileSnitch biedt betere controle bij het toewijzen van elk knooppunt naar het datacenter en rack. Het cluster gebruikt vervolgens roddels om deze informatie doorgegeven. Dit is veel eenvoudiger in instelling voor dynamische IP ten opzichte van PropertyFileSnitch |

## <a name="the-software-configuration"></a>DE SOFTWARECONFIGURATIE
De volgende softwareversies worden gebruikt tijdens de implementatie:

<table>
<tr><th>Software</th><th>Bron</th><th>Versie</th></tr>
<tr><td>JAVA RUNTIME ENVIRONMENT    </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 TNS</td></tr>
</table>

Wanneer u de JRE downloadt, moet u handmatig de Oracle-softwarelicentie accepteren. Dus, ter vereenvoudiging van de implementatie, downloadt u alle vereiste software op het bureaublad. Vervolgens te uploaden naar de Ubuntu-sjablooninstallatiekopie te maken als een voordat de implementatie van het cluster.

De bovenstaande software downloaden naar een bekende downloadmap (bijvoorbeeld %TEMP%/downloads op Windows of ~/Downloads op de meeste Linux-distributies of Mac) op de lokale computer.

### <a name="create-ubuntu-vm"></a>UBUNTU-VM MAKEN
In deze stap van het proces maakt u Ubuntu-installatiekopie met de vereiste software zodat de installatiekopie opnieuw kan worden gebruikt voor het inrichten van verschillende Cassandra-knooppunten.  

#### <a name="step-1-generate-ssh-key-pair"></a>STAP 1: De SSH-sleutelpaar genereren
Azure heeft een openbare sleutel die is PEM of DER-gecodeerd met de inrichtingstijd X509 nodig. Een van de instructies die zich bevindt in SSH gebruiken met Linux op Azure openbaar/persoonlijk sleutelpaar genereren. Als u van plan bent om te putty.exe gebruiken als een SSH-client op Windows of Linux, hebt u de PEM gecodeerd converteren RSA persoonlijke sleutel naar met behulp van puttygen.exe PPK-indeling. De instructies hiervoor vindt u in de bovenstaande webpagina wordt weergegeven.

#### <a name="step-2-create-ubuntu-template-vm"></a>STAP 2: Ubuntu sjabloon voor virtuele machine maken
De VM-sjabloon maken, meld u aan bij de Azure-portal en de volgende reeks gebruiken: klik op Nieuw, berekenen, virtuele MACHINE, FROM GALLERY, UBUNTU, Ubuntu Server 14.04 TNS, en klik vervolgens op de pijl-rechts. Zie voor een zelfstudie waarin wordt beschreven hoe u een Linux-VM maken, maken een Linux virtuele Machine wordt uitgevoerd.

Voer de volgende informatie op het scherm "virtuele-machineconfiguratie" #1:

<table>
<tr><th>VELDNAAM              </td><td>       VELDWAARDE               </td><td>         OPMERKINGEN                </td><tr>
<tr><td>VERSIE RELEASEDATUM    </td><td> Selecteer een datum in de vervolgkeuzelijst omlaag</td><td></td><tr>
<tr><td>NAAM VAN VIRTUELE MACHINE    </td><td> CAS-instanties-sjabloon                   </td><td> Dit is de hostnaam van de virtuele machine </td><tr>
<tr><td>LAAG                     </td><td> STANDARD                           </td><td> Laat de standaardwaarde              </td><tr>
<tr><td>GROOTTE                     </td><td> A1                              </td><td>Selecteer de virtuele machine op basis van de behoeften van de i/o; Laat de standaardwaarde voor dit doel. </td><tr>
<tr><td> NIEUWE GEBRUIKERSNAAM             </td><td> localadmin                       </td><td> 'admin' is een gereserveerde gebruikersnaam in Ubuntu 12. xx en na</td><tr>
<tr><td> VERIFICATIE         </td><td> Klik op selectievakje                 </td><td>Controleer of u wilt beveiligen met een SSH-sleutel </td><tr>
<tr><td> CERTIFICAAT             </td><td> bestandsnaam van de openbare-sleutelcertificaat </td><td> Gebruik de openbare sleutel die eerder zijn gegenereerd</td><tr>
<tr><td> Nieuw wachtwoord    </td><td> sterk wachtwoord </td><td> </td><tr>
<tr><td> Wachtwoord bevestigen    </td><td> sterk wachtwoord </td><td></td><tr>
</table>

Voer de volgende informatie op het scherm "virtuele-machineconfiguratie" #2:

<table>
<tr><th>VELDNAAM             </th><th> VELDWAARDE                       </th><th> OPMERKINGEN                                 </th></tr>
<tr><td> CLOUDSERVICE    </td><td> Een nieuwe cloudservice maken    </td><td>Cloudservice is een container compute-resources zoals virtuele machines</td></tr>
<tr><td> DE DNS-CLOUDSERVICENAAM    </td><td>ubuntu-template.cloudapp.net    </td><td>Geef een machinenaam agnostische load balancer</td></tr>
<tr><td> REGIO/AFFINITEITSGROEP/VIRTUEEL NETWERK </td><td>    US - west    </td><td> Selecteer een regio waarin uw web-apps toegang krijgen de Cassandra-cluster tot</td></tr>
<tr><td>OPSLAGACCOUNT </td><td>    Standaardwaarde gebruiken    </td><td>Het standaardopslagaccount of een vooraf gemaakte opslagaccount gebruiken in een bepaalde regio</td></tr>
<tr><td>BESCHIKBAARHEIDSSET </td><td>    Geen </td><td>    Laat dit veld leeg</td></tr>
<tr><td>EINDPUNTEN    </td><td>Standaardwaarde gebruiken </td><td>    De standaard-SSH-configuratie gebruiken </td></tr>
</table>

Klik op de pijl-rechts, laat u de standaardinstellingen op het scherm #3. Klik op de knop 'controleren' voor het voltooien van het proces van de VM-inrichting. Na een paar minuten moet de virtuele machine met de naam 'ubuntu-sjabloon' status 'running'.

### <a name="install-the-necessary-software"></a>DE BENODIGDE SOFTWARE INSTALLEREN
#### <a name="step-1-upload-tarballs"></a>STAP 1: Uploaden tarballs
Scp of pscp gebruikt, de eerder gedownloade software kopiëren naar ~/downloads directory met behulp van de opdrachtindeling van de volgende:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gz localadmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Herhaal de bovenstaande opdracht voor JRE ook als voor de Cassandra-bits.

#### <a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>STAP 2: Bereid de mapstructuur en het archief uitpakken
Meld u aan bij de virtuele machine en de mapstructuur maken en ophalen van software als een supergebruiker met behulp van de onderstaande bash-script:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Als u dit script in vim venster plakt, zorg ervoor dat u de regelterugloop verwijderen ('\r ') met behulp van de volgende opdracht uit:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>Stap 3: Enzovoort/profiel bewerken
Hiermee voegt u het volgende aan het einde:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>Stap 4: Installatie JNA voor productiesystemen
Gebruik de onderstaande opdrachtvolgorde: de volgende opdracht installeert jna 3.2.7.jar en jna-platform-3.2.7.jar naar /usr/share.java directory sudo apt-get libjna-java installeren

Symbolische koppelingen in de directory CASS_HOME/lib $ maken zodat het opstartscript Cassandra deze JAR-bestanden vindt:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>Stap 5: Cassandra.yaml configureren
Cassandra.yaml op elke virtuele machine om de configuratie die nodig zijn voor alle virtuele machines [u deze configuratie tijdens de daadwerkelijke inrichting aanpassen] bewerken:

<table>
<tr><th>Veldnaam   </th><th> Waarde  </th><th>    Opmerkingen </th></tr>
<tr><td>clusternaam </td><td>    "CustomerService"    </td><td> Gebruik de naam die overeenkomt met uw implementatie</td></tr>
<tr><td>listen_address    </td><td>[laat dit veld leeg]    </td><td> "Localhost" verwijderen </td></tr>
<tr><td>rpc_addres   </td><td>[laat dit veld leeg]    </td><td> "Localhost" verwijderen </td></tr>
<tr><td>seeds    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Lijst met alle IP-adressen die zijn aangewezen als basis.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Dit wordt gebruikt door de NetworkTopologyStrateg voor het afleiden van het datacenter en het rack van de virtuele machine</td></tr>
</table>

#### <a name="step-6-capture-the-vm-image"></a>Stap 6: De VM-installatiekopie vastleggen
Meld u aan bij de virtuele machine met behulp van de hostnaam (hk-cas-template.cloudapp.net) en de persoonlijke SSH-sleutel eerder hebt gemaakt. Zie hoe u kunt SSH gebruiken met Linux op Azure voor meer informatie over hoe u zich aanmeldt met de opdracht ssh of putty.exe.

Voer de volgende volgorde van bewerkingen voor het vastleggen van de afbeelding:

##### <a name="1-deprovision"></a>1. Inrichting
Gebruik de opdracht ' sudo waagent – inrichting + gebruiker "te verwijderen van specifieke gegevens voor een virtuele Machine-instantie. Zie voor [vastleggen van een virtuele Linux-Machine](capture-image-classic.md) kunt gebruiken om meer informatie als een sjabloon voor het vastleggen van de installatiekopie.

##### <a name="2-shut-down-the-vm"></a>2: de virtuele machine afsluiten
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling afsluiten op de opdrachtbalk onder.

##### <a name="3-capture-the-image"></a>3: de installatiekopie vastleggen
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling VASTLEGGEN vanuit de opdrachtbalk onder. In het volgende scherm, Geef een naam van de INSTALLATIEKOPIE (bijvoorbeeld hk-cas-2-08-ub-14-04-2014071), juiste beschrijving bij afbeelding, en op vinkje de "" om het proces VASTLEGGEN.

Dit proces duurt een paar seconden en de installatiekopie moet beschikbaar zijn in de sectie Mijn INSTALLATIEKOPIEËN van de galerie met installatiekopieën. De bron-VM wordt automatisch verwijderd nadat de installatiekopie met succes wordt vastgelegd. 

## <a name="single-region-deployment-process"></a>Implementatieproces voor één regio
**Stap 1: Maak het Virtueelnetwerk** Meld u aan bij de Azure-portal en een virtueel netwerk (klassiek) maken met de kenmerken die in de volgende tabel worden weergegeven. Zie [maken van een virtueel netwerk (klassiek) met behulp van de Azure-portal](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>Naam van de VM-kenmerk</th><th>Waarde</th><th>Opmerkingen</th></tr>
<tr><td>Naam</td><td>vnet-cass-west-us</td><td></td></tr>
<tr><td>Regio</td><td>US - west</td><td></td></tr>
<tr><td>DNS-servers</td><td>Geen</td><td>Negeer deze melding als er niet met behulp van een DNS-Server</td></tr>
<tr><td>Adresruimte</td><td>10.1.0.0/16</td><td></td></tr>    
<tr><td>IP-beginadres</td><td>10.1.0.0</td><td></td></tr>    
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Voeg de volgende subnetten:

<table>
<tr><th>Naam</th><th>IP-beginadres</th><th>CIDR</th><th>Opmerkingen</th></tr>
<tr><td>web</td><td>10.1.1.0</td><td>/24 (251)</td><td>Subnet voor de webfarm</td></tr>
<tr><td>gegevens</td><td>10.1.2.0</td><td>/24 (251)</td><td>Subnet voor de databaseknooppunten</td></tr>
</table>

Gegevens en Web subnetten kunnen worden beveiligd via netwerkbeveiligingsgroepen de dekking van die buiten het bereik van dit artikel valt.  

**Stap 2: Virtuele Machines voor inrichten** met behulp van de eerder hebt gemaakt, u de volgende virtuele machines in de cloud-server 'hk-c-svc-west' maken en koppelen aan de respectieve subnetten, zoals hieronder weergegeven:

<table>
<tr><th>Machinenaam    </th><th>Subnet    </th><th>IP-adres    </th><th>Beschikbaarheidsset</th><th>DC/Rack</th><th>Seed?</th></tr>
<tr><td>hk-c1-west-us    </td><td>gegevens    </td><td>10.1.2.4    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack1 = </td><td>Ja</td></tr>
<tr><td>HK-c2--VS-west    </td><td>gegevens    </td><td>10.1.2.5    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack1 =    </td><td>Nee </td></tr>
<tr><td>hk-c3-west-us    </td><td>gegevens    </td><td>10.1.2.6    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack2 =    </td><td>Ja</td></tr>
<tr><td>hk-c4-west-us    </td><td>gegevens    </td><td>10.1.2.7    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack2 =    </td><td>Nee </td></tr>
<tr><td>hk-c5-west-us    </td><td>gegevens    </td><td>10.1.2.8    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack3 =    </td><td>Ja</td></tr>
<tr><td>hk-c6-west-us    </td><td>gegevens    </td><td>10.1.2.9    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack3 =    </td><td>Nee </td></tr>
<tr><td>HK-c7--VS-west    </td><td>gegevens    </td><td>10.1.2.10    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack4 =    </td><td>Ja</td></tr>
<tr><td>hk-c8-west-us    </td><td>gegevens    </td><td>10.1.2.11    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack4 =    </td><td>Nee </td></tr>
<tr><td>HK-w1--VS-west    </td><td>web    </td><td>10.1.1.4    </td><td>hk-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
<tr><td>HK-w2--VS-west    </td><td>web    </td><td>10.1.1.5    </td><td>hk-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
</table>

Het maken van de bovenstaande lijst met virtuele machines moeten het volgende proces:

1. Maak een lege cloudservice in een bepaalde regio
2. Een virtuele machine van de eerder vastgelegde installatiekopie maken en deze koppelen aan het virtuele netwerk eerder; wordt gemaakt Herhaal dit voor alle virtuele machines
3. Een interne load balancer toevoegen aan de service in de cloud en deze koppelen aan het subnet "gegevens"
4. Voor elke virtuele machine eerder hebt gemaakt, voegt u toe een eindpunt met gelijke voor thrift-verkeer via een set met gelijke verbonden met de eerder gemaakte interne load balancer

De bovenstaande procedure kan worden uitgevoerd met behulp van Azure portal; Gebruik een Windows-machine (gebruik een virtuele machine op Azure als u geen toegang tot een Windows-machine), gebruik de volgende PowerShell-script voor het automatisch inrichten van alle 8 VM's.

**Lijst 1: PowerShell-script voor het inrichten van virtuele machines**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. create a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Stap 3: Cassandra op elke virtuele machine configureren**

Meld u aan bij de virtuele machine en voer het volgende:

* $CASS_HOME/conf/cassandra-rackdc.properties om op te geven van de data center en rack-eigenschappen bewerken:
  
       dc =EASTUS, rack =rack1
* Bewerken cassandra.yaml seed-knooppunten te configureren zoals hieronder:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Stap 4: Start de virtuele machines en testen van het cluster**

Meld u aan bij een van de knooppunten (bijvoorbeeld hk-c1-west-us) en voer de volgende opdracht om de status van het cluster te bekijken:

       nodetool –h 10.1.2.4 –p 7199 status

Hier ziet u de weergave die vergelijkbaar is met de hieronder voor een cluster met 8-knooppunten:

<table>
<tr><th>Status</th><th>Adres    </th><th>Belasting    </th><th>Tokens    </th><th>Eigenaar is van </th><th>Host-ID    </th><th>Rek</th></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (verwijderd).</td><td>rack1</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (verwijderd).</td><td>rack1</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd).</td><td>rack2</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd).</td><td>rack2</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd).</td><td>rack3</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd).</td><td>rack3</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd).</td><td>rack4</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd).</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testen van het Cluster één regio
Gebruik de volgende stappen voor het testen van het cluster:

1. Met behulp van de Powershell-opdracht Get-AzureInternalLoadbalancer commandlet, het IP-adres van de interne load balancer (bijvoorbeeld 10.1.2.101) verkrijgen. De syntaxis van de opdracht wordt hieronder weergegeven: Get-AzureLoadbalancer – ServiceName hk-c-svc--VS-west' [ziet u de details van de interne load balancer, samen met het IP-adres]
2. Meld u aan bij de virtuele machine (bijvoorbeeld hk-w1-west-us) van de webfarm gebruiken van Putty of ssh
3. Execute $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4. Gebruik de volgende CQL-opdrachten om te controleren of het cluster werkt:
   
     Maak KEYSPACE customers_ks met replicatie = {'class': 'SimpleStrategy', 'replication_factor': 3};   Gebruik customers_ks;   TABEL Customers(customer_id int PRIMARY KEY, firstname text, lastname text) maken;   INVOEGEN in Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, "Jan", "Doe");
   
     SELECTEREN * van klanten;

U ziet er ongeveer als de volgende resultaten:

<table>
  <tr><th> customer_id </th><th> Voornaam </th><th> Achternaam </th></tr>
  <tr><td> 1 </td><td> John </td><td> De Vries </td></tr>
  <tr><td> 2 </td><td> ANS </td><td> De Vries </td></tr>
</table>

De keyspace gemaakt in stap 4 gebruikt SimpleStrategy met een replication_factor van 3. SimpleStrategy wordt aanbevolen voor één data center implementaties terwijl NetworkTopologyStrategy voor meerdere data center-implementaties. Een replication_factor van 3 biedt tolerantie voor knooppuntfouten.

## <a id="tworegion"> </a>Implementatieproces voor meerdere regio 's
U gebruikmaken van de implementatie één regio is voltooid en Herhaal dezelfde procedure voor het installeren van de tweede regio. Het belangrijkste verschil tussen de implementatie in één of meerdere regio's wordt de VPN-tunnel-installatie voor de communicatie tussen regio's; u start met de installatie via het netwerk, de virtuele machines inrichten en configureer Cassandra.

### <a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Stap 1: Het virtuele netwerk maken in de 2e regio
Meld u aan bij de Azure-portal en maak een Virtueelnetwerk met de kenmerken weergeven in de tabel. Zie [een Cloud-Only Virtueelnetwerk configureren in Azure portal](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>Naam kenmerk    </th><th>Waarde    </th><th>Opmerkingen</th></tr>
<tr><td>Naam    </td><td>vnet-cass-east-us</td><td></td></tr>
<tr><td>Regio    </td><td>US - oost</td><td></td></tr>
<tr><td>DNS-servers        </td><td></td><td>Negeer deze melding als er niet met behulp van een DNS-Server</td></tr>
<tr><td>Een punt-naar-site-VPN configureren</td><td></td><td>        Negeer deze melding</td></tr>
<tr><td>Configureer een site-to-site VPN</td><td></td><td>        Negeer deze melding</td></tr>
<tr><td>Adresruimte    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>IP-beginadres    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Voeg de volgende subnetten:

<table>
<tr><th>Naam    </th><th>IP-beginadres    </th><th>CIDR    </th><th>Opmerkingen</th></tr>
<tr><td>web    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Subnet voor de webfarm</td></tr>
<tr><td>gegevens    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Subnet voor de databaseknooppunten</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>Stap 2: Lokale netwerken maken
Een lokaal netwerk in Azure virtuele netwerken is een proxy-adresruimte die wordt toegewezen aan een externe site, met inbegrip van een privécloud of een andere Azure-regio. Deze adresruimte proxy is gebonden aan een externe gateway voor routering netwerk naar de juiste netwerken bestemmingen. Zie [een VNet-naar-VNet-verbinding configureren](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) voor de instructies voor het tot stand brengen van VNET-naar-VNET-verbinding.

Hiermee maakt u twee lokale netwerken per de volgende gegevens:

| Netwerknaam | VPN-Gateway-adres | Adresruimte | Opmerkingen |
| --- | --- | --- | --- |
| hk-lnet-map-to-east-us |23.1.1.1 |10.2.0.0/16 |Geef een tijdelijke aanduiding gatewayadres tijdens het maken van het lokale netwerk. De echte gatewayadres wordt ingevuld zodra de gateway is gemaakt. Zorg ervoor dat de adresruimte exact overeenkomt met het desbetreffende externe VNET; in dit geval het VNET gemaakt in de regio VS-Oost. |
| hk-lnet-map-to-west-us |23.2.2.2 |10.1.0.0/16 |Geef een tijdelijke aanduiding gatewayadres tijdens het maken van het lokale netwerk. De echte gatewayadres wordt ingevuld zodra de gateway is gemaakt. Zorg ervoor dat de adresruimte exact overeenkomt met het desbetreffende externe VNET; in dit geval het VNET gemaakt in de regio VS-West. |

### <a name="step-3-map-local-network-to-the-respective-vnets"></a>Stap 3: "Local" netwerkverbinding naar de respectieve VNETs
Vanuit de Azure-portal, selecteert u elk vnet, klikt u op 'Configureren' controleren 'Verbinding maken met het lokale netwerk' en selecteert u de lokale netwerken per de volgende gegevens:

| Virtual Network | Lokaal netwerk |
| --- | --- |
| hk-vnet-west-us |hk-lnet-map-to-east-us |
| hk-vnet-east-us |hk-lnet-map-to-west-us |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Stap 4: Gateways maken voor VNET1 en VNET2
Klik op de GATEWAY maken voor het activeren van de VPN-gateway inrichtingsproces vanuit het dashboard van de virtuele netwerken. Na een paar minuten moet er een adres van de werkelijke gateway voor het dashboard van elk virtueel netwerk worden weergegeven.

### <a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Stap 5: Update "Local" netwerken met de respectieve "Gateway"-adressen
Bewerk de lokale netwerken ter vervanging van het IP-adres van de tijdelijke aanduiding-gateway met echte IP-adres van de zojuist ingerichte gateways. Gebruik de volgende toewijzing:

<table>
<tr><th>Lokaal netwerk    </th><th>Gateway voor een virtueel netwerk</th></tr>
<tr><td>hk-lnet-map-to-east-us </td><td>Gateway van hk-vnet-west-VS</td></tr>
<tr><td>hk-lnet-map-to-west-us </td><td>Gateway van hk-vnet-Oost-ons</td></tr>
</table>

### <a name="step-6-update-the-shared-key"></a>Stap 6: De gedeelde sleutel bijwerken
De volgende Powershell-script gebruiken om te werken van de IPSec-sleutel van elke VPN-gateway [Gebruik de sleutel geeft een beeld voor beide gateways]: Set-AzureVNetGatewayKey - VNetName hk-vnet-Oost-VS - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-west-VS - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-the-vnet-to-vnet-connection"></a>Stap 7: Maken van de VNET-naar-VNET-verbinding
Gebruik de ' ' in het menu van de virtuele netwerken gateway-naar-gateway-verbinding tot stand brengen van de Azure-portal. Gebruik de menuopdrachten 'Verbinding maken' in de onderste werkbalk. Na een paar minuten moet het dashboard de verbindingsdetails grafisch weergeven.

### <a name="step-8-create-the-virtual-machines-in-region-2"></a>Stap 8: Maak de virtuele machines in regio #2
De Ubuntu-installatiekopie maakt zoals beschreven in de regio #1-implementatie door het volgen van dezelfde stappen of kopieer de installatiekopie-VHD-bestand naar de Azure storage-account zich in regio #2 en maken van de installatiekopie. Deze installatiekopie gebruiken en de volgende lijst met virtuele machines maken in een nieuwe cloudservice hk-c-svc-Oost-VS:

| Machinenaam | Subnet | IP-adres | Beschikbaarheidsset | DC/Rack | Seed? |
| --- | --- | --- | --- | --- | --- |
| hk-c1-east-us |gegevens |10.2.2.4 |hk-c-aset-1 |DC = EASTUS rack rack1 = |Ja |
| HK-c2-Oost-ons |gegevens |10.2.2.5 |hk-c-aset-1 |DC = EASTUS rack rack1 = |Nee |
| HK-c3-Oost-ons |gegevens |10.2.2.6 |hk-c-aset-1 |DC = EASTUS rack rack2 = |Ja |
| hk-c5-east-us |gegevens |10.2.2.8 |hk-c-aset-2 |DC = EASTUS rack rack3 = |Ja |
| hk-c6-east-us |gegevens |10.2.2.9 |hk-c-aset-2 |DC = EASTUS rack rack3 = |Nee |
| HK-c7-Oost-ons |gegevens |10.2.2.10 |hk-c-aset-2 |DC = EASTUS rack rack4 = |Ja |
| hk-c8-east-us |gegevens |10.2.2.11 |hk-c-aset-2 |DC = EASTUS rack rack4 = |Nee |
| hk-w1-east-us |web |10.2.1.4 |hk-w-aset-1 |N/A |N/A |
| HK-w2-Oost-ons |web |10.2.1.5 |hk-w-aset-1 |N/A |N/A |

Volg de instructies als regio #1 maar 10.2.xxx.xxx adresruimte gebruiken.

### <a name="step-9-configure-cassandra-on-each-vm"></a>Stap 9: Cassandra op elke virtuele machine configureren
Meld u aan bij de virtuele machine en voer het volgende:

1. Bewerken $CASS_HOME/conf/cassandra-rackdc.properties om op te geven van de data center en rack-eigenschappen in de indeling: dc = EASTUS rack rack1 =
2. Bewerken cassandra.yaml voor het configureren van seed-knooppunten: Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

### <a name="step-10-start-cassandra"></a>Stap 10: Start Cassandra
Meld u aan bij elke virtuele machine en Cassandra op de achtergrond te starten met de volgende opdracht: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Testen van het Cluster meerdere regio 's
Nu is Cassandra met 16 knooppunten met 8 knooppunten in elke Azure-regio geïmplementeerd. Deze knooppunten zijn in hetzelfde cluster omdat algemene clusternaam van het en de configuratie van de seed-knooppunten. Gebruik het volgende proces voor het testen van het cluster:

### <a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Stap 1: Het interne load balancer IP-adres ophalen voor zowel de regio's met behulp van PowerShell
* Get-AzureInternalLoadbalancer -ServiceName "hk-c-svc-west-us"
* Get-AzureInternalLoadbalancer -ServiceName "hk-c-svc-east-us"  
  
    Noteer de IP-adressen (voor de voorbeeld-west - 10.1.2.101, Oost - 10.2.2.101) weergegeven.

### <a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Stap 2: Voer het volgende uit in de regio west nadat u bent aangemeld in hk-w1--VS-west
1. Execute $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2. Voer de volgende CQL-opdrachten uit:
   
     Maak KEYSPACE customers_ks met replicatie = {'class': 'NetworkToplogyStrategy', 'WESTUS': 3 'EASTUS': 3};   Gebruik customers_ks;   TABEL Customers(customer_id int PRIMARY KEY, firstname text, lastname text) maken;   INVOEGEN in Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, "Jan", "Doe");   SELECTEREN * van klanten;

U ziet een scherm zoals hieronder:

| customer_id | Voornaam | Achternaam |
| --- | --- | --- |
| 1 |John |De Vries |
| 2 |ANS |De Vries |

### <a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Stap 3: Voer het volgende uit in de regio Oost nadat u bent aangemeld in hk-w1-Oost-VS:
1. Execute $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2. Voer de volgende CQL-opdrachten uit:
   
     Gebruik customers_ks;   TABEL Customers(customer_id int PRIMARY KEY, firstname text, lastname text) maken;   INVOEGEN in Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, "Jan", "Doe");   SELECTEREN * van klanten;

U ziet de dezelfde weergave zoals te zien voor de regio West:

| customer_id | Voornaam | Achternaam |
| --- | --- | --- |
| 1 |John |De Vries |
| 2 |ANS |De Vries |

Uitvoeren van enkele meer ingevoegd en Zie dat die worden gerepliceerd naar west-VS deel uitmaakt van het cluster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra-Cluster op basis van Node.js
Met behulp van een van de virtuele Linux-machines in de laag 'web' eerder hebt gemaakt, uitvoeren u een eenvoudige Node.js-script om te lezen van de eerder ingevoegde gegevens

**Stap 1: Node.js en Cassandra-Client installeren**

1. Node.js en npm installeren
2. Knooppunt pakket 'cassandra-client' installeren met npm
3. Voer het volgende script op de shell-prompt waarin de json-tekenreeks van de opgehaalde gegevens worden weergegeven:
   
        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };
   
        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }
   
        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }
   
        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);
   
           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }
   
        //update also inserts the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }
   
        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }
   
        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)

## <a name="conclusion"></a>Conclusie
Microsoft Azure is een flexibel platform waarmee het uitvoeren van zowel Microsoft als open source-software, zoals door deze oefening wordt gedemonstreerd. Maximaal beschikbare Cassandra-clusters kunnen worden geïmplementeerd op een enkel Datacenter via het spreiden van de clusterknooppunten over meerdere foutdomeinen. Cassandra-clusters kunnen ook worden geïmplementeerd in meerdere geografisch verafgelegen Azure-regio voor noodherstel bewijs systemen. Azure en Cassandra samen kunnen de constructie van uiterst schaalbare en zeer beschikbare en noodherstel herstelbare cloudservices die nodig is via het internet van vandaag schaal services.  

## <a name="references"></a>Verwijzingen
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

