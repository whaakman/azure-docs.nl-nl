---
title: Azure Event raster SDK 's
description: Beschrijft de SDK's voor Azure Event raster. Deze SDK's bieden management, publiceren en verbruik.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 06/29/2018
ms.author: tomfitz
ms.openlocfilehash: 3c085074863aa166a5766116b6c63b7dc341ad96
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130832"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Gebeurtenis raster SDK's voor beheer en publiceren

Gebeurtenis raster biedt SDK's waarmee u programmatisch te beheren van uw resources en gebeurtenissen te plaatsen.

## <a name="management-sdks"></a>Management SDK 's

Het beheer van SDK's kunt u maken, bijwerken en verwijderen van gebeurtenis raster onderwerpen en abonnementen. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
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

* Bijvoorbeeld toepassingen, Zie [gebeurtenis raster codevoorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid).
* Zie voor een inleiding tot gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor gebeurtenis raster opdrachten in de Azure CLI, [Azure CLI](/cli/azure/eventgrid).
* Zie voor gebeurtenis raster opdrachten in PowerShell, [PowerShell](/powershell/module/azurerm.eventgrid).
