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
ms.openlocfilehash: 80503ad154a9fc4d01614ffd2816f9d5fd497fdb
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228894"
---
<a name="paths"></a>
## <a name="paths"></a>Paden

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Detecteren van afwijkingen punten voor de gegevenspunten van reeks keer aangevraagd
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parameters

|Type|Name|Beschrijving|Schema|
|---|---|---|---|
|**Hoofdtekst**|**body**  <br>*Vereist*|De tijdreeksen gegevenspunten en periode indien nodig.|[request](#request)|


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



