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
ms.openlocfilehash: fb02496d9c107a2c21acca6c65ef69fdfceb4597
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904564"
---
<a name="paths"></a>
## <a name="paths"></a>Paden

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Detecteren van afwijkingen punten voor de gegevenspunten van reeks keer aangevraagd
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parameters

|Type|Naam|Beschrijving|Schema|
|---|---|---|---|
|**Hoofdtekst**|**De hoofdtekst**  <br>*Vereist*|De tijdreeksen gegevenspunten en periode indien nodig.|[Aanvraag](#request)|


#### <a name="responses"></a>Antwoorden

|HTTP-Code|Beschrijving|Schema|
|---|---|---|
|**200**|De bewerking is geslaagd.|< [antwoord](#response) > matrix|
|**400**|JSON-aanvraag kan niet worden geparseerd.|Geen inhoud|
|**403**|Het certificaat dat u hebt opgegeven, wordt niet geaccepteerd door de server.|Geen inhoud|
|**405**|Methode is niet toegestaan.|Geen inhoud|
|**500**|Er is een interne serverfout opgetreden.|Geen inhoud|


#### <a name="consumes"></a>Verbruikt

* `application/json`


#### <a name="produces"></a>Produceert

* `application/json`


#### <a name="tags"></a>Tags

* anomalydetection



