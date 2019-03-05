---
title: Azure CLI-voorbeeldscript - werken met sleutelwaarden in een Azure-app-configuratiearchief | Microsoft Docs
description: Bevat informatie over het werken met sleutelwaarden in een Azure-app-configuratiearchief
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
ms.openlocfilehash: e69e2ca5ccd8e8edc2f55d74a0cca03eaabc9f49
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884242"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Werken met sleutelwaarden in een Azure-app-configuratiearchief

Met dit voorbeeldscript wordt een nieuwe sleutelwaarde in een Azure-app-configuratiearchief gemaakt, wordt een lijst met alle bestaande sleutelwaarden weergegeven en wordt de waarde van de zojuist gemaakte sleutel bijgewerkt en de laatste verwijderd.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli).

U moet eerst de CLI-extensie voor het Azure-app-configuratiearchief installeren door de volgende opdracht uit te voeren:

        az extension add -n appconfig

## <a name="sample-script"></a>Voorbeeldscript

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Dit script maakt gebruik van de volgende opdrachten om te werken met sleutelwaarden in een app-configuratiearchief. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az appconfig kv set](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-kv-set) | Hiermee wordt een sleutelwaarde gemaakt of bijgewerkt. |
| [az appconfig kv list](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-kv-list) | Hiermee worden sleutelwaarden in een app-configuratiearchief bijgewerkt. |
| [az appconfig kv delete](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-kv-delete) | Hiermee wordt een sleutelwaarde verwijderd. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van CLI-scripts voor een app-configuratiearchief vindt u in de [documentatie over Azure-app-configuratie](../cli-samples.md).
