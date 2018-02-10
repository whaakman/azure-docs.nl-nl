---
title: Een installatiekopie van een Linux VM in Azure met CLI 2.0 | Microsoft Docs
description: Een installatiekopie van een Azure-virtuele machine moet worden gebruikt voor grootschalige implementaties met de Azure CLI 2.0.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 3cbc25099b99499a6186e57c155d195e75bd61bf
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Het maken van een installatiekopie van een virtuele machine of de VHD

<!-- generalize, image - extended version of the tutorial-->

Voor het maken van meerdere exemplaren van een virtuele machine (VM) te gebruiken in Azure, een installatiekopie van de virtuele machine of de OS-VHD. Voor het maken van een installatiekopie, moet u persoonlijke gegevens die het maakt veiliger voor het implementeren van meerdere keren verwijderen. In de volgende stappen u inrichting ervan ongedaan maakt een bestaande virtuele machine, toewijzing en een installatiekopie maken. Deze installatiekopie kunt u virtuele machines maken voor elke willekeurige resourcegroep in uw abonnement.

Als u wilt maken van een kopie van uw bestaande Linux VM voor back-up of foutopsporing, of een gespecialiseerde Linux VHD uploaden van een lokale virtuele machine, Zie [uploaden en Linux-VM te maken van aangepaste schijfimage](upload-vhd.md).  

U kunt ook **verpakker** om uw aangepaste configuratie te maken. Zie voor meer informatie over het gebruik van verpakker [verpakker gebruiken om u te maken van installatiekopieën van Linux virtuele machines in Azure](build-image-with-packer.md).


## <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u voldoet aan de volgende vereisten:

* U moet een Azure VM gemaakt in het Resource Manager-implementatiemodel met beheerde schijven. Als u een Linux-VM nog niet hebt gemaakt, kunt u de [portal](quick-create-portal.md), wordt de [Azure CLI](quick-create-cli.md), of [Resource Manager-sjablonen](create-ssh-secured-vm-from-template.md). De virtuele machine naar wens configureren. Bijvoorbeeld: [gegevensschijven toevoegen](add-disk.md), toepassen van updates en toepassingen te installeren. 

* U moet bovendien beschikken over de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) geïnstalleerd en worden aangemeld bij een Azure-account met behulp van [az aanmelding](/cli/azure/#az_login).

## <a name="quick-commands"></a>Snelle opdrachten

Zie voor een vereenvoudigde versie van dit onderwerp voor het testen, evalueren of leren over virtuele machines in Azure, [maken van een aangepaste installatiekopie van een virtuele machine in Azure met behulp van de CLI](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>Stap 1: Inrichting ervan ongedaan maakt de virtuele machine
U inrichting ervan ongedaan maakt de virtuele machine met behulp van de Azure VM-agent machine specifieke bestanden en gegevens te verwijderen. Gebruik de `waagent` opdracht met de *-deprovision + user* parameter bij de bron-Linux-VM. Zie voor meer informatie de [gebruikershandleiding voor Azure Linux Agent](../windows/agent-user-guide.md).

1. Verbinding maken met uw Linux-VM met behulp van een SSH-client.
2. Typ de volgende opdracht in het venster SSH:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Deze opdracht alleen uitvoeren op een virtuele machine die u van plan bent om vast te leggen als afbeelding. Er is geen garantie dat de installatiekopie wordt gewist van alle gevoelige informatie of geschikt is voor de herdistributie. De *+ user* parameter verwijdert u ook de laatste ingerichte gebruikersaccount. Als u behouden accountreferenties in de virtuele machine wilt, gebruiken *-deprovision* moet het gebruikersaccount aanwezig blijven.
 
3. Type **y** om door te gaan. U kunt toevoegen de **-force** parameter om te voorkomen dat deze bevestigingsstap.
4. Nadat u de opdracht is voltooid, typt u **sluiten**. Deze stap wordt gesloten voor de SSH-client.

## <a name="step-2-create-vm-image"></a>Stap 2: Maak een VM-installatiekopie
De Azure CLI 2.0 gebruiken voor de virtuele machine niet markeren als gegeneraliseerd en vastleggen van de installatiekopie. In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter *myResourceGroup*, *myVnet*, en *myVM*.

1. Toewijzing van de virtuele machine die u gemaakt met [az vm ongedaan](/cli//azure/vm#deallocate). Het volgende voorbeeld de virtuele machine met de naam deallocates *myVM* in de resourcegroep met de naam *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. De virtuele machine niet markeren als gegeneraliseerd met [az vm generalize](/cli//azure/vm#generalize). Het volgende voorbeeld markeert de de virtuele machine met de naam *myVM* in de resourcegroep met de naam *myResourceGroup* zoals gegeneraliseerd:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Maak nu een afbeelding van de VM-resource met [az installatiekopie maken](/cli/azure/image#az_image_create). Het volgende voorbeeld wordt een installatiekopie met de naam *myImage* in de resourcegroep met de naam *myResourceGroup* met behulp van de VM-resource met de naam *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > De installatiekopie wordt gemaakt in dezelfde resourcegroep bevinden als de bron-VM. U kunt virtuele machines in elke willekeurige resourcegroep maken in uw abonnement vanuit deze installatiekopie. U kunt desgewenst een specifieke resourcegroep voor uw VM netwerkbronnen en installatiekopieën maken vanuit het oogpunt van beheer van.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Stap 3: Een virtuele machine van de vastgelegde installatiekopie maken
Een virtuele machine maken met de installatiekopie die u hebt gemaakt met [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVMDeployed* van de installatiekopie met de naam *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>De virtuele machine maken in een andere resourcegroep 

U kunt virtuele machines maken van een installatiekopie in elke willekeurige resourcegroep binnen uw abonnement. Voor het maken van een virtuele machine in een andere resourcegroep dan de installatiekopie, geef de volledige resource-ID aan de installatiekopie. Gebruik [az afbeeldingenlijst](/cli/azure/image#az_image_list) om een lijst met afbeeldingen weer te geven. De uitvoer lijkt op die in het volgende voorbeeld:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Het volgende voorbeeld wordt [az vm maken](/cli/azure/vm#az_vm_create) maken van een virtuele machine in een andere resourcegroep dan de broninstallatiekopie door te geven van de Afbeeldingsbron-ID:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>Stap 4: De implementatie controleren

Nu SSH aan de virtuele machine die u hebt gemaakt om te controleren of de implementatie en begin met behulp van de nieuwe virtuele machine. Als u wilt verbinding maken via SSH, vinden de IP-adres of FQDN van uw virtuele machine met [az vm weergeven](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Volgende stappen
U kunt meerdere virtuele machines maken van de bron-VM-installatiekopie. Als u wijzigingen aanbrengen in uw installatiekopie moet: 

- Een virtuele machine maken van uw installatiekopie.
- Controleer op updates of wijzigingen in de configuratie.
- Volg de stappen weer voor inrichting ervan ongedaan maakt, toewijzing generaliseren en een installatiekopie maken.
- Deze nieuwe installatiekopie gebruiken voor toekomstige implementaties. Indien gewenst, verwijdert u de originele installatiekopie.

Zie voor meer informatie over het beheren van uw virtuele machines met de CLI [Azure CLI 2.0](/cli/azure/overview).
