---
title: Inhoud leveren vanuit Azure Storage op Linux - App Service
description: Over het configureren en inhoud leveren vanuit Azure Storage in Azure App Service on Linux.
author: msangapu
manager: jeconnoc
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 2/04/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 29f2b725972f5ce92e756cb21621a19850ba9386
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55732937"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Inhoud leveren vanuit Azure Storage in App Service onder Linux

Deze handleiding wordt beschreven hoe u aan het leveren van statische inhoud in App Service on Linux met behulp van [Azure Storage](/azure/storage/common/storage-introduction). De volgende voordelen beveiligde inhoud, inhoud draagbaarheid, de toegang tot meerdere apps en meerdere methoden voor overdracht. In deze handleiding leert u hoe u inhoud leveren in Azure Storage door [aangepaste opslag configureren](https://blogs.msdn.microsoft.com/appserviceteam/2018/09/24/announcing-bring-your-own-storage-to-app-service/).

## <a name="prerequisites"></a>Vereisten

- Een bestaande web-app (App Service op Linux- of Web App for Containers).
- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 of hoger).

## <a name="create-azure-storage"></a>Azure-opslag maken

> [!NOTE]
> Azure-opslag is niet-standaard opslag en apart in rekening gebracht, niet zijn opgenomen in de web-app.
>

Maak een Azure [Azure storage-account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Bestanden uploaden naar Azure Storage

Als u wilt uploaden een lokale map met de storage-account, gebruikt u de [ `az storage blob upload-batch` ](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch) opdracht als in het volgende voorbeeld:

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app-preview"></a>Opslag van de koppeling naar uw web-app (preview)

> [!CAUTION]
> Een bestaande map in een web-app koppelen aan een opslagaccount, wordt de inhoud van de map verwijderd. Als u bestanden voor een bestaande app migreren, moet u een back-up van uw app en de bijbehorende inhoud voordat u begint.
>

Voor het koppelen van een storage-account naar een map in uw App Service-app, gebruikt u de [ `az webapp config storage-account add` ](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) opdracht. Opslagtype is AzureBlob of Azure Files. AzureBlob worden gebruikt voor deze container.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

U moet dit doen voor alle andere mappen die u wilt dat moet worden gekoppeld aan een storage-account.

## <a name="verify"></a>Verifiëren

Nadat een opslagcontainer is gekoppeld aan een web-app, kunt u dit controleren door de volgende opdracht uit:

```azurecli
az webapp config storage-account list --resource-group <group_name> --name <app_name>
```

## <a name="next-steps"></a>Volgende stappen

- [Web-apps configureren in Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-configure).
