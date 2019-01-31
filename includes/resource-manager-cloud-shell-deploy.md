---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 7c081b3bc5f9e6273f680b24897f9aced4999afa
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302339"
---
## <a name="deploy-template-from-cloud-shell"></a>Sjabloon implementeren vanuit Cloud Shell

U kunt [Cloud Shell](../articles/cloud-shell/overview.md) gebruiken om uw sjabloon te implementeren. Voor het implementeren van een externe-sjabloon, geeft u de URI van de sjabloon, precies zoals u zou voor elke externe implementatie doen. Voor het implementeren van een lokale sjabloon, moet u eerst uw sjabloon laden in het opslagaccount voor Cloud Shell. In deze sectie wordt beschreven hoe u de sjabloon om uw cloud shell-account te laden en implementeren als een lokaal bestand. Als u dit nog niet hebt Cloud Shell gebruikt, Zie [overzicht van Azure Cloud Shell](../articles/cloud-shell/overview.md) voor informatie over het instellen.

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
