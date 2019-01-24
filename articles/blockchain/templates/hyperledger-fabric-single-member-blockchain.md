---
title: Hyperledger Fabric Consortium één lid
description: De oplossingssjabloon Hyperledger Fabric Consortium gebruiken om te implementeren en configureren van een netwerk met één lid
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/29/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: a6aa6d925d47ad9d24de68342f4a6e76a1d9d81f
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54828227"
---
# <a name="hyperledger-fabric-single-member-network"></a>Hyperledger Fabric enkel lid netwerk

U kunt de oplossingssjabloon Hyperledger Fabric Consortium gebruiken om te implementeren en configureren van een Hyperledger Fabric enkel lid (met meerdere knooppunten) netwerk.

Wanneer u dit artikel hebt gelezen:

- Verkrijgen van praktische kennis van blockchain, Hyperledger Fabric en meer gecompliceerde consortium modellen van netwerkarchitectuur
- Meer informatie over het implementeren en configureren van een lid zijn van één Hyperledger Fabric consortium netwerk in Azure portal

## <a name="about-blockchain"></a>Over blockchains

Als u niet bekend bent met de blockchain-community, is deze oplossingssjabloon een uitstekende gelegenheid om te leren over de technologie op een manier eenvoudig en kunnen worden geconfigureerd in Azure. Blockchain is de onderliggende technologie achter Bitcoin; het is echter veel meer dan alleen een factor voor een virtuele valuta. Het is een samenstelling van de bestaande database, gedistribueerd systeem en cryptografische technologieën waarmee veilige meerdere partijen berekening met garanties onveranderbaarheid, verifiability controleerbaarheid en tolerantie voor aanvallen. Verschillende protocollen maken gebruik van verschillende mechanismen voor het opgeven van deze kenmerken. [Hyperledger Fabric](https://github.com/hyperledger/fabric) één dergelijk protocol is.

## <a name="consortium-architecture-on-azure"></a>Consortium architectuur op Azure

Deze sjabloon implementeert een topologie om u te helpen bij het testen en productie voor gebruikers in een enkel simuleren organisatie (één lid). Deze implementatie bestaat uit een netwerk met meerdere knooppunten in een enkele regio en snel moeten worden uitgebreid naar meerdere regio's.

Het netwerk bestaat uit drie soorten knooppunten:

1. **Lid knooppunt**: Een knooppunt waarop de service voor het lidmaatschap van Fabric die wordt geregistreerd en leden van het netwerk beheert. Dit knooppunt kan worden geclusterd voor schaalbaarheid en hoge beschikbaarheid; Maar in dit lab, een lid van één knooppunt zal worden gebruikt.
2. **Besteller knooppunten**: Een knooppunt waarop de implementatie van een garantie levering, zoals totale volgorde broadcast of atomic-transacties communicatieservice wordt uitgevoerd.
3. **Knooppunten op hetzelfde niveau**: Een knooppunt dat transacties doorgevoerd en onderhoudt de status en een kopie van het gedistribueerde grootboek.

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen, moet u een Azure-abonnement dat u kunt ondersteuning voor de implementatie van meerdere virtuele machines en standard storage-accounts. Als u een Azure-abonnement hebt, kunt u [maken van een gratis Azure-account](https://azure.microsoft.com/free/).

Standaard ondersteuning voor de meeste abonnementstypen een kleine implementatietopologie zonder om quotum te verhogen. De laagste mogelijke implementatie voor één lid nodig:

- 5 virtuele machines (5 kerngeheugens)
- 1 VNet
- 1 load balancer
- 1 openbaar IP-adres

Nadat u een abonnement hebt, gaat u naar de [Azure-portal](https://portal.azure.com). Selecteer **+**, selecteer **Blockchain**, en selecteer **Hyperledger Fabric één lid Blockchain**.

![Hyperledger Fabric één lid Blockchain Marketplace-sjabloon](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementatie

Als u wilt starten, selecteert u de **Hyperledger Fabric één lid Blockchain** en klikt u op **maken** openen de **basisbeginselen** blade in de wizard.

De sjabloonimplementatie begeleidt u bij het configureren van het netwerk met meerdere knooppunten. De stroom van de implementatie is onderverdeeld in drie stappen: Basisbeginselen, netwerkconfiguratie en configuratie van de Infrastructuurresources.

### <a name="basics"></a>Basisbeginselen

In de **basisbeginselen** blade waarden opgeven voor standard parameters voor elke implementatie. Abonnement, resourcegroep en basic virtuele machine, zoals eigenschappen.

![Basisbeginselen](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Parameternaam| Description| Toegestane waarden|Standaardwaarde
---|---|---|---
**Voorvoegsel van de resource**| Een tekenreeks die wordt gebruikt als basis voor de naam van de geïmplementeerde resources.|6 tekens of minder|N.v.t.
**VM-gebruikersnaam**| De gebruikersnaam van de beheerder voor elk van de virtuele machines die zijn geïmplementeerd voor dit lid.|1 - 64 tekens|azureuser
**Verificatietype**| De methode voor verificatie bij de virtuele machine.|Wachtwoord of SSH de openbare sleutel|Wachtwoord
**Wachtwoord (verificatietype = wachtwoord)**|Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd. Het wachtwoord moet drie van de tekentypen van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken.<br /><br />Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|N.v.t.
**SSH-sleutel (verificatietype = openbare sleutel)**|De veilige shell-sleutel die wordt gebruikt voor externe aanmelding.||N.v.t.
**Toegang beperken door van IP-adres**|Als u op het type te bepalen of clients eindpunt toegang beperkt tot of niet is.|Ja/Nee| Nee
**Toegestane IP-adres of subnet (toegang beperken door van IP-adres = Yes)**|Het IP-adres of de set IP-adressen die is toegestaan voor toegang tot het clienteindpunt van de als toegangsbeheer is ingeschakeld.||N.v.t.
**Abonnement** |Het abonnement waarnaar u wilt implementeren.
**Resourcegroep** |De resourcegroep waaraan de consortium network implementeren.||N.v.t.
**Locatie** |De Azure-regio waarnaar u wilt implementeren, het eerste lid ** s netwerk footprint.

### <a name="network-size-and-performance"></a>Netwerkgrootte en prestaties

Vervolgens in **grootte en prestaties, het netwerk** invoer voor de grootte van het netwerk consortium opgeven. Zoals het aantal lidmaatschap, besteller en peer-knooppunten. Kies Opties voor infrastructuur en de grootte van uw virtuele machine.

![Netwerkgrootte en prestaties](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Parameternaam| Description| Toegestane waarden|Standaardwaarde
---|---|---|---
**Aantal knooppunten dat lidmaatschap**|Het aantal knooppunten waarop de lidmaatschapsservice wordt uitgevoerd. Bekijk voor meer informatie over de lidmaatschapsservice beveiliging & Lidmaatschapsservices onder de Hyperledger [documentatie](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf).<br /><br />Deze waarde is momenteel beperkt tot 1 knooppunt, maar wij van plan bent te scale-out via clustering in de volgende wijziging.|1| 1
**Aantal besteller knooppunten** |Het aantal knooppunten dat bestellen (organiseren) transacties in een blok.--> deze instructie is gaat en verwarrend zijn. Voor meer informatie over de volgorde-service, gaat u naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html).<br /><br />Deze waarde is momenteel beperkt tot 1 knooppunt. |1 |1
**Aantal knooppunten van Peer**| Knooppunten die eigendom zijn van consortium-leden die transacties uitvoeren en onderhouden van de status en een kopie van het grootboek.<br /><br />Voor meer informatie over de volgorde-service, gaat u naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|3| 3 - 9
**Opslagprestaties**|Het type opslag back-ups maken van de geïmplementeerde knooppunten. Voor meer informatie over opslag, gaat u naar [Inleiding tot Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) en [Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standard- of Premium|Standard
**Grootte van virtuele machine** |De grootte van de virtuele machine die wordt gebruikt voor alle knooppunten in het netwerk|Standard A<br />Standard D<br />Standard D-v2<br />Standard F-serie<br />Standard DS<br />en Standard FS|Standard D1_v2

### <a name="fabric-specific-settings"></a>Fabric-specifieke instellingen

Ten slotte onder **Infrastructuurinstellingen**, infrastructuur-gerelateerde configuratie-instellingen opgeven.

![Fabric-instellingen](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Parameternaam| Description| Toegestane waarden|Standaardwaarde
---|---|---|---
**Bootstrap-gebruikersnaam**| De eerste gebruiker die wordt geregistreerd met de lid-services in het geïmplementeerde netwerk gemachtigd.|9 of minder tekens|beheerder
**Bootstrap-gebruikerswachtwoord voor CA-infrastructuur**|Het administrator-wachtwoord gebruikt voor het beveiligen van de Fabric-CA-account in het lidmaatschap van knooppunt geïmporteerd.<br /><br />Het wachtwoord moet bestaan uit één hoofdletter, één kleine letter en één cijfer.|12 of meer tekens|N.v.t.

### <a name="deploy"></a>Implementeren

In **samenvatting**, Controleer de invoer die is opgegeven en uit te voeren basisvalidatie vóór de implementatie.

![Samenvatting](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Wettelijke informatie en privacy voorwaarden bekijken en op **aankoop** te implementeren. Implementatietijd kan, afhankelijk van het aantal virtuele machines die zijn ingericht, variëren van enkele minuten tot tien minuten.

### <a name="accessing-nodes"></a>Toegang tot knooppunten

Als de implementatie is voltooid, een **overzicht** wordt weergegeven.

![Implementaties](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Als het scherm niet automatisch wordt weergegeven (misschien omdat u om de management portal tijdens verplaatst de implementatie is uitgevoerd), kunt u deze altijd vinden in het tabblad resourcegroepen in de navigatiebalk aan de linkerkant. Klik op de naam van de resourcegroep die u hebt opgegeven in stap 1 tot en gaat u naar de **overzicht** pagina.

Overzicht van een lijst met alle resources die zijn geïmplementeerd met de oplossingssjabloon. U kunt ze eens verkennen op wordt, maar in dit scherm u ook toegang tot de _uitvoerparameters_ die worden gegenereerd door de sjabloon. Deze uitvoerparameters geeft u nuttige informatie bij het verbinden met uw Hyperledger Fabric-netwerk.

Voor toegang tot de output-parameters, en klik eerst op de **implementaties** tabblad in de resourcegroep-blade. Geschiedenis van de implementatie wordt weergegeven.

![Implementatiegeschiedenis](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

Klik op de eerste implementatie in de lijst om te kijken naar de details uit de Implementatiegeschiedenis.

![Implementatiedetails](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

Het scherm met details ziet u een samenvatting van de implementatie, gevolgd door drie parameters die handig zijn:

- De _API-EINDPUNT_ kan worden gebruikt wanneer u een toepassing op het netwerk implementeert.
- De _VOORVOEGSEL_ , ook wel genoemd _implementatie voorvoegsel_ , unieke wijze identificeert en uw implementatie van uw resources. Deze worden gebruikt bij het gebruik van de opdrachtregelprogramma's op basis van.
- De _SSH eerste VM-_ biedt u een vooraf samengesteld ssh-opdracht met de juiste parameters vereist voor het verbinding maken met de eerste virtuele machine in uw netwerk. Voor Hyperledger Fabric worden de Fabric-CA-knooppunt.

U kunt op afstand verbinding maken met de virtuele machines voor elk knooppunt via SSH met uw beheerder opgegeven gebruikersnaam en het wachtwoord/SSH-sleutel. Omdat het knooppunt VM's geen hun eigen openbare IP-adressen, moet u om te gaan via de load balancer en het poortnummer opgeven. De SSH-opdracht voor toegang tot het eerste knooppunt van de transactie is de sjabloonuitvoer van de derde *SSH eerste VM-* (voor de Voorbeeldimplementatie: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Verhogen om door te gaan naar de knooppunten van extra transactielogboeken, het poortnummer dat door een (bijvoorbeeld het eerste knooppunt van de transactie is op poort 3000, de tweede is 3001, de derde is op 3002, enz.).

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om u te richten op toepassings- en chaincode ontwikkeling op basis van uw Hyperledger consortium blockchain-netwerk.
