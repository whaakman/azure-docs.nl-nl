---
title: 'Verificatie van eindgebruikers: Python met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met Python
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
ms.openlocfilehash: 1ba7dbd9436a15989564a806a7c8f586c01e5243
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128066"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-python"></a>Verificatie van de eindgebruiker met Azure Data Lake Storage Gen1 met behulp van Python
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET-SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
> 

In dit artikel leert u over het gebruik van de Python-SDK voor verificatie van eindgebruikers met Azure Data Lake Storage Gen1. Verificatie van de eindgebruiker kan verder worden gesplitst in twee categorieën:

* Verificatie van eindgebruikers zonder multi-factor authentication
* Verificatie van eindgebruikers met multi-factor authentication

Deze beide opties worden in dit artikel besproken. Zie voor service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Python, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Python](data-lake-store-service-to-service-authenticate-python.md).

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Native'**. U moet zijn voltooid de stappen in [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="install-the-modules"></a>De modules installeren

Werken met Data Lake Storage Gen1 met behulp van Python, u moet drie modules installeren.

* Module `azure-mgmt-resource`, die Azure-modules bevat voor Active Directory enzovoort.
* De `azure-mgmt-datalake-store` module, deze de accountbeheerbewerkingen voor Azure Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module [Azure Data Lake-opslagbeheer Gen1 Moduleverwijzing](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* De `azure-datalake-store` module, deze de bestandssysteembewerkingen voor Azure Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module [Moduleverwijzing voor azure-datalake-store-bestandssysteem](http://azure-datalake-store.readthedocs.io/en/latest/).

Gebruik de volgende opdrachten om de modules te installeren.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Een nieuwe Python-toepassing maken

1. In de IDE van uw keuze, bijvoorbeeld een nieuwe Python-toepassing maken **mysample.py**.

2. Voeg het volgende codefragment toe om de vereiste modules te importeren

    ```
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, pprint, uuid, time
    ```

3. Sla de wijzigingen in mysample.py op.

## <a name="end-user-authentication-with-multi-factor-authentication"></a>Verificatie van eindgebruikers met multi-factor authentication

### <a name="for-account-management"></a>Voor accountbeheer

Gebruik het volgende codefragment om te verifiëren met Azure AD voor accountbeheerbewerkingen in een Data Lake Storage Gen1-account. Het volgende fragment kan worden gebruikt om een toepassing te verifiëren met Multi-Factor Authentication. Geef de waarden hieronder voor een bestaande Azure AD **systeemeigen** toepassing.

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource = RESOURCE)

### <a name="for-filesystem-operations"></a>Voor bestandssysteembewerkingen

Gebruik dit om te verifiëren met Azure AD bestandssysteembewerkingen in een Data Lake Storage Gen1-account. Het volgende fragment kan worden gebruikt om een toepassing te verifiëren met Multi-Factor Authentication. Geef de waarden hieronder voor een bestaande Azure AD **systeemeigen** toepassing.

    adlCreds = lib.auth(tenant_id='FILL-IN-HERE', resource = 'https://datalake.azure.net/')

## <a name="end-user-authentication-without-multi-factor-authentication"></a>Verificatie van eindgebruikers zonder multi-factor authentication

Dit is afgeschaft. Zie voor meer informatie, [Azure-verificatie met behulp van Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python#mgmt-auth-token).
   
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u verificatie van eindgebruikers om u te verifiëren met Azure Data Lake Storage Gen1 met behulp van Python. U kunt nu de volgende artikelen die over hoe praten u Python gebruikt om te werken met Azure Data Lake Storage Gen1 kijken.

* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van Python](data-lake-store-get-started-python.md)
* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met behulp van Python](data-lake-store-data-operations-python.md)

