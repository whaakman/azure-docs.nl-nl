---
title: bestand opnemen
description: bestand opnemen
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 3f94481e6a8550479788d92c744327e1dc3b58c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66376923"
---
De volgende limieten gelden voor Azure Event Grid-onderwerpen voor systeem- en aangepaste onderwerpen *niet* gebeurtenis domeinen.

| Resource | Limiet |
| --- | --- |
| Aangepaste onderwerpen per Azure-abonnement | 100 |
| Abonnementen op gebeurtenissen per onderwerp | 500 |
| Publiceren-tarief voor een aangepast onderwerp (inkomend) | 5000 gebeurtenissen per seconde per onderwerp |
| Publiceren van aanvragen | 250 per seconde |
| Gebeurtenisgrootte | Ondersteuning voor 64 KB in het algemeen beschikbaar is (GA). Ondersteuning voor 1 MB is momenteel in preview. |

De volgende limieten gelden voor alleen gebeurtenis domeinen.

| Resource | Limiet |
| --- | --- |
| Onderwerpen per domein van de gebeurtenis | 1000 tijdens de openbare preview |
| Abonnementen op gebeurtenissen per onderwerp binnen een domein | 50 tijdens de openbare preview |
| Domein omvang gebeurtenisabonnementen | 50 tijdens de openbare preview |
| Publiceren-tarief voor een gebeurtenis-domein (inkomend) | 5000 gebeurtenissen per seconde tijdens de openbare preview |
| Publiceren van aanvragen | 250 per seconde |