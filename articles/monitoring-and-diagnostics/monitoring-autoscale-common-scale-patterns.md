---
title: Overzicht van algemene patronen voor automatisch schalen
description: Informatie over sommige van de algemene patronen automatisch schalen uw Azure-resource.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 84727ec3694f64d40ad002a248a255df9074d7f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263257"
---
# <a name="overview-of-common-autoscale-patterns"></a>Overzicht van algemene patronen voor automatisch schalen
Dit artikel worden enkele van de algemene patronen voor het schalen van uw resources in Azure.

Azure Monitor automatisch schalen geldt alleen voor virtuele Machine Scale Sets (VMSS), cloudservices, app-serviceabonnementen en app service-omgevingen. 

# <a name="lets-get-started"></a>Hiermee kunnen aan de slag

In dit artikel wordt ervan uitgegaan dat u bekend met automatisch schalen bent. U kunt [hier uw resource schalen aan de slag][1]. Hier volgen enkele van de algemene patronen van de schaal.

## <a name="scale-based-on-cpu"></a>Schalen op basis van CPU

U hebt een web-app (VMSS/cloud service rol) en 

- U wilt scale-out/schaal in op basis van de CPU.
- Bovendien wilt u Zorg dat er een minimum aantal exemplaren. 
- Bovendien wilt u ervoor te zorgen dat u een limiet ingesteld op het aantal exemplaren die om te kunnen worden geschaald.

![Schalen op basis van CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Anders weekdagen tegenover tijdens het weekend schalen

U hebt een web-app (VMSS/cloud service rol) en

- Op het gewenste 3 exemplaren standaard (weekdagen)
- U geen verkeer verwacht tijdens het weekend en moet daarom worden geschaald naar beneden 1 exemplaar in het weekend.

![Anders weekdagen tegenover tijdens het weekend schalen][3]

## <a name="scale-differently-during-holidays"></a>Anders tijdens de feestdagen schalen

U hebt een web-app (VMSS/cloud service rol) en 

- U wilt omhoog/omlaag schalen op basis van CPU-gebruik standaard
- Echter tijdens feestdagen (of specifieke dagen die belangrijk voor uw bedrijf zijn) wilt u de standaardinstellingen overschrijven en meer capaciteit hebben tot uw beschikking staan.

![Anders op feestdagen schaal][4]

## <a name="scale-based-on-custom-metric"></a>Schalen op basis van aangepaste metrische gegevens

U hebt een webfront-end en een API-laag die met de back-end communiceert. 

- U wilt schalen van de API-laag op basis van aangepaste gebeurtenissen in de front-end (voorbeeld: U wilt schalen van uw afrekenen op basis van het aantal items in de winkelwagen)

![Schalen op basis van aangepaste metrische gegevens][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png