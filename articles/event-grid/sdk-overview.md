---
title: Azure Event Grid-SDK 's
description: Beschrijving van de SDK's voor Azure Event Grid. Deze SDK's bieden beheren, publiceren en verbruik.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/19/2019
ms.author: spelluru
ms.openlocfilehash: 53a1f50647e05b71afe6accb85f8a90cd8b7e253
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470396"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Event Grid-SDK's voor het beheren en publiceren

Event Grid biedt SDK's waarmee u programmatisch beheren van uw resources en gebeurtenissen kunt posten.

## <a name="management-sdks"></a>Management SDK 's

De management SDK's kunt u maken, bijwerken en verwijderen van event grid-onderwerpen en abonnementen. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>Gegevens vlak SDK 's

De gegevenslaag SDK's kunnen u onderwerpen gebeurtenissen kunt posten door Zorg ervoor dat u voor verificatie, vormen de gebeurtenis en asynchroon items plaatsen op het opgegeven eindpunt. Ze ook mogelijk om te gebruiken van eerste partij gebeurtenissen. De volgende SDK's zijn momenteel beschikbaar:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Volgende stappen

* Bijvoorbeeld toepassingen, Zie [Event Grid-codevoorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid).
* Zie voor een inleiding tot Event Grid, [wat is Event Grid?](overview.md)
* Zie voor Event Grid-opdrachten in de Azure CLI, [Azure CLI](/cli/azure/eventgrid).
* Zie voor Event Grid-opdrachten in PowerShell, [PowerShell](/powershell/module/azurerm.eventgrid).
