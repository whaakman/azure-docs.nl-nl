---
title: 'Quickstart: Een Azure Data Explorer-cluster en -database maken met CLI'
description: In deze quickstart leert u hoe u een Azure Data Explorer-cluster en -database maakt met Azure CLI
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: quickstart
ms.date: 2/4/2019
ms.openlocfilehash: 9e0ae547df34594674dc03702310a1537717a4ed
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55881113"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-using-cli"></a>Een Azure Data Explorer-cluster en -database maken met CLI

Deze quickstart toont u hoe u een Azure Data Explorer-cluster en -database maakt met Azure CLI.

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze quickstart versie 2.0.4 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om uw versie te controleren. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

## <a name="configure-the-cli-parameters"></a>De CLI-parameters configureren

De volgende stappen zijn niet vereist als u opdrachten in Cloud Shell uitvoert. Als u de CLI lokaal uitvoert, moet u de volgende stappen uitvoeren om u aan te melden bij Azure en uw huidige abonnement in te stellen:

1. Voer de volgende opdracht uit om u aan te melden bij Azure:

    ```azurecli-interactive
    az login
    ```

2. Het abonnement instellen waar u uw cluster wilt maken. Vervang `MyAzureSub` door de naam van het Azure-abonnement dat u wilt gebruiken:

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

2. Voer de volgende opdracht uit om te controleren of het cluster is gemaakt:

    ```azurecli-interactive
    az kusto cluster show --name azureclitest --resource-group testrg
    ```

Als het resultaat "provisioningState" met waarde "Geslaagd" bevat, is het maken van het cluster geslaagd.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>De database in het Azure Data Explorer-cluster maken

1. Maak uw database met behulp van de volgende opdracht:

    ```azurecli-interactive
    az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
    ```

   |**Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**|
   |---|---|---|
   | cluster-name | *azureclitest* | De naam van het cluster waar de database moet worden gemaakt.|
   | naam | *clidatabase* | De gewenste naam van uw database.|
   | resource-group | *testrg* | Naam van de resourcegroep waar het cluster wordt gemaakt. |
   | soft-delete-period | *3650:00:00:00* | Hoe lang gegevens moeten worden opgeslagen, zodat deze beschikbaar zijn om op te vragen. |
   | hot-cache-period | *3650:00:00:00* | Hoe lang gegevens moeten worden opgeslagen in de cache. |

2. Voer de volgende opdracht uit om de database te zien die u hebt gemaakt:

    ```azurecli-interactive
    az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
    ```

U hebt nu een cluster en een database.

## <a name="clean-up-resources"></a>Resources opschonen

* Als u onze andere snelstarts en zelfstudies wilt volgen, behoudt u de gemaakte resources.
* Als u resources wilt opschonen, moet u het cluster verwijderen. Wanneer u een cluster verwijdert, worden alle databases hierin ook verwijderd. Gebruik de onderstaande opdracht om uw cluster te verwijderen:

    ```azurecli-interactive
    az kusto cluster delete --name azureclitest --resource-group testrg
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstart: Gegevens opnemen met behulp van de Python-bibliotheek voor Azure Data Explorer](python-ingest-data.md)
