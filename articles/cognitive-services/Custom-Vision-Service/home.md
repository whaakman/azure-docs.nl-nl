---
title: Wat is Azure Custom Vision?
titlesuffix: Azure Cognitive Services
description: Leer hoe u de Custom Vision-service kunt gebruiken om aangepaste classificaties voor afbeeldingen te bouwen in de Azure-cloud.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 10/26/2018
ms.author: anroth
ms.openlocfilehash: 5e8e675b32bfd4c741b82b1ab341a80adbb0529d
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741637"
---
# <a name="what-is-azure-custom-vision"></a>Wat is Azure Custom Vision?

De Azure Custom Vision API is een Cognitive Service waarmee u aangepaste classificaties voor afbeeldingen kunt bouwen, implementeren en verbeteren. Een classificatie voor afbeeldingen is een AI-service waarmee afbeeldingen worden gesorteerd in klassen (tags) op basis van bepaalde kenmerken. Anders dan bij de [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)-service kunt u met Custom Vision uw eigen classificaties maken.

## <a name="what-it-does"></a>Wat het doet

De Custom Vision Service gebruikt een Machine Learning-algoritme om afbeeldingen te classificeren. U, als ontwikkelaar, moet groepen afbeeldingen indienen die de betreffende classificaties wel of niet hebben. U geeft de juiste tags voor de afbeeldingen op wanneer u ze indient. Vervolgens wordt het algoritme getraind op basis van deze gegevens en berekent het algoritme de eigen nauwkeurigheid aan de hand van dezelfde gegevens. Zodra het model is getraind, kunt u het model testen, opnieuw trainen en uiteindelijk gebruiken om nieuwe afbeeldingen te classificeren op basis van de behoeften van de app. U kunt het model zelf ook exporteren voor offlinegebruik.

### <a name="classification-and-object-detection"></a>Classificatie- en objectdetectie

Custom Vision-functionaliteit kan worden onderverdeeld in twee functies. Met **Afbeeldingsclassificatie** wordt een verdeling van classificaties toegewezen aan elke afbeelding. **Objectdetectie** is vergelijkbaar, maar hiermee worden ook de coördinaten van de afbeelding geretourneerd waar de toegepaste tags kunnen worden gevonden.

### <a name="optimization"></a>Optimalisatie

Over het algemeen zijn de methoden waarvan wordt gebruikgemaakt in Custom Vision Service, niet erg gevoelig voor wijzigingen, waardoor u met zeer weinig gegevens kunt beginnen met het maken van een prototype. 50 afbeeldingen per tag is meestal al een goed begin. Dit betekent echter dat de service niet optimaal geschikt is voor het detecteren van subtiele verschillen in afbeeldingen (bijvoorbeeld het detecteren van kleine fouten in scenario’s voor kwaliteitscontrole).

Daarnaast kunt u kiezen uit verschillende Custom Vision-algoritmen die zijn geoptimaliseerd voor bepaalde onderwerpen&mdash;bijvoorbeeld oriëntatiepunten of detailhandelsartikelen. Zie de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) voor meer informatie hierover.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator
De Custom Vision Service is beschikbaar als een set met systeemeigen SDK’s of via een webinterface op de [Custom Vision-startpagina](https://customvision.ai/). U kunt een model of een interface maken, testen en trainen, of beide.

![Custom Vision-startpagina in een Chrome-browservenster](media/browser-home.png)

## <a name="next-steps"></a>Volgende stappen

Volg de handleiding [Een classificatie bouwen](getting-started-build-a-classifier.md) om aan de slag te gaan met Custom Vision op het web, of voltooi een [zelfstudie voor afbeeldingsclassificatie](csharp-tutorial.md) om het scenario in code te implementeren.
