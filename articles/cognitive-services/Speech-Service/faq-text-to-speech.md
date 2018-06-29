---
title: Veelgestelde vragen over spraak naar tekstservice in Azure | Microsoft Docs
description: Hier vindt u antwoorden op de meest populaire vragen over de spraak naar tekst.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 64e505889ef9472603471d67a961985c1290663a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045840"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Aangepaste spraak Service Veelgestelde vragen

Als u antwoorden op uw vragen in deze Veelgestelde vragen vinden kunt, probeert u het vragen van de aangepaste spraak Service-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) en [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Algemeen

**Vraag**: Wat is het verschil tussen de basislijn en aangepaste spraak naar tekst modellen?

**Antwoord**: basislijn modellen zijn getraind met Microsoft die eigendom zijn van gegevens en al zijn geïmplementeerd in de cloud. Aangepaste modellen kunnen de gebruiker aan te passen aan een model aanpassen aan een bepaalde omgeving met specifieke ambient ruis of taal. Factory grond verdieping, auto's, ruis streets zou moeten worden aangepast akoestisch model dat bepaalde onderwerpen zoals biologie, fysieke radiologie, productnamen en aangepaste acroniemen een taalmodel vereist.

**Vraag**: waar moet ik beginnen als ik wil een basislijn-model gebruiken?

**Antwoord**: eerst moet u een [abonnementssleutel](get-started.md). Als u maken van de REST-aanroepen naar de modellen predeployed basislijn wilt, raadpleegt u de [hier om details](rest-apis.md). Als u wilt gebruiken WebSockets downloaden de [SDK](speech-sdk.md)

**Vraag**: kan ik moet altijd een aangepaste spraak model bouwen?

**Antwoord**: Nee, als uw toepassing met behulp van algemene dagelijkse taal zonder aangepaste vocabulaire of die mogelijk zeldzame vervolgens niet hoeft u voor het aanpassen van een model. Bovendien als uw toepassing moet worden gebruikt in een omgeving waar er weinig of geen achtergrondruis wordt u niet nodig om aan te passen ofwel. De portal kan gebruikers te implementeren basislijn en aangepaste modellen en nauwkeurigheid tests uitgevoerd. Gebruikers kunnen deze functie gebruiken voor het meten van de nauwkeurigheid van een basislijn tegenover een aangepaste model.

**Vraag**: hoe weet ik wanneer de verwerking van mijn gegevensset of het model voltooid is?

**Antwoord**: de status van het model of de gegevensset in de tabel is momenteel alleen wilt weten.
Wanneer de verwerking voltooid is, zijn de status 'Gereed'.

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

**Antwoord**: bestaande modellen kunnen niet worden bijgewerkt. De oude gegevensset worden gecombineerd met de nieuwe en readapt als tijdelijke oplossing.

De oude en nieuwe gegevenssets moet worden gecombineerd in één Zip-(indien akoestisch gegevens) of een txt-bestand als dit language-gegevens. Aanpassing de nieuwe bijgewerkte model moet worden opgeheven geïmplementeerde verkrijgen van een nieuw eindpunt is gedaan

**Vraag**: Wat gebeurt er als ik gelijktijdigheid van hoger dan de standaardwaarde moet of in de portal wordt geboden. 

**Antwoord**: U kunt opschalen van het model in stappen van 20 gelijktijdige aanvragen. 

Contact met ons opnemen als u een hogere schaal vereist.

**Vraag**: kan ik mijn model downloaden en lokaal uitvoeren?

**Antwoord**: modellen niet kunnen worden gedownload en lokaal worden uitgevoerd.

**Vraag**: Mijn aanvragen die geregistreerd zijn?

**Antwoord**: U hebt de mogelijkheid tijdens het maken van een implementatie op dat moment geen audio tracering uitschakelen of transcriptie worden geregistreerd. Aanvragen worden anders meestal geregistreerd in Azure in de veilige opslag. Als u meer privacyproblemen verbieden met de Service voor aangepaste spraak hebt, contact op met een van de ondersteuningskanalen.

## <a name="importing-data"></a>Het importeren van gegevens

**Vraag**: wat de limiet voor de grootte van de gegevensset is? Hoe komt dat? 

**Antwoord**: de huidige limiet voor een gegevensset is 2 GB vereist, als gevolg van de beperking voor de grootte van een bestand voor het uploaden van HTTP. 

**Vraag**: kan ik mijn tekstbestanden zip om het uploaden van een grotere tekstbestand? 

**Antwoord**: Nee, momenteel alleen niet-gecomprimeerde tekstbestanden zijn toegestaan.

**Vraag**: het gegevensrapport staat er mislukte utterances zijn. Wat is het probleem?

**Antwoord**: mislukt voor het uploaden van 100% van de utterances in een bestand is geen probleem.
Als de grote meerderheid van de utterances in een gegevens- of taalinstellingen acoustic ingesteld (bijvoorbeeld > 95%) met succes worden geïmporteerd, de gegevensset kan worden gebruikt. Het is raadzaam dat u probeert te begrijpen waarom de utterances is mislukt en los de problemen. Meest voorkomende problemen, zoals opmaakfouten, zijn gemakkelijk op te lossen. 

## <a name="creating-am"></a>AM maken

**Vraag**: hoeveel akoestisch gegevens heb ik nodig?

**Antwoord**: het is raadzaam beginnen met 30 minuten aan één uur akoestisch gegevens

**Vraag**: welke gegevens moet ik verzamelen?

**Antwoord**: verzamelen van gegevens die zich zo dicht mogelijk bij het toepassingsscenario en gebruiksvoorbeeld mogelijk.
Het verzamelen van gegevens moet overeenkomen met de doeltoepassing en gebruikers in termen van apparaat of apparaten, omgevingen en typen luidsprekers. In het algemeen dient u verzamelen gegevens van als brede tal van luidsprekers mogelijk. 

**Vraag**: hoe moet ik deze verzamelen? 

**Antwoord**: U kunt een zelfstandige gegevens verzameling toepassing maken of sommige uit de audio-opname rek software gebruiken.
U kunt ook een versie van uw toepassing waarmee de audiogegevens registreert en die gebruikmaakt van maken. 

**Vraag**: heb ik nodig transcriberen aanpassing gegevens zelf? 

**Antwoord**: de gegevens moet worden omgezet. U kunt transcriberen met gebruikmaking deze zelf of een professionele schrijffouten-service gebruiken. Sommige van deze typistes gebruik en andere crowdsourcing gebruiken.

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

**Vraag**: hoe bepaal ik als de resultaten van een test nauwkeurigheid is het verstandig?

**Antwoord**: de resultaten blijkt een vergelijking tussen de basislijn-model en het u aangepast.
Het model van de basislijn om de aanpassingen waard tegen worden behaald

**Vraag**: hoe ik achterhalen de Windows Foutrapportage van de basis modellen, zodat ik zien kan als er verbetering is? 

**Antwoord**: de offline testresultaten nauwkeurigheid van de basislijn nauwkeurigheid van het aangepaste model en het verbeteren via basislijn weergeven

## <a name="creating-lm"></a>LM maken

**Vraag**: hoeveel tekstgegevens heb ik nodig voor het uploaden van?

**Antwoord**: dit is afhankelijk van hoe verschillende vocabulair en woordgroepen gebruikt in uw toepassing uit de eerste taal modellen zijn. Voor alle nieuwe woorden is het nuttig om vindt u voorbeelden van zoveel mogelijk van het gebruik van deze woorden. Voor veelvoorkomende zinnen die worden gebruikt in uw toepassing, is met inbegrip van items in de taal-gegevens ook nuttig als u krijgt het systeem om te luisteren naar deze voorwaarden eveneens. Het is gebruikelijk om ten minste één 100 en doorgaans enkele honderden utterances in de gegevensset taal of meer. Als u bepaalde typen query's naar verwachting wordt vaker dan andere, kunt u meerdere exemplaren van de algemene query's ook invoegen in de gegevensset.

**Vraag**: kan ik alleen uploaden een lijst met woorden?

**Antwoord**: het uploaden van een lijst met woorden zal woorden in woordenlijst maar geen informatie over het systeem hoe de woorden in die meestal worden gebruikt.
Dankzij de volledige of gedeeltelijke utterances (zinnen of zinnen van gebruikers, zullen in te spreken dingen) het taalmodel kan informatie over de nieuwe woorden en hoe ze worden gebruikt. Het taalmodel aangepaste is goed niet alleen voor het ophalen van nieuwe woorden in het systeem, maar ook voor het aanpassen van de kans van bekende woorden voor uw toepassing. Volledige utterances bieden, kunt u het systeem beter leren. 

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
