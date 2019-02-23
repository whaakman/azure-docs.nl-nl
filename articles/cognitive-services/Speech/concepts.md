---
title: Bing Speech-concepten | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Fundamentele concepten die worden gebruikt in Microsoft Speech-Service.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c114c726bea34465972a282acac6b8acbbf9a80f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670415"
---
# <a name="basic-concepts"></a>Basisbegrippen

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Deze pagina wordt enkele eenvoudige concepten in Microsoft spraakherkenningsservice beschreven. We raden u aan deze pagina te lezen voordat u Microsoft the spraakherkennings-API in uw toepassing.

## <a name="understanding-speech-recognition"></a>Informatie over spraakherkenning

Als dit de eerste keer dat u een spraak-toepassing maakt, of als het de eerste keer dat u spraakmogelijkheden aan een bestaande toepassing toevoegt is, wordt in deze sectie helpt u bij aan de slag. Als u al enige ervaring met toepassingen, kunt u besluiten alleen doornemen voordat deze sectie of u kunt deze volledig als u een oude volledig spraak en u meteen aan de protocoldetails van het wilt overslaan.

### <a name="audio-streams"></a>Audiostreams

Belangrijkste de basisconcepten van spraak is de *audiostream*. In tegenstelling tot een toetsaanslag, die wordt uitgevoerd op een bepaald tijdstip en bevat een los stukje van gegevens, een gesproken aanvraag is verspreid over honderden milliseconden en veel kB aan gegevens bevat. De duur van de gesproken uitingen geeft sommige problemen voor ontwikkelaars die willen biedt een gestroomlijnde en elegante spraak-ervaring voor de toepassing. Vandaag computers en -algoritmen uit te voeren spraaktranscriptie in ongeveer de helft van de duur van de utterance, zodat een utterance 2 seconden kan worden omgezet in ongeveer 1 seconde, maar alle die optreedt in een 1 seconde vertraging bij de verwerking van gebruiker toepassingen gestroomlijnde noch elegante.

Gelukkig zijn er manieren om 'verborgen' de transcriptie tijd door het uitvoeren van transcriptie op een deel van de utterance terwijl de gebruiker er spreekt door een ander deel. Bijvoorbeeld, is door het splitsen van een 1 seconde utterance in 10 segmenten van 100 milliseconden en door het uitvoeren van transcriptie op elke chunk op zijn beurt, meer dan 450 van de totale 500 milliseconden vereist voor transcriptie kan worden 'verborgen' zodat de gebruiker is geen informatie over transcriptie wordt uitgevoerd terwijl hij spreekt. Bij het nadenken over dit voorbeeld, houd er rekening mee dat de service transcriptie op de vorige 100 milliseconden van audio wordt uitgevoerd terwijl de gebruiker de volgende 100 spreekt, dus wanneer de gebruiker wordt gestopt spreken, de service alleen moet transcriberen ongeveer 100 milliseconden van audio een resultaat te produceren.

Voor het bereiken van deze gebruikerservaring gesproken audio gegevens worden verzameld in segmenten en getranscribeerde als de gebruiker spreekt. Deze audio segmenten gezamenlijk uit de *audiostream*, en het proces van het verzenden van deze audio segmenten met de service wordt aangeroepen *streamen van audio.* Audio streaming is een belangrijk onderdeel van een toepassing spraak ingeschakeld; de chunkgrootte afstemmen en optimaliseren van de streaming-implementatie, zijn sommige van de meeste impact hebben manieren verbetering van de gebruikerservaring van uw toepassing.

### <a name="microphones"></a>Microfoon

Mensen verwerking van gesproken audio met behulp van hun klinken, maar levenloze hardware microfoons worden gebruikt. Om in te schakelen gesproken tekst in een toepassing, die u wilt integreren met de microfoon de audiostream bieden voor uw toepassing.

De API's voor de microfoon moet kunt u starten en stoppen van audio bytes ontvangen van de microfoon. U moet bepalen welke acties van de gebruiker wordt geactiveerd de microfoon om te luisteren naar spraak starten. Kunt u besluiten om een druk op de knop activeren van het begin van het luisteren, of u wilt dat een *sleutelwoord* of *wake word* spotter altijd luisteren naar de microfoon en het gebruik van de uitvoer van deze module trigger audio verzenden naar de Microsoft Speech-Service.

### <a name="end-of-speech"></a>Einde van spraak

Detecteren van *wanneer* heeft een spreker *gestopt* spreken lijkt eenvoudig hebben maar is een probleem in plaats daarvan moeilijk buiten laboratorium-voorwaarden. Het is niet genoeg is om te zoeken naar pure stilte na een utterance, omdat er vaak een groot aantal ambient ruis te bemoeilijken dingen. De Microsoft Speech-Service biedt een uitstekende taak van snel detecteren wanneer een gebruiker is gestopt, spreken, en de service kan informeren over de toepassing van dit feit, maar deze rangschikking betekent dat uw toepassing de laatste is weten wanneer de gebruiker stoppen spreken. Dit is niet helemaal net als andere vormen van invoer wanneer uw toepassing is de *eerste* weten wanneer invoer van de gebruiker wordt gestart *en* eindigt.

### <a name="asynchronous-service-responses"></a>Service voor asynchrone antwoorden

Het feit dat de toepassing nodig heeft om te worden geïnformeerd over de invoer van de gebruiker is voltooid niet opleggen elke prestatiestraffen of programming problemen op uw toepassing, maar deze is vereist dat u van spraak aanvragen verschillend van de ingevoerde aanvraag vindt / antwoord patronen waarmee u bekend bent. Omdat uw toepassing niet kennen wanneer de gebruiker wordt gestopt spreken, moet uw toepassing doorgaan met het streamen van audio naar de service tijdens het tegelijkertijd en asynchroon wachten op een reactie van de service. Dit patroon is in tegenstelling tot andere aanvraag/antwoord-web-protocollen zoals HTTP. In deze protocollen, moet u een aanvraag voltooien voordat u ontvangt geen reactie; in het protocol van Microsoft Speech Service, ontvangt u antwoorden *terwijl u nog steeds van audio voor de aanvraag streamen*.

> [!NOTE]
> Deze functie wordt niet ondersteund bij het gebruik van spraak HTTP REST-API.

### <a name="turns"></a>Hiermee schakelt u

Spraak is een provider van gegevens. Wanneer u spreekt, probeert u om aanvullende informatie die in uw bezit naar iemand die naar die informatie luistert over te brengen. Bij het overbrengen van informatie om u doorgaans de beurt spreken en te hebben geluisterd. Ook kan communiceert uw toepassing spraak ingeschakelde met gebruikers door ook luisteren naar en reageren, hoewel uw toepassing meestal de meeste van de luisteren heeft. Gesproken invoer van de gebruiker en het antwoord van de service op deze invoer heet een *inschakelen*. Een *inschakelen* wordt gestart wanneer de gebruiker spreekt en eindigt wanneer uw toepassing de verwerking van het antwoord van de spraak-service is voltooid.

### <a name="telemetry"></a>Telemetrie

Het maken van een spraak-apparaat of de toepassing kan lastig zijn, zelfs voor ervaren ontwikkelaars. Stream-protocollen worden vaak uitdaging op het eerste gezicht en belangrijke informatie zoals Stiltedetectie mogelijk volledig nieuwe lijken. Met zo veel berichten die worden verzonden en ontvangen om uit te voeren van een paar één aanvraag/antwoord, is het *zeer* belangrijk voor het verzamelen van volledige en nauwkeurige gegevens over deze berichten. Het Microsoft Speech Service-protocol biedt voor het verzamelen van deze gegevens. U moet zich in om op te geven van de vereiste gegevens zo nauwkeurig mogelijk; door het opgeven van volledige en correcte gegevens u zal worden helpen zelf--als u ooit moet de hulp van het Team van Microsoft Speech-Service bij het oplossen van uw clientimplementatie, de kwaliteit van de telemetriegegevens die u hebt verzameld is van cruciaal belang voor probleem analyse.

> [!NOTE]
> Deze functie wordt niet ondersteund bij het gebruik van spraakherkenning REST-API.

### <a name="speech-application-states"></a>Spraak toepassing Staten

De stappen om in te schakelen spraakinvoer in uw toepassing zijn er enigszins anders uit dan de stappen voor andere soorten invoer zoals muisklikken of vinger tikt. U moet bijhouden van wanneer uw toepassing wordt geluisterd naar de microfoon en gegevens te verzenden naar de spraakservice wanneer er wordt gewacht tot een reactie van de service en wanneer deze zich in een niet-actieve status. De relatie tussen deze statussen wordt weergegeven in het onderstaande diagram.

![Toepassingsstatus voor spraak-Diagram](Images/speech-application-state-diagram.png)

Omdat de Service Microsoft Speech maakt deel uit van enkele van de Staten, definieert de serviceprotocol berichten, zodat uw toepassing overgang tussen Staten. Uw toepassing moet interpreteren en reageren op deze protocolberichten bijhouden en beheren van de statussen van de toepassing spraak.

## <a name="using-the-speech-recognition-service-from-your-apps"></a>Met behulp van de spraakherkenningsservice van uw apps

De spraakherkenningsservice Microsoft biedt twee manieren voor ontwikkelaars voor het toevoegen van de spraakherkenning aan hun apps.

- [REST-API's](GetStarted/GetStartedREST.md): Ontwikkelaars kunnen HTTP-aanroepen naar de service voor spraakherkenning van hun apps gebruiken.
- [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md): Voor geavanceerde functies, kunnen ontwikkelaars downloaden van Microsoft Speech-clientbibliotheken en koppel in hun apps.  De clientbibliotheken zijn beschikbaar op verschillende platformen (Windows, Android, iOS) met behulp van verschillende talen (C#, Java, JavaScript, ObjectiveC).

| Gebruiksvoorbeelden | [REST API's](GetStarted/GetStartedREST.md) | [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| (Audio lengte < 15 s) opdrachten converteren een korte gesproken audio, bijvoorbeeld zonder tussentijdse resultaten | Ja | Ja |
| Converteren van een lange audio (> 15 s) | Nee | Ja |
| Stream audio met tijdelijke resultaten gewenst | Nee | Ja |
| Inzicht in de tekst geconverteerd van audio gebruik maakt van LUIS | Nee | Ja |

 Als uw programmeertaal of platform heeft nog geen een SDK, kunt u uw eigen implementatie op basis van de [protocol documentatie](API-Reference-REST/websocketprotocol.md).

## <a name="recognition-modes"></a>Opname-modi

Er zijn drie modi van erkenning: `interactive`, `conversation`, en `dictation`. De opname-modus wordt aangepast op basis van hoe de gebruikers zijn waarschijnlijk zullen spreek spraakherkenning. Kies de juiste opname-modus voor uw toepassing.

> [!NOTE]
> Opname-modi mogelijk verschillend gedrag in de REST-protocol dan in de WebSocket-protocol. De REST-API biedt bijvoorbeeld geen ondersteuning voor doorlopende spraakherkenning, zelfs in de conversatie of dictation-modus.
> [!NOTE]
> Deze modi zijn van toepassing wanneer u rechtstreeks de REST- of WebSocket-protocol. De [clientbibliotheken](GetStarted/GetStartedClientLibraries.md) verschillende parameters gebruiken om op te geven van opname-modus. Zie voor meer informatie de clientbibliotheek van uw keuze.

De Service Microsoft Speech resultaat wordt slechts één erkenning woordgroep voor alle opname-modi. Er is een limiet van 15 seconden voor een enkele utterance.

### <a name="interactive-mode"></a>Interactieve modus

In `interactive` modus, een gebruiker maakt korte aanvragen en wordt verwacht dat de toepassing naar een actie uitvoert in de reactie.

De volgende kenmerken zijn typische van toepassingen interactieve modus:

- Gebruikers weten dat ze naar een virtuele machine en niet naar een andere human aanspreekt.
- Gebruikers van de toepassing van tevoren weet wat ze willen, en wel op basis van wat ze willen de toepassing te doen.
- Uitingen doorgaans over de laatste 2-3 seconden.

### <a name="conversation-mode"></a>Conversatiemodus

In `conversation` modus, gebruikers bezig zijn met een conversatie-mensen.

De volgende kenmerken zijn typische van toepassingen met servermodus conversatie:

- Gebruikers weten dat ze praten dan over naar een andere persoon.
- Spraakherkenning verbetert de menselijke conversaties doordat een of beide deelnemers aan de gesproken tekst wilt zien.
- Gebruikers niet altijd van plan bent ze wil instellen.
- Gebruikers gebruiken vaak slang en andere informele spraak.

### <a name="dictation-mode"></a>De modus voor spraakherkenning

In `dictation` modus, gebruikers voorgelezen langer uitingen tot de toepassing voor verdere verwerking.

De volgende kenmerken zijn typische van toepassingen met servermodus dicteren:

- Gebruikers weten dat ze praten dan over naar een virtuele machine.
- Gebruikers worden de resultaten van de spraakherkenning herkenning tekst weergegeven.
- Gebruikers plan vaak wat ze willen zeggen en meer formele taal gebruiken.
- Gebruikers werknemers volledige zinnen die afgelopen 5-8 seconden.

> [!NOTE]
> In de modi Dicteren en conversatie retourneert de Speech-Service van Microsoft geen gedeeltelijke resultaten. In plaats daarvan retourneert de service na stilte grenzen in de audiostream stabiel woordgroep resultaten. Microsoft kan de spraak-protocol ter verbetering van de gebruikerservaring in deze modi continue erkenning verbeteren.

## <a name="recognition-languages"></a>Van herkenningstalen

De *taal* Hiermee geeft u de taal die de gebruiker van uw toepassing spreekt. Geef de *taal* met de *taal* queryparameter URL voor de verbinding. De waarde van de *taal* query parameter maakt gebruik van de IETF-taalcode [BCP-47](https://en.wikipedia.org/wiki/IETF_language_tag), en **moet** een van de talen die worden ondersteund door de the spraakherkennings-API. De volledige lijst met talen die worden ondersteund door de Speech-Service kan worden gevonden op de pagina [talen ondersteund](API-Reference-REST/supportedlanguages.md).

De Service Microsoft Speech ongeldige verbindingsaanvragen geweigerd door weer te geven een `HTTP 400 Bad Request` antwoord. Een ongeldige aanvraag is een die:

- Bevat geen een *taal* query parameterwaarde.
- Bevat een *taal* queryparameter die heeft een onjuiste indeling.
- Bevat een *taal* queryparameter die niet een van de ondersteuning voor talen.

U kunt kiezen om een toepassing die ondersteuning biedt voor een of meer van de talen die worden ondersteund door de service te maken.

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt een toepassing gebruikt *conversatie* spraakherkenning van modus voor een Amerikaanse Engelstalige persoon.

```HTTP
https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

## <a name="transcription-responses"></a>Transcriptie antwoorden

De antwoorden transcriptie retourneert de geconverteerde tekst van audio naar clients. Een antwoord transcriptie bevat de volgende velden:

- `RecognitionStatus` Hiermee geeft u de status van de opname. De mogelijke waarden zijn opgegeven in de onderstaande tabel.

| Status | Description |
| ------------- | ---------------- |
| Geslaagd | De opname is voltooid en het veld weergavetekst aanwezig is |
| NoMatch | Spraak is gedetecteerd in de audiostream, maar er zijn geen woorden uit de doeltaal zijn afgestemd. Zie [NoMatch erkenning Status(#nomatch-recognition-status) voor meer informatie  |
| InitialSilenceTimeout | Het begin van de audiostream bevat alleen stilte en de time-out voor spraak-service |
| BabbleTimeout | Het begin van de audiostream bevat alleen ruis, en de time-out voor spraak-service |
| Fout | De opname-service is een interne fout opgetreden en kan niet worden voortgezet |

- `DisplayText` Hiermee geeft u de herkende woordgroep nadat hoofdletters, interpunctie en inverse-tekst-normalisering zijn toegepast en grof taalgebruik heeft met een sterretje zijn gemaskeerd. Het veld weergavetekst aanwezig is *alleen* als de `RecognitionStatus` veld heeft de waarde `Success`.

- `Offset` Geeft de verschuiving (in eenheden van 100 nanoseconden) waarmee de woordgroep wordt herkend, ten opzichte van het begin van de audio-stream.

- `Duration`Hiermee geeft u de duur (in eenheden van 100 nanoseconden) van deze zin spraak.

Een antwoord transcriptie retourneert meer informatie, indien gewenst. Zie [uitvoerindeling](#output-format) voor hoe meer gedetailleerde uitvoer worden geretourneerd.

Microsoft Speech-Service ondersteunt aanvullende transcriptie proces dat houdt het toevoegen van hoofdletters en leestekens, grof taalgebruik maskeren en normaliseren van tekst die moet worden veelgebruikte formulieren. Bijvoorbeeld, als een gebruiker spreekt een wachtwoordzin die wordt vertegenwoordigd door de woorden 'Help me herinneren aan te schaffen zes iPhones", van Microsoft Speech Services retourneert de getranscribeerde tekst"Herinner me aan te schaffen 6 iPhones." Het proces dat het woord '6' wordt omgezet in het nummer '6' heet *Inverse tekst normalisering* (*toevoegen* kortweg).

### <a name="nomatch-recognition-status"></a>NoMatch herkenning van status

Retourneert het antwoord transcriptie `NoMatch` in `RecognitionStatus` wanneer de Speech-Service van Microsoft spraak detecteert in de audiostream hebben, maar kan niet overeenkomen met die spraak-naar de taal-grammatica wordt gebruikt voor de aanvraag. Bijvoorbeeld, een *NoMatch* probleem kan zich voordoen als een gebruiker aangeeft er iets in het Duits dat wanneer de herkenning wordt verwacht Amerikaans Engels als de gesproken taal dat. Het patroon golf van de utterance, duidt dit op de aanwezigheid van menselijke spraak, maar geen van de gesproken woorden wordt gezocht naar de Engelse woordenlijst die wordt gebruikt door de herkenning van VS.

Een andere *NoMatch* probleem treedt op wanneer de opname-algoritme is niet een nauwkeurige overeenkomst voor de geluiden die deel uitmaken van de audiostream gevonden. Als dit probleem gebeurt, de Microsoft Speech-Service kan leiden tot *speech.hypothesis* berichten met *hypothetische tekst* maar produceert een *speech.phrase*bericht waarin de *RecognitionStatus* is *NoMatch*. Deze situatie doet zich normaal. u moet niet geen aannames doen over de nauwkeurigheid van de gegevens of de kwaliteit van de tekst in de *speech.hypothesis* bericht. Daarnaast kunt u moet niet wordt ervan uitgegaan dat omdat de Service Microsoft Speech produceert *speech.hypothesis* berichten die de service kan voor het produceren van een *speech.phrase* message met  *RecognitionStatus* *succes*.

## <a name="output-format"></a>Uitvoerindeling

Microsoft Speech Service kunt tal van nettolading-indelingen in transcriptie antwoorden retourneren. Alle nettoladingen zijn JSON-structuren.

U kunt de woordgroep resultaatindeling beheren door op te geven de `format` URL-queryparameter. Standaard stuurt de service `simple` resultaten.

| Indeling | Description |
|-----|-----|
| `simple` | Een vereenvoudigde woordgroep resultaat met de status van de spraakherkenning en de herkende tekst in formulier weergeven. |
| `detailed` | Een opname-status en een lijst van de N-beste woordgroep resultaten waarbij elk resultaat woordgroep alle vier herkenning van formulieren en een betrouwbaarheidsscore bevat. |

De `detailed` indeling bevat [N-beste waarden](#n-best-values), naast `RecognitionStatus`, `Offset`, en `duration`, in het antwoord.

### <a name="n-best-values"></a>N-beste waarden

Listeners, of mens of machine, kunnen nooit worden bepaalde ze gehoord *precies* wat er is gezegd. Een listener kunt toewijzen een *kans* alleen voor een bepaalde interpretatie van een utterance.

In normale omstandigheden, als andere gebruikers met wie ze vaak communiceren, hebben personen een hoge waarschijnlijkheid herkent de woorden die gesproken zijn. Op basis van machine spraak listeners streven ernaar om vergelijkbare niveaus van nauwkeurigheid en de juiste voorwaarden [zij pariteit met mensen bereiken](https://blogs.microsoft.com/next/2016/10/18/historic-achievement-microsoft-researchers-reach-human-parity-conversational-speech-recognition/#sm.001ykosqs14zte8qyxj2k9o28oz5v).

De algoritmen die worden gebruikt in spraakherkenning verkennen alternatieve interpretaties van een utterance als onderdeel van de normale verwerking. Alternatieve mogelijkheden worden normaal gesproken genegeerd als de gegevens en vervangen door een enkele interpretatie overweldigend. In minder dan optimale omstandigheden, maar de spraakherkenning is voltooid met een lijst met alternatieve mogelijk interpretaties. De bovenste *N* alternatieven in deze lijst worden genoemd de *lijst met N-beste*. Elke alternatief is toegewezen een [betrouwbaarheidsscore](#confidence). Vertrouwen beoordeelt tussen 0 en 1 liggen. Een score van 1 vertegenwoordigt de hoogste mate van betrouwbaarheid. Een score van 0 geeft het laagste niveau van betrouwbaarheid.

> [!NOTE]
> Het aantal vermeldingen in de lijst met N-beste verschillen voor verschillende meerdere uitingen. Het aantal vermeldingen kan verschillen tussen meerdere erkenning van de *dezelfde* utterance. Deze variatie is een natuurlijke en de verwachte resultaten van de probabilistic aard van het algoritme van de herkenning van spraak.

Elk item dat is geretourneerd in de lijst met N-beste bevat

- `Confidence`, welke vertegenwoordigt de [vertrouwen scores](#confidence) van dit item.
- `Lexical`, dit is de [lexicale formulier](#lexical-form) van de herkende tekst.
- `ITN`, dit is de [toevoegen formulier](#itn-form) van de herkende tekst.
- `MaskedITN`, dit is de [gemaskeerd toevoegen formulier](#masked-itn-form) van de herkende tekst.
- `Display`, dit is de [weergaveformulier](#display-form) van de herkende tekst.

### Vertrouwen van de scores <a id="confidence"></a>

Vertrouwen van de scores zijn integraal is voor spraakherkenning systemen. De Microsoft Speech-Service verkrijgt vertrouwen scores van een *vertrouwen classificatie*. De classificatie vertrouwen traint Microsoft ten opzichte van een set van functies die zijn ontworpen om bewaartemperatuur onderscheid maken tussen de juiste en onjuiste herkenning. Vertrouwen van scores worden voor afzonderlijke woorden en volledige uitingen geëvalueerd.

Als u ervoor de scores vertrouwen die zijn geretourneerd door de service gebruiken kiest, rekening met het volgende gedrag:

- Vertrouwen van de scores kunnen alleen binnen de dezelfde opname-modus en de taal worden vergeleken. Vergelijk niet scores tussen verschillende talen of herkenning van verschillende modi. Een betrouwbaarheidsscore in de modus voor interactieve erkenning heeft bijvoorbeeld *geen* samenhang met een betrouwbaarheidsscore in de modus Dicteren.
- Scores die vertrouwen op een beperkte set uitingen beste kunnen worden gebruikt. Er is op een natuurlijke manier een grote mate van variabiliteit bij de scores voor een groot aantal uitingen.

Als u wilt gebruiken van een waarde van de score vertrouwen als een *drempelwaarde* op die uw toepassing fungeert, spraakherkenning tot stand brengen van de drempelwaarden te gebruiken.

- Spraakherkenning niet uitvoeren op een representatieve steekproef van uitingen voor uw toepassing.
- Verzamel de scores vertrouwen voor elke opname in de voorbeeldset.
- De waarde voor drempel baseren op sommige percentiel van vertrouwen voor dat voorbeeld.

Er zijn geen enkelvoudige drempelwaarde-waarde is geschikt voor alle toepassingen. Een acceptabele betrouwbaarheidsscore voor één toepassing mogelijk niet toegestaan voor een andere toepassing.

### <a name="lexical-form"></a>Lexicale formulier

Het lexicale formulier is de herkende tekst precies hoe het is opgetreden in de utterance en zonder interpunctie of hoofdlettergebruik. Bijvoorbeeld, de lexicale vorm van het adres "1020 Enterprise manier" zou worden *tien twintig enterprise manier*, ervan uitgaande dat het op deze manier is gesproken. De lexicale vorm van de zin 'Help me herinneren aan te schaffen 5 pennen"is *herinner me aan te schaffen vijf pennen*.

Het lexicale formulier is het meest geschikt is voor toepassingen die nodig zijn om uit te voeren van normalisering van niet-standaard tekst. Het lexicale formulier is ook geschikt voor toepassingen waarvoor herkenning van niet-verwerkte woorden.

Grof taalgebruik wordt nooit in het formulier lexicale gemaskeerd.

### <a name="itn-form"></a>Formulier toevoegen

Tekst normalisatie is het proces van het converteren van tekst van het ene naar een andere "canonieke" vorm. Bijvoorbeeld, het telefoonnummer '555-1212' kan worden geconverteerd naar de canonieke vorm *vijf vijf vijf één twee één twee*. *Inverse* tekst normalisering (toevoegen) keert dit proces voor het converteren van de woorden "vijf vijf vijf één twee één twee ' aan de omgekeerde canonieke vorm *555-1212*. De vorm van het toevoegen van een herkenningsresultaat bevat geen gebruik van hoofdletters of leestekens.

Het formulier toevoegen is het meest geschikt is voor toepassingen die op de herkende tekst. Bijvoorbeeld, een toepassing die kan een gebruiker te spreken zoektermen en vervolgens maakt gebruik van deze voorwaarden in een web-query gebruikt het formulier toevoegen. Grof taalgebruik wordt nooit gemaskeerd in het formulier toevoegen. U wilt maskeren grof taalgebruik, gebruikt de *gemaskeerd toevoegen formulier*.

### <a name="masked-itn-form"></a>Gemaskeerde toevoegen-formulier

Omdat grof taalgebruik op natuurlijke wijze deel uitmaakt van gesproken taal, herkent de Speech-Service van Microsoft dergelijke woorden en woordgroepen wanneer ze worden gesproken. Grof taalgebruik, maar mogelijk niet geschikt is voor alle toepassingen, met name deze toepassingen met een beperkte, niet-volwassene publiek.

Het gemaskeerd toevoegen-formulier is van toepassing grof taalgebruik maskeren voor de formulieren inverse tekst. Om te maskeren grof taalgebruik, stel de waarde van de waarde van de parameter grof taalgebruik in `masked`. Wanneer grof taalgebruik wordt gemaskeerd, worden de woorden die worden herkend als onderdeel van de taal van grof taalgebruik op woordenlijsten vervangen door sterretjes. Bijvoorbeeld: *herinner me aan te schaffen 5 *** pennen*. De gemaskeerd toevoegen vorm van een herkenningsresultaat bevat geen gebruik van hoofdletters of leestekens.

> [!NOTE]
> Als de waarde van de queryparameter grof taalgebruik is ingesteld op `raw`, het gemaskeerd toevoegen-formulier is hetzelfde als het formulier toevoegen. Grof taalgebruik is *niet* gemaskeerd.

### <a name="display-form"></a>Formulier weergeven

Leestekens hoofdletters en kleine letters signaal waar u kunt benadrukken waar kunt onderbreken, enzovoort, waardoor tekst gemakkelijker te begrijpen. De weergaveformulier voegt interpunctie hoofdletters en kleine letters toe aan herkenningsresultaten, zodat u de meest geschikte vorm voor toepassingen die de gesproken tekst weer te geven.

Omdat de weergaveformulier de gemaskeerd toevoegen-formulier breidt, kunt u de waarde van de parameter grof taalgebruik instellen op `masked` of `raw`. Als de waarde is ingesteld op `raw`, de resultaten zijn er ongepast taalgebruik gesproken door de gebruiker. Als de waarde is ingesteld op `masked`, herkend als onderdeel van de taal van grof taalgebruik op woordenlijsten woorden vervangen door sterretjes.

### <a name="sample-responses"></a>Voorbeeld van reacties

Alle nettoladingen zijn JSON-structuren.

De indeling van de nettolading van de `simple` meer woorden resultaat:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

De indeling van de nettolading van de `detailed` meer woorden resultaat:

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

## <a name="profanity-handling-in-speech-recognition"></a>Grof taalgebruik verwerking van spraakherkenning

De Service Microsoft Speech herkent alle vormen van menselijke spraak, met inbegrip van woorden en zinnen die veel mensen zouden classificeren als "grof taalgebruik." U kunt bepalen hoe grof taalgebruik in de service worden verwerkt met behulp van de *grof taalgebruik* queryparameter. Standaard maskeert de service grof taalgebruik in *speech.phrase* resulteert en retourneert geen *speech.hypothesis* berichten met grof taalgebruik.

| *Grof taalgebruik* waarde | Description |
| - | - |
| `masked` | Maskeert grof taalgebruik met een sterretje. Dit gedrag is de standaardinstelling. |
| `removed` | Hiermee verwijdert u grof taalgebruik uit alle resultaten. |
| `raw` | Herkent en grof taalgebruik in alle resultaten geretourneerd. |

### <a name="profanity-value-masked"></a>Grof taalgebruik waarde `Masked`

U wilt maskeren grof taalgebruik, stelt de *grof taalgebruik* queryparameter met de waarde *gemaskeerd*. Wanneer de *grof taalgebruik* queryparameter deze waarde heeft of niet is opgegeven voor een aanvraag, de service *maskers* grof taalgebruik. De service uitvoert maskering door te vervangen door sterretjes grof taalgebruik in de resultaten. Wanneer u grof taalgebruik maskering verwerking opgeeft, retourneert de service geen *speech.hypothesis* berichten met grof taalgebruik.

### <a name="profanity-value-removed"></a>Grof taalgebruik waarde `Removed`

Wanneer de *grof taalgebruik* query heeft de waarde van parameter *verwijderd*, de service verwijdert grof taalgebruik van zowel *speech.phrase* en *speech.hypothesis* berichten. De resultaten zijn hetzelfde *alsof de grof taalgebruik woorden zijn niet gesproken*.

#### <a name="profanity-only-utterances"></a>Alleen-grof taalgebruik uitingen

Een gebruiker kan spreken *alleen* grof taalgebruik wanneer een toepassing heeft de service te verwijderen van grof taalgebruik hebt geconfigureerd. Voor dit scenario, als de opname-modus is *dicteren* of *conversatie*, de service retourneert een *speech.result*. Als de opname-modus is *interactieve*, de service geeft als resultaat een *speech.result* met de statuscode *NoMatch*.

### <a name="profanity-value-raw"></a>Grof taalgebruik waarde `Raw`

Wanneer de *grof taalgebruik* query heeft de waarde van parameter *onbewerkte*, de service niet verwijderen of maskeren grof taalgebruik in ofwel de *speech.phrase* of  *Speech.hypothesis* berichten.
