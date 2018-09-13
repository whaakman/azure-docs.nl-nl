---
title: Kaartstijlen ondersteund in Azure Maps | Microsoft Docs
description: Ondersteund door Azure Maps kaartstijlen
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 8f0910e9040c962bae30a33b91a93e71e692dfdb
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713439"
---
# <a name="azure-maps-supported-map-styles"></a>Azure Maps ondersteunde kaartstijlen
Azure maps ondersteunen vier verschillende ingebouwde kaartstijlen. De stijlen, samen met de bijbehorende beschrijvingen worden hieronder vermeld.

## <a name="road"></a>Kaartachtergrond als weg
Een **weg** kaart is een standaard-kaart die wordt weergegeven wegen, natuurlijke en kunstmatige functies samen met de labels voor deze functies.

![Kaartachtergrond als weg](./media/supported-map-styles/road.png)

**Van toepassing API's:**
* [Kaartafbeelding](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Tegel Serviceoverzicht](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Kaartbesturingselement JS

## <a name="satellite"></a>Satelliet 
De **satelliet** stijl is een combinatie van satelliet en Luchtfoto beelden.

![Satelliet](./media/supported-map-styles/satellite.png)

**Van toepassing API's:**
* [Satelliet-tegel](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Kaartbesturingselement JS

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
De stijl van deze kaart is een hybride van wegen en labels satelliet en Luchtfoto beelden heen.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**Van toepassing API's:**
* Kaartbesturingselement JS

## <a name="grayscaledark"></a>Grayscale_Dark
**Weg in grijswaarden donker** is een donkere versie van de roadmap-stijl.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**Van toepassing API's:**
* Kaartbesturingselement JS 