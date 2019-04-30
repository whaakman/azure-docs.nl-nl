---
title: Verbinding maken met Azure Media Services v3 API - Python
description: Leer hoe u verbinding maken met Media Services v3 API met Python.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 971e36b600a2c6be516e39ce84ca5780a2f23bbd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733093"
---
# <a name="connect-to-media-services-v3-api---python"></a>Verbinding maken met de API van Media Services v3 - Python

Dit artikel ziet u hoe u verbinding maakt met de SDK van Azure Media Services v3 Python met behulp van de service principal-aanmelding in de methode.

## <a name="prerequisites"></a>Vereisten

- Download Python van [python.org](https://www.python.org/downloads/)
- Zorg ervoor dat u stelt de `PATH` omgevingsvariabele
- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Moet u om te onthouden naam van de resourcegroep en de naam van de Media Services-account.
- Volg de stappen in de [Acces-API's](access-api-cli-how-to.md) onderwerp. Noteer de abonnements-ID, toepassings-ID (client-ID), de verificatiesleutel (geheim genoemd) en de tenant-ID die u nodig hebt in de volgende stap.

## <a name="install-the-modules"></a>De modules installeren

Als u wilt werken met Azure Media Services met behulp van Python, moet u deze modules installeren.

* De `azure-mgmt-resource` -module, waaronder Azure-modules voor Active Directory.
* De `azure-mgmt-media` module, deze de Media Services-entiteiten bevat.

Open een opdrachtregel-hulpprogramma en de volgende opdrachten gebruiken om de modules te installeren.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==1.1.1
```

## <a name="connect-to-the-python-client"></a>Verbinding maken met de Python-client

1. Maak een bestand met een `.py` extensie
1. Open het bestand in uw favoriete editor
1. Voeg de code die op het bestand volgt. De code importeert de vereiste modules en het Active Directory-referenties-object moet u verbinding met Media Services worden gemaakt.

      Stel de variabelen waarden in op de waarden die u hebt verkregen via [Acces-API's](access-api-cli-how-to.md)

      ```
      import adal
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
      from azure.mgmt.media import AzureMediaServices
      from azure.mgmt.media.models import MediaService

      RESOURCE = 'https://management.core.windows.net/'
      # Tenant ID for your Azure Subscription
      TENANT_ID = '00000000-0000-0000-000000000000'
      # Your Service Principal App ID
      CLIENT = '00000000-0000-0000-000000000000'
      # Your Service Principal Password
      KEY = '00000000-0000-0000-000000000000'
      # Your Azure Subscription ID
      SUBSCRIPTION_ID = '00000000-0000-0000-000000000000'
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = 'amsv3account'
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = 'AMSv3ResourceGroup'

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials,
          RESOURCE,
          CLIENT,
          KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print (client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. Het bestand uitvoeren

## <a name="next-steps"></a>Volgende stappen

- Gebruik [Python-SDK](https://aka.ms/ams-v3-python-sdk).
- Raadpleeg de [Python-naslagdocumentatie](https://aka.ms/ams-v3-python-ref) van Media Services.
