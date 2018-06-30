---
title: Azure-Stack Ethereum oplossingssjabloon
description: Aangepaste oplossingssjablonen gebruiken voor het implementeren en configureren van een consortium Ethereum-netwerk op Azure-Stack
services: azure-stack
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 6/28/2018
ms.topic: article
ms.service: azure-stack
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: 4c2b0cda2d4144cde733f7f57ac6311e1a69f547
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114727"
---
# <a name="azure-stack-ethereum-solution-templates"></a>Azure-Stack Ethereum oplossing-sjablonen

De sjabloon van de oplossing Ethereum is ontworpen voor het u gemakkelijker en sneller te implementeren en configureren van een lid van meerdere consortium Ethereum netwerk met minimale kennis van Azure en Ethereum.

Elk lid kan een handvol gebruikersinvoer en een implementatie met één klik via de Azure-Stack-beheerdersportal hun netwerk footprint inrichten. Netwerk voetafdruk van elk lid bestaat uit een set van netwerktaakverdeling transactie knooppunten met die een toepassing of de gebruiker communiceren kan als u wilt indienen transacties, een reeks analysemodel knooppunten Registreer transacties en een netwerk virtuele toestel (NVA). Een stap van de volgende verbinding maakt verbinding met de NVAs een volledig geconfigureerde meerdere leden blockchain-netwerk maken.

## <a name="prerequisites"></a>Vereisten

Downloaden van de volgende [vanuit de Marketplace](azure-stack-download-azure-marketplace-item.md):

* Ubuntu Server 16.04 LTS versie 16.04.201802220
* Windows Server 2016 
* Aangepast Script voor Linux 2.0 
* Aangepaste scriptextensie 

Zie voor meer informatie over scenario's voor blockchain op Azure [Ethereum bewijs van werkzaamheden consortium oplossingssjabloon](../blockchain-workbench/ethereum-deployment-guide.md).

Een Azure-abonnement die ondersteuning biedt voor het implementeren van verschillende virtuele machines is vereist. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

## <a name="deployment-architecture"></a>Architectuur voor implementatie

Deze oplossingssjabloon kan één of meerdere lid Ethereum consortium netwerk kunt implementeren. Het virtuele netwerk is in een keten-topologie-bronnen virtuele netwerkapparaat en de verbinding verbonden. 

## <a name="deployment-use-cases"></a>Gebruiksvoorbeelden voor implementatie

De sjabloon Ethereum consortium voor opvulteken en lid join in tal van manieren kunt implementeren, hier zijn die we hebben getest:
- Implementeer op een Azure-Stack van meerdere knooppunten, met Azure AD of AD FS, potentiële klanten en lid met hetzelfde abonnement of met verschillende abonnementen behoren.
- Implementeer lead en lid met hetzelfde abonnement op een Azure-Stack van één knooppunt (met Azure AD).

### <a name="standalone-and-consortium-leader-deployment"></a>Implementatie van zelfstandige en consortium opvulteken

Het eerste lid footprint configureert de consortium opvulteken-sjabloon in het netwerk. 

1. Download de [opvulteken sjabloon vanuit GitHub](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/ConsortiumLeader/mainTemplate.json)
2. Selecteer in de Azure-Stack-beheerportal, **Nieuw > sjabloonimplementatie** voor het implementeren van een aangepaste sjabloon.
3. Selecteer **template bewerken** de nieuwe aangepaste sjabloon te bewerken.
4. Klik in het deelvenster bewerking aan de rechterkant kopieert en plakt u de sjabloon opvulteken JSON die u eerder hebt gedownload.
    
    ![Opvulteken sjabloon bewerken](media/azure-stack-ethereum/edit-leader-template.png)

5. Selecteer **Opslaan**.
6. Selecteer **parameters bewerken** en de sjabloonparameters voor uw implementatie te voltooien.
    
    ![Sjabloonparameters opvulteken bewerken](media/azure-stack-ethereum/edit-leader-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | De tekenreeks die wordt gebruikt als basis voor de naamgeving van de geïmplementeerde resources. | Alfanumerieke tekens met lengte 1 tot en met 6 | Eth
    AUTHTYPE | De methode voor verificatie bij de virtuele machine. | Wachtwoord of SSH openbare sleutel | Wachtwoord
    ADMINUSERNAME | Gebruikersnaam voor de beheerder van elke geïmplementeerde virtuele machine | 1 - 64 tekens | gethadmin
    ADMINPASSWORD (verificatietype = wachtwoord)| Het wachtwoord voor het administrator-account voor elk van de geïmplementeerde virtuele machines. Het wachtwoord moet bevatten 3 van de volgende vereisten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. <br />Terwijl alle VM's in eerste instantie hetzelfde wachtwoord zijn, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|
    ADMINSSHKEY (verificatietype = sshPublicKey) | De sleutel voor secure shell gebruikt voor externe aanmelding. | |
    GENESISBLOCK | JSON-tekenreeks voor aangepaste genesis blok. | |
    ETHEREUMACCOUNTPSSWD | Het beheerderswachtwoord dat wordt gebruikt om Ethereum account te beveiligen. | |
    ETHEREUMACCOUNTPASSPHRASE | De wachtwoordzin die wordt gebruikt voor het genereren van de persoonlijke sleutel die is gekoppeld aan het account Ethereum. | |
    ETHEREUMNETWORKID | De netwerk-ID van het consortium. | Een waarde tussen 5 en 999.999.999 gebruiken | 72
    CONSORTIUMMEMBERID | De ID die is gekoppeld aan elk lid van het netwerk consortium.   | Deze ID moet uniek zijn in het netwerk. | 0
    NUMMININGNODES | Het aantal knooppunten van de analysestructuur. | Tussen 2 en 15. | 2
    MNNODEVMSIZE | VM-grootte van de knooppunten van het analysemodel. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | De prestaties van de opslag van de knooppunten van het analysemodel. | | Standard_LRS
    NUMTXNODES | Het aantal knooppunten van de transactie. | Tussen 1 en 5. | 1
    TXNODEVMSIZE | VM-grootte van de transactie-knooppunten. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | De prestaties van de opslag van de transactie-knooppunten. | | Standard_LRS
    BASEURL | Basis-URL ophalen van de betreffende sjablonen uit. | Gebruik de standaardwaarde, tenzij u wilt aanpassen van de van implementatiesjablonen. | 

7. Selecteer **OK**.
8. In **aangepaste implementatie**, geef **abonnement**, **resourcegroep**, en **locatie voor resourcegroep**.
    
    ![Opvulteken implementatieparameters](media/azure-stack-ethereum/leader-deployment-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    Abonnement | Het abonnement waaraan het consortium netwerk implementeren | | Abonnement voor verbruik
    Resourcegroep | De resourcegroep waartoe het consortium netwerk implementeren. | | EthereumResources
    Locatie | De Azure-regio voor de resourcegroep. | | lokaal

8. Selecteer **Maken**.

Implementatie kunt duurt ongeveer 20 minuten of langer duren.

Nadat de implementatie is voltooid, kunt u de samenvatting voor implementatie bekijken **Microsoft. Sjabloon** in het gedeelte van de implementatie van de resourcegroep. De samenvatting bevat de uitvoerwaarden die kunnen worden gebruikt voor het consortium leden toevoegen.

Blader om te controleren of de implementatie van opvulteken, opvulteken van beheer-site. Adres van de beheer-site vindt u in het gedeelte van de uitvoer van **Microsoft.Template** implementatie.  

![Implementatieoverzicht opvulteken](media/azure-stack-ethereum/ethereum-node-status.png)

### <a name="joining-consortium-member-deployment"></a>Lid worden van consortium lid implementatie

1. Download de [consortium lid sjabloon vanuit GitHub](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/JoiningMember/mainTemplate.json)
2. Selecteer in de Azure-Stack-beheerportal, **Nieuw > sjabloonimplementatie** voor het implementeren van een aangepaste sjabloon.
3. Selecteer **template bewerken** de nieuwe aangepaste sjabloon te bewerken.
4. Klik in het deelvenster bewerking aan de rechterkant kopieert en plakt u de sjabloon opvulteken JSON die u eerder hebt gedownload.
5. Selecteer **Opslaan**.
6. Selecteer **parameters bewerken** en de sjabloonparameters voor uw implementatie te voltooien.

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | De tekenreeks die wordt gebruikt als basis voor de naamgeving van de geïmplementeerde resources. | Alfanumerieke tekens met lengte 1 tot en met 6 | Eth
    AUTHTYPE | De methode voor verificatie bij de virtuele machine. | Wachtwoord of SSH openbare sleutel | Wachtwoord
    ADMINUSERNAME | Gebruikersnaam voor de beheerder van elke geïmplementeerde virtuele machine | 1 - 64 tekens | gethadmin
    ADMINPASSWORD (verificatietype = wachtwoord)| Het wachtwoord voor het administrator-account voor elk van de geïmplementeerde virtuele machines. Het wachtwoord moet bevatten 3 van de volgende vereisten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. <br />Terwijl alle VM's in eerste instantie hetzelfde wachtwoord zijn, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|
    ADMINSSHKEY (verificatietype = sshPublicKey) | De sleutel voor secure shell gebruikt voor externe aanmelding. | |
    CONSORTIUMMEMBERID | De ID die is gekoppeld aan elk lid van het netwerk consortium.   | Deze ID moet uniek zijn in het netwerk. | 0
    NUMMININGNODES | Het aantal knooppunten van de analysestructuur. | Tussen 2 en 15. | 2
    MNNODEVMSIZE | VM-grootte van de knooppunten van het analysemodel. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | De prestaties van de opslag van de knooppunten van het analysemodel. | | Standard_LRS
    NUMTXNODES | Het aantal knooppunten van de transactie. | Tussen 1 en 5. | 1
    TXNODEVMSIZE | VM-grootte van de transactie-knooppunten. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | De prestaties van de opslag van de transactie-knooppunten. | | Standard_LRS
    CONSORTIUMDATA | De URL die verwijst naar de relevante consortium configuratiegegevens geleverd door de implementatie van een ander lid. Deze waarde kan worden gevonden op opvulteken van implementatie uitvoer. | |
    REMOTEMEMBERVNETADDRESSSPACE | Het NVA-IP-adres van het opvulteken. Deze waarde kan worden gevonden op opvulteken van implementatie uitvoer. | | 
    REMOTEMEMBERNVAPUBLICIP | Het NVA-IP-adres van het opvulteken. Deze waarde kan worden gevonden op opvulteken van implementatie uitvoer. | | 
    CONNECTIONSHAREDKEY | Een vooraf vastgestelde geheim tussen de leden van het netwerk consortium die een verbinding tot stand brengt. | |
    BASEURL | Basis-URL voor de sjabloon. | Gebruik de standaardwaarde, tenzij u wilt aanpassen van de van implementatiesjablonen. | 

7. Selecteer **OK**.
8. In **aangepaste implementatie**, geef **abonnement**, **resourcegroep**, en **locatie voor resourcegroep**.

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    Abonnement | Het abonnement waaraan het consortium netwerk implementeren | | Abonnement voor verbruik
    Resourcegroep | De resourcegroep waartoe het consortium netwerk implementeren. | | MemberResources
    Locatie | De Azure-regio voor de resourcegroep. | | lokaal

8. Selecteer **Maken**.

Implementatie kunt duurt ongeveer 20 minuten of langer duren.

Nadat de implementatie is voltooid, kunt u de samenvatting voor implementatie bekijken **Microsoft.Template** in het gedeelte van de implementatie van de resourcegroep. De samenvatting bevat uitvoerwaarden die kunnen worden gebruikt voor verbinding consortium leden.

Blader om te controleren of de implementatie van het lid, lid zijn van beheer-site. Adres van de beheer-site vindt u in het gedeelte van de uitvoer van Microsoft.Template-implementatie.

![Implementatieoverzicht lid](media/azure-stack-ethereum/ethereum-node-status-2.png)

Zoals u in de afbeelding, status van de knooppunten van het lid is **niet actief**. Dit is omdat de verbinding tussen lid en opvulteken is niet gemaakt. De verbinding tussen een lid en opvulteken is een tweerichtingsrelatie tussen verbinding. Wanneer u lid implementeert, maakt-sjabloon automatisch de verbinding van lid voor het opvulteken. De verbinding van opvulteken lid Ga te maken met de volgende stap.

### <a name="connect-member-and-leader"></a>Verbinding maken met het lid en opvulteken

Deze sjabloon maakt een verbinding van het opvulteken naar een extern lid. 

1. Download de [lid en opvulteken sjabloon verbinding vanuit GitHub](https://raw.githubusercontent.com/seyadava/AzureStack-QuickStart-Templates-1/blockchain_nva/eth/marketplace/Connection/mainTemplate.json)
2. Selecteer in de Azure-Stack-beheerportal, **Nieuw > sjabloonimplementatie** voor het implementeren van een aangepaste sjabloon.
3. Selecteer **template bewerken** de nieuwe aangepaste sjabloon te bewerken.
4. Klik in het deelvenster bewerking aan de rechterkant kopieert en plakt u de sjabloon opvulteken JSON die u eerder hebt gedownload.
    
    ![Bewerken verbinding maken met de sjabloon](media/azure-stack-ethereum/edit-connect-template.png)

5. Selecteer **Opslaan**.
6. Selecteer **parameters bewerken** en de sjabloonparameters voor uw implementatie te voltooien.
    
    ![Bewerken verbinding Sjabloonparameters](media/azure-stack-ethereum/edit-connect-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    MEMBERNAMEPREFIX | Het voorvoegsel van opvulteken. Deze waarde kan worden gevonden op opvulteken van implementatie uitvoer.  | Alfanumerieke tekens met lengte 1 tot en met 6 | |
    MEMBERROUTETABLENAME | Naam van het opvulteken routetabel. Deze waarde kan worden gevonden op opvulteken van implementatie uitvoer. |  | 
    REMOTEMEMBERVNETADDRESSSPACE | De adresruimte van het lid. Deze waarde vindt u op de implementatie-uitvoer van het lid. | |
    CONNECTIONSHAREDKEY | Een vooraf vastgestelde geheim tussen de leden van het netwerk consortium die een verbinding tot stand brengt.  | |
    REMOTEMEMBERNVAPUBLICIP | Het NVA-IP-adres van het lid. Deze waarde vindt u op de implementatie-uitvoer van het lid. | |
    MEMBERNVAPRIVATEIP | Opvulteken van persoonlijke NVA IP-adres. Deze waarde kan worden gevonden op opvulteken van implementatie uitvoer. | |
    LOCATIE | Locatie van uw Azure-Stack-omgeving. | | lokaal
    BASEURL | Basis-URL voor de sjabloon. | Gebruik de standaardwaarde, tenzij u wilt aanpassen van de van implementatiesjablonen. | 

7. Selecteer **OK**.
8. In **aangepaste implementatie**, geef **abonnement**, **resourcegroep**, en **locatie voor resourcegroep**.
    
    ![Verbinding maken met de implementatieparameters](media/azure-stack-ethereum/connect-deployment-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    Abonnement | Het opvulteken abonnement. | | Abonnement voor verbruik
    Resourcegroep | Het opvulteken resourcegroep. | | EthereumResources
    Locatie | De Azure-regio voor de resourcegroep. | | lokaal

8. Selecteer **Maken**.

Nadat de implementatie is voltooid, duurt het enkele minuten opvulteken en lid communicatie wordt gestart. Vernieuwen om te controleren of de implementatie, beheer-site van het lid. Status van de knooppunten van het lid moet worden uitgevoerd. 

![De implementatie controleren](media/azure-stack-ethereum/ethererum-node-status-3.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Ethereum en Azure, [Blockchain technologie en toepassingen | Microsoft Azure](https://azure.microsoft.com/solutions/blockchain/).
- Zie voor meer informatie over scenario's voor blockchain op Azure [Ethereum bewijs van werkzaamheden consortium oplossingssjabloon](../blockchain-workbench/ethereum-deployment-guide.md).