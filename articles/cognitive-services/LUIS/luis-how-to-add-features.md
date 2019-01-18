---
title: Woordgroepenlijsten
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding (LUIS) gebruiken om toe te voegen van app-functies ter verbetering van de detectie of de voorspelling van intenties en entiteiten die categorieën en -patronen
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: 4f2343e1c6b95c495f03cd94ee58164c546f4b76
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382191"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Gebruik woordgroep lijsten moeten worden boost signaal van de lijst met woorden

U kunt functies toevoegen aan uw LUIS-app voor het verbeteren van de nauwkeurigheid. Functies LUIS u helpen met hints die bepaalde woorden en woordgroepen maken deel uit van een app-domein vocabulaire. 

Een [woordgroepenlijst](luis-concept-feature.md) bevat een aantal waarden (woorden of zinsdelen) die deel uitmaken van dezelfde klasse en op dezelfde manier (bijvoorbeeld de namen van steden of producten) moet worden behandeld. LUIS leert over een van beide wordt automatisch toegepast op de andere. Deze lijst is niet een lijst met gesloten entiteit (exact overeenkomende tekst overeenkomt met) van de overeenkomende woorden.

Een woordgroepenlijst wordt toegevoegd aan het vocabulaire van het domein van de app als een tweede signaal dat moet worden LUIS over deze woorden.

## <a name="add-phrase-list"></a>Woordgroepenlijst toevoegen

LUIS kunt u maximaal 10 woordgroep lijsten per app. 

1. Open uw app door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **bouwen**, klikt u vervolgens op **lijsten woordgroep** in het linkerdeelvenster van uw app. 

2. Op de **lijsten woordgroep** pagina, klikt u op **maken nieuwe woordgroepenlijst**. 
 
3. In de **woordgroepenlijst toevoegen** dialoogvenster typt u "Steden" Als de naam van de woordgroepenlijst met. In de **waarde** typt u de waarden van de woordgroepenlijst met. Typ een waarde op een tijdstip of een set met door komma's gescheiden waarden en druk vervolgens op **Enter**.

    ![Woordgroep lijst steden toevoegen](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS kunt gerelateerde waarden toe te voegen aan uw woordgroepenlijst met voorstellen. Klik op **raden** om op te halen van een groep van de voorgestelde waarden die zijn semantisch met betrekking tot de added value(s). U kunt op elk van de voorgestelde waarden of op **alle toevoegen** om toe te voegen ze alle.

    ![Lijst met voorgestelde waarden woordgroep - Alles toevoegen](./media/luis-add-features/related-values.png)

5. Klik op **deze waarden zijn verwisselbaar** als de waarden van de lijst met toegevoegde woordgroep alternatieven die door elkaar kunnen worden gebruikt.

    ![Lijst met voorgestelde waarden woordgroep - uitwisselbaar box selecteren ](./media/luis-add-features/interchangeable.png)

6. Klik op **Opslaan**. De woordgroep "Steden" lijst wordt toegevoegd aan de **lijsten woordgroep** pagina.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> U kunt verwijderen of een woordgroepenlijst van de contextuele werkbalk deactiveren op de **lijsten woordgroep** pagina.

## <a name="next-steps"></a>Volgende stappen

Na toevoegen, bewerken, verwijderen of een woordgroepenlijst deactiveren [trainen en testen van de app](luis-interactive-test.md) opnieuw om te zien als verbetert de prestaties.
