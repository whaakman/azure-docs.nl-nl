---
title: Maken en beheren van Azure Cosmos DB met behulp van PowerShell
description: Gebruik Azure Powershell beheren van uw Azure Cosmos DB-accounts, databases, containers en doorvoer.
author: markjbrown
ms.service: cosmos-db
ms.topic: samples
ms.date: 05/23/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: f720b678f2c7a6e564ef3e8fa9ae071b004ed1a6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66243394"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>Beheer van Azure Cosmos DB SQL API-resources met behulp van PowerShell

De volgende handleiding wordt beschreven hoe u PowerShell gebruiken voor het script en automatiseren van beheer van Azure Cosmos DB, met inbegrip van het account, database, -container en doorvoer. Beheer van Azure Cosmos DB is niet met Azure Cosmos DB-specifieke cmdlets, maar met de resourceprovider rechtstreeks via de cmdlet AzResource. Alle eigenschappen die kunnen worden beheerd met behulp van PowerShell voor de resourceprovider van Azure Cosmos DB Zie [providerschema voor Azure Cosmos DB-resource](/azure/templates/microsoft.documentdb/allversions)

Voor het beheer van meerdere platformen van Azure Cosmos DB, kunt u [Azure CLI](manage-with-cli.md), wordt de [REST-API][rp-rest-api], of de [Azure-portal](create-sql-api-dotnet.md#create-account).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Aan de slag

Volg de instructies in [hoe u Azure PowerShell installeren en configureren] [ powershell-install-configure] te installeren en meld u aan bij uw Azure-account in Powershell.

* Als u wilt de volgende opdrachten uitvoeren zonder bevestiging van de gebruiker, toevoegen de `-Force` vlag aan de opdracht.
* De volgende opdrachten worden synchroon.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos-Accounts

De volgende secties laten zien hoe u voor het beheren van de Azure Cosmos-account, met inbegrip van:

* [Een Azure Cosmos-account maken](#create-account)
* [Een Azure Cosmos-account bijwerken](#update-account)
* [Maak een Azure Cosmos-account](#get-account)
* [Een Azure Cosmos-account verwijderen](#delete-account)
* [Bijwerken van tags voor een Azure Cosmos-account](#update-tags)
* [Een lijst met sleutels voor een Azure Cosmos-account](#list-keys)
* [Sleutels opnieuw genereren voor een Azure Cosmos-account](#regenerate-keys)
* [Lijst met verbindingsreeksen voor een Azure Cosmos-account](#list-connection-strings)
* [Failover-prioriteit voor een Azure Cosmos-account wijzigen](#modify-failover-priority)

### <a id="create-account"></a> Een Azure Cosmos-account maken

Met deze opdracht kunt u een Azure Cosmos DB-databaseaccount maken. Uw nieuwe databaseaccount configureren als een van beide één regio of [meerdere regio's] [ distribute-data-globally] met een bepaalde [consistentie beleid](consistency-levels.md).

```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

* `$accountName` De naam voor de Azure Cosmos-account. Moet een kleine letter, alfanumerieke accepteert en de '-' teken, en tussen de 3 en 50 tekens.
* `$location` De locatie voor de Azure Cosmos-account.
* `$locations` De replica-regio's voor het account van de database. Er moet een regio voor schrijfbewerkingen per databaseaccount met de prioriteit van een failover van 0.
* `$consistencyPolicy` Het standaardconsistentieniveau van het Azure Cosmos-account. Zie voor meer informatie, [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md).
* `$CosmosDBProperties` De eigenschapswaarden doorgegeven aan de Cosmos DB Azure Resource Manager-Provider voor het inrichten van het account.

Azure Cosmos accounts kunnen worden geconfigureerd met IP-Firewall, evenals Virtual Network service-eindpunten. Zie voor meer informatie over het configureren van de IP-Firewall voor Azure Cosmos DB [IP-Firewall configureren](how-to-configure-firewall.md).  Zie voor meer informatie over het inschakelen van service-eindpunten voor Azure Cosmos DB [toegang van virtuele netwerken configureren](how-to-configure-vnet-service-endpoint.md) .

### <a id="get-account"></a> Hiermee worden de eigenschappen van een Azure Cosmos-account

Met deze opdracht kunt u Hiermee worden de eigenschappen van een bestaand Azure-Cosmos-account.

```azurepowershell-interactive
# Get the properties of an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName | Select-Object Properties
```

### <a id="update-account"></a> Een Azure Cosmos-account bijwerken

Met deze opdracht kunt u de eigenschappen van uw Azure Cosmos DB-database bijwerken. Eigenschappen die kunnen worden bijgewerkt, zijn onder andere:

* Toevoegen of verwijderen van regio 's
* Standaardbeleid voor consistentie wijzigen
* Failover-beleid wijzigen
* IP-adresfilter van bereik wijzigen
* Configuraties van het Virtueelnetwerk wijzigen
* Meerdere masters inschakelen

> [!NOTE]
> Met deze opdracht kunt u toevoegen en verwijderen van regio's, maar staat niet toe dat u failover-prioriteiten wijzigen. Zie voor het wijzigen van failover-prioriteiten [failover-prioriteit voor een Azure Cosmos-account wijzigen](#modify-failover-priority).

```azurepowershell-interactive
# Update an Azure Cosmos Account and set Consistency level to Session

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$account.Properties.consistencyPolicy = $consistencyPolicy
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="delete-account"></a> Een Azure Cosmos-account verwijderen

Met deze opdracht kunt u een bestaand Azure-Cosmos-account verwijderen.

```azurepowershell-interactive
# Delete an Azure Cosmos Account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName
```

### <a id="update-tags"></a> Bijwerken van Tags van een Azure Cosmos-account

Het volgende voorbeeld wordt beschreven hoe u om in te stellen [Azure resourcetags] [ azure-resource-tags] voor een Azure Cosmos-account.

> [!NOTE]
> Met deze opdracht kan worden gecombineerd met de opdrachten maken of bijwerken door toe te voegen de `-Tags` markering op in de bijbehorende parameter.

```azurepowershell-interactive
# Update tags for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$tags = @{
    "dept" = "Finance";
    "environment" = "Production"
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tags $tags
```

### <a id="list-keys"></a> Een lijst met Accountsleutels

Wanneer u een Azure Cosmos DB-account maakt, genereert de service twee master toegangssleutels die kunnen worden gebruikt voor verificatie wanneer het Azure Cosmos DB-account wordt geopend. Door twee toegangssleutels, kunt Azure Cosmos DB u de sleutels zonder onderbreking naar uw Azure Cosmos DB-account opnieuw genereren. Alleen-lezensleutels voor het verifiëren van alleen-lezen bewerkingen zijn ook beschikbaar. Er zijn twee sleutels voor lezen / schrijven (primaire en secundaire) en twee sleutels voor alleen-lezen (primaire en secundaire).

```azurepowershell-interactive
# List keys for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="list-connection-strings"></a> Lijst met verbindingsreeksen

Voor MongoDB-accounts, kan de verbindingsreeks voor uw MongoDB-app verbinden met account van de database worden opgehaald met de volgende opdracht.

```azurepowershell-interactive
# List connection strings for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="regenerate-keys"></a> Accountsleutels opnieuw genereren

Toegangssleutels aan een Azure Cosmos-account moeten periodiek opnieuw gegenereerd om ervoor te zorgen dat verbindingen beter te beveiligen. Een primaire en secundaire sleutels zijn toegewezen aan het account. Hiermee kunnen clients om toegang te behouden terwijl de andere is opnieuw gegenereerd. Er zijn vier soorten sleutels voor een Azure Cosmos-account (primair, secundair, PrimaryReadonly en SecondaryReadonly)

```azurepowershell-interactive
# Regenerate the primary key for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keyKind = @{ "keyKind"="Primary" }

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Select-Object $keys
```

### <a id="modify-failover-priority"></a> Failover-prioriteit wijzigen

Voor databaseaccounts voor meerdere regio's, kunt u de prioriteit van de failover van de verschillende regio's die het account van de Azure Cosmos DB-database deel uitmaakt. Zie voor meer informatie over failover in uw Azure Cosmos DB-databaseaccount [gegevens globaal distribueren met Azure Cosmos DB][distribute-data-globally].

Voor het volgende voorbeeld wordt ervan uitgegaan dat het account heeft een huidige failover-prioriteit van westus = 0 en eastus = 1. In het voorbeeld hieronder wordt de regio's te spiegelen.

> [!CAUTION]
> Met deze bewerking wordt een handmatige failover voor de nieuwe regio met een failoverPriority gelijk is aan nul voor uw account geactiveerd.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverPolicies = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a name="azure-cosmos-database"></a>Azure Cosmos-Database

De volgende secties laten zien hoe u voor het beheren van de Azure Cosmos-database, met inbegrip van:

* [Een Azure Cosmos-database maken](#create-db)
* [Een Azure Cosmos-database maken met gedeelde doorvoer](#create-db-ru)
* [Lijst van alle Azure-Cosmos-databases in een account](#get-all-db)
* [Ophalen van een individuele Azure-Cosmos-database](#get-db)
* [Een Azure Cosmos-database verwijderen](#delete-db)

### <a id="create-db"></a>Een Azure Cosmos-database maken

```azurepowershell-interactive
# Create an Azure Cosmos database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{"id"=$databaseName}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="create-db-ru"></a>Een Azure Cosmos-database maken met gedeelde doorvoer

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database2"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="get-all-db"></a>Alle Azure-Cosmos-databases in een account ophalen

```azurepowershell-interactive
# Get all databases in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceName = $accountName + "/sql/"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName  | Select-Object Properties
```

### <a id="get-db"></a>Ophalen van een individuele Azure-Cosmos-database

```azurepowershell-interactive
# Get a single database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-db"></a>Een Azure Cosmos-database verwijderen

```azurepowershell-interactive
# Delete a database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName
Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="azure-cosmos-container"></a>Azure Cosmos-Container

De volgende secties laten zien hoe u voor het beheren van de Azure Cosmos-container, met inbegrip van:

* [Een Azure Cosmos-container maken](#create-container)
* [Een Azure Cosmos-container maken met gedeelde doorvoer](#create-container-ru)
* [Een Azure Cosmos-container maken met aangepast indexeringsbeleid](#create-container-custom-index)
* [Een Azure Cosmos-container maken met de indexering uitgeschakeld](#create-container-no-index)
* [Een Azure Cosmos-container maken met een unieke sleutel en TTL](#create-container-unique-key-ttl)
* [Een Azure Cosmos-container maken met conflictoplossing](#create-container-lww)
* [Lijst van alle Azure-Cosmos-containers in een database](#list-all-container)
* [Een enkele Azure-Cosmos-container in een database ophalen](#get-container)
* [Een Azure Cosmos-container verwijderen](#delete-container)

### <a id="create-container"></a>Een Azure Cosmos-container maken

```azurepowershell-interactive
# Create an Azure Cosmos container with default indexes and throughput at 400 RU 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }
    }; 
    "options"=@{ "Throughput"="400" }
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-ru"></a>Een Azure Cosmos-container maken met gedeelde doorvoer

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container2"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }
    }; 
    "options"=@{}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties 
```

### <a id="create-container-custom-index"></a>Een Azure Cosmos-container maken met aangepaste index beleid

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container3"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent"; 
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @(@{
                        "kind"="Range";
                        "dataType"="number";
                        "precision"=-1
                    },
                    @{
                        "kind"="Range";
                        "dataType"="string";
                        "precision"=-1
                    }
                )
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-no-index"></a>Een Azure Cosmos-container maken met de indexering uitgeschakeld

```azurepowershell-interactive
# Create an Azure Cosmos container with no indexing 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container4"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="none"
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-unique-key-ttl"></a>Een Azure Cosmos-container maken met unieke sleutels, beleid en TTL

```azurepowershell-interactive
# Create a container with a unique key policy and TTL
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent"; 
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @(@{
                        "kind"="Range";
                        "dataType"="number";
                        "precision"=-1
                    },
                    @{
                        "kind"="Range";
                        "dataType"="string";
                        "precision"=-1
                    }
                )
            });
            "excludedPaths"= @()
        };
        "uniqueKeyPolicy"= @{
            "uniqueKeys"= @(@{
                "paths"= @(
                    "/myUniqueKey1";
                    "/myUniqueKey2"
                )
            })
        };
        "defaultTtl"= 100;
    }; 
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-lww"></a>Een Azure Cosmos-container maken met conflictoplossing

Als u een conflict resolutie beleid voor het gebruik van een opgeslagen procedure, stelt u `"mode"="custom"` en het pad van het probleem zou moeten instellen als de naam van de opgeslagen procedure `"conflictResolutionPath"="myResolverStoredProcedure"`. Instellen voor alle conflicten schrijven naar de ConflictsFeed en afzonderlijk te verwerken, `"mode"="custom"` en `"conflictResolutionPath"=""`

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container6"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "conflictResolutionPolicy"=@{
            "mode"="lastWriterWins"; 
            "conflictResolutionPath"="/myResolutionPath"
        }
    }; 
    "options"=@{ "Throughput"="400" }
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="list-all-container"></a>Lijst van alle Azure-Cosmos-containers in een database

```azurepowershell-interactive
# List all Azure Cosmos containers in a database 
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="get-container"></a>Een enkele Azure-Cosmos-container in een database ophalen

```azurepowershell-interactive
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-container"></a>Een Azure Cosmos-container verwijderen

```azurepowershell-interactive
# Delete an Azure Cosmos container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="next-steps"></a>Volgende stappen

* [Alle voorbeelden van PowerShell](powershell-samples.md)
* [Azure Cosmos-account beheren](how-to-manage-database-account.md)
* [Een Azure Cosmos-container maken](how-to-create-container.md)
* [Configureren van de time-to-live in Azure Cosmos DB](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/