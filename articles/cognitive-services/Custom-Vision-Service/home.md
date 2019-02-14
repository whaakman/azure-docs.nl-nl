---
title: Wat is Azure Custom Vision?
titlesuffix: Azure Cognitive Services
description: Leer hoe u de Custom Vision-service kunt gebruiken om aangepaste classificaties voor afbeeldingen te bouwen in de Azure-cloud.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: cc60166b4105cf38d3c1f73bdb558af7a9a7c831
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857478"
---
# <a name="what-is-azure-custom-vision"></a>Wat is Azure Custom Vision?

De Azure Custom Vision API is een Cognitive Service waarmee u aangepaste classificaties voor afbeeldingen kunt bouwen, implementeren en verbeteren. Een classificatie voor afbeeldingen is een AI-service waarmee afbeeldingen worden gesorteerd in klassen (tags) op basis van bepaalde kenmerken. Anders dan bij de [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)-service kunt u met Custom Vision uw eigen classificaties maken.

## <a name="what-it-does"></a>Wat het doet

De Custom Vision Service gebruikt een Machine Learning-algoritme om afbeeldingen te classificeren. U, als ontwikkelaar, moet groepen afbeeldingen indienen die de betreffende classificaties wel of niet hebben. U geeft de juiste tags voor de afbeeldingen op wanneer u ze indient. Vervolgens wordt het algoritme getraind op basis van deze gegevens en berekent het algoritme de eigen nauwkeurigheid aan de hand van dezelfde gegevens. Zodra het model is getraind, kunt u het model testen, opnieuw trainen en uiteindelijk gebruiken om nieuwe afbeeldingen te classificeren op basis van de behoeften van de app. U kunt het model zelf ook exporteren voor offlinegebruik.

### <a name="classification-and-object-detection"></a>Classificatie- en objectdetectie

Custom Vision-functionaliteit kan worden onderverdeeld in twee functies. Met **Afbeeldingsclassificatie** wordt een verdeling van classificaties toegewezen aan elke afbeelding. Classificatiemodellen met meerdere klassen (één tag per afbeelding) en meerdere labels (een willekeurig aantal tags per afbeelding) worden beide ondersteund. **Objectdetectie** is vergelijkbaar met classificatie met meerdere labels, maar hiermee worden ook de coördinaten van de afbeelding geretourneerd waar de toegepaste labels kunnen worden gevonden.

### <a name="optimization"></a>Optimalisatie

Over het algemeen zijn de methoden waarvan wordt gebruikgemaakt in Custom Vision Service, niet erg gevoelig voor wijzigingen, waardoor u met zeer weinig gegevens kunt beginnen met het maken van een prototype. 50 afbeeldingen per tag is meestal al een goed begin. Dit betekent echter dat de service niet optimaal geschikt is voor het detecteren van subtiele verschillen in afbeeldingen (bijvoorbeeld het detecteren van kleine fouten in scenario’s voor kwaliteitscontrole).

Daarnaast kunt u kiezen uit verschillende Custom Vision-algoritmen die zijn geoptimaliseerd voor bepaalde onderwerpen&mdash;bijvoorbeeld oriëntatiepunten of detailhandelsartikelen. Zie de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) voor meer informatie hierover.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator
De Custom Vision Service is beschikbaar als een set met systeemeigen SDK’s of via een webinterface op de [Custom Vision-startpagina](https://customvision.ai/). U kunt een model of een interface maken, testen en trainen, of beide.

![Custom Vision-startpagina in een Chrome-browservenster](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle Cognitive Services, dienen ontwikkelaars die de Custom Vision-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Volg de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) om aan de slag te gaan met Custom Vision op het web, of voltooi een [zelfstudie voor afbeeldingsclassificatie](csharp-tutorial.md) om het scenario in code te implementeren.
