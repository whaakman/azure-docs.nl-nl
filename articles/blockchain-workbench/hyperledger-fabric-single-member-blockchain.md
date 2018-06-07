---
title: Hyperledger Fabric Consortium
description: De sjabloon van de oplossing Hyperledger Fabric Consortium gebruiken om te implementeren en configureren van een netwerk met één lid
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 7b60c086896506e5883607db48a64d2a2efbd967
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654939"
---
# <a name="hyperledger-fabric-single-member-network"></a>Hyperledger enkel lid infrastructuurnetwerk

U kunt de oplossing Hyperledger Fabric Consortium sjabloon implementeren en configureren van een netwerk Hyperledger Fabric enkel lid (met meerdere knooppunten) gebruiken.

Wanneer u dit artikel hebt gelezen:

- Enige basiskennis van blockchain Hyperledger Fabric en gecompliceerdere consortium netwerkarchitectuur verkrijgen
- Meer informatie over het implementeren en configureren van een lid zijn van één Hyperledger consortium infrastructuurnetwerk uit binnen de Azure-portal

## <a name="about-blockchain"></a>Over blockchain

Als u niet bekend met de community blockchain bent, is dit een goede kans voor meer informatie over de technologie op een manier eenvoudig en kunnen worden geconfigureerd in Azure. Blockchain is de onderliggende technologie achter Bitcoin; het is echter veel meer dan alleen een factor voor een virtuele valuta. Het is een samenstelling van een bestaande database, gedistribueerde systemen en cryptografische technologieën waarmee veilige meerdere partijen berekening garanties rond onveranderbaarheid, verifiability controleerbaarheid en tolerantie voor aanvallen. Verschillende protocollen gebruiken verschillende mechanismen voor het opgeven van deze kenmerken. [Hyperledger Fabric](https://github.com/hyperledger/fabric) is een protocol voor dergelijke.

## <a name="consortium-architecture-on-azure"></a>Architectuur Consortium op Azure

Deze sjabloon wordt geïmplementeerd voor een topologie om te testen en productie voor gebruikers binnen één simuleren organisatie (één lid). Deze implementatie bestaat uit een netwerk met meerdere knooppunten in één regio, snel moeten worden uitgebreid met meerdere regio's.

Het netwerk bestaat uit drie soorten knooppunten:

1. **Lid knooppunt**: een knooppunt dat de service voor het lidmaatschap van Fabric die wordt geregistreerd en leden van het netwerk beheert. Dit knooppunt kan uiteindelijk worden geclusterd voor schaalbaarheid en hoge beschikbaarheid, maar in dit lab een enkel lid-knooppunt wordt gebruikt.
2. **Besteller knooppunten**: een knooppunt met de implementatie van een garantie levering zoals Totaal communicatieservice bestellen broadcast-atomic of transacties.
3. **Knooppunten op hetzelfde niveau**: een knooppunt dat transacties worden doorgevoerd en onderhoudt de status en een kopie van het gedistribueerde grootboek.

## <a name="getting-started"></a>Aan de slag

Om te beginnen, moet u een Azure-abonnement die ondersteuning biedt voor meerdere virtuele machines en opslagaccounts standaard implementeren. Als u niet een Azure-abonnement hebt, kunt u [maken van een gratis Azure-account](https://azure.microsoft.com/free/).

Standaard ondersteunt de meeste typen van de abonnementen een kleine implementatietopologie zonder quotum te verhogen. De kleinste mogelijke implementatie van één lid nodig:

- 5 virtuele machines (5 kernen)
- 1 VNet
- 1 load balancer
- 1 openbare IP-adres

Zodra u een abonnement hebt, gaat u naar de [Azure-portal](https://portal.azure.com). Selecteer **+**, selecteer **Blockchain**, en selecteer **Hyperledger Fabric één lid Blockchain**.

![Hyperledger Fabric één lid Blockchain Marketplace-sjabloon](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Implementatie

Als u wilt starten, selecteert u de **Hyperledger Fabric één lid Blockchain** en klik op **maken**. Hiermee opent u de **basisbeginselen** blade in de wizard.

De sjabloonimplementatie begeleidt u bij het configureren van het netwerk met meerdere knooppunten. De stroom van de implementatie is onderverdeeld in drie stappen: Basics netwerkconfiguratie en configuratie van de Infrastructuurresources.

### <a name="basics"></a>Basisbeginselen

Onder de **basisbeginselen** blade waarden opgeven voor de standaardparameters voor de implementatie, zoals abonnement, resourcegroep en eigenschappen van eenvoudige virtuele machine.

![Basisbeginselen](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Parameternaam| Beschrijving| Toegestane waarden|Standaardwaarde
---|---|---|---
**Resource-voorvoegsel**| Een tekenreeks die wordt gebruikt als basis voor de naamgeving van de geïmplementeerde resources.|6 tekens of minder|N.v.t.
**VM-gebruikersnaam**| De gebruikersnaam van de beheerder voor elk van de virtuele machines die zijn geïmplementeerd voor dit lid.|1 - 64 tekens|azureuser
**Verificatietype**| De methode voor verificatie bij de virtuele machine.|Wachtwoord of SSH openbare sleutel|Wachtwoord
**Wachtwoord (verificatietype = wachtwoord)**|Het wachtwoord voor het administrator-account voor elk van de geïmplementeerde virtuele machines. Het wachtwoord moet 3 van de volgende bevatten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken.<br /><br />Terwijl alle VM's in eerste instantie hetzelfde wachtwoord zijn, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|N.v.t.
**SSH-sleutel (verificatietype = openbare sleutel)**|De sleutel voor secure shell gebruikt voor externe aanmelding.||N.v.t.
**Toegang beperken door IP-adres**|De instelling om te bepalen type of client eindpunt toegang beperkt of niet wordt.|Ja/Nee| Nee
**IP-adres of subnet toegestaan (de toegang beperken op IP-adres = Yes)**|Het IP-adres of de set IP-adressen die is toegestaan voor toegang tot het eindpunt van de client wanneer toegangsbeheer is ingeschakeld.||N.v.t.
**Abonnement** |Het abonnement waarnaar u wilt implementeren.
**Resourcegroep** |De resourcegroep waartoe het consortium netwerk implementeren.||N.v.t.
**Locatie** |De Azure-regio waarop voor het implementeren van het eerste lid ** s netwerk footprint.

### <a name="network-size-and-performance"></a>De netwerkgrootte van het en prestaties

Vervolgens onder **grootte en de prestaties,** ingangen voor de grootte van het netwerk consortium, zoals het aantal lidmaatschap, besteller en peer-knooppunten opgeven. Kies infrastructuur opties en de grootte van uw virtuele machine.

![De netwerkgrootte van het en prestaties](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Parameternaam| Beschrijving| Toegestane waarden|Standaardwaarde
---|---|---|---
**Aantal knooppunten dat lidmaatschap**|Het aantal knooppunten die de lidmaatschapsservice wordt uitgevoerd. Bekijk voor meer informatie over de lidmaatschapsservice beveiliging & Lidmaatschapsservices onder de Hyperledger [documentatie](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf).<br /><br />Deze waarde is momenteel beperkt tot 1 knooppunt, maar we zullen ter ondersteuning van scale-out via clustering in de volgende revisie.|1| 1
**Aantal besteller knooppunten** |Het aantal knooppunten dat volgorde (organiseren) transacties naar een blok.--> deze instructie is lang en verwarrend zijn. Ga voor meer informatie over de bestellen service naar de Hyperledger [documentatie](http://hyperledger-fabric.readthedocs.io/en/latest/orderingservice.html).<br /><br />Deze waarde is momenteel beperkt tot 1 knooppunt. |1 |1
**Aantal Peer knooppunten**| Knooppunten die eigendom zijn van consortium leden die transacties worden uitgevoerd en onderhouden van de status en een kopie van het grootboek.<br /><br />Ga voor meer informatie over de bestellen service naar de Hyperledger [documentatie](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|3| 3 - 9
**Prestaties van de opslag**|Het type opslag dat een back-up elk van de geïmplementeerde knooppunten. Voor meer informatie over opslag, gaat u naar [Inleiding tot Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction) en [Premium-opslag](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standard- of Premium|Standard
**Grootte van virtuele machine** |De grootte van de virtuele machine die wordt gebruikt voor alle knooppunten in het netwerk|Standard A<br />Standaard D<br />Standaard D-v2<br />Standaard F-serie<br />Standaard DS<br />en standaard FS|Standaard D1_v2

### <a name="fabric-specific-settings"></a>Fabric-specifieke instellingen

Ten slotte onder **Fabric instellingen**, infrastructuur-gerelateerde configuratie-instellingen opgeven.

![Fabric-instellingen](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Parameternaam| Beschrijving| Toegestane waarden|Standaardwaarde
---|---|---|---
**Bootstrap-gebruikersnaam**| De eerste gebruiker die wordt geregistreerd met de services lid in het geïmplementeerde netwerk geautoriseerd.|9 of minder tekens|Beheerder
**Bootstrap gebruikerswachtwoord voor Fabric-CA**|Het administrator-wachtwoord gebruikt om de geïmporteerd in het knooppunt van het lidmaatschap van Fabric-CA-account te beveiligen.<br /><br />Het wachtwoord moet bestaan uit één hoofdletter, één kleine letter en één cijfer.|12 of meer ongeldige tekens|N.v.t.

### <a name="deploy"></a>Implementeren

In **samenvatting**, Controleer de opgegeven invoer en uit te voeren basisvalidatietests voorafgaand aan de implementatie.

![Samenvatting](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Wettelijke informatie en privacy rwaarden en klik op **aankoop** te implementeren. Implementatietijd kan afhankelijk van het aantal virtuele machines die zijn ingericht, variëren van enkele minuten tot tien minuten.

### <a name="accessing-nodes"></a>Toegang tot de knooppunten

Zodra de implementatie is voltooid, een **overzicht** wordt weergegeven.

![Implementaties](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Als het scherm niet automatisch wordt weergegeven (mogelijk omdat u verplaatst van de beheerportal tijdens de implementatie is uitgevoerd), u kunt deze altijd vinden in het tabblad resourcegroepen in de navigatiebalk aan de linkerkant. Klik op de Resource Group-naam die u hebt opgegeven in stap 1 naar de **overzicht** pagina.

Overzicht geeft een lijst van alle resources die zijn geïmplementeerd met de oplossingssjabloon. U kunt ze verkennen op worden, maar in dit scherm kunt u ook openen de _uitvoerparameters_ die worden gegenereerd door de sjabloon. Deze uitvoerparameters krijgt u nuttige informatie bij het verbinden met uw netwerk Hyperledger Fabric.

Voor toegang tot de output-parameters, klikt u eerst op de **implementaties** tabblad in de blade resourcegroep. Geschiedenis van de implementatie wordt weergegeven.

![Implementatiegeschiedenis](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

Klik op de eerste implementatie in de lijst om te kijken naar de details van de geschiedenis van de implementatie.

![Details van de implementatie](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

Het scherm details ziet u een overzicht van de implementatie, gevolgd door drie nuttig output-parameters:

- De _API-EINDPUNT_ kan worden gebruikt wanneer u een toepassing op het netwerk implementeert.
- De _VOORVOEGSEL_ , ook wel _implementatie voorvoegsel_ , unieke wijze identificeert en uw implementatie van uw resources. Deze wordt gebruikt wanneer u met de opdrachtregelprogramma's op basis.
- De _SSH eerste VM-_ biedt u een vooraf samengesteld ssh-met de juiste parameters opdracht vereist om verbinding met de eerste virtuele machine in uw netwerk te; in het geval van Hyperledger Fabric, zal zijn voor het Fabric-CA-knooppunt.

U kunt op afstand verbinding maken met de virtuele machines voor elk knooppunt via SSH met uw beheerder opgegeven gebruikersnaam en wachtwoord/SSH-sleutel. Omdat het knooppunt VM's geen eigen openbare IP-adressen, moet u Ga via de load balancer en het poortnummer opgeven. De SSH-opdracht voor toegang tot het eerste knooppunt van de transactie is de sjabloonuitvoer van de derde ** SSH eerste VM-(voor de implementatie van de steekproef: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Voor extra transactie knooppunten moet het poortnummer met één worden verhoogd (bijvoorbeeld, het eerste knooppunt van de transactie op poort 3000, is de tweede is op 3001, het derde op 3002, enz.).

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om u te concentreren op toepassings- en chaincode ontwikkelen op basis van uw netwerk Hyperledger consortium blockchain.
