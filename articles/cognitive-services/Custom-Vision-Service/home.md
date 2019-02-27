---
title: Wat is Azure Custom Vision?
titlesuffix: Azure Cognitive Services
description: Leer hoe u de Custom Vision-service kunt gebruiken om aangepaste classificaties voor afbeeldingen te bouwen in de Azure-cloud.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 6cbc6e351147ed5b4c31463b5cf319417f34da34
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56456772"
---
# <a name="what-is-azure-custom-vision"></a>Wat is Azure Custom Vision?

Azure Custom Vision is een cognitieve service waarmee u uw eigen classificaties voor afbeeldingen kunt bouwen, implementeren en verbeteren. Een afbeeldingsclassificatie is een AI-service die op basis van de visuele kenmerken van een afbeelding labels (die _klassen_ vertegenwoordigen) op afbeeldingen toepast. Anders dan bij de [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)-service kunt u met Custom Vision bepalen welke labels u wilt toepassen.

## <a name="what-it-does"></a>Wat het doet

De Custom Vision-service gebruikt een machine learning-algoritme om labels toe te passen op afbeeldingen. U, als ontwikkelaar, moet groepen afbeeldingen indienen die de betreffende kenmerken al dan niet hebben. U voorziet de afbeeldingen op het moment van inzending zelf van een label. Vervolgens wordt het algoritme getraind op basis van deze gegevens en berekent het algoritme de eigen nauwkeurigheid aan de hand van deze zelfde gegevens. Wanneer het algoritme is getraind, kunt u het model testen, opnieuw trainen en uiteindelijk gebruiken om nieuwe afbeeldingen te classificeren op basis van de behoeften van de app. U kunt het model zelf ook exporteren voor offlinegebruik.

## <a name="classification-and-object-detection"></a>Classificatie- en objectdetectie

Custom Vision-functionaliteit kan worden onderverdeeld in twee functies. Met **afbeeldingsclassificatie** worden een of meer labels toegepast op een afbeelding. **Objectdetectie** is vergelijkbaar, maar hiermee worden ook de coördinaten van de afbeelding geretourneerd waar de toegepaste labels kunnen worden gevonden.

## <a name="optimization"></a>Optimalisatie

De Custom Vision-service is geoptimaliseerd om snel belangrijke verschillen tussen afbeeldingen te herkennen. Op die manier kunt u uw model gaan prototypen met een kleine hoeveelheid gegevens. 50 afbeeldingen per label is meestal al een goed begin. Dit betekent echter dat de service niet optimaal geschikt is voor het detecteren van subtiele verschillen in afbeeldingen (bijvoorbeeld het detecteren van kleine fouten in scenario’s voor kwaliteitscontrole).

Daarnaast kunt u kiezen uit verschillende Custom Vision-algoritmen die zijn geoptimaliseerd voor afbeeldingen met bepaalde onderwerpen&mdash;bijvoorbeeld oriëntatiepunten of detailhandelsartikelen. Zie de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) voor meer informatie hierover.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator
De Custom Vision Service is beschikbaar als een set met systeemeigen SDK’s of via een webinterface op de [Custom Vision-startpagina](https://customvision.ai/). U kunt een model maken, testen en trainen via een van de interfaces of beide samen gebruiken.

![Custom Vision-startpagina in een Chrome-browservenster](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle Cognitive Services, dienen ontwikkelaars die de Custom Vision-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Volg de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) om aan de slag te gaan met Custom Vision op het web, of voltooi een [zelfstudie voor afbeeldingsclassificatie](csharp-tutorial.md) om een basisscenario in code te implementeren.
