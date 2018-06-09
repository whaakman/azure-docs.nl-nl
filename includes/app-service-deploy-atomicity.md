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
ms.openlocfilehash: ac2cf4d688b1bdc54ed2d7341f0e195d3b2fe42d
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236464"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Wat gebeurt er in mijn app tijdens de implementatie?

Alle methoden die officieel ondersteunde implementatie gemeen hebben één ding: deze wijzigingen aanbrengen in de bestanden in de `/site/home/wwwroot` map van uw app. Dit zijn dezelfde bestanden die worden uitgevoerd in de productieomgeving. Daarom de implementatie kan mislukken door vergrendelde bestanden of de app in productie hebben mogelijk onvoorspelbaar gedrag tijdens de implementatie omdat niet alle bestanden tegelijk worden bijgewerkt. Er zijn een aantal verschillende manieren om te voorkomen dat deze problemen:

- Uw app stoppen of offlinemodus voor uw app inschakelen tijdens de implementatie. Zie voor meer informatie [omgaan met vergrendelde bestanden tijdens de implementatie van](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementeren naar een [faseringssleuven](../articles/app-service/web-sites-staged-publishing.md) met [automatisch wisselen](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) ingeschakeld. 
- Gebruik [uitvoeren vanuit het Zip](https://github.com/Azure/app-service-announcements/issues/84) in plaats daarvan.
