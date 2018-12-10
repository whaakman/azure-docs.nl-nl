---
title: Maken en beheren van Azure Cosmos DB-resources met behulp van PowerShell
description: Gebruik Azure Powershell beheren uw Azure Cosmos DB-accounts.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 6dcb5e445c9c294b81de0363f5462b8d21621616
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135285"
---
# <a name="create-and-manage-azure-cosmos-db-resources-by-using-powershell"></a>Maken en beheren van Azure Cosmos DB-resources met behulp van PowerShell

De volgende handleiding wordt beschreven opdrachten voor het automatiseren van beheer van uw Azure Cosmos DB-database-accounts met behulp van Azure Powershell. Dit omvat ook opdrachten voor het beheren van accountsleutels en failover-prioriteiten in [databaseaccounts voor meerdere regio's][distribute-data-globally]. Bijwerken van uw databaseaccount, kunt u consistentie beleid wijzigen en regio's toevoegen/verwijderen. Voor het beheer van meerdere platforms van uw Azure Cosmos DB-account, kunt u een [Azure CLI](cli-samples.md), wordt de [Resource Provider REST API][rp-rest-api], of de [Azure-portal ](create-sql-api-dotnet.md#create-account).

## <a name="getting-started"></a>Aan de slag

Volg de instructies in [hoe u Azure PowerShell installeren en configureren] [ powershell-install-configure] te installeren en meld u aan bij uw Azure Resource Manager-serviceaccount in Powershell.

### <a name="notes"></a>Opmerkingen

* Als u wilt de volgende opdrachten uitvoeren zonder bevestiging van de gebruiker, toevoegen de `-Force` vlag aan de opdracht.
* De volgende opdrachten worden synchroon.

## <a id="create-documentdb-account-powershell"></a> Een Azure Cosmos DB-Account maken

Met deze opdracht kunt u een Azure Cosmos DB-databaseaccount maken. Uw nieuwe databaseaccount configureren als een van beide één regio of [meerdere regio's] [ distribute-data-globally] met een bepaalde [consistentie beleid](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` De locatienaam van de schrijfregio van account van de database. Deze locatie is vereist om de prioriteit van een failover van 0. Er moet exact één schrijfregio per databaseaccount.
* `<read-region-location>` De locatienaam van de leesregio van account van de database. Deze locatie is vereist om de prioriteit van een failover van groter dan 0. Er zijn meer dan één leesregio's per databaseaccount.
* `<ip-range-filter>` Hiermee geeft u de set met IP-adressen of IP-adresbereiken in CIDR-notatie om op te nemen als toegestane lijst van client-IP-adressen voor een bepaald databaseaccount. IP-adressen /-bereiken moet door komma's gescheiden en mag geen spaties bevatten. Zie voor meer informatie, [Firewallondersteuning van Azure Cosmos DB](firewall-support.md)
* `<default-consistency-level>` Het standaardconsistentieniveau van het Azure Cosmos DB-account. Zie voor meer informatie, [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md).
* `<max-interval>` Gebruikt in combinatie met consistentie voor gebonden veroudering, vertegenwoordigt deze waarde de tijd hoeveelheid veroudering (in seconden) toegestaan. Buiten het toegestane bereik voor deze waarde is 1-100.
* `<max-staleness-prefix>` Gebruikt in combinatie met consistentie voor gebonden veroudering, staat deze waarde voor het aantal verlopen aanvragen toegestaan. Buiten het toegestane bereik voor deze waarde is 1: 2.147.483.647.
* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<resource-group-location>` De locatie van de Azure-resourcegroep waaraan de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van het account van de Azure Cosmos DB-database moet worden gemaakt. Kan alleen kleine letters, cijfers, gebruiken de '-' teken en moet tussen 3 en 50 tekens.

Voorbeeld: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Opmerkingen
* Het vorige voorbeeld maakt een databaseaccount met twee regio's. Het is ook mogelijk te maken van een databaseaccount met één regio (die is aangewezen als de schrijfregio en een failover-prioriteit-waarde van 0) of meer dan twee regio's. Zie voor meer informatie, [databaseaccounts voor meerdere regio's][distribute-data-globally].
* De locaties moet regio's waar Azure Cosmos DB algemeen beschikbaar is. De huidige lijst met regio's wordt geleverd op de [Azure-regio's pagina](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a> Bijwerken van een Azure Cosmos DB-databaseaccount

Met deze opdracht kunt u de eigenschappen van uw Azure Cosmos DB-database bijwerken. Dit omvat het beleid consistentie en de locaties die de databaseaccount bestaat al in.

> [!NOTE]
> Met deze opdracht kunt u toevoegen en verwijderen van regio's, maar staat niet toe dat u failover-prioriteiten wijzigen. Zie voor het wijzigen van failover-prioriteiten [hieronder](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` De locatienaam van de schrijfregio van account van de database. Deze locatie is vereist om de prioriteit van een failover van 0. Er moet exact één schrijfregio per databaseaccount.
* `<read-region-location>` De locatienaam van de leesregio van account van de database. Deze locatie is vereist om de prioriteit van een failover van groter dan 0. Er zijn meer dan één leesregio's per databaseaccount.
* `<default-consistency-level>` Het standaardconsistentieniveau van het Azure Cosmos DB-account. Zie voor meer informatie, [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md).
* `<ip-range-filter>` Hiermee geeft u de set met IP-adressen of IP-adresbereiken in CIDR-notatie om op te nemen als toegestane lijst van client-IP-adressen voor een bepaald databaseaccount. IP-adressen /-bereiken moet door komma's gescheiden en mag geen spaties bevatten. Zie voor meer informatie, [Firewallondersteuning van Azure Cosmos DB](firewall-support.md)
* `<max-interval>` Gebruikt in combinatie met consistentie voor gebonden veroudering, vertegenwoordigt deze waarde de tijd hoeveelheid veroudering (in seconden) toegestaan. Buiten het toegestane bereik voor deze waarde is 1-100.
* `<max-staleness-prefix>` Gebruikt in combinatie met consistentie voor gebonden veroudering, staat deze waarde voor het aantal verlopen aanvragen toegestaan. Buiten het toegestane bereik voor deze waarde is 1: 2.147.483.647.
* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<resource-group-location>` De locatie van de Azure-resourcegroep waaraan de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van het account van de Azure Cosmos DB-database moet worden bijgewerkt.

Voorbeeld: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a> Verwijderen van een Azure Cosmos DB-databaseaccount

Met deze opdracht kunt u een bestaande Azure Cosmos DB-databaseaccount verwijderen.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van het account van de Azure Cosmos DB-database moet worden verwijderd.

Voorbeeld:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Hiermee worden eigenschappen van een Azure Cosmos DB-databaseaccount

Met deze opdracht kunt u de eigenschappen van een bestaande Azure Cosmos DB-databaseaccount ophalen.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van de account van de Azure Cosmos DB-database.

Voorbeeld:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a> Bijwerken van Tags van een Azure Cosmos DB-databaseaccount

Het volgende voorbeeld wordt beschreven hoe u om in te stellen [Azure resourcetags] [ azure-resource-tags] database voor uw Azure Cosmos DB-account.

> [!NOTE]
> Met deze opdracht kan worden gecombineerd met de opdrachten maken of bijwerken door toe te voegen de `-Tags` markering op in de bijbehorende parameter.

Voorbeeld:

    $tags = @{"dept" = "Finance"; environment = "Production"}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDB/databaseAccounts"  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Een lijst met Accountsleutels

Wanneer u een Azure Cosmos DB-account maakt, genereert de service twee master toegangssleutels die kunnen worden gebruikt voor verificatie wanneer het Azure Cosmos DB-account wordt geopend. Door twee toegangssleutels, kunt Azure Cosmos DB u de sleutels zonder onderbreking naar uw Azure Cosmos DB-account opnieuw genereren. Alleen-lezensleutels voor het verifiëren van alleen-lezen bewerkingen zijn ook beschikbaar. Er zijn twee sleutels voor lezen / schrijven (primaire en secundaire) en twee sleutels voor alleen-lezen (primaire en secundaire).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van de account van de Azure Cosmos DB-database.

Voorbeeld:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a> Lijst met verbindingsreeksen

Voor MongoDB-accounts, kan de verbindingsreeks voor uw MongoDB-app verbinden met account van de database worden opgehaald met de volgende opdracht.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van de account van de Azure Cosmos DB-database.

Voorbeeld:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Accountsleutel opnieuw genereren

Naar uw Azure Cosmos DB-account periodiek om te voorkomen dat uw verbindingen beter te beveiligen, moet u de toegangssleutels wijzigen. Twee toegangssleutels zijn zodat u verbindingen met de Azure Cosmos DB-account met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereren onderhouden toegewezen.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van de account van de Azure Cosmos DB-database.
* `<key-kind>` Een van de vier typen sleutels: ['Primaire' | ' Secundaire "|" PrimaryReadonly "|" SecondaryReadonly"] die u wilt genereren.

Voorbeeld:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a> Failover-prioriteit van een Azure Cosmos DB-databaseaccount wijzigen

Voor databaseaccounts voor meerdere regio's, kunt u de failover-prioriteit van de verschillende regio's waarop de Azure Cosmos DB-databaseaccount bestaat in. Zie voor meer informatie over failover in uw Azure Cosmos DB-databaseaccount [gegevens globaal distribueren met Azure Cosmos DB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>` De locatienaam van de schrijfregio van account van de database. Deze locatie is vereist om de prioriteit van een failover van 0. Er moet exact één schrijfregio per databaseaccount.
* `<read-region-location>` De locatienaam van de leesregio van account van de database. Deze locatie is vereist om de prioriteit van een failover van groter dan 0. Er zijn meer dan één leesregio's per databaseaccount.
* `<resource-group-name>` De naam van de [Azure-resourcegroep] [ azure-resource-groups] waartoe de nieuwe Azure Cosmos DB-databaseaccount behoort.
* `<database-account-name>` De naam van de account van de Azure Cosmos DB-database.

Voorbeeld:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Volgende stappen

* Als u wilt verbinding maken met behulp van .NET, Zie [verbinding maken en query's uitvoeren met .NET](create-sql-api-dotnet.md).
* Als u wilt verbinding maken met behulp van Node.js, Zie [verbinding maken en query's uitvoeren met Node.js en een MongoDB-app](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
