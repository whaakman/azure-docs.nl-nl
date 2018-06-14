---
title: Azure-beleid gebruiken voor het beperken van de installatie van de VM-extensie | Microsoft Docs
description: Azure-beleid gebruiken om te beperken van VM-extensie-implementaties.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: danis;cynthn
ms.openlocfilehash: 8e65b82730884947633688db9ed50080b96e0b8e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
ms.locfileid: "30248154"
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-linux-vms"></a>Azure-beleid gebruiken voor het beperken van de installatie van de extensies op de virtuele Linux-machines

Als u voorkomen dat het gebruik of de installatie van bepaalde extensies op uw virtuele Linux-machines wilt, kunt u een Azure-beleid met behulp van de CLI om te beperken van uitbreidingen voor virtuele machines binnen een resourcegroep maken. 

Deze zelfstudie maakt gebruik van de CLI vanuit de Azure Cloud-Shell, die voortdurend wordt bijgewerkt naar de nieuwste versie. Als u de Azure CLI lokaal uitvoeren wilt, moet u versie 2.0.26 installeren of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-rules-file"></a>Maak een regelbestand

Als u wilt beperken welke uitbreidingen kunnen worden geïnstalleerd, moet u beschikken over een [regel](/azure/azure-policy/policy-definition#policy-rule) voor de logica voor het identificeren van de extensie.

Dit voorbeeld ziet u hoe u kunt weigeren met het installeren van extensies die zijn gepubliceerd door 'Microsoft.OSTCExtensions' door het maken van een regelbestand in de Azure-Cloud-Shell, maar als u in de CLI lokaal werkt, u kunt ook een lokaal bestand maken en het pad (~/clouddrive) vervangen door het pad naar het lokale bestand op uw computer.

In een [bash-Shell Cloud](https://shell.azure.com/bash), type:

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

Wanneer u klaar bent, klik op de **Esc** sleutel en typ vervolgens **: wq** wilt opslaan en sluiten van het bestand.


## <a name="create-a-parameters-file"></a>Maak een parameterbestand

U moet ook een [parameters](/azure/azure-policy/policy-definition#parameters) -bestand dat wordt gemaakt van een structuur moet worden gebruikt voor het doorgeven in een lijst van de extensies te blokkeren. 

Dit voorbeeld ziet u een parameterbestand maken voor virtuele Linux-machines in de Cloud-Shell, maar als u in de CLI lokaal werkt, u kunt ook een lokaal bestand maken en het pad (~/clouddrive) vervangen door het pad naar het lokale bestand op uw computer.

In de [bash-Shell Cloud](https://shell.azure.com/bash), type:

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

Wanneer u klaar bent, klik op de **Esc** sleutel en typ vervolgens **: wq** wilt opslaan en sluiten van het bestand.

## <a name="create-the-policy"></a>Het beleid maken

Een beleidsdefinitie is een object dat wordt gebruikt voor het opslaan van de configuratie die u wilt gebruiken. De beleidsdefinitie gebruikt de regels en de parameters-bestanden voor het definiëren van het beleid. Maak beleid definitie met [az beleidsdefinitie maken](/cli/azure/role/assignment?view=azure-cli-latest#az_role_assignment_create).

In dit voorbeeld zijn de regels en parameters bestanden die u hebt gemaakt en opgeslagen als .json-bestanden in uw cloud-shell.

```azurecli-interactive
az policy definition create \
   --name 'not-allowed-vmextension-linux' \
   --display-name 'Block VM Extensions' \
   --description 'This policy governs which VM extensions that are blocked.' \
   --rules '~/clouddrive/azurepolicy.rules.json' \
   --params '~/clouddrive/azurepolicy.parameters.json' \
   --mode All
```


## <a name="assign-the-policy"></a>Wijs het beleid

In dit voorbeeld wordt het beleid toegewezen aan een resource-groep met [az beleidstoewijzing maken](/cli/azure/policy/assignment#az_policy_assignment_create). Een virtuele machine gemaakt de **myResourceGroup** resourcegroep kan niet worden de Linux VM-toegang of de aangepaste scriptextensies voor Linux installeren. De resourcegroep moet bestaan voordat u het beleid kunt toewijzen.

Gebruik [az Accountlijst](/cli/azure/account?view=azure-cli-latest#az_account_list) ophalen van uw abonnements-ID in plaats van het certificaat in het voorbeeld.


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

Het beleid testen door een nieuwe virtuele machine maken en bij het toevoegen van een nieuwe gebruiker.


```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

Maak een nieuwe gebruiker met de naam **myNewUser** met de toegang van de VM-extensie.

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
## <a name="remove-the-policy"></a>Het beleid verwijderen

```azurecli-interactive
az policy definition delete --name 'not-allowed-vmextension-linux'
```


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [Azure beleid](../../azure-policy/azure-policy-introduction.md).
