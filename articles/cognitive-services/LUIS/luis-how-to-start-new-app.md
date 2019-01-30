---
title: Een nieuwe app maken
titleSuffix: Language Understanding - Azure Cognitive Services
description: Maken en beheren van uw toepassingen op de webpagina van de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 3c35bb96c3ba5dbf1c3302836b2c73cf15128937
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215169"
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


## <a name="next-steps"></a>Volgende stappen

Uw eerste taak in de app bestaat uit het [intents toevoegen](luis-how-to-add-intents.md).
