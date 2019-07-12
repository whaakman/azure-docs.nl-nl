---
title: HTTP/2-ondersteuning in Azure CDN | Microsoft Docs
description: Meer informatie over ondersteuning voor HTTP/2- en CDN.
services: cdn
documentationcenter: ''
author: lichard
manager: erikre
editor: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: c59ecceee35587f1b7d844f160ac0f61c5288e0e
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593744"
---
# <a name="http2-support-in-azure-cdn"></a>Ondersteuning voor HTTP/2 in Azure CDN

HTTP/2 is een belangrijke revisie in HTTP/1.1\. Het biedt sneller webprestaties, verminderde reactietijd, en verbeterde gebruikerservaring, behoud van de vertrouwde HTTP-methoden, -statuscodes en semantiek. Hoewel HTTP/2 is ontworpen voor gebruik met HTTP en HTTPS, ondersteunen veel webbrowsers van de client alleen HTTP/2 via TLS.

### <a name="http2-benefits"></a>HTTP/2-voordelen

De voordelen van HTTP/2 zijn:

*   **Multiplexing en gelijktijdigheid**

    Met behulp van HTTP 1.1, meerdere aanvragen van de resource is vereist voor meerdere TCP-verbindingen en elke verbinding heeft prestatieoverhead die ermee verbonden zijn. HTTP/2 kunnen meerdere resources op één TCP-verbinding worden aangevraagd.

*   **Headercompressie**

    Door het comprimeren van de HTTP-headers voor aangeboden resources tijd op de kabel aanzienlijk verminderd.

*   **Stream-afhankelijkheden**

    Stream-afhankelijkheden dat de client om aan te geven op de server welke resources prioriteit hebben.


## <a name="http2-browser-support"></a>Ondersteuning voor HTTP/2-Browser

Alle belangrijke browsers hebt ondersteuning voor HTTP/2 geïmplementeerd in hun huidige versies. Niet-ondersteunde browsers automatisch terugval naar HTTP/1.1.

|Browser|Minimale versie|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-cdn"></a>HTTP/2-ondersteuning in Azure CDN inschakelen

Ondersteuning voor HTTP/2 is momenteel actief is voor alle Azure CDN-profielen. Is geen verdere actie vereist van klanten.

## <a name="next-steps"></a>Volgende stappen

Zie voor de voordelen van het HTTP/2 in actie [deze demo van Akamai](https://http2.akamai.com/demo).

Voor meer informatie over HTTP/2, gaat u naar de volgende bronnen:

*   [Startpagina voor HTTP/2-specificatie](https://http2.github.io/)
*   [Officiële HTTP/2 Veelgestelde vragen](https://http2.github.io/faq/)
*   [Akamai HTTP/2-gegevens](https://http2.akamai.com/)

Zie voor meer informatie over de beschikbare functies van Azure CDN, de [overzicht van Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).