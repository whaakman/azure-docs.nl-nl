---
title: Azure CLI-voorbeeldscript - een Azure App Configuration-archief verwijderen | Microsoft Docs
description: Azure CLI-voorbeeldscript - een Azure App Configuration-archief verwijderen
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 3482cc14e73801af6d0db910ded84adf722bc6f4
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884216"
---
# <a name="delete-an-azure-app-configuration-store"></a>Een Azure App Configuration-archief verwijderen

Met dit voorbeeldscript verwijdert u een exemplaar van Azure App Configuration.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

U moet eerst de CLI-extensie voor het Azure App Configuration-archief installeren door de volgende opdracht uit te voeren:

        az extension add -n appconfig

## <a name="sample-script"></a>Voorbeeldscript

```azurecli-interactive
#/bin/bash

# Delete an app configuration store named myTestAppConfigStore from the Resource Group myResourceGroup
az appconfig delete --name myTestAppConfigStore --resource-group myResourceGroup
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten om een app-configuratiearchief te verwijderen. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az appconfig kv delete](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-delete) | Resource voor app-configuratiearchief verwijderen. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van CLI-scripts voor een App Configuration-archief vindt u in de [documentatie over Azure App-configuratie](../cli-samples.md).
