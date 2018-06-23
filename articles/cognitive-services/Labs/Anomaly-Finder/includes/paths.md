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
ms.openlocfilehash: a806cac410eb57e59dacb42da9be954b2f962956
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345057"
---
<a name="paths"></a>
## <a name="paths"></a>Paden

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Afwijkingsdetectie punten voor de reeks gegevenspunten keer aangevraagd detecteren
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parameters

|Type|Naam|Beschrijving|Schema|
|---|---|---|---|
|**Hoofdtekst**|**Hoofdtekst**  <br>*Vereist*|De tijdreeks gegevenspunten en periode indien nodig.|[Aanvraag](#request)|


#### <a name="responses"></a>Antwoorden

|HTTP-Code|Beschrijving|Schema|
|---|---|---|
|**200**|De bewerking is geslaagd.|< [antwoord](#response) > matrix|
|**400**|Kan de JSON-aanvraag niet parseren.|Geen inhoud|
|**403**|Het certificaat dat u hebt opgegeven, wordt niet geaccepteerd door de server.|Geen inhoud|
|**405**|De methode is niet toegestaan.|Geen inhoud|
|**500**|Er is een interne serverfout opgetreden.|Geen inhoud|


#### <a name="consumes"></a>Verbruikt

* `application/json`


#### <a name="produces"></a>Produceert

* `application/json`


#### <a name="tags"></a>Tags

* anomalydetection



