---
title: N-beste vertalingen - retourneren Translator Text-API
titlesuffix: Azure Cognitive Services
description: N-beste vertalingen met behulp van de Microsoft Translator Text-API retourneert.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 41027cf83fd6074822c70b6039906592cb8698b0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878643"
---
# <a name="how-to-return-n-best-translations"></a>Hoe N-beste vertalingen worden geretourneerd

> [!NOTE]
> Deze methode is afgeschaft. Het is niet beschikbaar in V3.0 van de Translator Text-API.

De GetTranslations() en GetTranslationsArray() methoden van de Microsoft Translator-API zijn: een optionele Booleaanse vlag 'IncludeMultipleMTAlternatives'.
De methode retourneert maximaal maxTranslations alternatieven waarin de deltagegevens uit de N-beste lijst van de translator-engine wordt geleverd.

De handtekening is:

**Syntaxis**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parameters**

| Parameter | Description |
|:---|:---|
| appId | **Vereiste** als de autorisatie-header wordt gebruikt, laat het veld appid leeg anders geeft u een tekenreeks met "Bearer" + "" + toegangstoken.|
| tekst | **Vereiste** een tekenreeks voor de tekst te vertalen. De grootte van de tekst mag niet groter zijn dan 10000 tekens.|
| uit | **Vereiste** een tekenreeks voor de taalcode van de tekst te vertalen. |
| tot | **Vereiste** een tekenreeks voor de taalcode voor de omzetting van de tekst in. |
| maxTranslations | **Vereiste** een int het maximale aantal vertalingen om terug te keren. |
| opties | **Optionele** A TranslateOptions-object met de onderstaande waarden. Ze zijn optioneel en standaard naar de meest voorkomende instellingen.

* Categorie: De enige ondersteunde, en de standaard, optie 'Algemeen'.
* ContentType: De enige ondersteunde, en de standaard, optie ' text/plain'.
* Status: De status van de gebruiker om te vergelijken aanvraag en antwoord. Dezelfde inhoud in het antwoord geretourneerd.
* IncludeMultipleMTAlternatives: de vlag om te bepalen of retourneren van meer dan één alternatieven op de MT-engine. Standaard is ingesteld op false en slechts 1 alternatief bevat.

## <a name="ratings"></a>Beoordelingen
De classificaties zijn als volgt toegepast: De beste automatische vertaling heeft een classificatie van 5.
De automatisch gegenereerde (N-beste) vertaling alternatieven hebben een classificatie van 0 en de mate van een overeenkomst van 100.

## <a name="number-of-alternatives"></a>Aantal alternatieven
Het aantal geretourneerde alternatieven maximaal maxTranslations is echter mogelijk kleiner.

## <a name="language-pairs"></a>Taal paren
Deze functionaliteit is niet beschikbaar voor vertalingen tussen vereenvoudigd en traditioneel Chinees, beide richtingen. Het is beschikbaar voor alle andere paren van Microsoft Translator-ondersteunde taal.
