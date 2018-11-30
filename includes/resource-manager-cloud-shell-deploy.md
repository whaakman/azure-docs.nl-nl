---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/25/2018
ms.author: tomfitz
ms.openlocfilehash: a2ee8705be3f34b6df113c68d88e375411f84bf2
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440092"
---
## <a name="deploy-template-from-cloud-shell"></a>Sjabloon implementeren vanuit Cloud Shell

U kunt [Cloud Shell](../articles/cloud-shell/overview.md) gebruiken om uw sjabloon te implementeren. U moet echter eerst de sjabloon voor het laden naar het opslagaccount voor Cloud Shell. Als u Cloud Shell niet hebt gebruikt, raadpleegt u [Overzicht van Azure Cloud Shell](../articles/cloud-shell/overview.md) voor informatie over het instellen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer de Cloud Shell-resourcegroep. Het naampatroon is `cloud-shell-storage-<region>`.

   ![Resourcegroep selecteren](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Selecteer het opslagaccount voor Cloud Shell.

   ![Opslagaccount selecteren](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Selecteer **Blobs**.

   ![Blobs selecteren](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. Selecteer **+ Container**.

   ![Container toevoegen](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Geef uw container een naam en een niveau van toegang. De voorbeeldsjabloon in dit artikel bevat geen gevoelige informatie, waardoor het anonieme leestoegang. Selecteer **OK**.

   ![Container-waarden opgeven](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Selecteer de container die u hebt gemaakt.

   ![Nieuwe container selecteren](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. Selecteer **Uploaden**.

   ![Blob uploaden](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Zoek de sjabloon en upload deze.

   ![Bestand uploaden](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Nadat deze is geüpload, selecteert u de sjabloon.

   ![Selecteer de nieuwe sjabloon](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. Kopieer de URL.

   ![URL kopiëren](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. Open de prompt.

   ![Cloud Shell openen](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
