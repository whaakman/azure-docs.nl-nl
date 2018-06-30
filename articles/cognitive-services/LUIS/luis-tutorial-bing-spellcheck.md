---
title: Bing spellen controleren API v7 toevoegen aan LUIS query's | Microsoft Docs
titleSuffix: Azure
description: Juist gespeld woorden in utterances door Bing spellen controleren API V7 toe te voegen aan LUIS endpoint-query's.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/27/2018
ms.author: v-geberr
ms.openlocfilehash: 96b23146e726b7fee86b7e449c81d7efc0073e8d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127666"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Verkeerd gespelde woorden met spellingcontrole van Bing corrigeren

Integreer uw app LUIS met [Bing spellen controleren API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) verkeerd gespelde woorden in utterances corrigeren voordat LUIS de score en entiteiten van de utterance voorspelt. 

## <a name="create-first-key-for-bing-spell-check-v7"></a>Eerste sleutel voor Bing spellen controleren V7 maken
Uw [eerste Bing spellen controleren API v7 sleutel](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) is gratis. 

![Gratis sleutel maken](./media/luis-tutorial-bing-spellcheck/free-key.png)

< een naam 'maken-subscription-key"></a>
## <a name="create-endpoint-key"></a>Sleutel voor eindpunt maken
Als uw gratis sleutel is verlopen, moet u een eindpuntsleutel maken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer **maken van een resource** in de linkerbovenhoek.

3. Typ `Bing Spell Check API V7` in het zoekvak.

    ![Zoeken naar Bing spellen API V7 controleren](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Selecteer de service. 

5. Een paneel wordt weergegeven aan de rechterkant met gegevens met inbegrip van de juridische mededeling. Selecteer **maken** om te beginnen met het proces voor het abonnement maken. 

6. Voer uw service-instellingen in het volgende deelvenster. Wacht u totdat het proces voor het maken van service voltooid.

    ![Voer service-instellingen](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Selecteer **alle resources** onder de **Favorieten** titel op de navigatiebalk links.

8. Selecteer de nieuwe service. Het type **cognitieve Services** en de locatie is **globale**. 

9. Selecteer in het hoofdpaneel **sleutels** om te zien van uw nieuwe sleutels.

    ![Pak sleutels](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. Kopieer de eerste sleutel. U hoeft alleen een van de twee sleutels. 

## <a name="using-the-key-in-luis-test-panel"></a>Met behulp van de sleutel in LUIS test Configuratiescherm
Er zijn twee plaatsen in LUIS de sleutel te gebruiken. De eerste is in de [test Configuratiescherm](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). De sleutel wordt niet opgeslagen in LUIS, maar in plaats daarvan wordt een sessievariabele. U moet de sleutel instellen elke keer dat u wilt dat het paneel test de Bing spellen controleren API v7-service op de utterance toepassen. Zie [instructies](interactive-test.md#view-bing-spell-check-corrections-in-test-panel) in het deelvenster test voor het instellen van de sleutel.

## <a name="adding-the-key-to-the-endpoint-url"></a>De sleutel toe te voegen aan de eindpunt-URL
De eindpunt-query moet de sleutel doorgegeven in de queryreeksparameters voor elke query die u wilt toepassen correctie. U hebt mogelijk een chatbot die LUIS aanroept of kunt u rechtstreeks het eindpunt LUIS API aanroepen. Ongeacht hoe het eindpunt wordt aangeroepen, moet elke aanroep bevatten de vereiste informatie voor correcties goed te laten werken.

Het eindpunt-URL heeft meerdere waarden die moet juist worden doorgegeven. De Bing spellen controleren API v7-sleutel is gewoon een van deze. U moet instellen de **spellingcontrole** -parameter voor waar en u moet de waarde van **bing-spellen-controle-subscription-key** aan de sleutelwaarde:

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appID}?Subscription-Key={luisKey}&spellCheck=**true**& bing-spellen-controle-subscription-key =**{bingKey}**& uitgebreide = true & van timezoneoffset is = 0 & q = {utterance}

## <a name="send-misspelled-utterance-to-luis"></a>Verkeerd gespelde utterance naar LUIS verzenden
1. In een webbrowser, kopieert u de bovenstaande tekenreeks en vervang de `region`, `appId`, `luisKey`, en `bingKey` met uw eigen waarden. Zorg ervoor dat u de regio eindpunt gebruiken als deze van de publicatie verschilt [regio](luis-reference-regions.md).

2. Toevoegen aan een verkeerd gespelde utterance zoals 'hoe ver is de mountainn?'. In het Engels, `mountain`, met een `n`, de spelling juist is. 

3. Selecteer invoeren voor het verzenden van de query naar LUIS.

4. LUIS reageert met een JSON-resultaat voor `How far is the mountain?`. Als Bing spellen controleren API v7 onjuist gespeld, detecteert de `query` de oorspronkelijke query bevat het veld in de JSON-antwoord van de app LUIS en de `alteredQuery` veld bevat de gecorrigeerde query verzonden naar LUIS.

```
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Fouten negeren
Als u niet wilt dat de Bing spellen controleren API v7-service wilt gebruiken, kunt u utterances waarvoor spellingcontrole uit te voeren zodat LUIS juiste spelling, evenals typefouten weet label. Deze optie vereist meer labels inspanning dan het gebruik van een spellingcontrole.

## <a name="publishing-page"></a>Publishing pagina
De [publishing](publishapp.md) op de pagina een **spellingcontrole van Bing inschakelen** selectievakje. Dit is nuttig voor het maken van de sleutel en begrijpen hoe de eindpunt-URL wordt gewijzigd. U hebt nog wel de juiste eindpunt-parameters gebruiken om spelling voor elke utterance gecorrigeerd. 

> [!div class="nextstepaction"]
> [Meer informatie over voorbeeld utterances](luis-how-to-add-example-utterances.md)
