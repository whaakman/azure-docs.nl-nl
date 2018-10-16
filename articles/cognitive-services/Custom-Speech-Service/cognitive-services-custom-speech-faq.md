---
title: Veelgestelde vragen over de Custom Speech Service op Azure | Microsoft Docs
description: Hier vindt u antwoorden op de meest populaire vragen over de Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: 8e3d5e0e2b70d8f97099103ed369e48dd74d56e2
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341358"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Veelgestelde vragen over Custom Speech Service

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)] 

Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Custom Speech Service-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) en [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Algemeen

**Vraag**: hoe weet ik wanneer de verwerking van de gegevensset of het model voltooid is?

**Antwoord**: de status van het model of de gegevensset in de tabel is momenteel de enige wil weten.
Wanneer de verwerking voltooid is, zijn de status 'Gereed'.
Er wordt gewerkt aan betere methoden voor het verwerken van status, zoals e-mailmelding communicatie.

**Vraag**: kan ik meer dan één model maken op een tijdstip?

**Antwoord**: Er is geen limiet voor het aantal modellen in uw verzameling zijn, maar slechts één bewerking tegelijk op elke pagina kan worden gemaakt.
U kunt bijvoorbeeld een proces voor het maken van taal model niet starten als er een taalmodel in de procesfase is.
U kunt echter een akoestisch model en een taalmodel verwerken op hetzelfde moment hebben. 

**Vraag**: ik gerealiseerde ik een fout hebt gemaakt. Hoe ik mijn gegevens importeren annuleren of model maken die wordt uitgevoerd? 

**Antwoord**: momenteel u kan niet worden teruggedraaid een aanpassing van woordenlijsten akoestische of taal proces.
Geïmporteerde gegevens kunnen worden verwijderd nadat het importeren is voltooid

**Vraag**: Wat is het verschil tussen de zoekopdracht & dicteren modellen en het conversatie-modellen?

**Antwoord**: Er zijn twee Base akoestische & Taalmodellen kiezen uit in de Custom Speech Service.
zoekquery's of dicteren. De Microsoft-Conversational AM is geschikt voor het herkennen van spraak gesproken in een eigen stijl.
Dit type spraak is doorgaans gericht op een andere persoon, zoals in callcenters of vergaderingen.

**Vraag**: kan ik mijn bestaande model (model stapelen) bijwerken?

**Antwoord**: Wij bieden de mogelijkheid om bij te werken van een bestaand model met nieuwe gegevens.
Als u een nieuwe gegevensset hebt en u wilt aanpassen van een bestaand model moet u deze opnieuw aanpassen met de nieuwe gegevens en de oude gegevensset die u hebt gebruikt.
De oude en nieuwe gegevenssets moet worden gecombineerd tot een enkel Zip (indien dit om akoestische gegevens is) of een txt-bestand als taal gegevens eenmaal aanpassing is de nieuwe bijgewerkte model moet worden opgeheven geïmplementeerde verkrijgen van een nieuw eindpunt wordt uitgevoerd

**Vraag**: Wat gebeurt er als ik nodig heb hogere gelijktijdigheid dan de standaardwaarde. 

**Antwoord**: U kunt uw model in stappen van 5 gelijktijdige aanvragen noemen we schaaleenheden opschalen. Elke schaaleenheid garandeert dat uw model 5 audiostream tegelijk kan verwerken. U kunt kopen van 100 schaaleenheden (of 500 gelijktijdige aanvragen).

Neem contact op als u meer dan die nodig hebt.

**Vraag**: kan ik mijn model downloaden en lokaal uitvoeren?

**Antwoord**: We Schakel geen modellen om te worden gedownload en lokaal worden uitgevoerd.

**Vraag**: zijn mijn aanvragen in het logboek geregistreerd?

**Antwoord**: U hebt de mogelijkheid tijdens het maken van een implementatie te traceren, waarna er geen audio uitschakelen of transcripties worden geregistreerd. Anders aanvragen worden gewoonlijk is geregistreerd in Azure in veilige opslag. Als u nog meer vragen over privacy leiden die voorkomen dat u met behulp van de Custom Speech Service Neem contact met ons hebt.

## <a name="importing-data"></a>Gegevens importeren

**Vraag**: Wat is de limiet voor de grootte van de gegevensset? Hoe komt dat? 

**Antwoord**: de huidige limiet voor een gegevensset is 2 GB, vanwege de beperking voor de grootte van een bestand voor het uploaden van HTTP. 

**Vraag**: kan ik mijn tekstbestanden zip om het uploaden van een grotere tekstbestand? 

**Antwoord**: Nee, momenteel alleen niet-gecomprimeerde tekstbestanden zijn toegestaan.

**Vraag**: het gegevensrapport zegt er zijn mislukte uitingen. Is dit een probleem?

**Antwoord**: als er slechts een paar uitingen kunnen niet worden geïmporteerd, dit is geen probleem.
Als het overgrote deel van de uitingen in een akoestische of elke taal worden gegevens (bijvoorbeeld > 95%) correct zijn geïmporteerd, de gegevensset kan worden gebruikt. Het wordt echter aanbevolen dat u probeert te begrijpen waarom de uitingen is mislukt en de problemen kunt oplossen.
Meest voorkomende problemen, zoals het opmaken van fouten, zijn gemakkelijk op te lossen. 

## <a name="creating-am"></a>Het maken van AM

**Vraag**: hoeveel akoestische gegevens heb ik nodig?

**Antwoord**: kunt het beste beginnen met 30 minuten tot een uur om akoestische gegevens

**Vraag**: welk soort gegevens moet ik verzamelen?

**Antwoord**: U moet verzamelen van gegevens die zich zo dicht bij de toepassingsscenario en use-case mogelijk.
Dit betekent dat het verzamelen van gegevens moet overeenkomen met de doeltoepassing en gebruikers in termen van apparaten of apparaten, omgevingen en typen sprekers. In het algemeen dient u te verzamelen gegevens van als brede tal van sprekers mogelijk. 

**Vraag**: hoe moet ik deze verzamelen? 

**Antwoord**: U kunt een zelfstandige gegevens verzameling-toepassing maken of sommige uit de audio-opnamen plank software gebruiken.
U kunt ook een versie van uw toepassing die de audio gegevens geregistreerd en gebruikt die maken. 

**Vraag**: heb ik nodig transcriberen aanpassing van woordenlijsten gegevens zelf? 

**Antwoord**: de gegevens moet worden omgezet. U kunt deze moet transcriberen zelf of een professionele transcriptie-service gebruiken. Sommige van deze typistes gebruik en andere gebruik crowdsourcing. Kan het beste ook een transcriptie-service op aanvraag.

**Vraag**: hoe lang duurt het maken van een aangepast akoestisch model?

**Antwoord**: de verwerkingstijd voor het maken van een aangepast akoestisch model is ongeveer hetzelfde als de lengte van de akoestische gegevensset.
Dus een aangepast akoestisch model gemaakt op basis van een gegevensset vijf uur duurt ongeveer vijf uur om te verwerken. 

## <a name="offline-testing"></a>Offline testen

**Vraag**: kan ik uitvoeren offline testen van mijn aangepast akoestisch model met behulp van een aangepast taalmodel?

**Antwoord**: Ja, drukt u op het aangepaste taalmodel in de vervolgkeuzelijst bij het instellen van de offline testen

**Vraag**: kan ik uitvoeren offline testen van mijn aangepaste taalmodel met behulp van een aangepast akoestisch model?

**Antwoord**: Ja, selecteer de aangepast akoestisch model in de vervolgkeuzelijst bij het instellen van de offline-test.

**Vraag**: Wat is de Foutfrequentie Word en hoe wordt het berekend?

**Antwoord**: Foutfrequentie Word de evaluatie van metrische gegevens voor spraakherkenning is. Als het totale aantal fouten, waaronder invoegingen, verwijderingen en vervangingen, gedeeld door het totale aantal woorden in de referentie-transcriptie wordt meegeteld.

**Vraag**: ik nu de testresultaten van mijn aangepaste model weet, is dit een goede of slechte getal?

**Antwoord**: de resultaten staat dat een vergelijking tussen de basislijn-model en de versie die u hebt aangepast.
U moet erop gericht zijn op maat van het model basislijn zodat aanpassing van de moeite waard maken

**Vraag**: hoe ik achterhalen de Windows Foutrapportage van de basis modellen, zodat ik zien kan als er verbetering is? 

**Antwoord**: de resultaten van de offline nauwkeurigheid van de nauwkeurigheid van de basislijn van het aangepaste model en het verbeteren na verloop van basislijn weergeven

## <a name="creating-lm"></a>Het maken van LM

**Vraag**: hoeveel tekstgegevens heb ik nodig om te uploaden?

**Antwoord**: dit is een moeilijk vraag een nauwkeurige antwoord te geven, omdat deze afhankelijk hoe verschillende vocabulaire en zinnen die wordt gebruikt in uw toepassing afkomstig zijn van de eerste taalmodellen. Voor alle nieuwe woorden is het handig om zo veel voorbeelden bieden mogelijk van het gebruik van deze woorden. Voor veelvoorkomende zinnen die worden gebruikt in uw toepassing, is inclusief verbindingen in de gegevens van de taal ook handig als het systeem om te luisteren naar deze voorwaarden ook krijgt.
Het is gebruikelijk om ten minste één honderd en doorgaans enkele honderden uitingen in de gegevensset taal of meer.
Ook als bepaalde typen * van query's naar verwachting wordt vaker voor dan anderen, kunt u meerdere exemplaren van de algemene query's in de gegevensset.

**Vraag**: kan ik net uploaden een lijst met woorden?

**Antwoord**: een lijst met woorden uploaden wordt de woorden in vocabulaire maar geen informatie over het systeem hoe de woorden worden meestal gebruikt.
Het taalmodel kunt informatie over de nieuwe woorden en hoe ze worden gebruikt door te geven van de volledige of gedeeltelijke uitingen (zinnen bestaat van gebruikers maken zich waarschijnlijk in te spreken dingen). Het aangepaste taalmodel geldt niet alleen voor het ophalen van nieuwe woorden in het systeem, maar ook voor het aanpassen van de kans op bekende woorden voor uw toepassing. Volledige uitingen die helpt het systeem leert kennen. 

-----

 * [Overzicht](cognitive-services-custom-speech-home.md)
 * [Aan de slag](cognitive-services-custom-speech-get-started.md)
 * [Woordenlijst](cognitive-services-custom-speech-glossary.md)
