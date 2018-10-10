---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 9280790f6692096a0b3909c9d1dfab2e94a8c0d7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904661"
---
Met de [Anomaliedetectie Finder API](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), u kunt de time series-gegevens in JSON-indeling uploaden naar de API-eindpunt en het resultaat wordt gelezen uit de API-reactie. U kunt de time series-gegevens uploaden, elk gegevenspunt bevat:  
* Timestamp - het tijdstempel voor het gegevenspunt. Zorg ervoor dat deze gebruikmaakt van een tekenreeks van de tijd in de UTC datum, bijvoorbeeld ' 2017-08-01T00:00:00Z "
* Waarde: de meting van die gegevenspunt

De resultaten bestaan uit:
* Periode - de periodiciteit die gebruikmaakt van de API voor het detecteren van de afwijkingen
* WarningText - de mogelijke waarschuwing-informatie
* ExpectedValue - de voorspelde waarde door de learning op basis van model
* IsAnomaly - het resultaat op of de gegevenspunten afwijkingen zijn of niet in beide richtingen (pieken en dalen)
* IsAnomaly_Neg - het resultaat van of de gegevenspunten afwijkingen in negatieve richting (Spanningsdips)
* IsAnomaly_Pos - het resultaat van of de gegevenspunten afwijkingen in positieve richting (pieken)
* UpperMargin - de som van ExpectedValue en UpperMargin bepaalt de bovengrens die gegevenspunt is nog steeds beschouwd als normale
* LowerMargin - (ExpectedValue - LowerMargin) bepaalt de ondergrens dat gegevenspunt nog steeds wordt beschouwd als normale

Details van de opdracht gegevens vindt [hier](../apiref.md).

