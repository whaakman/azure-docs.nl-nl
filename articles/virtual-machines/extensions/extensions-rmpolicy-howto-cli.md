---
title: Azure Policy gebruiken voor het beperken van de installatie van de VM-extensie | Microsoft Docs
description: Azure Policy gebruiken voor het beperken van VM-extensie-implementaties.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: roiyz;cynthn
ms.openlocfilehash: 1f71276c25e3ec1e5791d9b35f89aa95190c6afd
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341953"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-linux-vms"></a>Azure Policy gebruiken voor het beperken van de installatie van de extensies op virtuele Linux-machines

Als u voorkomen dat het gebruik of de installatie van bepaalde uitbreidingen op uw virtuele Linux-machines wilt, kunt u een Azure-beleid met behulp van de CLI te beperken van extensies voor virtuele machines binnen een resourcegroep maken. 

In deze zelfstudie maakt gebruik van de CLI vanuit de Azure Cloud Shell, die voortdurend wordt bijgewerkt naar de nieuwste versie. Als u de Azure CLI lokaal uitvoert wilt, moet u versie 2.0.26 installeren of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="create-a-rules-file"></a>Maak een regelbestand

Als u wilt beperken welke extensies kunnen worden geïnstalleerd, moet u beschikken over een [regel](../../governance/policy/concepts/definition-structure.md#policy-rule) voor de logica voor het identificeren van de extensie.

In dit voorbeeld laat zien hoe weigeren installeren van extensies die zijn gepubliceerd door 'Microsoft.OSTCExtensions' door het maken van een bestand in Azure Cloud Shell, maar als u in de CLI lokaal werkt, kunt u ook een lokaal bestand gemaakt en vervangen door het pad naar het pad (~/clouddrive) het lokale bestand op uw computer.

In een [bash Cloud Shell](https://shell.azure.com/bash), type:

```azurecli-interactive 
vim ~/clouddrive/azurepolicy.rules.json
```

Kopieer en plak de volgende .json in het bestand.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.OSTCExtensions/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/publisher",
                "equals": "Microsoft.OSTCExtensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Wanneer u klaar bent, bereikt de **Esc** sleutel en typ vervolgens **: wq** opslaan en sluiten van het bestand.


## <a name="create-a-parameters-file"></a>Maak een parameterbestand

U moet ook een [parameters](../../governance/policy/concepts/definition-structure.md#parameters) -bestand dat wordt gemaakt van een structuur moet worden gebruikt voor het doorgeven in een lijst van de extensies blokkeren. 

Dit voorbeeld ziet u hoe u een parameterbestand maken voor virtuele Linux-machines in de Cloud Shell, maar als u in de CLI lokaal werkt, kunt u ook een lokaal bestand maken en het pad (~/clouddrive) vervangt door het pad naar het lokale bestand op uw computer.

In de [bash Cloud Shell](https://shell.azure.com/bash), type:

```azurecli-interactive
vim ~/clouddrive/azurepolicy.parameters.json
```

Kopieer en plak de volgende .json in het bestand.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied. Example: CustomScriptForLinux, VMAccessForLinux etc.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

Wanneer u klaar bent, bereikt de **Esc** sleutel en typ vervolgens **: wq** opslaan en sluiten van het bestand.

## <a name="create-the-policy"></a>Het beleid maken

Een beleidsdefinitie is een object dat wordt gebruikt voor het opslaan van de configuratie die u wilt gebruiken. De beleidsdefinitie maakt gebruik van de regels en de parameters-bestanden voor het definiëren van het beleid. Maakt de beleid definitie via [az beleidsdefinitie maken](/cli/azure/role/assignment?view=azure-cli-latest).

In dit voorbeeld worden de regels en de parameters de bestanden die u hebt gemaakt en opgeslagen als JSON-bestanden in uw cloudshell.

```azurecli-interactive
az policy definition create \
   --name 'not-allowed-vmextension-linux' \
   --display-name 'Block VM Extensions' \
   --description 'This policy governs which VM extensions that are blocked.' \
   --rules '~/clouddrive/azurepolicy.rules.json' \
   --params '~/clouddrive/azurepolicy.parameters.json' \
   --mode All
```


## <a name="assign-the-policy"></a>Het beleid toewijzen

In dit voorbeeld wordt het beleid voor toegewezen aan een resource-groep met [az beleidstoewijzing te maken](/cli/azure/policy/assignment). Een virtuele machine hebt gemaakt in de **myResourceGroup** resourcegroep niet mogelijk de Linux-VM-toegang of de Custom Script-extensies voor Linux installeren. De resourcegroep moet bestaan voordat u het beleid kunt toewijzen.

Gebruik [az account list](/cli/azure/account?view=azure-cli-latest) om op te halen van uw abonnements-ID in plaats van het certificaat in het voorbeeld.


```azurecli-interactive
az policy assignment create \
   --name 'not-allowed-vmextension-linux' \
   --scope /subscriptions/<subscription Id>/resourceGroups/myResourceGroup \
   --policy "not-allowed-vmextension-linux" \
   --params '{
        "notAllowedExtensions": {
            "value": [
                "VMAccessForLinux",
                "CustomScriptForLinux"
            ]
        }
    }'
```

## <a name="test-the-policy"></a>Het beleid testen

Test het beleid door het maken van een nieuwe virtuele machine en probeert toe te voegen een nieuwe gebruiker.


```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

Maak een nieuwe gebruiker met de naam **myNewUser** met de extensie voor VM-toegang.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --password 'mynewuserpwd123!'
```



## <a name="remove-the-assignment"></a>De toewijzing verwijderen

```azurecli-interactive
az policy assignment delete --name 'not-allowed-vmextension-linux' --resource-group myResourceGroup
```
## <a name="remove-the-policy"></a>Verwijder het beleid

```azurecli-interactive
az policy definition delete --name 'not-allowed-vmextension-linux'
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [Azure Policy](../../governance/policy/overview.md).