---
title: Vooraf gemaakte entiteiten voor Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS bevat een set met vooraf gemaakte entiteiten voor het herkennen van algemene typen gegevens, zoals datums, tijden, getallen, metingen en valuta. Ondersteuning voor vooraf gedefinieerde entiteit is afhankelijk van de cultuur van uw LUIS-app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/05/2018
ms.author: diberry
ms.openlocfilehash: 8371e99224b511a87b2061708f00822a70c024b0
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999072"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Vooraf gemaakte entiteiten voor het herkennen van veelvoorkomende gegevenstypen

LUIS bevat een set met vooraf gemaakte entiteiten voor het herkennen van algemene typen gegevens, zoals datums, tijden, getallen, metingen en valuta. 

## <a name="add-a-prebuilt-entity"></a>Een vooraf gedefinieerde entiteit toevoegen

1. Open uw app door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **entiteiten** in aan de linkerkant. 

1. Op de **entiteiten** pagina, klikt u op **vooraf gedefinieerde entiteit toevoegen**.

1. In **vooraf gemaakte entiteiten toevoegen** dialoogvenster Selecteer de datetimeV2 vooraf gedefinieerde entiteit. 

    ![In het dialoogvenster van vooraf gedefinieerde entiteit toevoegen](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Selecteer **Done**.

## <a name="publish-the-app"></a>Publiceer de app

De eenvoudigste manier om de waarde van een vooraf gedefinieerde entiteit weer te geven is voor query van het gepubliceerde-eindpunt. 

1. Selecteer in de bovenste werkbalk **publiceren**. Publiceren naar **productie**. 

1. Wanneer de melding groen geslaagd wordt weergegeven, selecteert u de **verwijzen naar de lijst met eindpunten** koppeling naar de eindpunten.

1. Selecteer een eindpunt. Er wordt een nieuw browsertabblad geopend naar dit eindpunt. Houd het browsertabblad geopend en Ga door met de **Test** sectie.

## <a name="test"></a>Testen
Nadat de entiteit is toegevoegd, hoeft u niet met het trainen van de app. 

Test de nieuwe doel op het eindpunt toegevoegd door een waarde voor de **q** parameter. Gebruik de volgende tabel voor voorgestelde uitingen voor **q**:

|Test utterance|Entiteitswaarde|
|--|:--|
|Boek een vlucht morgen|19-10-2018|
|de afspraak op 3 maart annuleren|LUIS de meest recente 3 maart in het verleden geretourneerd (2018-03-03) en 3 maart in de toekomst (03-03-2019) omdat de utterance hebt opgegeven een jaar.|
|Een vergadering om 10 uur plannen|10:00:00|

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Markeren van entiteiten met een vooraf gedefinieerde entiteit-token
 Als u tekst, zoals `HH-1234`, die u wilt markeren als een aangepaste entiteit _en_ hebt [vooraf gedefinieerde nummer](luis-reference-prebuilt-number.md) toegevoegd aan het model, kunt u zich niet aan de aangepaste entiteit in de portal LUIS markeren. U kunt markeren met de API. 

 Als u wilt markeren van dit type token, waar dat onderdeel van deze al is gemarkeerd met een vooraf gedefinieerde entiteit, de vooraf gedefinieerde entiteit uit de LUIS-app te verwijderen. U hoeft niet te trainen van de app. Markeert vervolgens het token aan uw eigen aangepaste entiteit. Voeg de vooraf gedefinieerde entiteit terug naar de LUIS-app.

 Voor een ander voorbeeld kunt u overwegen de utterance als een lijst van de klasse voorkeuren: `I want first year spanish, second year calculus, and fourth year english lit.` als de LUIS-app het rangtelwoord voor de Prebuild toegevoegd heeft, `first`, `second`, en `fourth` al gemarkeerd met rangtelwoorden. Als u wilt dat het rangtelwoord voor de en de klasse vast te leggen, kunt u een samengestelde entiteit maken en deze teruglopen van de vooraf gedefinieerde volgnummer en de aangepaste entiteit voor de klassenaam van de.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Verwijzing naar bestaande entiteit](./luis-reference-prebuilt-entities.md)
