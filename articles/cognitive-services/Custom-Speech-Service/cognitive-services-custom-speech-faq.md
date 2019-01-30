---
title: Veelgestelde vragen over de Custom Speech Service op Azure | Microsoft Docs
description: Hier vindt u antwoorden op de meest populaire vragen over de Custom Speech Service.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: 88486ec9d1ca11d25ca31ca0abb4a34509d19a27
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228029"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Veelgestelde vragen over Custom Speech Service

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)] 

Als u antwoorden op uw vragen niet in deze Veelgestelde vragen vinden, misschien dat de Custom Speech Service-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) en [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Algemeen

**Vraag**: Hoe weet ik wanneer de verwerking van de gegevensset of het model voltooid is?

**antwoord**: De status van het model of de gegevensset in de tabel is momenteel de enige wil weten.
Wanneer de verwerking voltooid is, zijn de status 'Gereed'.
Er wordt gewerkt aan betere methoden voor het verwerken van status, zoals e-mailmelding communicatie.

**Vraag**: Kan ik meer dan één model maken op een tijdstip?

**antwoord**: Er is geen limiet voor het aantal modellen in uw verzameling zijn, maar slechts één bewerking tegelijk op elke pagina kan worden gemaakt.
U kunt bijvoorbeeld een proces voor het maken van taal model niet starten als er een taalmodel in de procesfase is.
U kunt echter een akoestisch model en een taalmodel verwerken op hetzelfde moment hebben. 

**Vraag**: Ik gerealiseerde dat ik een fout hebt gemaakt. Hoe ik mijn gegevens importeren annuleren of model maken die wordt uitgevoerd? 

**antwoord**: Op dit moment u kan niet worden teruggedraaid een aanpassing van woordenlijsten akoestische of taal proces.
Geïmporteerde gegevens kunnen worden verwijderd nadat het importeren is voltooid

**Vraag**: Wat is het verschil tussen de zoekopdracht & dicteren modellen en het conversatie-modellen?

**antwoord**: Er zijn twee Base akoestische & Taalmodellen kiezen uit in de Custom Speech Service.
zoekquery's of dicteren. De Microsoft-Conversational AM is geschikt voor het herkennen van spraak gesproken in een eigen stijl.
Dit type spraak is doorgaans gericht op een andere persoon, zoals in callcenters of vergaderingen.

**Vraag**: Kan ik mijn bestaande model (model stapelen) bijwerken?

**antwoord**: Wij bieden de mogelijkheid om bij te werken van een bestaand model met nieuwe gegevens.
Als u een nieuwe gegevensset hebt en u wilt aanpassen van een bestaand model moet u deze opnieuw aanpassen met de nieuwe gegevens en de oude gegevensset die u hebt gebruikt.
De oude en nieuwe gegevenssets moet worden gecombineerd tot een enkel Zip (indien dit om akoestische gegevens is) of een txt-bestand als taal gegevens eenmaal aanpassing is de nieuwe bijgewerkte model moet worden opgeheven geïmplementeerde verkrijgen van een nieuw eindpunt wordt uitgevoerd

**Vraag**: Wat gebeurt er als ik moet hogere gelijktijdigheid dan de standaardwaarde. 

**antwoord**: U kunt uw model in stappen van 5 gelijktijdige aanvragen noemen we schaaleenheden opschalen. Elke schaaleenheid garandeert dat uw model 5 audiostream tegelijk kan verwerken. U kunt kopen van 100 schaaleenheden (of 500 gelijktijdige aanvragen).

Neem contact op als u meer dan die nodig hebt.

**Vraag**: Kan ik mijn model downloaden en lokaal uitvoeren?

**antwoord**: We schakelen niet modellen om te worden gedownload en lokaal worden uitgevoerd.

**Vraag**: Worden mijn aanvragen geregistreerd?

**antwoord**: U hebt een keuze tijdens het maken van een implementatie uitschakelen tracering in, waarna er geen audio- of transcripties worden geregistreerd. Anders aanvragen worden gewoonlijk is geregistreerd in Azure in veilige opslag. Als u nog meer vragen over privacy leiden die voorkomen dat u met behulp van de Custom Speech Service Neem contact met ons hebt.

## <a name="importing-data"></a>Gegevens importeren

**Vraag**: Wat is de limiet voor de grootte van de gegevensset? Hoe komt dat? 

**antwoord**: De huidige limiet voor een gegevensset is 2 GB, vanwege de beperking voor de grootte van een bestand voor HTTP uploaden. 

**Vraag**: Kan ik mijn tekstbestanden zip om een grotere tekstbestand uploaden 

**antwoord**: Nee, momenteel wordt alleen niet-gecomprimeerde tekstbestanden zijn toegestaan.

**Vraag**: Het gegevensrapport zegt er zijn mislukte uitingen. Is dit een probleem?

**antwoord**: Als er slechts een paar uitingen kunnen niet worden geïmporteerd, is dit geen probleem.
Als het overgrote deel van de uitingen in een akoestische of elke taal worden gegevens (bijvoorbeeld > 95%) correct zijn geïmporteerd, de gegevensset kan worden gebruikt. Het wordt echter aanbevolen dat u probeert te begrijpen waarom de uitingen is mislukt en de problemen kunt oplossen.
Meest voorkomende problemen, zoals het opmaken van fouten, zijn gemakkelijk op te lossen. 

## <a name="creating-am"></a>Het maken van AM

**Vraag**: Hoeveel akoestische gegevens heb ik nodig?

**antwoord**: Beste beginnen met 30 minuten tot een uur om akoestische gegevens

**Vraag**: Welk soort gegevens moet ik verzameld?

**antwoord**: U moet verzamelen van gegevens die zich zo dicht bij de toepassingsscenario en use-case mogelijk.
Dit betekent dat het verzamelen van gegevens moet overeenkomen met de doeltoepassing en gebruikers in termen van apparaten of apparaten, omgevingen en typen sprekers. In het algemeen dient u te verzamelen gegevens van als brede tal van sprekers mogelijk. 

**Vraag**: Hoe moet ik verzameld? 

**antwoord**: U kunt een zelfstandige gegevens verzameling-toepassing maken of sommige uit de audio-opnamen plank software gebruiken.
U kunt ook een versie van uw toepassing die de audio gegevens geregistreerd en gebruikt die maken. 

**Vraag**: Moet ik de gegevens van de aanpassing van woordenlijsten transcriberen zelf? 

**antwoord**: De gegevens moet worden omgezet. U kunt deze moet transcriberen zelf of een professionele transcriptie-service gebruiken. Sommige van deze typistes gebruik en andere gebruik crowdsourcing. Kan het beste ook een transcriptie-service op aanvraag.

**Vraag**: Hoe lang duurt het maken van een aangepast akoestisch model?

**antwoord**: De verwerkingstijd voor het maken van een aangepast akoestisch model is ongeveer hetzelfde als de lengte van de akoestische gegevensset.
Dus een aangepast akoestisch model gemaakt op basis van een gegevensset vijf uur duurt ongeveer vijf uur om te verwerken. 

## <a name="offline-testing"></a>Offline testen

**Vraag**: Kan ik de offline testen van mijn aangepast akoestisch model met behulp van een aangepast taalmodel uitvoeren?

**antwoord**: Ja, drukt u op het aangepaste taalmodel in de vervolgkeuzelijst bij het instellen van de offline testen

**Vraag**: Kan ik uitvoeren offline testen van mijn aangepaste taalmodel met behulp van een aangepast akoestisch model?

**antwoord**: Ja, selecteer de aangepast akoestisch model in de vervolgkeuzelijst bij het instellen van de offline-test.

**Vraag**: Wat is de Foutfrequentie Word en hoe wordt het berekend?

**antwoord**: Word Foutfrequentie is de evaluatie van metrische gegevens voor spraakherkenning. Als het totale aantal fouten, waaronder invoegingen, verwijderingen en vervangingen, gedeeld door het totale aantal woorden in de referentie-transcriptie wordt meegeteld.

**Vraag**: Ik nu de testresultaten van mijn aangepaste model weet, is dit een goede of slechte getal?

**antwoord**: De resultaten weergeven voor een vergelijking tussen de basislijn-model en de versie die u hebt aangepast.
U moet erop gericht zijn op maat van het model basislijn zodat aanpassing van de moeite waard maken

**Vraag**: Hoe ik achterhalen de Windows Foutrapportage van de basis modellen, zodat ik zien kan als er verbetering is? 

**antwoord**: De resultaten van de offline weergeven nauwkeurigheid van de nauwkeurigheid van de basislijn van het aangepaste model en het verbeteren na verloop van basislijn

## <a name="creating-lm"></a>Het maken van LM

**Vraag**: Hoeveel tekstgegevens heb ik nodig om te uploaden?

**antwoord**: Dit is een moeilijk vraag een nauwkeurige antwoord te geven, omdat deze afhankelijk hoe verschillende vocabulaire en zinnen die wordt gebruikt in uw toepassing afkomstig zijn van de eerste taalmodellen. Voor alle nieuwe woorden is het handig om zo veel voorbeelden bieden mogelijk van het gebruik van deze woorden. Voor veelvoorkomende zinnen die worden gebruikt in uw toepassing, is inclusief verbindingen in de gegevens van de taal ook handig als het systeem om te luisteren naar deze voorwaarden ook krijgt.
Het is gebruikelijk om ten minste één honderd en doorgaans enkele honderden uitingen in de gegevensset taal of meer.
Ook als bepaalde typen * van query's naar verwachting wordt vaker voor dan anderen, kunt u meerdere exemplaren van de algemene query's in de gegevensset.

**Vraag**: Kan ik een lijst met woorden alleen uploaden?

**antwoord**: Uploaden van een lijst met woorden de woorden in vocabulaire maar geen informatie over het systeem hoe de woorden die meestal worden gebruikt.
Het taalmodel kunt informatie over de nieuwe woorden en hoe ze worden gebruikt door te geven van de volledige of gedeeltelijke uitingen (zinnen bestaat van gebruikers maken zich waarschijnlijk in te spreken dingen). Het aangepaste taalmodel geldt niet alleen voor het ophalen van nieuwe woorden in het systeem, maar ook voor het aanpassen van de kans op bekende woorden voor uw toepassing. Volledige uitingen die helpt het systeem leert kennen. 

-----

 * [Overzicht](cognitive-services-custom-speech-home.md)
 * [Aan de slag](cognitive-services-custom-speech-get-started.md)
 * [Woordenlijst](cognitive-services-custom-speech-glossary.md)
