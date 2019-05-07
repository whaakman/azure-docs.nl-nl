---
title: Een nieuwe app maken
titleSuffix: Language Understanding - Azure Cognitive Services
description: Maken en beheren van uw toepassingen op de webpagina van de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 9d650a17ddfac6461341e50c4693e4522d9628b3
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148200"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Een nieuwe LUIS-app maken in de portal LUIS
Er zijn een aantal manieren om een LUIS-app te maken. U kunt een LUIS-app in de [LUIS](https://www.luis.ai) portal of via het ontwerpen van LUIS [API's](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

## <a name="using-the-luis-portal"></a>Met behulp van de portal LUIS
U kunt een nieuwe app maken in de portal LUIS op verschillende manieren:

* Beginnen met een lege app en intenties, uitingen en entiteiten te maken.
* Beginnen met een lege app en voeg een [vooraf gemaakte domein](luis-how-to-use-prebuilt-domains.md).
* Importeer een LUIS-app vanuit een JSON-bestand dat al intents, uitingen en entiteiten bevat.

## <a name="using-the-authoring-apis"></a>Met behulp van de API's voor ontwerpen
U kunt een nieuwe app maken met de authoring API's in een aantal manieren:

* [Start](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) met een lege app en intenties, uitingen en entiteiten te maken.
* [Start](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) met een vooraf gedefinieerde domein.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Nieuwe app maken in LUIS

1. Op **mijn Apps** weergeeft, schakelt **nieuwe app maken**.

    ![Lijst met apps van LUIS](./media/luis-create-new-app/apps-list.png)


2. Geef uw toepassing 'TravelAgent' in het dialoogvenster.

    ![Dialoogvenster voor een nieuwe app maken](./media/luis-create-new-app/create-app.png)

3. Kies de cultuur van uw toepassing (voor TravelAgent app, kiest u Engels), en selecteer vervolgens **gedaan**. 

    > [!NOTE]
    > De cultuur kan niet worden gewijzigd nadat de toepassing is gemaakt. 

## <a name="import-an-app-from-file"></a>Een app importeren vanuit bestand

1. Op **mijn Apps** weergeeft, schakelt **importeren nieuwe app**.
1. Selecteer een geldige app JSON-bestand en selecteer vervolgens in het pop-updialoogvenster **gedaan**.

### <a name="import-errors"></a>Fouten bij het importeren

Mogelijke fouten zijn: 

* Er bestaat al een app met die naam. Importeer de app en stel de **optionele naam** naar een nieuwe naam. 

## <a name="export-app-for-backup"></a>Uitvoer-app voor back-up

1. Op **mijn Apps** weergeeft, schakelt **exporteren**.
1. Selecteer **exporteren als JSON**. De browser de actieve versie van de app is gedownload.
1. Dit bestand toevoegen aan uw back-upsysteem voor het archiveren van het model.

## <a name="export-app-for-containers"></a>Uitvoer-app voor containers

1. Op **mijn Apps** weergeeft, schakelt **exporteren**.
1. Selecteer **exporteren als container** Selecteer welke gepubliceerde sleuf (productie- of fase) die u wilt exporteren.
1. Gebruik dit bestand met uw [LUIS container](luis-container-howto.md). 

    Als u geïnteresseerd in een getraind, maar niet exporteren nog gepubliceerde model bent te gebruiken met de container LUIS, gaat u naar de **versies** pagina en van daaruit exporteren. 

## <a name="delete-app"></a>App verwijderen

1. Op **mijn Apps** pagina, selecteert u de drie puntjes (...) aan het einde van de rij met Apps.
1. Selecteer **verwijderen** in het menu.
1. Selecteer **Ok** in het bevestigingsvenster.

## <a name="next-steps"></a>Volgende stappen

Uw eerste taak in de app bestaat uit het [intents toevoegen](luis-how-to-add-intents.md).
