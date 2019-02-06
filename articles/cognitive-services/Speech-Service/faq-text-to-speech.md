---
title: Veelgestelde vragen over de Text to Speech-service in Azure
titleSuffix: Azure Cognitive Services
description: Vind antwoorden op de meest populaire vragen over de Text to Speech-service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 9b8340221c61bedbe010b9ef280ada62cfee7a21
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754865"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Veelgestelde vragen over de tekst naar spraak

Als u geen antwoorden op uw vragen in deze Veelgestelde vragen vinden, bekijk dan [andere ondersteuningsopties](support.md).

## <a name="general"></a>Algemeen

**V: Wat is het verschil tussen een standard voice-model en een aangepaste spraak-model?**

**A**: Het standaard voice-model (ook wel een *spraakstijl*) op basis van gegevens die eigendom is getraind en al is geïmplementeerd in de cloud. Een aangepaste spraak-model kunt u een gemiddelde model passen en de timbre en de expressie van de stijl van de stem van de spreker overzetten of een volledige, nieuwe model op basis van de trainingsgegevens voorbereid door de gebruiker te trainen. Vandaag de dag wilt meer en meer klanten voor hun bots een stem één van een soort, huisstijl. De aangepaste spraak-sestavuje SE platforma is de juiste keuze voor die optie.

**V: Waar moet ik beginnen als ik wil een standard voice-model gebruiken?**

**A**: Meer dan 80 standard voice-modellen in meer dan 45 talen zijn beschikbaar via HTTP-aanvragen. Haal eerst een [abonnementssleutel](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started). Als u de REST-aanroepen naar de predeployed voice-modellen, Zie de [REST-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

**V: Als ik wil een aangepaste spraak-model gebruiken, is de API hetzelfde als de waarde die wordt gebruikt voor standard stemmen?**

**A**: Als een aangepaste spraak-model wordt gemaakt en geïmplementeerd, krijgt u een unieke eindpunt voor uw model. Voor het gebruik van de stem te spreken in uw apps, moet u het eindpunt opgeven in de HTTP-aanvragen. Dezelfde functionaliteit die beschikbaar is in de REST-API voor de Text to Speech-service is beschikbaar voor uw aangepast eindpunt. Meer informatie over het [maken en gebruiken van uw aangepast eindpunt](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**V: Heb ik nodig om voor te bereiden van de trainingsgegevens aangepaste gesproken om modellen te maken op mijn eigen?**

**A**: Ja, u moet voorbereiden op de trainingsgegevens uzelf een aangepaste spraak-model.

Een verzameling spraakgegevens is vereist voor het maken van een aangepaste spraak-model. Deze verzameling bestaat uit een set van audio-bestanden van opnamen van spraak en een tekstbestand met de schrijffouten van elk bestand audio. Het resultaat van uw digitale stem is sterk afhankelijk van de kwaliteit van uw trainingsgegevens. Voor het produceren van een goede tekst-naar-spraak, is het belangrijk dat de opnamen die worden aangebracht in een stille ruimte met een hoge kwaliteit, permanente microfoon. Een consistente volume spreken snelheid en spreekstijl inspiratie en zelfs consistentie in expressieve gebaren van spraak zijn essentieel voor het bouwen van een geweldige digitale stem. Wij raden de stemmen op te nemen in een opname-studio.

We niet op dit moment bieden ondersteuning voor online registratie of registratie studio aanbevelingen hebt. Zie voor de vereiste indeling [voorbereiden opnamen en transcripties](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts).

**V: Welke scripts moet ik gebruiken om vast te leggen van de gegevens die via spraak voor aangepaste gesproken training?**

**A**: De scripts voor spraakopname is niet beperkt. U kunt uw eigen scripts gebruiken om vast te leggen van de gesproken tekst. Zorg dat u voldoende fonetische dekking in uw spraakgegevens hebt. Een aangepaste gesproken trainen, kunt u beginnen met een kleine hoeveelheid spraakgegevens, die mogelijk 50 verschillende zinnen (ongeveer 3 tot 5 minuten van spraak). Hoe meer gegevens die u opgeeft, hoe meer natuurlijke uw stem. U kunt beginnen met het trainen van een volledige spraakstijl wanneer u opnames van meer dan 2000 zinnen (ongeveer 3-4 uur van spraak) opgeeft. Als u wilt een hoge kwaliteit, de volledige stem, moet u voorbereiden opnames van meer dan 6000 zinnen (ongeveer 8-10 uur van spraak).

We bieden extra services om te helpen bij het voorbereiden van scripts voor de opname. Neem contact op met [aangepaste spraak-klantondersteuning](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) voor query's.

**V: Wat gebeurt er als ik hogere gelijktijdigheid dan de standaardwaarde of wat wordt aangeboden in de portal nodig?**

**A**: U kunt uw model in stappen van 20 gelijktijdige aanvragen opschalen. Neem contact op met [aangepaste spraak-klantondersteuning](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) voor vragen over het schalen van hogere.

**V: Kan ik mijn model downloaden en lokaal uitvoeren?**

**A**: Modellen niet kunnen worden gedownload en lokaal worden uitgevoerd.

**V: Zijn mijn aanvragen beperkt?**

**A**: De REST-API beperkt aanvragen voor 25 per 5 seconden. Meer informatie vindt u in onze pagina's voor [tekst naar spraak](text-to-speech.md). 

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
