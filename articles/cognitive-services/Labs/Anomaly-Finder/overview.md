---
title: Wat is Afwijkingsdetectie? -Microsoft Cognitive Services | Microsoft Docs
description: Gebruik geavanceerde algoritmen in Afwijkingsdetectie voor het identificeren van afwijkingen in time series-gegevens en informatie retourneren in de Microsoft Cognitive Services.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 2a0715d3becf695600ed84edbae38151acf055a8
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48246839"
---
# <a name="what-is-anomaly-finder"></a>Wat is Afwijkingsdetectie?

[!INCLUDE [PrivatePreviewNote](../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Afwijkingsdetectie kunt u gegevens na verloop van tijd bewaken en anomalieën met machine learning die wordt aangepast aan uw unieke gegevens door het automatisch toepassen van de juiste statistische model, ongeacht de branche, scenario, of het gegevensvolume. Met behulp van een tijdreeks als invoer, de API voor Afwijkingsdetectie Finder geeft als resultaat al dan niet een gegevenspunt is een anomalie, bepaalt de verwachte waarde en boven- en ondergrenzen voor visualisatie. Als een vooraf gedefinieerde AI-service, Afwijkingsdetectie zijn vereist om een machine learning-expertise dan informatie over het gebruik van een RESTful-API. Hierdoor wordt ontwikkelen eenvoudig en veelzijdig omdat deze met een time series-gegevens werkt en kan ook worden ingebouwd in de streaming-gegevens systemen. Afwijkingsdetectie omvat een breed bereik van gebruiksvoorbeelden: bijvoorbeeld financiële hulpmiddelen voor het beheren van fraude, diefstal, markten en potentiële business incidenten wijzigen of bewaking van IoT-apparaat verkeer behoud anonimiteit. Deze oplossing kan ook worden gelde als onderdeel van een service voor end-klanten om te begrijpen van wijzigingen in gegevens, uitgaven, rendement op investering of gebruikersactiviteit.
Probeer de Finder-API voor Afwijkingsdetectie en profiteer van uw gegevens beter te begrijpen. 

Zie wat u met deze API kunt maken:

* Meer informatie over het voorspellen van de verwachte waarden op basis van historische gegevens in de tijdreeks
* Weet u of een gegevenspunt een anomalie buiten het historische patroon
* Genereren van een buiten-bandbeheer voor het visualiseren van het bereik van de 'normale' waarde

![Anomaly_Finder](./media/anomaly_detection1.png) 

Figuur 1: Detecteer afwijkingen in opbrengsten voor de verkoop

![Anomaly_Finder](./media/anomaly_detection2.png)

Figuur 2: Patroon wijzigingen detecteren in serviceaanvragen

## <a name="requirements"></a>Vereisten

- Minimale gegevens voor invoer time series: minimaal 13 gegevens verwijst voor tijdreeks zonder wissen periodiciteit minimum van 4 cycli van gegevens verwijst voor de tijdreeks met bekende periodiciteit. 
- Integriteit van gegevens: time series gegevenspunten in hetzelfde interval en geen ontbrekende punten worden gescheiden. 

## <a name="identify-anomalies"></a>Identificeren van afwijkingen

Anomaliedetectie-API retourneert resultaat dat of de opgegeven gegevenspunten afwijkingen of niet zijn en biedt aanvullende informatie die als volgt
* Periode - de periodiciteit die de API gebruikt voor het detecteren van de anomaliedetectie-punten.
* WarningText - de mogelijke waarschuwing-informatie.
* ExpectedValue - de voorspelde waarde door de learning op basis van model
* IsAnomaly - het resultaat op of de gegevenspunten afwijkingen of niet zijn
* IsAnomaly_Neg - het resultaat van of de gegevenspunten afwijkingen in negatieve richting (Spanningsdips)
* IsAnomaly_Pos - het resultaat van of de gegevenspunten afwijkingen in positieve richting (pieken)
* UpperMargin - de som van ExpectedValue en UpperMargin bepaalt de bovengrens die gegevenspunt is nog steeds beschouwd als normale
* LowerMargin - (ExpectedValue - LowerMargin) bepaalt de ondergrens dat gegevenspunt nog steeds wordt beschouwd als normale

> [!Note]
> UpperMargin en LowerMargin kunnen worden gebruikt voor het genereren van een rand rond werkelijke tijdreeksen voor het visualiseren van het bereik van de normale waarden. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Kleine en bovengrens in na verwerking van het antwoord aanpassen

Anomaliedetectie-API retourneert standaard resultaat op of een gegevenspunt afwijkingen is en de grens boven en ondergrenzen van ExpectedValue en UpperMargin/LowerMargin kan worden berekend. Deze standaardwaarden moeten werken prima voor de meeste gevallen. Sommige scenario's vereisen echter verschillende grenzen dan de standaardwaarden. De aanbevolen procedure is een coefficiency toepassen op de UpperMargin of LowerMargin om aan te passen van de dynamische grenzen.

### <a name="examples-with-1152-as-coefficiency"></a>Voorbeelden met 1/1.5/2 als coefficiency

![Standaard gevoeligheid](./media/sensitivity_1.png)

![1.5 gevoeligheid](./media/sensitivity_1.5.png)

![Gevoeligheid van 2](./media/sensitivity_2.png)

Aanvraag met voorbeeldgegevens

[!INCLUDE [Request](./includes/request.md)]

Voorbeeld van JSON-antwoord

[!INCLUDE [Response](./includes/response.md)]
