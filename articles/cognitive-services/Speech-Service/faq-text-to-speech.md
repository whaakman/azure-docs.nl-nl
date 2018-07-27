---
title: Veelgestelde vragen voor spraak-naar-tekst-Service op Azure
description: Hier vindt u antwoorden op de meest populaire vragen over de spraak-naar-tekst.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 8d70c4a359c713d6c5f46423193e9c9e7e1f3baf
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282854"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Veelgestelde vragen over de tekst naar spraak

Als u geen antwoorden op uw vragen in deze Veelgestelde vragen vinden, bekijk dan andere ondersteuningsopties [hier](support.md).

## <a name="general"></a>Algemeen

**Vraag**: Wat is het verschil tussen de standaardentiteiten en aangepaste spraak-modellen?

**Antwoord**: de standard-voice-modellen (ook wel) spraakstijlen) zijn getraind met gegevens eigendom zijn van Microsoft en al zijn geïmplementeerd in de cloud. Aangepaste spraak modellen kunnen de gebruiker op een gemiddelde model passen en de timbre en de wijze van de expressie op basis van de spreker stem stijl brengen of met het trainen van een volledig nieuw model op basis van de trainingsgegevens voorbereid door de gebruiker. Meer en meer klanten willen vandaag nog een stem één van een soort, huisstijl voor hun bots nog geavanceerder zijn. De aangepaste stem het bouwen van platform is voor dat de juiste keuze.

**Vraag**: waar moet ik beginnen als ik wil een standard voice-model gebruiken?

**Antwoord**: meer dan 80 standard voice-modellen in meer dan 45 talen zijn beschikbaar via HTTP-aanvragen. U moet eerst aan een [abonnementssleutel](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Als u de REST-aanroepen naar de vooraf geïmplementeerde voice-modellen, raadpleegt u de [hier details](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Vraag**: als ik wil gebruiken een aangepaste spraak-model, de API hetzelfde is als de standaard stemmen?

**Antwoord**: wanneer u het aangepaste spraak-model gemaakt en geïmplementeerd, krijgt u een unieke eindpunt voor uw model. U moet het eindpunt opgeven in de HTTP-aanvragen, het gebruik van de stem te spreken in uw apps. Dezelfde functionaliteit die beschikbaar is via de REST-API voor de Text to Speech-service is ook beschikbaar voor uw aangepast eindpunt. Zie hoe u [maken en gebruiken van uw aangepast eindpunt](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Vraag**: heb ik nodig om voor te bereiden van de trainingsgegevens voor het maken van aangepaste gesproken modellen op mijn eigen?

**Antwoord**: U moet de trainingsgegevens voorbereiden voor uzelf. Een verzameling spraakgegevens is vereist voor het maken van een aangepaste spraak-model. Deze verzameling bestaat uit een set van audio-bestanden van opnamen van spraak en een tekstbestand met de schrijffouten van elk bestand audio. Het resultaat van uw digitale stem is sterk afhankelijk van de kwaliteit van uw trainingsgegevens. Als u wilt een goede TTS stem produceren, is het belangrijk dat de opnamen in een stille ruimte klaar bent met een hoge kwaliteit permanente microfoon. Consistente volume, prijs, spreekstijl inspiratie en zelfs consistentie in expressieve gebaren van spraak spreken zijn essentieel voor het bouwen van een geweldige digitale stem. Het is raadzaam dat u de stemmen vastgelegd in een opname-studio hebt.
Op het moment dat we geen bieden ondersteuning voor online opname of hebben geen opname studio aanbevelingen. Zie voor de vereiste indeling [opnamen en transcripties voorbereiden](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts)
 
**Vraag**: welke scripts moet ik gebruiken voor het vastleggen van de gegevens die via spraak voor aangepaste gesproken training? 

**Antwoord**: We beperken niet de scripts voor opname van spraak. U kunt uw eigen scripts gebruiken om vast te leggen van de gesproken tekst. Zorg er voldoende dekking fonetische in uw spraakgegevens. Het trainen van een aangepaste spraak, kunt u beginnen met een klein volume van spraakgegevens, wat erop kan 50 verschillende zinnen (over 3 tot 5 minuten van spraak). Hoe meer gegevens die u opgeeft, hoe meer natuurlijke uw stem. U kunt beginnen met het trainen van een volledige spraakstijl wanneer u opnames van meer dan 2000 zinnen (ongeveer 3-4 uur van spraak) opgeeft. Als u een volledige stem van hoge kwaliteit, moet u om voor te bereiden opnames van meer dan 6000 zinnen (ongeveer 8-10 uur van spraak).  
We bieden extra services om te helpen bij het voorbereiden van scripts voor de opname. Neem contact op met [aangepaste spraak-klantondersteuning](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) voor query's.

**Vraag**: Wat gebeurt er als ik nodig heb hogere gelijktijdigheid dan de standaardwaarde of wat wordt aangeboden in de portal?

**Antwoord**: U kunt uw model in stappen van 20 gelijktijdige aanvragen opschalen. Neem contact op met [aangepaste spraak-klantondersteuning](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) voor query's voor hogere schalen.

**Vraag**: kan ik mijn model downloaden en lokaal uitvoeren?

**Antwoord**: modellen niet kunnen worden gedownload en lokaal worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
