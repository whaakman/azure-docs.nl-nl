---
title: Web SDK ondersteunde browsers - Azure-kaarten | Microsoft Docs
description: Meer informatie over ondersteunde browsers voor Azure Maps Web SDK
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: bc876fbf0eb15f887d57d4ddcca2301ef7233afa
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577336"
---
# <a name="web-sdk-supported-browsers"></a>Web SDK ondersteunde browsers

Azure Maps Web SDK biedt een Help-functie [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-) vereist om te detecteren of een webbrowser met een minimaal WebGL functies ter ondersteuning van het laden en weergeven van het kaartbesturingselement. 

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    //Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Bureaublad

Azure Maps Web SDK biedt ondersteuning voor de volgende browsers op desktopcomputers.

- De huidige en vorige versie van Microsoft Edge 
- De huidige en vorige versies van Chrome 
- De huidige en vorige versies van Firefox 
- De huidige en vorige versie van Safari (Mac OS X) 

Zie ook [gericht op verouderde browsers](#Target-Legacy-Browsers).

## <a name="mobile"></a>Mobiele telefoon

Azure Maps Web SDK biedt ondersteuning voor de volgende mobiele browsers.

-  Android
    * Huidige versie van Chrome op Android 6.0 en hoger
    * Chrome WebView op Android 6.0 en hoger
- iOS
    * Safari op de huidige en vorige primaire versie van iOS Mobile
    * UIWebView en WKWebView op de huidige en vorige primaire versie van iOS
    * Huidige versie van Chrome voor iOS

> [!TIP]
> Als u een kaart in een mobiele toepassing met behulp van een besturingselement WebView insluit, kunt u wellicht beter met behulp van de [npm-pakket van de Azure Maps Web SDK](https://www.npmjs.com/package/azure-maps-control) in plaats van die verwijst naar de CDN gehost versie van de SDK. Zo beperkt u tijd te laden als de SDK al worden op het apparaat van de gebruiker en niet moeten worden gedownload tijdens runtime.

## <a name="nodejs"></a>Node.js

De volgende Web SDK-modules worden ook ondersteund in Node.js:

- Services-module ([documentatie](how-to-use-services-module.md) | [npm-module](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Doel verouderde browsers

Als u oudere browsers die mogelijk niet ondersteund of beperkt worden ondersteund voor WebGL als doel moet, is het raadzaam de Services van Azure kaarten gebruiken in combinatie met een open-source kaartbesturingselement zoals [brochure](https://leafletjs.com/). 


<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps + brochure" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + brochure</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Azure Maps Web SDK.

> [!div class="nextstepaction"]
> [Kaartbesturingselement](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Services-module](how-to-use-services-module.md)
