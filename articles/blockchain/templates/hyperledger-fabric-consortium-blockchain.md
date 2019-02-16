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
ms.openlocfilehash: acbd41b7203e458a5ed60382354c862748863053
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328024"
---
# <a name="hyperledger-fabric-consortium-network"></a>Hyperledger Fabric consortium network

U kunt de oplossingssjabloon voor Hyperledger Fabric consortium gebruiken om te implementeren en configureren van een Hyperledger Fabric consortium network in Azure.

Wanneer u dit artikel hebt gelezen:

- Verkrijgen van praktische kennis van blockchain, Hyperledger Fabric en meer gecompliceerde consortium modellen van netwerkarchitectuur
- Meer informatie over het implementeren en configureren van een consortium network Hyperledger Fabric in Azure portal uit

## <a name="about-blockchain"></a>Over blockchains

Als u niet bekend bent met de blockchain-community, is deze oplossingssjabloon een uitstekende gelegenheid om te leren over de technologie op een manier eenvoudig en kunnen worden geconfigureerd in Azure. Blockchain is de onderliggende technologie achter Bitcoin; het is echter veel meer dan alleen een factor voor een virtuele valuta. Het is een samenstelling van de bestaande database, gedistribueerd systeem en cryptografische technologieën waarmee veilige meerdere partijen berekening met garanties onveranderbaarheid, verifiability controleerbaarheid en tolerantie voor aanvallen. Verschillende protocollen maken gebruik van verschillende mechanismen voor het opgeven van deze kenmerken. [Hyperledger Fabric](https://github.com/hyperledger/fabric) één dergelijk protocol is.

## <a name="consortium-architecture-on-azure"></a>Consortium architectuur op Azure

Om in te schakelen Hyperledger Fabric in Azure, zijn er twee primaire implementatietypen die worden ondersteund. Deze implementaties zijn ontworpen om te voorzien in verschillende topologieën, op basis van de gewenste doel.

- **Eén virtuele machine, ontwikkelaar server** -dit implementatietype is bedoeld als een ontwikkelomgeving die is gebruikt voor het bouwen en testen van oplossingen die zijn gebouwd op Hyperledger Fabric.
- **Meerdere virtuele machines, de schaal vergroten implementatie** -dit implementatietype is ontworpen voor omgevingen die model van een consortium van andere deelnemers aan gebruik te maken van een gedeelde omgeving.

In de implementatie zijn de bouwstenen die de kern van Hyperledger Fabric zijn hetzelfde.  De verschillen in de implementaties zijn hoe deze onderdelen worden uitgeschaald.

- **CA-knooppunten**: Een knooppunt met de certificeringsinstantie die wordt gebruikt voor het genereren van certificaten die worden gebruikt voor identiteiten in het netwerk.
- **Besteller knooppunten**: Een knooppunt waarop de implementatie van een garantie levering, zoals totale volgorde broadcast of atomic-transacties communicatieservice wordt uitgevoerd.
- **Knooppunten op hetzelfde niveau**: Een knooppunt dat transacties doorgevoerd en onderhoudt de status en een kopie van het gedistribueerde grootboek.
- **CouchDB knooppunten**: Een knooppunt dat de CouchDB-service die kunt houdt de status-database en bieden uitgebreide query chaincode gegevens, van eenvoudige sleutel/waarde uit te breiden naar JSON type opslag kan worden uitgevoerd.

### <a name="single-virtual-machine-architecture"></a>Architectuur van één virtuele machine

Zoals eerder vermeld, de enige virtuele is computerarchitectuur gebouwd voor ontwikkelaars beschikken over een lage footprint-server die wordt gebruikt om toepassingen te ontwikkelen. Alle containers die worden weergegeven worden in een enkele virtuele machine uitgevoerd. De volgorde service gebruikmaakt van [SOLO](https://github.com/hyperledger/fabric/tree/master/orderer) voor deze configuratie. Deze configuratie is *niet* een fout met betrekking tot fouttolerante bestellen service, maar is ontworpen om te worden lichtgewicht voor ontwikkelingsdoeleinden.

![Architectuur met één virtuele Machine](./media/hyperledger-fabric-consortium-blockchain/hlf-single-arch.png)

### <a name="multiple-virtual-machine-architecture"></a>Architectuur van meerdere virtuele machine

De meerdere virtuele machine, uitbreidbare architectuur is gebouwd met hoge beschikbaarheid en het schalen van elk onderdeel in de kern. Deze architectuur is veel meer geschikt is voor productie geavanceerde implementaties.

![Architectuur van meerdere virtuele machine](./media/hyperledger-fabric-consortium-blockchain/hlf-multi-arch.png)

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen, moet u een Azure-abonnement dat u kunt ondersteuning voor de implementatie van meerdere virtuele machines en standard storage-accounts. Als u een Azure-abonnement hebt, kunt u [maken van een gratis Azure-account](https://azure.microsoft.com/free/).

Nadat u een abonnement hebt, gaat u naar de [Azure-portal](https://portal.azure.com). Selecteer **een resource maken > Blockchain > Hyperledger Fabric Consortium**.

![Hyperledger Fabric één lid Blockchain Marketplace-sjabloon](./media/hyperledger-fabric-consortium-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementatie

In de **Hyperledger Fabric Consortium** sjabloon, selecteer **maken**.

De sjabloonimplementatie begeleidt u bij het configureren van meerdere knooppunten [Hyperledger 1.3](https://hyperledger-fabric.readthedocs.io/en/release-1.3/) netwerk. De stroom van de implementatie is onderverdeeld in vier stappen: Basisbeginselen, Consortium netwerkinstellingen, configuratie van de Infrastructuurresources en optionele onderdelen.

### <a name="basics"></a>Basisbeginselen

In **basisbeginselen**, waarden opgeven voor standard parameters voor elke implementatie. Abonnement, resourcegroep en basic virtuele machine, zoals eigenschappen.

![Basisbeginselen](./media/hyperledger-fabric-consortium-blockchain/basics.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Voorvoegsel van de resource**| Het voorvoegsel voor resources die worden ingericht als onderdeel van de implementatie |6 tekens of minder |
**Gebruikersnaam**| De gebruikersnaam van de beheerder voor elk van de virtuele machines die zijn geïmplementeerd voor dit lid |1 - 64 tekens |
**Verificatietype**| De methode voor verificatie bij de virtuele machine |Wachtwoord of SSH de openbare sleutel|
**Wachtwoord (verificatietype = wachtwoord)**|Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd. Het wachtwoord moet drie van de tekentypen van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken<br /><br />Hoewel alle virtuele machines in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord wijzigen na het inrichten|12 - 72 tekens|
**SSH-sleutel (verificatietype = openbare SSH-sleutel)**|De veilige shell-sleutel die wordt gebruikt voor externe aanmelding ||
**Abonnement** |Het abonnement waarnaar u wilt implementeren ||
**Resourcegroep** |De resourcegroep waaraan de consortium network implementeren ||
**Locatie** |De Azure-regio waarnaar u wilt het eerste lid in implementeren ||

Selecteer **OK**.

### <a name="consortium-network-settings"></a>Netwerkinstellingen Consortium

In **netwerkinstellingen**, geeft u de invoer voor het maken of lid worden van een bestaande consortium network en configureer de instellingen van uw organisatie.

![Netwerkinstellingen Consortium](./media/hyperledger-fabric-consortium-blockchain/network-settings.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Netwerkconfiguratie**|U kunt een nieuw netwerk maken of lid van een bestaande resourcegroep. Als u ervoor kiest *deelnemen aan bestaande*, moet u extra waarden op te geven. |Nieuw netwerk <br/> Bestaande koppelen |
**Wachtwoord HLF CA**|Een wachtwoord gebruikt voor de certificaten die worden gegenereerd door de certificeringsinstanties die worden gemaakt als onderdeel van de implementatie. Het wachtwoord moet drie van de tekentypen van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken.<br /><br />Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|1 - 25 tekens |
**Organisatie-instellingen** |U kunt de naam en het certificaat van uw organisatie aanpassen of standaardwaarden moet worden gebruikt.|Standaard <br/> Geavanceerd |
**Instellingen voor VPN-netwerken**| Een VPN-tunnel-gateway voor toegang tot de virtuele machines inrichten | Ja <br/> Nee |

Selecteer **OK**.

### <a name="fabric-specific-settings"></a>Fabric-specifieke instellingen

In **configuratie van de Infrastructuurresources**, u Configureer netwerkgrootte en de prestaties en geef de invoer voor de beschikbaarheid van het netwerk. Getal, zoals besteller en peer-knooppunten, persistentie-engine wordt gebruikt door elk knooppunt en de VM-grootte.

![Fabric-instellingen](./media/hyperledger-fabric-consortium-blockchain/fabric-specific-settings.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Schaaltype**|Het implementatietype van een enkele virtuele machine met meerdere containers of meerdere virtuele machines in een scale-out-model.|Één virtuele machine of meerdere VM 's |
**VM-schijftype**|Het type opslag back-ups maken van de geïmplementeerde knooppunten. <br/> Voor meer informatie over de typen beschikbare schijfruimte, gaat u naar [Selecteer een schijftype](../../virtual-machines/windows/disks-types.md).|Standard - SSD <br/> Premium SSD |

### <a name="multiple-vm-deployment-additional-settings"></a>Meerdere VM-implementatie (extra instellingen)

![Fabric-instellingen voor meerdere vm-implementaties](./media/hyperledger-fabric-consortium-blockchain/multiple-vm-deployment.png)

| Parameternaam | Description | Toegestane waarden |
|---|---|---|
**Aantal besteller knooppunten** |Het aantal knooppunten dat bestellen (organiseren) transacties in een blok. <br />Voor meer informatie over de volgorde-service, gaat u naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html) |1-4 |
**De grootte van de virtuele machine knooppunt besteller** |De grootte van de virtuele machine die wordt gebruikt voor besteller knooppunten in het netwerk|Standard Bs<br />Standard Ds<br />Standard FS |
**Aantal knooppunten van peer**| Knooppunten die eigendom zijn van consortium-leden die transacties uitvoeren en onderhouden van de status en een kopie van het grootboek.<br />Voor meer informatie over de volgorde-service, gaat u naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|1-4 |
**Knooppunt persistentie ingeschakeld** |De persistentie engine wordt gebruikt door de peer-knooppunten. U kunt deze engine per knooppunt op hetzelfde niveau configureren. Zie de details hieronder voor meerdere peer-knooppunten.|CouchDB </br>LevelDB |
**De grootte van de virtuele machine knooppunt peer** |De grootte van de virtuele machine die wordt gebruikt voor alle knooppunten in het netwerk|Standard Bs<br />Standard Ds<br />Standard FS |

### <a name="multiple-peer-node-configuration"></a>Configuratie met meerdere peer-knooppunt

Deze sjabloon kunt u uw engine persistentie per knooppunt op hetzelfde niveau te kiezen. Bijvoorbeeld, hebt u drie peer-knooppunten kunt u CouchDB op een en LevelDB op de andere twee.

![Configuratie met meerdere peer-knooppunt](./media/hyperledger-fabric-consortium-blockchain/multiple-peer-nodes.png)

Selecteer **OK**.

### <a name="deploy"></a>Implementeren

In **samenvatting**, Controleer de invoer die is opgegeven en uit te voeren basisvalidatie vóór de implementatie.

![Samenvatting](./media/hyperledger-fabric-consortium-blockchain/summary.png)

Wettelijke informatie en privacy voorwaarden bekijken en selecteer **aankoop** te implementeren. Implementatietijd kan, afhankelijk van het aantal virtuele machines die zijn ingericht, variëren van enkele minuten tot tien minuten.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om u te richten op toepassings- en chaincode ontwikkeling op basis van uw Hyperledger consortium blockchain-netwerk.
