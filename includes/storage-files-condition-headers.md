---
title: bestand opnemen
description: bestand opnemen
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/17/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0486b595bffd18b06d54e8377b24deab04e2aa93
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159870"
---
## <a name="error-conditionheadersnotsupported-from-a-web-application-using-azure-files-from-browser"></a>Fout ConditionHeadersNotSupported vanuit een webtoepassing met Azure Files in Browser

Wanneer u toegang tot inhoud die wordt gehost in Azure Files via een toepassing die maakt gebruik van voorwaardelijke kopteksten, zoals een webbrowser toegang is mislukt, waarbij een ConditionHeadersNotSupported-fout optreedt.

![ConditionHeaderNotSupported fout](media/storage-files-condition-headers/conditionalerror.png)

### <a name="cause"></a>Oorzaak

Voorwaardelijke headers zijn nog niet ondersteund. Toepassingen kunnen implementeren moet het volledige bestand aanvragen telkens als het bestand wordt geopend.

### <a name="workaround"></a>Tijdelijke oplossing

Wanneer een nieuw bestand wordt geüpload, wordt de cache-control-eigenschap standaard "no-cache". Als u wilt afdwingen van de toepassing naar het bestand aan te vragen moet telkens wanneer cache-control-eigenschap van het bestand worden bijgewerkt van 'no-cache' naar 'no-cache, no-store, moet-revalidate'. Dit kan worden bereikt met behulp van [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Storage explorer inhoud van een cache wijzigen](media/storage-files-condition-headers/storage-explorer-cache.png)