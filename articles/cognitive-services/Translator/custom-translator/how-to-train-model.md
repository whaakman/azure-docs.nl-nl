---
title: Een model - aangepaste Translator trainen
titleSuffix: Azure Cognitive Services
description: Een model training is een belangrijke stap bij het bouwen van een NAT-model. Training gebeurt op basis van documenten die u voor die u kunt selecteren.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 60e0485c28d90050a6ff775db41f8696a09fe033
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165755"
---
# <a name="train-a-model"></a>Een model trainen

Een model trainen namelijk de belangrijkste stap voor het bouwen van een NAT-model, zonder een training model kan niet worden gemaakt. Training gebeurt op basis van documenten die u hebt geselecteerd voor de trainingen.

Met het trainen van een model:

1.  Selecteer het project waarin u wilt een model bouwen.

2.  Het tabblad gegevens voor het project wordt de relevante documenten voor het paar van de taal project weergegeven. Selecteer handmatig de documenten die u wilt gebruiken voor het trainen van uw model. U kunt de training, afstemmen en testen van documenten in dit scherm selecteren. Ook u zojuist hebt trainingset selecteren en aangepaste Translator maken het afstemmen en sets testen voor u hebben.

    -  Naam van het document: Naam van het document.

    -  Koppelen: Als dit document een parallelle of ééntalig-document is. Ééntalig documenten worden momenteel niet ondersteund voor de training.

    -  Document van het type: Kan worden training, afstemmen, testen of woordenlijst.

    -  Combinatie van taal: Dit ziet u de bron en doel taal voor het project.

    -  Bron zinnen: Geeft het aantal zinnen geëxtraheerd uit het bronbestand.

    -  Doel zinnen: Geeft het aantal zinnen geëxtraheerd uit het doelbestand.

    ![Train model](media/how-to/how-to-train-model.png)

3.  Klik op knop van de trein.

4.  Geef een naam voor uw model in het dialoogvenster.

5.  Klik op Train-model.

    ![Dialoogvenster voor Train-model](media/how-to/how-to-train-model-2.png)

6.  Aangepaste Translator indienen van de training en de status van de training op het tabblad modellen weergeven.

    ![Pagina met het model trainen](media/how-to/how-to-train-model-3.png)


## <a name="edit-a-model"></a>Een model bewerken

U kunt een model met behulp van de link bewerken op de pagina met Details van Model bewerken.

1.  Klik op het potloodpictogram.

    ![Model bewerken](media/how-to/how-to-edit-model.png)

2.  In het dialoogvenster-wijzigen

    1.  Modelnaam (vereist): Uw model een betekenisvolle naam geven.

        ![Meer dialoogvenster bewerken](media/how-to/how-to-edit-model-dialog.png)

3.  Klik op Opslaan.


## <a name="next-steps"></a>Volgende stappen

- Informatie over [weergeven van details van model](how-to-view-model-details.md).
