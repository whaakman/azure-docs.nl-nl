---
title: Overzicht van algemene patronen voor automatisch schalen
description: Leer enkele van de algemene patronen voor automatisch schalen uw resource in Azure.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: ae0ad4422c8c8ce01ed9d00d2966212d1bdb5c7f
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440994"
---
# <a name="overview-of-common-autoscale-patterns"></a>Overzicht van algemene patronen voor automatisch schalen
In dit artikel worden enkele van de algemene patronen voor het schalen van uw resource in Azure beschreven.

Automatisch schalen van Azure Monitor is alleen bedoeld voor [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="lets-get-started"></a>Hiermee kunnen aan de slag

In dit artikel wordt ervan uitgegaan dat u bekend met automatisch schalen bent. U kunt [hier uw resource schalen aan de slag][1]. Hier volgen enkele van de algemene patronen van de schaal.

## <a name="scale-based-on-cpu"></a>Schaal op basis van CPU

U hebt een web-app (/ VMSS/cloud service rol) en

- U wilt scale-out/schaal in op basis van de CPU.
- Bovendien wilt u om ervoor te zorgen dat er een minimum aantal exemplaren.
- U wilt ook, om ervoor te zorgen dat u een limiet ingesteld op het aantal exemplaren die kan worden geschaald tot.

![Schaal op basis van CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Anders weekdagen vs tijdens het weekend schalen

U hebt een web-app (/ VMSS/cloud service rol) en

- Op het gewenste 3 exemplaren standaard (op weekdagen)
- U niet verwacht dat verkeer in het weekend en daarom moet worden geschaald naar 1 exemplaar in het weekend.

![Anders weekdagen vs tijdens het weekend schalen][3]

## <a name="scale-differently-during-holidays"></a>Anders tijdens de feestdagen schalen

U hebt een web-app (/ VMSS/cloud service rol) en

- U wilt omhoog/omlaag schalen op basis van CPU-gebruik standaard
- Tijdens de feestdagen (of specifieke dagen die belangrijk voor uw bedrijf zijn) wilt u echter overschrijven de standaardinstellingen en hebt meer capaciteit beschikken.

![Schaal anders op feestdagen][4]

## <a name="scale-based-on-custom-metric"></a>Schaal op basis van aangepaste meetwaarde

U hebt een webfront-end en een API-laag die met de back-end communiceert.

- U wilt schalen van de API-laag op basis van aangepaste gebeurtenissen in de front-end (voorbeeld: U wilt uw afrekenen op basis van het aantal items in de winkelwagen schalen)

![Schaal op basis van aangepaste meetwaarde][5]

<!--Reference-->
[1]: ./autoscale-get-started.md
[2]: ./media/autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/autoscale-common-scale-patterns/custom-metric-scale.png
