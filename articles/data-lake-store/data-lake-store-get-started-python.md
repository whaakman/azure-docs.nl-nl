---
title: 'Python: Accountbeheerbewerkingen in Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Leer hoe u Python SDK gebruikt om te werken met Azure Data Lake Storage Gen1 accountbeheerbewerkingen.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 0cc5b101e1afb6ea648963188887cf43b65a5afa
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124946"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-python"></a>Accountbeheerbewerkingen in Azure Data Lake Storage Gen1 met behulp van Python
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Informatie over het gebruik van de Python-SDK voor Azure Data Lake Storage Gen1 voor het uitvoeren van bewerkingen zoals het maken een Gen1 van Data Lake Storage-account, lijst met de Gen1 met Data Lake Storage-accounts, enzovoort. Zie voor instructies over het uitvoeren van bestandssysteembewerkingen in Data Lake Storage Gen1 met behulp van Python [bestandssysteembewerkingen in Data Lake Storage Gen1 met behulp van Python](data-lake-store-data-operations-python.md).

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een Azure-resourcegroep**. Zie [Een Azure-resourcegroep maken](../azure-resource-manager/resource-group-portal.md) voor instructies.

## <a name="install-the-modules"></a>De modules installeren

Werken met Data Lake Storage Gen1 met behulp van Python, u moet drie modules installeren.

* Module `azure-mgmt-resource`, die Azure-modules bevat voor Active Directory enzovoort.
* De `azure-mgmt-datalake-store` module, deze de accountbeheerbewerkingen voor Azure Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module [Azure Data Lake-opslagbeheer Gen1 Moduleverwijzing](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* De `azure-datalake-store` module, deze de bestandssysteembewerkingen voor Azure Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module [azure-datalake-store-bestandssysteemmodule Moduleverwijzing](http://azure-datalake-store.readthedocs.io/en/latest/).

Gebruik de volgende opdrachten om de modules te installeren.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Een nieuwe Python-toepassing maken

1. Gebruik de gewenste IDE om een nieuwe Python-toepassing te maken, bijvoorbeeld **mysample.py**.

2. Voeg het volgende codefragment toe om de vereiste modules te importeren

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Sla de wijzigingen in mysample.py op.

## <a name="authentication"></a>Verificatie

In deze sectie bespreken we de verschillende manieren om te verifiëren met Azure AD. De beschikbare opties zijn:

* Zie voor verificatie van eindgebruikers voor uw toepassing, [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Python](data-lake-store-end-user-authenticate-python.md).
* Zie voor service-naar-serviceverificatie voor uw toepassing, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Python](data-lake-store-service-to-service-authenticate-python.md).

## <a name="create-client-and-data-lake-storage-gen1-account"></a>Client- en Gen1 van Data Lake Storage-account maken

Het volgende fragment maakt u eerst de client Gen1 van Data Lake Storage-account. De clientobject wordt gebruikt om een Data Lake Storage Gen1-account te maken. Ten slotte maakt het fragment een clientobject voor het bestandssysteem.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create Data Lake Storage Gen1 account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Storage Gen1 account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-storage-gen1-accounts"></a>De Gen1 van Data Lake Storage-accounts weergeven

    ## List the existing Data Lake Storage Gen1 accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-storage-gen1-account"></a>Het Gen1 van Data Lake Storage-account verwijderen

    ## Delete an existing Data Lake Storage Gen1 account
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Volgende stappen
* [Bestandssysteembewerkingen in Data Lake Storage Gen1 met behulp van Python](data-lake-store-data-operations-python.md).

## <a name="see-also"></a>Zie ook

* [naslaginformatie over Azure-datalake-store-Python (bestandssysteem)](http://azure-datalake-store.readthedocs.io/en/latest)
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md)
