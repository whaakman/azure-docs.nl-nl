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
ms.openlocfilehash: 4a29435c0ace79fc3a5d3a5a42a0e91bdbc8da5e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082821"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Text To Speech Veelgestelde vragen

Als u antwoorden op uw vragen in deze Veelgestelde vragen vinden kunt, probeert u het vragen van de aangepaste spraak Service-community op [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) en [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Algemeen

**Vraag**: Wat is het verschil tussen de standaard- en aangepaste voice-modellen?

**Antwoord**: de standaard stem (ook modellen Voice lettertypen) met Microsoft die eigendom zijn van de gegevens hebben getraind en al zijn geïmplementeerd in de cloud. Aangepaste gesproken modellen kunnen de gebruiker naar een gemiddelde model passen en de timbre en de wijze expressie volgens de spreker stem stijl brengen of te trainen van een volledig nieuw model op basis van de trainingsgegevens voorbereid door de gebruiker. Meer klanten willen vandaag een stem één van een soort, huisstijl voor hun bots hebben. De aangepaste stem platform bouwen is voor dat de juiste keuze.

**Vraag**: waar moet ik beginnen als ik wil een standaard voice-model gebruiken?

**Antwoord**: meer dan 80 standaard voice-modellen in meer dan 45 talen zijn beschikbaar via HTTP-aanvragen. Eerst moet u een [abonnementssleutel](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Als u de REST-aanroepen naar de vooraf geïmplementeerde voice-modellen, raadpleegt u de [hier om details](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Vraag**: als ik wil een aangepaste stemberichten-model te gebruiken, is de API hetzelfde zijn als de standaard stemmen?

**Antwoord**: wanneer u het model voor aangepaste gesproken gemaakt en geïmplementeerd hebt, krijgt u een unieke eindpunt voor het model. U moet het eindpunt opgeven in de HTTP-aanvragen voor het gebruik van de stem te spreken in uw apps. Dezelfde functionaliteit die beschikbaar is via de REST-API voor de TTS-service is ook beschikbaar voor uw aangepaste eindpunt. Zie hoe [maken en gebruiken van uw aangepaste eindpunt](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Vraag**: heb ik nodig voor het voorbereiden van de trainingsgegevens voor het maken van aangepaste gesproken modellen zelf?

**Antwoord**: U moet de trainingsgegevens voorbereiden voor uzelf. Een verzameling spraakgegevens is vereist om een aangepaste stemberichten-model te maken. Deze verzameling bestaat uit een reeks audio-bestanden van spraak opnamen en een tekstbestand van de schrijffouten van elk bestand audio. Het resultaat van uw digitale stem is sterk afhankelijk van de kwaliteit van uw trainingsgegevens. Een goede TTS-stem produceren, is het belangrijk dat de opnamen in een stille lokaal worden uitgevoerd met een hoge kwaliteit permanente microfoon. Consistente volume spreken frequentie, spreken presentatie en zelfs consistentie in expressieve gebaren van spraak zijn essentieel voor het bouwen van een geweldige digitale stem. Het is raadzaam dat u de stemmen vastgelegd in een studio opnemen hebt.
Op het moment dat we geen bieden ondersteuning voor on line opname of geen opname studio aanbevelingen. Zie voor de vereiste indeling [opnamen en transcripties voorbereiden](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts)
 
**Vraag**: welke scripts moet ik gebruiken voor het vastleggen van de spraakgegevens voor aangepaste gesproken training? 

**Antwoord**: We de scripts voor spraakopname niet beperken. U kunt uw eigen scripts gebruiken om vast te leggen van de spraak. NET zorg voor voldoende dekking fonetische in uw spraakgegevens. Om een aangepaste gesproken training, kunt u starten met een kleine hoeveelheid spraakgegevens, die kan 50 verschillende zinnen worden (over 3-5 minuten van spraak). Hoe meer gegevens die u opgeeft, hoe meer natuurlijke uw stem. U kunt beginnen met een volledige voice-lettertype trainen wanneer u opnamen van meer dan 2000 zinnen (ongeveer 3 en 4 uur spraak) opgeven. Als u een volledige stem van hoge kwaliteit, moet u opnamen van meer dan 6000 zinnen (ongeveer 8-10 uur spraak) voorbereiden.  
We bieden extra services waarmee u scripts voorbereiden voor vastleggen van gegevens. Neem contact op met [klantondersteuning voor aangepaste gesproken](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) voor query's.

**Vraag**: Wat gebeurt er als ik gelijktijdigheid van hoger dan de standaardwaarde moet of wat wordt aangeboden in de portal?

**Antwoord**: U kunt opschalen van het model in stappen van 20 gelijktijdige aanvragen. Neem contact op met [klantondersteuning voor aangepaste gesproken](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) voor query's op de hogere schaalbaarheid.

**Vraag**: kan ik mijn model downloaden en lokaal uitvoeren?

**Antwoord**: modellen niet kunnen worden gedownload en lokaal worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

* [Problemen oplossen](troubleshooting.md)
* [Releaseopmerkingen](releasenotes.md)
