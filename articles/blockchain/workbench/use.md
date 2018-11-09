---
title: Met behulp van toepassingen in Azure Blockchain Workbench
description: Opdrachten voor het gebruik van de toepassing in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 4fe6f164882ffce7bf22ec0c0b94107abcf6a20e
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242661"
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Met behulp van toepassingen in Azure Blockchain Workbench

U kunt Blockchain Workbench gebruiken om contracten te maken en acties op contracten uit te voeren. Ook kunt u contractdetails bekijken zoals de status en transactie-geschiedenis.

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie [Azure Blockchain Workbench-implementatie](deploy.md) voor details over implementatie
* Een geïmplementeerde blockchain-toepassing in Blockchain Workbench. Zie [een blockchain-toepassing maken in Azure Blockchain Workbench](create-app.md)

[Open de Blockchain Workbench](deploy.md#blockchain-workbench-web-url) in uw browser.

![Blockchain Workbench](./media/use/workbench.png)

U moet zich aanmelden als lid van de Blockchain Workbench. Als er geen toepassingen worden vermeld, bent u lid van Blockchain Workbench, maar geen lid van een toepassing. De beheerder van de Blockchain Workbench kan leden toewijzen aan toepassingen.

## <a name="create-new-contract"></a>Nieuwe opdracht maken 

Voor het maken van een nieuwe overeenkomst, moet u als lid zijn opgegeven als een contract **initiator**. Zie voor informatie over het definiëren van toepassingsrollen en initiators voor het contract [werkstromen in het overzicht van de configuratie van](configuration.md#workflows). Zie voor meer informatie over het toewijzen van leden aan toepassingsrollen [lid toevoegen aan toepassing](manage-users.md#add-member-to-application).

1. In de sectie van de Blockchain Workbench-toepassing, selecteer de toepassingstegel met het contract dat u wilt maken. Een lijst met actieve contracten worden weergegeven.

2. Voor het maken van een nieuw contract selecteer **nieuwe contract**.

    ![Knop nieuwe contract](./media/use/contract-list.png)

3. Het **nieuwe contract** deelvenster wordt weergegeven. Geef de beginwaarden van de parameters op. Selecteer **Maken**.

    ![Nieuwe contract deelvenster](./media/use/new-contract.png)

    Het zojuist gemaakte contract wordt weergegeven in de lijst met de andere actieve contracten.

    ![Lijst met actieve contracten](./media/use/active-contracts.png)

## <a name="take-action-on-contract"></a>Actie ondernemen voor contract

Afhankelijk van de status van het contract kunnen leden acties ondernemen voor een overgang naar de volgende status van het contract. Acties zijn gedefinieerd als [overgangen](configuration.md#transitions) binnen een [status](configuration.md#states). Leden die behoren tot een toegestane toepassing of instantierol voor de overgang kunnen de actie ondernemen. 

1. In de sectie van de Blockchain Workbench-toepassing, selecteer de toepassingstegel met de opdracht om de actie te ondernemen.
2. Selecteer het contract in de lijst. Meer informatie over het contract wordt weergegeven in de verschillende secties. 

    ![Contractdetails](./media/use/contract-details.png)

    | Sectie  | Beschrijving  |
    |---------|---------|
    | Status | Geeft een lijst van de huidige voortgang binnen de fasen van het contract |
    | Details | De huidige waarden van het contract |
    | Bewerking | Meer informatie over de laatste actie |
    | Activiteit | Historie van de overeenkomst |
    
3. In de sectie **actie**, selecteer **actie ondernemen**.

4. De details over de huidige status van de opdracht worden in een deelvenster weergegeven. Kies de actie die u wilt uitvoeren in de vervolgkeuzelijst. 

    ![Kies actie](./media/use/choose-action.png)

5. Selecteer **actie ondernemen** om de actie te starten.
6. Als parameters vereist zijn voor de actie, geeft u de waarden voor de actie.

    ![Actie ondernemen](./media/use/take-action.png)

7. Selecteer **actie ondernemen** voor het uitvoeren van de actie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Problemen oplossen met Azure Blockchain Workbench](troubleshooting.md)
