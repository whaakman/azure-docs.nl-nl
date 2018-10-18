---
title: Azure ExpressRoute-controle, metrische gegevens en waarschuwingen | Microsoft Docs
description: Deze pagina bevat informatie over het controleren van ExpressRoute
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: fcdaf10ad30cee0e7b79304ff21d67560522875c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377794"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>Bewaking, metrische gegevens en waarschuwingen voor ExpressRoute

 Dit artikel helpt u inzicht in de ExpressRoute-controle, metrische gegevens en waarschuwingen. Azure Monitor is een centrale locatie voor alle metrische gegevens, waarschuwingen, diagnostische logboeken in heel Azure.

## <a name="circuit-metrics"></a>Circuit metrische gegevens

Om te navigeren naar **metrische gegevens**, klikt u op het ExpressRoute-pagina voor het circuit die u wilt bewaken. Onder **bewaking** vindt u de **metrische gegevens**. Selecteer BitsInPerSecond of BitsOutPerSecond en de aggregatie. U kunt eventueel toepassen splitsen die de metrische gegevens per peeringtype wordt weergegeven.

![Circuit metrische gegevens](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="metrics-per-peering"></a>Metrische gegevens per peering

Hier vindt u metrische gegevens voor persoonlijke, openbare en Microsoft-peering in bits per seconde.

![Metrische gegevens per peering](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>ExpressRoute-gateway-verbindingen in bits/seconden

![gateway-verbindingen](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>Waarschuwingen voor ExpressRoute-gateway-verbindingen

1. Als u wilt configureren van waarschuwingen, gaat u naar **Azure Monitor**, klikt u vervolgens op **waarschuwingen**.

  ![waarschuwingen](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Klik op **+ doel selecteren** en selecteer de verbindingsresource van de ExpressRoute-gateway.

  ![doel]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Definieer de details van de waarschuwing.

  ![Actiegroep](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)


4. Definiëren en toevoegen van de actiegroep.

  ![actiegroep toevoegen](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Waarschuwingen op basis van elke peering

 ![Wat](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Waarschuwingen voor activiteitenlogboeken voor circuits configureren

In de **waarschuwingscriteria**, kunt u **activiteitenlogboek** signaaltype en selecteer het signaal.

  ![een andere](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)

## <a name="next-steps"></a>Volgende stappen
* Configureer uw ExpressRoute-verbinding.
  
  * [Een circuit maken en wijzigen](expressroute-howto-circuit-arm.md)
  * [Een peeringconfiguratie maken en wijzigen](expressroute-howto-routing-arm.md)
  * [Een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)
