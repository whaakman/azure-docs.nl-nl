---
title: Wat is Afwijkingsdetectie zoeken? -Cognitieve Microsoft-Services | Microsoft Docs
description: Gebruik van geavanceerde algoritmen in Afwijkingsdetectie zoeken om te identificeren afwijkingen in de reeksgegevens en informatie geretourneerd in cognitieve Microsoft-Services.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 1080bb0ad1d901a8b9a5ace4993d4e0d46924a03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345061"
---
# <a name="what-is-anomaly-finder"></a>Wat is Afwijkingsdetectie zoeken?

Afwijkingsdetectie zoeken kunt u het bewaken van gegevens gedurende een bepaalde periode en afwijkingen met machine learning die wordt aangepast aan uw unieke gegevens door het toepassen van het recht statistische model ongeacht de branche, scenario, of gegevens volume automatisch detecteren. Met behulp van een tijdreeks als invoer, de Afwijkingsdetectie zoeken-API retourneert al dan niet een gegevenspunt is een afwijkingsdetectie bepaalt de verwachte waarde en boven- en ondergrenzen voor visualisatie. Als een vooraf gedefinieerde AI-service vereist geen Afwijkingsdetectie zoeken een machine learning-expertise afgezien van informatie over het gebruik van een RESTful-API. Dit is de ontwikkeling eenvoudig en veelzijdige omdat deze met alle gegevens van de reeks tijd werkt en kan ook worden in ingebouwd streaminggegevens systemen. Afwijkingsdetectie zoeken omvat een breed bereik van gebruiksvoorbeelden: bijvoorbeeld financiële hulpprogramma's voor het beheren van fraude, diefstal, markten en potentiële business incidenten wijzigen of bewaking van IoT-apparaat verkeer behoud anonimiteit. Deze oplossing kan ook monetized als onderdeel van een service voor end-klanten om te begrijpen wijzigingen in gegevens, uitgaven, rendement op investering of gebruikersactiviteit.
De Afwijkingsdetectie zoeken-API uitproberen en beter begrip van uw gegevens krijgen. 

Zie wat u kunt bouwen met deze API:

* Informatie over het voorspellen van de verwachte waarden op basis van historische gegevens in de tijdreeks
* Weet u of een gegevenspunt een afwijkingsdetectie buiten het historische patroon
* Een band visualiseren van het bereik van de 'normale' waarde genereren

![Anomaly_Finder](./media/anomaly_detection1.png) 

Figuur 1: Afwijkingen in verkoop inkomsten detecteren

![Anomaly_Finder](./media/anomaly_detection2.png)

Figuur 2: Patroon wijzigingen in serviceaanvragen detecteren

## <a name="requirements"></a>Vereisten

- Minimale gegevens voor invoer time series: minimaal 13 gegevens verwijst voor tijdreeks zonder wissen periodiciteit minimum van 4 cycli van gegevens verwijst voor de tijdreeks met bekende periodiciteit. 
- Integriteit van gegevens: time series gegevenspunten in hetzelfde interval en er geen ontbrekende punten worden gescheiden. 

## <a name="identify-anomalies"></a>Afwijkingen identificeren

Afwijkingsdetectie API resultaat retourneert die of de opgegeven gegevenspunten afwijkingen of niet zijn is en als volgt vindt u aanvullende informatie
* Periode - de frequentie waarmee de API voor het detecteren van de afwijkingsdetectie punten.
* WarningText - mogelijk waarschuwingsgegevens.
* ExpectedValue - de voorspelde waarde van de learning op basis van model
* IsAnomaly - het resultaat op of de gegevenspunten afwijkingen of niet zijn
* IsAnomaly_Neg - het resultaat van of de gegevenspunten afwijkingen in negatieve richting (dips)
* IsAnomaly_Pos - het resultaat van of de gegevenspunten afwijkingen in positieve richting (pieken)
* UpperMargin - de som van ExpectedValue en UpperMargin bepaalt de bovengrens die gegevenspunt is nog steeds normaal beschouwd
* LowerMargin - (ExpectedValue - LowerMargin) bepaalt de ondergrens dat gegevenspunt nog steeds wordt beschouwd als normale

> [!Note]
> UpperMargin en LowerMargin kunnen worden gebruikt voor het genereren van een rand rond de werkelijke tijdreeks voor het visualiseren van het bereik van de normale waarden. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Aanpassen van kleine en bovengrenzen in post-verwerking op de reactie

Afwijkingsdetectie API standaard resultaat van het feit een gegevenspunt afwijkingsdetectie is en de hogere en lagere is gebonden van ExpectedValue en UpperMargin/LowerMargin kan worden berekend. Deze standaardwaarden moeten prima voor de meeste gevallen werken. Sommige scenario's vereisen echter andere grenzen dan de standaardwaarden. De aanbevolen procedure is een coefficiency toepassen op de UpperMargin of LowerMargin aanpassen van de dynamische grenzen.

### <a name="examples-with-1152-as-coefficiency"></a>Voorbeelden met 1.5-1/2 als coefficiency

![Standaard gevoeligheid](./media/sensitivity_1.png)

![1.5 gevoeligheid](./media/sensitivity_1.5.png)

![2 gevoeligheid](./media/sensitivity_2.png)

Klik met voorbeeldgegevens aanvragen

[!INCLUDE [Request](./includes/request.md)]

Voorbeeld JSON-antwoord

[!INCLUDE [Response](./includes/response.md)]
