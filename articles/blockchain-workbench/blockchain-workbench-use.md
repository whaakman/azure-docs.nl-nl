---
title: Toepassingen gebruiken in Azure Blockchain Workbench
description: Opdrachten voor het gebruik van de toepassing in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: b15d4980875b805b49f7dc1cdb941e607232f3cb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258036"
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Toepassingen gebruiken in Azure Blockchain Workbench

U kunt Blockchain Workbench gebruiken voor het maken en er acties op contracten. U kunt ook weergeven contract gegevens zoals geschiedenis van de status en de transactie.

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie [Azure Blockchain Workbench-implementatie](blockchain-workbench-deploy.md) voor meer informatie over de implementatie
* Een toepassing geïmplementeerde blockchain in Blockchain Workbench. Zie [een blockchain-toepassing maken in Azure Blockchain Workbench]()

[Open de Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) in uw browser.

![Blockchain Workbench](media/blockchain-workbench-use/workbench.png)

U moet zich aanmelden als lid van de Blockchain Workbench. Als er geen toepassingen die zijn vermeld zijn, bent u lid zijn van Blockchain Workbench maar geen lid van alle toepassingen. De beheerder Blockchain Workbench kunt leden toewijzen aan toepassingen.

## <a name="create-new-contract"></a>Nieuwe contract maken. 

Voor het maken van een nieuw contract, moet u lid zijn opgegeven als een contract **initiator**. Zie voor informatie over het definiëren van toepassingsrollen en initiators voor het contract [werkstromen in het overzicht van de configuratie](blockchain-workbench-configuration-overview.md#workflows). Zie voor meer informatie over het toewijzen van leden aan de toepassingsrollen [een lid toevoegen aan de toepassing](blockchain-workbench-manage-users.md#add-member-to-application).

1. Selecteer in de sectie Blockchain Workbench toepassing, de tegel toepassing waarin het contract dat u wilt maken. Een lijst met actieve contracten worden weergegeven.

2. Selecteer voor het maken van een nieuw contract **nieuw contract**.

    ![Knop Nieuw contract](media/blockchain-workbench-use/contract-list.png)

3. De **nieuw contract** deelvenster wordt weergegeven. Geef de aanvankelijke parameterwaarden. Selecteer **Maken**.

    ![Nieuwe contract deelvenster](media/blockchain-workbench-use/new-contract.png)

    De zojuist gemaakte contract wordt weergegeven in de lijst met de actieve contracten.

    ![Lijst met actieve contracten](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Maatregelen nemen op contract

Afhankelijk van de status is het contract, leden kunnen maatregelen overstappen naar de volgende status van het contract. Acties zijn gedefinieerd als [overgangen](blockchain-workbench-configuration-overview.md#transitions) binnen een [status](blockchain-workbench-configuration-overview.md#states). Leden die behoren tot een toegestane toepassing of het exemplaar van de rol voor de overgang kunnen de actie te ondernemen. 

1. Selecteer in de sectie van de toepassing Blockchain Workbench, de toepassing tegel met de opdracht om de actie te ondernemen.
2. Selecteer het contract in de lijst. Meer informatie over het contract worden in verschillende secties weergegeven. 

    ![Contractdetails](media/blockchain-workbench-use/contract-details.png)

    | Sectie  | Beschrijving  |
    |---------|---------|
    | Status | Geeft een lijst van de huidige voortgang binnen de fasen van het contract |
    | Details | De huidige waarden van het contract |
    | Bewerking | Meer informatie over de laatste actie |
    | Activiteit | Historie van de overeenkomst |
    
3. In de **actie** sectie **maatregelen**.

4. De details over de huidige status van het contract worden in een deelvenster weergegeven. Selecteer de actie die u wilt maken in de vervolgkeuzelijst. 

    ![Selecteer actie](media/blockchain-workbench-use/choose-action.png)

5. Selecteer **maatregelen** de actie te initiëren.
6. Als parameters vereist voor de actie zijn, geeft u de waarden voor de actie.

    ![Actie ondernemen](media/blockchain-workbench-use/take-action.png)

7. Selecteer **maatregelen** de actie uitvoeren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench oplossen](blockchain-workbench-troubleshooting.md)