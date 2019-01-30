---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 3cc0e521e43f6855397a19fe34fce99da3e20494
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228859"
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

