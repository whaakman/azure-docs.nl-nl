---
title: 'Python: Accountbeheerbewerkingen in Azure Data Lake Store | Microsoft Docs'
description: Leer hoe u Python SDK gebruikt met Data Lake Store-accountbeheerbewerkingen.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 834dbc93348e00b0a0821bcbca989b0c2d539056
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="account-management-operations-on-azure-data-lake-store-using-python"></a>Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met Python
> [!div class="op_single_selector"]
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Informatie over het gebruik van Python SDK voor Azure Data Lake Store voor het uitvoeren van basisaccountbeheerbewerkingen, zoals het maken van een Data Lake Store-account, het weergeven van een Data Lake Store-account enzovoort. Als u wilt weten hoe u Python gebruikt voor het uitvoeren van bestandssysteembewerkingen in Data Lake Store, raadpleegt u [Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met Python](data-lake-store-data-operations-python.md).

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een Azure-resourcegroep**. Zie [Een Azure-resourcegroep maken](../azure-resource-manager/resource-group-portal.md) voor instructies.

## <a name="install-the-modules"></a>De modules installeren

U moet drie modules installeren voordat u Data Lake Store kunt gebruiken met Python.

* Module `azure-mgmt-resource`, die Azure-modules bevat voor Active Directory enzovoort.
* Module `azure-mgmt-datalake-store`, die de accountbeheerbewerkingen voor Azure Data Lake Store bevat. Zie [Naslaginformatie over Azure Data Lake Store-beheermodule](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) voor meer informatie over deze module.
* Module `azure-datalake-store`, die de bestandssysteembewerkingen voor Azure Data Lake Store bevat. Zie [Naslaginformatie over Azure Data Lake Store-bestandssysteemmodule](http://azure-datalake-store.readthedocs.io/en/latest/) voor meer informatie over deze module.

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

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
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

* Zie [End-user-authentication with Data Lake Store using Python](data-lake-store-end-user-authenticate-python.md) (Eindgebruikersverificatie met Data Lake Store met behulp van Python) voor de verificatie van eindgebruikers voor uw toepassing.
* Zie [Service-to-service authentication with Data Lake Store using Python](data-lake-store-service-to-service-authenticate-python.md) (Service-naar-serviceverificatie met Data Lake Store met behulp van Python) voor service-naar-serviceverificatie voor uw toepassing.

## <a name="create-client-and-data-lake-store-account"></a>Client en een Data Lake Store-account maken

Met het volgende fragment maakt u eerst de client voor het Data Lake Store-account. In het fragment wordt gebruikgemaakt van het clientobject om een Data Lake Store-account te maken. Ten slotte maakt het fragment een clientobject voor het bestandssysteem.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create data lake store account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-store-accounts"></a>De Data Lake Store-accounts weergeven

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-store-account"></a>Het Data Lake Store-account verwijderen

    ## Delete the existing Data Lake Store accounts
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Volgende stappen
* [Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met Python](data-lake-store-data-operations-python.md).

## <a name="see-also"></a>Zie ook
* [Naslaginformatie over Azure Data Lake Store - Python (Accountbeheer)](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html)
* [Naslaginformatie over Azure Data Lake Store - Python (Bestandssysteem)](http://azure-datalake-store.readthedocs.io/en/latest)
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)
