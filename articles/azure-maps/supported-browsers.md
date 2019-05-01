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
ms.openlocfilehash: 84c5dbcf5073ba8c0ae662af019cde590a9adf10
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686646"
---
# <a name="web-sdk-supported-browsers"></a>Browsers die Web-SDK ondersteunen

Azure Maps Web SDK biedt een Help-functie met de naam [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-). Deze functie wordt gedetecteerd of een webbrowser het minimale aantal WebGL-functies die vereist zijn ter ondersteuning van het laden en weergeven van het kaartbesturingselement bevat. Hier volgt een voorbeeld van hoe u de functie te gebruiken:

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Bureaublad

Azure Maps Web SDK ondersteunt de volgende pc-browsers:

- Microsoft Edge (huidige en vorige versie)
- Google Chrome (huidige en vorige versie)
- Mozilla Firefox (huidige en vorige versie)
- Apple Safari (Mac OS X) (huidige en vorige versie)

Zie ook [gericht op verouderde browsers](#Target-Legacy-Browsers) verderop in dit artikel.

## <a name="mobile"></a>Mobiele telefoon

Azure Maps Web SDK ondersteunt de volgende mobiele browsers:

- Android
  - Huidige versie van Chrome op Android 6.0 en hoger
  - Chrome WebView op Android 6.0 en hoger
- iOS
  - Safari op de huidige en vorige primaire versie van iOS Mobile
  - UIWebView en WKWebView op de huidige en vorige primaire versie van iOS
  - Huidige versie van Chrome voor iOS

> [!TIP]
> Als u inhoud van een kaart in een mobiele toepassing insluiten wilt met behulp van een besturingselement WebView, u mogelijk liever gebruik van de [npm-pakket van de Azure Maps Web SDK](https://www.npmjs.com/package/azure-maps-control) in plaats van die verwijst naar de versie van de SDK die wordt gehost op Azure Content Delivery Netwerk. Deze aanpak vermindert de tijd om te laden omdat de SDK is al op het apparaat van de gebruiker zijn en hoeft niet te worden gedownload tijdens de uitvoering.

## <a name="nodejs"></a>Node.js

De volgende Web SDK-modules worden ook ondersteund in Node.js:

- Services-module ([documentatie](how-to-use-services-module.md) | [npm-module](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Doel verouderde browsers

Mogelijk wilt u een oudere browsers die bieden geen ondersteuning voor WebGL of die slechts beperkt worden ondersteund voor dit doel. In dergelijke gevallen raden wij aan dat u Azure kaarten-services samen met een open-source-kaartbesturingselement zoals [brochure](https://leafletjs.com/). Hier volgt een voorbeeld:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps + brochure" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + brochure</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Azure Maps Web SDK:

> [!div class="nextstepaction"]
> [Kaartbesturingselement](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Services-module](how-to-use-services-module.md)
