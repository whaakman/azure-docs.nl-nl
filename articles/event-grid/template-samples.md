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
ms.date: 03/27/2018
ms.author: tomfitz
ms.openlocfilehash: f4d6a663b4e0d2c2166028051e713668534b20bc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-sjablonen voor Event Grid

De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen voor Event Grid.

| | |
|-|-|
|**Event Grid-abonnementen**||
| [Aangepast onderwerp en abonnement met WebHook-eindpunt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Hiermee wordt een aangepast Event Grid-onderwerp geïmplementeerd. Hiermee maakt u een abonnement op dat aangepaste onderwerp dat gebruikmaakt van een WebHook-eindpunt. |
| [Abonnement op aangepast onderwerp met EventHub-eindpunt](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeCustomTopicToEventHub.json)| Hiermee maakt u een Event Grid-abonnement op een aangepast onderwerp dat al bestaat. Het abonnement maakt gebruik van een Event Hub voor het eindpunt. |
| [Abonnement op resourcegroup](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeResourceGroupToWebHook.json)| Hiermee abonneert u zich op gebeurtenissen voor een resourcegroep. De resourcegroep die u als het doel opgeeft tijdens de implementatie, is de bron van gebeurtenissen. Het abonnement maakt gebruik van een Event Hub voor het eindpunt. |
| [Blob-opslagaccount en -abonnement](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/createBlobAndSubscribe.json)| Hiermee implementeert u een Azure Blob-opslagaccount en abonneert u zich op gebeurtenissen van die opslagaccount. |
| | |
