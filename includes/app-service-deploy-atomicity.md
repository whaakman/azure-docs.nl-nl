---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/08/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b8e3d32f0e230673ecbc043597afe8e7b5f25e06
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742241"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Wat gebeurt er aan mijn app tijdens de implementatie?

De officieel ondersteunde implementatiemethoden hebben één ding gemeen: ze brengen wijzigingen aan in de bestanden in de `/home/site/wwwroot` map van uw app. Dit zijn dezelfde bestanden die worden uitgevoerd in de productieomgeving. Daarom kan de implementatie mislukken door vergrendelde bestanden of de app in productie kan mogelijk onvoorspelbaar gedrag vertonen tijdens de implementatie omdat niet alle bestanden tegelijk worden bijgewerkt. Er zijn een aantal verschillende manieren om deze problemen te voorkomen:

- Stop uw app of schakel de offlinemodus in voor uw app tijdens de implementatie. Zie voor meer informatie, [omgaan met vergrendelde bestanden tijdens de implementatie](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementeren naar een [staging-sites](../articles/app-service/web-sites-staged-publishing.md) met [automatisch wisselen](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) ingeschakeld. 
- Gebruik [uitvoeren van pakket](https://github.com/Azure/app-service-announcements/issues/84).
