---
title: Uitvoeren van een cluster Cassandra op Linux in Azure met Node.js
description: Uitvoeren van een cluster Cassandra op Linux in Azure Virtual Machines vanaf een Node.js-app
services: virtual-machines-linux
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: 
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: 00e42a00dffd1be37073f10f6ff7bff619fdee85
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="run-a-cassandra-cluster-on-linux-in-azure-with-nodejs"></a>Uitvoeren van een cluster Cassandra op Linux in Azure met behulp van Node.js

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie Resource Manager-sjablonen voor [Datastax Enterprise](https://azure.microsoft.com/documentation/templates/datastax) en [Spark-cluster en Cassandra op CentOS](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Overzicht
Microsoft Azure is een open cloudplatform waarop Microsoft en niet-Microsoft-software, waaronder besturingssystemen, toepassingsservers, messaging middleware, evenals SQL en NoSQL-databases van beide modellen commerciële en open-source. Robuuste services op openbare clouds, inclusief Azure bouwen vereist zorgvuldige planning en architectuur opzettelijk voor beide toepassingsservers als opslag en lagen. Cassandra van gedistribueerde opslagarchitectuur is natuurlijk helpt bij het bouwen van maximaal beschikbare systemen die fouttolerant voor clusters zijn. Cassandra is een cloud schaal onderhouden door Apache Software Foundation op cassandra.apache.org NoSQL-database. Cassandra is geschreven in Java. Zodat deze wordt uitgevoerd op zowel op Windows- en Linux-platforms.

De focus van dit artikel is Cassandra implementatie op Ubuntu weergeven als een cluster met één of meerdere data center die gebruikmaakt van Azure Virtual Machines en virtuele netwerken. De implementatie van het cluster voor geoptimaliseerde productieworkloads valt buiten het bereik van dit artikel omdat hiervoor meerdere schijf knooppuntconfiguratie, geschikte topologie voor ringtopologie en gegevens modelleren ter ondersteuning van de benodigde replicatie, gegevensconsistentie, doorvoer en hoge de beschikbaarheidsvereisten van de.

Dit artikel wordt een fundamenteel aanpak om weer te geven wat betrokken is bij het bouwen van het cluster Cassandra vergeleken Docker, Chef of Puppet waardoor de implementatie van de infrastructuur eenvoudiger geworden.  

## <a name="the-deployment-models"></a>Het implementatiemodel
Microsoft Azure-netwerken kunt de implementatie van een geïsoleerd particulier clusters, de toegang van die beperkt worden kan tot fijnmazige netwerkbeveiliging bereiken.  Sinds dit artikel is over het weergeven van de implementatie Cassandra op een fundamenteel niveau, niet het zich richten op het consistentieniveau van de en de optimale Opslagontwerp voor doorvoer. Hier volgt de lijst met vereisten voor het cluster hypothetische netwerken:

* Externe systemen geen toegang tot de database van de Cassandra van binnen of buiten Azure
* Cassandra cluster heeft worden achter een load balancer voor thrift-verkeer
* Cassandra knooppunten in twee groepen in elk Datacenter voor de clusterbeschikbaarheid van een verbeterde implementeren
* Vergrendelen het cluster zodanig dat alleen serverfarm toepassing toegang tot de database direct heeft
* Er is geen openbare netwerken eindpunten dan SSH
* Elk knooppunt Cassandra moet een vaste IP-adres

Cassandra kan worden geïmplementeerd op één Azure-regio of op meerdere regio's op basis van de gedistribueerde aard van de werkbelasting. Voor eindgebruikers dichter bij een bepaalde Geografie via dezelfde Cassandra infrastructuur kunt u een meerdere landen/regio-implementatiemodel. Cassandra van ingebouwde knooppunt replicatie zorgt voor de synchronisatie van meerdere master schrijft die afkomstig zijn van meerdere datacenters en geeft een consistente weergave van de gegevens van toepassingen. Implementatie van meerdere landen/regio kan ook helpen bij de risicobeperking van de breder serviceonderbrekingen van Azure. Instelbare consistentie en replicatie-topologie van de Cassandra helpt bij de behoeften van diverse RPO van toepassingen.

### <a name="single-region-deployment"></a>Implementatie van één regio
Laten we beginnen met een implementatie met één regio en verzamelt de geleerde lessen bij het maken van een model meerdere landen/regio. Azure virtuele netwerken wordt gebruikt voor het maken van geïsoleerde subnetten, zodat de netwerk-beveiligingsvereisten bovengenoemde kunnen worden voldaan.  Ubuntu 14.04 TNS en Cassandra 2.08 maakt gebruik van de procedure beschreven in de implementatie van één regio wordt gemaakt. Het proces kan eenvoudig worden vastgesteld met de andere varianten van Linux. Hier volgen enkele van de al kenmerken van de implementatie van één regio.  

**Hoge beschikbaarheid:** Cassandra knooppunten weergegeven in de afbeelding 1 wordt geïmplementeerd voor twee beschikbaarheidssets zodat de knooppunten worden verdeeld tussen meerdere domeinen met fouten voor hoge beschikbaarheid. Virtuele machines van aantekeningen voorzien met elke beschikbaarheidsset is toegewezen aan domeinen met fouten 2. Azure maakt gebruik van het concept van het foutdomein voor het beheren van niet-geplande uitvaltijd (bijvoorbeeld hardware of software-fouten). Het concept van upgradedomein (bijvoorbeeld host of Gast OS patches of upgrades worden uitgevoerd, toepassingsupgrades) wordt gebruikt voor het beheer van geplande uitvaltijd. Zie [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](http://msdn.microsoft.com/library/dn251004.aspx) voor de rol van probleem- en domeinen in het bereiken van maximale beschikbaarheid.

![Implementatie van één regio](./media/cassandra-nodejs/cassandra-linux1.png)

Afbeelding 1: Implementatie van één regio

Houd er rekening mee dat op het moment van schrijven van dit Azure niet toegestaan de expliciete toewijzing van een groep van virtuele machines naar een specifieke foutdomein; Als gevolg daarvan kan zelfs met het implementatiemodel dat wordt weergegeven in afbeelding 1, is het statistisch waarschijnlijk dat alle virtuele machines kunnen worden toegewezen aan twee domeinen met fouten in plaats van vier.

**Load Balancing Thrift-verkeer:** Thrift-clientbibliotheken binnen de webserver verbinding maken met het cluster via een interne load balancer. Hiervoor moet het proces van de interne load balancer toe te voegen aan het subnet "gegevens" (Zie afbeelding 1) in de context van de cloudservice die als host fungeert voor het cluster Cassandra. Als de interne load balancer is gedefinieerd, vereist elk knooppunt het eindpunt taakverdeling moeten worden toegevoegd met de aantekeningen van een set met gelijke taakverdeling met vooraf gedefinieerde load balancer-naam. Zie [Azure interne Load Balancing ](../../../load-balancer/load-balancer-internal-overview.md)voor meer informatie.

**Cluster zaden:** is het belangrijk dat de meeste maximaal beschikbare knooppunten voor zaden selecteren als de nieuwe knooppunten communiceren met de seed-knooppunten om de topologie van het cluster te detecteren. Eén knooppunt uit elke beschikbaarheidsset is aangewezen als seed-knooppunten om te voorkomen dat storingspunt.

**Replicatie Factor en Consistentieniveau:** Cassandra van ingebouwde hoge beschikbaarheid en gegevens duurzaamheid wordt gekenmerkt door de replicatie-Factor (RF - aantal exemplaren van elke rij die is opgeslagen op het cluster) en Consistentieniveau (aantal replica's worden gelezen/geschreven voordat het resultaat wordt teruggezonden naar de aanroeper). Replicatie factor is opgegeven tijdens het maken van KEYSPACE (vergelijkbaar met een relationele database), terwijl het consistentieniveau van de is opgegeven tijdens het uitgeven van de CRUD-query. Zie de documentatie bij Cassandra [configureren voor consistentie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor consistentie details en de formule voor quorum berekeningen.

Cassandra ondersteunt twee soorten integriteit gegevensmodellen – consistentie en uiteindelijke consistentie; de replicatie Factor en Consistentieniveau vaststellen samen of de gegevens consistent zodra een schrijfbewerking voltooid of uiteindelijk consistent is is. Bijvoorbeeld: QUORUM opgeven als het niveau van de consistentie altijd zorgt ervoor gegevens consistentie terwijl u elk consistentieniveau lager dan het aantal replica's worden geschreven dat naar behoefte te bereiken QUORUM (bijvoorbeeld een) resulteert in gegevens wordt uiteindelijk consistent is.

De 8-node cluster hierboven, met een factor van de replicatie van 3 en QUORUM (2 knooppunten worden gelezen of geschreven voor consistentie) consistentieniveau lezen/schrijven, kunnen het theoretische verlies van maximaal 1 knooppunt per replicatiegroep overleven voordat de toepassing start daar iets van merkt de is mislukt. Hierbij wordt ervan uitgegaan dat alle de belangrijkste spaties hebben ook taakverdeling lezen/schrijven aanvragen.  Hier volgen de parameters voor de geïmplementeerde cluster gebruikt:

Één regio Cassandra clusterconfiguratie:

| Cluster-Parameter | Waarde | Opmerkingen |
| --- | --- | --- |
| Het aantal knooppunten (N) |8 |Totaal aantal knooppunten in het cluster |
| Replicatie Factor (RF) |3 |Aantal replica's van een bepaalde rij |
| Consistentieniveau (schrijven) |QUORUM[(RF/2) +1) = 2] is het resultaat van de formule wordt omlaag afgerond |Maximaal 2 replica's schrijft voordat het antwoord wordt verzonden naar de aanroeper; 3e replica is geschreven in een manier uiteindelijk consistent is. |
| Consistentieniveau (lezen) |QUORUM [(RF/2) + 1 = 2] het resultaat van de formule wordt omlaag afgerond |Leest 2 replica's moet worden verzonden naar de aanroeper. |
| Een replicatiestrategie voor |Zie NetworkTopologyStrategy [gegevensreplicatie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra-documentatie voor meer informatie |De implementatietopologie begrijpt en replica's op knooppunten geplaatst, zodat alle replica's niet op hetzelfde rack eindigen |
| Snitch |Zie GossipingPropertyFileSnitch [Switches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) in Cassandra-documentatie voor meer informatie |Een concept van snitch NetworkTopologyStrategy gebruikt om te begrijpen van de topologie. GossipingPropertyFileSnitch biedt een betere controle in de toewijzing van elk knooppunt aan het datacenter en rack. Het cluster gebruikt roddels vervolgens naar deze informatie is doorgegeven. Dit is veel eenvoudiger in dynamische IP-instelling ten opzichte van PropertyFileSnitch |

**Azure overwegingen voor het Cluster Cassandra:** Microsoft Azure Virtual Machines mogelijkheid gebruikt Azure Blob-opslag voor persistentie van de schijf; Azure-opslag bespaart drie replica's van elke schijf voor maximale duurzaamheid. Dit betekent dat elke rij gegevens ingevoegd in een tabel Cassandra is al opgeslagen in drie replica's. Dus is gegevensconsistentie al afgehandeld zelfs als de replicatie Factor (RF) 1 is. Het belangrijkste probleem met replicatie Factor 1 is dat de toepassing uitvaltijd optreedt, zelfs als een enkel Cassandra knooppunt mislukt. Als een knooppunt niet actief voor de problemen (bijvoorbeeld hardware, software systeemfouten) dat wordt herkend door de Azure-Infrastructuurcontroller is, levert dit een nieuw knooppunt in plaats daarvan met de dezelfde opslagstations. Een nieuw knooppunt ter vervanging van de oude inrichting kan enkele minuten duren.  Op dezelfde manier Cassandra upgrades voor gepland onderhoudsactiviteiten zoals Gast OS wijzigingen, en wijzigingen in de toepassing Azure-Infrastructuurcontroller voert rolling upgrades van de knooppunten in het cluster.  Rolling upgrades ook kan duren voordat u een paar knooppunten tegelijk, en daarom korte uitvaltijd voor enkele partities kan optreden in het cluster. De gegevens is echter niet verloren gegaan vanwege de ingebouwde redundantie voor Azure Storage.  

Voor systemen die zijn geïmplementeerd in Azure die geen hoge beschikbaarheid vereist (bijvoorbeeld ongeveer 99,9 die gelijk is aan 8.76 hrs-jaar; Zie [hoge beschikbaarheid](http://en.wikipedia.org/wiki/High_availability) voor meer informatie) u kunt mogelijk uit te voeren met RF = 1 en Consistentieniveau = een.  Voor toepassingen met hoge beschikbaarheidsvereisten, RF = 3 en Consistentieniveau = QUORUM maximaal wordt toegestaan de uitvaltijd van een van de knooppunten van een van de replica's. RF = 1 in traditionele implementaties (bijvoorbeeld op locatie) kunnen niet worden gebruikt vanwege het mogelijk gegevens verliest als gevolg van problemen zoals schijffouten.   

## <a name="multi-region-deployment"></a>Implementatie van meerdere landen/regio
De Cassandra data center bewust replicatie en consistentie model helpt bij de implementatie van meerdere landen/regio zonder de noodzaak van een externe tooling hierboven. Dit wijkt af van de traditionele relationele databases waar de instellingen voor het spiegelen van databases voor meerdere masters schrijfacties complexe kan zijn. Cassandra in de instellingen voor een meerdere landen/regio kan helpen bij het gebruiksscenario's met inbegrip van de scenario's:

**Implementatie op basis van nabijheid:** multitenant-toepassingen met de toewijzing van gebruikers van de tenant wissen-naar-regio, kan worden geprofiteerd door lage latenties van het cluster meerdere landen/regio. Bijvoorbeeld, een learning beheersystemen voor onderwijsinstellingen kunnen implementeren een gedistribueerde cluster in VS-Oost en VS-West gebieden voor het uitvoeren van de respectieve campussen voor transactionele evenals analytics. De gegevens lokaal consistent kunnen zijn op de tijd lees- en schrijfbewerkingen en uiteindelijk consistent kan worden over zowel de regio's. Er zijn andere voorbeelden zoals distributie van de media, e-commerce en alles en alles wat u geconcentreerd geo-gebruiker basis fungeert een goede gebruiksvoorbeeld voor dit implementatiemodel is.

**Hoge beschikbaarheid:** redundantie is een belangrijke rol bij het bereiken van maximale beschikbaarheid van de software en hardware; Zie betrouwbare Cloudsystemen gebouw in Microsoft Azure voor meer informatie. In Microsoft Azure is het alleen betrouwbare manier om dat te bereiken true redundantie door het implementeren van een cluster met meerdere landen/regio. Toepassingen kunnen worden geïmplementeerd in een actief-actief of actief / passief-modus en als een van de regio's niet actief is, Azure Traffic Manager verkeer kunt omleiden naar de actieve regio.  Met de implementatie van één regio als de beschikbaarheid van 99,9, een implementatie van de twee regio kan bereiken een beschikbaarheid van 99.9999 berekend door de formule: (1-(1-0.999) * (1-0,999)) * 100); Zie het bovenstaande artikel voor meer informatie.

**Herstel na noodgevallen:** meerdere landen/regio Cassandra cluster als goed ontworpen, kan tolereren onherstelbare data center storingen. Als één regio is niet actief is, kan de toepassing geïmplementeerd naar andere regio's kunt starten voor de eindgebruikers. Net als elke andere zakelijke continuïteit-implementaties heeft de toepassing moet fouttolerante voor sommige gegevens verliest als gevolg van de gegevens in de asynchrone pijplijn. Cassandra heeft echter het herstel veel sneller dan de tijd die door processen voor het herstellen van traditionele databases. Afbeelding 2 ziet het normale implementatie voor meerdere landen/regio-model met acht knooppunten in elke regio. Beide regio's zijn kopieën van de mirror van elkaar voor dezelfde hoogte van het; concrete ontwerpen, is afhankelijk van het type werkbelasting (bijvoorbeeld transactioneel of analytische), RPO, RTO, gegevensconsistentie en beschikbaarheidsvereisten.

![Implementatie van meerdere regio](./media/cassandra-nodejs/cassandra-linux2.png)

Afbeelding2: Implementatie van meerdere landen/regio Cassandra

### <a name="network-integration"></a>Netwerkintegratie
Sets van virtuele machines die zijn geïmplementeerd op particuliere netwerken zich op twee gebieden communiceert met elkaar met behulp van een VPN-tunnel. De VPN-tunnel maakt verbinding met twee software-gateways ingericht tijdens het implementatieproces van het netwerk. Beide regio's hebben vergelijkbare netwerkarchitectuur in termen van de subnetten 'web' en 'data'; Azure-netwerken kunt het maken van zoveel subnetten indien nodig en ACL's van toepassing als nodig is voor netwerkbeveiliging. Inter data center communicatie latentie en de economische gevolgen van het netwerkverkeer hoeft te worden beschouwd bij het ontwerpen van de clustertopologie.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Consistentie van de gegevens voor de implementatie van meerdere Datacenter
Gedistribueerde implementaties moeten op de hoogte van de cluster-topologie-gevolgen voor de doorvoer en hoge beschikbaarheid. De RF en Consistentieniveau moeten zodanig dat het quorum niet afhankelijk van de beschikbaarheid van de datacenters worden geselecteerd.
Voor een systeem dat hoge consistentie moet ervoor een LOCAL_QUORUM voor consistentieniveau (voor lees- en schrijfbewerkingen) zorgt dat de lokale lees- en schrijfbewerkingen is voldaan vanaf de lokale knooppunten terwijl gegevens asynchroon gerepliceerd naar de RAS-datacenters.  Tabel 2 geeft een overzicht van de configuratiegegevens voor het beschreven later in het schrijven van meerdere landen/regio-cluster.

**Twee regio Cassandra clusterconfiguratie**

| Cluster-Parameter | Waarde | Opmerkingen |
| --- | --- | --- |
| Het aantal knooppunten (N) |8 + 8 |Totaal aantal knooppunten in het cluster |
| Replicatie Factor (RF) |3 |Aantal replica's van een bepaalde rij |
| Consistentieniveau (schrijven) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] het resultaat van de formule wordt omlaag afgerond |2-knooppunten wordt geschreven naar het eerste Datacenter synchroon; de extra 2 knooppunten die nodig zijn voor het quorum is asynchroon geschreven naar het 2e Datacenter. |
| Consistentieniveau (lezen) |LOCAL_QUORUM ((RF/2) + 1) = 2, het resultaat van de formule wordt omlaag afgerond |Alleen aanvragen wordt van slechts één regio; voldaan 2 knooppunten worden gelezen voordat het antwoord terug naar de client wordt verzonden. |
| Een replicatiestrategie voor |Zie NetworkTopologyStrategy [gegevensreplicatie](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) in Cassandra-documentatie voor meer informatie |De implementatietopologie begrijpt en replica's op knooppunten geplaatst, zodat alle replica's niet op hetzelfde rack eindigen |
| Snitch |Zie GossipingPropertyFileSnitch [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) in Cassandra-documentatie voor meer informatie |Een concept van snitch NetworkTopologyStrategy gebruikt om te begrijpen van de topologie. GossipingPropertyFileSnitch biedt een betere controle in de toewijzing van elk knooppunt aan het datacenter en rack. Het cluster gebruikt roddels vervolgens naar deze informatie is doorgegeven. Dit is veel eenvoudiger in dynamische IP-instelling ten opzichte van PropertyFileSnitch |

## <a name="the-software-configuration"></a>DE SOFTWARECONFIGURATIE
De volgende softwareversies worden gebruikt tijdens de implementatie:

<table>
<tr><th>Software</th><th>Bron</th><th>Versie</th></tr>
<tr><td>JRE    </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Wanneer u JRE downloadt, moet u handmatig de Oracle-licentie accepteren. Downloaden dus om de implementatie vereenvoudigen, de vereiste software op het bureaublad. Upload het naar de Ubuntu-sjablooninstallatiekopie maken als een voordat de implementatie van het cluster.

Download de bovenstaande software naar een bekende downloadmap (bijvoorbeeld %TEMP%/downloads op Windows of ~/Downloads op de meeste Linux-distributies of Mac) op de lokale computer.

### <a name="create-ubuntu-vm"></a>VIRTUELE UBUNTU-MACHINE MAKEN
In deze stap van het proces maken u Ubuntu installatiekopie met de vereiste software zodat de installatiekopie opnieuw kan worden gebruikt voor het inrichten van verschillende Cassandra knooppunten.  

#### <a name="step-1-generate-ssh-key-pair"></a>STAP 1: De SSH-sleutelpaar genereren
Azure heeft een openbare sleutel die is PEM of DER-gecodeerd tijdens het inrichtingsproces X509 nodig. Een van de instructies die zich bevindt op het SSH gebruiken met Linux op Azure openbaar/persoonlijk sleutelpaar genereren. Als u van plan bent te gebruiken putty.exe als SSH-client op Windows of Linux, hebt u de gecodeerde PEM converteren RSA persoonlijke sleutel naar met puttygen.exe PPK-indeling. De instructies hiervoor vindt u in de bovenstaande webpagina.

#### <a name="step-2-create-ubuntu-template-vm"></a>STAP 2: Ubuntu sjabloon VM maken
De VM-sjabloon maken, meld u aan bij de Azure-portal en gebruik de volgende volgorde: klik op Nieuw, COMPUTE, virtuele MACHINE, FROM GALERIE, UBUNTU, Ubuntu Server 14.04 TNS, en klik vervolgens op de pijl naar rechts. Zie voor een zelfstudie waarin wordt beschreven hoe u een Linux-VM's te maken, maak een virtuele Machine uitgevoerd op Linux.

Voer de volgende gegevens op het scherm 'Virtuele-machineconfiguratie' #1:

<table>
<tr><th>VELDNAAM              </td><td>       WAARDE VAN VELD               </td><td>         REMARKS                </td><tr>
<tr><td>RELEASEDATUM VERSIE    </td><td> Selecteer een datum in de vervolgkeuzelijst omlaag</td><td></td><tr>
<tr><td>NAAM VAN VIRTUELE MACHINE    </td><td> Cass-sjabloon                   </td><td> Dit is de hostnaam van de virtuele machine </td><tr>
<tr><td>LAAG                     </td><td> STANDARD                           </td><td> Laat de standaardwaarde              </td><tr>
<tr><td>GROOTTE                     </td><td> A1                              </td><td>Selecteer de virtuele machine op basis van de i/o-behoeften; Laat de standaardwaarde voor dit doel. </td><tr>
<tr><td> NIEUWE GEBRUIKERSNAAM             </td><td> localadmin                       </td><td> 'admin' is een gereserveerde gebruikersnaam in Ubuntu 12. xx en na</td><tr>
<tr><td> VERIFICATIE         </td><td> Klik op het selectievakje                 </td><td>Controleer of u wilt beveiligen met een SSH-sleutel </td><tr>
<tr><td> CERTIFICAAT             </td><td> bestandsnaam van het openbare-sleutelcertificaat </td><td> Gebruik de openbare sleutel die eerder is gegenereerd</td><tr>
<tr><td> Nieuw wachtwoord    </td><td> sterk wachtwoord </td><td> </td><tr>
<tr><td> Wachtwoord bevestigen    </td><td> sterk wachtwoord </td><td></td><tr>
</table>

Voer de volgende gegevens op het scherm 'Virtuele-machineconfiguratie' #2:

<table>
<tr><th>VELDNAAM             </th><th> WAARDE VAN VELD                       </th><th> REMARKS                                 </th></tr>
<tr><td> CLOUDSERVICE    </td><td> Maak een nieuwe cloudservice    </td><td>Cloudservice is een container compute-bronnen zoals virtuele machines</td></tr>
<tr><td> DNS-NAAM VAN CLOUD-SERVICE    </td><td>ubuntu-template.cloudapp.net    </td><td>Geef de naam van een machine agnostisch load balancer</td></tr>
<tr><td> REGIO/AFFINITEITSGROEP/VIRTUEEL NETWERK </td><td>    VS - west    </td><td> Selecteer een regio van waaruit de toegang tot het cluster Cassandra van uw webtoepassingen</td></tr>
<tr><td>OPSLAGACCOUNT </td><td>    Standaardinstelling gebruiken    </td><td>Het standaardopslagaccount of een vooraf gemaakte opslagaccount gebruiken in een bepaald gebied</td></tr>
<tr><td>BESCHIKBAARHEIDSSET </td><td>    None </td><td>    Laat dit veld leeg</td></tr>
<tr><td>EINDPUNTEN    </td><td>Standaardinstelling gebruiken </td><td>    De standaard SSH-configuratie gebruiken </td></tr>
</table>

Klik op de pijl naar rechts, de standaardinstellingen laten staan op het scherm #3. Klik op de knop 'controleren' voor het voltooien van het proces van de VM-inrichting. Na een paar minuten moet de virtuele machine met de naam 'ubuntu-sjabloon' status 'actief'.

### <a name="install-the-necessary-software"></a>DE BENODIGDE SOFTWARE INSTALLEREN
#### <a name="step-1-upload-tarballs"></a>STAP 1: Het uploaden van tarballs
Gebruik scp of pscp, kopieert u de eerder gedownloade software naar ~/downloads map met de opdrachtindeling van de volgende:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gz localadmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Herhaal de bovenstaande opdracht voor JRE ook als voor de Cassandra bits.

#### <a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>STAP 2: Bereid de mapstructuur en uitpakken van het archief
Meld u aan bij de virtuele machine en maak de mapstructuur en pak software als supergebruiker met behulp van de onderstaande bash-script:

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


Als u dit script in vim venster plakt, zorg ervoor dat de regelterugloop verwijderen ('\r ") met de volgende opdracht:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>Stap 3: Enzovoort/profiel bewerken
Het volgende aan het eind toevoegen:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>Stap 4: Installatie JNA voor productiesystemen
Gebruik de onderstaande opdrachtvolgorde: de volgende opdracht installeert jna 3.2.7.jar en jna platform 3.2.7.jar naar /usr/share.java directory sudo apt get-libjna java installeren

Symbolische koppelingen in $CASS_HOME/lib directory maken, zodat het opstartscript Cassandra deze potten kunt vinden:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>Stap 5: Cassandra.yaml configureren
Cassandra.yaml op elke virtuele machine in overeenstemming met configuratie is vereist door alle virtuele machines [u deze configuratie tijdens het werkelijke inrichten aanpassen] bewerken:

<table>
<tr><th>Veldnaam   </th><th> Waarde  </th><th>    Opmerkingen </th></tr>
<tr><td>cluster_name </td><td>    “CustomerService”    </td><td> Gebruik de naam die overeenkomt met uw implementatie</td></tr>
<tr><td>listen_address    </td><td>[laat dit veld leeg]    </td><td> Verwijderen van 'localhost' </td></tr>
<tr><td>rpc_addres   </td><td>[laat dit veld leeg]    </td><td> Verwijderen van 'localhost' </td></tr>
<tr><td>zaden    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Lijst met alle IP-adressen die zijn aangewezen als basis.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Dit wordt gebruikt door de NetworkTopologyStrateg voor het afleiden van het datacenter en het rek van de virtuele machine</td></tr>
</table>

#### <a name="step-6-capture-the-vm-image"></a>Stap 6: De VM-installatiekopie vastleggen
Meld u aan bij de virtuele machine met behulp van de hostnaam (hk-cas-template.cloudapp.net) en de persoonlijke SSH-sleutel eerder hebt gemaakt. Zie hoe SSH gebruiken met Linux op Azure voor meer informatie over het aanmelden met de opdracht ssh of putty.exe.

De volgende reeks bewerkingen voor het vastleggen van de installatiekopie uitvoeren:

##### <a name="1-deprovision"></a>1. Identiteitsgegevens
Gebruik de opdracht ' sudo waagent-deprovision + user ' om specifieke gegevens van virtuele Machine exemplaar te verwijderen. Zie voor [virtuele Linux-Machine vastleggen](capture-image-classic.md) gebruiken als sjabloon meer details op het vastleggen van installatiekopie.

##### <a name="2-shut-down-the-vm"></a>2: de virtuele machine afsluiten
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling voor het afsluiten van de opdrachtbalk onder.

##### <a name="3-capture-the-image"></a>3: de installatiekopie vastleggen
Zorg ervoor dat de virtuele machine is geselecteerd en klik op de koppeling voor het VASTLEGGEN van de balk onderaan opdracht. In het volgende scherm, geeft de naam van een INSTALLATIEKOPIE (bijvoorbeeld hk-cas-2-08-ub-14-04-2014071) juiste beschrijving van afbeelding en klik op vinkje de ' ' voor het voltooien van het vastleggen.

Dit proces duurt een paar seconden en de installatiekopie moet beschikbaar zijn in de sectie Mijn afbeeldingen van de installatiekopie-galerie. De bron-VM wordt automatisch verwijderd nadat de installatiekopie wordt vastgelegd. 

## <a name="single-region-deployment-process"></a>Implementatieproces voor één regio
**Stap 1: Het virtuele netwerk maken** Meld u aan bij de Azure-portal en een virtueel netwerk (klassiek) maken met de kenmerken in de volgende tabel weergegeven. Zie [een virtueel netwerk (klassiek) met de Azure portal maken](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>De naam van de VM-kenmerk</th><th>Waarde</th><th>Opmerkingen</th></tr>
<tr><td>Naam</td><td>vnet-cass-west-us</td><td></td></tr>
<tr><td>Regio</td><td>VS - west</td><td></td></tr>
<tr><td>DNS-servers</td><td>None</td><td>Negeer deze melding als er niet met behulp van een DNS-Server</td></tr>
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

Gegevens en Web subnetten kunnen worden beveiligd via netwerkbeveiligingsgroepen de dekking van die buiten het bereik van dit artikel is.  

**Stap 2: Inrichten van virtuele Machines** met behulp van de installatiekopie van het eerder hebt gemaakt, u de volgende virtuele machines in de cloud-server 'hk-c-svc-west' maken en koppelen aan de respectieve subnetten zoals hieronder wordt weergegeven:

<table>
<tr><th>Machinenaam    </th><th>Subnet    </th><th>IP-adres    </th><th>Beschikbaarheidsset</th><th>DC/Rack</th><th>Seed?</th></tr>
<tr><td>hk-c1-west-us    </td><td>gegevens    </td><td>10.1.2.4    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack1 = </td><td>Ja</td></tr>
<tr><td>hk-c2-west-us    </td><td>gegevens    </td><td>10.1.2.5    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack1 =    </td><td>Nee </td></tr>
<tr><td>hk-c3-west-us    </td><td>gegevens    </td><td>10.1.2.6    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack2 =    </td><td>Ja</td></tr>
<tr><td>hk-c4-west-us    </td><td>gegevens    </td><td>10.1.2.7    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack rack2 =    </td><td>Nee </td></tr>
<tr><td>hk-c5-west-us    </td><td>gegevens    </td><td>10.1.2.8    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack3 =    </td><td>Ja</td></tr>
<tr><td>hk-c6-west-us    </td><td>gegevens    </td><td>10.1.2.9    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack3 =    </td><td>Nee </td></tr>
<tr><td>hk-c7-west-us    </td><td>gegevens    </td><td>10.1.2.10    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack4 =    </td><td>Ja</td></tr>
<tr><td>hk-c8-west-us    </td><td>gegevens    </td><td>10.1.2.11    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack rack4 =    </td><td>Nee </td></tr>
<tr><td>hk-w1-west-us    </td><td>web    </td><td>10.1.1.4    </td><td>hk-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
<tr><td>hk-w2-west-us    </td><td>web    </td><td>10.1.1.5    </td><td>hk-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
</table>

Maken van de bovenstaande lijst met virtuele machines moeten het volgende proces:

1. Een lege cloudservice maken in een bepaald gebied
2. Een virtuele machine uit de eerder vastgelegde installatiekopie maken en deze te koppelen aan het virtuele netwerk gemaakt eerder; Herhaal deze stap voor alle virtuele machines
3. Een interne load balancer toevoegen aan de cloudservice en deze te koppelen aan het subnet "gegevens"
4. Voor elke virtuele machine die eerder is gemaakt, voegt u een eindpunt voor de taakverdeling voor thrift-verkeer via een set met gelijke taakverdeling is verbonden met de eerder gemaakte interne load balancer

Dit proces kan worden uitgevoerd met Azure portal; Gebruik een Windows-machine (gebruik een VM op Azure als u geen toegang tot een Windows-computer), gebruik de volgende PowerShell-script voor het automatisch inrichten van alle 8 VM's.

**Lijst met 1: PowerShell-script voor het inrichten van virtuele machines**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

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
        #Add add the thrift endpoint to the internal load balancer for all the VMs
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
* Bewerk cassandra.yaml seed-knooppunten te configureren zoals hieronder:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Stap 4: Start de virtuele machines en testen van het cluster**

Meld u aan bij een van de knooppunten (bijvoorbeeld hk-c1-west-us) en voer de volgende opdracht om de status van het cluster te bekijken:

       nodetool –h 10.1.2.4 –p 7199 status

Hier ziet u de weergave lijkt op de onderstaande voor een cluster met 8 knooppunten:

<table>
<tr><th>Status</th><th>Adres    </th><th>Belasting    </th><th>Tokens    </th><th>Eigenaar is van </th><th>Host-ID    </th><th>Rek</th></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (verwijderd)</td><td>rack1</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (verwijderd)</td><td>rack1</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd)</td><td>rack2</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd)</td><td>rack2</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd)</td><td>rack3</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd)</td><td>rack3</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd)</td><td>rack4</td></tr>
<tr><th>ONGEDAAN MAKEN    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (verwijderd)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testen van het Cluster één regio
Gebruik de volgende stappen voor het testen van het cluster:

1. Met behulp van de cmdlet van Powershell-opdracht Get-AzureInternalLoadbalancer, het IP-adres van de interne load balancer (bijvoorbeeld 10.1.2.101) verkrijgen. De syntaxis van de opdracht wordt hieronder weergegeven: Get-AzureLoadbalancer – ServiceName 'hk-c-svc-west-us' [ziet u de details van de interne load balancer samen met het IP-adres]
2. Meld u aan bij de webfarm VM (bijvoorbeeld hk-w1-west-us) met Putty of ssh
3. Execute $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4. Gebruik de volgende CQL-opdrachten om te controleren of het cluster werkt:
   
     Maak met KEYSPACE customers_ks met replicatie = {'class': 'SimpleStrategy', 'replication_factor': 3};   Gebruik customers_ks;   MAKEN van tabel Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   INVOEGEN in Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INVOEGEN in Customers(customer_id, firstname, lastname) waarden (2, 'ANS', 'De Vries');
   
     Selecteer * van klanten;

U ziet dat lijkt op de volgende resultaten:

<table>
  <tr><th> customer_id </th><th> Voornaam </th><th> lastname </th></tr>
  <tr><td> 1 </td><td> Jan </td><td> Jansen </td></tr>
  <tr><td> 2 </td><td> ANS </td><td> Jansen </td></tr>
</table>

De keyspace gemaakt in stap 4 gebruikt SimpleStrategy met een replication_factor van 3. SimpleStrategy wordt aanbevolen voor één data center-implementaties terwijl NetworkTopologyStrategy voor meerdere data center-implementaties. Een replication_factor van 3 geeft tolerantie voor knooppuntfouten.

## <a id="tworegion"></a>Meerdere landen/regio-implementatieproces
U gebruikmaken van de implementatie van één regio is voltooid en hetzelfde proces herhalen voor het installeren van de tweede regio. Het belangrijkste verschil tussen de implementatie van één of meerdere regio is de installatie van de VPN-tunnel voor communicatie tussen regio; met de netwerkinstallatie te starten, de virtuele machines inrichten en Cassandra configureren.

### <a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Stap 1: Het virtuele netwerk op de 2e regio maken
Meld u aan bij de Azure-portal en een virtueel netwerk maken met de kenmerken weergeven in de tabel. Zie [Cloud-Only virtueel netwerk configureren in de Azure portal](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor gedetailleerde stappen van het proces.      

<table>
<tr><th>Naam kenmerk    </th><th>Waarde    </th><th>Opmerkingen</th></tr>
<tr><td>Naam    </td><td>vnet-cass-east-us</td><td></td></tr>
<tr><td>Regio    </td><td>VS - oost</td><td></td></tr>
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
Een lokaal netwerk in Azure virtuele netwerken is een proxy-adresruimte die is toegewezen aan een externe site, met inbegrip van een privécloud of een andere Azure-regio. Deze adresruimte proxy is gebonden aan een externe gateway voor routering netwerk naar de juiste bestemmingen netwerken. Zie [een VNet-naar-VNet-verbinding configureren](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) voor de instructies voor het VNET-naar-VNET-verbinding tot stand brengen.

Maak twee lokale netwerken per de volgende details:

| Netwerknaam | VPN-Gateway-adres | Adresruimte | Opmerkingen |
| --- | --- | --- | --- |
| hk-lnet-map-to-east-us |23.1.1.1 |10.2.0.0/16 |Geef een tijdelijke aanduiding gatewayadres tijdens het maken van het lokale netwerk. Het echte gatewayadres wordt ingevuld zodra de gateway is gemaakt. Zorg ervoor dat de adresruimte die exact overeenkomt met de desbetreffende externe VNET; in dit geval wordt het VNET gemaakt in de regio VS-Oost. |
| hk-lnet-map-to-west-us |23.2.2.2 |10.1.0.0/16 |Geef een tijdelijke aanduiding gatewayadres tijdens het maken van het lokale netwerk. Het echte gatewayadres wordt ingevuld zodra de gateway is gemaakt. Zorg ervoor dat de adresruimte die exact overeenkomt met de desbetreffende externe VNET; in dit geval wordt het VNET gemaakt in de regio VS-West. |

### <a name="step-3-map-local-network-to-the-respective-vnets"></a>Stap 3: "Local" netwerkverbinding naar de respectieve VNETs
Selecteer elke vnet vanuit de Azure-portal, klik op 'Configureren', controleren 'Verbinding maken met het lokale netwerk' en selecteert u de lokale netwerken per de volgende details:

| Virtueel netwerk | Lokale netwerk |
| --- | --- |
| hk-vnet-west-us |hk-lnet-map-to-east-us |
| hk-vnet-east-us |hk-lnet-map-to-west-us |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Stap 4: Gateways op VNET1 en VNET2 maken
Klik op GATEWAY maken voor het activeren van de VPN-gateway inrichtingsproces vanuit het dashboard van de virtuele netwerken. Het dashboard van elke virtuele netwerk moet na een paar minuten het werkelijke gateway-adres worden weergegeven.

### <a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Stap 5: Update "Local" netwerken met de respectieve ' ' gatewayadressen
De lokale netwerken ter vervanging van de IP-adres van de tijdelijke aanduiding voor gateway met het echte IP-adres van de zojuist ingerichte gateways bewerken. Gebruik de volgende toewijzing:

<table>
<tr><th>Lokale netwerk    </th><th>Gateway voor een virtueel netwerk</th></tr>
<tr><td>hk-lnet-map-to-east-us </td><td>De gateway van hk-vnet-west-ons</td></tr>
<tr><td>hk-lnet-map-to-west-us </td><td>De gateway van hk-vnet-Oost-ons</td></tr>
</table>

### <a name="step-6-update-the-shared-key"></a>Stap 6: De gedeelde sleutel bijwerken
De volgende Powershell-script gebruiken om te werken van de IPSec-sleutel van elke VPN-gateway [Gebruik de sleutel mogelijk te houden voor beide gateways]: Set-AzureVNetGatewayKey - VNetName hk-vnet-Oost-ons - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-west-ons - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

### <a name="step-7-establish-the-vnet-to-vnet-connection"></a>Stap 7: Maken van de VNET-naar-VNET-verbinding
Gebruik het menu 'DASHBOARD' van de virtuele netwerken gateway-naar-gateway-verbinding tot stand brengen van de Azure-portal. De menuopdrachten 'Verbinding maken' in de onderste werkbalk gebruiken. Na een paar minuten moet het dashboard de verbindingsdetails grafisch weergeven.

### <a name="step-8-create-the-virtual-machines-in-region-2"></a>Stap 8: Maak de virtuele machines in regio #2
De installatiekopie Ubuntu maken zoals beschreven in de regio #1 implementatie door de dezelfde stappen of kopieer de VHD-bestand van de installatiekopie naar de Azure storage-account zich in regio #2 te volgen en maken van de installatiekopie. Deze afbeelding niet gebruiken en de volgende lijst met virtuele machines maken in een nieuwe cloudservice hk-c-svc-Oost-ons:

| Machinenaam | Subnet | IP-adres | Beschikbaarheidsset | DC/Rack | Seed? |
| --- | --- | --- | --- | --- | --- |
| hk-c1-east-us |gegevens |10.2.2.4 |hk-c-aset-1 |DC = EASTUS rack rack1 = |Ja |
| hk-c2-east-us |gegevens |10.2.2.5 |hk-c-aset-1 |DC = EASTUS rack rack1 = |Nee |
| hk-c3-east-us |gegevens |10.2.2.6 |hk-c-aset-1 |DC = EASTUS rack rack2 = |Ja |
| hk-c5-east-us |gegevens |10.2.2.8 |hk-c-aset-2 |DC = EASTUS rack rack3 = |Ja |
| hk-c6-east-us |gegevens |10.2.2.9 |hk-c-aset-2 |DC = EASTUS rack rack3 = |Nee |
| hk-c7-east-us |gegevens |10.2.2.10 |hk-c-aset-2 |DC = EASTUS rack rack4 = |Ja |
| hk-c8-east-us |gegevens |10.2.2.11 |hk-c-aset-2 |DC = EASTUS rack rack4 = |Nee |
| hk-w1-east-us |web |10.2.1.4 |hk-w-aset-1 |N/A |N/A |
| hk-w2-east-us |web |10.2.1.5 |hk-w-aset-1 |N/A |N/A |

Volg de instructies van regio #1, maar gebruik 10.2.xxx.xxx adresruimte.

### <a name="step-9-configure-cassandra-on-each-vm"></a>Stap 9: Cassandra op elke virtuele machine configureren
Meld u aan bij de virtuele machine en voer het volgende:

1. Bewerken $CASS_HOME/conf/cassandra-rackdc.properties om de data center en rack-eigenschappen opgeven in notatie: dc = EASTUS rack rack1 =
2. Cassandra.yaml voor het configureren van de seed-knooppunten te bewerken: zaden: '10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10'

### <a name="step-10-start-cassandra"></a>Stap 10: Cassandra starten
Meld u aan bij elke virtuele machine en Cassandra op de achtergrond te starten met de volgende opdracht: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Testen van het Cluster meerdere landen/regio
Nu is Cassandra met 16 knooppunten met 8 knooppunten in elke Azure-regio geïmplementeerd. Deze knooppunten zich in hetzelfde cluster doordat de algemene clusternaam en de configuratie van de seed-knooppunt. Gebruik het volgende proces voor het testen van het cluster:

### <a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Stap 1: Haal de interne load balancer-IP voor zowel de regio's met behulp van PowerShell
* Get-AzureInternalLoadbalancer -ServiceName "hk-c-svc-west-us"
* Get-AzureInternalLoadbalancer -ServiceName "hk-c-svc-east-us"  
  
    Noteer de IP-adressen (voor voorbeeld west - 10.1.2.101, Oost - 10.2.2.101) weergegeven.

### <a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Stap 2: Het volgende in de regio west uitvoeren nadat de aanmelding bij hk-w1-west-ons
1. Execute $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2. Voer de volgende CQL-opdrachten:
   
     Maak met KEYSPACE customers_ks met replicatie = {'class': 'NetworkToplogyStrategy', 'WESTUS': 3, EASTUS: 3};   Gebruik customers_ks;   MAKEN van tabel Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   INVOEGEN in Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INVOEGEN in Customers(customer_id, firstname, lastname) waarden (2, 'ANS', 'De Vries');   Selecteer * van klanten;

U ziet een scherm wordt weergegeven zoals hieronder:

| customer_id | Voornaam | Lastname |
| --- | --- | --- |
| 1 |Jan |Jansen |
| 2 |ANS |Jansen |

### <a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Stap 3: Voer het volgende in de regio Oost na de aanmelding bij hk-w1-Oost-ons:
1. Execute $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2. Voer de volgende CQL-opdrachten:
   
     Gebruik customers_ks;   MAKEN van tabel Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   INVOEGEN in Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INVOEGEN in Customers(customer_id, firstname, lastname) waarden (2, 'ANS', 'De Vries');   Selecteer * van klanten;

U ziet de dezelfde weergave zoals te zien voor de regio West:

| customer_id | Voornaam | Lastname |
| --- | --- | --- |
| 1 |Jan |Jansen |
| 2 |ANS |Jansen |

Enkele meer invoegingen uitvoeren en Zie dat die worden gerepliceerd naar west-ons deel uit van het cluster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Cassandra Testcluster met Node.js
Met een van de virtuele Linux-machines eerder hebt gemaakt in de laag 'web', uitvoeren u een eenvoudige Node.js-script om te lezen van de eerder ingevoegde gegevens

**Stap 1: Node.js en Cassandra-Client installeren**

1. Installeer Node.js en npm
2. Knooppunt pakket 'cassandra-client' installeren met npm
3. Voer het volgende script op de shell-prompt de json-tekenreeks van de opgehaalde gegevens worden weergegeven:
   
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
Microsoft Azure is een flexibel platform waarmee het uitvoeren van zowel Microsoft als open-sourcesoftware zoals blijkt uit deze oefening. Maximaal beschikbare Cassandra clusters kunnen worden geïmplementeerd op een enkele Datacenter via het spreiden van de clusterknooppunten over meerdere domeinen met fouten. Cassandra clusters kunnen ook worden geïmplementeerd in meerdere geografisch verafgelegen Azure-regio's voor noodherstel bewijs systemen. Azure en Cassandra samen kunnen de constructie van zeer schaalbaar, maximaal beschikbare en na noodgevallen herstelbare cloudservices die nodig is door de hedendaagse internet schalen services.  

## <a name="references"></a>Verwijzingen
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)

