---
title: Aan de slag met Azure Data Lake Analytics met Python | Microsoft Docs
description: 'Leer hoe u met Python een Data Lake Store-account maakt en taken verzendt. '
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 5618650671badfc54860c3ad8af5d1e727d3d8c9
ms.openlocfilehash: 40ccfc59cccd86a7634ec89656571b3cd23566b4
ms.lasthandoff: 11/23/2016


---


# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-python"></a>Zelfstudie: Aan de slag met Azure Data Lake Analytics met Python
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van Python voor het maken van Azure Data Lake Analytics-accounts, het definiëren van Data Lake Analytics-taken in [U-SQL](data-lake-analytics-u-sql-get-started.md) en het verzenden van taken naar Data Lake Analytics-accounts. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

In deze zelfstudie gaat u een taak ontwikkelen die een bestand met door tabs gescheiden waarden (TSV) leest en converteert naar een bestand met door komma's gescheiden waarden (CSV). Om de zelfstudie te volgen met andere ondersteunde hulpprogramma’s klikt u op de tabbladen boven aan deze sectie.

##<a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

- **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
- **Een Azure Active Directory-toepassing**. U gebruikt de Azure AD-toepassing om de Data Lake Store-toepassing te verifiëren in Azure AD. Er zijn verschillende manieren om te verifiëren in Azure AD, zoals verificatie door eindgebruikers en service-naar-serviceverificatie. Zie [Verifiëren met Data Lake Store met behulp van Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.
- **[Python](https://www.python.org/downloads/)**. U moet de 64-bits versie gebruiken. In dit artikel wordt Python versie 3.5.2 gebruikt.


## <a name="install-azure-python-sdk"></a>Azure Python-SDK installeren

U moet drie modules installeren voordat u Data Lake Store kunt gebruiken met Python.

De module azure-mgmt-datalake-store bevat de accountbeheerbewerkingen voor Azure Data Lake Store. De module azure-mgmt-resource bevat andere Azure-modules voor Active Directory enzovoort. De Azure Data Lake Store-module bevat de bestandssysteembewerkingen voor Azure Data Lake Store. De Azure Data Lake Analytics-module bevat de bewerkingen voor Azure Data Lake Analytics. Gebruik de volgende opdrachten om de modules te installeren.

    pip install azure-mgmt-resource
    pip install azure-mgmt-datalake-store
    pip install azure-mgmt-datalake-analytics
    pip install azure-datalake-store

## <a name="create-a-python-application"></a>Een Python-toepassing maken

1. Gebruik de gewenste IDE om een nieuwe Python-toepassing te maken, bijvoorbeeld mysample.py.

2. Voeg de volgende regels toe om de vereiste modules te importeren.

        ## Use this only for Azure AD service-to-service authentication
        from azure.common.credentials import ServicePrincipalCredentials

        ## Use this only for Azure AD end-user authentication
        from azure.common.credentials import UserPassCredentials

        ## Required for Azure Resource Manager
        from azure.mgmt.resource.resources import ResourceManagementClient
        from azure.mgmt.resource.resources.models import ResourceGroup

        ## Required for Azure Data Lake Store account management
        from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
        from azure.mgmt.datalake.store.models import DataLakeStoreAccount

        ## Required for Azure Data Lake Store filesystem management
        from azure.datalake.store import core, lib, multithread

        ## Required for Azure Data Lake Analytics account management
        from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeAnalyticsAccountProperties, DataLakeStoreAccountInfo

        ## Required for Azure Data Lake Analytics job management
        from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
        from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

        ## Required for Azure Data Lake Analytics catalog management
        from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

        ## Use these as needed for your application
        import logging, getpass, pprint, uuid, time

3. Sla de wijzigingen op in het Python-toepassingsbestand.

## <a name="authentication"></a>Authentication

Gebruik een van de volgende methoden voor verificatie:

### <a name="end-user-authentication-for-account-management"></a>Verificatie van de eindgebruiker voor accountbeheer

Gebruik deze methode om accountbeheerbewerkingen te verifiëren met Azure AD (Data Lake Store-account maken/verwijderen enz.). U moet een gebruikersnaam en wachtwoord voor een Azure AD-gebruiker opgeven. Het gebruikersaccount kan niet worden geconfigureerd voor meervoudige verificatie en het account kan geen Microsoft-account of Live ID zijn, bijvoorbeeld @outlook.com, en @live.com.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Service-naar-serviceverificatie met clientgeheim voor accountbeheer

Gebruik deze methode om accountbeheerbewerkingen te verifiëren met Azure AD (Data Lake Store-account maken/verwijderen enz.). U kunt het volgende codefragment gebruiken voor het niet-interactief verifiëren van uw toepassing, door gebruik te maken van het clientgeheim voor een toepassing/service-principal. Gebruik dit met een bestaande Azure AD-toepassing voor webtoepassingen.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

## <a name="create-resource-management-group"></a>Bronbeheergroep maken

Gebruik het volgende codefragment om een Azure-resourcegroep te maken:

    ## Declare variables
    subscriptionId= '<Azure Subscription ID>'
    resourceGroup = '<Azure Resource Group Name>'
    location = '<Location>' # i.e. 'eastus2'

    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )

    ## Create an Azure Resource Group
    armGroupResult = resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )


## <a name="create-data-lake-store-account"></a>Data Lake Store-account maken

Elk Data Lake Analytics-account vereist een Data Lake Store-account. Zie [Een Data Lake Store-account maken](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) voor instructies.



## <a name="create-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken

    ## Declare variables
    subscriptionId= '<Azure Subscription ID>'
    resourceGroup = '<Azure Resource Group Name>'
    location = '<Location>' # i.e. 'eastus2'
    adlsAccountName = '<Azure Data Lake Store Account Name>'
    adlaAccountName = '<Azure Data Lake Analytics Account Name>'

    ## Create management client object
    adlaAcctClient = DataLakeAnalyticsAccountManagementClient(
        credentials,
        subscriptionId
    )

    ## Create an Azure Data Lake Analytics account
    adlaAcctResult = adlaAcctClient.account.create(
        resourceGroup,
        adlaAccountName,
        DataLakeAnalyticsAccount(
            location=location,
            default_data_lake_store_account=adlsAccountName,
            data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adlsAccountName)]
        )
    ).wait()


##<a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics-taken verzenden

Data Lake Analytics-taken worden geschreven in de U-SQL-taal. Zie [Aan de slag met de U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](http://go.microsoft.com/fwlink/?LinkId=691348) voor meer informatie over U-SQL.

    ## Declare variables
    adlsAccountName = '<Azure Data Lake Store Account Name>'

    ## Create management client object
    adlaJobClient = DataLakeAnalyticsJobManagementClient(
        credentials,
        'azuredatalakeanalytics.net'
    )

    ## Submit a U-SQL job
    jobId = str(uuid.uuid4())
    jobResult = adlaJobClient.job.create(
        adlaAccountName,
        jobId,
        JobInformation(
            name='Sample Job',
            type='USql',
            properties=USqlJobProperties(
                script='DROP DATABASE IF EXISTS FOO; CREATE DATABASE FOO;'
            )
        )
    )

    ## Print the job result
    while(jobResult.state != JobState.ended):
        print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
        time.sleep(3)
        jobResult = adlaJobClient.job.get(adlaAccountName, jobId)
        
    print ('Job finished with result: ' + jobResult.result.value)

## <a name="next-steps"></a>Volgende stappen

- Als u dezelfde zelfstudie wilt bekijken met een ander hulpprogramma, klikt u op de tabselectors boven aan de pagina.
- Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
- Zie [U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) om aan de slag te gaan met het ontwikkelen van U-SQL-toepassingen.
- Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
- Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.
- Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor een overzicht van Data Lake Analytics.


