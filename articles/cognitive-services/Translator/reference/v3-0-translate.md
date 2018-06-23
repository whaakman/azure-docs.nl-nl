---
title: Microsoft Translator tekst API vertalen methode | Microsoft Docs
titleSuffix: Cognitive Services
description: Gebruik de methode Microsoft Translator tekst API vertalen.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: d8d5e1e2fac747fa733f1d92c08008b7eac2a1bc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345823"
---
# <a name="text-api-30-translate"></a>Tekst API 3.0: vertalen

Zet tekst.

## <a name="request-url"></a>Aanvraag-URL

Verzenden van een `POST` aanvraag voor het:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
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
    <td>*Optionele parameter*.<br/>Hiermee geeft u de taal van de ingevoerde tekst. Welke talen beschikbaar zijn voor vertaald uit door het opzoeken van vinden [ondersteunde talen](.\v3-0-languages.md) met behulp van de `translation` bereik. Als de `from` parameter niet is opgegeven, de van de automatische taaldetectie wordt toegepast om te bepalen van de bron-taal.</td>
  </tr>
  <tr>
    <td>tot</td>
    <td>*Vereiste parameter*.<br/>Hiermee geeft u de taal van de uitvoertekst. De taal van het doel moet een van de [ondersteunde talen](.\v3-0-languages.md) opgenomen in de `translation` bereik. Bijvoorbeeld: `to=de` kunnen worden omgezet naar Duits.<br/>Het is mogelijk kunnen worden omgezet naar meerdere talen tegelijkertijd door de parameter in de queryreeks te herhalen. Bijvoorbeeld: `to=de&to=it` kunnen worden omgezet naar Duits en Italiaans.</td>
  </tr>
  <tr>
    <td>Teksttype</td>
    <td>*Optionele parameter*.<br/>Hiermee wordt aangegeven of de tekst wordt vertaald tekst zonder opmaak of HTML-tekst. HTML-code moet een geldige, volledige-element. Mogelijke waarden zijn: `plain` (standaard) of `html`.</td>
  </tr>
  <tr>
    <td>category</td>
    <td>*Optionele parameter*.<br/>Een tekenreeks opgeven van de categorie (domein) van de vertaling. Deze parameter wordt gebruikt om op te halen vertalingen van een aangepaste systeem gebouwd met [aangepaste conversieprogramma](../customization.md). Standaardwaarde: `general`.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td>*Optionele parameter*.<br/>Hiermee geeft u op hoe profanities moeten worden behandeld in vertalingen. Mogelijke waarden zijn: `NoAction` (standaard), `Marked` of `Deleted`. Zie voor informatie over de manieren om te behandelen taalgebruik, [taalgebruik verwerking](#handle-profanity).</td>
  </tr>
  <tr>
    <td>profanityMarker</td>
    <td>*Optionele parameter*.<br/>Hiermee geeft u op hoe profanities moet worden gemarkeerd met de vertaling. Mogelijke waarden zijn: `Asterisk` (standaard) of `Tag`. Zie voor informatie over de manieren om te behandelen taalgebruik, [taalgebruik verwerking](#handle-profanity).</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td>*Optionele parameter*.<br/>Geeft aan of de projectie van de uitlijning van tekst naar vertaalde tekst bevatten. Mogelijke waarden zijn: `true` of `false` (standaard). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td>*Optionele parameter*.<br/>Geeft aan of zin grenzen voor de ingevoerde tekst en de vertaalde tekst bevatten. Mogelijke waarden zijn: `true` of `false` (standaard).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td>*Optionele parameter*.<br/>Hiermee geeft u een alternatieve taal als de taal van de ingevoerde tekst kan niet worden vastgesteld. Taal automatische detectie wordt toegepast wanneer de `from` parameter wordt weggelaten. Als de detectie is mislukt, de `suggestedFrom` taal wordt aangenomen.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Optionele parameter*.<br/>Hiermee geeft u het script van de ingevoerde tekst.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Optionele parameter*.<br/>Hiermee geeft u het script van de vertaalde tekst.</td>
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

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met een string-eigenschap met de naam `Text`, die staat voor de tekenreeks kunnen worden omgezet.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 25 elementen hebben.
* De volledige tekst is opgenomen in de aanvraag kan niet groter zijn dan 5000 tekens inclusief spaties.

## <a name="response-body"></a>Antwoordtekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `detectedLanguage`: Er is een object met een beschrijving van de gedetecteerde taal via de volgende eigenschappen:

      * `language`: Een reeks maken die de code van de gedetecteerde taal.

      * `score`: Een float-waarde die aangeeft het vertrouwen in het resultaat. De score tussen nul en één is en een lage score geeft aan dat een lage vertrouwen.

    De `detectedLanguage` eigenschap is alleen aanwezig in het resultaatobject wanneer automatische detectie van taal wordt aangevraagd.

  * `translations`: Er is een matrix van de vertaling van resultaten. De grootte van de matrix overeenkomt met het aantal doel talen opgegeven via de `to` queryparameter. Elk element in de matrix bevat:

    * `to`: Een reeks maken die de taalcode van de doel-taal.

    * `text`: Een tekenreeks waarin de vertaalde tekst.

    * `transliteration`: Er is een object waardoor de vertaalde tekst in het script dat is opgegeven door de `toScript` parameter.

      * `script`: Een tekenreeks opgeven van het doel-script.   

      * `text`: Een tekenreeks waarin de vertaalde tekst in het doel-script.

    De `transliteration` object is niet opgenomen als transliteration niet plaats vindt.

    * `alignment`: Er is een object met een enkele tekenreekseigenschap met de naam `proj`, dat is toegewezen invoertekst vertaalde tekst. De uitlijning informatie is alleen beschikbaar wanneer de aanvraagparameter `includeAlignment` is `true`. Uitlijning wordt geretourneerd als een string-waarde van de volgende indeling: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  De dubbele punt scheidt begin en einde index, het streepje scheidt de talen en ruimte scheidt de woorden. Een woord mogelijk zijn uitgelijnd met nul, één of meerdere woorden in de andere taal en de uitgelijnde woorden is mogelijk niet-aaneengesloten. Wanneer er geen uitlijning-informatie beschikbaar is, wordt de uitlijning element niet leeg zijn. Zie [uitlijning informatie verkrijgen](#obtain-alignment-information) voor een voorbeeld en -beperkingen.

    * `sentLen`: Er is een object zin grenzen in de invoer en uitvoer teksten retourneren.

      * `srcSentLen`: Er is een matrix met gehele getallen die de lengte van de zinnen in de ingevoerde tekst vertegenwoordigt. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

      * `transSentLen`: Er is een matrix met gehele getallen die de lengte van de zinnen in de vertaalde tekst vertegenwoordigt. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

    Zin grenzen zijn alleen opgenomen wanneer de aanvraagparameter `includeSentenceLength` is `true`.

  * `sourceText`: Er is een object met een enkele tekenreekseigenschap met de naam `text`, waardoor de ingevoerde tekst in het standaardscript van de bron-taal. `sourceText` eigenschap is alleen beschikbaar wanneer de invoer wordt uitgedrukt in een script dat niet het gebruikelijke script voor de taal. Bijvoorbeeld, als de invoer zijn geschreven in Latijnse script vervolgens Arabisch `sourceText.text` zou worden dezelfde Arabische tekst geconverteerd naar Arabische script.

Voorbeeld van JSON-antwoorden vindt u in de [voorbeelden](#examples) sectie.

## <a name="response-status-codes"></a>Statuscodes van antwoorden

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag als resultaat geeft. 

<table width="100%">
  <th width="20%">Statuscode</th>
  <th>Beschrijving</th>
  <tr>
    <td>200</td>
    <td>Gelukt.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Een van de queryparameters is ontbreekt of ongeldig. Aanvraagparameters voordat u probeert te corrigeren.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>De aanvraag kan niet worden geverifieerd. Controleer of de referenties opgegeven en geldig zijn.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>De aanvraag is niet gemachtigd. Controleer het foutbericht voor meer informatie. Dit betekent meestal dat alle gratis vertalingen opgegeven met een proefabonnement van zijn gebruikt.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>De aanroeper is te veel aanvragen verzenden.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Er is een onverwachte fout opgetreden. Als de fout zich blijft voordoen, met rapporteren: datum en tijd van de fout, aanvraag-id van antwoordheader `X-RequestId`, en de client-id van de aanvraag-header `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>De server is tijdelijk niet beschikbaar. De aanvraag opnieuw proberen. Als de fout zich blijft voordoen, met rapporteren: datum en tijd van de fout, aanvraag-id van antwoordheader `X-RequestId`, en de client-id van de aanvraag-header `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Voorbeelden

### <a name="translate-a-single-input"></a>Een enkele invoer vertalen

In dit voorbeeld laat zien hoe Vertaal één zin van Engels naar vereenvoudigd Chinees.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

De antwoordtekst is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

De `translations` matrix bevat een element, wat zorgt voor de omzetting van de één tekstwaarde in de invoer.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Een enkele invoer met automatische detectie van taal te vertalen

In dit voorbeeld laat zien hoe Vertaal één zin van Engels naar vereenvoudigd Chinees. De aanvraag is geen opgegeven voor de invoertaal. Automatische detectie van de bron-taal wordt in plaats daarvan gebruikt.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

De antwoordtekst is:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
Het antwoord is vergelijkbaar met het antwoord van het vorige voorbeeld. Aangezien taal automatische detectie is aangevraagd, wordt ook informatie over de taal voor de ingevoerde tekst is gedetecteerd in het antwoord bevat. 

### <a name="translate-with-transliteration"></a>Met transliteration vertalen

Laten we het vorige voorbeeld uitbreiden door transliteration toe te voegen. De volgende aanvraag gevraagd of u een Chinees vertaling geschreven in Latijnse script.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

De antwoordtekst is:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"text":"nǐ hǎo , nǐ jiào shén me míng zì ？","script":"Latn"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Het resultaat van de vertaling bevat nu een `transliteration` eigenschap waarmee de vertaalde tekst met behulp van Latijnse tekens.

### <a name="translate-multiple-pieces-of-text"></a>Meerdere tekst vertalen

Meerdere tekenreeksen in één keer vertalen is gewoon een kwestie van het opgeven van een matrix met tekenreeksen in de aanvraagtekst.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

---

De antwoordtekst is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Kan vertalen naar meerdere talen

In dit voorbeeld laat zien hoe Vertaal dezelfde invoer voor verschillende talen in een aanvraag.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

De antwoordtekst is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Ingang taalgebruik

De NAT-service wordt normaal gesproken taalgebruik die aanwezig is in de bron in de vertaling behouden. De mate van taalgebruik en de context waarin woorden schennende verschillen tussen culturen en daardoor de mate van taalgebruik in de taal van het doel kan versterkt of verminderd.

Als u wilt vermijden taalgebruik in de vertaling, ongeacht de aanwezigheid van taalgebruik in de brontekst, kunt u de optie voor het filteren taalgebruik. De optie kunt u kiezen of u zien taalgebruik verwijderd wilt, ongeacht of u markeren profanities met de juiste labels wilt (zodat u de optie voor het toevoegen van uw eigen naverwerking) of u geen actie ondernomen wilt. De toegestane waarden van `ProfanityAction` zijn `Deleted`, `Marked` en `NoAction` (standaard).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Actie</th>
  <tr>
    <td>`NoAction`</td>
    <td>Dit is de standaardinstelling. Taalgebruik geeft van bron naar doel.<br/><br/>
    **Voorbeeld van de bron (Japans)**: 彼はジャッカスです。<br/>
    **Voorbeeld van de vertaling (Engels)**: hij een jackass is.
    </td>
  </tr>
  <tr>
    <td>`Deleted`</td>
    <td>Ongepaste woorden wordt verwijderd uit de uitvoer zonder vervanging.<br/><br/>
    **Voorbeeld van de bron (Japans)**: 彼はジャッカスです。<br/>
    **Voorbeeld van de vertaling (Engels)**: hij een.
    </td>
  </tr>
  <tr>
    <td>`Marked`</td>
    <td>Ongepaste woorden worden vervangen door een markering in de uitvoer. De markering is afhankelijk van de `ProfanityMarker` parameter.<br/><br/>
Voor `ProfanityMarker=Asterisk`, schennende woorden worden vervangen door `***`:<br/>
    **Voorbeeld van de bron (Japans)**: 彼はジャッカスです。<br/>
    **Voorbeeld van de vertaling (Engels)**: hij een \* \* \*.<br/><br/>
Voor `ProfanityMarker=Tag`, schennende woorden worden omgeven door een XML-labels &lt;taalgebruik&gt; en &lt;/profanity&gt;:<br/>
    **Voorbeeld van de bron (Japans)**: 彼はジャッカスです。<br/>
    **Voorbeeld van de vertaling (Engels)**: hij een &lt;taalgebruik&gt;jackass&lt;/profanity&gt;.
  </tr>
</table> 

Bijvoorbeeld:

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Hiermee wordt geretourneerd:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Vergelijken met:

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Deze laatste aanvraag is geretourneerd:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Vertaal inhoud met opmaak en bepaal wat wordt vertaald.

Het is gebruikelijk om te vertalen inhoud waaronder markup zoals uit een HTML-pagina-inhoud of inhoud van een XML-document. Queryparameter opnemen `textType=html` tijdens het vertalen van inhoud met labels. Bovendien is het soms handig voor het uitsluiten van specifieke inhoudstypen van de vertaling. U kunt het kenmerk `class=notranslate` om op te geven van inhoud die moet blijven in de oorspronkelijke taal. In het volgende voorbeeld wordt de inhoud in de eerste `div` element niet worden vertaald, terwijl de inhoud van de tweede `div` element worden vertaald.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Hier volgt een voorbeeld van een aanvraag ter illustratie.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

---

Het antwoord is:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Uitlijning informatie verkrijgen

Geef voor het ontvangen van uitlijning informatie `includeAlignment=true` op de queryreeks.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

Het antwoord is:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

De uitlijningsgegevens begint met `0:2-0:1`, wat inhoudt dat de eerste drie tekens in de brontekst (`The`) toewijzen aan de eerste twee tekens in de vertaalde tekst (`La`).

Houd rekening met de volgende beperkingen:

* Uitlijning wordt alleen geretourneerd voor een subset van de paren taal:
  - van de Engelse taal;
  - vanuit een andere taal Engels, met uitzondering van vereenvoudigd Chinees, traditioneel Chinees en Lets Engels;
  - van Japans, Koreaans of naar Koreaans op Japans.
* U ontvangt geen uitlijning als zinnen voorgedefinieerde vertaling. Voorbeeld van een voorgedefinieerde vertaling is 'Dit is een test', 'Ik hou van je' en andere zinnen met hoge frequentie.

### <a name="obtain-sentence-boundaries"></a>Zin grenzen verkrijgen

Geef voor het ontvangen van informatie over zinlengte in de brontekst en de vertaalde tekst, `includeSentenceLength=true` op de queryreeks.

# <a name="curltabcurl"></a>[cURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

Het antwoord is:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Met dynamische woordenlijst vertalen

Als u de vertaling die u wilt toepassen op een woord of woordgroep al weet, kunt u deze als aantekeningen in de aanvraag opgeven. Alleen is de dynamische woordenlijst voor samengestelde woorden zoals namen van de juiste en product veilig.

De opmaak op te geven, gebruikt de volgende syntaxis.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Neem bijvoorbeeld Engelse zinnen "het woord wordomatic is een dictionary-vermelding." Het woord handhaven _wordomatic_ in de vertaling de aanvraag te verzenden:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Het resultaat is:

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Deze functie werkt op dezelfde manier met `textType=text` of met `textType=html`. De functie moet spaarzaam worden gebruikt. De juiste en ver betere manier van het aanpassen van de vertaling is met behulp van aangepaste conversieprogramma. Aangepaste conversieprogramma maakt volledig gebruik van de context en statistische kansen. Als u hebt of betaalbare trainingsgegevens waarin uw werk- of woordgroep in context maken, kunt u veel betere resultaten krijgt. [Meer informatie over aangepaste conversieprogramma](../customization.md).
 





