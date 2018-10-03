---
title: Azure Stack Ethereum blockchain-oplossingssjabloon
description: Zelfstudie-een aangepaste oplossing-sjablonen gebruiken om te implementeren en configureren van een consortium Ethereum blockchain-netwerk in Azure Stack
services: azure-stack
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 09/13/2018
ms.topic: tutorial
ms.service: azure-stack
ms.reviewer: seyadava
ms.custom: mvc
manager: femila
ms.openlocfilehash: acfa94799f36728f4e0041f1a51403edf6ffe37e
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239469"
---
# <a name="tutorial-use-the-azure-stack-ethereum-blockchain-solution-template"></a>Zelfstudie: Gebruik de sjabloon van de Azure Stack Ethereum blockchain-oplossing

De sjabloon van de Ethereum-oplossing is ontworpen voor u gemakkelijker en sneller om te implementeren en configureren van een consortium voor meerdere leden Ethereum blockchain-netwerk met minimale kennis van Azure en Ethereum.

Met een aantal van de invoer van gebruikers en een implementatie met één klik via de portal van Azure Stack-tenant inrichten elk lid van de netwerk-footprint. Netwerk-voetafdruk van elk lid bestaat uit een set met load balancing transactie knooppunten met die een toepassing of de gebruiker communiceren kan om in te dienen transacties, een set knooppunten van de analysestructuur voor vastleggen van transacties en een Network Virtual Appliance (NVA). Een stap van de volgende verbinding maakt verbinding met de NVA's voor het maken van een volledig geconfigureerde meerdere leden blockchain-netwerk.

Dit als u wilt instellen, u het volgende doen:

> [!div class="checklist"]
> * Kies een implementatiearchitectuur
> * Een zelfstandige, consortium leider of consortium lid netwerk implementeren

## <a name="prerequisites"></a>Vereisten

Download de meest recente items [vanuit de Marketplace](azure-stack-download-azure-marketplace-item.md):

* Ubuntu Server 16.04 LTS
* Windows Server 2016
* Aangepast Script voor Linux 2.0
* Aangepaste Scriptextensie voor Windows

Zie voor meer informatie over blockchainscenario's [Ethereum bewijs van werk consortium oplossingssjabloon](../blockchain/templates/ethereum-deployment.md).

## <a name="deployment-architecture"></a>Implementatie-architectuur

Deze oplossingssjabloon kan één of meerdere items lid Ethereum consortium network implementeren. Het virtuele netwerk is verbonden in een keten-topologie met behulp van Network Virtual Appliance en verbinding bronnen. 

## <a name="deployment-use-cases"></a>Implementatie van use cases

De sjabloon Ethereum consortium voor leider en lid join in tal van manieren kunt implementeren, zijn hier die we hebben getest:

- Implementeren in een Azure-Stack van meerdere knooppunten, met Azure AD of AD FS, potentiële klanten en lid met behulp van hetzelfde abonnement of met verschillende abonnementen.
- Implementeren in een Azure-Stack van één knooppunt (met Azure AD) potentiële klanten en lid met behulp van hetzelfde abonnement.

### <a name="standalone-and-consortium-leader-deployment"></a>Zelfstandige en consortium leider-implementatie

De sjabloon van de leider consortium Hiermee configureert u het eerste lid footprint in het netwerk. 

1. Download de [leider-sjabloon](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/ConsortiumLeader/mainTemplate.json)
2. Selecteer in de beheerportal van Azure Stack **+ een resource maken > sjabloonimplementatie** om vanuit een aangepaste sjabloon te implementeren.
3. Selecteer **template bewerken** naar de nieuwe aangepaste sjabloon te bewerken.
4. In het deelvenster bewerken aan de rechterkant, kopieer en plak de JSON die u eerder hebt gedownload van de leider-sjabloon.
    
    ![Leider sjabloon bewerken](media/azure-stack-ethereum/edit-leader-template.png)

5. Selecteer **Opslaan**.
6. Selecteer **parameters bewerken** en de parameters van de sjabloon voor uw implementatie te voltooien.
    
    ![Leider Sjabloonparameters bewerken](media/azure-stack-ethereum/edit-leader-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | De tekenreeks die wordt gebruikt als basis voor de naam van de geïmplementeerde resources. | Alfanumerieke tekens met lengte 1 tot en met 6 | Eth
    AUTHTYPE | De methode voor verificatie bij de virtuele machine. | Wachtwoord of SSH de openbare sleutel | Wachtwoord
    ADMINUSERNAME | Gebruikersnaam voor de beheerder van elke geïmplementeerde virtuele machine | 1 - 64 tekens | gethadmin
    ADMINPASSWORD (verificatietype = wachtwoord)| Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd. Het wachtwoord moet bestaan 3 van de volgende vereisten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. <br />Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|
    ADMINSSHKEY (verificatietype = sshPublicKey) | De veilige shell-sleutel die wordt gebruikt voor externe aanmelding. | |
    GENESISBLOCK | JSON-tekenreeks voor aangepaste genesis blokkeren.  Een waarde opgeeft voor deze parameter is optioneel. | |
    ETHEREUMACCOUNTPSSWD | Het beheerderswachtwoord dat wordt gebruikt om Ethereum-account te beveiligen. | |
    ETHEREUMACCOUNTPASSPHRASE | De wachtwoordzin die wordt gebruikt voor het genereren van de persoonlijke sleutel die is gekoppeld aan het Ethereum-account. | |
    ETHEREUMNETWORKID | De netwerk-ID van het consortium. | Elke waarde tussen 5 en 999.999.999 gebruiken | 72
    CONSORTIUMMEMBERID | De ID die is gekoppeld aan elk lid van het netwerk consortium.   | Deze ID moet uniek zijn in het netwerk. | 0
    NUMMININGNODES | Het aantal knooppunten van de analysestructuur. | Tussen 2 en 15. | 2
    MNNODEVMSIZE | VM-grootte van de analysestructuur-knooppunten. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | De prestaties van de opslagruimte van de analysestructuur-knooppunten. | | Standard_LRS
    NUMTXNODES | Het aantal knooppunten van de transactie. | Tussen 1 en 5. | 1
    TXNODEVMSIZE | VM-grootte van de transactie-knooppunten. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | De prestaties van de opslagruimte van de transactie-knooppunten. | | Standard_LRS
    BASEURL | Basis-URL om de al naar gelang sjablonen uit te verkrijgen. | Gebruik de standaardwaarde, tenzij u wilt aanpassen van de sjablonen voor implementatie. | 

7. Selecteer **OK**.
8. In **aangepaste implementatie**, geef **abonnement**, **resourcegroep**, en **resourcegroeplocatie**.
    
    ![Leider implementatieparameters](media/azure-stack-ethereum/leader-deployment-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    Abonnement | Het abonnement waaraan het implementeren van het netwerk consortium | | Gebruik-abonnement
    Resourcegroep | De resourcegroep waaraan de consortium network implementeren. | | EthereumResources
    Locatie | De Azure-regio voor de resourcegroep. | | lokaal

8. Selecteer **Maken**.

Implementatie kunt duurt ongeveer 20 minuten of langer duren.

Nadat de implementatie is voltooid, kunt u de samenvatting voor implementatie controleren **Microsoft. Sjabloon** in het gedeelte van de implementatie van de resourcegroep. De samenvatting bevat uitvoerwaarden die kunnen worden gebruikt om toe te voegen consortium leden.

Bladeren om te controleren of de implementatie van de leider, beheer-site van de leider. Siteadres admin vindt u in de sectie uitvoer van **Microsoft.Template** implementatie.  

![Leider implementatieoverzicht](media/azure-stack-ethereum/ethereum-node-status.png)

### <a name="joining-consortium-member-deployment"></a>Implementatie van consortium lid toevoegen

1. Download de [consortium lid sjabloon vanuit GitHub](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/JoiningMember/mainTemplate.json)
2. Selecteer in de beheerportal van Azure Stack **+ een resource maken > sjabloonimplementatie** om vanuit een aangepaste sjabloon te implementeren.
3. Selecteer **template bewerken** naar de nieuwe aangepaste sjabloon te bewerken.
4. In het deelvenster bewerken aan de rechterkant, kopieer en plak de JSON die u eerder hebt gedownload van de leider-sjabloon.
5. Selecteer **Opslaan**.
6. Selecteer **parameters bewerken** en de parameters van de sjabloon voor uw implementatie te voltooien.

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    NAMEPREFIX | De tekenreeks die wordt gebruikt als basis voor de naam van de geïmplementeerde resources. | Alfanumerieke tekens met lengte 1 tot en met 6 | Eth
    AUTHTYPE | De methode voor verificatie bij de virtuele machine. | Wachtwoord of SSH de openbare sleutel | Wachtwoord
    ADMINUSERNAME | Gebruikersnaam voor de beheerder van elke geïmplementeerde virtuele machine | 1 - 64 tekens | gethadmin
    ADMINPASSWORD (verificatietype = wachtwoord)| Het wachtwoord voor het beheerdersaccount voor elk van de virtuele machines die zijn geïmplementeerd. Het wachtwoord moet bestaan 3 van de volgende vereisten: 1 hoofdletter, 1 kleine letter, 1 cijfer en 1 speciaal teken. <br />Hoewel alle virtuele machines wordt in eerste instantie hetzelfde wachtwoord hebben, kunt u het wachtwoord kunt wijzigen na het inrichten.|12 - 72 tekens|
    ADMINSSHKEY (verificatietype = sshPublicKey) | De veilige shell-sleutel die wordt gebruikt voor externe aanmelding. | |
    CONSORTIUMMEMBERID | De ID die is gekoppeld aan elk lid van het netwerk consortium.   | Deze ID moet uniek zijn in het netwerk. | 0
    NUMMININGNODES | Het aantal knooppunten van de analysestructuur. | Tussen 2 en 15. | 2
    MNNODEVMSIZE | VM-grootte van de analysestructuur-knooppunten. | | Standard_A1
    MNSTORAGEACCOUNTTYPE | De prestaties van de opslagruimte van de analysestructuur-knooppunten. | | Standard_LRS
    NUMTXNODES | Het aantal knooppunten van de transactie. | Tussen 1 en 5. | 1
    TXNODEVMSIZE | VM-grootte van de transactie-knooppunten. | | Standard_A1
    TXSTORAGEACCOUNTTYPE | De prestaties van de opslagruimte van de transactie-knooppunten. | | Standard_LRS
    CONSORTIUMDATA | De URL die verwijst naar de relevante consortium configuratiegegevens worden geleverd door een ander lid van de implementatie. Deze waarde kan worden gevonden op implementatie-uitvoer van de leider. | |
    REMOTEMEMBERVNETADDRESSSPACE | De NVA-IP-adres van de leider. Deze waarde kan worden gevonden op implementatie-uitvoer van de leider. | | 
    REMOTEMEMBERNVAPUBLICIP | De NVA-IP-adres van de leider. Deze waarde kan worden gevonden op implementatie-uitvoer van de leider. | | 
    CONNECTIONSHAREDKEY | Een vooraf vastgestelde geheim tussen de leden van het netwerk consortium die een verbinding tot stand brengt. | |
    BASEURL | Basis-URL voor de sjabloon. | Gebruik de standaardwaarde, tenzij u wilt aanpassen van de sjablonen voor implementatie. | 

7. Selecteer **OK**.
8. In **aangepaste implementatie**, geef **abonnement**, **resourcegroep**, en **resourcegroeplocatie**.

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    Abonnement | Het abonnement waaraan het implementeren van het netwerk consortium | | Gebruik-abonnement
    Resourcegroep | De resourcegroep waaraan de consortium network implementeren. | | MemberResources
    Locatie | De Azure-regio voor de resourcegroep. | | lokaal

8. Selecteer **Maken**.

Implementatie kunt duurt ongeveer 20 minuten of langer duren.

Nadat de implementatie is voltooid, kunt u de samenvatting voor implementatie controleren **Microsoft.Template** in het gedeelte van de implementatie van de resourcegroep. De samenvatting bevat uitvoerwaarden die kunnen worden gebruikt om verbinding maken met consortium leden.

Blader om te controleren of de implementatie van het lid, van het lid beheerder site. U kunt admin siteadres vinden in de sectie uitvoer van Microsoft.Template implementatie.

![Implementatieoverzicht lid](media/azure-stack-ethereum/ethereum-node-status-2.png)

Zoals weergegeven in de afbeelding, de status van de knooppunten van het lid is **niet actief**. Dit is omdat de verbinding tussen lid en leider is niet gemaakt. De verbinding tussen lid en leider is een tweerichtingenverbinding. Wanneer u lid implementeert, maakt sjabloon automatisch de verbinding van lid aan de leider. Maakt de verbinding van de leider aan een lid van gaat u naar de volgende stap.

### <a name="connect-member-and-leader"></a>Verbinding maken met het lid en leader

Deze sjabloon maakt een verbinding van de leider aan een extern lid. 

1. Download de [verbinding maken met sjabloon lid en leider vanuit GitHub](https://raw.githubusercontent.com/Azure/AzureStack-QuickStart-Templates/master/ethereum-consortium-blockchain/marketplace/Connection/mainTemplate.json)
2. Selecteer in de beheerportal van Azure Stack **+ een resource maken > sjabloonimplementatie** om vanuit een aangepaste sjabloon te implementeren.
3. Selecteer **template bewerken** naar de nieuwe aangepaste sjabloon te bewerken.
4. In het deelvenster bewerken aan de rechterkant, kopieer en plak de JSON die u eerder hebt gedownload van de leider-sjabloon.
    
    ![Bewerken verbinding maken met sjabloon](media/azure-stack-ethereum/edit-connect-template.png)

5. Selecteer **Opslaan**.
6. Selecteer **parameters bewerken** en de parameters van de sjabloon voor uw implementatie te voltooien.
    
    ![Bewerken verbinding maken met Sjabloonparameters](media/azure-stack-ethereum/edit-connect-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    MEMBERNAMEPREFIX | Het voorvoegsel van de leider. Deze waarde kan worden gevonden op implementatie-uitvoer van de leider.  | Alfanumerieke tekens met lengte 1 tot en met 6 | |
    MEMBERROUTETABLENAME | De naam van de routetabel van de leider. Deze waarde kan worden gevonden op implementatie-uitvoer van de leider. |  | 
    REMOTEMEMBERVNETADDRESSSPACE | De adresruimte van het lid. Deze waarde kan worden gevonden op implementatie-uitvoer van het lid. | |
    CONNECTIONSHAREDKEY | Een vooraf vastgestelde geheim tussen de leden van het netwerk consortium die een verbinding tot stand brengt.  | |
    REMOTEMEMBERNVAPUBLICIP | De NVA-IP-adres van het lid. Deze waarde kan worden gevonden op implementatie-uitvoer van het lid. | |
    MEMBERNVAPRIVATEIP | Privé NVA IP-adres van de leider. Deze waarde kan worden gevonden op implementatie-uitvoer van de leider. | |
    LOCATIE | Locatie van uw Azure Stack-omgeving. | | lokaal
    BASEURL | Basis-URL voor de sjabloon. | Gebruik de standaardwaarde, tenzij u wilt aanpassen van de sjablonen voor implementatie. | 

7. Selecteer **OK**.
8. In **aangepaste implementatie**, geef **abonnement**, **resourcegroep**, en **resourcegroeplocatie**.
    
    ![Verbinding maken met implementatieparameters](media/azure-stack-ethereum/connect-deployment-parameters.png)

    Parameternaam | Beschrijving | Toegestane waarden | Voorbeeldwaarde
    ---------------|-------------|----------------|-------------
    Abonnement | Abonnement van de leider. | | Gebruik-abonnement
    Resourcegroep | De resourcegroep van de leider. | | EthereumResources
    Locatie | De Azure-regio voor de resourcegroep. | | lokaal

8. Selecteer **Maken**.

Nadat de implementatie is voltooid, duurt het enkele minuten voordat de leider en lid te starten communicatie. Vernieuwen om te controleren of de implementatie, beheer-site van het lid. Status van de knooppunten van het lid moet worden uitgevoerd. 

![De implementatie controleren](media/azure-stack-ethereum/ethererum-node-status-3.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Kies een implementatiearchitectuur
> * Een zelfstandige, consortium leider of consortium lid netwerk implementeren

Zie voor meer informatie over Ethereum- en Azure:

> [!div class="nextstepaction"]
> [Blockchain-technologie en toepassingen](https://azure.microsoft.com/solutions/blockchain/)
