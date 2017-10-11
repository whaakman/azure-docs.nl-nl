---
title: Web-App Resources verplaatsen naar een andere resourcegroep
description: Beschrijft de scenario's waarin kunt u Web-Apps en App-Services van een resourcegroep naar een andere.
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 1b5059dc052005b6079f70ecf6771a3771df8d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="supported-move-configurations"></a>Ondersteunde Move-configuraties
Kunt u Web-App van Azure-resources met behulp van de [Resource Manager verplaatsen Resources API](../azure-resource-manager/resource-group-move-resources.md).

Azure Web Apps biedt momenteel ondersteuning voor de volgende move-scenario's:

* De volledige inhoud van een resourcegroep (web-apps, app service-abonnementen en certificaten) naar een andere resourcegroep verplaatsen. 
   > [!Note]
   > De resourcegroep voor de bestemming kan geen Microsoft.Web bronnen in dit scenario bevatten.

* Afzonderlijke web-apps naar een andere resourcegroep verplaatsen tijdens het hosten van ze nog steeds in de huidige app service-abonnement (het app service-abonnement blijft in de oude resourcegroep).


