---
title: Maak een Azure Data Explorer-cluster en een database met behulp van CLI
description: In dit artikel wordt beschreven hoe u een cluster van Azure Data Explorer en de database met behulp van Azure CLI maken
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: howto
ms.date: 1/31/2019
ms.openlocfilehash: 8c035524adebcb131872c700280201aaac07c52b
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747947"
---
# <a name="create-an-azure-data-explorer-cluster-and-a-database-using-cli"></a>Maak een Azure Data Explorer-cluster en een database met behulp van CLI

In dit artikel wordt beschreven hoe u een cluster van Azure Data Explorer en de database met behulp van Azure CLI maken.

## <a name="whats-the-difference-between-the-management-plane-and-data-plane-apis"></a>Wat is het verschil tussen de beheerlaag en gegevenslaag API's

Er zijn twee verschillende API-bibliotheken, beheer en -gegevenslaag-API's.
De beheer-API's worden gebruikt voor het beheren van de resources, bijvoorbeeld een cluster maken, een database maken, verwijderen van een gegevensverbinding, het aantal exemplaren aantal enzovoort wijzigen. De gegevenslaag API's worden gebruikt om te communiceren met de gegevens, query's uitvoeren, opname van gegevens, enzovoort.

## <a name="configure-the-cli-parameters"></a>De CLI-parameters configureren

Meld u aan bij uw account

```Bash
az login
```

Het abonnement instellen waar u wilt aan het cluster moet worden gemaakt.

```Bash
az account set --subscription "your_subscription"
```

## <a name="create-the-azure-data-explorer-cluster"></a>De Azure Data Explorer-cluster maken

Maak uw cluster met behulp van de volgende opdracht uit.

```Bash
az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
```

Geef de volgende waarden

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | naam | *azureclitest* | De gewenste naam van uw cluster.|
    | sku | *D13_v2* | De SKU die wordt gebruikt voor uw cluster. |
    | resource-group | *testrg* | Naam van de resourcegroep waar het cluster moet worden gemaakt. |
    | | |

Als u wilt, moet u er meer optionele parameters die u, zoals de capaciteit van het cluster enzovoort gebruiken kunt zijn.

Als u wilt controleren of het cluster is gemaakt, kunt u uitvoeren

```Bash
az kusto cluster show --name azureclitest --resource-group testrg
```

Als het resultaat 'provisioningState"met"Geslaagd"waarde bevat, betekent dit dat het cluster is gemaakt.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>De database in de Azure Data Explorer-cluster maken

Maak uw database met behulp van de volgende opdracht uit.

```Bash
az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
```

Geef de volgende waarden

    **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**
    |---|---|---|
    | naam van cluster | *azureclitest* | De naam van het cluster waar het moet worden gemaakt.|
    | naam | *clidatabase* | De gewenste naam van uw database.|
    | resource-group | *testrg* | Naam van de resourcegroep waar het cluster moet worden gemaakt. |
    | soft-delete-period | *3650:00:00:00* | Hoeveelheid tijd die gegevens moeten worden opgeslagen, zodat deze beschikbaar is om op te vragen. |
    | hot-cache-period | *3650:00:00:00* | Hoeveelheid tijd die gegevens moeten worden opgeslagen in de cache. |
    | | |

U kunt zien dat de database die u hebt gemaakt door te voeren

```Bash
az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
```

Dat is alles u hebt nu een cluster en een database.

## <a name="clean-up-resources"></a>Resources opschonen

Als u onze andere snelstarts en zelfstudies wilt volgen, behoudt u de gemaakte resources.

Wanneer u een cluster verwijdert, worden alle databases in deze ook verwijderd. Gebruik de onderstaande opdracht om uw cluster te verwijderen.

```Bash
az kusto cluster delete --name azureclitest --resource-group testrg
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstart: Opnemen van gegevens van Event Hub in Azure Data Explorer](ingest-data-event-hub.md)
