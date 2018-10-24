---
title: 'Service-naar-serviceverificatie: Python met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met behulp van Python
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
ms.openlocfilehash: a51eb8f560d598f86caf57b7b24737b0a3574b68
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957210"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-python"></a>Service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van Python
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET-SDK gebruiken](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-service-to-service-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

In dit artikel leert u over het gebruik van de Python-SDK-service-naar-serviceverificatie met Azure Data Lake Storage Gen1 doen. Zie voor verificatie van eindgebruikers met Data Lake Storage Gen1 met behulp van Python, [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Python](data-lake-store-end-user-authenticate-python.md).


## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.6.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Web'**. U moet zijn voltooid de stappen in [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="install-the-modules"></a>De modules installeren

Werken met Data Lake Storage Gen1 met behulp van Python, u moet drie modules installeren.

* Module `azure-mgmt-resource`, die Azure-modules bevat voor Active Directory enzovoort.
* De `azure-mgmt-datalake-store` module, deze de accountbeheerbewerkingen voor Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module [Azure Data Lake-opslagbeheer Gen1 Moduleverwijzing](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* De `azure-datalake-store` module, deze de bestandssysteembewerkingen voor Data Lake Storage Gen1 bevat. Zie voor meer informatie over deze module [Moduleverwijzing voor azure-datalake-store-bestandssysteem](http://azure-datalake-store.readthedocs.io/en/latest/).

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

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
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

Dit fragment gebruiken om te verifiëren met Azure AD voor accountbeheerbewerkingen in Data Lake Storage Gen1, zoals een Gen1 van Data Lake Storage-account, verwijderen een Gen1 met Data Lake Storage-account, enzovoort maken. Het volgende codefragment kan worden gebruikt voor niet-interactief verifiëren van uw toepassing met behulp van het clientgeheim voor een toepassing / service-principal van een bestaande Azure AD 'Web-App' toepassing.

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

Gebruik het volgende codefragment om te verifiëren met Azure AD voor bestandssysteembewerkingen in Data Lake Storage Gen1 zoals maken map, uploadbestand, enzovoort. U kunt het volgende codefragment gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het clientgeheim voor een toepassing/service-principal. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

    tenant = '<TENANT>'
    RESOURCE = 'https://datalake.azure.net/'
    client_id = '<CLIENT_ID>'
    client_secret = '<CLIENT_SECRET>'
    
    adlCreds = lib.auth(tenant_id = tenant,
                    client_secret = client_secret,
                    client_id = client_id,
                    resource = RESOURCE)

<!-- ## Service-to-service authentication with certificate for account management

Use this snippet to authenticate with Azure AD for account management operations on Data Lake Storage Gen1 such as create a Data Lake Storage Gen1 account, delete a Data Lake Storage Gen1 account, etc. The following snippet can be used to authenticate your application non-interactively, using the certificate of an existing Azure AD "Web App" application. For instructions on how to create an Azure AD application, see [Create service principal with certificates](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

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
In dit artikel hebt u geleerd hoe u service-naar-serviceverificatie gebruiken om te verifiëren met Data Lake Storage Gen1 met behulp van Python. U kunt nu de volgende artikelen die over hoe praten u Python gebruikt om te werken met Data Lake Storage Gen1 kijken.

* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van Python](data-lake-store-get-started-python.md)
* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met behulp van Python](data-lake-store-data-operations-python.md)


