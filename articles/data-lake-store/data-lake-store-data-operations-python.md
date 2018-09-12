---
title: 'Python: Bestandssysteembewerkingen in Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Leer hoe u Python SDK gebruiken om te werken met het bestandssysteem van Data Lake Storage Gen1.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 33abaf7488579a501dc7e2d0b63645726b86c28b
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44390719"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-python"></a>Bestandssysteembewerkingen in Azure Data Lake Storage Gen1 met behulp van Python
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

In dit artikel leert u hoe u Python SDK gebruikt voor het uitvoeren van bestandssysteembewerkingen in Azure Data Lake Storage Gen1. Zie voor instructies over het uitvoeren van accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van Python [accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van Python](data-lake-store-get-started-python.md).

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Storage Gen1 account**. Volg de instructies op [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](data-lake-store-get-started-portal.md).

## <a name="install-the-modules"></a>De modules installeren

Werken met Data Lake Storage Gen1 met behulp van Python, u moet drie modules installeren.

* Module `azure-mgmt-resource`, die Azure-modules bevat voor Active Directory enzovoort.
* De `azure-mgmt-datalake-store` module, deze de accountbeheerbewerkingen voor Azure Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module de [azure-mgmt-datalake-store Moduleverwijzing](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* De `azure-datalake-store` module, deze de bestandssysteembewerkingen voor Azure Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module de [azure-datalake-store-bestandssysteem Moduleverwijzing](http://azure-datalake-store.readthedocs.io/en/latest/).

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

    ## Required for Azure Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Storage Gen1 filesystem management
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

## <a name="create-filesystem-client"></a>Bestandssysteemclient maken

Het volgende fragment maakt u eerst de client Gen1 van Data Lake Storage-account. De clientobject wordt gebruikt om een Data Lake Storage Gen1-account te maken. Ten slotte maakt het fragment een clientobject voor het bestandssysteem.

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
* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van Python](data-lake-store-get-started-python.md).

## <a name="see-also"></a>Zie ook

* [Naslaginformatie voor Azure Data Lake Storage Gen1 Python (bestandssysteem)](http://azure-datalake-store.readthedocs.io/en/latest)
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md)
