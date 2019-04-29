---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 8709956adee06e4e783ac5a7b317b2c4dec43e73
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765591"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Wat gebeurt er aan mijn app tijdens de implementatie?

De officieel ondersteunde implementatiemethoden hebben één ding gemeen: ze brengen wijzigingen aan in de bestanden in de `/home/site/wwwroot` map van uw app. Dit zijn dezelfde bestanden die worden uitgevoerd in de productieomgeving. Daarom kan de implementatie mislukken door vergrendelde bestanden of de app in productie kan mogelijk onvoorspelbaar gedrag vertonen tijdens de implementatie omdat niet alle bestanden tegelijk worden bijgewerkt. Er zijn een aantal verschillende manieren om deze problemen te voorkomen:

- Stop uw app of schakel de offlinemodus in voor uw app tijdens de implementatie. Zie voor meer informatie, [omgaan met vergrendelde bestanden tijdens de implementatie](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementeren naar een [staging-sites](../articles/app-service/deploy-staging-slots.md) met [automatisch wisselen](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) ingeschakeld. 
- Gebruik [uitvoeren van pakket](https://github.com/Azure/app-service-announcements/issues/84).
