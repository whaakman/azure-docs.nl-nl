---
title: Hyperledger Fabric Consortium Network in Azure
description: Oplossingssjabloon implementeren en configureren van een Hyperledger Fabric consortium network
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 01/23/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: caleteet
manager: femila
ms.openlocfilehash: ce1afbd5499e798888e77f52d7b652e7e1f548fc
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005811"
---
# <a name="hyperledger-fabric-consortium-network"></a>Hyperledger Fabric consortium network

U kunt de oplossingssjabloon van de Hyperledger Fabric-consortium gebruiken om een consortiumnetwerk van Hyperledger Fabric op Azure te implementeren en configureren.

Wanneer u dit artikel hebt gelezen:

- Beschikt u over kennis van blockchain, Hyperledger Fabric en meer gecompliceerde consortiumnetwerkarchitecturen
- Weet u hoe u Hyperledger Fabric-consortiumnetwerk implementeert en configureert vanuit de Azure-portal

## <a name="about-blockchain"></a>Over blockchains

Als u nieuw bent bij de blockchain-community, biedt deze oplossingssjabloon een uitstekende gelegenheid om op een eenvoudige en configureerbare manier kennis te maken met de technologie op Azure. Blockchain is de onderliggende technologie achter Bitcoin; het is echter veel meer dan alleen een enabler voor een virtuele valuta. Het is een samenstelling van een bestaande database, gedistribueerd systeem en cryptografische technologieën die veilige multiparty-berekening mogelijk maakt met garanties rond onveranderlijkheid, verifieerbaarheid, controleerbaarheid en weerbaarheid tegen aanvallen. Verschillende protocollen gebruiken verschillende mechanismen om deze eigenschappen in te vullen. [Hyperledger Fabric](https://github.com/hyperledger/fabric) is zo'n protocol.

## <a name="consortium-architecture-on-azure"></a>Consortium architectuur op Azure

Om Hyperledger Fabric in Azure in te schakelen, zijn er twee primaire implementatietypes die worden ondersteund. Deze implementaties zijn ontworpen om tegemoet te komen aan verschillende topologieën, op basis van het gewenste doel.

- **Eén virtuele machine, ontwikkelserver** -dit implementatietype is bedoeld als een ontwikkelomgeving die is gebruikt voor het bouwen en testen van oplossingen die zijn gebouwd op Hyperledger Fabric.
- **Meerdere virtuele machines, de scale-out implementatie** -dit implementatietype is ontworpen voor omgevingen die een consortium van verschillende deelnemers modelleren met gebruikmaking van een gedeelde omgeving.

In beide implementaties zijn de bouwstenen die de kern vormen van Hyperledger Fabric hetzelfde.  De verschillen in de implementaties zijn hoe deze componenten worden geschaald.

- **CA-knooppunten**: Een knooppunt met de certificeringsinstantie die wordt gebruikt voor het genereren van certificaten die worden gebruikt voor identiteiten in het netwerk.
- **Besteller knooppunten**: Een knooppunt waarop de implementatie van een garantie levering, zoals totale volgorde broadcast of atomic-transacties communicatieservice wordt uitgevoerd.
- **Knooppunten op hetzelfde niveau**: Een knooppunt dat transacties doorgevoerd en onderhoudt de status en een kopie van het gedistribueerde grootboek.
- **CouchDB knooppunten**: Een knooppunt dat de CouchDB-service die kunt houdt de status-database en bieden uitgebreide query chaincode gegevens, van eenvoudige sleutel/waarde uit te breiden naar JSON type opslag kan worden uitgevoerd.

### <a name="single-virtual-machine-architecture"></a>Architectuur van één virtuele machine

Zoals eerder vermeld, is de enkele virtuele machine-architectuur gebouwd voor ontwikkelaars om een server met een lage footprint te hebben die wordt gebruikt om applicaties te ontwikkelen. Alle getoonde containers worden uitgevoerd in een enkele virtuele machine. De bestelservice gebruikt [SOLO](https://github.com/hyperledger/fabric/tree/master/orderer) voor deze configuratie. Deze configuratie is *geen* fouttolerante bestelservice, maar is ontworpen om van lichtgewicht te zijn voor ontwikkelingsdoeleinden.

![Architectuur met één virtuele Machine](./media/hyperledger-fabric-consortium-blockchain/hlf-single-arch.png)

### <a name="multiple-virtual-machine-architecture"></a>Architectuur van meerdere virtuele machines

De meerdere virtuele machine, uitbreidbare architectuur is gebouwd met hoge beschikbaarheid en het schalen van elk onderdeel in de kern. Deze architectuur is veel meer geschikt is voor productie geavanceerde implementaties.

![Architectuur van meerdere virtuele machines](./media/hyperledger-fabric-consortium-blockchain/hlf-multi-arch.png)

## <a name="getting-started"></a>Aan de slag

Om te beginnen hebt u een Azure-abonnement nodig dat ondersteuning biedt voor de implementatie van verschillende virtuele machines en standaardopslagaccounts. Als u geen Azure-abonnement hebt, kunt u een [gratis Azure-account maken](https://azure.microsoft.com/free/).

Zodra u een abonnement hebt, gaat u naar de [Azure-portal](https://portal.azure.com). Selecteer **een resource maken > Blockchain > Hyperledger Fabric Consortium**.

![Hyperledger Fabric één lid Blockchain Marketplace-sjabloon](./media/hyperledger-fabric-consortium-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementatie

In de **Hyperledger Fabric Consortium** sjabloon, selecteer **maken**.

De sjabloonimplementatie begeleidt u bij het configureren van meerdere knooppunten [Hyperledger 1.3](https://hyperledger-fabric.readthedocs.io/en/release-1.3/) netwerk. De stroom van de implementatie is onderverdeeld in vier stappen: Basisbeginselen, Consortium netwerkinstellingen, configuratie van de Infrastructuurresources en optionele onderdelen.

### <a name="basics"></a>Basisbeginselen

In **basisbeginselen**, waarden opgeven voor standard parameters voor elke implementatie. Abonnement, resourcegroep en basic virtuele machine, zoals eigenschappen.

![Basisbeginselen](./media/hyperledger-fabric-consortium-blockchain/basics.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Voorvoegsel van de resource** | Het voorvoegsel voor resources die worden ingericht als onderdeel van de implementatie |6 tekens of minder |
**Gebruikersnaam** | De gebruikersnaam van de beheerder voor elk van de virtuele machines die zijn geïmplementeerd voor dit lid |1 - 64 tekens |
**Verificatietype** | De methode voor verificatie bij de virtuele machine |Wachtwoord of SSH de openbare sleutel|
**Wachtwoord (verificatietype = wachtwoord)** |Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd. Het wachtwoord moet drie van de tekentypen van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken<br /><br />Hoewel alle virtuele machines in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord wijzigen na het inrichten|12 - 72 tekens|
**SSH-sleutel (verificatietype = openbare SSH-sleutel)** |De veilige shell-sleutel die wordt gebruikt voor externe aanmelding ||
**Abonnement** |Het abonnement waarnaar u wilt implementeren ||
**Resourcegroep** |De resourcegroep waaraan de consortium network implementeren ||
**Locatie** |De Azure-regio waarnaar u wilt het eerste lid in implementeren ||

Selecteer **OK**.

### <a name="consortium-network-settings"></a>Consortium Netwerkinstellingen

Geef in **Netwerkinstellingen** waarden op voor het maken of lid worden van een bestaand consortiumnetwerk en configureer uw organisatie-instellingen.

![Consortium Netwerkinstellingen](./media/hyperledger-fabric-consortium-blockchain/network-settings.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Netwerkconfiguratie** |U kunt een nieuw netwerk maken of lid van een bestaande resourcegroep. Als u ervoor kiest *deelnemen aan bestaande*, moet u extra waarden op te geven. |Nieuw netwerk <br/> Bestaande koppelen |
**Wachtwoord HLF CA** |Een wachtwoord gebruikt voor de certificaten die worden gegenereerd door de certificeringsinstanties die worden gemaakt als onderdeel van de implementatie. Het wachtwoord moet drie van de tekentypen van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken.<br /><br />Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|1 - 25 tekens |
**Organisatie-instellingen** |U kunt de naam en het certificaat van uw organisatie aanpassen of standaardwaarden moet worden gebruikt.|Standaard <br/> Geavanceerd |
**Instellingen voor VPN-netwerken** | Een VPN-tunnel-gateway voor toegang tot de virtuele machines inrichten | Ja <br/> Nee |

Selecteer **OK**.

### <a name="fabric-specific-settings"></a>Fabric-specifieke instellingen

In **Fabric-configuratie** configureert u de netwerkgrootte en -prestaties en geeft u invoer op voor de beschikbaarheid van het netwerk. Zoals nummersorteerders en peerknooppunten, de persistentie-engine die door elk knooppunt wordt gebruikt, en de VM-grootte.

![Fabric-instellingen](./media/hyperledger-fabric-consortium-blockchain/fabric-specific-settings.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Schaaltype** |Het implementatietype van een enkele virtuele machine met meerdere containers of meerdere virtuele machines in een scale-out-model.|Één virtuele machine of meerdere VM 's |
**VM-schijftype** |Het type opslag back-ups maken van de geïmplementeerde knooppunten. <br/> Voor meer informatie over de typen beschikbare schijfruimte, gaat u naar [Selecteer een schijftype](../../virtual-machines/windows/disks-types.md).|Standard - SSD <br/> Premium SSD |

### <a name="multiple-vm-deployment-additional-settings"></a>Meerdere VM-implementatie (extra instellingen)

![Fabric-instellingen voor meerdere vm-implementaties](./media/hyperledger-fabric-consortium-blockchain/multiple-vm-deployment.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Aantal besteller knooppunten** |Het aantal knooppunten dat bestellen (organiseren) transacties in een blok. <br />Voor meer informatie over de volgorde-service, gaat u naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html) |1-4 |
**De grootte van de virtuele machine knooppunt besteller** |De grootte van de virtuele machine die wordt gebruikt voor besteller knooppunten in het netwerk|Standard Bs<br />Standard Ds<br />Standard FS |
**Aantal knooppunten van peer** | Knooppunten die eigendom zijn van consortium-leden die transacties uitvoeren en onderhouden van de status en een kopie van het grootboek.<br />Voor meer informatie over de volgorde-service, gaat u naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|1-4 |
**Knooppunt persistentie ingeschakeld** |De persistentie engine wordt gebruikt door de peer-knooppunten. U kunt deze engine per knooppunt op hetzelfde niveau configureren. Zie de details hieronder voor meerdere peer-knooppunten.|CouchDB <br />LevelDB |
**De grootte van de virtuele machine knooppunt peer** |De grootte van de virtuele machine die wordt gebruikt voor alle knooppunten in het netwerk|Standard Bs<br />Standard Ds<br />Standard FS |

### <a name="multiple-peer-node-configuration"></a>Configuratie met meerdere peer-knooppunten

Met deze sjabloon kunt u uw persistentie-engine per peerknooppunt kiezen. Als u bijvoorbeeld drie peerknooppunten hebt, kunt u CouchDB op één knooppunt en LevelDB op de andere twee gebruiken.

![Configuratie met meerdere peer-knooppunten](./media/hyperledger-fabric-consortium-blockchain/multiple-peer-nodes.png)

Selecteer **OK**.

### <a name="deploy"></a>Implementeren

Bekijk in de **samenvatting** de ingevoerde waarden en voer de basisvalidatie vóór implementatie uit.

![Samenvatting](./media/hyperledger-fabric-consortium-blockchain/summary.png)

Wettelijke informatie en privacy voorwaarden bekijken en selecteer **aankoop** te implementeren. Implementatietijd kan, afhankelijk van het aantal virtuele machines die zijn ingericht, variëren van enkele minuten tot tien minuten.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om u te richten op de ontwikkeling van applicaties en chaincode op basis van uw Hyperledger consortium blockchain-netwerk.
