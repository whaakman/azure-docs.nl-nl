---
title: Verificatietoken doorgeven aan Azure Media Services | Microsoft Docs
description: Ontdek hoe verificatietokens van de client verzendt met Azure Media Services sleutellevering service
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
ms.openlocfilehash: 0e56726266898e5738dd797a8a019987d457170e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="how-clients-pass-tokens-to-azure-media-services-key-delivery-service"></a>Hoe clients tokens doorgeven met Azure Media Services sleutellevering service
We vragen over hoe een speler token naar onze services sleutellevering, die wordt ophalen geverifieerd kan doorgeven voortdurend ophalen en de speler haalt de sleutel. Eenvoudige Web Token (SWT) en ondersteund JSON Web Token (JWT) deze twee token indelingen. Tokenverificatie kan worden toegepast op elk type sleutel – ongeacht u Common Encryption of AES envelope versleuteling doen in het systeem.

Dit zijn de volgende manieren u het token met de speler doorgeven kan, is afhankelijk van de speler en platform u ontwikkelt voor:
- Via het HTTP-autorisatie-header.
> [!NOTE]
> Houd er rekening mee dat het voorvoegsel 'Bearer' wordt verwacht dat per OAuth 2.0-specificaties. Er is een voorbeeld-speler met Token configuratie gehost op Azure Media Player [voorbeeldpagina](http://ampdemo.azureedge.net/). Kies AES (JWT-Token) of AES (SWT Token) videobron instellen. Token wordt doorgegeven via de autorisatie-header.

- Via het toevoegen van een Url-queryparameter met ' token tokenvalue = '.  
> [!NOTE]
> Houd er rekening mee dat er geen voorvoegsel 'Bearer' wordt verwacht. Aangezien token wordt verzonden via een URL, moet u de tokentekenreeks beveiligen. Hier volgt een C#-voorbeeldcode op te werk:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

- Via CustomData veld.
Licentie voor PlayReady overname alleen via het veld CustomData van de uitdaging PlayReady licentie overname. In dit geval wordt moet het token in het xml-document die hieronder worden beschreven.

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```
Uw verificatietoken plaats in de <Token> element.

- Via een alternatieve HLS afspeellijst. Als u wilt configureren tokenverificatie voor AES + HLS afspelen op iOS/Safari is een manier die u rechtstreeks in het token kan verzenden. Raadpleeg dit [blogbericht](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) over de alternatieve de afspeellijst om dit scenario.

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]