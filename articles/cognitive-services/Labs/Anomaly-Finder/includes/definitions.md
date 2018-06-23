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
ms.openlocfilehash: ff36202b67f6262b7ba67fe48ef37f2b656b91fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345064"
---
<a name="definitions"></a>
## <a name="definitions"></a>Definities

<a name="point"></a>
### <a name="point"></a>Beheerpunt

|Naam|Beschrijving|Schema|
|---|---|---|
|**Timestamp**  <br>*Optioneel*|De tijdstempel van het gegevenspunt. Zorg ervoor dat deze wordt uitgelijnd met de middernacht en gebruik een UTC datum / tijd tekenreeks, bijvoorbeeld 2017-08-01T00:00:00Z.|tekenreeks (datum-tijd)|
|**Waarde**  <br>*Optioneel*|De gegevenswaarde meting.|nummer (double)|


<a name="request"></a>
### <a name="request"></a>Aanvraag

|Naam|Beschrijving|Schema|
|---|---|---|
|**Periode**  <br>*Optioneel*|De periode van de gegevenspunten. Als de waarde null is of geen, wordt de API van de periode automatisch bepalen.|nummer (double)|
|**Punten**  <br>*Optioneel*|Het gegevenstype van de reeks tijd verwijst. De gegevens moeten worden gesorteerd op oplopende zodat deze overeenkomt met het resultaat afwijkingsdetectie timestamp. Als de gegevens worden niet correct gesorteerd of er is een dubbele timestamp, de API de punten afwijkingsdetectie correct wordt gedetecteerd, maar u de punten geretourneerd met de invoer kan niet goed overeen. In dit geval wordt een waarschuwing weergegeven in het antwoord worden toegevoegd.|< [Wijs](#point) > matrix|


<a name="response"></a>
### <a name="response"></a>Antwoord

|Naam|Beschrijving|Schema|
|---|---|---|
|**ExpectedValues**  <br>*Optioneel*|De voorspelde waarde van de learning op basis van model. Als de invoergegevens punten zijn gesorteerd op oplopende timestamp, kan de index van de matrix kan worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < nummer (double) >|
|**IsAnomaly**  <br>*Optioneel*|Het resultaat op of de gegevenspunten afwijkingen zijn of niet in beide richtingen (pieken of dips). True betekent dat het punt afwijkingsdetectie, false betekent dat het punt niet afwijkingsdetectie. Als de invoergegevens punten zijn gesorteerd op oplopende timestamp, kan de index van de matrix kan worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < Booleaanse waarde >|
|**IsAnomaly_Neg**  <br>*Optioneel*|Het resultaat van of de gegevenspunten afwijkingen in negatieve richting (dips). True betekent dat de richting van de afwijkingsdetectie negatief is. Als de invoergegevens punten zijn gesorteerd op oplopende timestamp, kan de index van de matrix kan worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < Booleaanse waarde >|
|**IsAnomaly_Pos**  <br>*Optioneel*|Het resultaat van of de gegevenspunten afwijkingen in positieve richting (pieken). True betekent dat de richting van de afwijkingsdetectie positief is. Als de invoergegevens punten zijn gesorteerd op oplopende timestamp, kan de index van de matrix kan worden gebruikt om toe te wijzen de verwachte en de oorspronkelijke waarde.|matrix van < Booleaanse waarde >|
|**LowerMargin**  <br>*Optioneel*|(ExpectedValue - LowerMargin) bepaalt de ondergrens die gegevenspunt is nog steeds normaal beschouwd. Als de invoergegevens punten zijn gesorteerd op oplopende timestamp, kan de index van de matrix kan worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < nummer (double) >|
|**Periode**  <br>*Optioneel*|De periode die de API wordt gebruikt voor het detecteren van de afwijkingsdetectie punten.|nummer (float)|
|**UpperMargin**  <br>*Optioneel*|De som van ExpectedValue en UpperMargin bepaalt de bovengrens die gegevenspunt is nog steeds normaal beschouwd. Als de invoergegevens punten zijn gesorteerd op oplopende timestamp, kan de index van de matrix kan worden gebruikt om toe te wijzen de verwachte waarde en de oorspronkelijke waarde.|matrix van < nummer (double) >|
|**WarningText**  <br>*Optioneel*|Als de invoergegevens punten die de regel die de API vereist en de gegevens nog steeds kunnen worden gedetecteerd door de API niet volgt, wordt de API de gegevens analyseren en toevoegen van de waarschuwingsgegevens in dit veld.|tekenreeks|



