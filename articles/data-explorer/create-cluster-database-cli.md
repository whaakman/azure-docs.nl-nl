---
title: 'Quickstart: Een cluster van Azure Data Explorer en -database maken met behulp van Azure CLI'
description: Ontdek hoe u een Azure Data Explorer-cluster en -database kunt maken met de Azure CLI
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 3/25/2019
ms.openlocfilehash: 852eb32611f526c6fae4898d2bc85cac0859498f
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758428"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-azure-cli"></a>Een cluster van Azure Data Explorer en -database maken met behulp van Azure CLI

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>

Azure Data Explorer is een snelle, volledig beheerde service voor gegevensanalyses waarmee grote hoeveelheden gegevens van toepassingen, websites, IoT-apparaten en dergelijke in real-time kunnen worden geanalyseerd. Als u Azure Data Explorer wilt gebruiken, maakt u eerst een cluster. Daarna maakt u een of meer databases in het cluster. De volgende stap is het opnemen (laden) van gegevens in een database, zodat u er query's op kunt uitvoeren. In deze snelstartgids maakt u een cluster en een database met behulp van Azure CLI.

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de Azure CLI lokaal te installeren en te gebruiken, moet u voor deze quickstart versie 2.0.4 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om uw versie te controleren. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-the-cli-parameters"></a>De CLI-parameters configureren

De volgende stappen zijn niet vereist als u opdrachten in Azure Cloud Shell uitvoert. Als u de CLI lokaal uitvoert, volgt u deze stappen om u aan te melden bij Azure en uw huidige abonnement in te stellen:

1. Voer de volgende opdracht uit om u aan te melden bij Azure:

    ```azurecli-interactive
    az login
    ```

1. Stel het abonnement in waarin u uw cluster wilt maken. Vervang `MyAzureSub` door de naam van het Azure-abonnement dat u wilt gebruiken:

    ```azurecli-interactive
    az account set --subscription MyAzureSub
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Het Azure Data Explorer-cluster maken

1. Maak uw cluster met behulp van de volgende opdracht:

    ```azurecli-interactive
    az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
    ```

   |**Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**|
   |---|---|---|
   | naam | *azureclitest* | De gewenste naam van uw cluster.|
   | sku | *D13_v2* | De SKU die wordt gebruikt voor uw cluster. |
   | resource-group | *testrg* | Naam van de resourcegroep waar het cluster wordt gemaakt. |

    Er zijn aanvullende optionele parameters die u kunt gebruiken, zoals de capaciteit van het cluster.

1. Voer de volgende opdracht uit om te controleren of het cluster is gemaakt:

    ```azurecli-interactive
    az kusto cluster show --name azureclitest --resource-group testrg
    ```

Als het resultaat `provisioningState` met waarde `Succeeded` bevat, is het maken van het cluster geslaagd.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>De database in het Azure Data Explorer-cluster maken

1. Maak uw database met behulp van de volgende opdracht:

    ```azurecli-interactive
    az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
    ```

   |**Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**|
   |---|---|---|
   | cluster-name | *azureclitest* | De naam van het cluster waar de database wordt gemaakt.|
   | naam | *clidatabase* | De naam van uw database.|
   | resource-group | *testrg* | Naam van de resourcegroep waar het cluster wordt gemaakt. |
   | soft-delete-period | *3650:00:00:00* | Hoe lang gegevens worden opgeslagen om query's erop te kunnen uitvoeren. |
   | hot-cache-period | *3650:00:00:00* | Hoe lang gegevens worden opgeslagen in de cache. |

1. Voer de volgende opdracht uit om de database te bekijken die u hebt gemaakt:

    ```azurecli-interactive
    az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
    ```

U hebt nu een cluster en een database.

## <a name="clean-up-resources"></a>Resources opschonen

* Als u onze andere snelstarts en zelfstudies wilt volgen, behoudt u de gemaakte resources.
* Als u resources wilt opschonen, moet u het cluster verwijderen. Wanneer u een cluster verwijdert, worden alle databases hierin ook verwijderd. Gebruik de volgende opdracht om uw cluster te verwijderen:

    ```azurecli-interactive
    az kusto cluster delete --name azureclitest --resource-group testrg
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstart: Gegevens opnemen met behulp van de Python-bibliotheek voor Azure Data Explorer](python-ingest-data.md)
