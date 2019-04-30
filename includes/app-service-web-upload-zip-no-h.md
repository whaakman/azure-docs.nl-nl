---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 10/24/2018
ms.author: cephalin
ms.openlocfilehash: e4bc749047bbf0d55fc60a699ac956421775d5b0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710014"
---
In de [Azure-portal](https://portal.azure.com), klikt u op **resourcegroepen** > **cloud-shell-storage -\<your_region >**  >   **\<naam_opslagaccount >**.

![Cloud Shell-storage-account zoeken](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

In de **overzicht** pagina van het opslagaccount, selecteer **bestanden**.

Selecteer de automatisch gegenereerde bestandsshare en selecteer **uploaden**. Deze bestandsshare is gekoppeld in de Cloud Shell als `clouddrive`.

![Knop voor uploaden zoeken](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Klik op de kiezer bestand en selecteer het ZIP-bestand en klik vervolgens op **uploaden**. 

In de Cloud Shell gebruiken `ls` om te controleren of u dat het geüploade ZIP-bestand in de standaard zien kunt `clouddrive` delen.

```azurecli-interactive
ls clouddrive
```
