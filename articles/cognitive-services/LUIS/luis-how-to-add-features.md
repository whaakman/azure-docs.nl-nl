---
title: Woordgroep bevat ter verbetering van de detectie van entiteit
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) gebruiken om toe te voegen van app-functies ter verbetering van de detectie of de voorspelling van intenties en entiteiten die categorieën en -patronen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 32ef8ba2f6416e1b59fc98595f1b204e94bd2ead
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830987"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Gebruik woordgroep lijsten moeten worden boost signaal van de lijst met woorden

U kunt functies toevoegen aan uw LUIS-app voor het verbeteren van de nauwkeurigheid. Functies LUIS u helpen met hints die bepaalde woorden en woordgroepen maken deel uit van een app-domein vocabulaire. 

## <a name="add-phrase-list"></a>Woordgroepenlijst toevoegen

1. Open uw app door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **bouwen**, klikt u vervolgens op **lijsten woordgroep** in het linkerdeelvenster van uw app. 

2. Op de **lijsten woordgroep** pagina, klikt u op **maken nieuwe woordgroepenlijst**. 
 
3. In de **woordgroepenlijst toevoegen** dialoogvenster typt u "Steden" Als de naam van de woordgroepenlijst met. In de **waarde** typt u de waarden van de woordgroepenlijst met. Typ een waarde op een tijdstip of een set met door komma's gescheiden waarden en druk vervolgens op **Enter**.

    ![Woordgroep lijst steden toevoegen](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS kunt gerelateerde waarden toe te voegen aan uw woordgroepenlijst met voorstellen. Klik op **raden** om op te halen van een groep van de voorgestelde waarden die zijn semantisch met betrekking tot de added value(s). U kunt op elk van de voorgestelde waarden of op **alle toevoegen** om toe te voegen ze alle.

    ![Woordgroepenlijst met voorgestelde waarden](./media/luis-add-features/related-values.png)

5. Klik op **deze waarden zijn verwisselbaar** als de waarden van de lijst met toegevoegde woordgroep alternatieven die door elkaar kunnen worden gebruikt.

    ![Woordgroepenlijst met voorgestelde waarden](./media/luis-add-features/interchangeable.png)

6. Klik op **Opslaan**. De woordgroep "Steden" lijst wordt toegevoegd aan de **lijsten woordgroep** pagina.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> U kunt verwijderen of een woordgroepenlijst van de contextuele werkbalk deactiveren op de **lijsten woordgroep** pagina.

## <a name="pattern-regular-expression-feature"></a>De patroonfunctie (reguliere expressie) 
**Deze functie is afgeschaft**. Nieuwe functies van het patroon kunnen niet worden toegevoegd aan LUIS. Eventuele bestaande patroon-functies worden ondersteund tot mei 2018. Bijdragen aan de standaard LUIS reguliere expressie die overeenkomt met een pull-aanvraag naar de [kenmerken tekst Github-opslagplaats](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Volgende stappen

Na toevoegen, bewerken, verwijderen of een woordgroepenlijst deactiveren [trainen en testen van de app](luis-interactive-test.md) opnieuw om te zien als verbetert de prestaties.
