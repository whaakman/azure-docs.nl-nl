---
title: Maak een Linux-VM met een Azure-sjabloon met Azure CLI 1.0 | Microsoft Docs
description: Linux-VM in Azure met behulp van de Azure CLI 1.0 en een Azure Resource Manager-sjabloon maken.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 33d4aaa78fcdf3bd9e2e236606f2d3049f464a8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-vm-using-the-azure-cli-10-an-azure-resource-manager-template"></a>Het maken van een Azure Resource Manager-sjabloon voor een Linux-VM met behulp van de Azure CLI 1.0
In dit artikel laat zien hoe snel een virtuele Linux-Machine met behulp van de Azure CLI 1.0 en een Azure Resource Manager-sjabloon te implementeren. Het artikel schrijft het volgende als vereiste voor:

* een Azure-account ([probeer een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/))
* de [Azure CLI 1.0](../../cli-install-nodejs.md) aangemeld `azure login`.
* de Azure-CLI *moet in de* Azure Resource Manager-modus`azure config mode arm` staan.

U kunt een Linux-VM-sjabloon ook snel implementeren met behulp van de [Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#quick-command-summary) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](create-ssh-secured-vm-from-template.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

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
De volgende voorbeeldcode laat zien hoe u `azure group create` aanroept om een resourcegroep te maken en op hetzelfde moment een met SSH beveiligde virtuele Linux-machine te implementeren met behulp van [deze Azure Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Vergeet niet om voor de implementatie namen te gebruiken die uniek zijn voor uw omgeving. In dit voorbeeld wordt *myResourceGroup* als de naam van de resourcegroep, en *myVM* als de naam van de virtuele machine.

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

