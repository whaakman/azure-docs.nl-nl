---
title: Azure Search met behulp van Powershell-scripts beheren | Microsoft Docs
description: Uw Azure Search-service met de PowerShell-scripts beheren. Maken of bijwerken van een Azure Search-service en Azure Search administratorsleutels beheren
services: search
documentationcenter: 
author: seansaleh
manager: mblythe
editor: 
tags: azure-resource-manager
ms.assetid: 9b3dc1f2-3619-4235-ba1f-d2d6f5c45dd5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: powershell
ms.date: 08/15/2016
ms.author: seasa
ms.openlocfilehash: aa51c846efef12461ec382274199bc049c42aaa3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-your-azure-search-service-with-powershell"></a>Uw Azure Search-service met PowerShell beheren
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> 
> 

Dit onderwerp beschrijft de PowerShell-opdrachten uit te voeren veel van de beheertaken voor Azure Search-services. U kunt zien bij het maken van een service voor zoeken, schalen en het beheren van de API-sleutels.
Deze opdrachten parallel de beheeropties beschikbaar in de [Azure Search Management REST API](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Vereisten
* U moet Azure PowerShell 1.0 of hoger hebben. Zie voor instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
* U moet zijn aangemeld met uw Azure-abonnement in PowerShell zoals hieronder wordt beschreven.

Eerst moet u aanmelden bij Azure met deze opdracht:

    Login-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het bijbehorende wachtwoord in het dialoogvenster voor Microsoft Azure-aanmelding.

U kunt ook [Meld u aan niet-interactief met een service-principal](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Als u meerdere Azure-abonnementen hebt, moet u uw Azure-abonnement instellen. Een lijst van uw huidige abonnementen wilt bekijken, moet u deze opdracht uitvoeren.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Geef het abonnement door de volgende opdracht uitvoeren. In het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

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
Nu dat uw service hebt gemaakt, kunt u de volgende stappen uitvoeren: bouwen een [index](search-what-is-an-index.md), [query uitvoeren in een index](search-query-overview.md), en ten slotte maken en beheren van uw eigen-zoektoepassing die gebruikmaakt van Azure Search.

* [Een Azure Search-index maken in de Azure portal](search-create-index-portal.md)
* [Query uitvoeren op een Azure Search-index met behulp van de Search Explorer in de Azure portal](search-explorer.md)
* [Stel een indexeerfunctie om gegevens te laden van andere services](search-indexer-overview.md)
* [Het gebruik van Azure Search in .NET](search-howto-dotnet-sdk.md)
* [Analyseer het verkeer van uw Azure Search](search-traffic-analytics.md)

