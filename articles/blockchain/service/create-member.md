---
title: Maak een Azure Blockchain-Service met behulp van de Azure portal
description: Azure Blockchain-service gebruiken voor het maken van een consortium lid.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 5cb236dc38ae41b202004b7e4806d8129378cfdb
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417509"
---
# <a name="quickstart-create-an-azure-blockchain-service-using-the-azure-portal"></a>Quickstart: Maak een Azure Blockchain-Service met behulp van de Azure portal

Azure Blockchain-Service is een blockchain-platform dat u uw bedrijfslogica binnen een slimme contract kunt uitvoeren. In deze Quick Start laat zien hoe u aan de slag met het maken van een beheerde grootboek met behulp van de Azure portal.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-managed-ledger"></a>Maken van een beheerde grootboek

Azure Blockchain-Service wordt gemaakt met een gedefinieerde set met reken- en opslagresources.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer in de linkerbovenhoek van Azure Portal **Een resource maken**.
1. Selecteer **Blockchain** > **Azure Blockchain-Service**.
1. Het sjabloon kunt aanvullen.

    ![Service maken](./media/create-member/create-member.png)

    Instelling | Description
    --------|------------
    Blockchain lid | Kies een unieke naam ter identificatie van uw Azure Blockchain Service lid. De naam van de blockchain-lid mag alleen kleine letters en cijfers. Het eerste teken moet een letter zijn. De waarde moet tussen 2 en 20 tekens lang zijn.
    Abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken voor uw service. Als u meerdere abonnementen hebt, kiest u het abonnement waarin u wordt gefactureerd voor de resource.
    Resourcegroep | Een nieuwe resourcegroepnaam of een bestaande naam uit uw abonnement.
    Regio | Locatie moet hetzelfde zijn voor alle leden van het consortium.
    Lid-accountwachtwoord | Het wachtwoord voor het lid wordt gebruikt voor het versleutelen van de persoonlijke sleutel voor het Ethereum-account dat is gemaakt voor het lid. U gebruikt de lidaccount en het wachtwoord van het account lid voor consortium management.
    De naam van de Consortium | Voer een unieke naam voor een nieuwe consortium. Als u lid wordt van een consortium via een uitnodiging, is de waarde het consortium dat u samenvoegt.
    Description | Beschrijving van het consortium.
    Protocol |  Preview-versie biedt ondersteuning voor het Quorum-protocol.
    Prijzen | De configuratie van de knooppunten voor uw nieuwe service. selecteer **Standaard**. 2 validator knooppunten en 1 transactie-knooppunt zijn de standaardinstellingen.
    Wachtwoord voor transactie-knooppunt | Het wachtwoord voor het lid van de standaard transactie knooppunt. Gebruik het wachtwoord voor basisverificatie wordt gebruikt bij het verbinden met blockchain adreslid standaard transactie openbare eindpunt van het knooppunt.

1. Selecteer **maken** voor het inrichten van de service. De inrichting duurt ongeveer 10 minuten.
1. Selecteer **meldingen** op de werkbalk om het implementatieproces te bewaken.
1. Na de implementatie, gaat u naar uw blockchain-lid.

Selecteer **overzicht**, kunt u de algemene informatie bekijken over uw service, inclusief het RootContract adres en de lid-account.

![Overzicht van Blockchain-lid](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Resources opschonen

Voor de volgende quickstart of zelfstudie kunt u het lid dat u hebt gemaakt. Wanneer u niet meer nodig hebt, kunt u de resources verwijderen door het verwijderen van de `myResourceGroup` resourcegroep die u hebt gemaakt door de Azure Blockchain-Service.

De resourcegroep verwijderen:

1. In de Azure-portal, gaat u naar **resourcegroep** in het navigatiedeelvenster links en selecteert u de resourcegroep die u wilt verwijderen.
2. Selecteer **Resourcegroep verwijderen**. Bevestig de verwijdering door de naam van de resourcegroep in te voeren en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [MetaMask gebruiken voor verbinding maken en implementeren van een slimme contract](connect-metamask.md)