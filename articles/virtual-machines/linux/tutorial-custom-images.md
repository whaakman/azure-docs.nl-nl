---
title: "Maken van aangepaste installatiekopieën voor virtuele machine met de Azure CLI | Microsoft Docs"
description: Zelfstudie - maken van een aangepaste VM-installatiekopie met de Azure CLI.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e73494ff4827b74cbb42b2b0f1f9738c78960e23
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Een aangepaste installatiekopie van een virtuele machine met behulp van de CLI van Azure maken

Aangepaste installatiekopieën zijn zoals marketplace-installatiekopieën, maar u deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt voor de bootstrap configuraties, zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere configuraties OS. In deze zelfstudie maakt u uw eigen aangepaste installatiekopie van een virtuele machine van Azure. Procedures voor:

> [!div class="checklist"]
> * Inrichting ervan ongedaan en generalize van virtuele machines
> * Een aangepaste installatiekopie maken
> * Een virtuele machine van een aangepaste installatiekopie maken
> * Lijst van alle installatiekopieën in uw abonnement
> * Een afbeelding verwijderen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Voordat u begint

De onderstaande stappen worden in detail beschreven hoe moet worden overgenomen van een bestaande virtuele machine en schakelt u deze in een herbruikbare aangepaste installatiekopie die u gebruiken kunt om nieuwe VM-exemplaren te maken.

Als u het voorbeeld in deze zelfstudie, moet u een bestaande virtuele machine hebben. Indien nodig, dit [voorbeeldscript](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) kunt maken voor u. Wanneer het uitvoeren van de zelfstudie vervangt benoemt de resourcegroep en de virtuele machine waar nodig.

## <a name="create-a-custom-image"></a>Een aangepaste installatiekopie maken

Voor het maken van een installatiekopie van een virtuele machine, moet u de virtuele machine voorbereiden door opheffen van inrichting, toewijzing en vervolgens de bron-VM als gegeneraliseerd markeren. Zodra de VM is voorbereid, kunt u een installatiekopie maken.

### <a name="deprovision-the-vm"></a>De virtuele machine identiteitsgegevens 

Opheffen van inrichting de virtuele machine generaliseert door machine-specifieke informatie te verwijderen. Deze generaliseren maakt het mogelijk om te veel virtuele machines van één installatiekopie implementeren. Tijdens het opheffen van inrichting, de naam van de host opnieuw is ingesteld op *localhost.localdomain*. Host-SSH-sleutels, naamserver configuraties hoofdwachtwoord en in de cache DHCP-leases worden ook verwijderd.

Voor de inrichting ervan ongedaan maakt de virtuele machine, gebruikt u de Azure VM-agent (waagent). De Azure VM-agent is geïnstalleerd op de virtuele machine en inrichting en interactie met de Azure-Infrastructuurcontroller beheert. Zie voor meer informatie de [gebruikershandleiding voor Azure Linux Agent](agent-user-guide.md).

Verbinding maken met uw virtuele machine via SSH en voer de opdracht voor de inrichting ervan ongedaan maakt de virtuele machine. Met de `+user` argument, de laatste ingerichte gebruiker-account en alle bijbehorende gegevens worden ook verwijderd. De voorbeeld-IP-adres vervangen door het openbare IP-adres van uw virtuele machine.

SSH naar de virtuele machine.
```bash
ssh azureuser@52.174.34.95
```
Inrichting ervan ongedaan maakt de virtuele machine.

```bash
sudo waagent -deprovision+user -force
```
De SSH-sessie te sluiten.

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Toewijzing ongedaan maken en de virtuele machine niet markeren als gegeneraliseerd

Voor het maken van een installatiekopie, moet de VM ongedaan. Het gebruik van de VM ongedaan [az vm ongedaan](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Tot slot stelt de status van de virtuele machine als gegeneraliseerd met [az vm generalize](/cli//azure/vm#generalize) zodat de Azure-platform weet de VM is gegeneraliseerd. U kunt alleen een installatiekopie van het maken van een gegeneraliseerde virtuele machine.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>De installatiekopie maken

Nu u een installatiekopie van de virtuele machine maken met behulp van kunt [az installatiekopie maken](/cli//azure/image#create). Het volgende voorbeeld wordt een installatiekopie met de naam *myImage* van een virtuele machine met de naam *myVM*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Virtuele machines van de installatiekopie maken

Nu dat u een installatiekopie hebt, kunt u een of meer nieuwe virtuele machines van de installatiekopie met behulp [az vm maken](/cli/azure/vm#create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMfromImage* van de installatiekopie met de naam *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Beheer van installatiekopieën 

Hier volgen enkele voorbeelden van algemene beheertaken voor de installatiekopie en hoe ze met de Azure CLI te voltooien.

Lijst van alle installatiekopieën met de naam in een tabel.

```azurecli-interactive 
az image list \
    --resource-group myResourceGroup
```

Een afbeelding verwijderen. Dit voorbeeld wordt verwijderd van de installatiekopie met de naam *myOldImage* van de *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een aangepaste installatiekopie van de virtuele machine gemaakt. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Inrichting ervan ongedaan en generalize van virtuele machines
> * Een aangepaste installatiekopie maken
> * Een virtuele machine van een aangepaste installatiekopie maken
> * Lijst van alle installatiekopieën in uw abonnement
> * Een afbeelding verwijderen

Ga naar de volgende zelfstudie voor meer informatie over de maximaal beschikbare virtuele machines.

> [!div class="nextstepaction"]
> [Maximaal beschikbare virtuele machines maken](tutorial-availability-sets.md).

