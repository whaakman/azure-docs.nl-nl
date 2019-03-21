---
title: Verificatietokens doorgeven aan Azure Media Services | Microsoft Docs
description: Meer informatie over het verzenden van verificatietokens van de client naar de Azure Media Services-sleutelleveringsservice
services: media-services
keywords: beveiliging van inhoud, DRM-tokenverificatie
documentationcenter: ''
author: dbgeorge
manager: jasonsue
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: dwgeo
ms.openlocfilehash: 71925a1ee67956df45901950b2a59fa4c1b458a7
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260075"
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>Informatie over hoe clients tokens doorgeven aan de sleutel leveringsservice voor Azure Media Services
Klanten vragen vaak hoe een speler kunt doorgeven tokens aan de sleutel leveringsservice voor Azure Media Services om te verifiëren, zodat de speler de sleutel kunt verkrijgen. Media Services biedt ondersteuning voor het simple webtoken (SWT) en JSON Web Token (JWT) geformatteerd. Tokenverificatie wordt toegepast op elk type sleutel, ongeacht of u algemene versleuteling of Advanced Encryption Standard (AES) envelop versleuteling in het systeem.

 Afhankelijk van de speler en het platform die is gericht, kunt u het token met de speler doorgeven in de volgende manieren:

- Via de HTTP-autorisatie-header.
    > [!NOTE]
    > Het voorvoegsel 'Bearer' wordt verwacht per de OAuth 2.0-specificaties. Een voorbeeld-speler met de configuratie van sessietoken wordt gehost op de Azure Media Player [demo pagina](https://ampdemo.azureedge.net/). Om in te stellen de videobron, kies **AES (JWT-Token)** of **AES (SWT Token)**. Het token wordt doorgegeven via de autorisatie-header.

- Via het toevoegen van een URL queryparameter met ' token = tokenvalue. "  
    > [!NOTE]
    > Het voorvoegsel 'Bearer' wordt niet verwacht. Omdat het token wordt verzonden via een URL, moet u de token tekenreeks beveiligen. Hier volgt een C# voorbeeldcode die laat zien hoe u om dit te doen:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- Via de CustomData-veld.
Deze optie wordt gebruikt voor de PlayReady-licentie aanschaf alleen via de CustomData-veld van de PlayReady-licentie overname uitdaging. In dit geval moet het token in het XML-document zijn, zoals hier wordt beschreven:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Plaats uw verificatietoken in het Token-element.

- Via een alternatieve afspeellijst voor HTTP Live Streaming (HLS). Als u wilt configureren tokenverificatie voor AES + HLS afspelen op iOS/Safari, er is een manier die u rechtstreeks in het token verzenden kunt. Zie voor meer informatie over hoe u de afspeellijst voor dit scenario met alternatieve [blogbericht](https://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]