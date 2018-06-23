---
title: Veelgestelde vragen voor de aangepaste spraak-Service op Azure | Microsoft Docs
description: Hier vindt u antwoorden op de meest populaire vragen over de aangepaste spraak-Service.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: a929869b36387b3257b672308ceca36c84ff8cae
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344566"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Aangepaste spraak Service Veelgestelde vragen

Als u antwoorden op uw vragen in deze Veelgestelde vragen vinden kunt, probeert u het vragen van de aangepaste spraak Service-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) en [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Algemeen

**Vraag**: hoe weet ik wanneer de verwerking van mijn gegevensset of het model voltooid is?

**Antwoord**: de status van het model of de gegevensset in de tabel is momenteel alleen wilt weten.
Wanneer de verwerking voltooid is, zijn de status 'Gereed'.
We werken aan verbeterde methoden voor het verwerken van status, zoals e-mailmelding communicatie.

**Vraag**: kan ik meer dan één model tegelijk maken?

**Antwoord**: Er is geen limiet voor hoeveel modellen in uw verzameling zijn, maar slechts één op elke pagina tegelijk kan worden gemaakt.
U kunt bijvoorbeeld een proces voor het model taal maken niet starten als er momenteel een taalmodel in de procesfase.
U kunt wel een akoestisch model en een taalmodel tegelijk verwerken. 

**Vraag**: ik gerealiseerde ik een fout hebt gemaakt. Hoe ik mijn gegevens importeren annuleren of model maken die wordt uitgevoerd? 

**Antwoord**: momenteel u niet terugdraaien een acoustic of taal aanpassing-proces.
Geïmporteerde gegevens kunnen worden verwijderd nadat het importeren is voltooid

**Vraag**: Wat is het verschil tussen de zoekopdracht & dicteren modellen en de eigen modellen?

**Antwoord**: Er zijn twee Base akoestisch & modellen taal kiezen die in de aangepaste spraak-Service.
query's of dictation zoeken. De Microsoft-Conversational AM is geschikt voor het herkennen van spraak gesproken in een eigen stijl.
Dit type spraak is doorgaans gericht op een andere persoon, zoals in aanroep van afdelingen of vergaderingen.

**Vraag**: kan ik mijn bestaande model (model stapelen) bijwerken?

**Antwoord**: We bieden niet de mogelijkheid een bestaand model bijwerken met nieuwe gegevens.
Als u een nieuwe gegevensset hebt en u wilt aanpassen van een bestaand model moet u het opnieuw aanpassen met de nieuwe gegevens en de oude gegevensset die u hebt gebruikt.
De oude en nieuwe gegevenssets moet worden gecombineerd tot een enkel .zip (indien akoestisch gegevens) of een txt-bestand als dit eenmaal aanpassing language-gegevens wordt de nieuwe bijgewerkte model moet ongedaan geïmplementeerde verkrijgen van een nieuw eindpunt worden uitgevoerd

**Vraag**: Wat gebeurt er als ik gelijktijdigheid van hoger dan de standaardwaarde nodig. 

**Antwoord**: U kunt het model in stappen van 5 gelijktijdige aanvragen noemen we schaaleenheden opschalen. Elke schaaleenheid zorgt ervoor dat uw model 5 audiostroom tegelijk kan verwerken. U kunt kopen van 100 schaaleenheden (of 500 gelijktijdige aanvragen).

Neem contact met ons als u hoger dan die nodig hebt.

**Vraag**: kan ik mijn model downloaden en lokaal uitvoeren?

**Antwoord**: We modellen worden gedownload en lokaal uitgevoerd niet inschakelen.

**Vraag**: Mijn aanvragen die geregistreerd zijn?

**Antwoord**: U hebt de mogelijkheid tijdens het maken van een implementatie op dat moment geen audio tracering uitschakelen of transcriptie worden geregistreerd. Aanvragen worden anders meestal geregistreerd in Azure in de veilige opslag. Als u nog meer privacyproblemen verbieden met behulp van de aangepaste spraak Service Neem contact met ons hebt.

## <a name="importing-data"></a>Het importeren van gegevens

**Vraag**: wat de limiet voor de grootte van de gegevensset is? Hoe komt dat? 

**Antwoord**: de huidige limiet voor een gegevensset is 2 GB vereist, als gevolg van de beperking voor de grootte van een bestand voor het uploaden van HTTP. 

**Vraag**: kan ik mijn tekstbestanden zip om het uploaden van een grotere tekstbestand? 

**Antwoord**: Nee, momenteel alleen niet-gecomprimeerde tekstbestanden zijn toegestaan.

**Vraag**: het gegevensrapport staat er mislukte utterances zijn. Is dit een probleem?

**Antwoord**: als er slechts enkele utterances kunnen niet worden geïmporteerd, dit is geen probleem.
Als de grote meerderheid van de utterances in een gegevens- of taalinstellingen acoustic ingesteld (bijvoorbeeld > 95%) met succes worden geïmporteerd, de gegevensset kan worden gebruikt. Het is raadzaam dat u probeert te begrijpen waarom de utterances is mislukt en los de problemen.
Meest voorkomende problemen, zoals opmaakfouten, zijn gemakkelijk op te lossen. 

## <a name="creating-am"></a>AM maken

**Vraag**: hoeveel akoestisch gegevens heb ik nodig?

**Antwoord**: het is raadzaam beginnen met 30 minuten aan één uur akoestisch gegevens

**Vraag**: welk soort gegevens moet ik verzamelen?

**Antwoord**: U moet verzamelen van gegevens die zich zo dicht mogelijk bij het toepassingsscenario en gebruiksvoorbeeld mogelijk.
Dit betekent dat het verzamelen van gegevens moet overeenkomen met de doeltoepassing en gebruikers in termen van apparaat of apparaten, omgevingen en typen luidsprekers. In het algemeen dient u verzamelen gegevens van als brede tal van luidsprekers mogelijk. 

**Vraag**: hoe moet ik deze verzamelen? 

**Antwoord**: U kunt een zelfstandige gegevens verzameling toepassing maken of sommige uit de audio-opname rek software gebruiken.
U kunt ook een versie van uw toepassing waarmee de audiogegevens registreert en die gebruikmaakt van maken. 

**Vraag**: heb ik nodig transcriberen aanpassing gegevens zelf? 

**Antwoord**: de gegevens moet worden omgezet. U kunt transcriberen met gebruikmaking deze zelf of een professionele schrijffouten-service gebruiken. Sommige van deze typistes gebruik en andere crowdsourcing gebruiken. Ook kunt aangeraden een service schrijffouten op verzoek.

**Vraag**: hoe lang duurt om een aangepaste akoestisch model te maken?

**Antwoord**: de verwerkingstijd voor het maken van een aangepaste akoestisch model is over hetzelfde zijn als de lengte van de akoestisch gegevensset.
Dus een aangepaste akoestisch model gemaakt op basis van een gegevensset vijf uur duurt ongeveer vijf uur om te verwerken. 

## <a name="offline-testing"></a>Offline testen

**Vraag**: kan ik uitvoeren offline testen van mijn aangepaste akoestisch model gebruikmaken van een aangepaste taalmodel?

**Antwoord**: Ja, selecteer het taalmodel aangepaste in de vervolgkeuzelijst bij het instellen van de offline-test

**Vraag**: kan ik uitvoeren offline testen van mijn gebruikmaken van een aangepaste akoestisch model aangepaste taalmodel?

**Antwoord**: Ja, selecteer de aangepaste akoestisch model in de vervolgkeuzelijst bij het instellen van de offline-test.

**Vraag**: Wat is er Foutfrequentie Word en hoe wordt deze berekend?

**Antwoord**: Word Foutfrequentie is de metriek evaluatie voor spraakherkenning. Dit wordt beschouwd als het totale aantal fouten, waaronder invoegingen, verwijderingen en vervangingen, gedeeld door het totale aantal woorden in de schrijffouten verwijzing.

**Vraag**: ik nu de testresultaten van mijn aangepaste model weet, is dit een goede of ongeldige getal?

**Antwoord**: de resultaten blijkt een vergelijking tussen de basislijn-model en het u aangepast.
Het model van de basislijn om de aanpassingen waard tegen worden behaald

**Vraag**: hoe ik achterhalen de Windows Foutrapportage van de basis modellen, zodat ik zien kan als er verbetering is? 

**Antwoord**: de offline testresultaten nauwkeurigheid van de basislijn nauwkeurigheid van het aangepaste model en het verbeteren via basislijn weergeven

## <a name="creating-lm"></a>LM maken

**Vraag**: hoeveel tekstgegevens heb ik nodig voor het uploaden van?

**Antwoord**: dit is een moeilijk vraag een nauwkeurige antwoord te geven, zoals deze is afhankelijk van hoe verschillende vocabulair en woordgroepen gebruikt in uw toepassing uit de eerste taal modellen zijn. Voor alle nieuwe woorden is het nuttig om vindt u voorbeelden van zoveel mogelijk van het gebruik van deze woorden. Voor veelvoorkomende zinnen die worden gebruikt in uw toepassing, is inclusief verbindingen in de taal-gegevens ook handig als u krijgt het systeem om te luisteren naar deze voorwaarden eveneens.
Het is gebruikelijk om ten minste honderd en doorgaans enkele honderden utterances in de gegevensset taal of meer.
Ook als bepaalde typen * van query's naar verwachting wordt vaker dan andere, kunt u meerdere exemplaren van de algemene query's in de gegevensset.

**Vraag**: kan ik alleen uploaden een lijst met woorden?

**Antwoord**: het uploaden van een lijst met woorden zal woorden in woordenlijst maar geen informatie over het systeem hoe de woorden in die meestal worden gebruikt.
Dankzij de volledige of gedeeltelijke utterances (zinnen of zinnen van gebruikers, zullen in te spreken dingen) hebben het taalmodel informatie over de nieuwe woorden en hoe ze worden gebruikt. Het taalmodel aangepaste is goed niet alleen voor het ophalen van nieuwe woorden in het systeem, maar ook voor het aanpassen van de kans van bekende woorden voor uw toepassing. Volledige utterances bieden, kunt u het systeem meer informatie over dit. 

-----

 * [Overzicht](cognitive-services-custom-speech-home.md)
 * [Gestart](cognitive-services-custom-speech-get-started.md)
 * [Woordenlijst](cognitive-services-custom-speech-glossary.md)
