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
ms.openlocfilehash: a49df0c18ef9db9d0d41ca2e714474e6386ae7d1
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904649"
---
<a name="definitions"></a>
## <a name="definitions"></a>Definities

<a name="point"></a>
### <a name="point"></a>Punt

|Naam|Beschrijving|Schema|
|---|---|---|
|**Timestamp**  <br>*Optioneel*|De tijdstempel voor het gegevenspunt. Zorg ervoor dat deze worden uitgelijnd met de middernacht, en gebruik een UTC datum en tijd tekenreeks, bijvoorbeeld 2017-08-01T00:00:00Z.|tekenreeks (datum / tijd)|
|**Waarde**  <br>*Optioneel*|De waarde voor een meting.|getal (double)|


<a name="request"></a>
### <a name="request"></a>Aanvraag

|Naam|Beschrijving|Schema|
|---|---|---|
|**Periode**  <br>*Optioneel*|De periode van de gegevenspunten. Als de waarde null is of is niet aanwezig, wordt de API de periode automatisch bepaald.|getal (double)|
|**Punten**  <br>*Optioneel*|De time series-gegevens verwijst. De gegevens moeten worden gesorteerd op oplopende zodat deze overeenkomt met het resultaat van afwijkingen timestamp. Als de gegevens worden niet correct worden gesorteerd of als er dubbele timestamp, de API de anomaliedetectie-punten correct wordt gedetecteerd, maar u de punten die is geretourneerd met de invoer kan niet goed overeen. In dit geval is, wordt een waarschuwingsbericht wordt weergegeven in het antwoord worden toegevoegd.|< [Wijs](#point) > matrix|


<a name="response"></a>
### <a name="response"></a>Antwoord

|Naam|Beschrijving|Schema|
|---|---|---|
|**ExpectedValues**  <br>*Optioneel*|De voorspelde waarde door de learning op basis van model. Als de invoer gegevenspunten worden gesorteerd op oplopende timestamp, kan de index van de matrix worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|< getal (double) > matrix|
|**IsAnomaly**  <br>*Optioneel*|Het resultaat op of de gegevenspunten afwijkingen zijn of niet in beide richtingen (pieken en dalen). True betekent dat het punt zich afwijkingen, false betekent dat het beheerpunt is niet van afwijkingen. Als de invoer gegevenspunten worden gesorteerd op oplopende timestamp, kan de index van de matrix worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < Booleaanse waarde >|
|**IsAnomaly_Neg**  <br>*Optioneel*|Het resultaat op of de gegevenspunten afwijkingen in negatieve richting (Spanningsdips) zijn. True betekent dat de richting van de afwijkingen is negatief. Als de invoer gegevenspunten worden gesorteerd op oplopende timestamp, kan de index van de matrix worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < Booleaanse waarde >|
|**IsAnomaly_Pos**  <br>*Optioneel*|Het resultaat op of de gegevenspunten afwijkingen in positieve richting (pieken) zijn. True betekent dat de richting van de afwijkingen is positief. Als de invoer gegevenspunten worden gesorteerd op oplopende timestamp, kan de index van de matrix worden gebruikt om toe te wijzen de verwachte en de oorspronkelijke waarde.|matrix van < Booleaanse waarde >|
|**LowerMargin**  <br>*Optioneel*|(ExpectedValue - LowerMargin) bepaalt de ondergrens die gegevenspunt is nog steeds als normale beschouwd. Als de invoer gegevenspunten worden gesorteerd op oplopende timestamp, kan de index van de matrix worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|< getal (double) > matrix|
|**Periode**  <br>*Optioneel*|De periode dat de API gebruikt voor het detecteren van de anomaliedetectie-punten.|getal (drijvende komma)|
|**UpperMargin**  <br>*Optioneel*|De som van ExpectedValue en UpperMargin bepaalt de bovengrens die gegevenspunt is nog steeds als normale beschouwd. Als de invoer gegevenspunten worden gesorteerd op oplopende timestamp, kan de index van de matrix worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|< getal (double) > matrix|
|**WarningText**  <br>*Optioneel*|Als de invoergegevens punten die de regel die de API is vereist en de gegevens nog steeds kan worden gedetecteerd door de API niet volgt, wordt de API de gegevens worden geanalyseerd en de waarschuwing-informatie in dit veld toevoegen.|tekenreeks|



