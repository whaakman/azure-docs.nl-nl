---
title: Text to Speech-API van Microsoft Speech Service | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: De text to speech-API gebruiken voor realtime-naar-spraak conversie in een aantal stemmen en talen
services: cognitive-services
author: priyaravi20
manager: yanbo
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: priyar
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f7620c29e7d00a06be6d14740f05cc7543e49837
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674562"
---
# <a name="bing-text-to-speech-api"></a>Bing text to speech-API

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

## <a name="Introduction"></a>Inleiding

Met de Bing text to speech-API, kan uw toepassing HTTP-aanvragen verzenden naar een cloud-server, waarbij tekst onmiddellijk human klinkende spraaksynthese en geretourneerd als een geluidsbestand. Deze API kan in veel verschillende contexten worden gebruikt voor realtime-naar-spraak conversie in tal van verschillende stemmen en talen.

## <a name="VoiceSynReq"></a>Stem synthese aanvraag

### <a name="Subscription"></a>Autorisatietoken

Elke stem synthese-aanvraag moet een toegangstoken van JSON Web Token (JWT). De JWT-toegangstoken wordt doorgegeven in de aanvraagheader van spraak. Het token heeft een verlooptijd van 10 minuten. Zie voor meer informatie over het abonnement en het verkrijgen van API-sleutels die worden gebruikt om op te halen ongeldig JWT-tokens voor toegang tot [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/).

De API-sleutel wordt doorgegeven aan de service voor beveiligingstokens. Bijvoorbeeld:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

De vereiste header-informatie voor token-toegang is als volgt.

Name| Indeling | Description
----|----|----
OCP-Apim-Subscription-Key | ASCII | Uw abonnementssleutel

De service voor beveiligingstokens retourneert het toegangstoken JWT als `text/plain`. En vervolgens de JWT wordt doorgegeven als een `Base64 access_token` naar het eindpunt spraak als een autorisatie-header voorafgegaan door de tekenreeks `Bearer`. Bijvoorbeeld:

`Authorization: Bearer [Base64 access_token]`

Clients moeten het volgende eindpunt gebruiken voor toegang tot de Text to Speech-service:

`https://speech.platform.bing.com/synthesize`

>[!NOTE]
>Deze koppeling wordt gegenereerd totdat u kunt een toegangstoken hebt aangeschaft met uw abonnementssleutel, zoals eerder beschreven, een `403 Forbidden` antwoordfout.

### <a name="Http"></a>HTTP-headers

De volgende tabel ziet u de HTTP-headers die worden gebruikt voor spraak synthese aanvragen.

Header |Value |Opmerkingen
----|----|----
Content-Type | application/ssml+xml | De invoer inhoudstype.
X-Microsoft-OutputFormat | **1.** ssml-16 khz-16-bits-mono-tts <br> **2.** raw-16 khz-16-bits-mono-pcm <br>**3.** audio-16 khz-16 kbps-mono-siren <br> **4.** riff-16 khz-16 kbps-mono-siren <br> **5.** riff-16 khz-16-bits-mono-pcm <br> **6.** audio-16 khz-128kbitrate-mono-mp3 <br> **7.** audio-16 khz-64kbitrate-mono-mp3 <br> **8.** audio-16 khz-32kbitrate-mono-mp3 | De uitvoer audio-indeling.
X-Search-AppId | Een GUID (hexadecimaal alleen, geen streepjes) | Een ID die de clienttoepassing wordt aangeduid. Dit kan de opslag-ID voor apps zijn. Als een niet beschikbaar is, kan de ID gebruiker gegenereerd voor een toepassing zijn.
X-Search-ClientID | Een GUID (hexadecimaal alleen, geen streepjes) | Een ID die de instantie van een toepassing voor elke installatie wordt aangeduid.
Gebruikersagent | De naam van de toepassing | De toepassingsnaam is vereist en moet minder dan 255 tekens.
Autorisatie | Autorisatietoken |  Zie de <a href="#Subscription">Autorisatietoken</a> sectie.

### <a name="InputParam"></a>Invoerparameters

Aanvragen voor Bing text to speech-API worden gedaan met behulp van HTTP POST-aanroepen. De headers zijn opgegeven in de vorige sectie. De hoofdtekst bevat invoer spraak synthese Markup Language (SSML) waarmee de tekst die moet worden gemaakt. Zie voor een beschrijving van de markering die wordt gebruikt voor het beheren aspecten van spraakherkenning, zoals de taal en geslacht van de spreker analyseren, de [SSML W3C-specificatie](http://www.w3.org/TR/speech-synthesis/).

>[!NOTE]
>De maximale grootte van de invoer SSML die wordt ondersteund is 1024 tekens, inclusief alle tags.

###  <a name="SampleVoiceOR"></a>Voorbeeld: stem uitvoeraanvraag

Een voorbeeld van een aanvraag voice-uitvoer is als volgt:

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

## <a name="VoiceOutResponse"></a>Stem uitvoer antwoord

Bing text to speech-API maakt gebruik van HTTP POST voor het verzenden van audio terug naar de client. De API-reactie bevat de audiostream en de codec en deze overeenkomt met de indeling van de gevraagde uitvoer. De audio geretourneerd voor een bepaalde aanvraag mag niet groter zijn dan 15 seconden.

### <a name="SuccessfulRecResponse"></a>Voorbeeld: geslaagd synthese antwoord

De volgende code is een voorbeeld van een JSON-antwoord op een geslaagde stem synthese-aanvraag. De opmerkingen en opmaak van de code voor dit voorbeeld alleen zijn en zijn weggelaten uit de daadwerkelijke reactie.

```HTTP
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

### <a name="RecFailure"></a>Voorbeeld: synthese fout

De volgende voorbeeldcode ziet u een JSON-antwoord aan een stem-synthese query-fout:

```HTTP
HTTP/1.1 400 XML parser error
Content-Type: text/xml
Content-Length: 0
```

### <a name="ErrorResponse"></a>Foutberichten

Fout | Description
----|----
HTTP-/ 400 Ongeldige aanvraag | Er ontbreekt een vereiste parameter ontbreekt, is leeg of null zijn of de waarde die wordt doorgegeven aan een vereiste of optionele parameter is ongeldig. Eén reden voor het ophalen van het antwoord 'Ongeldige' is een tekenreekswaarde die langer is dan de toegestane lengte doorgegeven. Een korte beschrijving van de problematische parameter is opgenomen.
HTTP/401-niet gemachtigd | De aanvraag is niet gemachtigd.
HTTP/413 RequestEntityTooLarge  | De invoer SSML is groter dan wat wordt ondersteund.
HTTP/502 BadGateway | Er is een probleem met netwerk of een probleem met de serverzijde.

Een voorbeeld van een foutbericht is als volgt:

```HTTP
HTTP/1.0 400 Bad Request
Content-Length: XXX
Content-Type: text/plain; charset=UTF-8

Voice name not supported
```

## <a name="ChangeSSML"></a>Voice-uitvoer via SSML wijzigen

Text to Speech-API van Microsoft SSML 1.0 ondersteunt, zoals gedefinieerd in W3C [spraak synthese Markup Language (SSML) versie 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). In deze sectie ziet u voorbeelden van het wijzigen van bepaalde kenmerken van de gegenereerde voice-uitvoer zoals spreekstijl beoordelen, enz. met behulp van tags SSML uitspraak.

1. Einde toevoegen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)'> Welcome to use Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.</voice> </speak>
  ```

2. Snelheid van spreken wijzigen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody rate="+30.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

3. Uitspraak van

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'> <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme></voice> </speak>
  ```

4. Volume wijzigen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody volume="+20.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

5. Breedte wijzigen

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Welcome to use <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

6. Wijziging prosody contour

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody contour="(80%,+20%) (90%,+30%)" >Good morning.</prosody></voice> </speak>
  ```

> [!NOTE]
> Houd er rekening mee de audiogegevens is 8 kB of 16 kB wav wordt ingediend in de volgende indeling: **CRC code** (CRC-32): 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0xFFFFFFFF; **Audio-indeling vlag**: 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0xFFFFFFFF; **Aantal**: 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0x7FFFFFFF; **Grootte van de hoofdtekst van de binaire**: 4 bytes (DWORD) met het geldige bereik 0x00000000 ~ 0x7FFFFFFF; **Binaire hoofdtekst**: n bytes.

## <a name="SampleApp"></a>Voorbeeld van een toepassing

Zie voor meer informatie voor implementatie, de [Visual C# .NET-naar-spraak-voorbeeldtoepassing](https://github.com/Microsoft/Cognitive-Speech-TTS/blob/master/Samples-Http/CSharp/TTSProgram.cs).

## <a name="SupLocales"></a>Ondersteunde talen en spraakstijlen

De volgende tabel bevat enkele van de ondersteunde landinstellingen en gerelateerde spraakstijlen.

Landinstelling | Geslacht | De toewijzing van service
---------|--------|------------
ar bijvoorbeeld * | Vrouw | "Microsoft Server spraak tekst en spraak spraak (ar-bijvoorbeeld Hoda)"
ar-SA | Man | "Microsoft Server spraak tekst en spraak, spraak (ar-SA, Naayf)"
bg-BG | Man | "Microsoft Server tekst naar spraak stem (bg-BG, Ivan)"
CA-ES | Vrouw | "Microsoft Server tekst naar spraak stem (ca-ES, HerenaRUS)"
cs-CZ | Man | "Microsoft Server spraak tekst en spraak spraak (cs-CZ, Jakub)"
da-DK | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (da-DK, HelleRUS)"
de-AT | Man | "Microsoft Server spraak tekst en spraak, spraak (de-AT, Michael)"
de CH | Man | "Microsoft Server spraak tekst en spraak, spraak (de-h, Karsten)"
de-DE | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, Hedda)"
de-DE | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, HeddaRUS)"
de-DE | Man | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, Stefan, Apollo)"
el GR | Man | "Microsoft Server spraak tekst en spraak, spraak (el-GR, Stefanos)"
en-AU | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-AU, Catherine)"
en-AU | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-AU, HayleyRUS)"
NL-CA | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-CA, Linda)"
NL-CA | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-CA, HeatherRUS)"
en-GB | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-GB, Susan, Apollo)"
en-GB | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-GB, HazelRUS)"
en-GB | Man | "Microsoft Server spraak tekst en spraak, spraak (en-GB, George, Apollo)"
NL-Internet Explorer | Man | "Microsoft Server spraak tekst en spraak, spraak (en Internet Explorer, Jan)"
NL-IN | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-IN, Heera, Apollo)"
NL-IN | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-IN, PriyaRUS)"
NL-IN | Man | "Microsoft Server spraak tekst en spraak, spraak (en-IN, Ravi, Apollo)"
en-US | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, ZiraRUS)"
en-US | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, JessaRUS)"
en-US | Man | "Microsoft Server spraak tekst en spraak, spraak (en-US, BenjaminRUS)"
es-ES | Vrouw | "Microsoft Server tekst naar spraak stem (es-ES, Laura, Apollo)"
es-ES | Vrouw | "Microsoft Server tekst naar spraak stem (es-ES, HelenaRUS)"
es-ES | Man | "Microsoft Server tekst naar spraak stem (es-ES, Pablo, Apollo)"
es-MX | Vrouw | "Microsoft Server-stem tekst naar spraak (es-MX, HildaRUS)"
es-MX | Man | "Microsoft Server-stem tekst naar spraak (es-MX, Raul, Apollo)"
fi-FI | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (fi-FI, HeidiRUS)"
fr-CA | Vrouw | "Microsoft Server tekst naar spraak stem (fr-CA, Caroline)"
fr-CA | Vrouw | "Microsoft Server tekst naar spraak stem (fr-CA, HarmonieRUS)"
FR-h | Man | "Microsoft Server tekst naar spraak stem (fr-h, Guillaume)"
fr-FR | Vrouw | "Microsoft Server tekst naar spraak stem (fr-FR, Julia, Apollo)"
fr-FR | Vrouw | "Microsoft Server tekst naar spraak stem (fr-FR, HortenseRUS)"
fr-FR | Man | "Microsoft Server tekst naar spraak stem (fr-FR, Paul, Apollo)"
hij IL| Man| "Microsoft Server spraak tekst en spraak, spraak (hij-IL, Asaf)"
hi-IN | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Kalpana, Apollo)"
hi-IN | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Kalpana)"
hi-IN | Man | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Hemant)"
hr-HR | Man | "Microsoft Server tekst naar spraak stem (hr-HR, Matej)"
hu-HU | Man | "Microsoft Server spraak tekst en spraak, spraak (hu-HU, Szabolcs)"
id-ID | Man | "Microsoft Server spraak tekst en spraak, spraak (id-ID, Andika)"
IT-IT | Man | "Microsoft Server spraak tekst en spraak, spraak (it-IT, Cosimo, Apollo)"
IT-IT | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (it-IT, LuciaRUS)"
ja-JP | Vrouw | "Microsoft Server tekst naar spraak stem (ja-JP, Ayumi, Apollo)"
ja-JP | Man | "Microsoft Server tekst naar spraak stem (ja-JP, Ichiro, Apollo)"
ja-JP | Vrouw | "Microsoft Server tekst naar spraak stem (ja-JP, HarukaRUS)"
ko-KR | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (ko-KR, HeamiRUS)"
ms-MY | Man | "Microsoft Server-stem tekst naar spraak (ms-mijn Rizwan)"
nb-NO | Vrouw | "Microsoft Server-stem tekst naar spraak (nb-NO HuldaRUS)"
NL-NL | Vrouw | "Microsoft Server tekst naar spraak stem (nl-NL, HannaRUS)"
pl-PL | Vrouw | "Microsoft Server tekst naar spraak stem (pl-PL, PaulinaRUS)"
pt-BR | Vrouw | "Microsoft Server tekst naar spraak stem (pt-BR, HeloisaRUS)"
pt-BR | Man | "Microsoft Server tekst naar spraak stem (pt-BR, Daniel, Apollo)"
pt-PT | Vrouw | "Microsoft Server tekst naar spraak stem (pt-PT, HeliaRUS)"
ro-RO | Man | "Microsoft Server spraak tekst en spraak, spraak (ro-RO, Andrei)"
ru-RU | Vrouw | "Microsoft Server tekst naar spraak stem (ru-RU, Irina, Apollo)"
ru-RU | Man | "Microsoft Server tekst naar spraak stem (ru-RU, Pavel, Apollo)"
ru-RU | Vrouw | "Microsoft Server tekst naar spraak stem (ru-RU, EkaterinaRUS)"
sk-SK | Man | "Microsoft Server tekst naar spraak stem (sk-SK, Filip)"
sl-SI | Man | "Microsoft Server tekst naar spraak stem (sl-SI, Lado)"
SV-SE | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (sv-SE, HedvigRUS)"
TA-IN | Man | "Microsoft Server spraak tekst en spraak, spraak (ta-IN, Valluvar)"
e-e | Man | "Microsoft Server tekst naar spraak stem (th-TH, Pattara)"
tr-TR | Vrouw | "Microsoft Server tekst naar spraak stem (tr-TR, SedaRUS)"
vi-VN | Man | "Microsoft Server-stem tekst naar spraak (vi-VN, een)"
zh-CN | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, HuihuiRUS)"
zh-CN | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, Yaoyao, Apollo)"
zh-CN | Man | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, Kangkang, Apollo)"
zh-HK | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-HK Tracy, Apollo)"
zh-HK | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-HK TracyRUS)"
zh-HK | Man | "Microsoft Server spraak tekst en spraak, spraak (zh-HK Danny, Apollo)"
zh-TW | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, Yating, Apollo)"
zh-TW | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, HanHanRUS)"
zh-TW | Man | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, Zhiwei, Apollo)"
 * ar-bijvoorbeeld ondersteunt moderne Standard Arabisch (MSA).

> [!NOTE]
> Houd er rekening mee dat de vorige servicenamen **Microsoft Server spraak tekst en spraak spraak (cs-CZ, Vit)** en **Microsoft Server spraak tekst en spraak, spraak (en Internet Explorer, Shaun)** na 3/31 januari 2018 wordt afgeschaft de volgorde voor het optimaliseren van de Bing Speech-API-mogelijkheden. Werk uw code met de bijgewerkte namen.

## <a name="TrouNSupport"></a>Problemen oplossen en ondersteuning

Plaats alle vragen en problemen met de [Bing Speech-Service](https://social.msdn.microsoft.com/Forums/en-US/home?forum=SpeechService) MSDN-forum. Meer informatie, zoals zijn onder andere:

* Een voorbeeld van de volledige aanvraag-tekenreeks.
* Indien van toepassing, meld u met de volledige uitvoer van een mislukte aanvragen, waaronder id's.
* Het percentage aanvragen dat mislukken.
