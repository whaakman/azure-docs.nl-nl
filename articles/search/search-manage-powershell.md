---
title: Azure Search beheren met Powershell-scripts | Microsoft Docs
description: Beheer uw Azure Search-service met behulp van PowerShell-scripts. Maken of bijwerken van een Azure Search-service en Azure Search-beheerder sleutels beheren
author: HeidiSteen
manager: cgronlun
tags: azure-resource-manager
services: search
ms.service: search
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/15/2016
ms.author: heidist
ms.openlocfilehash: bae9e2dcb4320c1da4f1d8e3c6ad50ce90195544
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958564"
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Uw Azure Search-service met PowerShell beheren
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Dit onderwerp beschrijft de PowerShell-opdrachten uit te voeren veel van de beheertaken voor Azure Search-services. We helpen bij het maken van een search-service, schalen en beheren van de API-sleutels.
Deze opdrachten parallel lopen aan het van beschikbare beheeropties in de [Management REST API van Azure Search](https://docs.microsoft.com/rest/api/searchmanagement).

## <a name="prerequisites"></a>Vereisten
* U moet Azure PowerShell 1.0 of hoger hebben. Zie voor instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
* U moet zijn aangemeld met uw Azure-abonnement in PowerShell, zoals hieronder wordt beschreven.

Eerst moet u aanmelden bij Azure met de volgende opdracht:

    Connect-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het bijbehorende wachtwoord in het dialoogvenster voor aanmelding van Microsoft Azure.

U kunt ook [Meld u aan niet-interactief met een service-principal](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

Als u meerdere Azure-abonnementen hebt, moet u uw Azure-abonnement instellen. Een lijst van uw huidige abonnementen wilt bekijken, moet u deze opdracht uitvoeren.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Als u het abonnement, moet u de volgende opdracht uitvoeren. In het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Opdrachten om u te helpen aan de slag
    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search"

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1

    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # View your resource
    $resource

    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key

    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19

    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource

    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource

## <a name="next-steps"></a>Volgende stappen
Nu dat uw service is gemaakt, kunt u de volgende stappen uitvoeren: bouw een [index](search-what-is-an-index.md), [query uitvoeren in een index](search-query-overview.md), en ten slotte maken en beheren van uw eigen-zoektoepassing die gebruikmaakt van Azure Search.

* [Een Azure Search-index maken in Azure portal](search-create-index-portal.md)
* [Query uitvoeren op een Azure Search-index met behulp van Search explorer in Azure portal](search-explorer.md)
* [Stel een indexeerfunctie voor het laden van gegevens uit andere services](search-indexer-overview.md)
* [Azure Search in .NET gebruiken.](search-howto-dotnet-sdk.md)
* [Analyseer het verkeer van uw Azure Search](search-traffic-analytics.md)

