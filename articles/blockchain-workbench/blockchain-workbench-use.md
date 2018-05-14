---
title: Toepassingen gebruiken in Azure Blockchain Workbench
description: Opdrachten voor het gebruik van de toepassing in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 35a45947c2766ccc5e64f4c3523ce163a9680680
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
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

Voor het maken van een nieuw contract, moet u lid zijn van de **AllowedInstanceRoles** rol. 

1. Selecteer in de sectie Blockchain Workbench toepassing, de tegel toepassing waarin het contract dat u wilt maken. Een lijst met actieve contracten worden weergegeven.

2. Selecteer voor het maken van een nieuw contract **nieuw contract**.

    ![Knop Nieuw contract](media/blockchain-workbench-use/contract-list.png)

3. De **nieuw contract** deelvenster wordt weergegeven. Geef de aanvankelijke parameterwaarden. Selecteer **Maken**.

    ![Nieuwe contract deelvenster](media/blockchain-workbench-use/new-contract.png)

    De zojuist gemaakte contract wordt weergegeven in de lijst met de actieve contracten.

    ![Lijst met actieve contracten](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Maatregelen nemen op contract

1. Selecteer in de sectie van de toepassing Blockchain Workbench, de toepassing tegel met de opdracht om de actie te ondernemen.

    ![Lijst met toepassingen](media/blockchain-workbench-use/apps-list.png)

2. Selecteer het contract in de lijst.

    ![Lijst met contracten](media/blockchain-workbench-use/select-contract.png)

    Meer informatie over het contract worden in verschillende secties weergegeven. 

    ![Contractdetails](media/blockchain-workbench-use/contract-details.png)

    | Sectie  | Beschrijving  |
    |---------|---------|
    | Status | Geeft een lijst van de huidige voortgang binnen de fasen van het contract |
    | Details | De huidige waarden van het contract |
    | Bewerking | Meer informatie over de laatste actie |
    | Activiteit | Historie van de overeenkomst |
    
3. In de **actie** sectie **maatregelen**.

4. De details over de huidige status van het contract worden in een deelvenster weergegeven. Selecteer de actie die u wilt maken in de vervolgkeuzelijst. 

    ![Actie ondernemen](media/blockchain-workbench-use/take-action.png)

5. Selecteer **Execute** de actie te ondernemen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench oplossen](blockchain-workbench-troubleshooting.md)