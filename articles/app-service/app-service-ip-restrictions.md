---
title: Azure App Service IP-beperkingen | Microsoft Docs
description: IP-beperkingen gebruiken met Azure App Service
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal
ms.openlocfilehash: 22e05af889b4e792dcc6f6fc438e8a58674b9f0e
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-app-service-static-ip-restrictions"></a>Azure App Service statische IP-beperkingen #

IP-beperkingen kunt u een lijst met IP-adressen die toegang hebben tot uw app definiëren. De lijst met toegestane kan afzonderlijke IP-adressen of een bereik met IP-adressen die zijn gedefinieerd door een subnetmasker.

Wanneer een aanvraag naar de app wordt gegenereerd vanuit een client, wordt het IP-adres wordt geëvalueerd op basis van de lijst toestaan. Als het IP-adres niet in de lijst, wordt de app geantwoord met een [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) statuscode.

IP-beperkingen zijn gedefinieerd in het bestand web.config die uw app tijdens runtime verbruikt. Onder bepaalde omstandigheden mogelijk bepaalde module worden uitgevoerd voordat de logica van de IP-beperkingen in de HTTP-pipeline. Als dit gebeurt, wordt de aanvraag mislukt met een andere HTTP-foutcode.

IP-beperkingen worden geëvalueerd op dezelfde App Service plan exemplaren toegewezen aan uw app.

Om een regel voor IP-beperking toevoegen aan uw app, gebruikt u het menu openen **netwerk**>**IP-beperkingen** en klik op **IP-beperkingen configureren**

! [IP-adresbeperkingen] (media/app-service-ip-restrictions/ip-restrictions.png)

Hier kunt kunt u de lijst met IP-beperking regels gedefinieerd voor uw app bekijken.

![lijst met IP-beperkingen](media/app-service-ip-restrictions/browse-ip-restrictions.png)

U kunt klikken op **[+] toevoegen** toevoegen van een nieuwe regel voor het IP-beperkingen.

![IP-beperkingen toevoegen](media/app-service-ip-restrictions/add-ip-restrictions.png)
