---
title: Microsoft Translator Text-API-woordenlijst Lookup methode | Microsoft Docs
description: Gebruik de Microsoft Translator Text-API Dictionary Lookup-methode.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 5a186f60dc099b095c00056d965aa92618c2c708
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868082"
---
# <a name="text-api-30-dictionary-lookup"></a>Tekst-API 3.0: Woordenlijst opzoeken

Biedt alternatieve vertalingen voor een woord en een klein aantal idiomatisch zinnen. Elke vertaling heeft een part-of-speech en een lijst met back-vertalingen. De back-vertalingen inschakelen om te begrijpen van de vertaling in de context van een gebruiker. De [woordenlijst voorbeeld](.\v3-0-dictionary-examples.md) bewerking kunt verder inzoomen op Zie dit voorbeeld wordt van elk paar vertaling.

## <a name="request-url"></a>Aanvraag-URL

Verzendt een `POST` aanvragen:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Parameters van de aanvraag doorgegeven aan de query-tekenreeks zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Beschrijving</th>
  <tr>
    <td>API-versie</td>
    <td>*Vereiste parameter*.<br/>De versie van de API die is aangevraagd door de client. De waarde moet liggen `3.0`.</td>
  </tr>
  <tr>
    <td>uit</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de invoertekst. De source-taal moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
  </tr>
  <tr>
    <td>tot</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de uitvoertekst. De doeltaal moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `dictionary` bereik.</td>
  </tr>
</table>

Aanvraagheaders zijn onder andere:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>_Een autorisatie_<br/>_koptekst_</td>
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
    <td>*Optioneel*.<br/>Een client gegenereerde GUID voor het aanduiden van de aanvraag. U kunt deze header weglaten als u de trace-ID opnemen in de querytekenreeks met behulp van een queryparameter met de naam `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagtekst

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met de tekenreekseigenschap van een met de naam `Text`, die de term lookup vertegenwoordigt.

```json
[
    {"Text":"fly"}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 10 elementen hebben.
* De tekstwaarde van een element van de matrix niet langer zijn dan 100 tekens inclusief spaties.

## <a name="response-body"></a>De hoofdtekst van antwoord

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `normalizedSource`: Een tekenreeks waarin de gestandaardiseerde vorm van de bronterm. Bijvoorbeeld, als de aanvraag 'JOHN', is de gestandaardiseerde vorm 'john'. De inhoud van dit veld wordt de invoer naar [lookup voorbeelden](.\v3-0-dictionary-examples.md).
    
  * `displaySource`: Een tekenreeks die de bronterm best in een formulier geven geschikt om eindgebruikers weer te geven. Bijvoorbeeld, als de invoer is 'JOHN', de weergaveformulier weer de gebruikelijke spelling van de naam: "John". 

  * `translations`: Een lijst van vertalingen voor de bronterm. Elk element van de lijst is een object met de volgende eigenschappen:

    * `normalizedTarget`: Een tekenreeks waarin de gestandaardiseerde vorm van deze term in de doel-taal. Deze waarde moet worden gebruikt als invoer voor [lookup voorbeelden](.\v3-0-dictionary-examples.md).

    * `displayTarget`: Een tekenreeks die de term in de gewenste taal en in een formulier beste geven geschikt om eindgebruikers weer te geven. Over het algemeen dit alleen wijkt af van de `normalizedTarget` in termen van hoofdlettergebruik. Bijvoorbeeld, een hoofdletter, zoals "Juan" heeft `normalizedTarget = "juan"` en `displayTarget = "Juan"`.

    * `posTag`: Een tekenreeks die deze term koppelen aan een tag part-of-speech.

        | Naam van de tag | Beschrijving  |
        |----------|--------------|
        | CORRECTIE      | Bijvoeglijke naamwoorden   |
        | ADV      | Bewerkingsparameters      |
        | CONJ     | Voegwoorden |
        | DET      | Determiners  |
        | MODAAL    | Termen        |
        | ZELFSTANDIG NAAMWOORD     | Zelfstandige naamwoorden        |
        | PREP     | Voorzetsels |
        | PRON     | Voornaamwoorden     |
        | TERM     | Termen        |
        | ANDERE    | Overige        |

        Deze tags zijn als een opmerking implementatie bepaald door part-of-speech tagging aan van de Engelse, en vervolgens de meest voorkomende tag voor elk paar bron/doel. Dus als mensen vaak Spaans woord naar een ander part-of-speech label in het Engels vertalen, kunnen tags uiteindelijk worden verkeerde (met betrekking tot het Spaans woord).

    * `confidence`: Een waarde tussen 0,0 en 1,0 waarmee de "Vertrouwelijk" (of meer nauwkeurig, "waarschijnlijkheid in de trainingsgegevens') van deze NAT-paar. De som van de scores vertrouwen voor een gegevensbron word kan wel of niet kan oplopen tot 1.0. 

    * `prefixWord`: Een tekenreeks waarin het woord om weer te geven als een voorvoegsel van de vertaling. Dit is momenteel de gendered determiner van zelfstandige naamwoorden in talen die u hebt gendered determiners. Bijvoorbeeld, is het voorvoegsel van het Spaans woord 'mosca' 'la', omdat 'mosca' een vrouwelijke zelfstandig naamwoord in het Spaans is. Dit is alleen afhankelijk van de vertaling en niet op de bron. Als er geen voorvoegsel, wordt het een lege tekenreeks zijn.
    
    * `backTranslations`: Een lijst met "back vertalingen' van het doel. Bijvoorbeeld, de woorden die het doel zich vertalen in gegevensbron. De lijst kan worden gegarandeerd de bron-woord dat was aangevraagd bevatten (bijvoorbeeld, als de bron word wordt opgezocht is "vliegen" en vervolgens kan worden gegarandeerd dat 'vliegen' worden opgenomen in de `backTranslations` lijst). Maar het is niet noodzakelijkerwijs bevindt zich op de eerste positie en vaak niet worden. Elk element van de `backTranslations` lijst is een object dat wordt beschreven door de volgende eigenschappen:

        * `normalizedText`: Een tekenreeks waarin de gestandaardiseerde vorm van de bronterm die een back-omzetting van het doel is. Deze waarde moet worden gebruikt als invoer voor [lookup voorbeelden](.\v3-0-dictionary-examples.md).        

        * `displayText`: Een tekenreeks die de bronterm die een back-vertaling van het doel in een formulier aanbevolen geschikt om eindgebruikers weer te geven.

        * `numExamples`: Een geheel getal dat het aantal voorbeelden die beschikbaar voor deze combinatie van de vertaling zijn vertegenwoordigt. Werkelijke voorbeelden moeten worden opgehaald met een afzonderlijke aanroep naar [lookup voorbeelden](.\v3-0-dictionary-examples.md). Het nummer van de is voornamelijk bedoeld om u te vergemakkelijken weergeven in een UX. Een gebruikersinterface kan bijvoorbeeld een hyperlink toevoegen aan de back-vertaling als het aantal voorbeelden groter dan nul is en de back-vertaling als tekst zonder opmaak weergeven als er geen voorbeelden. Houd er rekening mee dat het werkelijke aantal voorbeelden geretourneerd door een aanroep naar [lookup voorbeelden](.\v3-0-dictionary-examples.md) mogelijk minder dan `numExamples`, omdat er aanvullende filters kan worden toegepast op elk gewenst moment verwijderen 'slechte'-voorbeelden.
        
        * `frequencyCount`: Een geheel getal dat de frequentie van deze NAT-paar in de gegevens vertegenwoordigt. Het belangrijkste doel van dit veld is om te voorzien van een manier om te sorteren van back-vertalingen, zodat de meest voorkomende voorwaarden die voor het eerst een gebruikersinterface.

    > [!NOTE]
    > Als de term wordt opgezochte niet in de woordenlijst bestaat, het antwoord is 200 (OK), maar de `translations` lijst is een lege lijst.

## <a name="examples"></a>Voorbeelden

In dit voorbeeld laat zien hoe voor het opzoeken van alternatieve vertalingen in het Spaans van de Engelse term `fly` .

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

---

De hoofdtekst van antwoord (afgekort voor de duidelijkheid) is:

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

Dit voorbeeld laat zien wat er gebeurt wanneer de term wordt opgezocht niet voor het paar geldig woordenlijst bestaat.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

---

Omdat de term niet in de woordenlijst gevonden is, de antwoordtekst bevat een lege `translations` lijst.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
