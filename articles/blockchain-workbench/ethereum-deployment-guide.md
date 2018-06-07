---
title: Ethereum bewijs van werkzaamheden consortium oplossingssjabloon
description: De sjabloon van de oplossing Etherereum bewijs van werkzaamheden Consortium implementeren en configureren van een lid van meerdere consortium Ethereum netwerk gebruiken
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 0ea9bd2c7d23aa227e62858983e7170b771751da
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654944"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Ethereum bewijs van werkzaamheden consortium oplossingssjabloon

De sjabloon van de oplossing Ethereum bewijs van werkzaamheden Consortium is ontworpen voor het u gemakkelijker en sneller te implementeren en configureren van een lid van meerdere consortium Ethereum netwerk met minimale kennis van Azure en Ethereum.

Met een handvol gebruikersinvoer en een implementatie met één klik via de Azure portal kunt elk lid van hun netwerk, met behulp van Microsoft Azure Compute, netwerk- en opslagservices inrichten overal ter wereld. Netwerk voetafdruk van elk lid bestaat uit een set van netwerktaakverdeling transactie knooppunten met die een toepassing of de gebruiker communiceren kan als u wilt indienen transacties, een reeks analysemodel knooppunten Registreer transacties en een VPN-gateway. Een stap van de volgende verbinding verbindt de gateways voor het maken van een volledig geconfigureerde meerdere leden blockchain-netwerk.

## <a name="about-blockchain"></a>Over blockchain

De versie van deze oplossing is voor degenen die toegevoegd aan de community blockchain zijn een geweldige kans voor meer informatie over de technologie op een manier eenvoudig en kunnen worden geconfigureerd in Azure. We raden echter aan de slag aan de eenvoudiger zelfstandige Ethereum netwerktopologie met deze aanwijzingen, voordat wordt opgebouwd uit meerdere leden consortium netwerken te implementeren.

### <a name="mining-node-details"></a>Details van de gegevensanalyse-knooppunt

Knooppunten die fungeren als mijn transacties worden gescheiden van de knooppunten die transacties om ervoor te zorgen dat de twee acties niet elkaar om dezelfde bronnen beconcurreren accepteren.

Een lid consortium kan maximaal vijf regio's met een of meer analysemodel knooppunten, ondersteund door een beheerde schijf inrichten. Een of meer knooppunten in de regio zijn geconfigureerd als een knooppunt opstarten ter ondersteuning van dynamische zichtbaarheid van de knooppunten in het netwerk. Mijnbouw knooppunten communiceren met andere knooppunten analysemodel tot consensus komen van de status van het onderliggende gedistribueerde grootboek. Er is niet nodig voor uw toepassing worden op de hoogte van of deze knooppunten worden gecommuniceerd. De knooppunten van het analysemodel zijn gericht op particuliere netwerken, geïsoleerd van binnenkomend openbare internetverkeer naar een secundaire beschermingslaag toevoegen. Uitgaand verkeer is toegestaan, maar niet voor de detectie Ethereum poort.

Alle knooppunten een stabiele versie van de client gaat Ethereum (Geth) hebben en zijn geconfigureerd als knooppunten van de analysestructuur. Als u een aangepaste genesis blok niet opgeeft hebt, gebruiken alle knooppunten de hetzelfde Ethereum adres en het sleutelpaar dat wordt beveiligd door het accountwachtwoord Ethereum. De door u opgegeven wachtwoordzin voor het Ethereum wordt gebruikt voor het genereren van het standaardaccount (coinbase) voor elk knooppunt van het analysemodel. Mijn als gegevensanalyse-knooppunten, verzamelen van kosten die zijn toegevoegd aan dit account.

Het aantal knooppunten per consortium lid analysemodel is afhankelijk van de totale grootte van het gewenste netwerk en de hoeveelheid hash-stroom die specifiek zijn voor elk lid. Hoe groter het netwerk, de meer knooppunten die nodig zijn voor het verkrijgen van een oneerlijke voordeel mogelijk onveilig. De sjabloon ondersteunt maximaal 15 analysemodel knooppunten per regio ingericht met behulp van de virtuele-machineschaalsets.

### <a name="transaction-node-details"></a>Knooppunt transactiedetails

Consortium lid heeft ook een set van netwerktaakverdeling transactie knooppunten. Deze knooppunten zijn bereikbaar vanaf buiten het virtuele netwerk, zodat toepassingen deze knooppunten gebruiken kunnen om te verzenden van transacties of smartcard contracten in het netwerk blockchain uitvoeren. Alle knooppunten een stabiele versie van de client gaat Ethereum (Geth) hebben en zijn geconfigureerd voor het onderhouden van een volledige kopie van het gedistribueerde grootboek. Als een aangepaste genesis blok niet opgegeven is, wordt door deze knooppunten dezelfde Ethereum account, beveiligd door het accountwachtwoord Ethereum gebruiken.

Transactie-knooppunten zijn taakverdeling binnen een beschikbaarheidsset om hoge beschikbaarheid te houden. De sjabloon ondersteunt maximaal vijf transactie knooppunten die zijn ingericht met behulp van de virtuele-machineschaalsets.

### <a name="log-analytics-details"></a>Log analytics-details

Elke implementatie ook maakt een nieuw exemplaar van logboekanalyse of kan worden gekoppeld aan een bestaand exemplaar. Hierdoor is de bewaking van verschillende maatstaven voor prestaties van elke virtuele machine waaruit het geïmplementeerde netwerk.

## <a name="deployment-architecture"></a>Architectuur voor implementatie

### <a name="description"></a>Beschrijving

Deze oplossingssjabloon kan één of meerdere op basis van regio multi lid Ethereum consortium netwerk kunt implementeren. Het virtuele netwerk van elke regio is verbonden met een andere regio in een keten-topologie met behulp van de VNET-gateways en de verbindingsbronnen. Deze bepalingen ook een registrar, die de vereiste informatie van alle mijnwerkers en transactie knooppunten geïmplementeerd in elke regio bevat.

### <a name="standalone-and-consortium-leader-overview"></a>Overzicht van zelfstandige en consortium opvulteken

![Zelfstandige en Consortium opvulteken overzicht](./media/ethereum-deployment-guide/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Overzicht van consortium lid toevoegen

![Overzicht van Consortium lid toevoegen](./media/ethereum-deployment-guide/member-overview.png)

## <a name="getting-started"></a>Aan de slag

Dit proces vereist een Azure-abonnement dat u kunt ondersteuning voor de implementatie van meerdere virtuele machines-schaalsets en schijven die worden beheerd. Maak indien nodig om te beginnen met een gratis Azure-account.

Als een abonnement is beveiligd, gaat u naar de Azure-portal. Selecteer **+ maken van een resource**, Marketplace (Zie alle), en zoek naar **Ethereum bewijs van werkzaamheden Consortium**.

De sjabloonimplementatie begeleidt u stapsgewijs door het eerste lid footprint configureren in het netwerk. De stroom van de implementatie is onderverdeeld in vijf stappen: basisbeginselen, Operations Management Suite, regio's voor implementatie, de grootte van het netwerk en prestaties, Ethereum instellingen.

### <a name="basics"></a>Basisbeginselen

Onder **basisbeginselen**, waarden opgeven voor de standaardparameters voor de implementatie, zoals abonnement, resourcegroep en eigenschappen van eenvoudige virtuele machine.

![Basisbeginselen](./media/ethereum-deployment-guide/sample-deployment.png)

Parameternaam|Beschrijving| Toegestane waarden|Standaardwaarden
---|---|---|---
Een nieuw netwerk of een bestaande join-netwerk maken?|Een nieuw netwerk maken of koppelen van een reeds bestaand consortium netwerk|Nieuwe Join bestaande maken|Create New
Een netwerk dat onderdeel van een consortium zijn zal implementeren?|Een consortium kunt toekomstige implementaties om toe te voegen dit netwerk (zichtbaar wanneer *nieuw* hierboven is geselecteerd)|Zelfstandige Consortium|Zelfstandig
Resource-voorvoegsel |De tekenreeks die wordt gebruikt als basis voor de naamgeving van resources (2 tot 4 alfanumerieke tekens). Een unieke hash is functienaam geplaatst om de tekenreeks voor bepaalde bronnen, terwijl de resource-specifieke informatie wordt toegevoegd.|Alfanumerieke tekens met lengte 2 tot 4|N.v.t.
VM-gebruikersnaam| Gebruikersnaam voor de beheerder van elke geïmplementeerde virtuele machine (alleen alfanumerieke tekens)|1 - 64 tekens |gethadmin
Verificatietype|De methode voor verificatie bij de virtuele machine. |Wachtwoord of SSH openbare sleutel|Wachtwoord
Wachtwoord (verificatietype = wachtwoord)|Het wachtwoord voor het administrator-account voor elk van de geïmplementeerde virtuele machines. Het wachtwoord moet bevatten 3 van de volgende vereisten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. <br />Terwijl alle VM's in eerste instantie hetzelfde wachtwoord zijn, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|N.v.t.
SSH-sleutel (verificatietype = openbare sleutel)|De sleutel voor secure shell gebruikt voor externe aanmelding.|| N.v.t.
Abonnement| Het abonnement waaraan het consortium netwerk implementeren||N.v.t.
Resourcegroep| De resourcegroep waartoe het consortium netwerk implementeren.||N.v.t.
Locatie| De Azure-regio voor de resourcegroep. ||N.v.t.



### <a name="operations-management-suite"></a>Operations Management Suite

De blade Operations Management Suite (OMS) kunt u een OMS-bron voor uw netwerk te configureren. OMS verzamelt en surface nuttig metrische gegevens en logboeken van uw netwerk, de mogelijkheid om snel te controleren de netwerkstatus of foutopsporing uitgeeft. Het gratis aanbod van OMS mislukken probleemloos zodra capaciteit is bereikt.

![Maken van nieuwe OMS](./media/ethereum-deployment-guide/new-oms.png)

Parameternaam|Beschrijving| Toegestane waarden|Standaardwaarden
---|---|---|---
Verbinding maken met bestaande OMS|Maak een nieuw exemplaar van logboekanalyse of Word lid van een bestaand exemplaar|Nieuwe Join bestaande maken|Maak nieuwe Log Analytics-locatie|De regio waar de nieuwe logboekanalyse wordt geïmplementeerd (weergegeven indien *nieuw* is geselecteerd)
Bestaande OMS-werkruimte-Id|Werkruimte-ID van het bestaande exemplaar (weergegeven indien *deelnemen aan bestaande* is geselecteerd) OMS-servicelaag|Kies de prijscategorie voor het nieuwe exemplaar. Meer informatie op https://azure.microsoft.com/pricing/details/log-analytics/ (weergegeven indien *deelnemen aan bestaande* is geselecteerd)|Gratis zelfstandige Per knooppunt|Gratis
Bestaande OMS primaire sleutel|De primaire sleutel gebruikt voor verbinding met de bestaande OMS-instantie (weergegeven indien *deelnemen aan bestaande* is geselecteerd)

### <a name="deployment-regions"></a>Implementatie-regio 's

Vervolgens onder **implementatie regio's**, geef ingangen voor **aantal regio('s)** het consortium netwerk en de selectie van Azure-regio's op basis van het aantal opgegeven regio's implementeren. Gebruiker kunt in maximaal vijf regio's implementeren.

![Implementatie-regio 's](./media/ethereum-deployment-guide/deployment-regions.png)

Parameternaam| Beschrijving| Toegestane waarden |Standaardwaarden
---|---|---|---
Aantal regio('s)| Aantal regio's voor het implementeren van het netwerk consortium|1, 2, 3, 4, 5| 2
Eerste gebied| Eerste regio voor de implementatie van het netwerk consortium|Alle toegestaan Azure-regio 's| VS - west
Tweede regio |Tweede regio voor de implementatie van het netwerk consortium (zichtbaar alleen wanneer het aantal gebieden is geselecteerd als 2)|Alle toegestaan Azure-regio 's| VS - oost
Derde regio| Derde regio voor de implementatie van het netwerk consortium (zichtbaar alleen wanneer het aantal gebieden is geselecteerd als 3)|Alle toegestaan Azure-regio 's| VS - midden
Vierde regio| Vierde regio voor de implementatie van het netwerk consortium (zichtbaar alleen wanneer het aantal gebieden is geselecteerd als 4)|Alle toegestaan Azure-regio 's| VS - oost 2
Vijfde regio| Vijfde regio voor de implementatie van het netwerk consortium (zichtbaar alleen wanneer het aantal gebieden is geselecteerd als 5)|Alle toegestaan Azure-regio 's| VS - west 2

### <a name="network-size-and-performance"></a>De netwerkgrootte van het en prestaties

Vervolgens onder **grootte en de prestaties** ingangen voor de grootte van het netwerk consortium, zoals het aantal en de grootte van het analysemodel knooppunten en knooppunten van de transactie opgeven.

![De netwerkgrootte van het en prestaties](./media/ethereum-deployment-guide/network-size-performance.png)

Parameternaam |Beschrijving |Toegestane waarden| Standaardwaarden
---|---|---|---
Het aantal knooppunten voor gegevensanalyse|Het aantal analysemodel knooppunten per regio is geïmplementeerd|2 - 15| 2
Prestaties van de analysestructuur knooppunt opslag|Het type beheerde upschijf van elk van de geïmplementeerde gegevensanalyse-knooppunten.|Standard- of Premium|Standard
Grootte van de virtuele machine mijnbouw knooppunt|De grootte van de virtuele machine wordt gebruikt voor knooppunten van de analysestructuur.|Standard A <br />Standaard D <br />Standaard D-v2 <br />Standaard F-serie <br />Standaard DS <br />en standaard FS|Standaard D1v2
Aantal taakverdeling transactie knooppunten|Het aantal knooppunten van de transactie te richten als onderdeel van het netwerk.|1 - 5| 2
Prestaties van de transactie knooppunt opslag|Het type beheerde upschijf van elk van de knooppunten van de gedistribueerde transactie.|Standard- of Premium|Standard
De grootte van de virtuele machine knooppunt transactie|De grootte van de virtuele machine wordt gebruikt voor transactie-knooppunten.|Standard A <br />Standaard D <br />Standaard D-v2 <br />Standaard F-serie <br />Standaard DS <br />en standaard FS|Standaard D1v2

### <a name="ethereum-settings"></a>Ethereum instellingen

Vervolgens onder **Ethereum instellingen**, Ethereum-gerelateerde configuratie-instellingen opgeven, zoals de netwerk-ID en Ethereum account wachtwoord of genesis blok.

![Ethereum instellingen](./media/ethereum-deployment-guide/standalone-leader-deployment.png)

Parameternaam |Beschrijving |Toegestane waarden|Standaardwaarden
---|---|---|---
ConsortiumMember-ID|De ID die is gekoppeld aan elk lid deel uitmaakt van het consortium netwerk gebruikt voor het IP-adresruimten configureren om te voorkomen van conflicten. <br /><br />Lid-ID moet uniek zijn in verschillende organisaties in hetzelfde netwerk. Een unieke lid-ID is vereist, zelfs wanneer dezelfde organisatie naar meerdere regio's implementeert.<br /><br />Noteer de waarde van deze parameter omdat u wel wilt delen met andere leden van de gekoppelde.|0 - 255
Ethereum netwerk-ID|De netwerk-ID voor het consortium Ethereum netwerk wordt geïmplementeerd. Elk netwerk Ethereum heeft een eigen netwerk-ID, met 1 wordt de ID voor het openbare netwerk. Toegang tot het netwerk is niet toegestaan voor gegevensanalyse knooppunten, nog steeds wordt aangeraden om conflicten te voorkomen dat met behulp van een groot aantal.|5 - 999.999.999| 10101010
Aangepaste genesis blokkeren|De optie voor het automatisch genereren van een blok genesis of een aangepaste planning opgeven.|Ja/Nee| Nee
Accountwachtwoord Ethereum (aangepaste genesis blok = Nee)|Het administrator-wachtwoord gebruikt om de Ethereum-account dat is geïmporteerd in elk knooppunt te beveiligen. Het wachtwoord moet bevatten het volgende: 1 hoofdletter, 1 kleine letter en 1 cijfer.|12 of meer ongeldige tekens|N.v.t.
De persoonlijke sleutel wachtwoordzin Ethereum (aangepaste genesis blok = Nee)|De wachtwoordzin die wordt gebruikt voor het genereren van de persoonlijke ECC-sleutel die is gekoppeld aan het standaardaccount voor Ethereum die wordt gegenereerd. Een vooraf gegenereerde persoonlijke sleutel hoeft niet expliciet worden doorgegeven.<br /><br />Houd rekening met een wachtwoordzin met voldoende aanvraaggrootte zodat een sterke persoonlijke sleutel en zonder overlap met andere leden consortium. De wachtwoordzin voor de volgende minimaal moet bevatten: 1 hoofdletter, 1 kleine letter en 1 cijfer.<br /><br />Opmerking Als twee leden gebruikmaken van dezelfde wachtwoordzin de accounts die worden gegenereerd, wordt hetzelfde zijn. Dezelfde wachtwoordzin is handig als één organisatie probeert te implementeren in regio's en wil delen één account (knoopcelbatterij base) op alle knooppunten.|12 of meer ongeldige tekens|N.v.t.
Genesis blok (aangepaste genesis blok = Yes)|JSON-tekenreeks voor aangepaste genesis blok. U vindt meer informatie over de indeling van het blok genesis hier onder aangepaste netwerken.<br /><br />Een Ethereum-account is nog steeds gemaakt bij het opgeven van een aangepaste genesis-blok. Overweeg een prefunded Ethereum-account in het blok genesis niet wachttijd voor gegevensanalyse.|Geldige JSON |N.v.t.
Gedeelde sleutel voor verbinding|Een gedeelde sleutel voor de verbinding tussen VNET-gateways.| 12 of meer ongeldige tekens|N.v.t.
Consortium gegevens-URL|De URL die verwijst naar de relevante consortium configuratiegegevens geleverd door de implementatie van een ander lid. <br /><br />Deze informatie wordt verstrekt door een lid is al verbonden met een implementatie. Als u de rest van het netwerk hebt geïmplementeerd, is de URL van de sjabloon implementatie uitvoer, CONSORTIUM gegevens genoemd.||N.v.t.
VNet-Gateway verbinding maken met|Het bronpad van de VNet-Gateway op om verbinding te maken.<br />Deze informatie wordt verstrekt door een lid is al verbonden met een implementatie. Als u de rest van het netwerk hebt geïmplementeerd, wordt de URL in met de naam CONSORTIUM_MEMBER_GATEWAY_ID de sjabloon implementatie uitvoer is. Opmerking: Hetzelfde lid consortium gegevens URL en VNet-Gateway resource moet worden gebruikt.||N.v.t.
Eindpunt van de registratieservice voor Peer-informatie|Peer info eindpunt geleverd door een ander lid implementatie|Ongeldig eindpunt van het eerste lid in consortium|N.v.t.
Sleutel van Peer informatie registrar|Peer info primaire sleutel van de implementatie van een ander lid|Geldige primaire sleutel van het eerste lid in consortium|N.v.t.

### <a name="summary"></a>Samenvatting

Klik in de samenvatting blade om te controleren van de opgegeven invoer en uit te voeren basisvalidatietests voorafgaand aan de implementatie.

![Samenvatting](./media/ethereum-deployment-guide/summary.png)

Wettelijke informatie en privacy rwaarden en klik op **aankoop** te implementeren. Als de implementatie meer dan één regio heeft, of voor een netwerk consortium, implementeert deze sjabloon vooraf vervolgens de benodigde VPN-gateways ter ondersteuning van verbinding met het netwerk met andere leden. Implementatie van de gateway kan tot 45 tot 50 minuten duren.

## <a name="post-deployment-sanity-checks"></a>Bevestigingen voor post-implementatie

### <a name="administrator-page"></a>Administrator-pagina

Zodra de implementatie is voltooid en alle resources zijn ingericht, kunt u naar de beheerderspagina voor een weergave van uw netwerk blockchain gaan en sanity Controleer de implementatiestatus van de. De URL van de beheerpagina is de DNS-naam van de load balancer. het is aanwezig in het gedeelte van de uitvoer van de sjabloonimplementatie met de naam als ADMIN_SITE.

Als u wilt vinden, selecteer de resourcegroep die is geïmplementeerd. Selecteer **overzicht**, en klik op de koppeling onmiddellijk onder **implementaties** geeft het getal dat is voltooid.

![Overzicht van de groep bronnen](./media/ethereum-deployment-guide/resource-overview.png)

Het nieuwe scherm toont de geschiedenis van implementatie. Selecteer de eerste implementatie van resource (bijvoorbeeld microsoft-azure-blockchain.azure-blockchain-servi...) en zoekt u naar de **uitvoer** sectie in de onderste helft van de pagina. Hier ziet u de URL voor de beheerpagina die worden vermeld in de sjabloon implementatie uitvoerparameter als ADMIN_SITE.

![Implementaties van resource](./media/ethereum-deployment-guide/resource-deployments.png)

![Implementatie-uitvoer](./media/ethereum-deployment-guide/deployment-output.png)

Als u naar de pagina beheerder, kopieert u de **ADMIN-SITE** uitvoer en geopend in een ander tabblad.

Op de beheerpagina kunt u een overzicht van de topologie die u hebt geïmplementeerd aan de hand van de sectie Ethereum knooppunt Status ophalen. Dit omvat alle knooppunt hostnamen, het aantal peer en het laatste blok gezien. Het aantal peer voor elk knooppunt is tussen het minimum van een kleiner zijn dan de *totaal aantal knooppunten* en het aantal geconfigureerde maximale peer. Noteer het aantal peer wordt niet beperkt het aantal knooppunten dat kan worden geïmplementeerd in het netwerk.
Soms ziet u het aantal peer fluctueren en niet kleiner zijn dan de (totaal aantal knooppunten - 1). Het verschil in de telling is niet altijd een teken dat de knooppunten beschadigd, zijn omdat recoveryforks in het grootboek kunnen kleine wijzigingen in peer aantal. Ten slotte kunt u het laatste blok zichtbaar zijn voor elk knooppunt in het netwerk om te bepalen recoveryforks of lag in het systeem controleren.

![Status van knooppunt](./media/ethereum-deployment-guide/node-status.png)

De status van het knooppunt wordt vernieuwd elke 10 seconden. Laad de pagina via de browser opnieuw of **opnieuw laden** om de weergave bijwerken.

### <a name="oms-portal"></a>OMS-portal

De OMS-portal kunt u vinden door de koppeling in de uitvoer van de implementatie (OMSPORTALURL) of door het selecteren van de OMS-bron in uw geïmplementeerde resourcegroep.

![OMS-URL](./media/ethereum-deployment-guide/oms-url.png)

![OMS-resource](./media/ethereum-deployment-guide/oms-resource.png)

De portal wordt eerst op hoog niveau worden verzameld als een overzicht weergegeven.

![OMS-overzicht](./media/ethereum-deployment-guide/oms-overview.png)

Klik op het overzicht doorgeleid naar een portal statistieken per knooppunt weergeven.

![De statistieken per knooppunt](./media/ethereum-deployment-guide/per-node-stats.png)

### <a name="accessing-nodes"></a>Toegang tot de knooppunten

U kunt op afstand verbinding maken met de virtuele machines voor de transactie knooppunten via SSH met uw beheerder opgegeven gebruikersnaam en wachtwoord/SSH-sleutel. Aangezien het transactie-knooppunt VM's geen eigen openbare IP-adressen, moet u om te gaan via de load balancer en het poortnummer opgeven. De SSH-opdracht uit te voeren om toegang tot het eerste knooppunt van de transactie wordt vermeld in de sjabloon implementatie uitvoerparameter als **SSH_TO_FIRST_TX_NODE** (voor de Voorbeeldimplementatie: de ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Voor extra transactie knooppunten moet het poortnummer met één worden verhoogd (bijvoorbeeld het eerste knooppunt van de transactie is op poort 4000).

Aangezien de virtuele machines waarop de knooppunten van het analysemodel uitgevoerd niet extern toegankelijk is, moet u via een van de transactie-knooppunten. Zodra u een SSH-sessie op een knooppunt transactie hebt, installeren van uw persoonlijke sleutel op het knooppunt transactie of uw wachtwoord met een SSH-sessie starten in een van de knooppunten van het analysemodel.

**Opmerking**

De hostnamen kan worden verkregen van beheer-Site of van de Azure-portal. In Azure-portal, de hostnamen van knooppunten die aanwezig zijn in de bron van de virtuele machine scale set (VMSS) wordt vermeld onder **exemplaren**, dat afwijkt van de werkelijke hostnamen. Bijvoorbeeld, de hostnaam in Azure-portal eruit als **n-asdfmv-reg1_0** , maar de werkelijke hostnaam kan worden vergeleken met **n-asdfmv-reg**.

Bijvoorbeeld:

Azure portal hostnaam| Werkelijke hostnaam
---|---
n-ethwvu-reg1_0| n-ethwvu-reg1000000
n-ethwvu-reg1_1 |n-ethwvu-reg1000001
n-ethwvu-reg1_2 |n-ethwvu-reg1000002

## <a name="adding-a-new-consortium-member"></a>Het toevoegen van een nieuwe consortium lid

### <a name="sharing-data"></a>Delen van gegevens

Als het eerste lid (of verbonden lid) van het consortium moet u andere leden bieden enkele stukjes informatie zodat ze kunnen koppelen en hun verbinding tot stand brengen. Specifiek:

1. **Gedeelde Consortium configuratiegegevens**: Er is een set van gegevens die wordt gebruikt voor het indelen van de Ethereum verbinding tussen twee leden. De nodige informatie, waaronder de genesis blok, consortium netwerk-ID en opstartknooppunten, wordt naar een bestand op de transactie-knooppunten van het opvulteken of een ander lid van de geïmplementeerde geschreven. De locatie van dit bestand wordt vermeld in de sjabloon implementatie output-parameter met de naam **CONSORTIUM gegevens**.
2. **Peer-Info eindpunt**: de Peer info registrar eindpunt opvragen van alle knooppunten die is verbonden met het netwerk Ethereum van de toonaangevende leveranciers of een ander lid implementatie. De database worden opgeslagen een reeks informatie met betrekking tot elk knooppunt verbonden in het netwerk, zoals de hostnaam van het knooppunt, persoonlijke IP-adres, enzovoort. Dit is de sjabloon implementatie output-parameter met de naam **PEER_INFO_ENDPOINT**.
3. **De primaire sleutel Info peer**: de Peer info registrar primaire sleutel wordt gebruikt voor toegang tot het opvulteken of andere lid Peer info primaire sleutel. Dit is de sjabloon implementatie output-parameter met de naam **PEER_INFO_PRIMARY_KEY**.


4. **VNET-Gateway**: elk lid van een verbinding tot stand met het hele blockchain netwerk via een bestaand lid. Als u wilt verbinding maken met een VNET, moet u het bronpad naar de VNET-Gateway van het lid waarmee u verbinding maakt. Dit is de sjabloon implementatie output-parameter met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
5. **Gedeelde sleutel**: een vooraf vastgestelde geheime tussen twee leden van het netwerk consortium die een verbinding tot stand brengt. Dit is een alfanumerieke tekenreeks (tussen-1 tot 128 tekens) die is overeengekomen buiten de context van de implementatie. (Bijvoorbeeld **MySharedKeyAbc123**)

### <a name="acceptance-of-new-member"></a>Acceptatie van nieuwe lid

Deze stap moet worden uitgevoerd nadat het gekoppelde lid heeft hun netwerk is geïmplementeerd. Voordat een lid kan verbinding met het netwerk en transactie verkeer ziet, moet een bestaand lid een uiteindelijke configuratie uitvoeren op hun VPN-Gateway het verbinding accepteren. Dit betekent dat de Ethereum knooppunten van het gekoppelde lid wordt niet uitgevoerd totdat een verbinding tot stand is gebracht. Deze configuratie kan worden uitgevoerd via PowerShell of xPlat CLI. Een PowerShell-module en xPlat CLI-script worden ook opgeslagen op het knooppunt transactie naast de consortium-gegevens. Locatie van het script wordt de implementatie van output-parameters met de naam **paar-GATEWAY-PS-MODULE** en **paar-GATEWAY-AZURE-CLISCRIPT**respectievelijk.

**PowerShell/CLI Setup**

Meer informatie over het aan de slag met Azure PowerShell-cmdlets en Azure xPlat CLI vindt u in Azure-documentatie.

U moet de meest recente versie van de Azure-cmdlets lokaal zijn geïnstalleerd en een sessie geopend. Controleer of aan te melden bij de sessie met de referenties van uw Azure-abonnement.

**PowerShell: Verbinding tot stand brengen**

De PowerShell-module downloaden en lokaal opslaan. De locatie van de PowerShell-module is opgegeven als de **paar-GATEWAY-PS-MODULE** sjabloonimplementatie uitvoerparameter.

Als u nog niet is ingeschakeld, gebruikt u de **Set-ExecutionPolicy** cmdlet voor de lokale sessie om toe te staan een niet-ondertekende module uitvoert.

**Set-ExecutionPolicy Unrestricted CurrentUser**

Volgende, meld u aan bij uw Azure-abonnement waarin u het opvulteken implementatie met behulp van geïmplementeerd

**Login-AzureRmAccount**

Importeer vervolgens de module:

**Import-Module <filepath to downloaded file>**

Voer ten slotte de functie met de juiste invoer:

- **MyGatewayResourceId** : bronpad van uw Gateway. Dit is de sjabloon implementatie output-parameter met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId** : bronpad van het gekoppelde lid gateway. Dit wordt aangeboden door het gekoppelde lid en de sjabloon uitvoerparameter van de implementatie ook heet **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName** : een naam op voor u deze Gateway-verbinding wilt aanduiden.
- **Gedeelde sleutel** : het vooraf vastgestelde geheim tussen de twee leden van het netwerk consortium die een verbinding tot stand brengt.

**CreateConnection** -MyGatewayResourceId <resource path of your Gateway> - OtherGatewayResourceId < bronpad van het gekoppelde lid gateway > - ConnectionName mijnVerbinding - SharedKey 'MySharedKeyAbc123'

**xPlat CLI: verbinding maken**

Download de Azure CLI-script en lokaal opslaan. De locatie van de Azure CLI-script is opgegeven in de sjabloonparameter implementatie met de naam **paar-GATEWAY-AZURE-CLI-SCRIPT**.

Voer het script met de juiste invoer:

- **MyGatewayResourceId** : bronpad van uw Gateway. Dit is de sjabloon implementatie output-parameter met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId** : bronpad van het gekoppelde lid gateway. Dit wordt geleverd door de gekoppelde lid en de implementatie sjabloonparameter van hun implementatie ook met de naam **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName** : een naam op voor u deze Gateway-verbinding wilt aanduiden.
- **Gedeelde sleutel** : het vooraf vastgestelde geheim tussen de twee leden van het netwerk consortium die een verbinding tot stand brengt.
- **Locatie** : de Azure-regio waar uw gateway-resource is geïmplementeerd.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

De architectuur als volgt zijn nadat u hebt geconfigureerd verbinding tussen **opvulteken** en **lid** implementaties.

![Opvulteken en lid-architectuur](./media/ethereum-deployment-guide/leader-member.png)

## <a name="fund-new-member-account"></a>Het nieuwe lidaccount Fund

### <a name="generated-genesis-block"></a>Gegenereerde genesis blokkeren

Uw account Ethereum is als het eerste lid basis met een miljard ether als de implementatie van het blok genesis genereert (aangepaste Genesis blok = Nee). Andere leden heeft een account dat is niet vooraf basis en Ether verzamelen als hun knooppunten analysemodel begint met mijn blokken moet wachten. Om te voorkomen dat nieuwe leden Ether wacht, moet u expliciet fi gekoppelde leden Ethereum accounts.

Om dit te doen moet lid te worden leden bieden u de Ethereum-account dat wordt weergegeven op de website van de beheerder. U kunt uw website admin vervolgens gebruiken om over te dragen van uw account Ether bij hun account door te voeren van het opgegeven account.

### <a name="custom-genesis-block"></a>Aangepaste genesis blokkeren

Als een aangepaste genesis blok wordt geleverd met een opgegeven Ethereum-account, kunt u MetaMask of een ander hulpprogramma ether overbrengen van die aan de vooraf gegenereerde Ethereum account zichtbaar in de website admin-account opgegeven. Zie voor instructies over het gebruik van MetaMask [Ethereum-Account maken](#create-ethereum-account).

Als een aangepaste genesis blok wordt geleverd zonder een account of u geen toegang tot een vooraf toegewezen accounts hebt, moet u wachten totdat de knooppunten van het analysemodel begint met mijn voor het genereren van Ether bij uw account (knoopcelbatterij base). Hoe snel de middelen die worden gegenereerd, is afhankelijk van het niveau van de moeite dat u in het aangepaste genesis-blok opgeeft.

## <a name="create-ethereum-account"></a>Ethereum-account maken

Als u wilt een extra account maakt, kunt u verschillende oplossingen. Een dergelijke oplossing is MetaMask, een Chrome-uitbreiding die voorzien in een kluis identiteit en verbinding met een netwerk Ethereum openbare, test of aangepast. MetaMask formuleert een transactie voor het registreren van het account in het netwerk.

Deze transactie, zoals een andere transactie gaat naar een van de transactie-knooppunten en uiteindelijk mijn worden verborgen in een blok zoals hieronder weergegeven.

![Transactie-knooppunt](./media/ethereum-deployment-guide/transaction-node.png)

Voor het installeren van de extensie in Chrome, gaat u naar aanpassen en beheren van de Google Chrome (knop Overloop) > meer extra > extensies > meer uitbreidingen ophalen en zoek naar MetaMask.

![MetaMask-uitbreiding](./media/ethereum-deployment-guide/metamask-extension.png)

Na de installatie configureert MetaMask openen en een nieuwe kluis maken. Standaard wordt de kluis worden verbonden met het testnetwerk Morden. U moet deze verbinding maken met het netwerk geïmplementeerde persoonlijke consortium specifiek aan de load balancer voor de transactie-knooppunten wijzigen. Ophalen van de sjabloonuitvoer van de de blootgestelde Ethereum RPC-eindpunt op poort 8545, met de naam als `ETHEREUM-RPC-ENDPOINT`, en voer deze in aangepaste RPC zoals hieronder wordt weergegeven.

![MetaMask instellingen](./media/ethereum-deployment-guide/metamask-settings.png)

Als u de kluis maakt, moet u een wallet met een account maken. Als u extra accounts, selecteert **Switch Accounts** en vervolgens de **+** knop.

![MetaMask-account maken](./media/ethereum-deployment-guide/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>Aanvankelijke Ether toewijzing initiëren

Via de beheerderspagina kunt u een transactie om over te dragen Ether uit het vooraf toegewezen account aan een andere Ethereum account formuleren. Deze overdracht Ether is een transactie wordt verzonden naar het knooppunt transactie en mijn verborgen in een blok, zoals hieronder weergegeven.

![Transactie-knooppunt](./media/ethereum-deployment-guide/transaction-node-mined.png)

Via het pictogram van het Klembord in de wallet MetaMask, kopieert u het adres van de Ethereum account die u wilt overdragen ether en Ga terug naar de beheerderspagina. Plak de gekopieerde account in het invoerveld 1000 ether overzetten van de vooraf toegewezen Ethereum account naar uw nieuwe account. Klik op **indienen** en wacht tot de transactie tot mijn worden verborgen in een blok.

![Status van knooppunt](./media/ethereum-deployment-guide/node-status2.png)

Zodra de transactie is doorgevoerd in een mined blok, wordt het accountsaldo in MetaMask voor uw account de overdracht van 1000 Ether weerspiegelen.

![MetaMask overdracht](./media/ethereum-deployment-guide/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Overdracht van Ether tussen accounts

Op dit moment bent u klaar om uit te voeren binnen uw netwerk persoonlijke consortium transacties. De eenvoudigste transactie is om over te dragen Ether vanuit het ene account naar de andere. Als u wilt dat een dergelijke transactie formuleren, kunt u MetaMask nogmaals geld overzetten van het eerste account gebruikt boven aan een tweede account.

Van **Wallet 1** MetaMask, klik op verzenden. Kopieer het adres van de tweede wallet gemaakt in **adres geadresseerde** invoer veld en hoeveelheid Ether om over te brengen de **bedrag** invoerveld. Klik op **verzenden** en de transactie te accepteren.

![MetaMask verzenden transactie](./media/ethereum-deployment-guide/metamask-send.png)

Nogmaals: als de transactie is mijn verborgen vastgelegd in een blok, de accountsaldo's worden doorgevoerd dienovereenkomstig. Opmerking: **Wallet 1**van saldo wordt afgetrokken meer dan 15 Ether, aangezien moest u analysemodel betalen voor het verwerken van de transactie.

![Wallet 1](./media/ethereum-deployment-guide/wallet1.png)

![Wallet 2](./media/ethereum-deployment-guide/wallet2.png)

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om u te concentreren op toepassings- en slimme contract ontwikkelen op basis van uw persoonlijke consortium blockchain netwerk.
