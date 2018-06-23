---
title: Text to Speech API van de Service Microsoft Speech | Microsoft Docs
description: De tekst-naar-spraak API gebruiken voor realtime-naar-spraak conversie in tal van stemmen en talen
services: cognitive-services
author: priyaravi20
manager: yanbo
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 03/16/2017
ms.author: priyar
ms.openlocfilehash: 4b633cefa37c11511a8171d5a7f61b03dfaa4466
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344786"
---
# <a name="bing-text-to-speech-api"></a>Bing tekst-naar-spraak API

## <a name="Introduction"></a>Inleiding

Met de Bing tekst-naar-spraak API, kan uw toepassing HTTP-aanvragen naar een cloud-server, waarbij tekst onmiddellijk moet worden gemaakt in human klinkt spraak en geretourneerd als een audiobestand verzenden. Deze API kan worden gebruikt in veel verschillende contexten voor realtime-naar-spraak conversie in tal van verschillende stemmen en talen.

## <a name="VoiceSynReq"></a>Voice synthese aanvraag

### <a name="Subscription"></a>Verificatietoken

Elke stem synthese aanvraag vereist een toegangstoken JSON Web Token (JWT). Het toegangstoken JWT wordt doorgegeven in de aanvraagheader spraak. Het token heeft een verlooptijd van 10 minuten. Zie voor meer informatie over abonneren en het verkrijgen van de API-sleutels die worden gebruikt voor het ophalen van geldige JWT-toegangstokens [cognitieve Services abonnement](https://azure.microsoft.com/try/cognitive-services/).

De API-sleutel wordt doorgegeven aan de service voor beveiligingstokens. Bijvoorbeeld:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

De vereiste header-informatie voor tokentoegang is als volgt.

Naam| Indeling | Beschrijving
----|----|----
OCP-Apim-Subscription-Key | ASCII | Uw abonnementssleutel

Retourneert de token service de toegang JWT-token als `text/plain`. En vervolgens de JWT wordt doorgegeven als een `Base64 access_token` naar het eindpunt spraak als een verificatieheader voorafgegaan door de tekenreeks `Bearer`. Bijvoorbeeld:

`Authorization: Bearer [Base64 access_token]`

Clients moeten het volgende eindpunt gebruiken voor toegang tot de service-naar-spraak:

`https://speech.platform.bing.com/synthesize`

>[!NOTE]
>Deze koppeling wordt gegenereerd nadat u een toegangstoken aangeschaft met de abonnementssleutel van uw, hebt zoals eerder beschreven, een `403 Forbidden` antwoordfout.

### <a name="Http"></a>HTTP-headers

De volgende tabel bevat de HTTP-headers die worden gebruikt voor stem synthese aanvragen.

Koptekst |Waarde |Opmerkingen
----|----|----
Inhoudstype | toepassing/ssml + xml | De invoer inhoudstype.
X-Microsoft-OutputFormat | **1.** ssml-16 khz-16-bits-mono-tts <br> **2.** raw-16 khz-16-bits-mono-pcm <br>**3.** audio-16 khz-16 kbps-mono-siren <br> **4.** riff-16 khz-16 kbps-mono-siren <br> **5.** riff-16 khz-16-bits-mono-pcm <br> **6.** audio-16 khz-128kbitrate-mono-mp3 <br> **7.** audio-16 khz-64kbitrate-mono-mp3 <br> **8.** audio-16 khz-32kbitrate-mono-mp3 | De uitvoer audio-indeling.
X-Search-toepassings-id | Een GUID (hex alleen, geen streepjes) | Een ID die een unieke identificatie van de clienttoepassing. Dit kan zijn dat de opslag-ID voor apps. Indien deze niet beschikbaar is, kan de ID van de gebruiker gegenereerde voor een toepassing zijn.
X-Search-ClientID | Een GUID (hex alleen, geen streepjes) | Een ID die een unieke identificatie van de instantie van een toepassing voor elke installatie.
Gebruikersagent | De naam van de toepassing | Naam van de toepassing is vereist en moet minder dan 255 tekens.
Autorisatie | Verificatietoken |  Zie de <a href="#Subscription">verificatietoken</a> sectie.

### <a name="InputParam"></a>Invoerparameters

Aanvragen voor de Bing tekst-naar-spraak API worden gedaan met behulp van HTTP POST-aanroepen. De headers worden opgegeven in de vorige sectie. De hoofdtekst bevat spraak synthese Markup Language (SSML) invoer met de tekst die moet worden gemaakt. Zie voor een beschrijving van de markering die wordt gebruikt voor het beheren van de aspecten van spraak zoals de taal en geslacht van de spreker de [SSML W3C-specificatie](http://www.w3.org/TR/speech-synthesis/).

>[!NOTE]
>De maximale grootte van de SSML-invoer, die wordt ondersteund is 1024 tekens, inclusief alle codes.

###  <a name="SampleVoiceOR"></a>Voorbeeld: een verzoek mondeling uitvoer

Een voorbeeld van een aanvraag van de uitvoer stem is als volgt:

```HTTP
POST /synthesize
HTTP/1.1
Host: speech.platform.bing.com

X-Microsoft-OutputFormat: riff-8khz-8bit-mono-mulaw
Content-Type: application/ssml+xml
Host: speech.platform.bing.com
Content-Length: 197
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>Microsoft Bing Voice Output API</voice></speak>
```

## <a name="VoiceOutResponse"></a>Voice uitvoer antwoord

De Bing tekst-naar-spraak API gebruikt HTTP POST te verzenden audio terug naar de client. De API-reactie bevat de audiostroom en de codec en deze overeenkomt met de indeling van de gevraagde uitvoer. De audio geretourneerd voor een bepaalde aanvraag mag niet groter zijn dan 15 seconden.

### <a name="SuccessfulRecResponse"></a>Voorbeeld: geslaagde synthese antwoord

De volgende code is een voorbeeld van een JSON-antwoord op een geslaagde stem synthese-aanvraag. De opmerkingen en opmaak van de code zijn voor dit voorbeeld alleen en worden weggelaten uit het werkelijke antwoord.

```HTTP
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

### <a name="RecFailure"></a>Voorbeeld: synthese mislukt

De volgende voorbeeldcode ziet u een JSON-antwoord aan een gesproken synthese query-fout:

```HTTP
HTTP/1.1 400 XML parser error
Content-Type: text/xml
Content-Length: 0
```

### <a name="ErrorResponse"></a>Foutberichten

Fout | Beschrijving
----|----
Onjuiste aanvraag HTTP/400 | Een vereiste parameter ontbreekt, is leeg of null is of de waarde die is doorgegeven aan een vereiste of optionele parameter is ongeldig. Een reden voor het ophalen van het antwoord 'ongeldig' met het doorgeven van een tekenreekswaarde die langer is dan de toegestane lengte. Een korte beschrijving van de parameter problematisch is opgenomen.
HTTP/401-niet toegestaan | De aanvraag is niet gemachtigd.
HTTP-/ 413 RequestEntityTooLarge  | De SSML-invoer is groter dan wat wordt ondersteund.
HTTP 502/BadGateway | Er is een probleem met netwerk of een probleem met de serverzijde.

Een voorbeeld van een reactie op een fout is als volgt:

```HTTP
HTTP/1.0 400 Bad Request
Content-Length: XXX
Content-Type: text/plain; charset=UTF-8

Voice name not supported
```

## <a name="ChangeSSML"></a>Voice-uitvoer via SSML wijzigen

Microsoft TTS-API ondersteunt SSML 1.0 zoals gedefinieerd in W3C [spraak synthese Markup Language (SSML) versie 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). Deze sectie ziet u voorbeelden van het wijzigen van bepaalde kenmerken van de gegenereerde stem uitvoer spreken classificeren, de uitspraak enz. met behulp van SSML-codes.

1. Einde toevoegen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)'> Welcome to use Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.</voice> </speak>
  ```

2. De snelheid van spreken wijzigen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody rate="+30.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

3. Uitspraak

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'> <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme></voice> </speak>
  ```

4. Volume wijzigen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody volume="+20.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

5. Presentatie wijzigen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Welcome to use <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

6. Wijziging prosody werklast

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody contour="(80%,+20%) (90%,+30%)" >Good morning.</prosody></voice> </speak>
  ```

> [!NOTE]
> De audiogegevens heeft 8 kB of 16 k wav worden opgeslagen in de volgende indeling: **CRC code** (CRC-32): 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0xFFFFFFFF; **Audio-indeling vlag**: 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0xFFFFFFFF; **Aantal samples**: 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0x7FFFFFFF; **Grootte van de hoofdtekst van de binaire**: 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0x7FFFFFFF; **Binaire hoofdtekst**: n bytes.

## <a name="SampleApp"></a>Voorbeeld van een toepassing

Zie voor meer informatie voor implementatie, de [Visual C# .NET-naar-spraak voorbeeldtoepassing](https://github.com/Microsoft/Cognitive-Speech-TTS/blob/master/Samples-Http/CSharp/TTSProgram.cs).

## <a name="SupLocales"></a>Ondersteunde talen en stem lettertypen

De volgende tabel bevat enkele van de ondersteunde talen en verwante stem lettertypen.

Landinstelling | Geslacht | De toewijzing van service
---------|--------|------------
ar bijvoorbeeld * | Vrouw | "Microsoft Server Speech Text naar stem (ar-BIJV: Hoda)"
ar-SA | Man | 'Microsoft Server Speech tekst stem (ar-SA, Naayf)'
bg-BG | Man | "Microsoft Server Speech Text to Speech stem (bg-BG, Ivan)"
CA-ES | Vrouw | "Microsoft Server Speech Text to Speech stem (ca-ES, HerenaRUS)"
cs-CZ | Man | "Microsoft Server Speech Text naar stem (cs-CZ, Jakub)"
da-DK | Vrouw | 'Microsoft Server Speech tekst stem (da-DK, HelleRUS)'
de AT | Man | 'Microsoft Server Speech tekst stem (de-AT, Michael)'
de CH | Man | 'Microsoft Server Speech tekst stem (de-h, Karsten)'
de-DE | Vrouw | 'Microsoft Server Speech tekst stem (nl-nl, Hedda)'
de-DE | Vrouw | 'Microsoft Server Speech tekst stem (nl-nl, HeddaRUS)'
de-DE | Man | 'Microsoft Server Speech tekst stem (nl-nl, Stefan, Apollo)'
el-GR | Man | 'Microsoft Server Speech tekst stem (el-GR, Stefanos)'
en-AU | Vrouw | 'Microsoft Server Speech tekst stem (en-AU, Catherine)'
en-AU | Vrouw | 'Microsoft Server Speech tekst stem (en-AU, HayleyRUS)'
NL-CA | Vrouw | 'Microsoft Server Speech tekst stem (nl-CA, Linda)'
NL-CA | Vrouw | 'Microsoft Server Speech tekst stem (nl-CA, HeatherRUS)'
NL GB | Vrouw | 'Microsoft Server Speech tekst stem (nl-GB, Susan, Apollo)'
NL GB | Vrouw | 'Microsoft Server Speech tekst stem (nl-GB, HazelRUS)'
NL GB | Man | 'Microsoft Server Speech tekst stem (nl-GB, George, Apollo)'
NL-IE | Man | 'Microsoft Server Speech tekst stem (nl-IE, Jan)'
NL-IN | Vrouw | 'Microsoft Server Speech tekst stem (nl-IN, Heera, Apollo)'
NL-IN | Vrouw | 'Microsoft Server Speech tekst stem (nl-IN, PriyaRUS)'
NL-IN | Man | 'Microsoft Server Speech tekst stem (nl-IN, Ravi, Apollo)'
nl-NL | Vrouw | 'Microsoft Server Speech tekst stem (en-US, ZiraRUS)'
nl-NL | Vrouw | 'Microsoft Server Speech tekst stem (en-US, JessaRUS)'
nl-NL | Man | 'Microsoft Server Speech tekst stem (en-US, BenjaminRUS)'
es-ES | Vrouw | "Microsoft Server Speech Text to Speech stem (es-ES, Ellen, Apollo)"
es-ES | Vrouw | "Microsoft Server Speech Text to Speech stem (es-ES, HelenaRUS)"
es-ES | Man | "Microsoft Server Speech Text to Speech stem (es-ES, Pablo, Apollo)"
es-MX | Vrouw | "Microsoft Server-stem Text to Speech (es-MX, HildaRUS)"
es-MX | Man | "Microsoft Server-stem Text to Speech (es-MX, Raul, Apollo)"
fi-FI | Vrouw | 'Microsoft Server Speech tekst stem (fi-FI, HeidiRUS)'
fr-CA | Vrouw | "Microsoft Server Speech Text to Speech stem (fr-CA, Caroline)"
fr-CA | Vrouw | "Microsoft Server Speech Text to Speech stem (fr-CA, HarmonieRUS)"
FR CH | Man | "Microsoft Server Speech Text to Speech stem (fr-h, Guillaume)"
fr-FR | Vrouw | "Microsoft Server Speech Text to Speech stem (fr-FR, Julia, Apollo)"
fr-FR | Vrouw | "Microsoft Server Speech Text to Speech stem (fr-FR, HortenseRUS)"
fr-FR | Man | "Microsoft Server Speech Text to Speech stem (fr-FR, Paul, Apollo)"
hij IL| Man| 'Microsoft Server Speech tekst stem (he-IL, Asaf)'
hi-IN | Vrouw | 'Microsoft Server Speech tekst stem (Hallo-IN, Kalpana, Apollo)'
hi-IN | Vrouw | 'Microsoft Server Speech tekst stem (Hallo-IN, Kalpana)'
hi-IN | Man | 'Microsoft Server Speech tekst stem (Hallo-IN, Hemant)'
hr-HR | Man | "Microsoft Server Speech Text to Speech stem (hr-HR, Matej)"
hu-HU | Man | 'Microsoft Server Speech tekst stem (hu-HU, Szabolcs)'
id-ID | Man | 'Microsoft Server Speech tekst stem (id-ID, Andika)'
IT-IT | Man | 'Microsoft Server Speech tekst stem (it-IT, Cosimo, Apollo)'
ja-JP | Vrouw | "Microsoft Server Speech Text to Speech stem (ja-JP, Ayumi, Apollo)"
ja-JP | Man | "Microsoft Server Speech Text to Speech stem (ja-JP, Ichiro, Apollo)"
ja-JP | Vrouw | "Microsoft Server Speech Text to Speech stem (ja-JP, HarukaRUS)"
ja-JP | Vrouw | "Microsoft Server Speech Text to Speech stem (ja-JP, LuciaRUS)"
ja-JP | Man | "Microsoft Server Speech Text to Speech stem (ja-JP, EkaterinaRUS)"
ko-KR | Vrouw | 'Microsoft Server Speech tekst stem (ko-KR, HeamiRUS)'
ms-MY | Man | "Microsoft Server-stem Text to Speech (ms-mijn Rizwan)"
nb-NO | Vrouw | "Microsoft Server-stem Text to Speech (nb-NO HuldaRUS)"
NL-NL | Vrouw | "Microsoft Server Speech Text to Speech stem (nl-NL, HannaRUS)"
pl-PL | Vrouw | "Microsoft Server Speech Text to Speech stem (pl-PL, PaulinaRUS)"
pt-BR | Vrouw | "Microsoft Server Speech Text to Speech stem (pt-BR, HeloisaRUS)"
pt-BR | Man | "Microsoft Server Speech Text to Speech stem (pt-BR, Daniel, Apollo)"
pt-PT | Vrouw | "Microsoft Server Speech Text to Speech stem (pt-PT, HeliaRUS)"
ro-RO | Man | 'Microsoft Server Speech tekst stem (ro-RO, Andrei)'
ru-RU | Vrouw | "Microsoft Server Speech Text to Speech stem (ru-RU, Irina, Apollo)"
ru-RU | Man | "Microsoft Server Speech Text to Speech stem (ru-RU, Pavel, Apollo)"
sk-SK | Man | "Microsoft Server Speech Text to Speech stem (sk-SK, Filip)"
sl-SI | Man | "Microsoft Server Speech Text to Speech stem (sl-SI, Lado)"
SV-SE | Vrouw | 'Microsoft Server Speech tekst stem (sv-SE, HedvigRUS)'
DBC-IN | Man | 'Microsoft Server Speech tekst stem (DBC-IN, Valluvar)'
th-TH | Man | "Microsoft Server Speech Text to Speech stem (th-TH, Pattara)"
tr-TR | Vrouw | "Microsoft Server Speech Text to Speech stem (tr-TR, SedaRUS)"
vi-VN | Man | "Microsoft Server-stem Text to Speech (vi-VN een)"
zh-CN | Vrouw | 'Microsoft Server Speech tekst stem (zh-CN, HuihuiRUS)'
zh-CN | Vrouw | 'Microsoft Server Speech tekst stem (zh-CN, Yaoyao, Apollo)'
zh-CN | Man | 'Microsoft Server Speech tekst stem (zh-CN, Kangkang, Apollo)'
zh-HK | Vrouw | 'Microsoft Server Speech tekst stem (zh-HK Tracy, Apollo)'
zh-HK | Vrouw | 'Microsoft Server Speech tekst stem (zh-HK TracyRUS)'
zh-HK | Man | 'Microsoft Server Speech tekst stem (zh-HK Danny, Apollo)'
zh-TW. | Vrouw | 'Microsoft Server Speech tekst stem (zh-TW, Yating, Apollo)'
zh-TW. | Vrouw | 'Microsoft Server Speech tekst stem (zh-TW, HanHanRUS)'
zh-TW. | Man | 'Microsoft Server Speech tekst stem (zh-TW, Zhiwei, Apollo)'
 * ar-bijvoorbeeld ondersteunt moderne standaard Arabisch (MSA).

> [!NOTE]
> Houd er rekening mee dat de vorige servicenamen **Microsoft Server Speech Text naar stem (cs-CZ, Vit)** en **Microsoft Server Speech Text naar stem (nl-IE Shaun)** afgeschaft na 31-3/2018 in de volgorde om te optimaliseren van de Bing Speech-API-mogelijkheden. Werk uw code bij met de bijgewerkte namen.

## <a name="TrouNSupport"></a>Probleemoplossing en ondersteuning

Plaats alle vragen en problemen op de [Bing Speech Service](https://social.msdn.microsoft.com/Forums/en-US/home?forum=SpeechService) MSDN-forum. Meer informatie, zoals omvatten:

* Een voorbeeld van de volledige aanvraag-tekenreeks.
* Indien van toepassing, meld u met de volledige uitvoer van een mislukte aanvraag, waaronder id's.
* Het percentage verzoeken die mislukken.
