---
title: Kaartstijlen ondersteund in Azure Maps | Microsoft Docs
description: Ondersteund door Azure Maps kaartstijlen
author: walsehgal
ms.author: v-musehg
ms.date: 10/02/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: c8edaba8de597e3e76e760e1f5109006338a663c
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238817"
---
# <a name="azure-maps-supported-map-styles"></a>Azure Maps ondersteunde kaartstijlen
Azure maps ondersteunen vier verschillende ingebouwde kaartstijlen. De stijlen op met de bijbehorende beschrijvingen worden hieronder vermeld.

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

## <a name="satelliteroadlabels"></a>satellite_road_labels
De stijl van deze kaart is een hybride van wegen en labels satelliet en Luchtfoto beelden heen.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**Van toepassing API's:**
* Kaartbesturingselement JS

## <a name="grayscaledark"></a>grayscale_dark
**Weg in grijswaarden donker** is een donkere versie van de roadmap-stijl.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**Van toepassing API's:**
* Kaartbesturingselement JS 