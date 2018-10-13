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
ms.openlocfilehash: 91a4a9ae1d3d84f1396adad07d1cda73ee3747c9
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49312459"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Wat gebeurt er aan mijn app tijdens de implementatie?

De officieel ondersteunde implementatiemethoden hebben één ding gemeen: ze wijzigingen aanbrengen in de bestanden in de `/site/home/wwwroot` map van uw app. Dit zijn dezelfde bestanden die worden uitgevoerd in de productieomgeving. Daarom de implementatie kan mislukken door vergrendelde bestanden of de app in productie hebben mogelijk onvoorspelbaar gedrag tijdens de implementatie omdat niet alle bestanden tegelijk worden bijgewerkt. Er zijn een aantal verschillende manieren om deze problemen te voorkomen:

- Uw app stoppen of offlinemodus voor uw app inschakelen tijdens de implementatie. Zie voor meer informatie, [omgaan met vergrendelde bestanden tijdens de implementatie van](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementeren naar een [staging-sites](../articles/app-service/web-sites-staged-publishing.md) met [automatisch wisselen](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) ingeschakeld. 
- Gebruik [uitvoeren van pakket](https://github.com/Azure/app-service-announcements/issues/84) in plaats daarvan.
