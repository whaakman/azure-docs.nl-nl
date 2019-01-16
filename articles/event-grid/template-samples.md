---
title: Voorbeeldsjablonen van Azure Resource Manager - Event Grid | Microsoft Docs
description: Voorbeeldsjablonen van Azure Resource Manager voor Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 01/04/2019
ms.author: tomfitz
ms.openlocfilehash: 788d23c7bddd90c1e12a118742c651eb9759529c
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062958"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-sjablonen voor Event Grid

Zie [Microsoft.EventGrid resource types](/azure/templates/microsoft.eventgrid/allversions) (Microsoft.EventGrid-resourcetypen) voor de JSON-syntaxis en eigenschappen die u kunt gebruiken in een sjabloon. De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen voor Event Grid.

| | |
|-|-|
|**Event Grid-abonnementen**||
| [Aangepast onderwerp en abonnement met WebHook-eindpunt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Hiermee wordt een aangepast Event Grid-onderwerp geïmplementeerd. Hiermee maakt u een abonnement op dat aangepaste onderwerp dat gebruikmaakt van een WebHook-eindpunt. |
| [Abonnement op aangepast onderwerp met EventHub-eindpunt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Hiermee maakt u een Event Grid-abonnement voor een aangepast onderwerp. Het abonnement maakt gebruik van een Event Hub voor het eindpunt. |
| [Azure-abonnement of abonnement op resourcegroep](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Hiermee abonneert u zich op gebeurtenissen voor een resourcegroep of neemt u een Azure-abonnement. De resourcegroep die u als het doel opgeeft tijdens de implementatie, is de bron van gebeurtenissen. Het abonnement maakt gebruik van een webhook voor het eindpunt. |
| [Blob-opslagaccount en -abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Hiermee implementeert u een Azure Blob-opslagaccount en abonneert u zich op gebeurtenissen van die opslagaccount. |
| | |
