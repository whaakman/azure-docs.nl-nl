---
title: Veelgestelde vragen over de spraak-naar-tekst-service in Azure
titleSuffix: Azure Cognitive Services
description: Vind antwoorden op de meest populaire vragen over de spraak-naar-tekst-service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 965d690184071da3f01ba165ccb0edd7e15f99b8
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225343"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Veelgestelde vragen over spraak naar tekst

Als u geen antwoorden op uw vragen in deze Veelgestelde vragen vinden, bekijk dan [andere ondersteuningsopties](support.md).

## <a name="general"></a>Algemeen

**V: Wat is het verschil tussen een basislijn-model en een aangepaste spraak naar tekst-model?**

**A**: Een basislijn-model is getraind met behulp van eigendom van Microsoft gegevens en al is geïmplementeerd in de cloud.  U kunt een aangepast model gebruiken om aan te passen van een model voor een betere aansluiting op een specifieke omgeving met specifieke omgevingsgeluid of taal. Factory verdiepingen, auto's of ruis Streets (straten) moet een aangepast akoestisch model. Onderwerpen zoals biologie, natuurkunde, radiologie, productnamen en aangepaste acroniemen moet een aangepast taalmodel.

**V: Waar moet ik beginnen als u wilt gebruiken een model basislijn?**

**A**: Haal eerst een [abonnementssleutel](get-started.md). Als u maken van de REST-aanroepen naar de modellen predeployed basislijn wilt, raadpleegt u de [REST-API's](rest-apis.md). Als u wilt gebruikmaken van WebSockets, [de SDK downloaden](speech-sdk.md).

**V: Heb ik altijd nodig om een aangepaste spraak-model te bouwen?**

**A**: Nee. Als uw toepassing gebruikmaakt van algemene, dagelijkse taal niet moet u een model aanpassen. Als uw toepassing in een omgeving wordt gebruikt wanneer er weinig of geen achtergrondgeluiden, hoeft u niet voor het aanpassen van een model.

U kunt implementeren basislijn en aangepaste modellen in de portal en vervolgens nauwkeurigheidstests uitgevoerd. U kunt deze functie gebruiken voor het meten van de nauwkeurigheid van een model basislijn ten opzichte van een aangepast model.

**V: Hoe weet ik wanneer de verwerking voor de gegevensset of het model is voltooid?**

**A**: De status van het model of de gegevensset in de tabel is momenteel de enige manier om te weten. Wanneer de verwerking voltooid is, wordt de status is **geslaagd**.

**V: Kan ik meer dan één model maken?**

**A**: Er is geen limiet voor het aantal modellen die u in uw verzameling hebben kunt.

**V: Ik gerealiseerde dat ik een fout hebt gemaakt. Hoe ik mijn gegevens importeren annuleren of model maken die wordt uitgevoerd?**

**A**: Op dit moment u kan niet worden teruggedraaid een aanpassing van woordenlijsten akoestische of taal proces. U kunt de geïmporteerde gegevens en modellen verwijderen wanneer ze zich in een definitieve status heeft.

**V: Wat is het verschil tussen de zoek- en spraakherkenning model en de conversatie model?**

**A**: U kunt kiezen uit meer dan één basislijn-model in de Speech-service. De conversatie model is handig voor het herkennen van spraak in een eigen stijl wordt gezegd. Dit model is ideaal voor telefoongesprekken te transcriberen. Het model voor zoeken en dicteren is ideaal voor apps stem wordt geactiveerd. Het universele model is een nieuw model dat is erop gericht voor beide scenario's. De universele model is momenteel of een hoger kwaliteitsniveau van de conversatie model in de meeste landen.

**V: Kan ik mijn bestaande model (model stapelen) bijwerken?**

**A**: U kunt een bestaand model niet bijwerken. Als een oplossing combineert de oude gegevensset met de nieuwe gegevensset en readapt.

De oude gegevensset en de nieuwe gegevensset moeten worden gecombineerd in één ZIP-bestand (voor akoestische gegevens) of in een txt-bestand (voor taal gegevens). Wanneer de aanpassing is voltooid, moet de nieuwe, bijgewerkte model worden geïmplementeerd om te verkrijgen van een nieuw eindpunt

**V: Als een nieuwe versie van een basislijn beschikbaar is, is mijn implementatie automatisch bijgewerkt?**

**A**: Implementaties wordt niet automatisch worden bijgewerkt.

Als u hebt aangepast en een model met basislijn V1.0 geïmplementeerd, blijft deze implementatie is. Klanten kunnen decommision het gedistribueerde model opnieuw aan te passen met behulp van de nieuwere versie van de basislijn en opnieuw implementeren.

**V: Wat gebeurt er als ik hogere gelijktijdigheid nodig voor mijn geïmplementeerde model dan wat wordt aangeboden in de portal?**

**A**: U kunt uw model in stappen van 20 gelijktijdige aanvragen opschalen.

Contact met ons opnemen als u een hogere schaal nodig hebt.

**V: Kan ik mijn model downloaden en lokaal uitvoeren?**

**A**: Modellen niet kunnen worden gedownload en lokaal worden uitgevoerd.

**V: Worden mijn aanvragen geregistreerd?**

**A**: U hebt een keuze bij het maken van een implementatie tracering uitschakelen. Op dat moment worden geen audio- of transcripties geregistreerd. Anders aanvragen worden gewoonlijk is geregistreerd in Azure in veilige opslag.

**V: Zijn mijn aanvragen beperkt?**

**A**: De REST-API beperkt aanvragen voor 25 per 5 seconden. Meer informatie vindt u in onze pagina's voor [spraak naar tekst](speech-to-text.md).

Als u nog meer vragen over privacy leiden die voorkomen dat u met behulp van de aangepaste spraakservice hebt, neem contact op met een van de ondersteuningskanalen.

## <a name="importing-data"></a>Gegevens importeren

**V: Wat is de limiet voor de grootte van een gegevensset en waarom is het de limiet?**

**A**: De huidige limiet voor een gegevensset is 2 GB. De limiet is vanwege de beperking voor de grootte van een bestand voor het uploaden van HTTP. 

**V: Kan ik mijn tekstbestanden zip zodat ik kan een grotere tekstbestand uploaden?** 

**A**: Nee. Op dit moment zijn alleen niet-gecomprimeerde tekstbestanden toegestaan.

**V: Het gegevensrapport zegt er zijn mislukte uitingen. Wat is het probleem?**

**A**: Failover-overschakeling naar 100 procent van de uitingen in een bestand uploaden is geen probleem. Als het overgrote deel van de uitingen in een gegevensset akoestische of taal (voor een voorbeeld, meer dan 95 procent) geïmporteerd zijn, kan de gegevensset worden gebruikt. We raden echter aan dat u probeert te begrijpen waarom de uitingen is mislukt en de problemen kunt oplossen. Meest voorkomende problemen, zoals het opmaken van fouten, zijn gemakkelijk op te lossen. 

## <a name="creating-an-acoustic-model"></a>Het maken van een akoestisch model

**V: Hoeveel akoestische gegevens heb ik nodig?**

**A**: U wordt aangeraden beginnen met tussen 30 minuten en één uur om akoestische gegevens.

**V: Welke gegevens moet ik verzameld?**

**A**: Verzamelen van gegevens die zich zo dicht bij de toepassingsscenario en use-case mogelijk. Het verzamelen van gegevens moet overeenkomen met de doeltoepassing en gebruikers in termen van apparaten of apparaten, omgevingen en typen sprekers. In het algemeen dient u te verzamelen gegevens van als brede tal van sprekers mogelijk. 

**V: Hoe moet ik om akoestische gegevens verzameld?**

**A**: U kunt een zelfstandige gegevens verzameling-toepassing maken of klare audio-opnamesoftware gebruiken. U kunt ook een versie van uw toepassing waarmee de audiogegevens-logboeken en gebruikt vervolgens de gegevens maken. 

**V: Moet ik de gegevens van de aanpassing van woordenlijsten transcriberen zelf?**

**A**: Ja. U kunt deze moet transcriberen zelf of een professionele transcriptie-service gebruiken. Sommige gebruikers liever typistes en anderen crowdsourcing gebruiken of de transcripties zelf doen.

## <a name="accuracy-testing"></a>Nauwkeurigheidstests

**V: Kan ik de offline testen van mijn aangepast akoestisch model met behulp van een aangepast taalmodel uitvoeren?**

**A**: Ja, selecteer de aangepaste taalmodel in de vervolgkeuzelijst bij het instellen van de offline-test.

**V: Kan ik uitvoeren offline testen van mijn aangepaste taalmodel met behulp van een aangepast akoestisch model?**

**A**: Ja, selecteer de aangepast akoestisch model in de vervolgkeuzelijst bij het instellen van de offline-test.

**V: Wat is word Foutfrequentie (Windows Foutrapportage) en hoe wordt het berekend?**

**A**: Windows Foutrapportage is de evaluatie van metrische gegevens voor spraakherkenning. Windows Foutrapportage wordt geteld als het totale aantal fouten, waaronder invoegingen, verwijderingen en vervangingen, gedeeld door het totale aantal woorden in de transcriptie verwijzing. Zie voor meer informatie, [word Foutfrequentie](https://en.wikipedia.org/wiki/Word_error_rate).

**V: Hoe bepaal ik of de resultaten van een test nauwkeurigheid goed zijn?**

**A**: De resultaten weergeven voor een vergelijking tussen de basislijn-model en het model dat u hebt aangepast. U moet erop gericht zijn op maat van het model basislijn zodat aanpassing waard.

**V: Hoe bepaal ik de Windows Foutrapportage van een basis-model, zodat ik zien kan als er een verbetering is?** 

**A**: De resultaten van de offline weergeven, de nauwkeurigheid van de basislijn van het aangepaste model en het verbeteren op basislijn.

## <a name="creating-a-language-model"></a>Het maken van een taalmodel

**V: Hoeveel tekstgegevens heb ik nodig om te uploaden?**

**A**: Dit is afhankelijk van hoe verschillende vocabulaire en zinnen die wordt gebruikt in uw toepassing afkomstig zijn van de eerste taalmodellen. Voor alle nieuwe woorden is het handig om zo veel voorbeelden bieden mogelijk van het gebruik van deze woorden. Voor algemene zinnen die worden gebruikt in uw toepassing, is met inbegrip van zinnen in de gegevens van de taal ook handig omdat deze geeft het systeem ook luisteren naar deze voorwaarden. Het is gebruikelijk om ten minste 100 en doorgaans enkele honderden of meer uitingen in de taal-gegevensset. Als bepaalde typen query's worden naar verwachting worden vaker voor dan anderen, kunt u meerdere exemplaren van de algemene query's invoegen in de gegevensset.

**V: Kan ik een lijst met woorden alleen uploaden?**

**A**: Uploaden van een lijst met woorden de woorden worden toegevoegd aan de woordenlijst, maar deze wordt niet het systeem leren hoe de woorden die meestal worden gebruikt. Dankzij de volledige of gedeeltelijke uitingen (zinnen bestaat die gebruikers maken zich waarschijnlijk in te spreken) hebben het taalmodel informatie over de nieuwe woorden en hoe ze worden gebruikt. Het aangepaste taalmodel geldt niet alleen voor het toevoegen van nieuwe woorden naar het systeem, maar ook voor het aanpassen van de kans op bekende woorden voor uw toepassing. Volledige uitingen die helpt het systeem meer beter. 

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
