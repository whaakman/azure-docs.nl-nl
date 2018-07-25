---
title: Veelgestelde vragen voor spraak-naar-tekst-Service op Azure | Microsoft Docs
description: Hier vindt u antwoorden op de meest populaire vragen over de spraak-naar-tekst.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 543e8d6fb68a351dfe75c962debaf15eeb080a3f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223884"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Veelgestelde vragen over spraak naar tekst

Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Custom Speech Service-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) en [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Algemeen

**Vraag**: Wat is het verschil tussen de basislijn en aangepaste spraak naar tekst-modellen?

**Antwoord**: basislijn modellen zijn getraind met gegevens eigendom zijn van Microsoft en al zijn geïmplementeerd in de cloud. Aangepaste modellen kunnen de gebruiker aan te passen van een model voor een betere aansluiting op een specifieke omgeving met specifieke omgevingsgeluid of -taal. Factory verdiepingen, auto's, ruis Streets (straten) aangepast akoestisch model zou vereisen dat bepaalde onderwerpen zoals biologie, natuurkunde, radiologie, productnamen en aangepaste acroniemen een aangepast taalmodel moet.

**Vraag**: waar moet ik beginnen als u wilt gebruiken een model basislijn?

**Antwoord**: u moet eerst aan een [abonnementssleutel](get-started.md). Als u maken van de REST-aanroepen naar de modellen predeployed basislijn wilt, raadpleegt u de [hier details](rest-apis.md). Als u wilt gebruiken van WebSockets downloaden de [SDK](speech-sdk.md)

**Vraag**: kan ik altijd nodig hebt om een aangepast spraakmodel ontwikkelen?

**Antwoord**: Nee, als uw toepassing van algemene dagelijkse taal gebruikmaakt en u niet hoeft voor het aanpassen van een model. Bovendien als uw toepassing wordt gebruikt in een omgeving waar er weinig of geen achtergrondgeluiden wordt u niet nodig hebt om aan te passen ofwel. De portal kan gebruikers basislijn en aangepaste modellen implementeren en nauwkeurigheidstests uitgevoerd. Gebruikers kunnen deze functie gebruiken om te meten nauwkeurigheid van een basislijn vs een aangepast model.

**Vraag**: hoe weet ik wanneer de verwerking van de gegevensset of het model voltooid is?

**Antwoord**: de status van het model of de gegevensset in de tabel is momenteel de enige manier om te weten.
Wanneer de verwerking voltooid is, wordt de status worden 'geslaagd'.

**Vraag**: kan ik meer dan één model maken?

**Antwoord**: Er is geen limiet voor het aantal modellen in uw verzameling zijn.

**Vraag**: ik gerealiseerde ik een fout hebt gemaakt. Hoe ik mijn gegevens importeren annuleren of model maken die wordt uitgevoerd? 

**Antwoord**: momenteel u kan niet worden teruggedraaid een aanpassing van woordenlijsten akoestische of taal proces. Geïmporteerde gegevens en -modellen kunnen worden verwijderd nadat ze in een definitieve status heeft zijn.

**Vraag**: Wat is het verschil tussen de zoekopdracht & dicteren modellen en het conversatie-modellen?

**Antwoord**: Er zijn meer dan één basislijn modellen kiezen uit in de Speech-Service. Het conversatie-model is geschikt voor het herkennen van spraak gesproken in een eigen stijl. Dit model zou ideaal voor het aanroepen van tijdens het zoeken te transcriberen en dicteren is ideaal voor spraak geactiveerd Apps. Universal is een nieuw model dat is erop gericht voor beide scenario's.

**Vraag**: kan ik mijn bestaande model (model stapelen) bijwerken?

**Antwoord**: bestaande modellen kunnen niet worden bijgewerkt. De oude gegevensset worden gecombineerd met de nieuwe en readapt als tijdelijke oplossing.

De oude en nieuwe gegevenssets moet worden gecombineerd in één Zip-(indien dit om akoestische gegevens is) of een txt-bestand als het language-gegevens. Aanpassing van woordenlijsten gebeurt nadat de nieuwe bijgewerkte model moet worden opgeheven geïmplementeerde verkrijgen van een nieuw eindpunt

**Vraag**: Wat gebeurt er als ik hogere gelijktijdigheid nodig voor mijn geïmplementeerde model dan wat wordt aangeboden in de portal. 

**Antwoord**: U kunt uw model in stappen van 20 gelijktijdige aanvragen opschalen. 

Contact met ons opnemen als u hogere schaal nodig hebt.

**Vraag**: kan ik mijn model downloaden en lokaal uitvoeren?

**Antwoord**: modellen niet kunnen worden gedownload en lokaal worden uitgevoerd.

**Vraag**: zijn mijn aanvragen in het logboek geregistreerd?

**Antwoord**: U hebt de mogelijkheid tijdens het maken van een implementatie te traceren, waarna er geen audio uitschakelen of transcripties worden geregistreerd. Anders aanvragen worden gewoonlijk is geregistreerd in Azure in veilige opslag. Als u nog meer vragen over privacy leiden die voorkomen dat u met behulp van de Custom Speech Service hebt, neem contact op met een van de ondersteuningskanalen.

## <a name="importing-data"></a>Gegevens importeren

**Vraag**: Wat is de limiet voor de grootte van de gegevensset? Hoe komt dat? 

**Antwoord**: de huidige limiet voor een gegevensset is 2 GB, vanwege de beperking voor de grootte van een bestand voor het uploaden van HTTP. 

**Vraag**: kan ik mijn tekstbestanden zip om het uploaden van een grotere tekstbestand? 

**Antwoord**: Nee, momenteel alleen niet-gecomprimeerde tekstbestanden zijn toegestaan.

**Vraag**: het gegevensrapport zegt er zijn mislukte uitingen. Wat is het probleem?

**Antwoord**: Failover-overschakeling naar 100% van de uitingen in een bestand uploaden is geen probleem.
Als het overgrote deel van de uitingen in een akoestische of elke taal worden gegevens (bijvoorbeeld > 95%) correct zijn geïmporteerd, de gegevensset kan worden gebruikt. Het wordt echter aanbevolen dat u probeert te begrijpen waarom de uitingen is mislukt en de problemen kunt oplossen. Meest voorkomende problemen, zoals het opmaken van fouten, zijn gemakkelijk op te lossen. 

## <a name="creating-am"></a>Het maken van AM

**Vraag**: hoeveel akoestische gegevens heb ik nodig?

**Antwoord**: kunt het beste beginnen met 30 minuten tot een uur om akoestische gegevens.

**Vraag**: welke gegevens moet ik verzamelen?

**Antwoord**: verzamelen van gegevens die zich zo dicht bij de toepassingsscenario en use-case mogelijk.
Het verzamelen van gegevens moet overeenkomen met de doeltoepassing en gebruikers in termen van apparaten of apparaten, omgevingen en typen sprekers. In het algemeen dient u te verzamelen gegevens van als brede tal van sprekers mogelijk. 

**Vraag**: hoe moet ik deze verzamelen? 

**Antwoord**: U kunt een zelfstandige gegevens verzameling-toepassing maken of sommige uit de audio-opnamen plank software gebruiken.
U kunt ook een versie van uw toepassing die de audio gegevens geregistreerd en gebruikt die maken. 

**Vraag**: heb ik nodig transcriberen aanpassing van woordenlijsten gegevens zelf? 

**Antwoord**: Ja! U kunt deze moet transcriberen zelf of een professionele transcriptie-service gebruiken. Sommige gebruikers liever typistes terwijl anderen crowdsourcing gebruiken of de transcripties zelf doen.

## <a name="accuracy-testing"></a>Nauwkeurigheidstests

**Vraag**: kan ik uitvoeren offline testen van mijn aangepast akoestisch model met behulp van een aangepast taalmodel?

**Antwoord**: Ja, selecteer de aangepaste taalmodel in de vervolgkeuzelijst bij het instellen van de offline testen

**Vraag**: kan ik uitvoeren offline testen van mijn aangepaste taalmodel met behulp van een aangepast akoestisch model?

**Antwoord**: Ja, selecteer de aangepast akoestisch model in de vervolgkeuzelijst bij het instellen van de offline-test.

**Vraag**: Wat is er Word fout tarief (WER) en hoe wordt het berekend?

**Antwoord**: Word fout tarief (WER) is de evaluatie van metrische gegevens voor spraakherkenning. Als het totale aantal fouten, waaronder invoegingen, verwijderingen en vervangingen, gedeeld door het totale aantal woorden in de referentie-transcriptie wordt meegeteld. [Hier](https://en.wikipedia.org/wiki/Word_error_rate) vindt u meer informatie.

**Vraag**: hoe bepaal ik als de resultaten van een test nauwkeurigheid is het verstandig?

**Antwoord**: de resultaten staat dat een vergelijking tussen de basislijn-model en de versie die u hebt aangepast.
U moet erop gericht zijn op maat van het model basislijn zodat aanpassing van de moeite waard.

**Vraag**: hoe ik achterhalen de frequentie van de Word-fouten van de basis modellen, zodat ik zien kan als er verbetering is? 

**Antwoord**: de resultaten van de offline nauwkeurigheid van de nauwkeurigheid van de basislijn van het aangepaste model en het verbeteren na verloop van basislijn weergeven.

## <a name="creating-lm"></a>Het maken van LM

**Vraag**: hoeveel tekstgegevens heb ik nodig om te uploaden?

**Antwoord**: dit is afhankelijk van hoe verschillende vocabulaire en zinnen die wordt gebruikt in uw toepassing afkomstig zijn van de eerste taalmodellen. Voor alle nieuwe woorden is het handig om zo veel voorbeelden bieden mogelijk van het gebruik van deze woorden. Voor veelvoorkomende zinnen die worden gebruikt in uw toepassing, is met inbegrip van zinnen in de taal-gegevens ook handig als het systeem om te luisteren naar deze voorwaarden ook krijgt. Het is gebruikelijk om ten minste één honderd en doorgaans enkele honderden uitingen in de gegevensset taal of meer. Als bepaalde typen query's worden naar verwachting worden vaker voor dan anderen, kunt u ook meerdere exemplaren van de algemene query's invoegen in de gegevensset.

**Vraag**: kan ik net uploaden een lijst met woorden?

**Antwoord**: een lijst met woorden uploaden wordt de woorden in vocabulaire maar geen informatie over het systeem hoe de woorden worden meestal gebruikt.
Door op te geven van de volledige of gedeeltelijke uitingen (zinnen bestaat dingen die gebruikers maken zich waarschijnlijk in te spreken) het taalmodel kunt informatie over de nieuwe woorden en hoe ze worden gebruikt. Het aangepaste taalmodel geldt niet alleen voor het ophalen van nieuwe woorden in het systeem, maar ook voor het aanpassen van de kans op bekende woorden voor uw toepassing. Volledige uitingen die helpt het systeem meer beter. 

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
