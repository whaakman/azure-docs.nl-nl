---
title: Vertrouwen Score - cognitieve Services van Microsoft | Microsoft Docs
titleSuffix: Azure
description: Uitleg over vertrouwen score
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: c97bdb7e57275ebd1893bc28248c4ecc6b35c153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345300"
---
# <a name="confidence-score"></a>Betrouwbaarheidsscore

Een score vertrouwen geeft aan dat de mate van de overeenkomst is tussen de gebruiker vraag en het antwoord geretourneerd.

Wanneer de aanvraag voor een gebruiker wordt vergeleken met knowledge base-inhoud, kunnen er meer dan een antwoord geretourneerd. De antwoorden worden geretourneerd in een gerangschikte volgorde voor afname vertrouwen score.

Er is een vertrouwen score tussen 0 en 100.

|Score-waarde|Betrouwbaarheid|
|--|--|
|100|Een exacte overeenkomst met gebruiker query's en een vraag KB|
|90|Hoge betrouwbaarheid - de meeste van de woorden in die overeenkomen met|
|40-60|Evenredige vertrouwen - belangrijke woorden overeen met|
|10|Lage vertrouwen - belangrijke woorden komen niet overeen|
|0|Geen overeenkomst word|


## <a name="similar-confidence-scores"></a>Vergelijkbare vertrouwen scores
Wanneer u meerdere antwoorden op een vergelijkbare vertrouwen score hebben, is het waarschijnlijk dat de query te algemeen en daarom overeenkomende kans met meerdere antwoorden is. Probeer structuur van uw QnAs beter zodat elke entiteit QnA een afzonderlijke opzet heeft.


## <a name="improving-confidence-scores"></a>Vertrouwen scores verbeteren
Ter verbetering van de score betrouwbaarheidsinterval van een bepaalde respons op een gebruikersquery, kunt u de gebruikersquery naar de knowledge base toevoegen als een alternatieve vraag op reactie.
   
## <a name="confidence-score-differences"></a>Vertrouwen score verschillen
De score betrouwbaarheidsinterval van een antwoord overgaan negligibly tussen de test- en de gepubliceerde versie van de knowledge base zelfs als de inhoud hetzelfde is. Dit komt doordat de inhoud van de test en de gepubliceerde knowledge base bevinden zich in verschillende Azure Search-index.

Hier ziet hoe de [publiceren](../How-To/publish-knowledge-base.md) bewerking werkt.


## <a name="no-match-found"></a>Geen overeenkomst gevonden
Geen goede overeenkomst wordt gevonden door de ranker, de score vertrouwen van 0,0 of 'None' geretourneerd als het antwoord van de standaardwaarde is 'Geen goede overeenkomst gevonden in de KB'. U kunt dit standaardantwoord in de bot of toepassing code aanroepen van het eindpunt overschrijven. Ook kunt u ook het antwoord onderdrukking instellen in Azure en Hiermee wijzigt u de standaardwaarde voor alle kennis basissen geïmplementeerd in een bepaalde QnA Maker-service.

1. Ga naar de [Azure-portal](http://portal.azure.com) en navigeer naar de resourcegroep die de QnA Maker-service die u hebt gemaakt.

2. Klik hier om de **App Service**.

    ![Toegang tot App service](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Klik op **toepassingsinstellingen** en bewerkt u de **DefaultAnswer** veld aan het gewenste Standaardantwoord. Klik op **Opslaan**.

    ![Standaardantwoord wijzigen](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Uw App-service opnieuw starten

    ![QnA Maker appservice opnieuw opstarten](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gegevensbronnen die worden ondersteund](./data-sources-supported.md)

## <a name="see-also"></a>Zie ook 

[QnA Maker-overzicht](../Overview/overview.md)
