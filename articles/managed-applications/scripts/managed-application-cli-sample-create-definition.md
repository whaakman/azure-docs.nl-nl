---
title: Azure CLI-script steekproef - definitie van een beheerde maken | Microsoft Docs
description: 'Azure CLI-script voorbeeld: de definitie van een beheerde toepassing maken'
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
ms.openlocfilehash: 6cfc51e5787ad6ab100638d0965b69232cda070a
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>De definitie van een beheerde toepassing maken met Azure CLI

Dit script wordt de definitie van een beheerde toepassing publiceert naar een Servicecatalogus. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdracht om de definitie van de beheerde toepassing te maken. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [AZ managedapp definitie maken](https://docs.microsoft.com/cli/azure/managedapp/definition#az_managedapp_definition_create) | De definitie van een beheerde toepassing maken. Geef het pakket met de vereiste bestanden. |


## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](../overview.md).
* Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).
