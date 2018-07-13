---
title: Overzicht van Custom Vision Service Machine Learning - Azure Cognitive Services | Microsoft Docs
description: Custom Vision Service is een Microsoft Cognitive Service waarmee u aangepaste classificeerders voor afbeeldingen kunt bouwen op het Azure-platform.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: de45fc399470a806fb7456cbbe936cecf659ee7c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37087573"
---
# <a name="what-is-the-custom-vision-service"></a>Wat is Custom Vision Service?

Custom Vision Service is een Microsoft Cognitive Service waarmee u aangepaste classificeerders voor afbeeldingen kunt bouwen. Op deze manier kunt u eenvoudig en snel classificeerders voor afbeeldingen bouwen, implementeren en verbeteren. Custom Vision Service biedt een REST API en een webinterface om uw afbeeldingen te uploaden en de classificeerder te trainen.

## <a name="what-does-custom-vision-service-do-well"></a>Waar blinkt Custom Vision Service in uit?

Custom Vision Service werkt het beste wanneer het item dat uw wilt classificeren prominent aanwezig is in de afbeelding. 

Er is een aantal afbeeldingen vereist om een classificeerder of detector te maken. 50 afbeeldingen per classificatie zijn voldoende om een prototype te starten. De methoden waarvan wordt gebruikgemaakt in Custom Vision Service zijn niet erg gevoelig voor wijzigingen, waardoor u met zeer weinig gegevens kunt beginnen met het maken van een prototype. Dit betekent wel dat Custom Vision Service minder geschikt is voor scenario’s waarin u subtiele verschillen wilt detecteren. Bijvoorbeeld kleine foutjes in kwaliteitscontrolescenario’s.

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="may-7-2018"></a>7 mei 2018
- Preview-functie voor objectdetectie geïntroduceerd voor beperkte proefprojecten.
- Upgrade naar 2.0 API’s
- S0-laag is uitgebreid naar 250 labels en 50.000 afbeeldingen. 
- Belangrijke back-endverbeteringen aan de machine learning-pijplijn voor classificatieprojecten voor afbeeldingen. Projecten die zijn getraind na 27 april 2018, profiteren van deze updates.
- Modelexport naar ONNX toegevoegd voor gebruik met Windows ML.
- Modelexport naar DockerFile toegevoegd. Dit stelt u in staat om de artefacten te downloaden om uw eigen Windows- en Linux-containers te bouwen, inclusief een DockerFile, TensorFlow-model en servicecode. 
- Voor onlangs getrainde modellen die zijn geëxporteerd naar TensorFlow in de domeinen Algemeen (compact) en Oriëntatiepunt (compact), zijn de [Gemiddelde waarden nu (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), voor consistentie in alle projecten. 

### <a name="march-1-2018"></a>1 maart 2018
- Start van betaalde preview-versie en onboarding bij Azure Portal. Projecten kunnen nu worden gekoppeld aan Azure-resources met een F0-laag (gratis) of een S0-laag (Standard). Introductie van S0-laagprojecten die ruimte bieden voor maximaal 100 labels en 25.000 afbeeldingen. 
- Back-endwijzigingen in de machine learning-pijplijn/-normalisatieparameter. Hierdoor hebben klanten meer controle bij afwegingen over de precisie van de resultaten bij het aanpassen van de waarschijnlijkheidsdrempelwaarde. Als onderdeel van deze wijzigingen is de standaardwaarde van de waarschijnlijkheidsdrempel in de CustomVision.ai-portal ingesteld op 50%.

### <a name="december-19-2017"></a>19 december 2017

- Export naar Android (TensorFlow) is toegevoegd, naast de eerder uitgebrachte export naar iOS (CoreML). Hierdoor kan het exporteren van een getraind compact-model offline worden uitgevoerd in een toepassing.
- Domeinen Detailhandel en Oriëntatiepunt (compact) zijn toegevoegd om modelexports voor deze domeinen in te schakelen.
- Uitgebrachte versie [1.2 Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) en [1.1 Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Bijgewerkte API’s bieden ondersteuning voor modelexports, nieuwe voorspellingsbewerking waarmee afbeeldingen niet worden opgeslagen in Voorspellingen, en introductie van batchbewerkingen in de Training API.
- UX-aanpassingen, waaronder de mogelijkheid om te zien welk domein is gebruikt om een iteratie te trainen.
- Bijgewerkte [C# SDK en voorbeeld](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

## <a name="next-steps"></a>Volgende stappen

[Informatie over het bouwen van een classificeerder](getting-started-build-a-classifier.md)
