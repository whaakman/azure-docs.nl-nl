---
title: Een nieuwe app maken met LUIS | Microsoft Docs
description: Maken en beheren van uw toepassingen op de webpagina Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 75edd39346995cdef72bb1e1fcb9eaff53d29702
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345865"
---
# <a name="create-an-app"></a>Een app maken
U maakt een nieuwe app op verschillende manieren: 

* [Start](#create-new-app) met een lege app en intents, utterances en entiteiten maken.
* [Start](#create-new-app) met een lege app en voeg een [vooraf gemaakte domein](luis-how-to-use-prebuilt-domains.md).
* [Importeer een app LUIS](#import-new-app) vanuit een JSON-bestand dat al intents, utterances en entiteiten bevat.

## <a name="what-is-an-app"></a>Wat is een app
De app bevat [versies](luis-how-to-manage-versions.md) van uw model evenals alle [deelnemers](luis-how-to-collaborate.md) voor de app. Als u de app maakt, selecteert u de cultuur ([taal](luis-supported-languages.md)) die **later kan niet worden gewijzigd**. 

De standaardversie van een nieuwe app is "0,1." 

U kunt maken en beheren van uw toepassingen op **mijn Apps** pagina. U kunt altijd toegang tot deze pagina als u selecteert **mijn apps** op de bovenste navigatiebalk van de [LUIS](luis-reference-regions.md) website. 

[![](media/luis-create-new-app/apps-list.png "Schermafbeelding van de lijst met apps")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Nieuwe app maken

1. Op **mijn Apps** pagina **nieuwe app maken**.
2. Naam in het dialoogvenster uw toepassing 'TravelAgent'.

    ![Dialoogvenster Nieuwe app maken](./media/luis-create-new-app/create-app.png)

3. Kies de cultuur van uw toepassing (Kies Engels voor TravelAgent-app), en selecteer vervolgens **gedaan**. 

    >[!NOTE]
    >De cultuur kan niet worden gewijzigd wanneer de toepassing is gemaakt. 

## <a name="import-new-app"></a>De nieuwe app importeren
U kunt de naam (maximaal 50 tekens), de versie (maximaal 10 char) en de beschrijving van een app instellen in het JSON-bestand. Voorbeelden van JSON-bestanden van toepassing zijn beschikbaar op [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. Op **mijn Apps** pagina **importeren nieuwe app**.
2. In de **importeren nieuwe app** dialoogvenster, selecteert u het definiëren van de app LUIS JSON-bestand.

    ![Een nieuwe app dialoogvenster importeren](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>App exporteren
1. Op **mijn Apps** pagina, selecteert u de drie puntjes (...) aan het einde van de app-rij.

    [![](media/luis-create-new-app/apps-list.png "Schermafbeelding van de pop-upvenster van acties per app")](media/luis-create-new-app/three-dots.png#lightbox)

2. Selecteer **Export app** in het menu. 

## <a name="rename-app"></a>Wijzig de naam van app

1. Op **mijn Apps** pagina, selecteert u de drie puntjes (...) aan het einde van de app-rij. 
2. Selecteer **naam** in het menu.
3. Geef de nieuwe naam van de app en selecteer **gedaan**.

## <a name="delete-app"></a>App verwijderen

> [!CAUTION]
> U kunt de app voor alle deelnemers en de eigenaar wilt verwijderen. [Exporteren](#export-app) de app voordat u het verwijdert. 

1. Op **mijn Apps** pagina, selecteert u de drie puntjes (...) aan het einde van de app-rij. 
2. Selecteer **verwijderen** in het menu.
3. Selecteer **Ok** in het bevestigingsvenster.

## <a name="export-endpoint-logs"></a>Exporteren van endpoint-Logboeken
De logboeken bevatten de Query UTC-tijd en LUIS JSON-antwoord.

1. Op **mijn Apps** pagina, selecteert u de drie puntjes (...) aan het einde van de app-rij. 
2. Selecteer **exporteren eindpunt logboeken** in het menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Volgende stappen

Is uw eerste taak in de app te [intents toevoegen](luis-how-to-add-intents.md).