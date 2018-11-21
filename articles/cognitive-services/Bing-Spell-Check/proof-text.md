---
title: Wat is de Bing Spellingcontrole-API?
titlesuffix: Azure Cognitive Services
description: De Bing Spellingcontrole-API maakt gebruik van Machine Learning en statistische automatische vertaling voor contextuele spellingcontrole.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: overview
ms.date: 05/03/2018
ms.author: nolachar
ms.openlocfilehash: 3a277b10561b2756fab0af6455d17557a8d93a53
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345705"
---
# <a name="what-is-bing-spell-check-api"></a>Wat is de Bing Spellingcontrole-API?

Met de Bing Spellingcontrole-API kunt u contextuele grammatica- en spellingcontrole uitvoeren.

Wat is het verschil tussen reguliere spellingcontroleprogramma's en de Bing-spellingcontrole van de derde generatie? In huidige spellingcontroleprogramma's worden de spelling en grammatica gecontroleerd op basis van regelsets van woordenlijsten, die periodiek worden bijgewerkt en uitgebreid. Dit houdt in dat de kwaliteit van de spellingcontrole afhankelijk is van de ondersteunende woordenlijst en van de mensen die de woordenlijst bijhouden. U kent dit type spellingcontrole van Microsoft Word en andere tekstverwerkingsprogramma's.

Bing daarentegen heeft een webgebaseerde spellingcontrole waarvoor wordt gebruikgemaakt van Machine Learning en statistische automatische vertaling om een steeds groter wordend, zeer contextueel algoritme dynamisch te trainen. De spellingcontrole is gebaseerd op een zeer grote verzameling webzoekopdrachten en documenten.

Deze spellingcontrole is geschikt voor elk tekstverwerkingsscenario:

- Slang en informeel taalgebruik worden herkend
- Fouten in algemene namen in context worden herkend
- Verkeerd afgebroken woorden met één vlag worden gecorrigeerd
- Kan homofonen in context corrigeren, en andere moeilijk te vinden fouten
- Biedt ondersteuning voor nieuwe merken, digitale entertainment en populaire uitdrukkingen
- Woorden die hetzelfde klinken, maar een verschillende betekenis en spelling hebben, bijvoorbeeld 'noch' en 'nog'.

## <a name="spell-check-modes"></a>Modi van spellingcontroles

De API ondersteunt twee controlemodi, `Proof` en `Spell`.  Probeer [hier](https://azure.microsoft.com/services/cognitive-services/spell-check/) voorbeelden uit.
### <a name="proof---for-documents-scenario"></a>Controleren - voor documenten
De standaardmodus is `Proof`. De spellingmodus `Proof` bevat de meest uitgebreide controles, inclusief hoofdlettergebruik, basisinterpunctie en andere functies om te helpen bij het maken van het document. Deze is echter alleen beschikbaar in en-US (Engels-Verenigde Staten), es-ES (Spaans), pt-BR (Portugees). (Opmerking: alleen in de bètaversie voor Spaans en Portugees). Voor alle andere markten stelt u de queryparameter mode in op Spelling. 
<br /><br/>**Opmerking:** als de querytekst langer is dan 4096, wordt deze afgekapt tot 4096 tekens, waarna deze wordt verwerkt. 
### <a name="spell----for-web-searchesqueries-scenario"></a>Spelling - voor webzoekopdrachten/query's
`Spell` is een krachtigere functie waarmee betere zoekresultaten worden geretourneerd. In de modus `Spell` worden de meeste spelfouten gevonden, maar worden bepaalde grammaticafouten over het hoofd gezien die wel worden ontdekt door `Proof`, bijvoorbeeld hoofdlettergebruik en herhaalde woorden.

> [!NOTE]
> * Hieronder ziet u de maximale ondersteunde querylengte. Als de query de maximale lengte overschrijdt, worden de query en de bijbehorende resultaten niet gewijzigd.
>    * 130 tekens voor de volgende taalcodes: en, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh, ko. 
>    * 65 tekens voor alle andere taalcodes.
> * De spellingmodus biedt geen ondersteuning voor vierkante haakjes (`[` en `]`) in query's, en kunnen inconsistente resultaten tot gevolg hebben. We raden u aan deze te verwijderen uit uw query's wanneer u de spellingmodus gebruikt.

## <a name="market-setting"></a>Marktinstelling
De markt moet worden opgegeven in de queryparameter in de aanvraag-URL, anders wordt de standaardmarkt gebruikt op basis van het IP-adres.


## <a name="post-vs-get"></a>POST versus GET

De API biedt ondersteuning voor HTTP POST en HTTP GET. Welke u gebruikt, is afhankelijk van de lengte van de tekst die u wilt controleren. Als de tekenreeksen altijd korter zijn dan 1500 tekens, gebruikt u een GET. Gebruik POST ter ondersteuning van tekenreeksen van maximaal 10.000 tekens. De tekstreeks kan een ongeldig UTF-8-teken bevatten.

In het volgende voorbeeld ziet u een POST-aanvraag voor het controleren van de spelling en grammatica van een tekstreeks. Het voorbeeld bevat de queryparameter [mode](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#mode) voor de volledigheid (deze kan worden weggelaten omdat `mode` standaard is ingesteld op Controleren). De queryparameter [tekst](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) bevat de tekenreeks die moet worden gecontroleerd.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

Als u HTTP GET gebruikt, neemt u de queryparameter `text` op in de queryreeks van de URL
  
Hieronder ziet u het antwoord op de vorige aanvraag. Het antwoord bevat een object [SpellCheck](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#spellcheck). 
  
```  
{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  
  
Het veld [flaggedTokens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#flaggedtokens) bevat de spel- en grammaticafouten die door de API zijn gevonden in de [tekst](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text)reeks. Het veld `token` bevat het woord dat moet worden vervangen. Gebruik nu de op nul gebaseerde offset in het veld `offset` om de token te zoeken in de `text`-tekenreeks. Vervang vervolgens het woord op die locatie door het woord in het veld `suggestion`. 

Als het veld `type` RepeatedToken is, vervangt u het token nog steeds door `suggestion`, maar moet u waarschijnlijk ook de afsluitende spatie verwijderen.

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Uw eerste aanvraag maken](quickstarts/csharp.md) om snel aan de slag te gaan met uw eerste aanvraag.

Lees de [naslaghandleiding over de Bing Spellingcontrole-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference) om hiermee vertrouwd te raken. De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten aan te vragen en de definities van de antwoordobjecten. 

Lees [Gebruiks- en weergavevereisten voor Bing](./useanddisplayrequirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.
