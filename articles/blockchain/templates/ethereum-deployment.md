---
title: Ethereum bewijs van werk consortium-oplossingssjabloon
description: De oplossingssjabloon Ethereum bewijs van werk Consortium gebruiken om te implementeren en configureren van een consortium voor meerdere leden Ethereum-netwerk
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/28/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: 6530e86f59124e324953e4ca082c247ebec3bc91
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650917"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Ethereum bewijs van werk consortium-oplossingssjabloon

De oplossingssjabloon Ethereum bewijs van werk Consortium is ontworpen voor u gemakkelijker en sneller te implementeren en configureren van een consortium voor meerdere leden Ethereum-netwerk met minimale kennis van Azure en Ethereum.

Met behulp van een Azure Resource Manager-sjabloon, kunt elk lid inrichten netwerk, met behulp van Microsoft Azure Compute, netwerken en opslagservices. Netwerk-voetafdruk van elk lid bestaat uit een set met load balancing transactie knooppunten met die een toepassing of de gebruiker communiceert om in te dienen transacties, een set knooppunten van de analysestructuur voor vastleggen van transacties en een VPN-gateway. Na de implementatie verbindt u de gateways voor het maken van een volledig geconfigureerde meerdere leden blockchain-netwerk.

## <a name="about-blockchain"></a>Over blockchains

De release van deze oplossing is bedoeld voor degenen die niet bekend bent met de blockchain-community een uitstekende gelegenheid om te leren over de technologie op een manier eenvoudig en kunnen worden geconfigureerd in Azure. Als u wilt beginnen, raden we echter aan de eenvoudiger zelfstandige Ethereum--topologie met deze aanwijzingen, voor het bouwen van netwerken met meerdere leden consortium implementeren.

### <a name="mining-node-details"></a>Knooppuntdetails voor gegevensanalyse

Knooppunten die mijn transacties worden gescheiden van de knooppunten die transacties om ervoor te zorgen dat de twee acties niet elkaar om dezelfde bronnen beconcurreren accepteren.

Lid van een consortium kan maximaal vijf regio's met een of meer analysestructuur knooppunten, ondersteund door een beheerde schijf worden ingericht. Een of meer knooppunten in de regio zijn geconfigureerd als een opstartknooppunt voor de ondersteuning van dynamische zichtbaarheid van de knooppunten in het netwerk. Analysestructuur knooppunten communiceren met andere knooppunten van de analysestructuur tot consensus bent gekomen over de status van het onderliggende gedistribueerd grootboek. Er is niet nodig voor uw toepassing zich bewust zijn van of communiceren met deze knooppunten. De analysestructuur-knooppunten zijn door te focussen op particuliere netwerken, geïsoleerd van inkomend verkeer in de openbare internet op een tweede beveiligingslaag toevoegen. Uitgaand verkeer is toegestaan, maar niet voor de poort Ethereum-detectie.

Alle knooppunten hebben een stabiele versie van de client gaat Ethereum (Geth) en zijn geconfigureerd als knooppunten van de analysestructuur. Als u een aangepaste genesis blok niet opgeeft hebt, gebruikt u alle knooppunten dezelfde Ethereum-adres en -sleutelpaar dat wordt beveiligd door het accountwachtwoord Ethereum. De door u opgegeven wachtwoordzin voor het Ethereum wordt gebruikt voor het genereren van het standaardaccount (coinbase) voor elk knooppunt van de analysestructuur. Als de knooppunten van de analysestructuur, Mijn, verzamelen van kosten die zijn toegevoegd aan dit account.

Het aantal knooppunten van de analysestructuur per consortium lid, is afhankelijk van de totale grootte van het gewenste netwerk en de hoeveelheid hash power dat is toegewezen aan elk lid. Grotere netwerken vereist meer knooppunten om te krijgen van een oneerlijk voordeel is. De sjabloon biedt ondersteuning voor maximaal 15 knooppunten van de analysestructuur per regio die zijn ingericht met behulp van schaalsets voor virtuele machines.

### <a name="transaction-node-details"></a>Knooppunt-transactiedetails

Lid van een consortium heeft ook een set met load balancing transactie-knooppunten. Deze knooppunten zijn bereikbaar is vanaf buiten het virtuele netwerk, zodat toepassingen deze knooppunten gebruiken kunnen transacties indienen of in slimme contracten binnen de blockchain-netwerk worden uitgevoerd. Alle knooppunten hebben een stabiele versie van de client gaat Ethereum (Geth) en zijn geconfigureerd voor het onderhouden van een volledige kopie van het gedistribueerde grootboek. Als een aangepaste genesis blok niet opgegeven is, wordt in deze knooppunten de dezelfde Ethereum-account, beveiligd door het accountwachtwoord Ethereum gebruiken.

Transactie-knooppunten zijn taakverdeling binnen een beschikbaarheidsset om hoge beschikbaarheid te houden. De sjabloon biedt ondersteuning voor maximaal vijf transactie knooppunten die zijn ingericht met behulp van schaalsets voor virtuele machines.

### <a name="azure-monitor-logs-details"></a>Details van Azure Monitor-Logboeken

Elke implementatie maakt een nieuw exemplaar van Azure Monitor Logboeken ook of kan deelnemen aan een bestaand exemplaar. Logboeken in Azure Monitor kunt u het bewaken van verschillende maatstaven voor prestaties van elke virtuele machine die het geïmplementeerde netwerk vormt.

## <a name="deployment-architecture"></a>Implementatie-architectuur

### <a name="description"></a>Description

Deze oplossingssjabloon kan één of meerdere items regio op basis van meerdere lid Ethereum consortium network implementeren. Het virtuele netwerk van elke regio is verbonden met andere regio's in een keten-topologie met behulp van de VNET-gateways en verbindingsresources. Het product biedt ook een registrar, waarin de vereiste gegevens van alle minder en transactie knooppunten geïmplementeerd in elke regio.

### <a name="standalone-and-consortium-leader-overview"></a>Zelfstandige en consortium leider-overzicht

![Zelfstandige en Consortium leider-overzicht](./media/ethereum-deployment/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Overzicht van consortium lid toevoegen

![Overzicht van Consortium lid toevoegen](./media/ethereum-deployment/member-overview.png)

## <a name="getting-started"></a>Aan de slag

Dit proces vereist een Azure-abonnement dat u kunt ondersteuning voor de implementatie van meerdere schaalsets voor virtuele machines en beheerde schijven. Indien nodig, maakt u een gratis Azure-account om te beginnen.

Nadat een abonnement is beveiligd, gaat u naar Azure portal. Selecteer **+ een resource maken**, Marketplace (Zie alle), en zoek naar de **Ethereum bewijs van werk Consortium**.

De sjabloonimplementatie begeleidt u bij het configureren van het eerste lid footprint in het netwerk. De stroom van de implementatie is onderverdeeld in vijf stappen: Basisbeginselen, Operations Management Suite, regio's voor implementatie, netwerk-grootte en prestaties, Ethereum-instellingen.

### <a name="basics"></a>Basisbeginselen

Onder **basisbeginselen**, waarden opgeven voor standard parameters voor elke implementatie, zoals abonnement, resourcegroep en virtuele-eigenschappen.

![Basisbeginselen](./media/ethereum-deployment/sample-deployment.png)

Parameternaam|Description| Toegestane waarden|Standaardwaarden
---|---|---|---
Maak een nieuwe netwerk- of toevoegen aan bestaande netwerk?|Een nieuw netwerk maken of lid van een bestaande consortium network|Nieuwe Join bestaande maken|Create New
Implementeren van een netwerk die deel van een consortium uitmaken?|Een netwerk consortium kunt toekomstige implementaties voor deelname aan dit netwerk (zichtbaar wanneer *nieuw* hierboven is geselecteerd)|Zelfstandige Consortium|Zelfstandig
Voorvoegsel van de resource |De tekenreeks die wordt gebruikt als basis voor de naamgeving van resources (2 tot 4 alfanumerieke tekens). Een unieke hash wordt voorafgegaan aan de tekenreeks voor sommige resources, terwijl de resource-specifieke informatie wordt toegevoegd.|Alfanumerieke tekens met lengte 2 tot 4|N.v.t.
VM-gebruikersnaam| Gebruikersnaam voor de beheerder van elke geïmplementeerde VM (alleen alfanumerieke tekens)|1 - 64 tekens |gethadmin
Verificatietype|De methode voor verificatie bij de virtuele machine. |Wachtwoord of SSH de openbare sleutel|Wachtwoord
Wachtwoord (verificatietype = wachtwoord)|Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd. Het wachtwoord moet bestaan 3 van de volgende vereisten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. <br />Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|N.v.t.
SSH-sleutel (verificatietype = openbare sleutel)|De veilige shell-sleutel die wordt gebruikt voor externe aanmelding.|| N.v.t.
Abonnement| Het abonnement waaraan het implementeren van het netwerk consortium||N.v.t.
Resourcegroep| De resourcegroep waaraan de consortium network implementeren.||N.v.t.
Locatie| De Azure-regio voor de resourcegroep. ||N.v.t.

### <a name="operations-management-suite"></a>Operations Management Suite

Operations Management Suite (OMS) kunt u een OMS-resource voor het netwerk configureren. Met OMS wordt verzameld en surface nuttig metrische gegevens en logboeken van uw netwerk, biedt de mogelijkheid om snel te controleren de netwerkstatus of foutopsporing problemen. Het gratis aanbod van OMS wordt zonder problemen mislukken als de capaciteit is bereikt.

![Het maken van nieuwe OMS](./media/ethereum-deployment/new-oms.png)

Parameternaam|Description| Toegestane waarden|Standaardwaarden
---|---|---|---
Verbinding maken met bestaande OMS|Maak een nieuw exemplaar van Azure Monitor Logboeken of Word lid van een bestaand exemplaar|Nieuwe Join bestaande maken|Nieuwe locatie van de Azure Monitor logboeken maken|De regio waarin de nieuwe Azure Monitor-logboeken worden geïmplementeerd (weergegeven indien *nieuw* is geselecteerd)
Bestaande OMS-werkruimte-Id|Werkruimte-ID van het bestaande exemplaar (weergegeven indien *deelnemen aan bestaande* is geselecteerd) categorie OMS-Service|Kies de prijscategorie voor het nieuwe exemplaar. Meer informatie op https://azure.microsoft.com/pricing/details/log-analytics/ (weergegeven indien *deelnemen aan bestaande* is geselecteerd)|Gratis zelfstandige Per knooppunt|Gratis
Bestaande OMS-primaire sleutel|De primaire sleutel gebruikt voor verbinding met de bestaande OMS-instantie (weergegeven indien *deelnemen aan bestaande* is geselecteerd)

### <a name="deployment-regions"></a>Implementatie-regio 's

Vervolgens onder **implementatie regio's**, geef de invoer voor **aantal regio('s)** implementeren de consortium network en de selectie van Azure-regio's op basis van het aantal regio's die zijn opgegeven. Gebruiker kunt implementeren in maximaal vijf regio's.

![Implementatie-regio 's](./media/ethereum-deployment/deployment-regions.png)

Parameternaam| Description| Toegestane waarden |Standaardwaarden
---|---|---|---
Aantal regio('s)| Aantal regio's om de consortium-netwerk te implementeren|1, 2, 3, 4, 5| 2
Eerste regio| Eerste regio voor de implementatie van het netwerk consortium|Alle toegestaan Azure-regio 's| US - west
Tweede regio |Tweede regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 2)|Alle toegestaan Azure-regio 's| US - oost
Derde regio| Derde regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 3)|Alle toegestaan Azure-regio 's| US - centraal
Vierde regio| Vierde regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 4)|Alle toegestaan Azure-regio 's| US - oost 2
Vijfde regio| Vijfde regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 5)|Alle toegestaan Azure-regio 's| US - west 2

### <a name="network-size-and-performance"></a>Netwerkgrootte en prestaties

Vervolgens onder **grootte en prestaties van het netwerk** invoer voor de grootte van het netwerk consortium opgeven. Zoals aantal en de grootte van de analysestructuur knooppunten en transactie-knooppunten.

![Netwerkgrootte en prestaties](./media/ethereum-deployment/network-size-performance.png)

Parameternaam |Description |Toegestane waarden| Standaardwaarden
---|---|---|---
Aantal knooppunten voor gegevensanalyse|Het aantal analysestructuur knooppunten geïmplementeerd per regio|2 - 15| 2
Analysestructuur knooppunt opslagprestaties|Het type beheerde schijf een back-up elk van de geïmplementeerde gegevensanalyse-knooppunten.|Standard- of Premium|Standard
De grootte van de virtuele machine knooppunt gegevensanalyse|De grootte van de virtuele machine wordt gebruikt voor knooppunten van de analysestructuur.|Standard A <br />Standard D <br />Standard D-v2 <br />Standard F-serie <br />Standard DS <br />en Standard FS|Standard D1v2
Het aantal knooppunten met gelijke transactie|Het aantal knooppunten van de transactie om in te richten als onderdeel van het netwerk.|1 - 5| 2
Transactie-knooppunt opslagprestaties|Het type beheerde schijf een back-up van de gedistribueerde transactie-knooppunten.|Standard- of Premium|Standard
De grootte van de virtuele machine knooppunt transactie|De grootte van de virtuele machine wordt gebruikt voor transactie-knooppunten.|Standard A <br />Standard D <br />Standard D-v2 <br />Standard F-serie <br />Standard DS <br />en Standard FS|Standard D1v2

### <a name="ethereum-settings"></a>Ethereum-instellingen

Vervolgens onder **Ethereum-instellingen**, Ethereum-gerelateerde configuratie-instellingen, zoals de netwerk-ID en Ethereum-account wachtwoord of genesis blok opgeven.

![Ethereum-instellingen](./media/ethereum-deployment/standalone-leader-deployment.png)

Parameternaam |Description |Toegestane waarden|Standaardwaarden
---|---|---|---
ConsortiumMember-ID|De ID die is gekoppeld aan elk lid van die deel uitmaken van het consortium netwerk gebruikt voor het configureren van IP-adresruimten om te voorkomen van conflicten. <br /><br />Lid-ID moet uniek zijn in verschillende organisaties in hetzelfde netwerk. Een unieke lid-ID is vereist, zelfs wanneer dezelfde organisatie wordt geïmplementeerd in meerdere regio's.<br /><br />Noteer de waarde van deze parameter omdat moet u deze delen met andere leden van de lid te worden.|0 - 255
Ethereum Network ID|De netwerk-ID voor het consortium Ethereum-netwerk worden geïmplementeerd. Elk Ethereum-netwerk heeft een eigen netwerk-ID, met 1 wordt de ID voor het openbare netwerk. Toegang tot het netwerk is niet toegestaan voor de analysestructuur knooppunten, raden wij nog steeds gebruik van een groot aantal om te voorkomen van conflicten.|5 - 999,999,999| 10101010
Aangepaste genesis blokkeren|Optie voor het automatisch genereren van een blok genesis of geef een aangepaste classificatie.|Ja/Nee| Nee
Ethereum-accountwachtwoord (aangepaste genesis blok = Nee)|Het administrator-wachtwoord gebruikt om de Ethereum-account dat is geïmporteerd in elk knooppunt te beveiligen. Het wachtwoord moet bevatten: 1 hoofdletter, 1 kleine letter en 1 cijfer.|12 of meer tekens|N.v.t.
Ethereum-wachtwoordzin voor persoonlijke sleutel (aangepaste genesis blok = Nee)|De wachtwoordzin die wordt gebruikt voor het genereren van de persoonlijke sleutel van ECC die zijn gekoppeld aan de standaard Ethereum-account die wordt gegenereerd. Een vooraf gegenereerde persoonlijke sleutel hoeft niet expliciet worden doorgegeven.<br /><br />Houd rekening met een wachtwoordzin met voldoende aanvraaggrootte om ervoor te zorgen voor een sterke persoonlijke sleutel en zonder overlap met andere leden consortium. De wachtwoordzin moet ten minste bevatten: 1 hoofdletter, 1 kleine letter en 1 cijfer.<br /><br />Houd er rekening mee als twee leden gebruikmaken van dezelfde wachtwoordzin de accounts die gegenereerd zijn hetzelfde. Dezelfde wachtwoordzin is handig als één organisatie probeert te implementeren in regio's en wil delen van één account (munt basis) voor alle knooppunten.|12 of meer tekens|N.v.t.
Genesis blok (blok van aangepaste genesis = Yes)|JSON-tekenreeks voor aangepaste genesis blokkeren. U vindt meer informatie over de indeling van het blok genesis hier, aangepaste netwerken.<br /><br />Een Ethereum-account is nog steeds gemaakt bij het opgeven van een aangepaste genesis-blok. Houd rekening met het instellen van een prefunded Ethereum-account in het blok genesis niet na afloop van de analysestructuur.|Geldige JSON |N.v.t.
Gedeelde sleutel voor de verbinding|Een gedeelde sleutel voor de verbinding tussen VNET-gateways.| 12 of meer tekens|N.v.t.
Consortium gegevens-URL|De URL die verwijst naar de relevante consortium configuratiegegevens worden geleverd door een ander lid van de implementatie. <br /><br />Deze informatie wordt verstrekt door een lid is al verbonden met een implementatie. Als u de rest van het netwerk hebt geïmplementeerd, is de URL van de sjabloonuitvoer implementatie, met de naam CONSORTIUM-gegevens.||N.v.t.
VNet-Gateway verbinding maken met|Het resourcepad van de VNet-Gateway waarmee u verbinding wilt.<br />Deze informatie wordt verstrekt door een lid is al verbonden met een implementatie. Als u de rest van het netwerk hebt geïmplementeerd, wordt de URL in de uitvoer van implementatie van de sjabloon, met de naam CONSORTIUM_MEMBER_GATEWAY_ID is. Opmerking: Hetzelfde lid consortium gegevens-URL en VNet-Gateway resource moet worden gebruikt.||N.v.t.
Eindpunt van de registrar voor Peer-informatie|Peer-info eindpunt dat is opgegeven door de implementatie van een ander lid|Ongeldig eindpunt van het eerste lid in consortium|N.v.t.
Sleutel van de Peer informatie registrar|Peer-info primaire sleutel geleverd door een ander lid van de implementatie|Geldige primaire sleutel van het eerste lid in consortium|N.v.t.

### <a name="summary"></a>Samenvatting

Klik in het overzicht bekijken van de invoer die is opgegeven en uit te voeren basisvalidatie vóór de implementatie.

![Samenvatting](./media/ethereum-deployment/summary.png)

Wettelijke informatie en privacy voorwaarden bekijken en op **aankoop** te implementeren. Als de implementatie meer dan één regio heeft of voor een netwerk consortium is, klikt u vervolgens deze sjabloon implementeert vooraf de VPN-gateways die nodig zijn ter ondersteuning van verbinding met het netwerk met andere leden. Implementatie van de gateway kan tot 45 tot 50 minuten duren.

## <a name="post-deployment-sanity-checks"></a>Bevestigingen voor post-implementatie

### <a name="administrator-page"></a>Administrator-pagina

Zodra de implementatie is voltooid en alle resources zijn ingericht, gaat u naar de beheerder voor een overzicht van uw blockchain-netwerk en sanity de implementatiestatus controleren. De URL van de beheerpagina is de DNS-naam van de load balancer. het is aanwezig in de sectie uitvoer van de sjabloonimplementatie met de naam als ADMIN_SITE.

Om te zoeken, selecteert u de resourcegroep die is geïmplementeerd. Selecteer **overzicht**, en klik op de koppeling onder **implementaties** die bevat het getal dat is voltooid.

![Overzicht van resource](./media/ethereum-deployment/resource-overview.png)

Het nieuwe scherm ziet u de implementatiegeschiedenis van de. Selecteer de eerste implementatie-resource (bijvoorbeeld microsoft-azure-blockchain.azure-blockchain-servi...) en zoek naar de **uitvoer** sectie in de onderste helft van de pagina. Hier ziet u de URL voor de beheerpagina die worden vermeld in de sjabloon implementatie output-parameter als ADMIN_SITE.

![Resource-implementaties](./media/ethereum-deployment/resource-deployments.png)

![Implementatie-uitvoer](./media/ethereum-deployment/deployment-output.png)

Kopieer het volgende om door te gaan naar de beheerpagina, de **ADMIN-SITE** uitvoer en opent u het in een ander tabblad.

Op de beheerpagina krijgt u een overzicht op hoog niveau van de topologie die u aan de hand van de sectie Ethereum-Knooppuntstatus hebt geïmplementeerd. Deze bevat alle knooppunt hostnamen, het aantal peer en het laatste blok gezien. Het aantal peer voor elk knooppunt is tussen het minimum van een kleiner is dan de *totaal aantal knooppunten* en het aantal geconfigureerde maximale peer. Houd er rekening mee dat het aantal peer, niet het aantal knooppunten dat kan worden geïmplementeerd in het netwerk beperken.
Van tijd tot tijd, ziet u het aantal peer variëren en kleiner zijn dan de (totaal aantal knooppunten: 1). Het verschil in de telling is niet altijd een teken dat de knooppunten niet in orde, zijn omdat het hoofddomein in het grootboek kunnen leiden tot kleine wijzigingen in peer aantal. Ten slotte kunt u het laatste blok zichtbaar voor elk knooppunt in het netwerk om te bepalen splitsingen of lag in het systeem controleren.

![Knooppuntstatus](./media/ethereum-deployment/node-status.png)

Status van het knooppunt wordt elke tien seconden vernieuwd. Laad de pagina via de browser of **opnieuw laden** om de weergave bijwerken.

### <a name="oms-portal"></a>OMS-portal

Uw OMS-portal kunt u vinden door de koppeling in de implementatie-uitvoer (OMSPORTALURL) of door de OMS-resource in de geïmplementeerde resourcegroep selecteren.

![OMS URL](./media/ethereum-deployment/oms-url.png)

![OMS-resource](./media/ethereum-deployment/oms-resource.png)

De portal wordt eerst op hoog niveau worden verzameld als een overzicht weergegeven.

![Overzicht van OMS](./media/ethereum-deployment/oms-overview.png)

Op het overzicht klikt, wordt u doorgeleid naar een portal statistieken per knooppunt weergeven.

![De statistieken per knooppunt](./media/ethereum-deployment/per-node-stats.png)

### <a name="accessing-nodes"></a>Toegang tot knooppunten

U kunt op afstand verbinding maken met de virtuele machines voor de transactie-knooppunten via SSH met uw beheerder opgegeven gebruikersnaam en het wachtwoord/SSH-sleutel. Omdat de transactie-knooppunt VM's geen hun eigen openbare IP-adressen, moet u om te gaan via de load balancer en het poortnummer opgeven. De SSH-opdracht uit te voeren om toegang tot het eerste knooppunt van de transactie wordt vermeld in de sjabloon implementatie output-parameter als **SSH_TO_FIRST_TX_NODE** (voor de Voorbeeldimplementatie: ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Verhogen om door te gaan naar de knooppunten van extra transactielogboeken, het poortnummer dat door een (bijvoorbeeld het eerste knooppunt van de transactie is op poort 4000).

Aangezien de virtuele machines waarop de analysestructuur knooppunten uitgevoerd niet extern toegankelijk is, moet u een van de transactie-knooppunten. Zodra u een SSH-sessie op een knooppunt transactie hebt, uw persoonlijke sleutel installeren op het knooppunt transactie of uw wachtwoord gebruikt voor het starten van een SSH-sessie in een van de analysestructuur-knooppunten.

**Opmerking**

De hostnamen kunnen worden verkregen van beheer-Site of van de Azure-portal. In Azure portal, de hostnamen van de knooppunten die aanwezig zijn in de resource VM scale set (VMSS) wordt vermeld onder **exemplaren**, die verschilt van de werkelijke hostnamen. Bijvoorbeeld, de hostnaam in Azure portal eruit als **mn-asdfmv-reg1_0** maar de werkelijke hostnaam zou zijn, zoals **mn-asdfmv-reg**.

Bijvoorbeeld:

Hostnaam van Azure portal| Werkelijke hostnaam
---|---
mn-ethwvu-reg1_0| mn-ethwvu-reg1000000
mn-ethwvu-reg1_1 |mn-ethwvu-reg1000001
mn-ethwvu-reg1_2 |mn-ethwvu-reg1000002

## <a name="adding-a-new-consortium-member"></a>Een nieuwe consortium lid toevoegen

### <a name="sharing-data"></a>Delen van gegevens

Als het eerste lid (of een verbonden lid) van het consortium moet u andere leden van de bieden een aantal soorten informatie, zodat ze kunnen lid worden en de verbinding tot stand brengen. Specifiek:

1. **Gedeelde Consortium configuratiegegevens**: Er is een set gegevens die worden gebruikt voor het indelen van de Ethereum-verbinding tussen de twee leden. De nodige informatie, waaronder de genesis blokkeren, consortium netwerk-ID en -opstartknooppunten, is naar een bestand op de transactie-knooppunten van de leider of een ander lid van de geïmplementeerde geschreven. De locatie van dit bestand wordt vermeld in de sjabloon implementatie output-parameter met de naam **CONSORTIUM gegevens**.
2. **Peer-Info eindpunt**: Registrar eindpunt van de Peer-info voor informatie van alle knooppunten die al verbonden met het Ethereum-netwerk van de leiders of een ander lid van de implementatie. De DB slaat een verzameling van informatie met betrekking tot elk knooppunt is verbonden in het netwerk, informatie zoals de hostnaam van het knooppunt, privé-IP-adres, enzovoort. Dit is de sjabloon implementatie output-parameter met de naam **PEER_INFO_ENDPOINT**.
3. **Peer-primaire sleutel Info**: De Peer info registrar primaire sleutel wordt gebruikt voor toegang tot een van de leider of andere adreslid Peer info primaire sleutel. Dit is de sjabloon implementatie output-parameter met de naam **PEER_INFO_PRIMARY_KEY**.


4. **VNET-Gateway**: Elk lid van verbinding een met het hele blockchain-netwerk via een bestaand lid. Als u wilt verbinding maken met een VNET, moet u de resourcepad naar de VNET-Gateway van het lid waarmee u verbinding maakt. Dit is de sjabloon implementatie output-parameter met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
5. **Gedeelde sleutel**: Een vooraf vastgestelde geheim tussen de twee leden van het netwerk consortium die een verbinding tot stand brengt. Dit is een alfanumerieke tekenreeks (tussen 1 tot 128 tekens) die is overeengekomen buiten de context van de implementatie. (Bijvoorbeeld **MySharedKeyAbc123**)

### <a name="acceptance-of-new-member"></a>Acceptatie van nieuwe lid

Deze stap moet worden uitgevoerd nadat het lid te worden lid heeft het netwerk is geïmplementeerd. Voordat een lid kunt deelnemen aan het netwerk en transactie-verkeer ziet, moet een bestaand lid een definitieve configuratie uitvoeren op de VPN-Gateway om de verbinding te accepteren. Dit betekent dat de Ethereum-knooppunten van het gekoppelde lid wordt niet uitgevoerd totdat het een verbinding tot stand is gebracht. Deze configuratie kan worden gedaan via PowerShell of xPlat CLI. Een PowerShell-module en xPlat CLI-script worden ook opgeslagen op het knooppunt transactie, naast de consortium-gegevens. Locatie van het script is de implementatieparameters uitvoer met de naam **paar-GATEWAY-PS-MODULE** en **paar-GATEWAY-AZURE-CLISCRIPT**, respectievelijk.

**PowerShell/CLI instellen**

Meer informatie over hoe u aan de slag met Azure PowerShell-cmdlets en Azure xPlat CLI kan worden gevonden in Azure-documentatie.

U moet de meest recente versie van de Azure-cmdlets lokaal is geïnstalleerd en een sessie geopend. Zorg ervoor dat u zich aanmelden bij de sessie met de referenties van uw Azure-abonnement.

**PowerShell: Verbinding tot stand brengen**

De PowerShell-module downloaden en lokaal opslaat. De locatie van de PowerShell-module is opgegeven als de **paar-GATEWAY-PS-MODULE** sjabloonimplementatie uitvoerparameter.

Als u nog niet is ingeschakeld, gebruikt u de **Set-ExecutionPolicy** cmdlet voor de lokale sessie om toe te staan een niet-ondertekende module uitvoert.

**Set-ExecutionPolicy Unrestricted CurrentUser**

Vervolgens Meld u aan bij uw Azure-abonnement waarin u geïmplementeerd de leider implementatie met behulp van

**Login-AzureRmAccount**

Importeer vervolgens de module:

**Import-Module <filepath to downloaded file>**

Ten slotte de functie uitvoeren met de juiste invoer:

- **MyGatewayResourceId:** Het resourcepad van uw Gateway. Dit is de sjabloon implementatie output-parameter met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId:** Het resourcepad van het gekoppelde lid-gateway. Dit wordt geleverd door het gekoppelde lid en is de sjabloon output-parameter van de implementatie ook met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **Verbindingsnaam:** Een naam voor u deze Gateway-verbinding wilt aanduiden.
- **Gedeelde sleutel:** De vooraf ingestelde geheim tussen de twee leden van het netwerk consortium die een verbinding tot stand brengt.

**CreateConnection** -MyGatewayResourceId <resource path of your Gateway> - OtherGatewayResourceId < bronpad van het gekoppelde lid gateway > - ConnectionName mijnVerbinding - SharedKey "MySharedKeyAbc123"

**xPlat CLI: Verbinding tot stand brengen**

De Azure CLI-script downloaden en lokaal opslaat. De locatie van de Azure CLI-script is opgegeven in de implementatie sjabloonparameter met de naam **paar-GATEWAY-AZURE-CLI-SCRIPT**.

Voer het script met de juiste invoer:

- **MyGatewayResourceId:** Het resourcepad van uw Gateway. Dit is de sjabloon implementatie output-parameter met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId:** Het resourcepad van het gekoppelde lid-gateway. Dit wordt geleverd door het gekoppelde lid en parameter voor de implementatie van hun implementatie ook met de naam is **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **Verbindingsnaam:** Een naam voor u deze Gateway-verbinding wilt aanduiden.
- **Gedeelde sleutel:** De vooraf ingestelde geheim tussen de twee leden van het netwerk consortium die een verbinding tot stand brengt.
- **Locatie:** De Azure-regio waar uw gateway-resource is geïmplementeerd.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

De architectuur wordt als volgt worden nadat u hebt verbinding tussen geconfigureerd **leider** en **lid** implementaties.

![Architectuur van de leider en lid](./media/ethereum-deployment/leader-member.png)

## <a name="fund-new-member-account"></a>Nieuwe LEDENACCOUNT samenleving

### <a name="generated-genesis-block"></a>Gegenereerde genesis blokkeren

Als het eerste lid uw Ethereum-account met één biljoen ether is gefinancierd als de implementatie het blok genesis genereert (aangepaste Genesis blok = Nee). Andere leden heeft een account dat is niet vooraf gefinancierd en Ether verzamelen als de knooppunten van de analysestructuur begint met mijn blokken moet wachten. Om te voorkomen dat nieuwe leden Ether wacht, moet u expliciet samenleving lid te worden leden Ethereum-accounts.

Om dit te doen, moet het toevoegen van leden u voorzien van de Ethereum-account dat wordt weergegeven op de website van de beheerder. U kunt vervolgens de website van uw beheerder om over te dragen Ether uit uw account aan het account door te voeren van het account dat is opgegeven.

### <a name="custom-genesis-block"></a>Aangepaste genesis blokkeren

Als een aangepaste genesis blok wordt geleverd met een opgegeven Ethereum-account, kunt u MetaMask of een ander hulpprogramma om over te dragen ether uit dat account aan de vooraf gegenereerde zichtbaar in de beheerwebsite Ethereum-account opgegeven. Zie voor instructies over het gebruik van MetaMask [Ethereum-Account voor het maken van](#create-ethereum-account).

Als een aangepaste genesis blok wordt aangeboden zonder een account of u geen toegang tot een vooraf toegewezen accounts hebt, moet u wachten totdat de knooppunten van de analysestructuur begint met mijn voor het genereren van Ether bij uw account (munt basis). Hoe snel de fondsen worden gegenereerd, is afhankelijk van het niveau van de problemen dat u in het blok aangepaste genesis opgeven.

## <a name="create-ethereum-account"></a>Ethereum-account maken

Voor het maken van een extra account, kunt u tal van oplossingen. Een dergelijke oplossing is MetaMask, een Chrome-uitbreiding die voorzien in een kluis voor identiteit- en verbinding met een netwerk Ethereum, openbare-, test- of aangepaste. MetaMask formuleert een transactie voor het registreren van het account in het netwerk.

Deze transactie, zoals een andere transactie, wordt omgeleid naar een van de transactie-knooppunten en uiteindelijk worden worden in een blok zoals hieronder weergegeven.

![Transactie-knooppunt](./media/ethereum-deployment/transaction-node.png)

Voor het installeren van de extensie in Chrome, gaat u naar aanpassen en beheren van Google Chrome (knop Overloop) > meer extra > extensies > meer extensies ophalen en zoek naar MetaMask.

![MetaMask-extensie](./media/ethereum-deployment/metamask-extension.png)

Na de installatie MetaMask openen en een nieuwe kluis te maken. Standaard wordt de kluis worden verbonden met het testnetwerk Morden. Wijzig deze verbinding maken met het netwerk geïmplementeerde persoonlijke consortium, specifiek voor de load balancer vóór de transactie-knooppunten. Ophalen van de sjabloonuitvoer van de de beschikbaar gemaakte Ethereum RPC-eindpunt op poort 8545, met de naam als `ETHEREUM-RPC-ENDPOINT`, en voer deze in aangepaste RPC zoals hieronder wordt weergegeven.

![Instellingen voor MetaMask](./media/ethereum-deployment/metamask-settings.png)

Als u de kluis maakt, moet u een portefeuille met een account maken. Voor het maken van extra accounts selecteert **Switch Accounts** en vervolgens de **+** knop.

![MetaMask-account maken](./media/ethereum-deployment/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>Aanvankelijke Ether toewijzing initiëren

Via de beheerderspagina, kunt u een transactie om over te dragen Ether uit het vooraf toegewezen account naar een ander Ethereum-account te formuleren. Deze overdracht Ether is een transactie die is verzonden naar het knooppunt transactie en worden in een blok, zoals hieronder weergegeven.

![Transactie-knooppunt](./media/ethereum-deployment/transaction-node-mined.png)

Via het pictogram van het Klembord in de portefeuille MetaMask, kopieert u het adres van de Ethereum-account die u wilt overbrengen ether en gaat u terug naar de beheerderspagina met de. Plak de gekopieerde account in het invoerveld om over te dragen van 1000 ether uit het vooraf toegewezen Ethereum-account aan uw nieuwe account. Klik op **indienen** en wacht tot de transactie worden worden in een blok.

![Knooppuntstatus](./media/ethereum-deployment/node-status2.png)

Nadat de transactie wordt doorgevoerd in een mined blok, is het saldo in MetaMask voor uw account, de overdracht van 1000 Ether verschijnt.

![MetaMask overdracht](./media/ethereum-deployment/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Overdracht van Ether tussen accounts

Op dit moment bent u klaar voor het uitvoeren van transacties in uw persoonlijke consortium-netwerk. De eenvoudigste transactie is om over te dragen Ether vanuit het ene account naar een andere. Als u wilt dat een dergelijke transactie formuleren, kunt u MetaMask nogmaals geld overdragen van het eerste account hierboven hebt gebruikt voor een tweede account.

Van **Wallet 1** in MetaMask, klikt u op verzenden. Kopieer het adres van de tweede wallet gemaakt in **adres geadresseerde** veld en de hoeveelheid Ether om over te brengen de **bedrag** invoerveld. Klik op **verzenden** en de transactie te accepteren.

![MetaMask verzenden transactie](./media/ethereum-deployment/metamask-send.png)

Nogmaals, als de transactie is worden vastgelegd in een blok, worden de accountsaldi weerspiegeld dienovereenkomstig. Let op: **Wallet 1**van afgetrokken van saldo meer dan 15 Ether, omdat u had een analysestructuur kosten voor het verwerken van de transactie.

![Wallet 1](./media/ethereum-deployment/wallet1.png)

![Wallet 2](./media/ethereum-deployment/wallet2.png)

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om u te richten op de toepassing en slimme contract ontwikkeling op basis van uw persoonlijke consortium blockchain-netwerk.
