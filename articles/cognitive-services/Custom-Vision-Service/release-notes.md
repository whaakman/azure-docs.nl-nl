---
title: Opmerkingen bij de release - Custom Vision Service
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: ce48a240c67c6077027210ffb0ca85db93610782
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472953"
---
# <a name="custom-vision-service-release-notes"></a>Opmerkingen bij de Release van de Custom Vision Service

## <a name="march-26-2019"></a>26 maart 2019
- Custom Vision Service heeft ingevoerd van algemene beschikbaarheid op Azure! 
- Extra Training geavanceerde-functie met een nieuwe machine learning-back-end voor betere prestaties, met name op uitdaging zijn om gegevenssets en verfijnde classificatie. Met geavanceerde training, kunt u dat een compute-tijd budget voor training en aangepaste Vision experimenteel identificeert de aanbevolen instellingen voor training en uitbreiden van tokens. Voor snelle iteraties, kunt u echter ook doorgaan met de bestaande snelle training. 
- 3.0 geïntroduceerd API's. Komende afschaffing van pre-3.0-API's op 1 oktober 2019 aangekondigd. Zie de documentatie snelstartgidsen voor [.Net](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/csharp-tutorial), [Python](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/python-tutorial), [knooppunt](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/node-tutorial), [Java](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/java-tutorial), of [gaat](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/go-tutorial) voor voorbeelden hoe u aan de slag. 
- Vervangen "Iteraties standaard" voorzien van publiceren/publicatie ongedaan maken in de 3.0 API's. 
- Nieuw model exporteren doelen zijn toegevoegd. Docker-bestand exporteren is, ter ondersteuning van ARM voor Raspberry Pi 3 bijgewerkt. Ondersteuning voor het exporteren is toegevoegd aan de [Vision AI Dev Kit.](https://visionaidevkit.com/) 
- Verbeterde aantal Tags per project op 500 voor S0-laag. Verbeterde limiet van afbeeldingen per project 100.000 voor S0-laag.
- Volwassenen domein verwijderd. Algemene domein wordt aangeraden in plaats daarvan.
- Aangekondigd [prijzen](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/custom-vision-service/) voor algemene beschikbaarheid.  


## <a name="february-25-2019"></a>25 februari 2019
- Het einde van een beperkte proefversie projecten (die niet zijn gekoppeld aan een Azure-resource), aangekondigd als aangepaste Vision bijna te kiezen voor Azure preview-versie is voltooid. 25 maart 2019 vanaf ondersteunt de site CustomVision.ai alleen weergeven-projecten die zijn gekoppeld aan een Azure-resource, zoals de gratis Custom Vision-resource. Tot en met 1 oktober 2019 zult u nog steeds toegang tot uw bestaande beperkte proefversie projecten via de aangepaste Vision-API's. Hierdoor krijgt u veel tijd aan API-sleutels voor alle apps die u hebt die zijn geschreven met aangepaste Vision update. Na 1 oktober 2019, worden een beperkte proefversie projecten u dit nog niet hebt verplaatst naar Azure verwijderd.

## <a name="january-22-2019"></a>22 januari 2019
- Er is ondersteuning toegevoegd voor nieuwe Azure-regio's: VS-West 2, VS-Oost, VS-Oost 2, West-Europa, Noord-Europa, Zuidoost-Azië, Australië-Oost, centraal-India, UK-Zuid, Japan-Oost en Noord-centraal VS. Ondersteuning voor blijft voor Zuid-centraal VS. 

## <a name="december-12-2018"></a>12 december 2018
- Ondersteuning voor exporteren voor detectie van Object-modellen (geïntroduceerd Object detectie Compact domein).
- Een aantal toegankelijkheidsproblemen voor verbeterde schermlezer en ondersteuning voor toetsenbord navigatie opgelost. 
- UX-updates voor Afbeeldingsviewer en verbeterde objectdetectie ervaring voor snellere tagging tagging.  
- Bijgewerkte basismodel voor Object detectie domein voor de detectie van betere kwaliteit-object. 
- Bugfixes.

## <a name="november-6-2018"></a>6 november 2018
- Er is ondersteuning toegevoegd voor domein-Logo-Object wordt gedetecteerd.

## <a name="october-9-2018"></a>9 oktober 2018
- Objectdetectie van krijgt de betaalde Preview-versie. U kunt nu Objectdetectie projecten maken met een Azure-resource.
- 'Verplaatsen naar Azure'-functie toegevoegd aan, zodat u gemakkelijk om te upgraden van een beperkte proefversie project koppelen aan een Azure-website. resource in de gekoppelde project (F0 of S0). U kunt dit vinden op de pagina instellingen voor uw product.  
- Toegevoegde exporteren naar de ONNX-1.2, ter ondersteuning van de Update van oktober 2018 Windows-versie van Windows-ML.
Bugfixes, met inbegrip van ONNX exporteren met speciale tekens. 

## <a name="august-14-2018"></a>14 augustus 2018
- Toegevoegde 'Aan de slag' widget naar site customvision.ai gebruikers begeleidt bij project training. 
- Verdere verbeteringen aan de machine learning-pijplijn om te profiteren van multilabel projecten (nieuwe verlies laag).

## <a name="june-28-2018"></a>28 juni 2018
- Verbeteringen in bugfixes & back-end.
- Multiklassen indeling, voor projecten waar afbeeldingen precies één label hebt ingeschakeld. In voorspellingen voor multiklassen modus kansen zal oplopen tot een (alle installatiekopieën worden geclassificeerd onder de opgegeven labels).

## <a name="june-13-2018"></a>13 juni 2018
- UX vernieuwen, gericht op gebruiksgemak en toegankelijkheid. 
- Verbeteringen aan de machine learning-pijplijn om te profiteren van multilabel projecten met een groot aantal tags.
- Bug verholpen in TensorFlow exporteren. Geëxporteerde model versiebeheer, ingeschakeld zodat iteraties kunnen meer dan één keer worden geëxporteerd. 

## <a name="may-7-2018"></a>7 mei 2018
- Preview-functie voor objectdetectie geïntroduceerd voor beperkte proefprojecten.
- Upgrade naar 2.0 API’s
- S0-laag is uitgebreid naar 250 labels en 50.000 afbeeldingen. 
- Belangrijke back-endverbeteringen aan de machine learning-pijplijn voor classificatieprojecten voor afbeeldingen. Projecten die zijn getraind na 27 april 2018, profiteren van deze updates.
- Modelexport naar ONNX toegevoegd voor gebruik met Windows ML.
- Modelexport naar DockerFile toegevoegd. Dit stelt u in staat om de artefacten te downloaden om uw eigen Windows- en Linux-containers te bouwen, inclusief een DockerFile, TensorFlow-model en servicecode. 
- Voor nieuwe getrainde modellen die zijn geëxporteerd naar TensorFlow in het algemeen (cd) en oriëntatiepunt (cd) domeinen, [betekenen waarden zijn nu (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), voor consistentie in alle projecten. 

## <a name="march-1-2018"></a>1 maart 2018
- Start van betaalde preview-versie en onboarding bij Azure Portal. Projecten kunnen nu worden gekoppeld aan Azure-resources met een F0-laag (gratis) of een S0-laag (Standard). Introductie van S0-laagprojecten die ruimte bieden voor maximaal 100 labels en 25.000 afbeeldingen. 
- Back-endwijzigingen in de machine learning-pijplijn/-normalisatieparameter. Hierdoor hebben klanten meer controle bij afwegingen over de precisie van de resultaten bij het aanpassen van de waarschijnlijkheidsdrempelwaarde. Als onderdeel van deze wijzigingen is de standaardwaarde van de waarschijnlijkheidsdrempel in de CustomVision.ai-portal ingesteld op 50%.

## <a name="december-19-2017"></a>19 december 2017

- Export naar Android (TensorFlow) is toegevoegd, naast de eerder uitgebrachte export naar iOS (CoreML). Hierdoor kan het exporteren van een getraind compact-model offline worden uitgevoerd in een toepassing.
- Domeinen Detailhandel en Oriëntatiepunt (compact) zijn toegevoegd om modelexports voor deze domeinen in te schakelen.
- Uitgebrachte versie [1.2 Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) en [1.1 Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Bijgewerkte API’s bieden ondersteuning voor modelexports, nieuwe voorspellingsbewerking waarmee afbeeldingen niet worden opgeslagen in Voorspellingen, en introductie van batchbewerkingen in de Training API.
- UX-aanpassingen, waaronder de mogelijkheid om te zien welk domein is gebruikt om een iteratie te trainen.
- Bijgewerkte [C# SDK en voorbeeld](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

