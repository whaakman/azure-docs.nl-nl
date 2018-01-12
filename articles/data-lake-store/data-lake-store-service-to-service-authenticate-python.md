---
title: 'Verificatie van de service-naar-service: Python met Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van de service to service-verificatie met Data Lake Store met Azure Active Directory met behulp van Python
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
ms.openlocfilehash: c04b870e72c5d29df95d16b96cc423441af6fd85
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-python"></a>Service to service-verificatie met Data Lake Store met behulp van Python
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET-SDK gebruiken](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-service-to-service-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

In dit artikel leert u hoe u met de SDK voor Python Authentication service-naar-service uitvoeren met Azure Data Lake Store. Zie voor de verificatie van de eindgebruiker met Data Lake Store met behulp van Python, [eindgebruiker verificatie met Data Lake Store met behulp van Python](data-lake-store-end-user-authenticate-python.md).


## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory 'Web' toepassing**. U moet voltooid van de stappen in [authentication Service-naar-service met Data Lake Store met Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

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
    import logging, getpass, pprint, uuid, time
    ```

3. Sla de wijzigingen in mysample.py op.

## <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Service-naar-serviceverificatie met clientgeheim voor accountbeheer

In dit fragment gebruiken om te verifiëren met Azure AD voor accountbeheerbewerkingen op Data Lake Store, zoals Data Lake Store-account maken, verwijderen van Data Lake Store-account, enzovoort. Het volgende fragment kan worden gebruikt voor verificatie van uw toepassing niet-interactief, met behulp van het clientgeheim voor een toepassing / service-principal van een bestaande Azure AD 'Web-App' toepassing.

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    RESOURCE = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_credentials(RESOURCE, client_id, client_secret)
    armCreds = AADTokenCredentials(mgmt_token, client_id, resource=RESOURCE)

## <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Service-naar-serviceverificatie met clientgeheim voor bestandssysteembewerkingen

Gebruik het volgende codefragment om te verifiëren met Azure AD voor Bestandssysteembewerkingen op Data Lake Store, zoals maken map, uploadbestand, enzovoort. U kunt het volgende codefragment gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het clientgeheim voor een toepassing/service-principal. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

    adlCreds = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE', resource = 'https://datalake.azure.net/')

<!-- ## Service-to-service authentication with certificate for account management

Use this snippet to authenticate with Azure AD for account management operations on Data Lake Store such as create Data Lake Store account, delete Data Lake Store account, etc. The following snippet can be used to authenticate your application non-interactively, using the certificate of an existing Azure AD "Web App" application. For instructions on how to create an Azure AD application, see [Create service principal with certificates](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

    authority_host_uri = 'https://login.microsoftonline.com'
    tenant = '<TENANT>'
    authority_uri = authority_host_uri + '/' + tenant
    resource_uri = 'https://management.core.windows.net/'
    client_id = '<CLIENT_ID>'
    client_cert = '<CLIENT_CERT>'
    client_cert_thumbprint = '<CLIENT_CERT_THUMBPRINT>'

    context = adal.AuthenticationContext(authority_uri, api_version=None)
    mgmt_token = context.acquire_token_with_client_certificate(resource_uri, client_id, client_cert, client_cert_thumbprint)
    credentials = AADTokenCredentials(mgmt_token, client_id) -->

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe service to service-verificatie gebruiken om te verifiëren met Azure Data Lake Store met behulp van Python. U kunt nu de volgende artikelen die over het gebruik van Python communiceren werken met Azure Data Lake Store bekijken.

* [Accountbeheerbewerkingen op Data Lake Store met behulp van Python](data-lake-store-get-started-python.md)
* [Bewerkingen van de gegevens in Data Lake Store met behulp van Python](data-lake-store-data-operations-python.md)


