---
title: Functies toevoegen in toepassingen LUIS | Microsoft Docs
description: Language Understanding (LUIS) gebruiken om toe te voegen van app-functies ter verbetering van de detectie of de voorspelling van intenties en entiteiten die categorieën en -patronen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/30/2018
ms.author: diberry
ms.openlocfilehash: 5ec75436c7df5c08f5507794229bec1f9adb2804
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222950"
---
# <a name="use-features-to-improve-your-luis-apps-performance"></a>Functies gebruiken om uw LUIS-app-prestaties te verbeteren  

U kunt functies toevoegen aan uw LUIS-app voor het verbeteren van de nauwkeurigheid. Functies LUIS u helpen met hints die bepaalde woorden en zinnen maken deel uit van een categorie. Als LUIS leert hoe u voor het herkennen van een lid van de categorie, kan deze de andere op dezelfde manier behandelen.

## <a name="add-phrase-list"></a>Woordgroepenlijst toevoegen

1. Open uw app door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **bouwen**, klikt u vervolgens op **lijsten woordgroep** in het linkerdeelvenster van uw app. 

    ![Woordgroep lijst navigatie](./media/luis-add-features/phrase-list-nav.png)

2. Op de **lijsten woordgroep** pagina, klikt u op **maken nieuwe woordgroepenlijst**. 
 
    ![Nieuwe woordgroepenlijst maken](./media/luis-add-features/create-new-phrase-list.png)
    
3. In de **woordgroepenlijst toevoegen** dialoogvenster typt u "Steden" Als de naam van de woordgroepenlijst met. In de **waarde** typt u de waarden van de woordgroepenlijst met. Typ een waarde op een tijdstip of een set met door komma's gescheiden waarden en druk vervolgens op **Enter**.

    ![Woordgroep lijst steden toevoegen](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS kunt gerelateerde waarden toe te voegen aan uw woordgroepenlijst met voorstellen. Klik op **raden** om op te halen van een groep van de voorgestelde waarden die zijn semantisch met betrekking tot de added value(s). U kunt op elk van de voorgestelde waarden of op **alle toevoegen** om toe te voegen ze alle.

    ![Woordgroepenlijst met voorgestelde waarden](./media/luis-add-features/related-values.png)

5. Klik op **deze waarden zijn verwisselbaar** als de waarden van de lijst met toegevoegde woordgroep alternatieven die door elkaar kunnen worden gebruikt.

    ![Woordgroepenlijst met voorgestelde waarden](./media/luis-add-features/interchangeable.png)

6. Klik op **Opslaan**. De woordgroep "Steden" lijst wordt toegevoegd aan de **lijsten woordgroep** pagina.

    ![Woordgroepenlijst toegevoegd](./media/luis-add-features/phrase-list-cities.png)

## <a name="edit-phrase-list"></a>Woordgroepenlijst met bewerken

Klik op de naam van de woordgroepenlijst met in de **lijsten woordgroep** pagina. In de **woordgroepenlijst bewerken** in het dialoogvenster wordt geopend, breng een of meer vereiste wijzigingen en klik vervolgens op bewerken **opslaan**.

 ![Woordgroepenlijst toegevoegd](./media/luis-add-features/edit-phrase-list.png)

## <a name="delete-phrase-list"></a>Woordgroepenlijst verwijderen 

Klik op het weglatingsteken (***...*** ) aan het einde van de rij en selecteer **verwijderen**.

 ![Verwijderen van de lijst toegevoegd](./media/luis-add-features/delete-phrase-list.png)

## <a name="deactivate-phrase-list"></a>Woordgroepenlijst met deactiveren 

Klik op het weglatingsteken (***...*** ) aan het einde van de rij en selecteer **deactiveren**.

 ![Deactiveren van de lijst toegevoegd](./media/luis-add-features/deactivate-phrase-list.png)

## <a name="pattern-regular-expression-feature"></a>De patroonfunctie (reguliere expressie) 
**Deze functie is afgeschaft**. Nieuwe functies van het patroon kunnen niet worden toegevoegd aan LUIS. Eventuele bestaande patroon-functies worden ondersteund tot mei 2018. Bijdragen aan de standaard LUIS reguliere expressie die overeenkomt met een pull-aanvraag naar de [kenmerken tekst Github-opslagplaats](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Volgende stappen

Na toevoegen, bewerken, verwijderen of een woordgroepenlijst deactiveren [trainen en testen van de app](luis-interactive-test.md) opnieuw om te zien als verbetert de prestaties.
