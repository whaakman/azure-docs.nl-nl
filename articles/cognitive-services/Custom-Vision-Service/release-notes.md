---
title: Opmerkingen bij de Release van de Custom Vision Service
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.date: 08/28/2018
ms.author: anroth
ms.openlocfilehash: 8423051fa5169eb8acddc7297e36188e297cb9e4
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288671"
---
# <a name="custom-vision-service-release-notes"></a>Opmerkingen bij de Release van de Custom Vision Service

## <a name="august-14-2018"></a>14 augustus 2018
- Toegevoegde 'Aan de slag' widget naar site customvision.ai gebruikers begeleidt bij project training. 
- Verdere verbeteringen aan de machine learning-pijplijn om te profiteren van multilabel projecten (nieuwe verlies laag).

## <a name="june-28-2018"></a>28 juni 2018
- Verbeteringen in bugfixes & back-end.
- Enabeled Multiklasse classificatie voor projecten waar afbeeldingen precies één label hebben. In voorspellingen voor multiklassen modus kansen zal oplopen tot een (alle installatiekopieën worden geclassificeerd onder de opgegeven labels).

## <a name="june-13-2018"></a>13 juni 2018
- UX vernieuwen, gericht op gebruiksgemak en toegankelijkheid. 
- Verbeteringen aan de machine learning-pijplijn om te profiteren van multilabel projecten met een groot aantal tags.
- Bug verholpen in TensorFlow exporteren. Enabeled geëxporteerd model versiebeheer, zodat iteraties kunnen meer dan één keer worden geëxporteerd. 

## <a name="may-7-2018"></a>7 mei 2018
- Preview-functie voor objectdetectie geïntroduceerd voor beperkte proefprojecten.
- Upgrade naar 2.0 API’s
- S0-laag is uitgebreid naar 250 labels en 50.000 afbeeldingen. 
- Belangrijke back-endverbeteringen aan de machine learning-pijplijn voor classificatieprojecten voor afbeeldingen. Projecten die zijn getraind na 27 april 2018, profiteren van deze updates.
- Modelexport naar ONNX toegevoegd voor gebruik met Windows ML.
- Modelexport naar DockerFile toegevoegd. Dit stelt u in staat om de artefacten te downloaden om uw eigen Windows- en Linux-containers te bouwen, inclusief een DockerFile, TensorFlow-model en servicecode. 
- Voor onlangs getrainde modellen die zijn geëxporteerd naar TensorFlow in de domeinen Algemeen (compact) en Oriëntatiepunt (compact), zijn de [Gemiddelde waarden nu (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), voor consistentie in alle projecten. 

## <a name="march-1-2018"></a>1 maart 2018
- Start van betaalde preview-versie en onboarding bij Azure Portal. Projecten kunnen nu worden gekoppeld aan Azure-resources met een F0-laag (gratis) of een S0-laag (Standard). Introductie van S0-laagprojecten die ruimte bieden voor maximaal 100 labels en 25.000 afbeeldingen. 
- Back-endwijzigingen in de machine learning-pijplijn/-normalisatieparameter. Hierdoor hebben klanten meer controle bij afwegingen over de precisie van de resultaten bij het aanpassen van de waarschijnlijkheidsdrempelwaarde. Als onderdeel van deze wijzigingen is de standaardwaarde van de waarschijnlijkheidsdrempel in de CustomVision.ai-portal ingesteld op 50%.

## <a name="december-19-2017"></a>19 december 2017

- Export naar Android (TensorFlow) is toegevoegd, naast de eerder uitgebrachte export naar iOS (CoreML). Hierdoor kan het exporteren van een getraind compact-model offline worden uitgevoerd in een toepassing.
- Domeinen Detailhandel en Oriëntatiepunt (compact) zijn toegevoegd om modelexports voor deze domeinen in te schakelen.
- Uitgebrachte versie [1.2 Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) en [1.1 Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Bijgewerkte API’s bieden ondersteuning voor modelexports, nieuwe voorspellingsbewerking waarmee afbeeldingen niet worden opgeslagen in Voorspellingen, en introductie van batchbewerkingen in de Training API.
- UX-aanpassingen, waaronder de mogelijkheid om te zien welk domein is gebruikt om een iteratie te trainen.
- Bijgewerkte [C# SDK en voorbeeld](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

