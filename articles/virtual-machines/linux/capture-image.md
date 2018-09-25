---
title: Een installatiekopie maken van een Linux-VM in Azure met behulp van Azure CLI | Microsoft Docs
description: Een installatiekopie maken van een Azure-VM moet worden gebruikt voor grootschalige implementaties met de Azure CLI.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2018
ms.author: cynthn
ms.openlocfilehash: 98d98c1337830ce54c7ff96c19812169be129584
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946813"
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Hoe u een installatiekopie van een virtuele machine of een VHD maken

<!-- generalize, image - extended version of the tutorial-->

Voor het maken van meerdere exemplaren van een virtuele machine (VM) te gebruiken in Azure, moet u een installatiekopie van de virtuele machine of VHD met het besturingssysteem vastleggen. Voor het maken van een afbeelding, moet u persoonlijke gegevens zodat u eenvoudig door meerdere keren implementeren kunt verwijderen. In de volgende stappen u inrichting ongedaan maken van een bestaande virtuele machine, toewijzing ongedaan maken en een installatiekopie maken. Deze afbeelding kunt u virtuele machines in een resourcegroep in uw abonnement maakt.

Als u wilt een kopie maken van uw bestaande Linux-VM voor back-up of foutopsporing, of een gespecialiseerde VHD met Linux vanuit een on-premises VM uploaden, Zie [uploaden en een Linux-VM maken van aangepaste installatiekopie](upload-vhd.md).  

U kunt ook **Packer** om uw aangepaste configuratie te maken. Zie voor meer informatie over het gebruik van Packer [hoe u Linux-installatiekopieën voor virtuele machines maken in Azure met Packer](build-image-with-packer.md).


## <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u voldoet aan de volgende vereisten:

* U moet een Azure-VM gemaakt in het Resource Manager-implementatiemodel met behulp van beheerde schijven. Als u een Linux-VM hebt gemaakt, kunt u de [portal](quick-create-portal.md), wordt de [Azure CLI](quick-create-cli.md), of [Resource Manager-sjablonen](create-ssh-secured-vm-from-template.md). De virtuele machine configureren indien nodig. Bijvoorbeeld, [gegevensschijven toevoegen](add-disk.md), toepassen van updates en toepassingen te installeren. 

* U moet ook de meest recente versie [Azure CLI](/cli/azure/install-az-cli2) geïnstalleerd en worden aangemeld bij een Azure-account met [az login](/cli/azure/reference-index#az_login).

## <a name="quick-commands"></a>Snelle opdrachten

Voor een vereenvoudigde versie van dit onderwerp, voor het testen, evalueren of hier meer over virtuele machines in Azure, Zie [maken van een aangepaste installatiekopie van een Azure-VM met behulp van de CLI](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>Stap 1: Inrichting van de virtuele machine ongedaan maken
De inrichting van de virtuele machine, met behulp van de Azure VM-agent verwijderen van machine-specifieke bestanden en gegevens. Gebruik de `waagent` opdracht met de *-inrichting + gebruiker* parameter op de bron-Linux-VM. Zie de [Gebruikershandleiding voor Azure Linux Agent](../extensions/agent-linux.md) voor meer informatie.

1. Verbinding maken met uw Linux-VM met behulp van een SSH-client.
2. Typ de volgende opdracht in het venster SSH:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Alleen deze opdracht uitvoeren op een virtuele machine die u van plan bent om vast te leggen als afbeelding. Er is geen garantie dat de installatiekopie van alle gevoelige informatie is uitgeschakeld of geschikt voor verdere distributie is. De *+ gebruiker* parameter verwijdert u ook de laatste ingerichte gebruikersaccount. Als u behouden accountreferenties in de virtuele machine wilt, gebruikt *-inrichting* account van de gebruiker intact laten.
 
3. Type **y** om door te gaan. U kunt toevoegen de **-forceren** parameter om te voorkomen dat deze bevestigingsstap.
4. Nadat de opdracht is voltooid, typt u **afsluiten**. Deze stap wordt de SSH-client gesloten.

## <a name="step-2-create-vm-image"></a>Stap 2: VM-installatiekopie maken
De Azure-CLI gebruiken voor de virtuele machine markeren als gegeneraliseerd en vastleggen van de installatiekopie. In de volgende voorbeelden kunt u voorbeeldnamen parameter vervangen door uw eigen waarden. Voorbeeld-parameternamen bevatten *myResourceGroup*, *myVnet*, en *myVM*.

1. Wijs de virtuele machine die u met de inrichting ongedaan gemaakt [az vm deallocate](/cli//azure/vm#deallocate). Het volgende voorbeeld wordt de virtuele machine met de naam de toewijzing ingetrokken *myVM* in de resourcegroep met de naam *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. De virtuele machine markeren als gegeneraliseerd met [az vm generalize](/cli//azure/vm#generalize). Het volgende voorbeeld wordt de virtuele machine met de naam *myVM* in de resourcegroep met de naam *myResourceGroup* als gegeneraliseerd:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Maak nu een afbeelding van de virtuele machine met [az afbeelding maken](/cli/azure/image#az_image_create). Het volgende voorbeeld wordt een installatiekopie met de naam *myImage* in de resourcegroep met de naam *myResourceGroup* met behulp van de VM-resource met de naam *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > De installatiekopie is gemaakt in dezelfde resourcegroep bevinden als de bron-VM. U kunt virtuele machines maken in elke willekeurige resourcegroep binnen uw abonnement van deze installatiekopie. U kunt desgewenst een specifieke resourcegroep voor uw VM-resources en afbeeldingen maken vanuit het oogpunt van beheer.
   >
   > Als u wilt voor het opslaan van uw installatiekopie in de zone-robuuste opslag, moet u deze maken in een regio die ondersteuning biedt voor [beschikbaarheidszones](../../availability-zones/az-overview.md) en bevatten de `--zone-resilient true` parameter.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Stap 3: Een virtuele machine maken van de vastgelegde installatiekopie
Maken van een virtuele machine met behulp van de installatiekopie die u hebt gemaakt met [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMDeployed* van de installatiekopie met de naam *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Het maken van de virtuele machine in een andere resourcegroep 

U kunt virtuele machines maken van een installatiekopie in elke willekeurige resourcegroep binnen uw abonnement. Voor het maken van een virtuele machine in een andere resourcegroep dan de installatiekopie, geef de volledige resource-ID aan de installatiekopie. Gebruik [az afbeeldingenlijst](/cli/azure/image#az_image_list) om een lijst met installatiekopieën weer te geven. De uitvoer lijkt op die in het volgende voorbeeld:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Het volgende voorbeeld wordt [az vm maken](/cli/azure/vm#az_vm_create) te maken van een virtuele machine in een andere resourcegroep dan de broninstallatiekopie door de resource-ID van de installatiekopie op te geven:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>Stap 4: De implementatie controleren

Nu SSH naar de virtuele machine die u hebt gemaakt om te controleren of de implementatie en maak gebruik van de nieuwe virtuele machine. Als u wilt verbinding maken via SSH, vinden de IP-adres of FQDN-naam van uw virtuele machine met [az vm show](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Volgende stappen
U kunt meerdere virtuele machines maken vanuit de bron-VM-installatiekopie. Als u moet wijzigingen aanbrengen in uw installatiekopie: 

- Een virtuele machine maken door uw installatiekopie.
- Controleer alle updates of wijzigingen in de configuratie.
- Volg de stappen opnieuw voor de inrichting, toewijzing ongedaan maken, generaliseren en een installatiekopie maken.
- Deze nieuwe installatiekopie gebruiken voor toekomstige implementaties. Indien gewenst, de oorspronkelijke installatiekopie verwijderen.

Zie voor meer informatie over het beheren van uw VM's met de CLI [Azure CLI](/cli/azure).
