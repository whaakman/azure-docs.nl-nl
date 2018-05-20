---
title: Azure Event raster SDK 's
description: Beschrijft de SDK's voor Azure Event raster. Deze SDK's bieden management, publiceren en verbruik.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 82f08341f8c96695a4ceddb19d1b610d70b89f88
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Gebeurtenis raster SDK's voor beheer en publiceren

Gebeurtenis raster biedt SDK's waarmee u programmatisch te beheren van uw resources en gebeurtenissen te plaatsen.

## <a name="management-sdks"></a>Management SDK 's

Het beheer van SDK's kunt u maken, bijwerken en verwijderen van gebeurtenis raster onderwerpen en abonnementen. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure.eventgrid-2018-01-01/azure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>Gegevens vlak SDK 's

Het vlak van gegevens met de SDK's kunnen u boeken van gebeurtenissen naar onderwerpen met verificatie, vormen de gebeurtenis en asynchroon posten naar het opgegeven eindpunt wordt gelet. Ze Hiermee kunt u de eerste gebeurtenissen van derden gebruiken. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor gebeurtenis raster opdrachten in de Azure CLI, [Azure CLI](/cli/azure/eventgrid).
* Zie voor gebeurtenis raster opdrachten in PowerShell, [PowerShell](/powershell/module/azurerm.eventgrid).