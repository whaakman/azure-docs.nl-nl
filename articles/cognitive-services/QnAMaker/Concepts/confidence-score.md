---
title: Betrouwbaarheidsscore - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een betrouwbaarheidsscore geeft aan dat de mate van overeenkomst tussen de vraag van de gebruiker en het antwoord geretourneerd.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 33da5cf5724b8314ce813f12eb077d9a15ec1b2a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041520"
---
# <a name="confidence-score"></a>Betrouwbaarheidsscore

Een betrouwbaarheidsscore geeft aan dat de mate van overeenkomst tussen de vraag van de gebruiker en het antwoord geretourneerd.

Wanneer de aanvraag voor een gebruiker wordt vergeleken met knowledge base-inhoud, kunnen er meer dan één antwoord geretourneerd. De antwoorden worden geretourneerd in een gerangschikte volgorde van de betrouwbaarheidsscore verlagen.

Er is een betrouwbaarheidsscore tussen 0 en 100 liggen.

|Score-waarde|Betrouwbaarheid|
|--|--|
|100|Een exacte overeenkomst van de aanvraag voor de gebruiker en een vraag KB|
|90|Overeenkomen met hoge betrouwbaarheid - de meeste van de woorden|
|40-60|Vertrouwen van de reële - belangrijk woorden overeen met|
|10|Lage vertrouwen - belangrijk woorden komen niet overeen|
|0|Geen overeenkomst word|


## <a name="similar-confidence-scores"></a>Vergelijkbare vertrouwen scores
Wanneer u meerdere antwoorden op een vergelijkbare betrouwbaarheidsscore hebt, is het waarschijnlijk dat de query te algemeen en daarom overeenkomende kans met meerdere antwoorden was. Probeer uw vragen en antwoorden supereenvoudig beter structuur zodat elke entiteit QnA een afzonderlijke intentie heeft.


## <a name="improving-confidence-scores"></a>Verbetering van vertrouwen scores
Ter verbetering van de betrouwbaarheidsscore van een bepaald antwoord op de gebruikersquery van een, kunt u de gebruikersquery toevoegen aan de knowledge base als een alternatieve vraag op dat-antwoord.
   
## <a name="confidence-score-differences"></a>Vertrouwen van de score verschillen
De betrouwbaarheidsscore van een antwoord kan negligibly wisselen tussen de test- en gepubliceerde versie van de knowledge base, zelfs als de inhoud hetzelfde is. Dit komt doordat de inhoud van de test en de gepubliceerde knowledge base bevinden zich in verschillende Azure Search-index.

Hier ziet u hoe de [publiceren](../How-To/publish-knowledge-base.md) bewerking werkt.


## <a name="no-match-found"></a>Er is geen overeenkomst gevonden
Wanneer er geen goede overeenkomst is gevonden door de kerntechnologie, wordt de betrouwbaarheidsscore van 0,0 of 'Geen' wordt geretourneerd en wordt het standaardantwoord is 'Geen goede treffer gevonden in de KB'. U kunt deze reactie standaard in de bot of toepassing code aanroepen van het eindpunt overschrijven. U kunt ook u kunt ook het antwoord voor overschrijven instellen in Azure en Hiermee wordt de standaardwaarde voor alle knowledge bases geïmplementeerd in een bepaalde QnA Maker-service gewijzigd.

1. Ga naar de [Azure-portal](http://portal.azure.com) en navigeer naar de resourcegroep die de QnA Maker-service die u hebt gemaakt.

2. Klik hier om de **App Service**.

    ![Toegang tot appservice](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Klik op **toepassingsinstellingen** en bewerk de **DefaultAnswer** veld naar de gewenste Standaardantwoord. Klik op **Opslaan**.

    ![Standaardantwoord wijzigen](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Uw App-service opnieuw starten

    ![QnA Maker appservice opnieuw opstarten](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gegevensbronnen die worden ondersteund](./data-sources-supported.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van QnA Maker](../Overview/overview.md)
