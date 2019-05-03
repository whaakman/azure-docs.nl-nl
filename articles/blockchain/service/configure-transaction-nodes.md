---
title: Azure Blockchain Service transactie knooppunten configureren
description: Azure Blockchain Service transactie knooppunten configureren
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: dffeb81ae1eb244c38639a1241c0581e6fcdf94a
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027958"
---
# <a name="configure-azure-blockchain-service-transaction-nodes"></a>Azure Blockchain Service transactie knooppunten configureren

Om te communiceren met Azure Blockchain-Service, doet u dat via de verbinding te maken met een of meer transactie-knooppunten in uw blockchain-lid.  Om te communiceren met de transactie-knooppunten, moet u uw knooppunten voor toegang configureren.

## <a name="prerequisites"></a>Vereisten

* [Een Azure Blockchain-lid maken](create-member.md)

## <a name="transaction-node-overview"></a>Overzicht van de transactie-knooppunt

Transactie-knooppunten worden gebruikt voor het verzenden van blockchain-transacties met Azure Blockchain-Service via een openbaar eindpunt. Het knooppunt van de transactie standaard bevat de persoonlijke sleutel van het Ethereum-account dat is geregistreerd op de blockchain, en als zodanig kan niet worden verwijderd.

Het hulpprogramma voor het knooppunt van de standaard-transactiedetails weergeven:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Service voor Azure Blockchain-lid. Selecteer **transactie knooppunten**.

    ![Standaard transactie knooppunt selecteren](./media/configure-transaction-nodes/nodes.png)

    Overzicht van details omvatten openbaar eindpunt-adressen en openbare sleutel.

## <a name="create-transaction-node"></a>Transactie-knooppunt maken

U kunt maximaal negen extra transactielogboeken knooppunten toevoegen aan een lid van uw blockchain, voor een totaal van de tien knooppunten van de transactie. Transactie-knooppunten toevoegt, kunt u meer schaalbaarheid of verdelen. Bijvoorbeeld, kan er een eindpunt van het knooppunt transactie voor verschillende toepassingen.

Een transactie-knooppunt toevoegen:

1. In de Azure-portal, gaat u aan een lid van uw Azure Blockchain-Service en selecteer **transactie knooppunten > toevoegen**.
1. De instellingen voor het nieuwe knooppunt van de transactie hebt voltooid.

    ![Transactie-knooppunt toevoegen](./media/configure-transaction-nodes/add-node.png)

    | Instelling | Description |
    |---------|-------------|
    | Name | Naam van de transactie-knooppunt. De naam wordt gebruikt om te maken van de DNS-adres van het eindpunt van de transactie-knooppunt. Bijvoorbeeld `newnode-myblockchainmember.blockchain.azure.com`. Naam van het knooppunt kan niet worden gewijzigd nadat deze is gemaakt. |
    | Wachtwoord | Stel een sterk wachtwoord. Gebruik het wachtwoord voor toegang tot het eindpunt van het knooppunt transactie met basisverificatie.

1. Selecteer **Maken**.

    Het duurt ongeveer 10 minuten een nieuwe transactie-knooppunt wordt ingericht. Extra transactielogboeken knooppunten kosten in rekening gebracht. Zie voor meer informatie over kosten [Azure-prijzen](https://aka.ms/ABSPricing).

## <a name="endpoints"></a>Eindpunten

Transactie-knooppunten hebben een unieke DNS-naam en openbare eindpunten.

Een transactie-knooppunt-eindpunt om details te bekijken:

1. In de Azure-portal, gaat u naar een van uw Azure Blockchain Service lid transactie knooppunten en selecteer **overzicht**.

    ![Eindpunten](./media/configure-transaction-nodes/endpoints.png)

Transactie-knooppunt eindpunten zijn beveiligd en verificatie vereisen. U kunt verbinding maken met een transactie-eindpunt met behulp van Azure AD-verificatie, HTTPS basisverificatie wordt gebruikt, en een toegangssleutel via HTTPS of Websocket via SSL.

### <a name="azure-active-directory-access-control"></a>Toegangsbeheer Azure Active Directory

Azure Blockchain-Service-transactie knooppunt eindpunten ondersteuning voor verificatie van Azure Active Directory (Azure AD). U kunt Azure AD-gebruikers, groepen en service-principal toegang verlenen aan uw eindpunt.

Verlenen van Azure AD-toegang beheren aan uw eindpunt:

1. Navigeer in de Azure-portal aan een lid van uw Azure Blockchain-Service en selecteer **transactie knooppunten > toegangsbeheer (IAM) > toevoegen > roltoewijzing toevoegen**.
1. Maak een nieuwe roltoewijzing voor een gebruiker, groep of service-principal (toepassingsrollen).

    ![IAM-functie toevoegen](./media/configure-transaction-nodes/add-role.png)

    | Instelling | Bewerking |
    |---------|-------------|
    | Rol | Selecteer **eigenaar**, **Inzender**, of **lezer**.
    | Toegang toewijzen aan | Selecteer **Azure AD-gebruiker, groep of service-principal**.
    | Selecteer | Zoeken naar de gebruiker, groep of service-principal die u wilt toevoegen.

1. Selecteer **opslaan** om toe te voegen van de roltoewijzing.

Zie voor meer informatie over Azure AD-toegangsbeheer [toegang tot Azure-resources met behulp van RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md)

Zie voor meer informatie over hoe u verbinding maakt met behulp van Azure AD-verificatie, [verbinding maken met uw AAD-verificatie gebruiken knooppunt](configure-aad.md).

### <a name="basic-authentication"></a>Basisverificatie

Voor de basisverificatie HTTPS, worden de referenties van de naam en het wachtwoord van de gebruiker in de HTTPS-header van de aanvraag doorgegeven aan het eindpunt.

Een transactie-knooppunt basisverificatie eindpunt informatie vindt u in Azure portal. Navigeer naar een van uw Azure Blockchain Service lid transactie knooppunten en selecteer **basisverificatie** in instellingen.

![Basisverificatie](./media/configure-transaction-nodes/basic.png)

De gebruikersnaam is de naam van het knooppunt en kan niet worden gewijzigd.

Voor het gebruik van de URL vervangen \<wachtwoord\> met het wachtwoord instellen wanneer het knooppunt is ingericht. U kunt het wachtwoord bijwerken door het selecteren van **wachtwoord opnieuw instellen**.

### <a name="access-keys"></a>Toegangssleutels

Voor verificatie van de sleutel van de toegang, is de toegangssleutel opgenomen in de eindpunt-URL. Wanneer het knooppunt van de transactie is ingericht, worden twee toegangssleutels worden gegenereerd. Een toegangssleutel kan worden gebruikt voor verificatie. Twee sleutels kunnen u wijzigings- en draaien sleutels.

U kunt een transactie-knooppunt toegang tot belangrijke details bekijken en kopiëren eindpuntadressen die de toegang tot sleutels bevatten. Navigeer naar een van uw Azure Blockchain Service lid transactie knooppunten en selecteer **toegangssleutels** in instellingen.

### <a name="firewall-rules"></a>Firewall-regels

Firewall-regels kunnen u beperken van de IP-adressen die u proberen kunnen om uw transactie-knooppunt te verifiëren.  Als er geen firewallregels zijn geconfigureerd voor uw transactie-knooppunt, kan deze niet kan worden geopend vanaf een partij.  

Als u wilt weergeven van een transactie knooppunt firewall-regels, gaat u naar een van uw Azure Blockchain Service lid transactie knooppunten en selecteer **Firewall-regels** in instellingen.

U kunt firewallregels toevoegen door de regelnaam van een te voeren, vanaf IP-adres en een laatste IP-adres in de **Firewall-regels** raster.

![Firewall-regels](./media/configure-transaction-nodes/firewall-rules.png)

Om in te schakelen:

* **Één IP-adres:** Hetzelfde IP-adres voor de eerste en laatste IP-adres configureren.
* **IP-adresbereik:** Configureer het begin en eind-IP-adresbereik. Een bereik op 10.221.34.0 begint en eindigt bij 10.221.34.255 zou bijvoorbeeld het gehele 10.221.34.xxx subnet inschakelen.
* **Alle IP-adressen toestaan:** Het eerste IP-adres op 0.0.0.0 en het IP-eindadres 255.255.255.255 configureren.

## <a name="connection-strings"></a>Verbindingsreeksen

De syntaxis van verbindingsreeks voor uw transactie-knooppunt is opgegeven voor de basic-verificatie of met behulp van toegangssleutels. Tekenreeksen voor databaseverbindingen, waaronder de toegang via HTTPS en WebSockets zijn opgegeven.

U kunt een transactie-knooppunt verbindingsreeksen weergeven en eindpuntadressen kopiëren. Navigeer naar een van uw Azure Blockchain Service lid transactie knooppunten en selecteer **verbindingsreeksen** in instellingen.

![Verbindingsreeksen](./media/configure-transaction-nodes/connection-strings.png)

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode is opgegeven voor het snel inschakelen verbinding te maken met uw transactie-knooppunt via Web3, Nethereum Web3js en Truffle.

U kunt een transactie-knooppunt-voorbeeldcode verbinding bekijken en kopiëren voor gebruik met populaire ontwikkeltools. Navigeer naar een van uw Azure Blockchain Service lid transactie knooppunten en selecteer **voorbeeldcode** in instellingen.

Kies het tabblad Web3 of Nethereum om weer te geven van de voorbeeldcode die u wilt gebruiken.

![Voorbeeldcode](./media/configure-transaction-nodes/sample-code.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Transactie-knooppunten met behulp van Azure CLI configureren](manage-cli.md)
