---
title: Ethereum Proof-of-Authority Consortium - Azure
description: De oplossing Ethereum Proof-of-Authority Consortium gebruiken om te implementeren en configureren van een consortium voor meerdere leden Ethereum-netwerk
services: azure-blockchain
keywords: ''
author: CodyBorn
ms.author: coborn
ms.date: 8/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: vamelech
ms.openlocfilehash: 50d24fd41a0a933d9cfec37477773463a918ca0a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549065"
---
# <a name="ethereum-proof-of-authority-consortium"></a>Ethereum proof-of-authority consortium

## <a name="overview"></a>Overzicht
[Deze oplossing](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) is ontworpen om eenvoudiger te implementeren, configureren en beheren van een consortium voor meerdere leden Proof-of-authority Ethereum-netwerk met minimale kennis van Azure en Ethereum.

Met een aantal van de invoer van gebruikers en een implementatie met één klik via de Azure-portal inrichten elk lid van een netwerk-footprint, met behulp van Microsoft Azure Compute, netwerken en opslagservices over de hele wereld. Netwerk-voetafdruk van elk lid bestaat uit een set met load balancing validator knooppunten met die een toepassing of de gebruiker communiceren kan om in te dienen Ethereum-transacties.

## <a name="concepts"></a>Concepten

### <a name="terminology"></a>Terminologie

-   **Consensus** -de handeling van het synchroniseren van gegevens via het netwerk gedistribueerd via blok validatie en maken.

-   **Consortium lid** : een entiteit die deel uitmaakt van consensus op de Blockchain-netwerk.

-   **Beheerder** -een Ethereum-account dat wordt gebruikt voor het beheren van deelname aan voor een bepaalde consortium lid.

-   **Validator** -een virtuele machine die is gekoppeld aan een Ethereum-account dat deel van consensus namens een beheerder uitmaakt.

### <a name="proof-of-authority"></a>Proof-of-authority

De release van deze oplossing is bedoeld voor degenen die niet bekend bent met de blockchain-community een uitstekende gelegenheid om te leren over de technologie op een manier eenvoudig en kunnen worden geconfigureerd in Azure. Bewijs van werk is een Sybil weerstand mechanisme die gebruikmaakt van berekenen van de kosten voor het netwerk zelf regelen en geoorloofd deelname toestaan. Deze optie werkt uitstekend anoniem, open blockchain-netwerken waarin concurrentie voor cryptocurrency bevordert voor beveiliging op het netwerk. In particuliere/consortium netwerken echter heeft de onderliggende Ether geen waarde. Een alternatieve proof-of-authority-protocol is beter geschikt is voor toegestane netwerken waarin alle consensus deelnemers zijn bekend en betrouwbaar. Zonder de noodzaak voor gegevensanalyse is er nog steeds behouden Byzantine fouttolerantie Proof-of-authority efficiënter.

### <a name="consortium-governance"></a>Consortium governance

Omdat er is een proof-of-authority afhankelijk van een lijst van netwerk-instanties voor het netwerk gezond te houden, is het belangrijk om te voorzien van een geoorloofd mechanisme om de wijzigingen aanbrengen in deze lijst met machtigingen. Elke implementatie wordt geleverd met een set van smart-contracten en -portal voor in de chain beheer van deze toegestane lijst. Zodra een voorgestelde wijziging een meerderheid van stemmen door consortium leden bereikt, wordt de wijziging van kracht. Hierdoor kan nieuwe consensus deelnemers worden toegevoegd of aangetast deelnemers moet worden verwijderd op een transparante manier die een eerlijk netwerk worden gebruikers aangemoedigd.

### <a name="admin-account"></a>Beheeraccount

Tijdens de implementatie van de knooppunten proof-of-authority, zult u worden gevraagd om een Admin Ethereum-adres. U kunt meerdere verschillende mechanismen te genereren en deze Ethereum-account te beveiligen. Wanneer dit adres is toegevoegd als een instantie in het netwerk, kunt u dit account gebruiken om deel te nemen in governance. Dit beheerdersaccount wordt ook gebruikt om te delegeren consensus deelname aan de validator knooppunten die zijn gemaakt als onderdeel van deze implementatie. Omdat alleen de openbare Ethereum-adres wordt gebruikt, heeft elke beheerder de flexibiliteit voor het beveiligen van de persoonlijke sleutels op een manier die volgt op de gewenste beveiligingsmodel.

### <a name="validator-node"></a>Validatie van knooppunt

In het protocol proof-of-authority ter validator knooppunten vervanging van traditionele minder knooppunten. Elke validatiefunctie heeft een unieke Ethereum-identiteit die wordt toegevoegd aan een lijst met machtigingen van de smartcard-contract. Zodra een validator in deze lijst staat is, kunt het proces voor het maken van het blok deelnemen. Zie voor meer informatie over dit proces, van pariteit documentatie op [instantie ronde consensus](https://wiki.parity.io/Aura). Elk lid van de consortium kunt twee of meer knooppunten van de validator inrichten in vijf regio's voor geografische redundantie. Validatie van knooppunten communiceren met andere knooppunten validator tot consensus bent gekomen over de status van het onderliggende gedistribueerd grootboek.
Om ervoor te zorgen geoorloofd deelname op het netwerk, elk consortium lid van het gebruik van meer validators dan het eerste lid op het netwerk is niet toegestaan (als het eerste lid heeft drie validators geïmplementeerd, elk lid kan alleen hebben maximaal drie validators).

### <a name="identity-store"></a>-Identiteitenarchief

Omdat elk lid heeft meerdere validator knooppunten tegelijk worden uitgevoerd en elk knooppunt moet een toegestane identiteit hebben, is het belangrijk dat de validators een unieke actieve identiteit in het netwerk veilig kunnen verkrijgen. Als u wilt dit eenvoudiger maken hebben we een Identity-Store die wordt geïmplementeerd in het abonnement van elk lid van die veilig de gegenereerde Ethereum-identiteiten bevat gebouwd. De orchestration-container wordt bij de implementatie van een persoonlijke Ethereum-sleutel genereren voor elke validator en opslaan in Azure Key Vault. Voordat het knooppunt pariteit wordt gestart, krijgt deze eerst een lease op een niet-gebruikte identiteit om te controleren of dat de identiteit is niet opgehaald door een ander knooppunt. De identiteit wordt geleverd aan de client die de bevoegdheid om te beginnen met het maken van blokken, krijgt het. Als de host virtuele machine er een storing optreedt, worden de identiteit lease vrijgegeven, zodat een knooppunt vervanging van de identiteit in de toekomst te hervatten.

### <a name="bootnode-registrar"></a>Bootnode registrar

Om in te schakelen het gemak van connectiviteit, elk lid van de host fungeert voor een set met informatie over de verbinding op de [gegevens API-eindpunt](#data-api). Deze gegevens omvatten een lijst met bootnodes die zijn opgegeven als peering-knooppunten voor een lid van het lid te worden. Als onderdeel van deze gegevens API houden we deze lijst bootnode up-to-date

### <a name="bring-your-own-operator"></a>Breng uw eigen operator

Vaak is lid van een consortium wilt deelnemen aan netwerk governance maar niet wilt gebruiken en onderhouden van de infrastructuur. In tegenstelling tot traditionele systemen, met een één-operator in het netwerk werkt op basis van de gedecentraliseerd model van blockchain-systemen. In plaats van een gecentraliseerde tussenpersoon functioneren van een netwerk bent, kan elk lid van de consortium infrastructuurbeheer aan de operator van hun keuze delegeren. Hiermee wordt een hybride model waarin elk lid van de eigen infrastructuur of bewerking is een andere partner delegeren kunt kiezen. De werkstroom gedelegeerde bewerking werkt als volgt:

1.  **Consortium lid** genereert een Ethereum-adres (een persoonlijke sleutel bevat)

2.  **Consortium lid** biedt openbare Ethereum-adres **Operator**

3.  **Operator** implementeert en configureert u de PoA validator knooppunten met behulp van onze Azure Resource Manager-oplossing

4.  **Operator** biedt de RPC- en eindpunt naar **Consortium lid**

5.  **Consortium lid** hun persoonlijke sleutel gebruikt voor het ondertekenen van een aanvraag accepteren van de knooppunten validator **Operator** om deel te nemen namens hen is geïmplementeerd

### <a name="azure-monitor"></a>Azure Monitor

Deze oplossing wordt geleverd met Azure Monitor voor het bijhouden van statistieken van knooppunt en het netwerk. Voor ontwikkelaars van toepassingen biedt dit inzicht in de onderliggende blockchain om bij te houden blok genereren van statistieken. Netwerkoperators kunnen Azure Monitor gebruiken om snel te detecteren en te voorkomen dat netwerkstoringen via waarop Logboeken en statistieken van de infrastructuur. Zie voor meer informatie, [-Service controleren](#service-monitoring).

### <a name="deployment-architecture"></a>Implementatie-architectuur

#### <a name="description"></a>Description

Deze oplossing kunt implementeren één of meerdere regio's op basis van meerdere leden Ethereum consortium network. Standaard zijn de RPC- en peering eindpunten toegankelijk via openbare IP-adres om eenvoudig verbindingen tussen abonnementen en clouds. Het is raadzaam gebruik te maken van [pariteit van rollen contracten](https://wiki.parity.io/Permissioning) niveau voor toepassing besturingselementen voor toegang. We ondersteunen ook netwerken die zijn geïmplementeerd achter VPN's, die gebruikmaken van VNet-gateways voor abonnementoverschrijdende connectiviteit. Deze implementaties zijn complexer worden, zodat het wordt aanbevolen om te beginnen met het openbare IP-model eerst.

#### <a name="consortium-member-overview"></a>Overzicht van consortium lid

De implementatie van elke consortium lid bevat:

-   Virtuele Machines voor het uitvoeren van de validators PoA

-   Azure Load Balancer voor het distribueren van RPC-peering en Governance DApp aanvragen

-   Azure Key Vault voor het beveiligen van de validatie van identiteiten

-   Azure Storage voor permanente netwerkgegevens hosten en de coördinatie van overdracht

-   Azure Monitor voor het verzamelen van Logboeken en statistieken over de prestaties

-   VNet-Gateway die (optioneel) zijn voor het VPN-verbindingen toestaan via persoonlijke vnet 's

![Implementatie-architectuur](./media/ethereum-poa-deployment/deployment-architecture.png)

We maken gebruik van Docker-containers voor betrouwbaarheid en modulariteit mogelijk te maken. We gebruiken Azure Container Registry te hosten en versies installatiekopieën fungeren als onderdeel van elke implementatie. De containerinstallatiekopieën bestaan uit:

-   Orchestrator

    -   Wordt eenmaal uitgevoerd tijdens de implementatie

    -   Genereert identiteiten en beheer contracten

    -   Winkels identiteiten in identiteit Store

-   Pariteit Client

    -   Identiteit van de identiteit Store leases

    -   Detecteert en maakt verbinding met collega 's

-   EthStats Agent

    -   Lokale logboeken en -statistieken verzamelt via RPC en gepusht naar Azure Monitor

-   Governance DApp

    -   Web-interface voor interactie met Governance-opdrachten

## <a name="how-to-guides"></a>Handleidingen
### <a name="governance-dapp"></a>Governance DApp

Is de kern van proof-of-authority gedecentraliseerde governance. De governance DApp een set is vooraf geïmplementeerd [slimme contracten](https://github.com/Azure-Samples/blockchain/tree/master/ethereum-on-azure/) en een webtoepassing die worden gebruikt om te bepalen van de instanties in het netwerk.
Instanties zijn verdeeld in beheerder-id's en validatie van knooppunten.
Beheerders hebben de mogelijkheid om te delegeren consensus deelname aan een set Validator knooppunten. Beheerders kunnen stemmen andere beheerders in of buiten het netwerk.

![governance dapp](./media/ethereum-poa-deployment/governance-dapp.png)

-   **Governance - gedecentraliseerde** wijzigingen in de netwerk-instanties worden beheerd via het in de chain stemmen door beheerders van selecteren.

-   **Validatie van delegatie -** instanties hun validator knooppunten die zijn ingesteld in elke implementatie PoA kunnen beheren.

-   **De wijzigingsgeschiedenis van controleerbare -** elke wijziging is geregistreerd op de blockchain transparantie en controleerbaarheid te geven.

#### <a name="getting-started-with-governance"></a>Aan de slag met governance
Als u elk soort transacties via de DApp Governance, moet u gebruikmaken van een Ethereum-wallet.  De eenvoudigste manier is om te gebruiken, zoals een in de browser wallet [MetaMask](https://metamask.io), maar omdat dit slimme contracten geïmplementeerd op het netwerk u ook het gebruik van het contract Governance kan automatiseren.

Na de installatie van MetaMask, gaat u naar de DApp Governance in de browser.  U vindt de URL in de implementatie bevestigingse-mail of via Azure portal in de uitvoer van de implementatie.  Als u een in de browser wallet geïnstalleerd geen hebt u waarschijnlijk niet alle acties; uitvoeren echter nog steeds vindt u de status van de beheerder.  

#### <a name="becoming-an-admin"></a>Een beheerder uw account
Als u het eerste lid dat geïmplementeerd op het netwerk bent, klikt u vervolgens automatisch raakt u een beheerder en uw pariteit knooppunten worden vermeld als Systeemstatuscontrolepunten.  Als u bent lid wordt van het netwerk, moet u ophalen gestemd als beheerder door een meerderheid (groter dan 50%) van de bestaande set van de beheerder.  Als u ervoor kiest niet te worden van een beheerder en vervolgens uw knooppunten wordt nog steeds synchroniseren en valideren van de blockchain; Deze wordt echter niet opgenomen in het maakproces blokkeren. Voor het starten van de stemmende proces om te worden van een beheerder, klikt u op __Nominate__ en geef uw Ethereum-adres en de alias.

![Benoemen](./media/ethereum-poa-deployment/governance-dapp-nominate.png)

#### <a name="candidates"></a>Kandidaten
Selecteren van de __kandidaten__ tabblad ziet u de huidige reeks candidate beheerders.  Wanneer een kandidaat een stem meerderheid van de huidige beheerders is bereikt, ophalen de uiteindelijke gepromoot naar een beheerder.  Selecteer de rij om te stemmen op een kandidaat, en klik op 'Stem ' aan de bovenkant.  Als u van op een stem gedachten, kunt u de uiteindelijke selecteren en klikt u op 'Rescind stem'.

![Kandidaten](./media/ethereum-poa-deployment/governance-dapp-candidates.png)


#### <a name="admins"></a>Beheerders
De __beheerders__ tabblad wordt de huidige reeks beheerders weergeven en bieden u de mogelijkheid om te stemmen op basis van.  Wanneer een beheerder meer verliest dan 50% ondersteuning, ze als een beheerder zijn van het netwerk moeten worden verwijderd.  Validatie van knooppunten die eigendom zijn van deze beheerder wordt validator status verloren en transactie-knooppunten in het netwerk.  Een beheerder kan worden verwijderd voor een willekeurig aantal redenen; echter, het is aan het consortium om van tevoren een beleid akkoord te gaan.

![Beheerders](./media/ethereum-poa-deployment/governance-dapp-admins.png)

#### <a name="validators"></a>Systeemstatuscontrolepunten
Selecteren van de __Validators__ tabblad in het menu links wordt de huidige geïmplementeerde pariteit knooppunten voor dit exemplaar en de huidige status (knooppunttype) worden weergegeven.  Elk lid van de consortium heeft een andere set validators in deze lijst, omdat deze weergave de huidige geïmplementeerde consortium lid vertegenwoordigt.  Als dit een zojuist geïmplementeerde exemplaar en u uw validators nog niet hebt toegevoegd, ziet u de optie voor 'Validators toevoegen'.  Als u deze wordt automatisch kiezen van een set regionaal met gelijke taakverdeling van pariteit knooppunten en deze toewijzen aan uw verzameling validator.  Als u meer knooppunten dan de toegestane capaciteit hebt geïmplementeerd, worden de resterende knooppunten transactie knooppunten in het netwerk.

Het adres van elke validator wordt automatisch toegewezen de [identiteitsarchief](#identity-store) in Azure.  Als een knooppunt uitvalt, af het te zien de identiteit, zodat een ander knooppunt in uw implementatie moet de plaatsvinden.  Dit zorgt ervoor dat uw deelname consensus maximaal beschikbaar is.

![Systeemstatuscontrolepunten](./media/ethereum-poa-deployment/governance-dapp-validators.png)

#### <a name="consortium-name"></a>De naam van de Consortium
Een beheerder kan de naam van de Consortium, weergegeven aan de bovenkant van de pagina bijgewerkt.  Selecteer het tandwielpictogram in de linkerbovenhoek om bij te werken van de naam van de Consortium.

#### <a name="account-menu"></a>Menu Account
Is uw Ethereum-accountalias en identicon in de rechterbovenhoek.  Als u een beheerder bent hebt u de mogelijkheid uw alias bij te werken.

![Account](./media/ethereum-poa-deployment/governance-dapp-account.png)

### <a name="deploy-ethereum-proof-of-authority"></a>Ethereum Proof-of-Authority implementeren

Hier volgt een voorbeeld van een implementatie van meerdere partijen-stroom:

1.  Drie leden een Ethereum-account met behulp van MetaMask genereren

2.  *Lid A* Ethereum PoA, bieden hun Ethereum openbaar adres wordt geïmplementeerd

3.  *Lid A* biedt u de URL van consortium *lid B* en *lid C*

4.  *Lid B* en *lid C* implementeert, Ethereum PoA, bieden hun openbare Ethereum-adres en *lid A*van consortium URL

5.  *Lid A* stemmen in *lid B* als beheerder

6.  *Lid A* en *lid B* beide stem *lid C* als beheerder

Dit proces vereist een Azure-abonnement dat u kunt ondersteuning voor de implementatie van meerdere virtuele machines en beheerde schijven. Indien nodig, [maken van een gratis Azure-account](https://azure.microsoft.com/free/) om te beginnen.

Nadat een abonnement is beveiligd, gaat u naar Azure portal. Selecteer '+', Marketplace ('Zie alle'), en zoek naar Ethereum PoA Consortium.

De volgende sectie helpen u bij het configureren van het eerste lid footprint in het netwerk. De stroom van de implementatie is onderverdeeld in vijf stappen: Basisbeginselen, regio's voor implementatie, netwerk-grootte en prestaties, Ethereum-instellingen, Azure Monitor.

#### <a name="basics"></a>Basisbeginselen

Onder **basisbeginselen**, waarden opgeven voor standard parameters voor elke implementatie, zoals abonnement, resourcegroep en virtuele-eigenschappen.

Hier volgt een gedetailleerde beschrijving van elke parameter:

Parameternaam|Description|Toegestane waarden|Standaardwaarden
---|---|---|---
Maak een nieuwe netwerk- of toevoegen aan bestaande netwerk?|Een nieuw netwerk maken of lid van een bestaande consortium network|Nieuwe Join bestaande maken|Create New
E-mailadres (optioneel)|U ontvangt een e-mailmelding wanneer uw implementatie is voltooid met informatie over uw implementatie.|Geldig e-mailadres|N.v.t.
VM-gebruikersnaam|Gebruikersnaam voor de beheerder van elke geïmplementeerde VM (alleen alfanumerieke tekens)|1 tot 64 tekens|N.v.t.
Verificatietype|De methode voor verificatie bij de virtuele machine.|Wachtwoord of SSH de openbare sleutel|Wachtwoord
Wachtwoord (verificatietype = wachtwoord)|Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd.  Het wachtwoord moet 3 van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|12-72 tekens|N.v.t.
SSH-sleutel (verificatietype = openbare sleutel)|De veilige shell-sleutel die wordt gebruikt voor externe aanmelding.||N.v.t.
Abonnement|Het abonnement waaraan het implementeren van het netwerk consortium||N.v.t.
Resourcegroep|De resourcegroep waaraan de consortium network implementeren.||N.v.t.
Locatie|De Azure-regio voor de resourcegroep.||N.v.t.

Een voorbeeldimplementatie wordt hieronder weergegeven: ![basic blade](./media/ethereum-poa-deployment/basic-blade.png)

#### <a name="deployment-regions"></a>Implementatie-regio 's

Geef vervolgens onder implementatie regio's, invoer voor het aantal regio('s) implementeren de consortium network en de selectie van Azure-regio's op basis van het aantal regio's die zijn opgegeven. Gebruiker kunt implementeren in maximaal 5 regio's. Het is raadzaam om de eerste regio zodat deze overeenkomen met de locatie voor de resourcegroep uit de sectie over de grondbeginselen te kiezen. Een enkele regio per lid wordt aanbevolen voor ontwikkelings- of -netwerken. Voor de productie, is het raadzaam implementeren in twee of meer regio's voor hoge beschikbaarheid.

Hier volgt een gedetailleerde beschrijving van elke parameter:

  Parameternaam|Description|Toegestane waarden|Standaardwaarden
  ---|---|---|---
  Aantal regio('s)|Aantal regio's om de consortium-netwerk te implementeren|1, 2, 3, 4, 5|1
  Eerste regio|Eerste regio voor de implementatie van het netwerk consortium|Alle toegestaan Azure-regio 's|N.v.t.
  Tweede regio|Tweede regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 2)|Alle toegestaan Azure-regio 's|N.v.t.
  Derde regio|Derde regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 3)|Alle toegestaan Azure-regio 's|N.v.t.
  Vierde regio|Vierde regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 4)|Alle toegestaan Azure-regio 's|N.v.t.
  Vijfde regio|Vijfde regio voor de implementatie van het netwerk consortium (Visible alleen wanneer het aantal regio's is geselecteerd als 5)|Alle toegestaan Azure-regio 's|N.v.t.

Een voorbeeldimplementatie wordt hieronder weergegeven: ![implementatie regio's](./media/ethereum-poa-deployment/deployment-regions.png)

#### <a name="network-size-and-performance"></a>Netwerkgrootte en prestaties

Geef vervolgens invoer voor de grootte van het netwerk consortium, zoals aantal en de grootte van de validatiefunctie knooppunten onder 'netwerkgrootte en prestaties'.
De mogelijke grootte van de blockchain worden bepaald door de grootte van de validatie van knooppunt. Dit kan worden gewijzigd na de implementatie.

Hier volgt een gedetailleerde beschrijving van elke parameter:

  Parameternaam|Description|Toegestane waarden|Standaardwaarden
  ---|---|---|---
  Het aantal knooppunten met gelijke validator|Het aantal knooppunten validator om in te richten als onderdeel van het netwerk|2-15|2
  Validatie van knooppunt-opslagprestaties|Het type beheerde schijf een back-up elk van de validatie van geïmplementeerde knooppunten.|Standard-SSD- of Premium|Standard - SSD
  De grootte van de virtuele machine knooppunt Validator|De grootte van de virtuele machine wordt gebruikt voor validatie van knooppunten.|Standard A Standard D, Standard D-v2, Standard F-serie, Standard DS en Standard FS|Standard D1 v2

[Prijsinformatie voor opslag](https://azure.microsoft.com/pricing/details/managed-disks/)

[Prijsinformatie voor virtuele Machine](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)

Virtuele Machine en Storage-laag heeft invloed op de prestaties van het netwerk.  U wordt aangeraden de volgende SKU's op basis van de gewenste kostenefficiënt:

  Virtuele Machine SKU|Storage-laag|Prijs|Doorvoer|Latentie
  ---|---|---|---|---
  F1|Standard - SSD|laag|laag|Hoog
  D2_v3|Standard - SSD|gemiddeld|gemiddeld|gemiddeld
  F16s|Premium SSD|Hoog|Hoog|laag

Een voorbeeldimplementatie wordt hieronder weergegeven: ![netwerk grootte en prestaties](./media/ethereum-poa-deployment/network-size-and-performance.png)

#### <a name="ethereum-settings"></a>Ethereum-instellingen

Geef vervolgens onder Ethereum-instellingen, Ethereum-gerelateerde configuratie-instellingen, zoals de netwerk-ID en Ethereum-account wachtwoord of genesis blokkeren.

Hier volgt een gedetailleerde beschrijving van elke parameter:

  Parameternaam|Description|Toegestane waarden|Standaardwaarden
  ---|---|---|---
Consortium lid-ID|De ID die is gekoppeld aan elk lid van die deel uitmaken van het consortium netwerk gebruikt voor het configureren van IP-adresruimten om te voorkomen van conflicten. Lid-ID moet in het geval van een particulier netwerk, uniek zijn in verschillende organisaties in hetzelfde netwerk.  Een unieke lid-ID is vereist, zelfs wanneer dezelfde organisatie wordt geïmplementeerd in meerdere regio's. Noteer de waarde van deze parameter omdat, moet u deze delen met andere leden van de gekoppelde om ervoor te zorgen dat er geen conflicten.|0-255|N.v.t.
Netwerk-id|De netwerk-ID voor het consortium Ethereum-netwerk worden geïmplementeerd.  Elk Ethereum-netwerk heeft een eigen netwerk-ID, met 1 wordt de ID voor het openbare netwerk.|5 - 999,999,999|10101010
Ethereum-mailadres van beheerder|Het adres Ethereum die wordt gebruikt voor uw deelname aan PoA governance.  We adviseren MetaMask gebruiken voor het genereren van een Ethereum-adres.|42 alfanumerieke tekens beginnen met 0 x|N.v.t.
Geavanceerde opties|Geavanceerde opties voor Ethereum-instellingen|In- of uitschakelen|Uitschakelen
Openbaar IP-adres (geavanceerde opties = inschakelen)|Het netwerk achter een VNet-Gateway implementeert en peering toegang verwijderd. Als deze optie is geselecteerd, moeten alle leden van een VNet-Gateway voor de verbinding gebruiken om compatibel te zijn.|Openbaar IP-adres privé VNet|Openbare IP
Limiet voor Gas blokkeren (geavanceerde opties = inschakelen)|De eerste blok gas limiet van het netwerk|Een numerieke waarde|50,000,00
Verzegelen Blokkeringsperiode (sec)|De frequentie waarmee leeg blokken worden gemaakt wanneer er geen transacties in het netwerk zijn. Een hogere frequentie hebben sneller definitieve karakter maar hogere opslagkosten.|Een numerieke waarde|15
Transactie machtiging Contract (geavanceerde opties = inschakelen)|Bytecode voor de transactie-rollen-contract. Hiermee beperkt u de slimme contract implementatie en uitvoering aan een lijst met toegestane van Ethereum-accounts.|Contract bytecode|N.v.t.

Een voorbeeldimplementatie wordt hieronder weergegeven: ![ethereum-instellingen](./media/ethereum-poa-deployment/ethereum-settings.png)

#### <a name="monitoring"></a>Bewaking

De blade controle kunt u een resource van Azure Monitor-logboeken voor uw netwerk te configureren. De monitoring agent worden verzameld en surface nuttig metrische gegevens en logboeken van uw netwerk, biedt de mogelijkheid om snel te controleren de netwerkstatus of foutopsporing problemen.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

  Parameternaam|Description|Toegestane waarden|Standaardwaarden
  ---|---|---|---
Bewaking|Optie voor het inschakelen van bewaking|In- of uitschakelen|Inschakelen
Verbinding maken met bestaande Azure Monitor-Logboeken|Maak een nieuw exemplaar van Azure Monitor Logboeken of Word lid van een bestaand exemplaar|Maak een nieuwe of bestaande koppelen|Nieuwe maken
Locatie controleren (verbinding maken met bestaande Azure Monitor-logboeken = maken nieuw)|De regio waar de nieuwe Azure Monitor-exemplaar Logboeken wordt geïmplementeerd|Monitor voor alle Azure-regio's Logboeken|N.v.t.
Bestaande log analytics-werkruimte-ID (verbinding maken met bestaande Azure Monitor-logboeken = bestaande Join)|Werkruimte-ID van de bestaande Azure Monitor-logboeken exemplaar||N.v.t.
Primaire sleutel van bestaande log analytics (verbinding maken met bestaande Azure Monitor-logboeken = bestaande Join)|De primaire sleutel gebruikt voor verbinding met de bestaande instantie van de Azure Monitor-Logboeken||N.v.t.


Een voorbeeldimplementatie wordt hieronder weergegeven: ![met azure monitor](./media/ethereum-poa-deployment/azure-monitor.png)

#### <a name="summary"></a>Samenvatting

Klik op de blade samenvatting om te controleren van de invoer die is opgegeven en uit te voeren basisvalidatie vóór de implementatie. U kunt de sjabloon en parameters downloaden voordat u implementeert.

Wettelijke informatie en privacy voorwaarden bekijken en klik op 'Inkoop' om te implementeren. Als de implementatie van VNet-Gateways bevat, wordt de implementatie van 45 tot 50 minuten duren.

#### <a name="post-deployment"></a>Na de implementatie

##### <a name="deployment-output"></a>Implementatie-uitvoer

Als de implementatie is voltooid, kunt u toegang tot de benodigde parameters via de bevestigingse-mail of via de Azure-portal. In deze parameters vindt u het:

-   Ethereum RPC-eindpunt

-   Governance-Dashboard-URL

-   Azure Monitor URL

-   Gegevens-URL

-   VNet-Gateway Resource-ID (optioneel)

##### <a name="confirmation-email"></a>Bevestigingse-mail

Als u een e-mailadres opgeven ([basisbeginselen sectie](#basics)), een e-mailbericht naar het e-mailadres met de informatie over de implementatie-uitvoer worden verzonden.

![implementatie van e-mailadres](./media/ethereum-poa-deployment/deployment-email.png)

##### <a name="portal"></a>Portal

Zodra de implementatie is voltooid en alle resources zijn ingericht, kunt u de output-parameters kunt weergeven in de resourcegroep.

1.  Zoek de resourcegroep in de portal

2.  Navigeer naar *implementaties*

3.  Selecteer de bovenste implementatie met dezelfde naam als uw resourcegroep

4.  Selecteer *uitvoer*

### <a name="growing-the-consortium"></a>Het consortium groeiende

Als u wilt uw consortium wilt uitbreiden, moet u eerst het fysieke netwerk te verbinden.
Deze eerste stap is met behulp van de implementatie op basis van een openbaar IP-adres naadloze. Als achter een VPN-verbinding implementeert, Zie de sectie [verbinding te maken van de VNet-Gateway](#connecting-vnet-gateways) te doen de netwerkverbinding als onderdeel van de implementatie van het nieuwe lid.  Nadat uw implementatie gebruikt de [Governance DApp](#governance-dapp) om te worden van een netwerk-beheerder.

#### <a name="new-member-deployment"></a>Nieuwe implementatie van het lid

1.  De volgende informatie delen met het gekoppelde lid. Deze informatie kan worden gevonden in uw e-mail na de implementatie of in de uitvoer van de portal-implementatiescenario.

    -  Consortium Data Url

    -  Het aantal knooppunten dat u hebt geïmplementeerd

    -  VNet-Gateway Resource-ID (als u VPN)

2.  Het lid van de implementatie moet gebruiken de [dezelfde oplossing](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) bij het implementeren van hun aanwezigheid netwerk met de volgende waarmee u rekening moet houden:

    -  Selecteer *deelnemen aan bestaande*

    -  Kies de hetzelfde aantal validator knooppunten als de rest van de leden in het netwerk om ervoor te zorgen geoorloofd weergave

    -  Gebruik hetzelfde Ethereum-adres dat is opgegeven in de vorige stap

    -  In de opgegeven doorgeven *Consortium gegevens Url* op de *Ethereum-instellingen* tabblad

    -  Als de rest van het netwerk zich achter een VPN-verbinding, selecteert u *privé VNet* onder de sectie Geavanceerd

#### <a name="connecting-vnet-gateways"></a>VNet-gateways verbinden

U kunt deze stap overslaan als u hebt geïmplementeerd met de standaardinstellingen van het openbare IP-adres. In het geval van een particulier netwerk, worden de verschillende leden zijn verbonden via VNet-gateway-verbindingen. Voordat een lid kunt deelnemen aan het netwerk en transactie-verkeer ziet, moet een bestaand lid een definitieve configuratie op de VPN-gateway om te accepteren van de verbinding doen. Dit betekent dat de Ethereum-knooppunten van het gekoppelde lid niet uitgevoerd totdat een verbinding tot stand is gebracht. Het is raadzaam om te maken van redundante netwerkverbindingen (mesh) in het consortium te verminderen de kans op een single point of failure.

Nadat het nieuwe lid heeft geïmplementeerd, moet het lid van de bestaande de bidirectionele verbinding voltooien door het instellen van een VNet-gateway-verbinding met het nieuwe lid. Om dit te bereiken, moeten de bestaande lid:

1.  De VNet-gateway ResourceID van het lid dat verbinding maakt (Zie de uitvoer van de implementatie)

2.  De sleutel gedeelde verbinding

Het lid van de bestaande moet het volgende PowerShell-script voor het voltooien van de verbinding uitvoeren. Het is raadzaam om met behulp van Azure Cloud Shell zich in de rechterbovenhoek navigatiebalk in de portal.

![cloudshell](./media/ethereum-poa-deployment/cloud-shell.png)

```Powershell
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzureRmSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzureRmVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

### <a name="service-monitoring"></a>Servicecontrole

U kunt uw Azure Monitor-portal door de koppeling in de implementatie van e-mailadres of zoeken naar de parameter in de uitvoer van de implementatie vinden \[OMS\_PORTAL\_URL\].

De portal wordt eerst de statistieken en knooppunt overzicht op hoog niveau netwerk weergegeven.

![monitor-categorieën](./media/ethereum-poa-deployment/monitor-categories.png)

Selecteren **knooppunt overzicht** wordt u doorgeleid naar een portal voor statistieken van de infrastructuur per knooppunt weergeven.

![knooppunt-statistieken](./media/ethereum-poa-deployment/node-stats.png)

Selecteren **netwerk statistieken** leidt u Ethereum netwerkstatistieken weergeven.

![netwerk-statistieken](./media/ethereum-poa-deployment/network-stats.png)

#### <a name="sample-kusto-queries"></a>Voorbeeld Kusto-query 's

Is een set waarop ruwe logboeken achter deze dashboards. Deze onbewerkte Logboeken kunt u pas de dashboards, mislukte pogingen onderzoeken of drempelwaarde voor waarschuwingen instellen. Hieronder vindt u een aantal voorbeelden van query's die kunnen worden uitgevoerd in het hulpprogramma voor zoeken in Logboeken:

##### <a name="lists-blocks-that-have-been-reported-by-more-than-one-validator-useful-to-help-find-chain-forks"></a>Geeft een lijst van blokken die zijn gemeld door meer dan één validator. Dit is handig om te zoeken naar hoofddomein keten.

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

##### <a name="get-average-peer-count-for-a-specified-validator-node-averaged-over-5-minute-buckets"></a>Gemiddelde peer aantal krijgen buckets voor gemiddelde meer dan vijf minuten een validatie van het opgegeven knooppunt.

```sql
let PeerCountRegex = @"Syncing with peers: (\d+) active, (\d+) confirmed, (\d+)";
ParityLog_CL
| where Computer == "vl-devn3lgdm-reg1000001"
| project RawData, TimeGenerated
| where RawData matches regex PeerCountRegex
| extend ActivePeers = extract(PeerCountRegex, 1, RawData, typeof(int))
| summarize avg(ActivePeers) by bin(TimeGenerated, 5m)
```

### <a name="ssh-access"></a>SSH-toegang

Uit veiligheidsoverwegingen, de toegang tot de SSH-poort is standaard niet toegestaan door een regel voor de beveiliging. Voor toegang tot de virtuele machine-exemplaren in het netwerk PoA, moet u deze regel voor het wijzigen \"toestaan\"

1.  Start in de sectie overzicht van de geïmplementeerde resourcegroep vanuit Azure portal.

    ![SSH overzicht](./media/ethereum-poa-deployment/ssh-overview.png)

2.  De Netwerkbeveiligingsgroep voor de regio van de virtuele machine die u voor toegang tot uw selecteren

    ![SSH nsg](./media/ethereum-poa-deployment/ssh-nsg.png)

3.  Selecteer de \"toestaan-ssh\" regel

    ![ssh-allow](./media/ethereum-poa-deployment/ssh-allow.png)

4.  Wijziging \"actie\" om toe te staan

    ![SSH inschakelen toestaan](./media/ethereum-poa-deployment/ssh-enable-allow.png)

5.  Klik op \"opslaan\" (wijzigingen duurt een paar minuten om toe te passen)

U kunt nu op afstand verbinding maken met de virtuele machines voor de validatie van knooppunten via SSH met uw beheerder opgegeven gebruikersnaam en het wachtwoord/SSH-sleutel.
De SSH-opdracht uit te voeren om toegang tot het eerste knooppunt van de validator wordt vermeld in de sjabloon implementatie output-parameter als ' SSH\_aan\_eerste\_VL\_knooppunt\_Regio1 ' (voor de Voorbeeldimplementatie: ssh -p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com). Verhogen om door te gaan naar de knooppunten van extra transactielogboeken, het poortnummer dat door een (bijvoorbeeld het eerste knooppunt van de transactie is op poort 4000).

Als u meer dan één regio geïmplementeerd, wijzigt u de bovenstaande opdracht aan de DNS-naam of IP-adres van de load balancer in die regio. Als u zoekt de DNS-naam of IP-adres van de andere regio's, vinden de resource met de naamconventie \* \* \* \* \*- lbpip reg\#, en bekijk de DNS-naam en het IP-eigenschappen.

### <a name="azure-traffic-manager-load-balancing"></a>Azure Traffic Manager voor taakverdeling

Met Azure Traffic Manager kunt u downtime verminderen en de reactietijd van het netwerk PoA verbeteren door Routering van inkomend verkeer meerdere implementaties in verschillende regio's. Ingebouwde statuscontroles en automatische een andere vlucht helpen hoge beschikbaarheid van de RPC-eindpunten en de DApp Governance. Deze functie is handig als u hebt geïmplementeerd in meerdere regio's en klaar voor productie zijn.

Met Traffic Manager kunt u:

-   De PoA netwerkbeschikbaarheid met automatische failover.

-   Verhoog de reactiesnelheid van uw netwerken routering kunnen eindgebruikers tot Azure-locatie met de laagste netwerklatentie.

Als u besluit om een Traffic Manager-profiel te maken, kunt u de DNS-naam van het profiel gebruiken voor toegang tot uw netwerk. Eenmaal andere consortium leden zijn toegevoegd aan het netwerk, Traffic Manager kan ook worden gebruikt voor taakverdeling tussen hun geïmplementeerde systeemstatuscontrolepunten.

#### <a name="creating-a-traffic-manager-profile"></a>Het maken van een Traffic Manager-profiel

Zoek en selecteer \"Traffic Manager-profiel\" na het klikken op de \"een resource maken\" knop in de Azure portal.

![zoekt u azure traffic manager](./media/ethereum-poa-deployment/traffic-manager-search.png)

Geef het profiel een unieke naam en selecteer de resourcegroep die is gemaakt tijdens de implementatie PoA. Klik op de knop 'Maken' om te implementeren.

![traffic manager maken](./media/ethereum-poa-deployment/traffic-manager-create.png)

Zodra deze geïmplementeerd, selecteert u het exemplaar in de resourcegroep. De DNS-naam voor toegang tot de traffic manager kunt u vinden in het tabblad Overzicht

![Ga naar traffic manager DNS](./media/ethereum-poa-deployment/traffic-manager-dns.png)

Selecteer het tabblad eindpunten en klik op de knop toevoegen. Geef het eindpunt een unieke naam. Wijzigen van het doelbrontype naar openbare IP-adres. Selecteer vervolgens het openbare IP-adres van de eerste regio\'s load balancer.

![Routering traffic manager](./media/ethereum-poa-deployment/traffic-manager-routing.png)

Herhalen voor elke regio in de geïmplementeerde netwerk. Zodra de eindpunten zijn de \"ingeschakeld\" status, ze automatisch worden laden en de regio met gelijke taakverdeling op de DNS-naam van traffic manager. U kunt nu deze DNS-naam in plaats van de \[CONSORTIUM\_gegevens\_URL\] parameter in de overige stappen van het document.

### <a name="data-api"></a>Gegevens-API

Elk lid van de consortium als host fungeert voor de benodigde gegevens voor andere gebruikers verbinding maken met het netwerk. Het lid van de bestaande biedt de [CONSORTIUM_DATA_URL] vóór de implementatie van het lid. Na de implementatie, worden lid van een lid te worden gegevens opgehaald uit de JSON-interface op het eindpunt van de volgende:

`<CONSORTIUM_DATA_URL>/networkinfo`

Het antwoord bevat informatie die nuttig zijn voor leden (Genesis blokkeren, validatie ingesteld contract ABI, bootnodes) en nuttige informatie toevoegen aan het bestaande lid (validator adressen). U wordt aangeraden gebruik van deze standaardisatie om uit te breiden het consortium voor cloudproviders. Deze API retourneert een JSON-indeling-antwoord met de volgende structuur:
```json
{
  "$id": "",
  "type": "object",
  "definitions": {},
  "$schema": "https://json-schema.org/draft-07/schema#",
  "properties": {
    "majorVersion": {
      "$id": "/properties/majorVersion",
      "type": "integer",
      "title": "This schema’s major version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "minorVersion": {
      "$id": "/properties/minorVersion",
      "type": "integer",
      "title": "This schema’s minor version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "bootnodes": {
      "$id": "/properties/bootnodes",
      "type": "array",
      "items": {
        "$id": "/properties/bootnodes/items",
        "type": "string",
        "title": "This member’s bootnodes",
        "default": "",
        "examples": [
          "enode://a348586f0fb0516c19de75bf54ca930a08f1594b7202020810b72c5f8d90635189d72d8b96f306f08761d576836a6bfce112cfb6ae6a3330588260f79a3d0ecb@10.1.17.5:30300",
          "enode://2d8474289af0bb38e3600a7a481734b2ab19d4eaf719f698fe885fb239f5d33faf217a860b170e2763b67c2f18d91c41272de37ac67386f80d1de57a3d58ddf2@10.1.17.4:30300"
        ]
      }
    },
    "valSetContract": {
      "$id": "/properties/valSetContract",
      "type": "string",
      "title": "The ValidatorSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\n\nimport \"./SafeMath.sol\";\nimport \"./Utils.sol\";\n\ncontract ValidatorSet …"
      ]
    },
    "adminContract": {
      "$id": "/properties/adminContract",
      "type": "string",
      "title": "The AdminSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\nimport \"./SafeMath.sol\";\nimport \"./SimpleValidatorSet.sol\";\nimport \"./Admin.sol\";\n\ncontract AdminValidatorSet is SimpleValidatorSet { …"
      ]
    },
    "adminContractABI": {
      "$id": "/properties/adminContractABI",
      "type": "string",
      "title": "The Admin Contract ABI",
      "default": "",
      "examples": [
        "[{\"constant\":false,\"inputs\":[{\"name\":\"proposedAdminAddress\",\"type\":\"address\"},…"
      ]
    },
    "paritySpec": {
      "$id": "/properties/paritySpec",
      "type": "string",
      "title": "The Parity client spec file",
      "default": "",
      "examples": [
        "\n{\n \"name\": \"PoA\",\n \"engine\": {\n \"authorityRound\": {\n \"params\": {\n \"stepDuration\": \"2\",\n \"validators\" : {\n \"safeContract\": \"0x0000000000000000000000000000000000000006\"\n },\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\"\n }\n }\n },\n \"params\": {\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\",\n \"wasmActivationTransition\": \"0x0\"\n },\n \"genesis\": {\n \"seal\": {\n \"authorityRound\": {\n \"step\": \"0x0\",\n \"signature\": \"0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\"\n }\n },\n \"difficulty\": \"0x20000\",\n \"gasLimit\": \"0x2FAF080\"\n },\n \"accounts\": {\n \"0x0000000000000000000000000000000000000001\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ecrecover\", \"pricing\": { \"linear\": { \"base\": 3000, \"word\": 0 } } } },\n \"0x0000000000000000000000000000000000000002\": { \"balance\": \"1\", \"builtin\": { \"name\": \"sha256\", \"pricing\": { \"linear\": { \"base\": 60, \"word\": 12 } } } },\n \"0x0000000000000000000000000000000000000003\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ripemd160\", \"pricing\": { \"linear\": { \"base\": 600, \"word\": 120 } } } },\n \"0x0000000000000000000000000000000000000004\": { \"balance\": \"1\", \"builtin\": { \"name\": \"identity\", \"pricing\": { \"linear\": { \"base\": 15, \"word\": 3 } } } },\n \"0x0000000000000000000000000000000000000006\": { \"balance\": \"0\", \"constructor\" : \"…\" }\n }\n}"
      ]
    },
    "errorMessage": {
      "$id": "/properties/errorMessage",
      "type": "string",
      "title": "Error message",
      "default": "",
      "examples": [
        ""
      ]
    },
    "addressList": {
      "$id": "/properties/addressList",
      "type": "object",
      "properties": {
        "addresses": {
          "$id": "/properties/addressList/properties/addresses",
          "type": "array",
          "items": {
            "$id": "/properties/addressList/properties/addresses/items",
            "type": "string",
            "title": "This member’s validator addresses",
            "default": "",
            "examples": [
              "0x00a3cff0dccc0ecb6ae0461045e0e467cff4805f",
              "0x009ce13a7b2532cbd89b2d28cecd75f7cc8c0727"
            ]
          }
        }
      }
    }
  }
}

```
## <a name="tutorials"></a>Zelfstudies

### <a name="programmatically-interacting-with-a-smart-contract"></a>Via een programma interactie met een slimme contract

> [!WARNING]
> De persoonlijke sleutel van uw Ethereum nooit verzenden via het netwerk. Zorg ervoor dat elke transactie is lokaal ondertekend eerst en de ondertekende transactie wordt verzonden via het netwerk.

In het volgende voorbeeld gebruiken we *ethereumjs wallet* voor het genereren van een adres Ethereum *ethereumjs tx* lokaal aanmelden en *web3* voor het verzenden van de onbewerkte transactie naar de Ethereum RPC-eindpunt.

In dit voorbeeld gebruiken we dit eenvoudige Hello-World slimme contract:

```javascript
pragma solidity ^0.4.11;
contract postBox {
    string message;
    function postMsg(string text) public {
        message = text;
    }
    function getMsg() public view returns (string) {
        return message;
    }
}
```

In dit voorbeeld wordt ervan uitgegaan dat het contract al is geïmplementeerd. U kunt *solc* en *web3* voor het implementeren van een contract via een programma. Installeer eerst de volgende node-modules:
```
sudo npm install web3@0.20.2
sudo npm install ethereumjs-tx@1.3.6
sudo npm install ethereumjs-wallet@0.6.1
```
Dit nodeJS-script wordt het volgende doen:

-   Maken van een onbewerkte transactie: postMsg

-   Meld u aan de transactie met behulp van de gegenereerde persoonlijke sleutel

-   De ondertekende transactie met het netwerk Ethereum verzenden

```javascript
var ethereumjs = require('ethereumjs-tx')
var wallet = require('ethereumjs-wallet')
var Web3 = require('web3')

// TODO Replace with your contract address
var address = "0xfe53559f5f7a77125039a993e8d5d9c2901edc58";
var abi = [{"constant": false,"inputs": [{"name": "text","type": "string"}],"name": "postMsg","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "getMsg","outputs": [{"name": "","type": "string"}],"payable": false,"stateMutability": "view","type": "function"}];

// Generate a new Ethereum account
var account = wallet.generate();
var accountAddress = account.getAddressString()
var privateKey = account.getPrivateKey();

// TODO Replace with your RPC endpoint
var web3 = new Web3(new Web3.providers.HttpProvider(
    "http://testzvdky-dns-reg1.eastus.cloudapp.azure.com:8545"));

// Get the current nonce of the account
web3.eth.getTransactionCount(accountAddress, function (err, nonce) {
   var data = web3.eth.contract(abi).at(address).postMsg.getData("Hello World");
   var rawTx = {
     nonce: nonce,
     gasPrice: '0x00',
     gasLimit: '0x2FAF080',
     to: address,
     value: '0x00',
     data: data
   }
   var tx = new ethereumjs(rawTx);

   tx.sign(privateKey);

   var raw = '0x' + tx.serialize().toString('hex');
   web3.eth.sendRawTransaction(raw, function (txErr, transactionHash) {
     console.log("TX Hash: " + transactionHash);
     console.log("Error: " + txErr);
   });
 });
```

### <a name="deploy-smart-contract-with-truffle"></a>Slimme servicecontract met Truffle implementeren

-   Vereiste bibliotheken installeren

```javascript
npm init

npm install truffle-hdwallet-provider --save
```
-   Truffle.js, voeg de volgende code om uw account MetaMask ontgrendelen en het knooppunt PoA configureren als invoer door te geven van het symbool woordgroep (MetaMask / instellingen / Seed woorden weergeven)

```javascript
var HDWalletProvider = require("truffle-hdwallet-provider");

var rpc_endpoint = "XXXXXX";
var mnemonic = "twelve words you can find in metamask/settings/reveal seed words";

module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    },
    poa: {
      provider: new HDWalletProvider(mnemonic, rpc_endpoint),
      network_id: 3,
      gasPrice : 0
    }
  }
};

```

-   Implementeren naar PoA netwerk

```javascript
$ truffle migrate --network poa
```

### <a name="debug-smart-contract-with-truffle"></a>Fouten opsporen in slimme servicecontract met Truffle

Truffle heeft een lokale ontwikkelen-netwerk dat beschikbaar is voor het opsporen van fouten in slimme contract. U vindt de volledige zelfstudie [hier](https://truffleframework.com/tutorials/debugging-a-smart-contract).

### <a name="webassembly-wasm-support"></a>Ondersteuning voor WebAssembly (WASM)

Ondersteuning voor WebAssembly is al ingeschakeld voor u op de zojuist geïmplementeerde PoA netwerken. Hierdoor kan voor de ontwikkeling van smart-contract in elke taal die transpiles naar Web-Assembly (TDD, C, C++). Zie de onderstaande koppelingen voor meer informatie

-   Overzicht van WebAssembly - pariteit <https://wiki.parity.io/WebAssembly-Home>

-   Zelfstudie van pariteit Tech- <https://github.com/paritytech/pwasm-tutorial>

## <a name="reference"></a>Referentie

### <a name="faq"></a>Veelgestelde vragen

#### <a name="i-notice-there-are-many-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>Ik zie dat er veel transacties in het netwerk zijn die ik komen\'t verzenden. Waar deze vandaan?

Het is onveilig voor het ontgrendelen van het [persoonlijke API](https://web3js.readthedocs.io/en/1.0/web3-eth-personal.html). Bots luisteren voor niet-vergrendelde Ethereum-accounts en verwijderen uit de bestaande fondsen proberen. De bot wordt ervan uitgegaan dat deze accounts real-ether bevatten en proberen om het saldo siphon als eerste. Schakel de persoonlijke API op het netwerk niet. Vooraf zich in plaats daarvan de transacties handmatig met behulp van een portefeuille zoals MetaMask of via een programma zoals wordt beschreven in de sectie [via een programma interactie met een slimme Contract](#programmatically-interacting-with-a-smart-contract).

#### <a name="how-to-ssh-onto-a-vm"></a>Hoe kunt u SSH naar een virtuele machine?

De SSH-poort is niet beschikbaar voor opmaaktalen wordt om beveiligingsredenen. Ga als volgt [deze handleiding voor het inschakelen van de SSH-poort](#ssh-access).

#### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>Hoe stel ik een audit lid of de transactie-knooppunten

Transactie-knooppunten zijn een set van pariteit-clients die zijn gekoppeld aan het netwerk, maar niet deelnemen aan consensus. Deze knooppunten kunnen indienen Ethereum-transacties en de contractstatus van de slimme lezen nog steeds worden gebruikt.
Dit werkt ook als mechanisme voor het bieden van controleerbaarheid aan niet-authority consortium leden in het netwerk. Voor het bereiken van dit gewoon volgt u stap 2 van het Consortium groeit.

#### <a name="why-are-metamask-transactions-taking-a-long-time"></a>Waarom zijn MetaMask transacties veel tijd nodig?

Om ervoor te zorgen voor transacties in de juiste volgorde worden ontvangen, wordt elke transactie Ethereum geleverd met een ophogende nonce. Als u een account hebt gebruikt in MetaMask op een ander netwerk, moet u de nonce waarde. Klik op het Instellingenpictogram (3-balken), instellingen, Account opnieuw instellen. De transactiegeschiedenis worden gewist en u kunt nu de transactie opnieuw.

#### <a name="do-i-need-to-specify-gas-fee-in-metamask"></a>Heb ik nodig om op te geven gas kosten in MetaMask?

Ether dienen niet een doel in proof-of-authority consortium. Er is daarom niet nodig om op te geven gas kosten bij het indienen van transacties in MetaMask.

#### <a name="what-should-i-do-if-my-deployment-fails-due-to-failure-to-provision-azure-oms"></a>Wat moet ik doen als mijn implementatie is mislukt vanwege een fout voor het inrichten van OMS in Azure?

Bewaking is een optionele functie. In sommige gevallen waar uw implementatie vanwege de modeldatabase groter om in te richten is resource van Azure Monitor mislukt kunt u opnieuw implementeren zonder Azure Monitor.

#### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>Openbare IP-implementaties compatibel zijn met het particuliere netwerkimplementaties?

Nee, voor peering moeten communicatie in twee richtingen, zodat het hele netwerk lid van openbare of privécloud zijn moet.

#### <a name="what-is-the-expected-transaction-throughput-of-proof-of-authority"></a>Wat is de verwachte transactie-doorvoer van Proof-of-Authority?

De transactiedoorvoer is sterk afhankelijk van de typen van transacties en de netwerktopologie.  Met behulp van eenvoudige transacties, hebben we een gemiddelde van 400 transacties per seconde benchmarked met een netwerk in meerdere regio's.

#### <a name="how-do-i-subscribe-to-smart-contract-events"></a>Hoe ik abonneren op slimme contract gebeurtenissen?

Ethereum Proof-of-Authority biedt nu ondersteuning voor web-sockets.  Controleer de implementatie van e-mailadres of de implementatie van de uitvoer naar de web socket-URL en de poort niet vinden.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met behulp van de [Ethereum Proof-of-Authority Consortium](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) oplossing.
