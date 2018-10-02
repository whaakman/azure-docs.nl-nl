---
title: Betrouwbaarheidsscore - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Uitleg over betrouwbaarheidsscore
services: cognitive-services
author: tulasim88
manager: pchoudh
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 09/27/2018
ms.author: tulasim
ms.openlocfilehash: e878da5f6741b1a4c31874af05b7a37f6dee21df
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586220"
---
# <a name="confidence-score"></a>Betrouwbaarheidsscore
Wanneer de aanvraag voor een gebruiker wordt vergeleken met een knowledge base, retourneert de QnA Maker relevante antwoorden, samen met een betrouwbaarheidsscore. Deze score geeft aan dat het vertrouwen dat het antwoord de juiste overeenkomst voor de opgegeven gebruiker-query is. 

De betrouwbaarheidsscore is een getal tussen 0 en 100 liggen. Een score van 100, is waarschijnlijk een exacte overeenkomst, terwijl een score van 0 betekent dat dat er geen overeenkomende antwoord is gevonden. Hoe hoger de score - hoe groter het vertrouwen in het antwoord. Voor een bepaalde query, kunnen er meerdere antwoorden geretourneerd. In dat geval worden de antwoorden geretourneerd in volgorde van de betrouwbaarheidsscore verlagen.

In het onderstaande voorbeeld ziet u één QnA entiteit, met 2 vragen. 


![Voorbeeld van een combinatie van QnA](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

U kunt voor het bovenstaande voorbeeld - scores, zoals het bereik van de voorbeeld-score onderstaande-voor verschillende typen query's van gebruikers verwachten:


![Kerntechnologie score bereik](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


De volgende tabel staan de typische vertrouwen die voor een bepaald scorebereik zijn gekoppeld.

|Score-waarde|Betekenis van score|Voorbeeld van een Query|
|--|--|--|
|90 - 100|Een bijna exact overeenkomen met de gebruikersquery en een vraag KB|"Mijn wijzigingen in de Knowledge base worden niet bijgewerkt nadat deze is gepubliceerd"|
|> 70|Hoge betrouwbaarheid - doorgaans een goed antwoord die volledig vindt u antwoorden op query van de gebruiker|'Ik mijn KB gepubliceerd, maar deze niet wordt bijgewerkt'|
|50 - 70|Gemiddeld vertrouwen - doorgaans een redelijk goed antwoord dat de belangrijkste doel van de gebruikersquery moet beantwoorden|"Moet ik mijn updates opslaan voordat ik mijn KB publiceren?"|
|30 - 50|Lage vertrouwen - doorgaans een gerelateerde antwoord, die gedeeltelijk vindt u antwoorden op de intentie van de gebruiker|"Wat doet de opslaan en train?"|
|< 30|Zeer lage vertrouwen - doorgaans query van de gebruiker niet reageert, maar heeft een overeenkomende woorden of zinsdelen |"Waar kan ik synoniemen toevoegen aan mijn KB"|
|0|Er is geen overeenkomst, zodat het antwoord niet wordt geretourneerd.|"Wat de service kost"|

## <a name="choose-a-score-threshold"></a>Kies een score drempelwaarde
De bovenstaande tabel bevat de scores dat wordt verwacht dat op de meeste kB's. Echter omdat elke KB anders is en verschillende typen woorden heeft, intents en doelstellingen-aangeraden u testen en de drempelwaarde kiezen die het meest werkt voor u. De standaard- en aanbevolen drempelwaarde die voor de meeste kB's werken moet, is **50**.

Bij het kiezen van de drempelwaarde, houd rekening met de balans tussen nauwkeurigheid en dekking en de drempelwaarde op basis van uw behoeften aanpassen.

- Als **nauwkeurigheid** (of precisie) is belangrijker voor uw scenario en vervolgens uw drempelwaarde verhogen. Op deze manier telkens wanneer u een antwoord retourneren, worden een nog veel meer CONFIDENT case en nog veel meer waarschijnlijk dat de antwoord-gebruikers op zoek bent. In dit geval als u uiteindelijk meer vragen met openstaande verlaten. *Bijvoorbeeld:* als u de drempelwaarde **70**, mogelijk ontbreken bepaalde niet-eenduidige voorbeelden likes 'wat wordt opgeslagen en trainen?'.

- Als **dekking** (of intrekken) is meer belangrijke - en u wilt beantwoorden als veel vragen mogelijk, zelfs als er slechts een gedeeltelijke relatie van de gebruiker vragen-vervolgens VERLAAGT de drempel. Dit betekent dat er mogelijk meer situaties waarbij het antwoord niet de werkelijke query van de gebruiker beantwoordt, maar sommige andere enigszins gerelateerde antwoord krijgt. *Bijvoorbeeld:* als u de drempelwaarde **30**, u mogelijk niet erg gerelateerde antwoorden zoals beantwoorden met het bovenstaande voorbeeld geven, voor query's, zoals "waar kan ik bewerken mijn KB?'


## <a name="improve-confidence-scores"></a>Vertrouwen van de scores verbeteren
Ter verbetering van de betrouwbaarheidsscore van een bepaald antwoord op de gebruikersquery van een, kunt u de gebruikersquery toevoegen aan de knowledge base als een alternatieve vraag op dat-antwoord.


## <a name="similar-confidence-scores"></a>Vergelijkbare vertrouwen scores
Wanneer u meerdere antwoorden op een vergelijkbare betrouwbaarheidsscore hebt, is het waarschijnlijk dat de query te algemeen en daarom overeenkomende kans met meerdere antwoorden was. Probeer uw vragen en antwoorden supereenvoudig beter structuur zodat elke entiteit QnA een afzonderlijke intentie heeft.


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
