---
title: Toepassingen gebruiken in Azure Blockchain Workbench
description: Zelfstudie over het gebruiken van toepassingscontracten in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 04/15/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 89c83ed6d02a60978bd54fb97d37063e34f6c0c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61128250"
---
# <a name="tutorial-using-applications-in-azure-blockchain-workbench"></a>Zelfstudie: Toepassingen gebruiken in Azure Blockchain Workbench

U kunt Blockchain Workbench gebruiken om contracten te maken en acties op contracten uit te voeren. Daarnaast kunt gegevens van contracten bekijken, zoals de status en transactiegeschiedenis.

U leert het volgende:

> [!div class="checklist"]
> * Een nieuw contract maken
> * Actie ondernemen op een contract

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie [Azure Blockchain Workbench-implementatie](deploy.md) voor details over implementatie
* Een geïmplementeerde blockchain-toepassing in Blockchain Workbench. Zie [Een blockchain-toepassing maken in Azure Blockchain Workbench](create-app.md) voor meer informatie.

[Open Blockchain Workbench](deploy.md#blockchain-workbench-web-url) in uw browser.

![Blockchain Workbench](./media/use/workbench.png)

U moet zich aanmelden als lid van Blockchain Workbench. Als er geen toepassingen worden vermeld, bent u lid van Blockchain Workbench, maar geen lid van een toepassing. De beheerder van de Blockchain Workbench kan leden toewijzen aan toepassingen.

## <a name="create-new-contract"></a>Nieuw contract maken

Voor het maken van een nieuwe overeenkomst, moet u lid zijn opgegeven als een contract **initiator**. Zie [Werkstromen in het configuratieoverzicht](configuration.md#workflows) voor informatie over het definiëren van toepassingsrollen en initiators voor het contract. Zie [Een lid toevoegen aan een toepassing](manage-users.md#add-member-to-application) voor informatie over het toewijzen van leden aan toepassingsrollen.

1. Selecteer in de sectie Blockchain Workbench-toepassing de toepassingstegel met het contract dat u wilt maken. U ziet een lijst met actieve contracten.

2. Selecteer **Nieuw contract** om een nieuw contract te maken.

    ![De knop Nieuw contract](./media/use/contract-list.png)

3. Het **nieuwe contract** deelvenster wordt weergegeven. Geef de beginwaarden van de parameters op. Selecteer **Maken**.

    ![Deelvenster voor nieuw contract](./media/use/new-contract.png)

    Het zojuist gemaakte contract wordt weergegeven in de lijst met de andere actieve contracten.

    ![Lijst met actieve contracten](./media/use/active-contracts.png)

## <a name="take-action-on-contract"></a>Actie ondernemen op een contract

Afhankelijk van de status van het contract kunnen leden acties ondernemen voor een overgang naar de volgende status van het contract. Acties zijn gedefinieerd als [overgangen](configuration.md#transitions) binnen een [status](configuration.md#states). Leden die behoren tot een toegestane toepassing of instantierol voor de overgang kunnen de actie ondernemen. 

1. In de sectie van de Blockchain Workbench-toepassing, selecteer de toepassingstegel met de opdracht om de actie te ondernemen.
2. Selecteer het contract in de lijst. Meer informatie over het contract wordt weergegeven in de verschillende secties. 

    ![Contractdetails](./media/use/contract-details.png)

    | Sectie  | Beschrijving  |
    |---------|---------|
    | Status | Een overzicht van de huidige voortgang binnen de fasen van het contract |
    | Details | De huidige waarden van het contract |
    | Bewerking | Gegevens van de laatste actie |
    | Activiteit | Transactiegeschiedenis van het contract |
    
3. In de sectie **actie**, selecteer **actie ondernemen**.

4. De gegevens van de huidige status van het contract worden in een deelvenster weergegeven. Kies de actie die u wilt uitvoeren in de vervolgkeuzelijst. 

    ![Actie kiezen](./media/use/choose-action.png)

5. Selecteer **actie ondernemen** om de actie te starten.
6. Als parameters vereist zijn voor de actie, geeft u de waarden voor de actie.

    ![Actie ondernemen](./media/use/take-action.png)

7. Selecteer **Actie ondernemen** om de actie uit te voeren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Versiebeheer van Azure Blockchain Workbench-toepassing](version-app.md)
