---
title: Azure CLI sample script - ophalen van een groep beheerde bron en het formaat van virtuele machines | Microsoft Docs
description: Azure CLI-voorbeeldscript - ophalen van een groep beheerde bron en het formaat van virtuele machines
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: c78d2646471e40d60972cf91cb5bbd351f71a66c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Ophalen van resources in een groep beheerde bron en het formaat van VM's met Azure CLI

Dit script resources opgehaald uit een groep van beheerde bronnen en Hiermee wordt ingesteld voor de virtuele machines in die resourcegroep.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten om de beheerde toepassing te implementeren. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ managedapp lijst](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_list) | Beheerde toepassingen weergeven. Querywaarden om de resultaten richten opgeven. |
| [lijst met resources AZ](https://docs.microsoft.com/cli/azure/resource#az_resource_list) | Lijst met bronnen. Geef een bron groep en query waarden richt het resultaat. |
| [AZ vm-formaat wijzigen](https://docs.microsoft.com/cli/azure/vm#az_vm_resize) | Grootte van een virtuele machine bijwerken. |


## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](../overview.md).
* Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).
