---
title: Aan de slag met Azure Data Lake Store met de Python-SDK | Microsoft Docs
description: Leer hoe u de Python-SDK gebruikt met Data Lake Store-accounts en het bestandssysteem.
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
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 9e4efc9de7979c98fcb4afbe530c73e9013326c3
ms.lasthandoff: 04/06/2017


---

# <a name="get-started-with-azure-data-lake-store-using-python"></a>Aan de slag met Azure Data Lake Store met Python

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Lees hoe u met de Python-SDK voor Azure en Azure Data Lake Store basisbewerkingen uitvoert, zoals het maken van mappen, het uploaden en downloaden van gegevensbestanden enz. Zie [Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake.

## <a name="prerequisites"></a>Vereisten

* **Python**. U kunt Python [hier](https://www.python.org/downloads/) downloaden. In dit artikel wordt Python 3.5.2 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een Azure Active Directory-toepassing maken**. U gebruikt de Azure AD-toepassing om de Data Lake Store-toepassing te verifiëren in Azure AD. Er zijn verschillende manieren om te verifiëren in Azure AD, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Verifiëren met Data Lake Store met behulp van Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.

## <a name="install-the-modules"></a>De modules installeren

U moet drie modules installeren voordat u Data Lake Store kunt gebruiken met Python.

* De module `azure-mgmt-resource`. Deze bevat Azure-modules voor Active Directory enz.
* De module `azure-mgmt-datalake-store`. Deze bevat de accountbeheerbewerkingen voor Azure Data Lake Store. Zie [Naslaginformatie over Azure Data Lake Store-beheermodule](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html) voor meer informatie over deze module.
* De module `azure-datalake-store`. Deze bevat de bestandssysteembewerkingen voor Azure Data Lake Store. Zie [Naslaginformatie over Azure Data Lake Store-bestandssysteemmodule](http://azure-datalake-store.readthedocs.io/en/latest/) voor meer informatie over deze module.

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

* Verificatie van de eindgebruiker
* Verificatie van service-tot-service
* Multi-Factor Authentication

U moet deze verificatieopties gebruiken voor zowel accountbeheer- als bestandssysteembeheermodules.

### <a name="end-user-authentication-for-account-management"></a>Verificatie van de eindgebruiker voor accountbeheer

Hiermee kunt u accountbeheerbewerkingen verifiëren met Azure AD (Data Lake Store-account maken/verwijderen enz.). U moet een gebruikersnaam en wachtwoord voor een Azure AD-gebruiker opgeven. De gebruiker mag niet zijn geconfigureerd voor meervoudige verificatie.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="end-user-authentication-for-filesystem-operations"></a>Verificatie van de eindgebruiker voor bestandssysteembewerkingen

Hiermee kunt u met Azure AD bestandssysteembewerkingen (map maken, bestand uploaden enz.) verifiëren. Gebruik dit met een bestaande **native clienttoepassing** van Azure AD. De Azure AD-gebruiker voor wie u referenties verstrekt, mag niet zijn geconfigureerd voor meervoudige verificatie.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Service-naar-serviceverificatie met clientgeheim voor accountbeheer

Hiermee kunt u accountbeheerbewerkingen verifiëren met Azure AD (Data Lake Store-account maken/verwijderen enz.). U kunt het volgende codefragment gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het clientgeheim voor een toepassing/service-principal. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

### <a name="service-to-service-authentication-with-client-secret-for-filesystem-operations"></a>Service-naar-serviceverificatie met clientgeheim voor bestandssysteembewerkingen

Hiermee kunt u met Azure AD bestandssysteembewerkingen (map maken, bestand uploaden enz.) verifiëren. U kunt het volgende codefragment gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het clientgeheim voor een toepassing/service-principal. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

### <a name="multi-factor-authentication-for-account-management"></a>Multi-Factor Authentication voor accountbeheer

Hiermee kunt u accountbeheerbewerkingen verifiëren met Azure AD (Data Lake Store-account maken/verwijderen enz.). Het volgende fragment kan worden gebruikt om een toepassing te verifiëren met Multi-Factor Authentication. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

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
    credentials = AADTokenCredentials(mgmt_token, client_id)

### <a name="multi-factor-authentication-for-filesystem-management"></a>Multi-Factor Authentication voor bestandssysteembeheer

Hiermee kunt u met Azure AD bestandssysteembewerkingen (map maken, bestand uploaden enz.) verifiëren. Het volgende fragment kan worden gebruikt om een toepassing te verifiëren met Multi-Factor Authentication. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

    token = lib.auth(tenant_id='FILL-IN-HERE')

## <a name="create-an-azure-resource-group"></a>Een Azure-resourcegroep maken

Gebruik het volgende codefragment om een Azure-resourcegroep te maken:

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

## <a name="create-clients-and-data-lake-store-account"></a>Clients en een Data Lake Store-account maken

Met het volgende fragment maakt u eerst de client voor het Data Lake Store-account. In het fragment wordt gebruikgemaakt van het clientobject om een Data Lake Store-account te maken. Ten slotte maakt het fragment een clientobject voor het bestandssysteem.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

## <a name="list-the-data-lake-store-accounts"></a>De Data Lake Store-accounts weergeven

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

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

## <a name="see-also"></a>Zie ook

- [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Naslaginformatie over Data Lake Store .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx)
- [Naslaginformatie over Data Lake Store REST](https://msdn.microsoft.com/library/mt693424.aspx)

