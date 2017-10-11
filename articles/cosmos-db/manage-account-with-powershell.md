---
title: Azure Cosmos DB Automation - beheer met Powershell | Microsoft Docs
description: Gebruik Azure Powershell beheren uw Azure DB die Cosmos-accounts.
services: cosmos-db
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: dimakwan
ms.openlocfilehash: 25c543528119410dff0684845a713dcb0d6151d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>Een Azure DB die Cosmos-account maken met PowerShell

De volgende handleiding beschrijft automatiseren beheer van uw Azure Cosmos DB database accounts met Azure Powershell-opdrachten. Dit omvat ook opdrachten voor het beheren van sleutels en failover prioriteiten in [meerdere landen/regio database accounts][scaling-globally]. Bijwerken van uw databaseaccount, kunt u consistentie beleid wijzigen en regio's toevoegen of verwijderen. Voor het beheer van de platformoverschrijdende van uw Azure DB die Cosmos-account, kunt u een gebruiken [Azure CLI](cli-samples.md), wordt de [Resource Provider REST-API][rp-rest-api], of de [Azure-portal ](create-documentdb-dotnet.md#create-account).

## <a name="getting-started"></a>Aan de slag

Volg de instructies in [installeren en configureren van Azure PowerShell] [ powershell-install-configure] installeren en aanmelden bij uw Azure Resource Manager-account in Powershell.

### <a name="notes"></a>Opmerkingen

* Als u wilt de volgende opdrachten worden uitgevoerd zonder gebruikersbevestiging, toevoeg-de `-Force` vlag aan de opdracht.
* De volgende opdrachten worden synchroon.

## <a id="create-documentdb-account-powershell"></a>Een Azure Cosmos DB-Account maken

Met deze opdracht kunt u een Azure DB die Cosmos-databaseaccount maken. Uw nieuwe databaseaccount configureren als een regio of [meerdere landen/regio] [ scaling-globally] met een bepaald [consistentie beleid](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`De locatienaam van de regio van het schrijven van het account van de database. Deze locatie is vereist voor een failover-prioriteitswaarde van 0 hebben. Er moet exact één schrijven regio per databaseaccount.
* `<read-region-location>`De locatienaam van de gelezen regio van het account van de database. Deze locatie is vereist voor failover-prioriteit waarde groter dan 0. Er zijn meer dan één lezen regio's per database-account.
* `<ip-range-filter>`Hiermee geeft u het IP-adressen of IP-adresbereiken in CIDR-vorm moet worden opgenomen als de lijst met toegestane van client-IP-adressen voor een bepaalde database-account. IP-adressen of-adresbereiken moet door komma's gescheiden en mag geen spaties bevatten. Zie voor meer informatie [Azure Cosmos DB-Firewallondersteuning](firewall-support.md)
* `<default-consistency-level>`De consistentiecontrole standaardniveau van de Azure DB die Cosmos-account. Zie voor meer informatie [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md).
* `<max-interval>`Bij gebruik met consistentie voor gebonden veroudering is vertegenwoordigt deze waarde de tijd hoeveelheid veroudering (in seconden) toegestaan. Geaccepteerde bereik voor deze waarde is 1-100.
* `<max-staleness-prefix>`Met consistentie voor gebonden veroudering wordt gebruikt, geeft deze waarde aan het aantal verlopen aanvragen toegestaan. Geaccepteerde bereik voor deze waarde is 1 – 2.147.483.647.
* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<resource-group-location>`De locatie van de Azure-resourcegroep waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database worden gemaakt. Het kan alleen kleine letters, cijfers, gebruiken de '-' bevatten, en moet tussen 3 en 50 tekens.

Voorbeeld: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Opmerkingen
* Het vorige voorbeeld maakt een databaseaccount met twee regio's. Het is ook mogelijk te maken van een databaseaccount met één regio (die is aangewezen als de regio schrijven en een failover-prioriteitswaarde van 0 hebben) of meer dan twee regio's. Zie voor meer informatie [meerdere landen/regio database accounts][scaling-globally].
* De locaties moet gebieden waarin Azure Cosmos DB in het algemeen beschikbaar is. De huidige lijst met regio's is opgegeven op de [Azure-gebieden pagina](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a>Een DocumentDB-databaseaccount bijwerken

Met deze opdracht kunt u de eigenschappen van uw Azure DB die Cosmos-database bijwerken. Dit omvat het beleid van de consistentie en de locaties waarvan de account van de database bestaat in.

> [!NOTE]
> Met deze opdracht kunt u toevoegen en verwijderen van de regio's, maar is niet toegestaan voor u failover prioriteiten wijzigen. Zie het wijzigen van failover prioriteiten [hieronder](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`De locatienaam van de regio van het schrijven van het account van de database. Deze locatie is vereist voor een failover-prioriteitswaarde van 0 hebben. Er moet exact één schrijven regio per databaseaccount.
* `<read-region-location>`De locatienaam van de gelezen regio van het account van de database. Deze locatie is vereist voor failover-prioriteit waarde groter dan 0. Er zijn meer dan één lezen regio's per database-account.
* `<default-consistency-level>`De consistentiecontrole standaardniveau van de Azure DB die Cosmos-account. Zie voor meer informatie [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md).
* `<ip-range-filter>`Hiermee geeft u het IP-adressen of IP-adresbereiken in CIDR-vorm moet worden opgenomen als de lijst met toegestane van client-IP-adressen voor een bepaalde database-account. IP-adressen of-adresbereiken moet door komma's gescheiden en mag geen spaties bevatten. Zie voor meer informatie [Azure Cosmos DB-Firewallondersteuning](firewall-support.md)
* `<max-interval>`Bij gebruik met consistentie voor gebonden veroudering is vertegenwoordigt deze waarde de tijd hoeveelheid veroudering (in seconden) toegestaan. Geaccepteerde bereik voor deze waarde is 1-100.
* `<max-staleness-prefix>`Met consistentie voor gebonden veroudering wordt gebruikt, geeft deze waarde aan het aantal verlopen aanvragen toegestaan. Geaccepteerde bereik voor deze waarde is 1 – 2.147.483.647.
* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<resource-group-location>`De locatie van de Azure-resourcegroep waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database moet worden bijgewerkt.

Voorbeeld: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a>Een DocumentDB-databaseaccount verwijderen

Met deze opdracht kunt u een bestaande account voor Azure DB die Cosmos-database verwijderen.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database moet worden verwijderd.

Voorbeeld:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a>Eigenschappen van een DocumentDB-databaseaccount opgehaald

Met deze opdracht kunt u de eigenschappen van een bestaande account voor Azure DB die Cosmos-database.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database.

Voorbeeld:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a>Labels van een Azure Cosmos DB databaseaccount bijwerken

Het volgende voorbeeld wordt beschreven hoe u [Azure resourcetags] [ azure-resource-tags] database voor uw Azure DB die Cosmos-account.

> [!NOTE]
> Met deze opdracht kan worden gecombineerd met de opdrachten maken of bijwerken door toe te voegen de `-Tags` markering op in de bijbehorende parameter.

Voorbeeld:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a>Lijst met sleutels

Wanneer u een Azure DB die Cosmos-account maakt, genereert de service twee master toegangstoetsen die kunnen worden gebruikt voor verificatie wanneer het account voor Azure Cosmos DB wordt geopend. Dankzij twee toegangssleutels, kunt u opnieuw genereren van de sleutels zonder onderbreking naar uw Azure DB die Cosmos-account Azure Cosmos DB. Alleen-lezen sleutels voor het verifiëren van alleen-lezen bewerkingen zijn ook beschikbaar. Er zijn twee lezen / schrijven-sleutels (primair en secundair) en twee sleutels voor alleen-lezen (primair en secundair).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database.

Voorbeeld:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>Lijst met verbindingsreeksen

MongoDB-accounts, worden de verbindingsreeks naar uw MongoDB-app verbinden met het account van de database opgehaald met de volgende opdracht.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database.

Voorbeeld:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a>Accountsleutel opnieuw genereren

Aan uw account voor Azure Cosmos DB regelmatig om te voorkomen dat uw verbindingen beter te beveiligen, moet u de toegangssleutels wijzigen. Twee toegangssleutels zijn zodat u verbindingen met de Azure DB die Cosmos-account met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereert onderhouden toegewezen.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database.
* `<key-kind>`Een van de vier typen sleutels: ["Primaire" | " Secundaire "|" PrimaryReadonly "|" SecondaryReadonly'] die u wilt genereren.

Voorbeeld:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a>Prioriteit van de Failover van een databaseaccount Azure Cosmos DB wijzigen

U kunt de prioriteit van de failover van de verschillende regio's die het account van de Azure DB die Cosmos-database bestaat in wijzigen voor meerdere landen/regio-database-accounts. Zie voor meer informatie over failover in uw Azure DB die Cosmos-databaseaccount [gegevens globaal met Azure Cosmos DB distribueren][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`De locatienaam van de regio van het schrijven van het account van de database. Deze locatie is vereist voor een failover-prioriteitswaarde van 0 hebben. Er moet exact één schrijven regio per databaseaccount.
* `<read-region-location>`De locatienaam van de gelezen regio van het account van de database. Deze locatie is vereist voor failover-prioriteit waarde groter dan 0. Er zijn meer dan één lezen regio's per database-account.
* `<resource-group-name>`De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe het account van de nieuwe Azure Cosmos DB database behoort.
* `<database-account-name>`De naam van de account van de Azure DB die Cosmos-database.

Voorbeeld:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Volgende stappen

* Als u wilt verbinding maken met .NET, Zie [Connect en query met .NET](create-documentdb-dotnet.md).
* Zie voor verbinding met behulp van .NET Core, [Connect en query met .NET Core](create-documentdb-dotnet-core.md).
* Zie voor verbinding met behulp van Node.js, [Connect en query met Node.js en een app MongoDB](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
