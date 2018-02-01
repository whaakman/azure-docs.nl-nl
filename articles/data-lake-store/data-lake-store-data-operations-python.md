---
title: 'Python: Bestandssysteembewerkingen in Azure Data Lake Store | Microsoft Docs'
description: Leer hoe u Python SDK gebruikt met het Data Lake Store-bestandssysteem.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 19e41cb0bb22bf27db4581954f1f6e68b8a941c2
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-python"></a>Bestandssysteembewerkingen in Azure Data Lake Store met behulp van Python
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

In dit artikel leert u hoe u Python SDK gebruikt voor het uitvoeren van bestandssysteembewerkingen in Azure Data Lake Store. Als u wilt weten hoe u Python SDK gebruikt voor het uitvoeren van accountbeheerbewerkingen in Data Lake Store, raadpleegt u [Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met Python SDK](data-lake-store-get-started-python.md).

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Store-account**. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md).

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

2. Voeg de volgende regels toe om de vereiste modules te importeren.

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

## <a name="create-filesystem-client"></a>Bestandssysteemclient maken

Met het volgende fragment maakt u eerst de client voor het Data Lake Store-account. In het fragment wordt gebruikgemaakt van het clientobject om een Data Lake Store-account te maken. Ten slotte maakt het fragment een clientobject voor het bestandssysteem.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(adlCreds, store_name=adlsAccountName)

## <a name="create-a-directory"></a>Een map maken

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

## <a name="upload-a-file"></a>Bestand uploaden


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


## <a name="download-a-file"></a>Bestand downloaden

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

## <a name="delete-a-directory"></a>Een map verwijderen

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

## <a name="next-steps"></a>Volgende stappen
* [Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met Python](data-lake-store-get-started-python.md).

## <a name="see-also"></a>Zie ook
* [Naslaginformatie over Azure Data Lake Store - Python (Accountbeheer)](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html)
* [Naslaginformatie over Azure Data Lake Store - Python (Bestandssysteem)](http://azure-datalake-store.readthedocs.io/en/latest)
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)
