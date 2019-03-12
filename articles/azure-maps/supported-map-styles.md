---
title: Kaartstijlen ondersteund in Azure Maps | Microsoft Docs
description: Ondersteund door Azure Maps kaartstijlen
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 76ab49c7f28260249483bf3bc4387e8cbaca13b2
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570535"
---
# <a name="azure-maps-supported-map-styles"></a>Azure Maps ondersteunde kaartstijlen
Azure kaarten biedt ondersteuning voor diverse verschillende ingebouwde kaartstijlen zoals hieronder wordt beschreven.

## <a name="road"></a>kaartachtergrond als weg
Een **weg** kaart is een standaard-kaart die wordt weergegeven wegen, natuurlijke en kunstmatige functies samen met de labels voor deze functies.

![kaartachtergrond als weg](./media/supported-map-styles/road.png)

**Van toepassing API's:**
* [Kaartafbeelding](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Tegel Serviceoverzicht](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Kaartbesturingselement JS
* Android kaartbesturingselement

## <a name="satellite"></a>satellite 
De **satelliet** stijl is een combinatie van satelliet en Luchtfoto beelden.

![satellite](./media/supported-map-styles/satellite.png)

**Van toepassing API's:**
* [Satelliet-tegel](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Kaartbesturingselement JS
* Android kaartbesturingselement

## <a name="satelliteroadlabels"></a>satellite_road_labels
De stijl van deze kaart is een hybride van wegen en labels satelliet en Luchtfoto beelden heen.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**Van toepassing API's:**
* Kaartbesturingselement JS
* Android kaartbesturingselement

## <a name="grayscaledark"></a>grayscale_dark
**Weg in grijswaarden donker** is een donkere versie van de roadmap-stijl.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**Van toepassing API's:**
* Kaartbesturingselement JS 
* Android kaartbesturingselement

## <a name="night"></a>'s nachts
**'s nachts** is een donkere versie van de stijl overzicht met gekleurde wegen en symbolen.

!['s nachts](./media/supported-map-styles/night.png)

**Van toepassing API's:**
* Kaartbesturingselement JS
* Android kaartbesturingselement

## <a name="roadshadedrelief"></a>road_shaded_relief
**weg gearceerde vrijstelling** is een Azure-kaarten belangrijkste stijl is voltooid met beschrijvingen van de aarde.

![gearceerde vrijstelling](./media/supported-map-styles/shaded-relief.png)

**Van toepassing API's:**
* [Tegel Serviceoverzicht](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Kaartbesturingselement JS
* Android kaartbesturingselement
