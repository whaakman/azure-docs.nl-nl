---
title: Inzicht in de verwerking van de tijd in Azure Stream Analytics
description: Meer informatie over time verwerken in Azure Stream Analytics
author: jasonwhowell
ms.author: zhongc
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 2a59a81b0894cbf58c5d3ab5a5569f4749b64b00
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543284"
---
# <a name="understand-time-handling-in-azure-stream-analytics"></a>Inzicht in de verwerking van de tijd in Azure Stream Analytics

In dit artikel bespreken we hoe u de ontwerpbeslissingen die betrekking hebben op te lossen praktische tijd verwerking van problemen in de Azure Stream Analytics-service kunt maken. Tijd verwerking ontwerp beslissingen nauw verwant zijn met gebeurtenisvolgordes factoren.

## <a name="background-time-concepts"></a>Achtergrond tijd concepten

Betere frame de discussie, gaan we enkele achtergrondconcepten definiëren:

- **Tijd van de gebeurtenis**: De tijd waarop de oorspronkelijke gebeurtenis heeft plaatsgevonden. Wanneer u een verplaatst benadert auto op de weg een tolloket nummer.

- **Verwerkingstijd**: De tijd waarop de gebeurtenis bereikt van het systeem verwerkt en wordt waargenomen. Bijvoorbeeld, duurt wanneer een nummer tolloket sensor ziet de auto's en het systeem enkele minuten om de gegevens te verwerken.

- **Watermerk**: Een gebeurtenis tijdsmarkering die aangeeft van alle gebeurtenissen tot welk moment is inkomend verkeer naar de processor voor streaming. Watermerken kunnen het systeem aangeven wissen wordt uitgevoerd op de gebeurtenissen kan opnemen. Door de aard van streams, de binnenkomende gebeurtenisgegevens nooit wordt gestopt, zodat watermerken de voortgang naar een bepaald punt in de stroom geven.

   Het watermerk-concept is belangrijk. Watermerken kunnen Stream Analytics om te bepalen wanneer het systeem voltooid, juist produceren kan en herhaalbare resultaten die niet hoeven te worden ingetrokken. De verwerking kan worden uitgevoerd in een voorspelbare en herhaalbare gegarandeerde manier. Bijvoorbeeld, als een na worden uitgevoerd voor foutafhandeling is voorwaarde moet, zijn watermerken veilig eerste en laatste punten.

Als u meer resources in dit onderwerp ziet u blogberichten van Tyler Akidau [Streaming 101](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101) en [Streaming 102](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-102).

## <a name="choosing-the-best-starting-time"></a>De beste begintijd kiezen

Stream Analytics biedt gebruikers twee opties voor het verzamelen van de tijd van de gebeurtenis:

1. **Ontvangsttijd**  

   Ontvangsttijd is toegewezen aan de invoerbron wanneer de gebeurtenis is bereikt de bron. U hebt toegang tot aankomsttijd via de **EventEnqueuedUtcTime** eigenschap voor Event Hubs-invoer, **IoTHub.EnqueuedTime** eigenschap voor IoT Hub en het gebruik van de **BlobProperties.LastModified**  eigenschap voor blob-invoer.

   Met behulp van aankomsttijd is de standaardinstelling en het meest geschikt voor scenario's, archivering van gegevens waarbij er geen temporele logica die nodig zijn.

2. **Tijd van de toepassing** (ook genoemd tijd van de gebeurtenis)

   Tijd van de toepassing wordt toegewezen wanneer de gebeurtenis wordt gegenereerd en is het deel van de nettolading. Voor het verwerken van gebeurtenissen op tijd van de toepassing, gebruikt u de **Timestamp door** -component in de select-query. Als de **Timestamp door** component niet aanwezig is, gebeurtenissen worden verwerkt door aankomsttijd.

   Het is belangrijk om te gebruiken een tijdstempel in de nettolading wanneer temporele logica is betrokken. Op die manier kunnen vertragingen in het bronsysteem of in het netwerk kunnen worden verwerkt.

## <a name="how-time-progresses-in-azure-stream-analytics"></a>Hoe tijd worden uitgevoerd in Azure Stream Analytics

Wanneer u de toepassing de tijd, wordt de voortgang van de tijd is gebaseerd op de binnenkomende gebeurtenissen. Het is moeilijk voor de verwerkingssysteem weten als er zijn geen gebeurtenissen of als de gebeurtenissen worden vertraagd. Om deze reden genereert Azure Stream Analytics heuristische watermerken in de volgende manieren voor elke invoer partitie:

1. Wanneer er sprake is van een inkomende gebeurtenis, is het watermerk de grootste gebeurtenistijd die we tot nu toe hebben gezien minus de venstergrootte out volgorde tolerantie.

2. Als er geen inkomende gebeurtenis is, is het watermerk de huidige geschatte aankomsttijd (de verstreken tijd op achter de schermen virtuele machine de verwerking van de gebeurtenissen van de laatste keer een invoer gebeurtenis wordt gezien plus die invoer van de gebeurtenis aankomsttijd) minus het latere aankomst tolerantie-venster.

   De aankomsttijd kan alleen worden geschat omdat de echte ontvangsttijd is gegenereerd op de invoer gebeurtenis-broker, zoals Event Hubs, en niet de Azure Stream Analytics VM de gebeurtenissen worden verwerkt.

Het ontwerp van de fungeert twee andere doeleinden, naast het genereren van watermerken:

1. Het systeem genereert resultaten op tijdige wijze met of zonder binnenkomende gebeurtenissen.

   U hebt controle over hoe tijdige ze willen om de resultaten van de uitvoer te bekijken. In de Azure-portal op de **Gebeurtenisvolgorde** pagina van uw Stream Analytics-taak, kunt u configureren de **niet-geordende gebeurtenissen** instelling. Wanneer u deze instelling configureert, houd rekening met de verhouding van de actualiteit met tolerantie van out volgorde gebeurtenissen in de gebeurtenisstroom.

   Het laat aankomst tolerantie-venster is belangrijk om te blijven genereren van watermerken, zelfs in de afwezigheid van binnenkomende gebeurtenissen. Soms kunnen er een periode waarin geen binnenkomende gebeurtenissen zijn verkrijgbaar in, zoals wanneer de invoerstroom van een gebeurtenis verspreid is. Dit probleem wordt veroorzaakt door het gebruik van meerdere partities in de invoer gebeurtenis-broker.

   Streaming van gegevensverwerking systemen zonder een latere aankomst tolerantie venster mogelijk vertraagd uitvoer lijden als invoer sparse en meerdere partities worden gebruikt.

2. De werking van het systeem moet worden herhaald. Herhaalbaarheid is een belangrijke eigenschap van een systeem met streaming gegevens verwerken.

   Het watermerk is afgeleid van de aankomsttijd en toepassing. Beide worden persistent gemaakt in de gebeurtenis-broker, en dus herhaalbare. In het geval is de aankomsttijd worden bepaald in de afwezigheid van gebeurtenissen, Azure Stream Analytics tijdschriften, de verwachte aankomsttijd voor herhaalbaarheid tijdens een herhaling voor foutherstel.

U ziet dat wanneer u wilt gebruiken **aankomsttijd** als de tijd van de gebeurtenis, is het niet nodig om de out-van-order tolerantie en laat aankomst tolerantie te configureren. Aangezien **aankomsttijd** worden gestaag toenemende in de invoer gebeurtenis-broker, Azure Stream Analytics gewoon negeert de configuraties is gegarandeerd.

## <a name="late-arriving-events"></a>Te late binnenkomende gebeurtenissen

Per definitie laat aankomst tolerantie venster worden voor elke inkomende gebeurtenis, Azure Stream Analytics vergelijkt de **gebeurtenistijd** met de **aankomsttijd**; als de tijd van de gebeurtenis buiten het venster tolerantie is u kan het systeem te verwijderen van de gebeurtenis of aanpassen van de tijd van de gebeurtenis moet zich binnen de tolerantie configureren.

Houd rekening met dat nadat watermerken zijn gegenereerd, de service mogelijk gebeurtenissen met de tijd van de gebeurtenis is lager dan het watermerk ontvangen kan. U kunt de service te configureren **neerzetten** die gebeurtenissen of **aanpassen** tijd van de gebeurtenis voor de grenswaarde.

Als onderdeel van de aanpassing, de gebeurtenis van **System.Timestamp** is ingesteld op de nieuwe waarde, maar de **gebeurtenistijd** veld zelf worden niet gewijzigd. Deze aanpassing is de enige situatie waarbij van een gebeurtenis **System.Timestamp** kan anders zijn dan de waarde in het veld van de tijd gebeurtenis en kan leiden tot onverwachte resultaten moeten worden gegenereerd.

## <a name="handling-time-variation-with-substreams"></a>Tijd variatie met substreams verwerken

Het mechanisme voor het genereren van heuristische watermerk beschreven hier werkt goed in de meeste gevallen waar tijd voornamelijk worden gesynchroniseerd tussen de verschillende afzenders van gebeurtenissen. In de praktijk heeft met name in veel IoT-scenario's, het systeem echter weinig controle over de klok op de afzenders van gebeurtenissen. De afzenders van gebeurtenissen mogelijk allerlei apparaten in het veld, mogelijk op verschillende versies van hardware en software.

In plaats van een watermerk globale aan alle gebeurtenissen in een invoer-partitie, is Stream Analytics in een ander mechanisme substreams om u te helpen met de naam. U kunt substreams gebruiken in uw taak door het schrijven van de query van een taak die gebruikmaakt van de [ **TIMESTAMP BY** ](/stream-analytics-query/timestamp-by-azure-stream-analytics) component en het sleutelwoord **via**. Als u wilt aanwijzen de substromen, geef de naam van een sleutelkolom na de **via** trefwoord, zoals een `deviceid`, zodat het systeem tijd beleidsregels toegepast op die kolom. Elke substromen Hiermee haalt u een eigen onafhankelijke watermerk. Dit mechanisme is nuttig voor het toestaan van tijdige uitvoer wordt gegenereerd, als laat omgaan met grote kloksnelheid of netwerk vertragingen tussen afzenders van gebeurtenissen.

Substreams een unieke oplossing die is geleverd door Azure Stream Analytics zijn en niet door andere streaming gegevensverwerking-systemen worden aangeboden. Stream Analytics geldt het latere aankomst tolerantie venster voor binnenkomende gebeurtenissen wanneer substreams worden gebruikt. De standaardinstelling (5 seconden) is waarschijnlijk te klein voor apparaten met uiteenlopende tijdstempels. U wordt aangeraden dat u met 5 minuten begint en op basis van hun apparaat klok scheeftrekken patroon past.

## <a name="early-arriving-events"></a>Vroege binnenkomende gebeurtenissen

U hebt mogelijk een andere concept met de naam vroege aankomst venster dat lijkt op het tegenovergestelde van laat aankomst tolerantie venster gezien. In dit venster wordt vastgesteld op 5 minuten en heeft een ander doel vanaf eind aankomst één.

Omdat Azure Stream Analytics volledige resultaten altijd wordt gegenereerd garandeert, kunt u alleen opgeven **begintijd taak** als de eerste uitvoertijd van de taak niet de invoer. De begintijd van taak is vereist zodat het volledige venster wordt verwerkt, niet alleen uit het midden van het venster.

Stream Analytics is vervolgens afgeleid van de begintijd van de queryspecificatie. Omdat invoer gebeurtenis-broker wordt alleen geïndexeerd door aankomsttijd, moet het systeem heeft echter voor de omzetting van de begintijd van de gebeurtenis naar aankomsttijd. Het systeem kunt verwerken van gebeurtenissen starten vanaf dat moment in de invoer gebeurtenis-broker. Met de eerdere limiet voor binnenkomende venster is de vertaling eenvoudig. Deze wordt tijd minus de 5 minuten vroege binnenkomen venster van de gebeurtenis gestart. Deze berekening betekent ook dat het systeem alle gebeurtenissen die worden bekeken met gebeurtenistijd vijf minuten groter dan Aankomsttijd komt.

Dit concept wordt gebruikt om te controleren of dat de verwerking is herhaalbare, ongeacht waar u begint met de uitvoer van. Zonder een mechanisme zou het niet mogelijk om te waarborgen van herhaalbaarheid, als er veel streaming systemen claim dat ze doen.

## <a name="side-effects-of-event-ordering-time-tolerances"></a>Neveneffecten van tijdtoleranties gebeurtenisvolgordes

Stream Analytics-taken hebben verschillende **Gebeurtenisvolgorde** opties. Twee kan worden geconfigureerd in Azure portal: de **niet-geordende gebeurtenissen** instelling (out volgorde tolerantie), en de **gebeurtenissen die te laat gemeld worden** instelling (laat aankomst tolerantie). De **vroege aankomst** tolerantie is vast en kan niet worden aangepast. Deze beleidsregels tijd worden gebruikt door Stream Analytics voor sterke garanties bieden voor de. Deze instellingen hebben echter enkele soms onverwacht gevolgen:

1. Per ongeluk verzenden van gebeurtenissen die te vroeg zijn.

   Vroege gebeurtenissen moeten normaal gesproken niet output. Het is mogelijk dat er vroege gebeurtenissen worden verzonden naar de uitvoer als de klok van de afzender te snel al wordt uitgevoerd. Alle eerdere binnenkomende gebeurtenissen worden verwijderd, zodat u ze uit de uitvoer niet zal zien.

2. Oude gebeurtenissen verzenden naar Event Hubs moeten worden verwerkt door Azure Stream Analytics.

   Terwijl de oude gebeurtenissen lijken misschien onschadelijk op eerst, vanwege de toepassing van de latere aankomst tolerantie, kunnen de oude gebeurtenissen worden verwijderd. Als de gebeurtenissen te oud is, zijn de **System.Timestamp** waarde is gewijzigd tijdens gebeurtenisopname. Vanwege dit gedrag is momenteel Azure Stream Analytics beter geschikt voor scenario's, in plaats van de verwerking van historische gebeurtenissen scenario's voor near-real-time-gebeurtenisverwerking. U kunt instellen dat de **gebeurtenissen die te laat gemeld worden** introductie op de grootste mogelijke waarde (20 dagen) om dit gedrag in sommige gevallen te omzeilen.

3. Uitvoer lijken te worden uitgesteld.

   Het eerste watermerk is gegenereerd toen de berekende tijd: de **maximale gebeurtenistijd** het systeem tot nu toe is waargenomen minus de venstergrootte out volgorde tolerantie. De tolerantie out volgorde is standaard geconfigureerd voor nul (00 minuten en 00 seconden). Wanneer u dit op een hoger, niet-nul tijd-waarde instellen, de eerste uitvoer van de streaming-taak is vertraagd worden door de waarde van de tijd (of hoger) vanwege het watermerk eerst die wordt berekend.

4. Invoer zijn verspreid.

   Wanneer er geen invoer in een bepaalde partitie, de tijd van het watermerk wordt berekend als de **aankomsttijd** minus het latere aankomst tolerantie-venster. Als gevolg hiervan als invoergebeurtenissen niet-frequente en sparse zijn, kan de uitvoer kan worden uitgesteld door deze hoeveelheid tijd. De standaardwaarde **gebeurtenissen die te laat gemeld worden** waarde is 5 seconden. U kunt verwachten om te zien van enige vertraging bij het verzenden van invoergebeurtenissen één bewerking tegelijk, bijvoorbeeld. De vertragingen slechter, kunnen krijgen bij het instellen van **gebeurtenissen die te laat gemeld worden** venster met een grote waarde.

5. **System.Timestamp** waarde verschilt van de tijd in de **gebeurtenistijd** veld.

   Zoals eerder beschreven, past het systeem de tijd van de gebeurtenis door de tolerantie van-order-out- of laat aankomst tolerantie windows. De **System.Timestamp** waarde van de gebeurtenis wordt aangepast, maar niet de **gebeurtenistijd** veld.

## <a name="metrics-to-observe"></a>Metrische gegevens om te observeren

U kunt een aantal van de gevolgen van de tijd tolerantie via voor Gebeurtenisvolgordes observeren [metrische gegevens voor Stream Analytics-taak](stream-analytics-monitoring.md). De volgende metrische gegevens die zijn relevant:

|Gegevens  | Description  |
|---------|---------|
| **Out-van-Order gebeurtenissen** | Geeft het aantal gebeurtenissen ontvangen andere volgorde, die zijn verwijderd of een aangepaste tijdstempel gegeven. Met deze metriek wordt direct beïnvloed door de configuratie van de **niet-geordende gebeurtenissen** instellen op de **Gebeurtenisvolgorde** pagina op de taak in Azure portal. |
| **Late Invoergebeurtenissen** | Geeft het aantal gebeurtenissen dat binnenkomt laat vanuit de bron. Met deze metriek bevat gebeurtenissen die zijn verwijderd of hebben gehad hun timestamp is aangepast. Met deze metriek wordt direct beïnvloed door de configuratie van de **gebeurtenissen die te laat gemeld worden** instellen in de **Gebeurtenisvolgorde** pagina op de taak in Azure portal. |
| **Vroege-Invoergebeurtenissen** | Geeft het aantal gebeurtenissen dat vroeg binnenkomt vanuit de bron die ofwel is verwijderd of hun timestamp als ze meer dan 5 minuten te vroeg zijn is aangepast. |
| **Vertraging voor watermerk** | Geeft aan dat de vertraging van de streamingtaak voor gegevensverwerking. Zie meer informatie in de volgende sectie.|

## <a name="watermark-delay-details"></a>Details van de vertraging watermerk

De **watermerk vertraging** metrische waarde wordt berekend als de kloktijd van het verwerkingsknooppunt minus het grootste watermerk deze tot nu toe is gezien. Zie voor meer informatie de [watermerk vertraging blogbericht](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/).

Er zijn verschillende redenen deze metrische waarde groter dan 0 onder normale omstandigheden is:

1. Inherente verwerkingsvertraging van de streamingpijplijn automatiseert. Deze vertraging is normaal gesproken nominale.

2. Het venster out volgorde tolerantie geïntroduceerd vertraging, omdat het watermerk wordt verminderd door de grootte van de tolerantie-venster.

3. Het laat aankomst venster geïntroduceerd vertraging, omdat het watermerk wordt beperkt door de grootte van de tolerantie-venster.

4. Klok scheeftrekken van de van verwerkingsknooppunt genereren van de metrische gegevens.

Er zijn een aantal andere resourcebeperkingen die leiden de streamingpijplijn automatiseert tot kunnen te vertragen. De vertraging watermerk metriek kan oplopen vanwege:

1. Er is onvoldoende verwerkingscapaciteit in Stream Analytics om het volume aan invoergebeurtenissen te verwerken. Als u wilt schalen van resources, Zie [begrijpen en aanpassen van Streaming-eenheden](stream-analytics-streaming-unit-consumption.md).

2. Er is onvoldoende doorvoer in de invoer-brokers, zodat ze worden beperkt. Zie voor mogelijke oplossingen [automatisch schalen van Azure Event Hubs-doorvoereenheden](../event-hubs/event-hubs-auto-inflate.md).

3. Uitvoerlocaties zijn niet ingericht met voldoende capaciteit, zodat ze worden beperkt. De mogelijke oplossingen variëren sterk op basis van de versie van de uitvoer-service wordt gebruikt.

## <a name="output-event-frequency"></a>Frequentie van uitvoer-gebeurtenis

Azure Stream Analytics maakt gebruik van watermerk wordt uitgevoerd als de trigger alleen voor het produceren van uitvoergebeurtenissen. Omdat het watermerk is afgeleid van invoergegevens, is het herhaalbare tijdens het foutherstel en ook in door de gebruiker geïnitieerde opnieuw verwerken.

Bij het gebruik van [statistische functies in vensters](stream-analytics-window-functions.md), de service levert alleen uitvoer aan het einde van de windows. In sommige gevallen kunnen gebruikers willen om te zien van gedeeltelijke statistische functies die zijn gegenereerd op basis van de windows. Gedeeltelijke statistische functies worden momenteel niet ondersteund in Azure Stream Analytics.

In andere streamingoplossingen kunnen uitvoergebeurtenissen worden gematerialiseerd op verschillende tijdstippen van de trigger, afhankelijk van externe omstandigheden. Het is mogelijk in een aantal oplossingen die de uitvoergebeurtenissen voor een bepaalde periode meerdere keren kunnen worden gegenereerd. Als de ingevoerde waarden verfijnd worden, worden de resultaten samenvoegen meer nauwkeurige. Gebeurtenissen kunnen worden speculated op eerste en de herziene na verloop van tijd. Wanneer een bepaalde apparaat offline is vanaf het netwerk is, kan er bijvoorbeeld een geschatte waarde worden gebruikt door een systeem. Later op komt hetzelfde apparaat online met het netwerk. Gegevens van de werkelijke gebeurtenis kan vervolgens worden opgenomen in de invoerstroom. Het resultaat dat tijdvenster worden verwerkt, wordt meer nauwkeurige uitvoer gegenereerd.

## <a name="illustrated-example-of-watermarks"></a>Voorbeeld van watermerken

De volgende afbeeldingen laten zien hoe de voortgang van watermerken onder verschillende omstandigheden.

Deze tabel bevat de bijvoorbeeld gegevens die hieronder wordt uitgezet. U ziet dat de tijd van de gebeurtenis en de aankomsttijd verschillen, soms die overeenkomen met en soms niet.

| Tijdstip van gebeurtenis | Ontvangsttijd | DeviceId |
| --- | --- | --- |
| 12:07 | 12:07 | device1
| 12:08 | 12:08 | device2
| 12:17 | 12:11 | device1
| 12:08 | 12:13 | device3
| 12:19 | 12:16 | device1
| 12:12 | 12:17 | device3
| 12:17 | 12:18 | device2
| 12:20 | 12:19 | device2
| 12:16 | 12:21 | device3
| 12:23 | 12:22 | device2
| 12:22 | 12:24 | device2
| 12:21 | 12:27 | device3

In deze afbeelding worden de volgende toleranties gebruikt:

- Vroege aankomst windows is 5 minuten
- Laat binnenkomen venster is 5 minuten
- Venster voor opnieuw ordenen is 2 minuten

1. Afbeelding van het watermerk is voortgezet door middel van deze gebeurtenissen:

   ![Afbeelding van Azure Stream Analytics-watermerk](media/stream-analytics-time-handling/WaterMark-graph-1.png)

   Belangrijke processen geïllustreerd in de voorgaande afbeelding:

   1. De eerste gebeurtenis (device1) en de tweede gebeurtenis (device2) tijden zijn uitgelijnd en zonder aanpassingen worden verwerkt. De voortgang van het watermerk bij elke gebeurtenis.

   2. Tijdens het verwerken van de derde gebeurtenis (device1), voorafgegaan door de aankomsttijd (12:11) de tijd van de gebeurtenis (12:17). De gebeurtenis is aangekomen 6 minuten vroeg mogelijk bij, zodat de gebeurtenis is verwijderd vanwege de tolerantie voor vroege aankomst van 5 minuten.

      De voortgang van het watermerk niet in dit geval van een vroege gebeurtenis.

   3. De gebeurtenis (device3) van de vierde en vijfde gebeurtenis (device1) tijden zijn uitgelijnd en worden verwerkt zonder aanpassing. De voortgang van het watermerk bij elke gebeurtenis.

   4. Wanneer de zesde gebeurtenis (device3) is verwerkt, wordt de aankomsttijd (12:17) en het tijdstip van de gebeurtenis (12:12) is lager dan het niveau van het watermerk. De tijd van de gebeurtenis wordt ingesteld op het niveau van de water mark (12:17).

   5. Wanneer de achtste gebeurtenis (device3) is verwerkt, is de aankomsttijd (12:27) 6 minuten vooraf de gebeurtenis (12:21). Het laat aankomst-beleid wordt toegepast. De tijd van de gebeurtenis is aangepast (12:22) is hoger dan het watermerk (12:21) zodat er geen verdere aanpassing wordt toegepast.

2. Tweede afbeelding van het watermerk wordt voortgezet zonder een vroege aankomst beleid:

   ![Azure Stream Analytics geen vroege beleid watermerk-afbeelding](media/stream-analytics-time-handling/watermark-graph-2.png)

   In dit voorbeeld zijn er geen vroege aankomst-beleid wordt toegepast. Uitschieter gebeurtenissen die vroeger verhogen aanzienlijk van het watermerk. U ziet de derde gebeurtenis (deviceId1 tijd 12:11) niet in dit scenario is verwijderd en het watermerk is verhoogd tot 12:15. De vierde tijd van de gebeurtenis is als gevolg hiervan aangepaste forward 7 minuten (12:08 tot 12:15).

3. In de laatste afbeelding substreams gebruikt (via de apparaat-id). Meerdere watermerken worden bijgehouden, één per stream. Er zijn minder gebeurtenissen met hun tijden aangepast als gevolg hiervan.

   ![Azure Stream Analytics substreams watermerk afbeelding](media/stream-analytics-time-handling/watermark-graph-3.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure Stream Analytics gebeurtenis order overwegingen](stream-analytics-out-of-order-and-late-events.md)
- [Stream Analytics-taak metrische gegevens](stream-analytics-monitoring.md)
