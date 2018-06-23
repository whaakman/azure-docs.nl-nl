---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: e37d3ef5b6f65ad31bc19f9f8c15350014d1c9ad
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345065"
---
Met de [Afwijkingsdetectie zoeken-API](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), tijd reeksgegevens in JSON-indeling uploaden naar de API-eindpunt en vervolgens het resultaat van de API-reactie gelezen. U kunt de time series gegevens uploaden, elk gegevenspunt bevat:  
* Tijdstempel - het tijdstempel voor het gegevenspunt. Zorg ervoor dat wordt gebruikt een UTC tijd datumtekenreeks, bijvoorbeeld ' 2017-08-01T00:00:00Z '
* Waarde: de meting van dat gegevenspunt

De resultaten bestaan uit:
* Periode - de periodiciteit die gebruikmaakt van de API voor het detecteren van de afwijkingen
* WarningText - mogelijk waarschuwingsgegevens
* ExpectedValue - de voorspelde waarde van de learning op basis van model
* IsAnomaly - het resultaat op of de gegevenspunten afwijkingen zijn of niet in beide richtingen (pieken of dips)
* IsAnomaly_Neg - het resultaat van of de gegevenspunten afwijkingen in negatieve richting (dips)
* IsAnomaly_Pos - het resultaat van of de gegevenspunten afwijkingen in positieve richting (pieken)
* UpperMargin - de som van ExpectedValue en UpperMargin bepaalt de bovengrens die gegevenspunt is nog steeds normaal beschouwd
* LowerMargin - (ExpectedValue - LowerMargin) bepaalt de ondergrens dat gegevenspunt nog steeds wordt beschouwd als normale

Details van het gegevenscontract zijn te vinden [hier](../apiref.md).

