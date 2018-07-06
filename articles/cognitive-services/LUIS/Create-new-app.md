---
title: Een nieuwe app maken met LUIS | Microsoft Docs
description: Maken en beheren van uw toepassingen op de webpagina van de Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 998a85720f5707fbf6ed4c5cfa3ed0dab5d1cc0e
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865042"
---
# <a name="create-an-app"></a>Een app maken
U kunt een nieuwe app maken op verschillende manieren: 

* [Start](#create-new-app) met een lege app en intenties, uitingen en entiteiten te maken.
* [Start](#create-new-app) met een lege app en voeg een [vooraf gemaakte domein](luis-how-to-use-prebuilt-domains.md).
* [Een LUIS-app importeren](#import-new-app) vanuit een JSON-bestand dat al intents, uitingen en entiteiten bevat.

## <a name="what-is-an-app"></a>Wat is een app
De app bevat [versies](luis-how-to-manage-versions.md) van uw model, evenals een [samenwerkers](luis-how-to-collaborate.md) voor de app. Als u de app maakt, selecteert u de cultuur ([taal](luis-supported-languages.md)) die **kan later worden gewijzigd**. 

De standaardversie van een nieuwe app is "0.1." 

U kunt maken en beheren van uw toepassingen op **mijn Apps** pagina. U kunt altijd toegang tot deze pagina door te selecteren **mijn apps** op de bovenste navigatiebalk van de [LUIS](luis-reference-regions.md) website. 

[![](media/luis-create-new-app/apps-list.png "Schermafbeelding van de lijst met apps")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Nieuwe app maken

1. Op **mijn Apps** weergeeft, schakelt **nieuwe app maken**.
2. Geef uw toepassing 'TravelAgent' in het dialoogvenster.

    ![Dialoogvenster voor een nieuwe app maken](./media/luis-create-new-app/create-app.png)

3. Kies de cultuur van uw toepassing (voor TravelAgent app, kiest u Engels), en selecteer vervolgens **gedaan**. 

    >[!NOTE]
    >De cultuur kan niet worden gewijzigd nadat de toepassing wordt gemaakt. 

## <a name="import-new-app"></a>Nieuwe app importeren
U kunt de naam (maximaal 50 tekens), de versie (maximaal 10 tekens) en de beschrijving van een app instellen in de JSON-bestand. Voorbeelden van JSON-bestanden van toepassing zijn beschikbaar op [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. Op **mijn Apps** weergeeft, schakelt **importeren nieuwe app**.
2. In de **importeren nieuwe app** dialoogvenster, selecteer het JSON-bestand definiëren van de LUIS-app.

    ![Het dialoogvenster voor een nieuwe app importeren](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>App exporteren
1. Op **mijn Apps** pagina, selecteer het weglatingsteken (***...*** ) aan het einde van de rij met Apps.

    [![](media/luis-create-new-app/apps-list.png "Schermopname van het pop-upvenster van acties per app")](media/luis-create-new-app/three-dots.png#lightbox)

2. Selecteer **Export app** in het menu. 

## <a name="rename-app"></a>Wijzig de naam van app

1. Op **mijn Apps** pagina, selecteer het weglatingsteken (***...*** ) aan het einde van de rij met Apps. 
2. Selecteer **naam** in het menu.
3. Geef de nieuwe naam van de app en selecteer **gedaan**.

## <a name="delete-app"></a>App verwijderen

> [!CAUTION]
> U kunt de app voor alle deelnemers en de eigenaar wilt verwijderen. [Exporteren](#export-app) de app voordat deze worden verwijderd. 

1. Op **mijn Apps** pagina, selecteer het weglatingsteken (***...*** ) aan het einde van de rij met Apps. 
2. Selecteer **verwijderen** in het menu.
3. Selecteer **Ok** in het bevestigingsvenster.

## <a name="export-endpoint-logs"></a>Eindpunt-logboeken exporteren
De logboeken bevatten de Query UTC-tijd en LUIS JSON-antwoord.

1. Op **mijn Apps** pagina, selecteer het weglatingsteken (***...*** ) aan het einde van de rij met Apps. 
2. Selecteer **eindpunt-logboeken exporteren** in het menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Volgende stappen

Uw eerste taak in de app bestaat uit het [intents toevoegen](luis-how-to-add-intents.md).