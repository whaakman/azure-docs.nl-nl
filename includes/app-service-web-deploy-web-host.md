---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 87fd6b626efb60c7fc7ec8896f2c3758ae5cc33c
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134140"
---
### <a name="app-service-plan"></a>App Service-plan
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

