---
title: Verificatietokens doorgeven aan Azure Media Services | Microsoft Docs
description: Ontdek hoe verificatietokens van de client verzendt met de service Azure Media Services-sleutellevering
services: media-services
keywords: beveiliging van inhoud, DRM tokenverificatie
documentationcenter: 
author: dbgeorge
manager: jasonsue
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: dwgeo
ms.openlocfilehash: 7d143242231444b8557a303d1b504d5311693f1a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>Meer informatie over hoe clients tokens doorgeven aan de service Azure Media Services-sleutellevering
Klanten wordt vaak vragen hoe een speler kan doorgeven tokens met de service Azure Media Services sleutellevering voor verificatie zodat Windows media player kan de sleutel verkrijgen. Media Services ondersteunt de eenvoudige web token (SWT) en JSON Web Token (JWT) geformatteerd. Tokenverificatie wordt toegepast op elk type sleutel, ongeacht of u common encryption of Advanced Encryption Standard (AES) envelop versleuteling in het systeem.

 Afhankelijk van de player en platform die is gericht, kunt u het token doorgeven met de speler in de volgende manieren:

- Via het HTTP-autorisatie-header.
    > [!NOTE]
    > Het voorvoegsel 'Bearer' wordt verwacht per OAuth 2.0-specificaties. Een voorbeeld-speler met de configuratie van de token wordt gehost op de Azure Media Player [voorbeeldpagina](http://ampdemo.azureedge.net/). Kies de videobron stelt **AES (JWT-Token)** of **AES (SWT Token)**. Het token wordt doorgegeven via de autorisatie-header.

- Via het toevoegen van een URL queryparameter met ' token = tokenvalue. "  
    > [!NOTE]
    > Het voorvoegsel 'Bearer' wordt niet verwacht. Omdat het token wordt verzonden via een URL, moet u de tokentekenreeks beveiligen. Hier volgt een C# voorbeeldcode die laat zien hoe u dit doen:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- Via het veld CustomData.
Deze optie wordt gebruikt voor PlayReady licentie overname alleen via het veld CustomData van de uitdaging PlayReady licentie overname. In dit geval wordt het token moet in het XML-document zoals hier wordt beschreven:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Uw verificatietoken in het Token-element geplaatst.

- Via een afspeellijst met alternatieve HTTP Live Streaming (HLS). Als u wilt configureren voor AES + HLS-tokenverificatie afspelen op iOS/Safari, moet u er een manier die u rechtstreeks in het token verzenden kunt niet. Zie voor meer informatie over hoe u de afspeellijst om dit scenario alternatieve [blogbericht](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]