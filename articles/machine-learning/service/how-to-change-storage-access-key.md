---
title: Toegangssleutels voor opslag account wijzigen
titleSuffix: Azure Machine Learning service
description: Informatie over het wijzigen van de toegangssleutels voor het Azure Storage-account voor uw werkruimte. Azure Machine Learning-service maakt gebruik van een Azure Storage-account voor het opslaan van gegevens en -modellen. Wanneer u de toegangssleutel voor het opslagaccount opnieuw genereert, moet u de Azure Machine Learning-service voor het gebruik van de nieuwe sleutels bijwerken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 05/17/2019
ms.openlocfilehash: 11abbb0494a9771676dcf2436b5e808346c16368
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078558"
---
# <a name="regenerate-storage-account-access-keys"></a>Storage-account toegang tot sleutels opnieuw genereren

Informatie over het wijzigen van de toegangssleutels voor Azure Storage-accounts die worden gebruikt door de service Azure Machine Learning. Azure Machine Learning kunt storage-accounts gebruiken voor het opslaan van gegevens of getrainde modellen.

Uit veiligheidsoverwegingen moet u mogelijk de toegangssleutel voor een Azure Storage-account wijzigen. Wanneer u de toegangssleutel opnieuw genereert, moet Azure Machine Learning voor het gebruik van de nieuwe sleutel worden bijgewerkt. Azure Machine Learning kan het storage-account gebruiken voor beide model-opslag en als een gegevensarchief.

## <a name="prerequisites"></a>Vereisten

* Een werkruimte van Azure Machine Learning-service. Zie voor meer informatie de [maken van een werkruimte](setup-create-workspace.md) artikel.

* De [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* De [Azure Machine Learning CLI-extensie](reference-azure-machine-learning-cli.md).

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Wat moet worden bijgewerkt

Storage-accounts kunnen worden gebruikt door de werkruimte van de Azure Machine Learning-service (voor het opslaan van Logboeken, modellen, momentopnamen, enzovoort) en een gegevensarchief. Het proces voor het bijwerken van de werkruimte is slechts één Azure CLI-opdracht, en kan worden uitgevoerd na het bijwerken van de opslagsleutel. Het proces voor het bijwerken van de gegevensopslag is meer betrokken, en vereist detecteren welke gegevensopslag gebruikt momenteel de storage-account en vervolgens opnieuw registreren.

> [!IMPORTANT]
> De werkruimte met behulp van de Azure CLI en de gegevensopslag met behulp van Python, op hetzelfde moment bijwerken. Bijwerken van slechts één of de andere is niet voldoende, en kan leiden tot fouten totdat beide worden bijgewerkt.

Voor het detecteren van de storage-accounts die worden gebruikt door uw gegevensopslag, gebruik de volgende code:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " + default_ds.account_name + ", container name: " + ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob" or ds.datastore_type == "AzureFile":
        print("datastore name: " + name + ", storage account name: " + ds.account_name + ", container name: " + ds.container_name)
```

Deze code zoekt naar alle geregistreerde gegevensopslag die gebruikmaken van Azure Storage en bevat de volgende informatie:

* Naam van de gegevensopslag: De naam van het gegevensarchief dat de storage-account is geregistreerd.
* Naam van opslagaccount: De naam van de Azure Storage-account.
* Container: De container in het opslagaccount dat wordt gebruikt door deze registratie.

Als er een vermelding bestaat voor het opslagaccount dat u van plan bent opnieuw genereren van toegangssleutels voor, sla de gegevensopslag naam, de naam van het opslagaccount en de containernaam.

## <a name="update-the-access-key"></a>De toegangssleutel bijwerken

Voor het bijwerken van Azure Machine Learning-service voor het gebruik van de nieuwe sleutel, gebruikt u de volgende stappen uit:

> [!IMPORTANT]
> Voer alle stappen uit, zowel de werkruimte met behulp van de CLI en gegevensopslag met behulp van Python wordt bijgewerkt. Bijwerken van slechts één van beide kan fouten veroorzaken totdat beide worden bijgewerkt.

1. De sleutel opnieuw genereren. Zie voor informatie over een toegangssleutel opnieuw genereren, de [een storage-account beheren](/azure/storage/common/storage-account-manage.md#access-keys) artikel. Sla de nieuwe sleutel.

1. Voor het bijwerken van de werkruimte voor het gebruik van de nieuwe sleutel, gebruikt u de volgende stappen uit:

    1. Aanmelden bij de Azure-abonnement met uw werkruimte met behulp van de volgende Azure CLI-opdracht:

        ```azurecli-interactive
        az login
        ```

    1. Gebruik de volgende opdracht voor het installeren van de Azure Machine Learning-extensie:

        ```azurecli-interactive
        az extension add -n azure-cli-ml 
        ```

    1. Gebruik de volgende opdracht voor het bijwerken van de werkruimte voor het gebruik van de nieuwe sleutel. Vervang `myworkspace` door de naam van de Azure Machine Learning-werkruimte en vervang `myresourcegroup` met de naam van de Azure-resourcegroep met de werkruimte.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        Met deze opdracht worden automatisch gesynchroniseerd met de nieuwe sleutels voor het Azure storage-account voor de werkruimte.

1. Gebruik voor het opnieuw registreren elke gegevensopslag die gebruikmaken van het opslagaccount dat de waarden uit de [wat moet worden bijgewerkt](#whattoupdate) sectie en de sleutel uit stap 1 door de volgende code:

    ```python
    ds = Datastore.register_azure_blob_container(workspace=ws, 
                                              datastore_name='your datastore name', 
                                              container_name='your container name',
                                              account_name='your storage account name', 
                                              account_key='new storage account key',
                                              overwrite=True)
    ```

    Omdat `overwrite=True` is opgegeven, met deze code overschrijft de bestaande registratie en updates voor het gebruik van de nieuwe sleutel.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het registreren van gegevensopslag, de [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) klasse verwijzing.
