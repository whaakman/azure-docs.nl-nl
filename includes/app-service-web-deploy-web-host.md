---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 87fd6b626efb60c7fc7ec8896f2c3758ae5cc33c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132876"
---
### <a name="app-service-plan"></a>App Service-abonnement
Hiermee maakt u de service-plan voor het hosten van de web-app. U geeft u de naam van het abonnement via de **hostingPlanName** parameter. De locatie van het plan is de locatie die wordt gebruikt voor de resourcegroep. De grootte van de prijzen laag- en werkrollen zijn opgegeven in de **sku** en **workerSize** parameters

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

