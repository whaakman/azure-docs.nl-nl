---
title: Opslag configureren met behulp van Azure Files
description: Over het configureren en verbinding maken met Azure-bestanden in Windows-Container in App Service.
author: msangapu-msft
manager: gwallace
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu-msft
ms.openlocfilehash: ea6555e8459b8f372a73d7f943e9a96523d4c791
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789044"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>Azure-bestanden in een Windows-Container in App Service configureren

> [!NOTE]
> In dit artikel is van toepassing op aangepaste Windows-containers. Om te implementeren in App Service op _Linux_, Zie [Content leveren vanuit Azure Storage](./containers/how-to-serve-content-from-azure-storage.md).
>

Deze handleiding wordt beschreven hoe u toegang tot Azure Storage in Windows-Containers. Alleen [Azure Files-Shares](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) en [Premium bestandsshares](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare) worden ondersteund. In deze instructies kunt u Azure-bestandsshares gebruiken. De volgende voordelen beveiligde inhoud, inhoud draagbaarheid, de toegang tot meerdere apps en meerdere methoden voor overdracht.

## <a name="prerequisites"></a>Vereisten

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 of hoger).
- [Een bestaande Windows-Container-app in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Azure-bestandsshare maken](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Bestanden uploaden naar Azure-bestandsshare](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure Files is niet-standaard opslag en apart in rekening gebracht, niet zijn opgenomen in de web-app. Het biedt geen ondersteuning voor het gebruik van firewallconfiguratie vanwege beperkingen van de infrastructuur.
>

## <a name="link-storage-to-your-web-app-preview"></a>Opslag van de koppeling naar uw web-app (preview)

 Voor het koppelen van een Azure-bestandsshare naar een map in uw App Service-app, gebruikt u de [ `az webapp config storage-account add` ](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) opdracht. Opslagtype moet Azure Files.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

U moet dit doen voor alle andere mappen die u wilt dat moet worden gekoppeld aan een Azure-bestanden delen.

## <a name="verify"></a>Verifiëren

Nadat een Azure-bestandsshare is gekoppeld aan een web-app, kunt u dit controleren door de volgende opdracht uit:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```


## <a name="next-steps"></a>Volgende stappen

- [Een ASP.NET-app migreren naar Azure App Service met behulp van een Windows-container (Preview)](app-service-web-tutorial-windows-containers-custom-fonts.md).