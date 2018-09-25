---
title: Azure voordeur Service - ondersteuning voor HTTP2 | Microsoft Docs
description: Dit artikel helpt u meer informatie over HTTP/2-ondersteuning in Azure voordeur Service
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 4282c9e9b660476992ba6f948bc5e408e9b064a5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968608"
---
# <a name="http2-support-in-azure-front-door-service"></a>HTTP/2-ondersteuning in Azure voordeur Service
HTTP/2 is een belangrijke revisie voor HTTP/1.1. Het biedt sneller webprestaties, verminderde reactietijd, en verbeterde gebruikerservaring, behoud van de vertrouwde HTTP-methoden, -statuscodes en semantiek. Hoewel HTTP/2 is ontworpen voor gebruik met HTTP en HTTPS, ondersteunen veel client webbrowsers alleen HTTP/2 via Transport Layer Security (TLS).

### <a name="http2-benefits"></a>HTTP/2-voordelen

De voordelen van HTTP/2 zijn:

*   **Multiplexing en gelijktijdigheid**

    Met behulp van HTTP 1.1, meerdere aanvragen van de resource is vereist voor meerdere TCP-verbindingen en elke verbinding heeft prestatieoverhead die ermee verbonden zijn. HTTP/2 kunnen meerdere resources op één TCP-verbinding worden aangevraagd.

*   **Headercompressie**

    Door het comprimeren van de HTTP-headers voor aangeboden resources tijd op de kabel aanzienlijk verminderd.

*   **Stream-afhankelijkheden**

    Stream-afhankelijkheden dat de client om aan te geven op de server welke resources prioriteit hebben.


## <a name="http2-browser-support"></a>Ondersteuning voor HTTP/2-browser

Alle belangrijke browsers hebt ondersteuning voor HTTP/2 geïmplementeerd in hun huidige versies. Niet-ondersteunde browsers automatisch terugval naar HTTP/1.1.

|Browser|Minimale versie|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-front-door-service"></a>HTTP/2-ondersteuning in Azure voordeur Service in te schakelen

Op dit moment is HTTP/2-ondersteuning voor alle configuraties met voordeur actief. Is geen verdere actie vereist van klanten.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over HTTP/2, gaat u naar de volgende bronnen:

- [Startpagina voor HTTP/2-specificatie](https://http2.github.io/)
- [Officiële HTTP/2 Veelgestelde vragen](https://http2.github.io/faq/)
- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).
