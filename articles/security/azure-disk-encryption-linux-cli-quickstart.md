---
title: Maak en Versleutel een Linux-VM met Azure CLI
description: In deze snelstartgids leert u hoe u Azure CLI gebruiken om te maken en een virtuele Linux-machine versleutelen
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: f9ad544f8710376b22d5cecf0161bcebc229f109
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081495"
---
# <a name="quickstart-create-and-encrypt-a-linux-vm-with-the-azure-cli"></a>Quickstart: Maak en Versleutel een Linux-VM met de Azure CLI

De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze Quick Start laat zien hoe de Azure CLI gebruiken om te maken en versleutelen van een Linux virtuele machine (VM).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.30 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een VM met [az vm create](/cli/azure/vm?view=azure-cli-latest#az-vm-create). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --size Standard_D2S_V3 \
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten. In het volgende voorbeeld van uitvoer ziet u dat het maken van de virtuele machine is geslaagd.

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Maak een Key Vault geconfigureerd voor versleutelingssleutels

Azure disk encryption slaat de versleutelingssleutel in een Azure Key Vault. Maak een Key Vault met [az keyvault maken](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create). Om in te schakelen de Key Vault voor het opslaan van versleutelingssleutels, gebruikt u de--ingeschakeld-voor--schijfversleuteling-parameter.

> [!Important]
> Elke Key Vault moet een unieke naam hebben. Het volgende voorbeeld wordt een Key Vault met de naam *myKV*, maar u moet de naam die iets anders.

```azurecli
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location eastus --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>De virtuele machine versleutelen

Versleutelen van uw virtuele machine met [az vm encryption](/cli/azure/vm/encryption?view=azure-cli-latest), bieden unieke Key Vault de naam van de--schijf-versleuteling-keyvault-parameter.

```azurecli-interactive
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

Na een moment dat die het proces wordt geretourneerd, ' de versleuteling aanvraag is geaccepteerd. Gebruik 'show'-opdracht om de voortgang te controleren. '. De opdracht 'show' [az vm show](/cli/azure/vm/encryption#az-vm-encryption-show).

```azurecli-interactive
az vm show --name MyVM -g MyResourceGroup
```

Als versleuteling is ingeschakeld, ziet u het volgende in de resulterende uitvoer:

```azurecli-interactive
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, kunt u de [az group delete](/cli/azure/group) opdracht om te verwijderen van de resourcegroep, de virtuele machine en de Key Vault. 

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een virtuele machine, een Sleutelkluis die is ingeschakeld voor versleutelingssleutels en de virtuele machine versleuteld gemaakt gemaakt.  Ga naar het volgende artikel voor meer informatie over de vereisten voor Azure Disk Encryption voor IaaS-VM's.

> [!div class="nextstepaction"]
> [Vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)