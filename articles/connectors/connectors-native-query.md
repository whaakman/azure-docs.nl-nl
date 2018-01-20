---
title: De queryactie in logic apps toevoegen | Microsoft Docs
description: Overzicht van de queryactie voor het uitvoeren van acties zoals matrix van filter.
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 05dd4ae3c4ee439d66401a3f5595f9104051f8ee
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-query-action"></a>Aan de slag met de queryactie
Met behulp van de queryactie, kunt u met de batches en matrices voor het uitvoeren van werkstromen te werken:

* Een taak maken voor alle hoge prioriteit records uit een database.
* Sla alle PDF-bijlagen voor e-mailberichten naar een Azure-blob.

Om te beginnen met de queryactie in een logische app, Zie [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-query-action"></a>Gebruik de queryactie
Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](connectors-overview.md).  

De actie van de query heeft momenteel een bewerking, die de matrix van filter, die wordt weergegeven in de ontwerpfunctie wordt genoemd. Hiermee kunt u een matrix doorzoeken en een set gefilterde resultaten geretourneerd.

Hier ziet u hoe u deze in een logische app kunt toevoegen:

1. Selecteer de **nieuwe stap** knop.
2. Kies **een actie toevoegen**.
3. Typ in het zoekvak actie **filter** aan lijst met de **matrix van Filter** in te grijpen.
   
    ![Selecteer de queryactie](./media/connectors-native-query/using-action-1.png)
4. Selecteer een matrix om te filteren. (De volgende schermafbeelding ziet u de matrix van resultaten van een zoekopdracht Twitter.)
5. Een voorwaarde om te evalueren voor elk item maken. (In de volgende schermafbeelding filters tweets van gebruikers die beschikken over meer dan 100 PC-gebruikers.)
   
    ![De queryactie niet voltooien](./media/connectors-native-query/using-action-2.png)
   
    De uitvoer van de actie wordt een nieuwe matrix die alleen de resultaten die voldoet aan de vereisten van het filter bevat.
6. Klik op de linkerbovenhoek van de werkbalk om op te slaan en uw logische app wordt opslaan en publiceren (activeren).

\*Als u het aanroepen van een HTTP-eindpunt, en een JSON-antwoord ontvangen, gebruikt u de _parseren JSON_ actie parseren van het JSON-antwoord. Zonder deze stap _matrix van Filter_ wordt alleen tekst weer en geen inzicht in de structuur van de JSON-nettolading.

## <a name="query-action"></a>Queryactie
Hier volgen de details voor de actie die ondersteuning biedt voor deze connector. De connector is een mogelijke actie.

| Bewerking | Beschrijving |
| --- | --- |
| Matrix filteren |Evalueert een voorwaarde voor elk item in een matrix en de resultaten geretourneerd |

## <a name="action-details"></a>Actiedetails
De queryactie wordt geleverd met een mogelijke actie. De volgende tabellen beschrijven de vereiste en optionele invoervelden voor de actie en de bijbehorende uitvoerdetails die zijn gekoppeld met de actie.

### <a name="filter-array"></a>Matrix filteren
Hier volgen de invoervelden voor de actie, waardoor een uitgaande HTTP-aanvraag.
A * houdt in dat een vereist veld.

| Weergavenaam | Naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Van * |uit |De matrix om te filteren |
| Voorwaarde * |waar |De voorwaarde om te evalueren voor elk item |

<br>

### <a name="output-details"></a>Uitvoergegevens
Hier volgen de uitvoerdetails voor het HTTP-antwoord.

| Naam van eigenschap | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Gefilterde matrix |matrix |Een matrix met een object voor elk gefilterde resultaat |

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). U kunt de beschikbare connectors in logische apps door te kijken verkennen onze [API's lijst](apis-list.md).

