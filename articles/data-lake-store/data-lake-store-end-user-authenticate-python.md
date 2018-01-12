---
title: 'Verificatie van de eindgebruiker: Python met Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van de eindgebruiker verificatie met Data Lake Store met Azure Active Directory met behulp van Python
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 1fa8df760ac22ae915765895b498f21d628eea76
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="end-user-authentication-with-data-lake-store-using-python"></a>Verificatie van de eindgebruiker met Data Lake Store met behulp van Python
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET-SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
> 

In dit artikel leert u hoe u met de SDK voor Python eindgebruiker Authentication uitvoeren met Azure Data Lake Store. Verificatie van de eindgebruiker kan verder worden gesplitst in twee categorieën worden onderverdeeld:

* Verificatie van de eindgebruiker zonder multi-factor authentication
* Verificatie van de eindgebruiker met meervoudige verificatie

Deze beide opties worden besproken in dit artikel. Zie voor de verificatie van de service-naar-service met Data Lake Store met behulp van Python, [authentication Service-naar-service met Data Lake Store met behulp van Python](data-lake-store-service-to-service-authenticate-python.md).

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maak een 'Systeemeigen' Azure Active Directory-toepassing**. U moet voltooid van de stappen in [eindgebruiker verificatie met Data Lake Store met Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

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

1. In de IDE van uw keuze, bijvoorbeeld een nieuwe Python-toepassing maken **mysample.py**.

2. Voeg het volgende codefragment toe om de vereiste modules te importeren

    ```
    ## Use this for Azure AD authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    import adal
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, pprint, uuid, time
    ```

3. Sla de wijzigingen in mysample.py op.

## <a name="end-user-authentication-with-multi-factor-authentication"></a>Verificatie van de eindgebruiker met meervoudige verificatie

### <a name="for-account-management"></a>Voor accountbeheer

Het volgende fragment voor verificatie met Azure AD voor accountbeheerbewerkingen op een Data Lake Store-account gebruiken. Het volgende fragment kan worden gebruikt om een toepassing te verifiëren met Multi-Factor Authentication. Geef de waarden onder voor een bestaande Azure AD **systeemeigen** toepassing.

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

Gebruik dit om te verifiëren met Azure AD voor Bestandssysteembewerkingen op een Data Lake Store-account. Het volgende fragment kan worden gebruikt om een toepassing te verifiëren met Multi-Factor Authentication. Geef de waarden onder voor een bestaande Azure AD **systeemeigen** toepassing.

    adlCreds = lib.auth(tenant_id='FILL-IN-HERE', resource = 'https://datalake.azure.net/')

## <a name="end-user-authentication-without-multi-factor-authentication"></a>Verificatie van de eindgebruiker zonder multi-factor authentication

Dit is afgeschaft. Zie voor meer informatie [Azure Authentication met behulp van Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python#mgmt-auth-token).
   
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe eindgebruikers verificatie gebruiken om te verifiëren met Azure Data Lake Store met behulp van Python. U kunt nu de volgende artikelen die over het gebruik van Python communiceren werken met Azure Data Lake Store bekijken.

* [Accountbeheerbewerkingen op Data Lake Store met behulp van Python](data-lake-store-get-started-python.md)
* [Bewerkingen van de gegevens in Data Lake Store met behulp van Python](data-lake-store-data-operations-python.md)

