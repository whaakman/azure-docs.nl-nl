---
title: De queryactie in logische apps toevoegen | Microsoft Docs
description: Overzicht van de queryactie voor het uitvoeren van acties zoals matrix filteren.
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 2a82afe396039857e5b9ad6b8a6d0e710573037f
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893564"
---
# <a name="get-started-with-the-query-action"></a>Aan de slag met de queryactie
Met behulp van de queryactie, kunt u werken met batches en matrices om uit te voeren van werkstromen op:

* Een taak voor alle records met hoge prioriteit van een database maken.
* Sla alle PDF-bijlagen voor e-mailberichten in een Azure-blob.

Om te beginnen met de queryactie in een logische app, Zie [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-query-action"></a>Gebruik de queryactie
Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een logische app. 
[Meer informatie over acties](../connectors/apis-list.md).  

De queryactie heeft momenteel één bewerking, de matrix filteren, die wordt weergegeven in de ontwerpfunctie genoemd. Hiermee kunt u query's uitvoeren een matrix en een set gefilterde resultaten retourneren.

Hier ziet u hoe u deze in een logische app kunt toevoegen:

1. Selecteer de **nieuwe stap** knop.
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **filter** aan lijst met de **matrix filteren** actie.
   
    ![Selecteer de queryactie](./media/connectors-native-query/using-action-1.png)
4. Selecteer een matrix te filteren. (De volgende schermafbeelding ziet u de matrix van de resultaten van een zoekopdracht in Twitter.)
5. Een voorwaarde om te evalueren voor elk item maken. (De volgende schermafbeelding filters voor tweets van gebruikers die beschikken over meer dan 100 Volgers.)
   
    ![De queryactie niet voltooien](./media/connectors-native-query/using-action-2.png)
   
    De uitvoer van de actie wordt een nieuwe matrix die alleen de resultaten die voldoet aan de vereisten van het filter bevat.
6. Klik op de linkerbovenhoek van de werkbalk om op te slaan en de logische app wordt zowel opslaan en publiceren (activeren).

\* Als u bent een HTTP-eindpunt aanroepen en een JSON-antwoord ontvangen, gebruikt u de _JSON parseren_ actie parseren van het JSON-antwoord. Zonder deze stap _matrix filteren_ wordt alleen hoofdtekst zien en de structuur van de JSON-nettolading wordt niet begrepen.

## <a name="query-action"></a>Queryactie.
Hier volgen de details voor de actie die ondersteuning biedt voor deze connector. De connector heeft een mogelijke actie.

| Bewerking | Description |
| --- | --- |
| Matrix filteren |Evalueert een voorwaarde voor elk item in een matrix en retourneert de resultaten |

## <a name="action-details"></a>Actiedetails
De queryactie wordt geleverd met een mogelijke actie. De volgende tabellen beschrijven de vereiste en optionele velden voor de actie en de bijbehorende uitvoergegevens die zijn gekoppeld met de actie.

### <a name="filter-array"></a>Matrix filteren
Hier volgen de invoervelden voor de actie, waardoor een uitgaande HTTP-aanvraag.
Een * betekent dat het een verplicht veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Van * |uit |De matrix om te filteren |
| Voorwaarde * |waarbij |De voorwaarde om te evalueren voor elk item |

<br>

### <a name="output-details"></a>Uitvoergegevens
Hier volgen de uitvoerdetails van het HTTP-antwoord.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Gefilterde matrix |array |Een matrix met een object voor elk gefilterde resultaat |

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). U kunt de beschikbare connectors in logic apps verkennen door te kijken onze [lijst van API's](apis-list.md).

