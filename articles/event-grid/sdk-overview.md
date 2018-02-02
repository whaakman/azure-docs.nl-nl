---
title: Azure Event raster SDK 's
description: Beschrijft de SDK's voor Azure Event raster. Deze SDK's bieden management, publiceren en verbruik.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9c56e4c3314090ad55017d5c681a0cfd7bf5722c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Gebeurtenis raster SDK's voor beheer en publiceren

Gebeurtenis raster biedt SDK's waarmee u programmatisch te beheren van uw resources en gebeurtenissen te plaatsen.

## <a name="management-sdks"></a>Management SDK 's

Het beheer van SDK's kunt u maken, bijwerken en verwijderen van gebeurtenis raster onderwerpen en abonnementen. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="publish-sdks"></a>SDK's publiceren

Het publiceren SDK's kunnen u gebeurtenissen naar onderwerpen boeken door wordt gelet op verifiëren, vormen de gebeurtenis en asynchroon posten naar het opgegeven eindpunt. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor gebeurtenis raster opdrachten in de Azure CLI, [Azure CLI](/cli/azure/eventgrid).
* Zie voor gebeurtenis raster opdrachten in PowerShell, [PowerShell](/powershell/module/azurerm.eventgrid).