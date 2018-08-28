---
title: Met behulp van toepassingen in Azure Blockchain Workbench
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
ms.openlocfilehash: e17a9275792e3a7fdbea6e3b95e596eaa15f4359
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105808"
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Met behulp van toepassingen in Azure Blockchain Workbench

Blockchain Workbench kunt u maken en acties uitvoeren op opdrachten. Ook vindt u informatie zoals de geschiedenis van de status en transactie-contract.

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie, [Azure Blockchain Workbench-implementatie](blockchain-workbench-deploy.md) voor meer informatie over implementatie
* Een geïmplementeerde blockchain-toepassing in Blockchain Workbench. Zie [een blockchain-toepassing maken in Azure Blockchain Workbench](blockchain-workbench-create-app.md)

[Open de Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) in uw browser.

![Blockchain Workbench](media/blockchain-workbench-use/workbench.png)

U moet zich aanmelden als lid van de Blockchain Workbench. Als er geen toepassingen die worden vermeld zijn, bent u lid zijn van Blockchain Workbench, maar geen lid van alle toepassingen. De beheerder van de Blockchain Workbench kunt leden toewijzen aan toepassingen.

## <a name="create-new-contract"></a>Nieuwe opdracht maken 

Voor het maken van een nieuwe overeenkomst, moet u lid zijn opgegeven als een contract **initiator**. Zie voor informatie over het definiëren van toepassingsrollen en initiators voor het contract [werkstromen in het overzicht van de configuratie van](blockchain-workbench-configuration-overview.md#workflows). Zie voor meer informatie over het toewijzen van leden aan toepassingsrollen [lid toevoegen aan toepassing](blockchain-workbench-manage-users.md#add-member-to-application).

1. In de sectie van de toepassing Blockchain Workbench, selecteer de toepassing-tegel met de opdracht die u wilt maken. Een lijst met actieve contracten worden weergegeven.

2. Voor het maken van een nieuwe overeenkomst selecteert **nieuwe contract**.

    ![Knop nieuwe contract](media/blockchain-workbench-use/contract-list.png)

3. De **nieuwe contract** deelvenster wordt weergegeven. De waarden van de aanvankelijke parameters opgeven. Selecteer **Maken**.

    ![Nieuwe contract deelvenster](media/blockchain-workbench-use/new-contract.png)

    De zojuist gemaakte contract wordt weergegeven in de lijst met de andere actieve contracten.

    ![Lijst met actieve contracten](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Actie ondernemen voor contract

Afhankelijk van de status is het contract, leden kunnen ondernemen om een overgang naar de volgende status van het contract. Acties zijn gedefinieerd als [overgangen](blockchain-workbench-configuration-overview.md#transitions) binnen een [status](blockchain-workbench-configuration-overview.md#states). Leden die behoren tot een toegestane toepassing of het exemplaar van de rol voor de overgang kunnen duren voordat de actie. 

1. In de sectie van de toepassing Blockchain Workbench, selecteer de toepassing-tegel met de opdracht om de actie te ondernemen.
2. Selecteer de overeenkomst in de lijst. Meer informatie over het contract worden weergegeven in de verschillende secties. 

    ![Contractdetails](media/blockchain-workbench-use/contract-details.png)

    | Sectie  | Beschrijving  |
    |---------|---------|
    | Status | Geeft een lijst van de huidige voortgang binnen de fasen van het contract |
    | Details | De huidige waarden van het contract |
    | Bewerking | Meer informatie over de laatste actie |
    | Activiteit | Historie van de overeenkomst |
    
3. In de **actie** sectie, selecteer **actie ondernemen**.

4. De details over de huidige status van de opdracht worden in een deelvenster weergegeven. Kies de actie die u wilt maken in de vervolgkeuzelijst. 

    ![Kies actie](media/blockchain-workbench-use/choose-action.png)

5. Selecteer **actie ondernemen** naar de actie start.
6. Als parameters vereist voor de actie zijn, geeft u de waarden voor de actie.

    ![Actie ondernemen](media/blockchain-workbench-use/take-action.png)

7. Selecteer **actie ondernemen** voor het uitvoeren van de actie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Problemen oplossen met Azure Blockchain Workbench](blockchain-workbench-troubleshooting.md)
