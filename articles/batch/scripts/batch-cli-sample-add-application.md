---
title: Azure CLI Script voorbeeld - een toepassing toevoegen in een Batch | Microsoft Docs
description: Azure CLI Script voorbeeld - een toepassing toevoegen in Batch
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: cbfe8ab565ecf7f298a9a6c0f0c8298c675f178c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Toepassingen toevoegen aan Azure Batch met Azure CLI

Dit script laat zien hoe een toepassing voor gebruik met een Azure Batch-pool of taak instellen. Als u een toepassing instelt, het uitvoerbare bestand, samen met eventuele afhankelijkheden, in een ZIP-bestand van het pakket. In dit voorbeeld het uitvoerbare bestand zip-bestand wordt aangeroepen ' Mijn-toepassing-exe.zip'.

## <a name="prerequisites"></a>Vereisten

- Installeer de Azure CLI met behulp van de instructies in de [Azure CLI installatiehandleiding](https://docs.microsoft.com/cli/azure/install-azure-cli), als u dit nog niet hebt gedaan.
- Maak een Batch-account als u er nog geen hebt. Zie [een Batch-account maken met de Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) voor een voorbeeld van een script dat een account maakt.

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Opschonen van de toepassing

Nadat u het bovenstaande voorbeeld van een script uitgevoerd, voer de volgende opdrachten om de toepassing en alle bijbehorende geüploade toepassingspakketten te verwijderen.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Script uitleg

Dit script gebruikt de volgende opdrachten voor het maken van een toepassing en upload een toepassingspakket.
Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ batch-toepassing maken](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_create) | Een toepassing maakt.  |
| [AZ batch-toepassing instellen](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_set) | Eigenschappen van een toepassing van updates.  |
| [AZ batch-toepassingspakket maken](https://docs.microsoft.com/cli/azure/batch/application/package#az_batch_application_package_create) | Voegt een toepassingspakket toe aan de opgegeven toepassing.  |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Aanvullende Batch CLI scriptvoorbeelden vindt u in de [documentatie van Azure Batch CLI](../batch-cli-samples.md).
