---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/12/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 93332002cd2ac513d125e0f9eb75dff4a2d8760c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836727"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Wat gebeurt er aan mijn app tijdens de implementatie?

De officieel ondersteunde implementatiemethoden wijzigingen aanbrengen in de bestanden in de `/home/site/wwwroot` map van uw app. Deze bestanden zijn dezelfde zijn als die worden uitgevoerd in de productieomgeving. Daarom kan de implementatie mislukken vanwege vergrendelde bestanden. De app in productie kan ook onvoorspelbaar gedrag tijdens de implementatie, omdat niet alle bestanden op hetzelfde moment bijgewerkt. Er zijn een aantal verschillende manieren om deze problemen te voorkomen:

- Stop uw app of schakel de offlinemodus in voor uw app tijdens de implementatie. Zie voor meer informatie, [omgaan met vergrendelde bestanden tijdens de implementatie van](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementeren naar een [staging-sites](../articles/app-service/deploy-staging-slots.md) met [automatisch wisselen](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) ingeschakeld. 
- Gebruik [uitvoeren van pakket](https://github.com/Azure/app-service-announcements/issues/84) in plaats van continue implementatie.
