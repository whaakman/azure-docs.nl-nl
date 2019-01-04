---
title: Verbinding maken met Azure Data Explorer in Azure Databricks met behulp van Python
description: Dit onderwerp ziet u hoe u een Python-bibliotheek in Azure Databricks voor toegang tot gegevens uit Azure Data Explorer met behulp van een van de twee ondersteunde verificatiemethoden.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 1101a89fd4ddb0e020d0bac237e6119b137fa978
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017494"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-by-using-python"></a>Verbinding maken met Azure Data Explorer in Azure Databricks met behulp van Python

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) is een Apache Spark gebaseerd analyseplatform dat geoptimaliseerd voor het Microsoft Azure-platform. In dit artikel wordt beschreven hoe u een Python-bibliotheek in Azure Databricks gebruiken voor toegang tot gegevens uit Azure Data Explorer. Er zijn verschillende manieren om te verifiëren met Azure Data Explorer, met inbegrip van een apparaataanmelding en een Azure Active Directory (Azure AD)-app.

## <a name="prerequisites"></a>Vereisten

- [Maak een Azure Data Explorer-cluster en de database](/azure/data-explorer/create-cluster-database-portal).
- [Een Azure Databricks-werkruimte maken](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace). Onder **Azure Databricks Service**, in de **prijscategorie** vervolgkeuzelijst, selecteer **Premium**. Deze selectie kunt u Azure Databricks-geheimen gebruiken om uw referenties op te slaan en ernaar te verwijzen in notitieblokken en -taken.

- [Een cluster maken](https://docs.azuredatabricks.net/user-guide/clusters/create.html) in Azure Databricks met de volgende specificaties (de minimale instellingen die nodig zijn om uit te voeren van de voorbeeld-notebooks):

   ![De specificaties voor het maken van een cluster](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-the-python-library-on-your-azure-databricks-cluster"></a>Installeer de Python-bibliotheek op uw Azure Databricks-cluster

Voor het installeren van de [Python-bibliotheek](/azure/kusto/api/python/kusto-python-client-library) op uw Azure Databricks-cluster:

1. Ga naar uw Azure Databricks-werkruimte en [maakt u een bibliotheek](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library).
2. [Uploaden van een Python-PyPI pakket of Python-ei](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg).
   - Uploaden, installeren en de bibliotheek koppelen aan uw Databricks-cluster.
   - Voer de naam PyPi: **azure-kusto-data**.

## <a name="connect-to-azure-data-explorer-by-using-a-device-login"></a>Verbinding maken met Azure Data Explorer met behulp van een apparaataanmelding

[Importeren van een laptop](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) met behulp van de [Query-ADX-apparaat-aanmelding](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) notebook. U kunt vervolgens naar Azure Data Explorer verbinden met uw referenties.

## <a name="connect-to-adx-by-using-an-azure-ad-app"></a>Verbinding maken met ADX met behulp van een Azure AD-app

1. Maken van Azure AD-app door [inrichten van een Azure AD-toepassing](/azure/kusto/management/access-control/how-to-provision-aad-app).
1. Toegang verlenen aan uw Azure AD-app in uw Azure Data Explorer-database als volgt te werk:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | de databasenaam van uw |
    | ```AAD App ID``` | uw Azure AD-app-ID |
    | ```AAD Tenant ID``` | uw Azure AD-tenant-ID |

### <a name="find-your-azure-ad-tenant-id"></a>Uw Azure AD-tenant-ID vinden

Als u wilt verifiëren van een toepassing, Azure Data Explorer maakt gebruik van uw Azure AD-tenant-ID. Als u wilt zoeken in uw tenant-ID, de volgende URL te gebruiken. Vervang uw domein voor *uwdomein*.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Dus als uw domein *contoso.com* is, wordt de URL bijvoorbeeld: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Selecteer deze URL om de resultaten te bekijken. De eerste regel is als volgt: 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Uw tenant-ID is `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Store en Beveilig uw Azure AD-app-ID en -sleutel 

Store en beveiligen van uw Azure AD-app-ID en -sleutel met behulp van Azure Databricks [geheimen](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) als volgt:
1. [Instellen van de CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli).
1. [Installeer de CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 
1. [Verificatie instellen](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Configureer de [geheimen](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) met behulp van de volgende voorbeeldopdrachten:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Importeren van een laptop
[Importeren van een laptop](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) met behulp van de [Query-ADX-AAD-App](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) notebook verbinding maken met Azure Data Explorer. Bijwerken van de tijdelijke aanduiding met de clusternaam, databasenaam en Azure AD-tenant-ID.
