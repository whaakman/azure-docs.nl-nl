---
title: Verbinding maken met Azure Data Explorer vanuit Azure Databricks met behulp van Python
description: Dit onderwerp wordt beschreven hoe u Python-bibliotheek in Azure Databricks voor toegang tot gegevens uit Azure Data Explorer (ADX) met behulp van een van twee verificatiemethoden.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 53f51db8d1b495f9a6faec86450d2b4e08a4fb72
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52428520"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-using-python"></a>Verbinding maken met Azure Data Explorer vanuit Azure Databricks met behulp van Python

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) is een op Apache Spark gebaseerd analyseplatform, geoptimaliseerd voor het Microsoft Azure-platform voor cloudservices. Dit artikel ziet u hoe u Python-bibliotheek in Azure Databricks voor toegang tot gegevens uit Azure Data Explorer (ADX). Er zijn verschillende manieren om te verifiëren met ADX inclusief apparaataanmelding en Azure Active Directory (Azure AD)-App.

## <a name="prerequisites"></a>Vereisten

- [Een cluster van Azure Data Explorer en -database maken](/azure/data-explorer/create-cluster-database-portal)
- [Een Azure Databricks-werkruimte maken](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace)

    Onder **Azure Databricks Service**, in de **prijscategorie** vervolgkeuzelijst **Premium**. Hiermee kunt u Azure Databricks-geheimen gebruiken om uw referenties op te slaan en ernaar te verwijzen in notitieblokken en -taken.

- [Een cluster maken](https://docs.azuredatabricks.net/user-guide/clusters/create.html) in Azure Databricks met de volgende specificaties (de minimale instellingen die nodig zijn om uit te voeren van de voorbeeld-notebooks):

![Cluster maken](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-python-library-on-your-azure-databricks-cluster"></a>Python-bibliotheek installeren op uw Azure Databricks-cluster

Voor het installeren van [Python-bibliotheek](/azure/kusto/api/python/kusto-python-client-library) op uw Azure Databricks-cluster:

1. Ga naar uw Azure Databricks-werkruimte en [maakt u een bibliotheek](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)
2. [Een Python-PyPI pakket of Python-ei uploaden](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg)
    - Uploaden, installeren en de bibliotheek koppelen aan uw Databricks-cluster.
    - Voer de naam PyPi: *azure kusto-gegevens*

## <a name="connect-to-adx-using-device-login"></a>Verbinding maken met ADX met behulp van apparaataanmelding

[Importeren van een laptop](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) met behulp van de [Query-ADX-apparaat-aanmelding](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) notebook verbinding maken met ADX met uw referenties.

## <a name="connect-to-adx-using-azure-ad-app"></a>Verbinding maken met ADX met behulp van Azure AD-App

1. Azure AD-App door maken [inrichten van een AAD-toepassing](/azure/kusto/management/access-control/how-to-provision-aad-app).
1. Toegang verlenen aan uw Azure AD-App op uw Azure Data Explorer-database als volgt te werk:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | de databasenaam van uw |
    | ```AAD App ID``` | uw Azure AD-App-ID |
    | ```AAD Tenant ID``` | uw Azure AD-Tenant-ID |

### <a name="find-your-azure-ad-tenant-id"></a>Uw Azure AD-Tenant-ID vinden

Als u wilt verifiëren van een toepassing, Azure Data Explorer maakt gebruik van uw Azure AD-tenant-ID. Om uw tenant-id te vinden, gebruikt u de volgende URL, waarbij u *YourDomain* vervangt door uw domeinnaam.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Dus als uw domein *contoso.com* is, wordt de URL bijvoorbeeld: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Klik op deze URL om de resultaten weer te geven. De eerste regel is als volgt. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Uw tenant-ID is `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Store en Beveilig uw Azure AD-App-ID en -sleutel 

Store en Beveilig uw Azure AD-App-ID en -sleutel met behulp van Azure Databricks [geheimen](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) als volgt:
1. [De CLI instellen](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli)
1. [De CLI installeren](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli) 
1. [Verificatie instellen](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Configureer de [geheimen](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) met behulp van de volgende voorbeeldopdrachten:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Importeren van een laptop
[Importeren van een laptop](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) met behulp van de [Query-ADX-AAD-App](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) notebook ADX verbinden. Bijwerken van de tijdelijke aanduiding met de clusternaam, databasenaam en Azure AD-Tenant-ID.