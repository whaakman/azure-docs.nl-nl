---
title: Een virtuele Linux-machine maken met een Azure-sjabloon | Microsoft Docs
description: Een virtuele Linux-machine in Azure maken met behulp van een Azure Resource Manager-sjabloon.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: v-livech
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fa842efd99718be7fa9eaf8aac8030c32cbceeec
ms.openlocfilehash: bd0c9fd0044ce8a5de9004c68b1267b8b66df80a
ms.lasthandoff: 03/01/2017


---
# <a name="how-to-create-a-linux-vm-using-an-azure-resourec-manager-template"></a>Een virtuele Linux-machine maken met behulp van een Azure Resource Manager-sjabloon
Dit artikel laat zien hoe u in Azure snel een virtuele Linux-machine kunt implementeren met behulp van een Azure-sjabloon.  Het artikel schrijft het volgende als vereiste voor:

* een Azure-account ([probeer een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure-CLI](../xplat-cli-install.md) die is aangemeld bij `azure login`.
* de Azure-CLI *moet in de *Azure Resource Manager-modus`azure config mode arm` staan.

U kunt een Linux-VM-sjabloon ook snel implementeren met behulp van de [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Beknopt overzicht van opdrachten
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Gedetailleerd overzicht
Met sjablonen kunt u in Azure virtuele machines maken met instellingen die u tijdens het starten wilt aanpassen, zoals gebruikersnamen en hostnamen. Voor dit artikel starten we een Azure-sjabloon waarbij we gebruikmaken van een Ubuntu VM en een netwerkbeveiligingsgroep (NSG) waarbij poort 22 is geopend voor SSH .

Azure Resource Manager-sjablonen zijn JSON-bestanden die kunnen worden gebruikt voor eenvoudige eenmalige taken, zoals het starten van een Ubuntu VM zoals in dit artikel wordt gedaan.  Azure-sjablonen kunnen ook worden gebruikt om complexe Azure-configuraties te maken van volledige omgevingen, zoals een test-, ontwikkelings- of productie-implementatiestack.

## <a name="create-the-linux-vm"></a>De virtuele Linux-machine maken
De volgende voorbeeldcode laat zien hoe u `azure group create` aanroept om een resourcegroep te maken en op hetzelfde moment een met SSH beveiligde virtuele Linux-machine te implementeren met behulp van [deze Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Vergeet niet om voor de implementatie namen te gebruiken die uniek zijn voor uw omgeving. In dit voorbeeld wordt `myResourceGroup` als de naam van de resourcegroep gebruikt en `myVM` als de naam van de virtuele machine.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Uw uitvoer moet eruitzien als in het volgende uitvoerblok:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

In dat voorbeeld werd een virtuele machine geïmplementeerd met de parameter `--template-uri`.  U kunt ook een sjabloon downloaden of lokaal een sjabloon maken en deze sjabloon vervolgens doorgeven met behulp van de parameter `--template-file`. Hierbij neemt u het pad naar het sjabloonbestand op als argument. De Azure CLI vraagt u om de parameters die vereist zijn voor de sjabloon.

## <a name="next-steps"></a>Volgende stappen
Doorzoek de [Sjablonengalerie](https://azure.microsoft.com/documentation/templates/) om te bekijken welke app-frameworks u hierna wilt gaan implementeren.


