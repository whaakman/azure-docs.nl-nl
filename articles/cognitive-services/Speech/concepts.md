---
title: Concepten | Microsoft Docs
description: Basisconcepten gebruikt in Microsoft spraak Service.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: bc23f4fb7dfc045a0f8cc87155c31875c4de8450
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344716"
---
# <a name="basic-concepts"></a>Basisbegrippen

Deze pagina beschrijft enkele basisconcepten in Microsoft speech recognition service. We raden u aan deze pagina te lezen voordat u Microsoft-spraakherkenning API in uw toepassing.

## <a name="understanding-speech-recognition"></a>Spraakherkenning begrijpen

Als dit de eerste keer dat u een spraak-toepassing maakt, of als de eerste keer dat u spraakmogelijkheden aan een bestaande toepassing toevoegt is, wordt in deze sectie helpt u aan de slag. Als u al enige ervaring met spraak-toepassingen, kunt u alleen in deze sectie stapels of kunt u deze overslaan volledig als u een oude hand Speech bent en u wilt direct aan de protocoldetails.

### <a name="audio-streams"></a>Audio-stromen

Belangrijkste de basisconcepten van spraak is de *audiostroom*. In tegenstelling tot een toetsaanslag die plaatsvindt op één punt in tijd en bevat een stukje informatie, een gesproken aanvraag is verspreid over honderden milliseconden en veel kB aan gegevens bevat. De duur van gesproken utterances geeft een aantal problemen voor ontwikkelaars willen een gestroomlijnde en elegante spraak-ervaring te bieden voor de toepassing. Vandaag de dag computers en -algoritmen uitvoeren, spraak schrijffouten in ongeveer helft van de duur van de utterance zodat een utterance 2 seconden kan worden omgezet in ongeveer 1 seconde, maar alle die optreedt in een 1 seconde vertraging bij de verwerking van gebruiker toepassingen gestroomlijnde noch elegante.

Gelukkig zijn er manieren 'verbergen"de tijd schrijffouten door schrijffouten uitvoeren op een deel van de utterance terwijl de gebruiker is een ander deel spreken. Bijvoorbeeld, is een 1 seconde utterance in 10 segmenten van 100 milliseconden splitsen en schrijffouten op zijn beurt uitvoeren op elk segment, dan 450 van de totale 500 milliseconden vereist voor schrijffouten kan worden 'verborgen' zodat de gebruiker niet op de hoogte is schrijffouten wordt uitgevoerd terwijl hij is spreken. Bij het nadenken over dit voorbeeld, houd er rekening mee dat de service schrijffouten is uitvoeren op de vorige 100 milliseconden van audio terwijl de gebruiker is de volgende 100 spreken, dus wanneer de gebruiker stopt spreken de service slechts hoeven transcriberen ongeveer 100 milliseconden van audio om een resultaat te produceren.

Zodat deze gebruikerservaring gesproken audio-informatie worden verzameld in segmenten en als de gebruiker spreekt omgezet. Deze segmenten audio gezamenlijk van de *audiostroom*, en de naam van het proces van het verzenden van deze audio segmenten naar de service *audio streaming.* Audio-streaming is een belangrijk onderdeel van een toepassing met spraak; de chunkgrootte afstemmen en optimaliseren van de streaming-implementatie zijn enkele van de meest indrukwekkende manieren van uw toepassing gebruikerservaring te verbeteren.

### <a name="microphones"></a>Een microfoon

Mensen gesproken audio met behulp van hun oren verwerken, maar levenloze hardware gebruikt een microfoon. Zodat spraak in elke toepassing die u wilt integreren met de microfoon de audiostroom bieden voor uw toepassing.

De API's voor de microfoon moet kunt u starten en stoppen audio bytes ontvangt van de microfoon. U moet bepalen door welke gebruikersacties trigger de microfoon beginnen met luisteren voor spraak. U kunt een druk op de knop activeren van het begin van het luisteren, of u kan kiezen voor een *trefwoord* of *wake word* spotter luisteren altijd de microfoon en de uitvoer van deze module die u wilt gebruiken trigger audio verzenden naar de Service Microsoft spraak.

### <a name="end-of-speech"></a>Einde van spraak

Detecteren van *wanneer* heeft een spreker *gestopt* spreken lijkt eenvoudige genoeg voor mensen maar is een probleem in plaats daarvan moeilijk buiten laboratorium. Het is niet voldoende gewoon zoekt pure stilte na een utterance omdat er vaak een groot aantal ambient ruis op dingen bemoeilijken. De Service Microsoft spraak legt snel detecteren of een gebruiker is gestopt spreken en de service kan informeren over uw toepassing van dit feit, maar deze regeling betekent dat uw toepassing de laatste is weten wanneer de gebruiker stoppen spreken. Dit is niet op alle zoals andere vormen van invoer waar de toepassing is de *eerste* weten wanneer de gebruiker begint de invoer *en* eindigt.

### <a name="asynchronous-service-responses"></a>Service voor asynchrone antwoorden

Het feit dat uw toepassing wordt geïnformeerd over de invoer van de gebruiker is voltooid moet niet opleggen eventuele prestatiestraffen of programming moeilijkheden op uw toepassing, maar deze is vereist dat u over spraak aanvragen anders van de invoeraanvraag nadenkt / antwoord patronen waarmee u bekend bent. Aangezien uw toepassing weet niet wanneer de gebruiker stopt spreken, moet uw toepassing blijven stream geluid om de service bij het tegelijkertijd en asynchroon wachten op een reactie van de service. Dit patroon is in tegenstelling tot andere aanvraag/antwoord-web-protocollen zoals HTTP. In deze protocollen, moet u een aanvraag uitvoeren voordat u kunt geen antwoord; ontvangen in het protocol Microsoft spraak Service, ontvangt u antwoorden *terwijl u nog steeds streaming audio voor de aanvraag*.

> [!NOTE]
> Deze functie wordt niet ondersteund bij gebruik van spraak HTTP REST-API.

### <a name="turns"></a>Hiermee schakelt u

Spraak is een carrier van informatie. Wanneer u spreekt probeert u om informatie die in uw bezit naar iemand die naar die informatie luistert over te brengen. Bij het overbrengen van informatie om u doorgaans de beurt spreken en te hebben geluisterd. Ook kan communiceert uw spraak toepassing met gebruikers door ook luisteren en reageren, hoewel uw toepassing meestal de meeste van de luisteren heeft. Gesproken invoer van de gebruiker en het antwoord van de service op deze invoer heet een *schakelen*. Een *schakelen* wordt gestart wanneer de gebruiker spreekt en eindigt wanneer uw toepassing de verwerking van het antwoord van de spraak-service is voltooid.

### <a name="telemetry"></a>Telemetrie

Maken van een spraak-apparaat of de toepassing kan lastig zijn, zelfs voor ervaren ontwikkelaars. Stroom-protocollen lijken vaak op het eerste gezicht uitdaging en belangrijke informatie zoals Stiltedetectie mogelijk volledig nieuw. Met zoveel berichten die met succes worden verzonden en ontvangen een combinatie van één aanvraag/antwoord voltooid, is het *zeer* belangrijk voor het verzamelen van volledige en nauwkeurige gegevens over deze berichten. Het protocol Microsoft spraak Service biedt voor het verzamelen van deze gegevens. Moet u alles in het werk op te geven van de vereiste gegevens zo nauwkeurig mogelijk; volledige en nauwkeurige gegevens levert, u zal worden helpt zelf--u ooit moet help van het Microsoft-accountteam spraak Service bij het oplossen van uw clientimplementatie, de kwaliteit van de telemetriegegevens die u hebt verzameld zijn essentieel voor een probleem analyse.

> [!NOTE]
> Deze functie wordt niet ondersteund bij gebruik van spraakherkenning REST-API.

### <a name="speech-application-states"></a>Spraak toepassingsstatussen

De stappen om in te schakelen spraakinvoer in uw toepassing zijn enigszins afwijken van de stappen voor andere vormen van invoer zoals muisklikken of vinger op tikt. U moet van bijhouden wanneer uw toepassing wordt geluisterd naar de microfoon en verzenden van gegevens naar de service spraak wanneer er wordt gewacht tot een reactie van de service en wanneer deze zich in een niet-actieve status. De relatie tussen deze status wordt weergegeven in het onderstaande diagram.

![Toepassingsstatus voor spraak-Diagram](Images/speech-application-state-diagram.png)

Omdat de Service Microsoft spraak deel van een aantal van de statussen uitmaakt, betekent dit dat het serviceprotocol berichten, zodat uw toepassing overgang tussen statussen bepaalt. Uw toepassing moet interpreteren en worden toegepast op deze protocolberichten bijhouden en beheren van de statussen van de toepassing spraak.

## <a name="using-the-speech-recognition-service-from-your-apps"></a>Gebruik van de service spraak herkenning van uw apps

Microsoft speech recognition service biedt twee manieren voor ontwikkelaars spraak toevoegen aan hun apps.

- [REST API's](GetStarted/GetStartedREST.md): ontwikkelaars kunnen gebruikmaken van HTTP-aanroepen vanuit hun apps naar de service voor spraakherkenning.
- [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md): voor geavanceerde functies ontwikkelaars kunnen Microsoft Speech clientbibliotheken downloaden, en in hun apps koppelen.  De clientbibliotheken zijn beschikbaar op verschillende platforms (Android, Windows, iOS) met behulp van verschillende talen (C#, Java, JavaScript, ObjectiveC).

| Gebruiksvoorbeelden | [REST API's](GetStarted/GetStartedREST.md) | [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Converteert een kort gesproken audio opdrachten bijvoorbeeld (audio lengte < 15 s) zonder tussentijdse resultaten | Ja | Ja |
| Converteren van een lang (> 15 s)-audio | Nee | Ja |
| Stroom audio met tussentijdse resultaten gewenst | Nee | Ja |
| De tekst geconverteerd van audio met LUIS begrijpen | Nee | Ja |

 Als uw taal of platform nog geen een SDK, kunt u uw eigen implementatie op basis van de [protocol documentatie](API-Reference-REST/websocketprotocol.md).

## <a name="recognition-modes"></a>Opname-modi

Er zijn drie beschikbare modi van de erkenning: `interactive`, `conversation`, en `dictation`. De opname-modus wordt aangepast op basis van hoe de gebruikers zijn waarschijnlijk uitspreken spraakherkenning. Kies de juiste erkenning modus voor uw toepassing.

> [!NOTE]
> Herkenning modi mogelijk andere gedrag in de [REST protocol](#rest-speech-recognition-api) dan in de [WebSocket-protocol](#webSocket-speech-recognition-api). Bijvoorbeeld, ondersteunt de REST-API geen continue opname, zelfs in de conversatie of dictation modus.
> [!NOTE]
> Deze modi zijn van toepassing wanneer u rechtstreeks de REST- of WebSocket-protocol gebruikt. De [clientbibliotheken](GetStarted/GetStartedClientLibraries.md) verschillende parameters gebruiken om op te geven voor de modus. Zie voor meer informatie de clientbibliotheek van uw keuze.

De Service Microsoft spraak resultaat wordt slechts één erkenning woordgroep voor alle opname-modi. Er is een limiet van 15 seconden voor een enkele utterance.

### <a name="interactive-mode"></a>Interactieve modus

In `interactive` modus, een gebruiker maakt korte aanvragen en de toepassing een actie uitvoert in het antwoord verwacht.

De volgende kenmerken zijn typische van interactieve modus toepassingen:

- Gebruikers weten dat ze zijn op een machine en niet op een andere human spreken.
- Gebruikers van de toepassing van tevoren weet wat ze graag willen weten op basis van wat ze willen de toepassing te doen.
- Utterances laatste doorgaans over 2-3 seconden.

### <a name="conversation-mode"></a>Conversatiemodus

In `conversation` modus, gebruikers bezig zijn met een conversatie human naar human.

De volgende kenmerken zijn kenmerkend voor toepassingen met servermodus conversatie:

- Gebruikers weten dat ze naar iemand praten.
- Spraakherkenning verbetert de menselijke conversaties doordat een of beide deelnemers de gesproken tekst wilt zien.
- Gebruikers niet altijd van plan bent ze willen in te spreken.
- Gebruikers gebruiken vaak taalgebruik en andere informele spraak.

### <a name="dictation-mode"></a>De modus Dicteren

In `dictation` modus, gebruikers wordt voorgelezen langer utterances tot de toepassing voor verdere verwerking.

De volgende kenmerken zijn typische dicteren modus toepassingen:

- Gebruikers weten dat ze communiceren met een machine communiceren.
- De tekst van spraak herkenningsresultaten gebruikers weergegeven.
- Gebruikers plant vaak ze willen spreken en meer formele taal gebruiken.
- Gebruikers werknemers volledige zinnen die laatste 5-8 seconden.

> [!NOTE]
> De Service Microsoft spraak wordt spraakherkenning en conversatie modi, geen gedeeltelijke resultaten geretourneerd. In plaats daarvan stuurt de service stabiel woordgroep resultaten na stilte grenzen in de audiostroom. Microsoft mogelijk om het protocol spraak ter verbetering van de gebruikerservaring in deze modi continue herkenning te verbeteren.

## <a name="recognition-languages"></a>Welke talen

De *taal* bepaalt de taal die de gebruiker van uw toepassing spreekt. Geef de *taal* met de *taal* URL-queryparameter op de verbinding. De waarde van de *taal* query parameter maakt gebruik van de IETF taal tag [BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag), en **moet** een van de talen die worden ondersteund door spraakherkenning API. De volledige lijst met talen die worden ondersteund door de Service spraak vindt u in de pagina [ondersteunde talen](API-Reference-REST/supportedlanguages.md).

De Service Microsoft spraak weigert ongeldige verbindingsaanvragen door weer te geven een `HTTP 400 Bad Request` antwoord. Een ongeldige aanvraag is een die:

- Omvat geen een *taal* query parameterwaarde.
- Bevat een *taal* queryparameter die heeft een onjuiste indeling.
- Bevat een *taal* queryparameter die niet een van de talen voor ondersteuning.

U kunt een toepassing bouwt die ondersteuning biedt voor een of meer van de talen die worden ondersteund door de service.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt een toepassing gebruikt *conversatie* speech recognition modus voor een Amerikaans Engels spreker.

```HTTP
https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

## <a name="transcription-responses"></a>Schrijffouten antwoorden

De antwoorden schrijffouten wordt de geconverteerde tekst van audio aan clients geretourneerd. Schrijffouten antwoord bevat de volgende velden:

- `RecognitionStatus` Hiermee geeft u de status van de erkenning. De mogelijke waarden zijn opgegeven in de onderstaande tabel.

| Status | Beschrijving |
| ------------- | ---------------- |
| Geslaagd | De erkenning is voltooid en het veld weergavetekst aanwezig is |
| NoMatch | Spraak is gedetecteerd in de audiostroom, maar geen woorden van de taal van het doel zijn afgestemd. Zie [NoMatch erkenning Status(#nomatch-recognition-status) voor meer informatie  |
| InitialSilenceTimeout | Het begin van de audiostroom opgenomen alleen stilte en de time-out voor spraak-service |
| BabbleTimeout | Het begin van de audiostroom opgenomen alleen ruis en de time-out voor spraak-service |
| Fout | De opname-service heeft een interne fout aangetroffen en kan niet worden voortgezet |

- `DisplayText` Hiermee geeft u de herkende woordgroep nadat hoofdlettergebruik, leestekens en inverse-tekst-normalisatie zijn toegepast en taalgebruik heeft zijn gemaskeerd met sterretjes. De weergavetekst veld aanwezig is *alleen* als de `RecognitionStatus` veld heeft de waarde `Success`.

- `Offset` Geeft de verschuiving (in eenheden van 100 nanoseconden) waarmee de woordgroep wordt herkend, ten opzichte van het begin van de audiostroom.

- `Duration`Hiermee geeft u de duur (in eenheden van 100 nanoseconden) van deze zin spraak.

Een antwoord schrijffouten retourneert meer informatie, indien gewenst. Zie [uitvoerindeling](#output-format) voor het retourneren meer gedetailleerde uitvoer.

Microsoft spraak Service ondersteunt extra schrijffouten proces dat houdt het toevoegen van hoofdlettergebruik en leestekens, taalgebruik maskeren en normaliseren van tekst naar veelgebruikte formulieren. Bijvoorbeeld als een gebruiker spreekt een wachtwoordzin die wordt vertegenwoordigd door de woorden 'Help me herinneren zes iPhones aanschaffen', spraak-Services van Microsoft wordt geretourneerd van de transcribed tekst "Help me herinneren 6 iPhones aanschaffen." Het proces dat het woord 'zes' naar het nummer '6 converteert' heet *Inverse tekst normalisatie* (*ITN* kortweg).

### <a name="nomatch-recognition-status"></a>NoMatch erkenning status

Retourneert het antwoord schrijffouten `NoMatch` in `RecognitionStatus` wanneer de Service Microsoft spraak spraak detecteert in de audiostroom, maar kan niet overeenkomen met die stem de taal-grammatica wordt gebruikt voor de aanvraag. Bijvoorbeeld, een *NoMatch* situatie kan ontstaan als er een gebruiker iets in het Duits wanneer de herkenner Amerikaans Engels als de taal gesproken verwacht. Het Wave-patroon van de utterance, duidt dit op de aanwezigheid van menselijke spraak, maar geen van de woorden die overeenkomen met het Amerikaans Engels lexicon wordt gebruikt door de herkenner.

Een andere *NoMatch* probleem treedt op wanneer de opname-algoritme is niet een nauwkeurige overeenkomst voor de geluiden opgenomen in de audiostroom gevonden. Als dit probleem zich voordoet, de Service Microsoft spraakherkenning kan leiden tot *speech.hypothesis* berichten met *tekst hypothetische* maar heeft een *speech.phrase*bericht waarin de *RecognitionStatus* is *NoMatch*. Deze voorwaarde is normaal. u moet geen maken veronderstellingen over de nauwkeurigheid van de gegevens of de betrouwbaarheid van de tekst in de *speech.hypothesis* bericht. Bovendien u moet niet wordt ervan uitgegaan dat omdat de Service Microsoft spraak produceert *speech.hypothesis* -berichten dat de service kan voor het produceren van een *speech.phrase* message met  *RecognitionStatus* *geslaagd*.

## <a name="output-format"></a>De indeling van uitvoer

Microsoft spraak Service kan een aantal nettolading indelingen in schrijffouten antwoorden retourneren. Alle nettoladingen zijn JSON-structuren.

U kunt de resultaatindeling woordgroep beheren door te geven de `format` URL-queryparameter. Standaard stuurt de service `simple` resultaten.

| Indeling | Beschrijving |
|-----|-----|
| `simple` | Een vereenvoudigde woordgroep resultaat met de status van de erkenning en de herkende tekst in formulier weergeven. |
| `detailed` | Een herkenning status- en N beste lijst met woordgroep resultaten waarbij elk resultaat woordgroep alle vier erkenning formulieren en een score vertrouwen bevat. |

De `detailed` notatie bevat [N beste waarden](#n-best-values), naast `RecognitionStatus`, `Offset`, en `duration`, in het antwoord.

### <a name="n-best-values"></a>N beste waarden

Listeners, of menselijke of computer, kunnen nooit worden bepaalde hen horen *exact* wat is gesproken. Een listener kunt toewijzen een *kans* alleen voor een bepaalde interpretatie van een utterance. 

Onder normale omstandigheden tijdens het spreken naar andere personen met wie ze vaak communiceren hebben mensen een hoge mate van waarschijnlijkheid van de woorden die zijn gesproken herkent. Op basis van machine spraak listeners willen vergelijkbare niveaus van nauwkeurigheid en de juiste voorwaarden [zij pariteit met mensen bereiken](https://blogs.microsoft.com/next/2016/10/18/historic-achievement-microsoft-researchers-reach-human-parity-conversational-speech-recognition/#sm.001ykosqs14zte8qyxj2k9o28oz5v).

De algoritmen die worden gebruikt in spraakherkenning verkennen alternatieve interpretatie van een utterance als onderdeel van de normale verwerking. Normaal gesproken deze alternatieven verwijderd zodra het bewijs voor een enkele interpretatie overweldigend. In minder dan optimale omstandigheden echter de spraakherkenner is voltooid met een lijst van mogelijke interpretatie voor de alternatieve. De bovenste *N* alternatieven in deze lijst worden genoemd de *N beste lijst*. Elke alternatief is toegewezen een [vertrouwen score](#confidence). Vertrouwen scores tussen 0 en 1. Een score van 1 geeft het hoogste niveau van betrouwbaarheid. Een score van 0 vertegenwoordigt het laagste niveau van betrouwbaarheid.

> [!NOTE]
> Het aantal vermeldingen in de lijst N beste variëren tussen meerdere utterances. Het aantal vermeldingen kan variëren tussen meerdere erkenning van de *dezelfde* utterance. Deze afwijking wordt een natuurlijke en de verwachte resultaten van de probabilistische aard van de algoritme van de erkenning spraak.

Elk item dat is geretourneerd in de lijst N beste bevat

- `Confidence`, welke vertegenwoordigt de [vertrouwen scores](#confidence) van dit item.
- `Lexical`, dit is de [lexicale formulier](#lexical-form) van de herkende tekst.
- `ITN`, dit is de [ITN formulier](#itn-form) van de herkende tekst.
- `MaskedITN`, dit is de [gemaskeerd ITN formulier](#masked-itn-form) van de herkende tekst.
- `Display`, dit is de [formulier weergeven](#display-form) van de herkende tekst.

### Scores vertrouwen <a id="confidence"></a>

Vertrouwen scores zijn integraal is voor spraak erkenning systemen. De Service Microsoft spraak verkrijgt vertrouwen scores van een *vertrouwen classificatie*. De classificatie vertrouwen traint Microsoft ten opzichte van een set van functies die zijn ontworpen voor het bewaartemperatuur onderscheid tussen juiste en onjuiste herkenning. Vertrouwen scores worden voor afzonderlijke woorden en volledige utterances geëvalueerd.

Als u de scores vertrouwen die zijn geretourneerd door de service gebruiken wilt, worden op de hoogte van het volgende gedrag:

- Vertrouwen scores kunnen alleen binnen dezelfde erkenning modus en taal worden vergeleken. Vergelijk niet scores tussen verschillende talen of andere erkenning modi. Een score vertrouwen in de modus voor interactieve erkenning heeft bijvoorbeeld *geen* correlatie naar een score vertrouwen in de modus Dicteren.
- Vertrouwen scores op een beperkte set van utterances beste kunnen worden gebruikt. Er is natuurlijk grote mate van variabiliteit in de scores voor een groot aantal utterances.

Als u kiest voor een waarde van de score vertrouwen als een *drempelwaarde* op die uw toepassing fungeert spraakherkenning tot stand brengen van de drempelwaarden te gebruiken.

- Spraakherkenning niet uitvoeren op een representatieve steekproef van utterances voor uw toepassing.
- Verzamel de scores vertrouwen voor elke opname in de voorbeeldset.
- De drempelwaarde baseren op sommige percentiel van vertrouwen voor dat voorbeeld.

Er is geen waarde enkelvoudige drempelwaarde is geschikt voor alle toepassingen. Een score acceptabele vertrouwen voor een toepassing is mogelijk onjuist voor een andere toepassing.

### <a name="lexical-form"></a>Lexicale formulier

Het lexicale formulier is de herkende tekst precies hoe het is opgetreden in de utterance en zonder interpunctie of hoofdlettergebruik. Bijvoorbeeld, de lexicale vorm van het adres '1020 Enterprise manier' zou worden *tien twintig enterprise manier*, ervan uitgaande dat deze op die manier is gesproken. De lexicale vorm van de zin 'Help me herinneren 5 potloden aanschaffen' is *herinneren aanschaffen vijf potloden*.

Het lexicale formulier is het meest geschikt voor toepassingen die moet niet-standaard tekst normalisatie uitvoeren. Het formulier lexicale is ook geschikt voor toepassingen die niet-verwerkte erkenning woorden nodig.

Taalgebruik wordt nooit in het formulier lexicale gemaskeerd.

### <a name="itn-form"></a>ITN formulier

Tekst normalisatie is het proces van het converteren van de tekst van het ene naar een andere vorm van 'canonieke'. Bijvoorbeeld, het telefoonnummer '555-1212' kan worden geconverteerd naar de canonieke vorm *vijf vijf vijf één twee één twee*. *Inverse* tekst normalisatie (ITN) keert dit proces voor het converteren van de woorden ' vijf vijf vijf één twee één twee ' aan de omgekeerde canonieke vorm *555-1212*. De ITN vorm van een resultaat erkenning omvat geen hoofdlettergebruik of leestekens.

Het formulier ITN is het meest geschikt voor toepassingen die op de herkende tekst. Een toepassing die toestaat dat een gebruiker te spreken zoektermen en gebruikt vervolgens deze voorwaarden in een webquery zou bijvoorbeeld het formulier ITN gebruiken. Taalgebruik wordt nooit in het formulier ITN gemaskeerd. Als u wilt maskeren taalgebruik, gebruiken de *gemaskeerd ITN formulier*.

### <a name="masked-itn-form"></a>Gemaskeerd ITN formulier

Omdat taalgebruik natuurlijk deel van de taal gesproken uitmaakt, herkent de Service Microsoft spraak dergelijke woorden en woordgroepen wanneer ze worden gesproken. Taalgebruik kan echter niet geschikt is voor alle toepassingen, met name die toepassingen met een beperkte, niet volwassene publiek.

Het gemaskeerd ITN formulier geldt taalgebruik dat maskering weergegeven voor de inverse tekst normalisatieformulier. Als u wilt maskeren taalgebruik, stel de waarde van de waarde van de parameter taalgebruik te `masked`. Wanneer het taalgebruik wordt gemaskeerd, die worden herkend als onderdeel van de taal taalgebruik lexicon vervangen met een sterretje. Bijvoorbeeld: *herinneren aanschaffen 5 *** potloden*. De gemaskeerd ITN vorm van een resultaat erkenning omvat geen hoofdlettergebruik of leestekens.

> [!NOTE]
> Als de waarde van de queryparameter taalgebruik is ingesteld op `raw`, het gemaskeerd ITN formulier is hetzelfde als het formulier ITN. Taalgebruik is *niet* gemaskeerd.

### <a name="display-form"></a>Formulier weergeven

Interpunctie hoofdletters en kleine letters signaal waar te benadrukken waar kunt onderbreken, enzovoort, waardoor tekst gemakkelijker te begrijpen. Het formulier weergeven wordt leestekens hoofdletters en kleine letters toegevoegd aan herkenningsresultaten, zodat u het meest geschikte formulier voor toepassingen die de gesproken tekst wordt weergegeven.

Omdat het formulier weergeven de gemaskeerd ITN formulier breidt, kunt u de waarde van de parameter taalgebruik instellen op `masked` of `raw`. Als de waarde is ingesteld op `raw`, de herkenningsresultaten omvatten alle taalgebruik gesproken door de gebruiker. Als de waarde is ingesteld op `masked`, herkend als onderdeel van de taal taalgebruik lexicon vervangen met een sterretje.

### <a name="sample-responses"></a>Voorbeeld antwoorden

Alle nettoladingen zijn JSON-structuren.

De indeling van de nettolading van de `simple` woorden resultaat:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

De indeling van de nettolading van de `detailed` woorden resultaat:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="profanity-handling-in-speech-recognition"></a>Taalgebruik afhandeling van spraakherkenning

De Service Microsoft spraak herkent alle vormen van menselijke spraak, met inbegrip van woorden en woordgroepen die veel mensen zouden classificeren als "taalgebruik." U kunt bepalen hoe de service taalgebruik verwerkt met behulp van de *taalgebruik* queryparameter. Standaard maskeert de service taalgebruik in *speech.phrase* resulteert en retourneert geen *speech.hypothesis* berichten die taalgebruik bevatten.

| *Taalgebruik* waarde | Beschrijving |
| - | - |
| `masked` | Maskers taalgebruik met een sterretje. Dit gedrag is de standaardinstelling. | 
| `removed` | Hiermee verwijdert u taalgebruik uit alle resultaten. |
| `raw` | Herkent en taalgebruik in alle resultaten retourneert. |

### <a name="profanity-value-masked"></a>Taalgebruik waarde `Masked`

Taalgebruik maskeren, stelt u de *taalgebruik* queryparameter op de waarde *gemaskeerd*. Wanneer de *taalgebruik* queryparameter deze waarde heeft of niet is opgegeven voor een aanvraag, de service *maskers* taalgebruik. De service uitvoert maskering door taalgebruik in de herkenningsresultaten vervangen met een sterretje. Wanneer u taalgebruik maskering verwerking opgeeft, retourneert de service geen *speech.hypothesis* berichten die taalgebruik bevatten.

### <a name="profanity-value-removed"></a>Taalgebruik waarde `Removed`

Wanneer de *taalgebruik* query heeft de waarde van parameter *verwijderd*, de service verwijdert taalgebruik van zowel *speech.phrase* en *speech.hypothesis* berichten. De resultaten zijn hetzelfde *alsof de taalgebruik woorden zijn niet uitgesproken*.

#### <a name="profanity-only-utterances"></a>Utterances taalgebruik alleen-lezen

Een gebruiker mogelijk uitspreken *alleen* taalgebruik wanneer een toepassing heeft de service verwijderen taalgebruik hebt geconfigureerd. In dit scenario als de opname-modus is *dicteren* of *conversatie*, de service geen waarde retourneert een *speech.result*. Als u de opname-modus is *interactieve*, de service retourneert een *speech.result* met de statuscode *NoMatch*. 

### <a name="profanity-value-raw"></a>Taalgebruik waarde `Raw`

Wanneer de *taalgebruik* query heeft de waarde van parameter *onbewerkte*, de service niet verwijderen of maskeren taalgebruik in ofwel de *speech.phrase* of  *Speech.hypothesis* berichten.
