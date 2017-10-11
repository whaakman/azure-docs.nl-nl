---
title: HTTP-/ 2-ondersteuning in Azure CDN | Microsoft Docs
description: Meer informatie over ondersteuning voor HTTP/2- en CDN.
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 4f8dd685c3ae89535217d7a17a01c5129ca7e6e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="http2-support-in-azure-cdn"></a>HTTP-/ 2-ondersteuning in Azure CDN

HTTP/2 is een grote revisie naar HTTP/1.1\. Het biedt sneller webprestaties, verminderde reactietijd, en verbeterde gebruikerservaring, terwijl de bekende HTTP-methoden, statuscodes en semantiek behouden. Hoewel HTTP/2 is ontworpen voor gebruik met HTTP en HTTPS, ondersteunen veel client webbrowsers alleen HTTP/2 via TLS.

###<a name="http2-benefits"></a>Voordelen van de HTTP-/ 2

De voordelen van het HTTP/2 zijn onder andere:

*   **Multiplex en gelijktijdigheid**

    HTTP 1.1 meerdere waardoor meerdere aanvragen van de resource is vereist voor meerdere TCP-verbindingen en elke verbinding heeft prestatieoverhead gekoppeld. HTTP/2 kunt meerdere resources moet worden gevraagd op een enkele TCP-verbinding.

*   **Headercompressie**

    Door het comprimeren van de HTTP-headers voor aangeboden bronnen tijd op de kabel aanzienlijk verminderd.

*   **Stroom-afhankelijkheden**

    Afhankelijkheden van de stroom kunnen de client om aan te geven met de server die bronnen prioriteit hebben.


##<a name="http2-browser-support"></a>Ondersteuning voor HTTP/2-Browser

Alle belangrijke browsers hebt HTTP/2-ondersteuning in de versies van hun huidige geïmplementeerd. Niet-ondersteunde browsers wordt automatisch fallback naar HTTP/1.1.

|Browser|Minimale versie|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

##<a name="enabling-http2-support-in-azure-cdn"></a>HTTP-/ 2-ondersteuning in Azure CDN inschakelen

HTTP-/ 2-ondersteuning is momenteel actief is voor **Azure CDN van Akamai** en **Azure CDN van Verizon** profielen. Er is geen verdere actie vereist van klanten.

##<a name="next-steps"></a>Volgende stappen

Zie voor de voordelen van het HTTP/2 in actie [deze demo van Akamai](https://http2.akamai.com/demo).

Voor meer informatie over HTTP/2, gaat u naar de volgende bronnen:

*   [HTTP-/ 2-specificatie startpagina](https://http2.github.io/)
*   [Officiële HTTP/2 Veelgestelde vragen](https://http2.github.io/faq/)
*   [Akamai HTTP/2-gegevens](https://http2.akamai.com/)

Zie voor meer informatie over de beschikbare functies van Azure CDN, de [overzicht van Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).