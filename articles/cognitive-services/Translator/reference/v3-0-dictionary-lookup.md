---
title: Microsoft Translator tekst API woordenlijst look-upmethode | Microsoft Docs
description: Gebruik de methode Microsoft Translator tekst API Dictionary Lookup.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 31435fcfca61517bfc72d534e911a1dcadbee52b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345605"
---
# <a name="text-api-30-dictionary-lookup"></a>Tekst API 3.0: Het woordenboek

Alternatieve vertalingen biedt voor een woord en een klein aantal idiomatische zinnen. Elke vertaling is onderdeel van spraak en een lijst met back-vertalingen. De back-vertalingen inschakelen om te begrijpen van de vertaling in de context van een gebruiker. De [woordenlijst voorbeeld](.\v3-0-dictionary-examples.md) bewerking verdere detailanalyse naar beneden Zie voorbeeld maakt gebruik van elk paar vertaling staat.

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `POST` aanvraag voor het:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Aanvragen in de queryreeks doorgegeven parameters zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Beschrijving</th>
  <tr>
    <td>API-versie</td>
    <td>*Vereiste parameter*.<br/>De versie van de API die door de client wordt aangevraagd. De waarde moet `3.0`.</td>
  </tr>
  <tr>
    <td>uit</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de ingevoerde tekst. De taal van de bron moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
  </tr>
  <tr>
    <td>tot</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de uitvoertekst. De taal van het doel moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
  </tr>
</table>

Aanvraagheaders omvatten:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>_Een autorisatie_<br/>_Koptekst_</td>
    <td>*Vereiste aanvraagheader*.<br/>Zie [beschikbare opties voor verificatie](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Inhoudstype</td>
    <td>*Vereiste aanvraagheader*.<br/>Hiermee geeft u het type inhoud van de nettolading. Mogelijke waarden zijn: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Vereiste aanvraagheader*.<br/>De lengte van de aanvraagtekst.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optioneel*.<br/>Een client gegenereerde GUID als unieke identificatie van de aanvraag. U kunt deze header weglaten als u de trace-ID aanwezig in de query-tekenreeks met een queryparameter met de naam `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagtekst

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met een string-eigenschap met de naam `Text`, die staat voor de term opzoeken.

```json
[
    {"Text":"fly"}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 10 elementen hebben.
* De tekstwaarde van een matrixelement kan niet groter zijn dan 100 tekens inclusief spaties.

## <a name="response-body"></a>Antwoordtekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `normalizedSource`: Een tekenreeks waarin de genormaliseerde vorm van de bronterm. Als de aanvraag is "Jan", wordt de genormaliseerde vorm bijvoorbeeld 'john' zijn. De inhoud van dit veld wordt de invoer voor [lookup voorbeelden](.\v3-0-dictionary-examples.md).
    
  * `displaySource`: Een tekenreeks die de bronterm best in een formulier geeft geschikt voor eindgebruikers weergegeven. Bijvoorbeeld als de invoer is "Jan", het formulier weergeven wordt afgestemd op de gebruikelijke spelling van de naam: 'John'. 

  * `translations`: Een lijst met vertalingen voor de bronterm. Elk element van de lijst is een object met de volgende eigenschappen:

    * `normalizedTarget`: Een tekenreeks waarin de genormaliseerde vorm van deze term in de doel-taal. Deze waarde moet worden gebruikt als invoer voor [lookup voorbeelden](.\v3-0-dictionary-examples.md).

    * `displayTarget`: Een tekenreeks die de term in de taal van het doel en in een formulier beste geeft geschikt voor eindgebruikers weergegeven. Over het algemeen wordt deze alleen wordt afwijken van de `normalizedTarget` in termen van hoofdlettergebruik. Bijvoorbeeld, een juiste zelfstandig naamwoord zoals 'Juan' hebben `normalizedTarget = "juan"` en `displayTarget = "Juan"`.

    * `posTag`: Een tekenreeks die deze term koppelen aan een onderdeel van spraak-tag.

        | Codenaam | Beschrijving  |
        |----------|--------------|
        | CORRECTIE      | Bijvoeglijke naamwoorden   |
        | ADV      | Bewerkingsparameters      |
        | CONJ     | Voegwoorden |
        | DET      | Determiners  |
        | MODAAL    | Termen        |
        | ZELFSTANDIG NAAMWOORD     | De volgende zelfstandige naamwoorden        |
        | PREP     | Voorzetsels |
        | PRON     | Voornaamwoorden     |
        | TERM     | Termen        |
        | ANDERE    | Overige        |

        Deze tags zijn als een opmerking implementatie bepaald door deel uit van spraak de Engelse kant labels, en vervolgens de meest voorkomende tag voor elk paar bron/doel. Dus als mensen vaak een Spaanse woord naar een ander deel van spraak-label in het Engels vertalen, labels kunnen worden uiteindelijk verkeerde (met betrekking tot het Spaans woord).

    * `confidence`: Een waarde tussen 0,0 en 1,0 waarmee het 'vertrouwen' (of mogelijk meer nauwkeurig 'kans in de trainingsgegevens') van die paar vertaling. De som van de scores vertrouwen voor een bron word kan of kan niet som aan 1.0. 

    * `prefixWord`: Een tekenreeks waarin de het woord om weer te geven als voorvoegsel van de vertaling. Dit is momenteel de gendered determiner van woorden in talen die u hebt gendered determiners. Het voorvoegsel van de Spaanse word 'mosca' is bijvoorbeeld 'la', omdat 'mosca' een vrouwelijke zelfstandig naamwoord in het Spaans is. Dit is alleen afhankelijk van de vertaling en niet op de bron. Als er geen voorvoegsel, zal de lege tekenreeks zijn.
    
    * `backTranslations`: Een lijst met 'back vertalingen' van het doel. Bijvoorbeeld, woorden die het doel naar vertalen kan de gegevensbron. De lijst is gegarandeerd het woord bron die is aangevraagd bevatten (bijvoorbeeld als de bron word wordt opgezocht 'vliegen', wordt kan worden gegarandeerd dat 'vliegen' worden opgenomen in de `backTranslations` lijst). Echter deze zich in de eerste positie kan niet worden gegarandeerd en vaak niet worden. Elk element van de `backTranslations` lijst is een object dat is beschreven door de volgende eigenschappen:

        * `normalizedText`: Een tekenreeks waarin de genormaliseerde vorm van de bronterm die een back-vertaling van het doel. Deze waarde moet worden gebruikt als invoer voor [lookup voorbeelden](.\v3-0-dictionary-examples.md).        

        * `displayText`: Een tekenreeks die de bronterm die een back-vertaling van het doel in een formulier best geschikt voor eindgebruikers weergegeven.

        * `numExamples`: Er is een geheel getal dat het aantal voorbeelden die beschikbaar voor deze combinatie vertaling zijn vertegenwoordigt. Werkelijke voorbeelden moeten worden opgehaald met een afzonderlijke aanroep [lookup voorbeelden](.\v3-0-dictionary-examples.md). Het nummer is voornamelijk bedoeld ter bevordering van de weergave in een UX Een gebruikersinterface kan bijvoorbeeld een hyperlink toevoegen aan de back-vertaling als het aantal voorbeelden groter zijn dan nul is en de back-vertaling als tekst zonder opmaak weergeven als er geen voorbeelden. Houd er rekening mee dat het werkelijke aantal voorbeelden geretourneerd door een aanroep naar [lookup voorbeelden](.\v3-0-dictionary-examples.md) mogelijk minder dan `numExamples`, omdat er extra filteren op elk gewenst moment verwijderen 'slechte' voorbeelden kan worden toegepast.
        
        * `frequencyCount`: Er is een geheel getal dat aangeeft van de frequentie van deze vertaling paar in de gegevens. Het belangrijkste doel van dit veld is om te voorzien van een methode terug vertalingen sorteren zodat de meest voorkomende voorwaarden die voor het eerst een gebruikersinterface.

    > [!NOTE]
    > Als de term wordt opgezochte niet in de woordenlijst bestaat, het antwoord is 200 (OK), maar de `translations` lijst is een lege lijst.

## <a name="examples"></a>Voorbeelden

In dit voorbeeld laat zien hoe voor het opzoeken van alternatieve vertalingen in het Spaans van de Engelse term `fly` .

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

---

De hoofdtekst van de reactie (afgekort voor de duidelijkheid) is:

```
[
    {
        "normalizedSource":"fly",
        "displaySource":"fly",
        "translations":[
            {
                "normalizedTarget":"volar",
                "displayTarget":"volar",
                "posTag":"VERB",
                "confidence":0.4081,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":4637},
                    {"normalizedText":"flying","displayText":"flying","numExamples":15,"frequencyCount":1365},
                    {"normalizedText":"blow","displayText":"blow","numExamples":15,"frequencyCount":503},
                    {"normalizedText":"flight","displayText":"flight","numExamples":15,"frequencyCount":135}
                ]
            },
            {
                "normalizedTarget":"mosca",
                "displayTarget":"mosca",
                "posTag":"NOUN",
                "confidence":0.2668,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":1697},
                    {"normalizedText":"flyweight","displayText":"flyweight","numExamples":0,"frequencyCount":48},
                    {"normalizedText":"flies","displayText":"flies","numExamples":9,"frequencyCount":34}
                ]
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

In dit voorbeeld ziet wat er gebeurt wanneer de term wordt opgezocht niet voor het paar geldige woordenlijst bestaat.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

---

Aangezien de term niet in de woordenlijst gevonden is, hoofdtekst van de reactie bevat een lege `translations` lijst.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
