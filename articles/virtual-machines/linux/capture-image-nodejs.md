---
title: Vastleggen van een Azure Linux VM te gebruiken als sjabloon | Microsoft Docs
description: Informatie over het vastleggen en een installatiekopie van een Linux-gebaseerde Azure virtuele machine (VM) gemaakt met het Azure Resource Manager-implementatiemodel generalize.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: b1164fbd816eea5189786850f096438e32f8f802
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Vastleggen van een virtuele Linux-machine uitgevoerd op Azure
Volg de stappen in dit artikel generaliseren en vastleggen van uw Azure Linux virtuele machine (VM) in het Resource Manager-implementatiemodel. Wanneer u de virtuele machine generalize, kunt u persoonlijke gegevens te verwijderen en voorbereiden van de virtuele machine moet worden gebruikt als een afbeelding. U vervolgens een installatiekopie van een gegeneraliseerde virtuele harde schijf (VHD) voor het besturingssysteem, virtuele harde schijven voor bijgesloten gegevensschijven, vastleggen en een [Resource Manager-sjabloon](../../azure-resource-manager/resource-group-overview.md) voor nieuwe VM-implementaties. Dit artikel wordt uitgelegd hoe u een VM-installatiekopie met de Azure CLI 1.0 vastleggen voor een virtuele machine met niet-beheerde schijven. U kunt ook [vastleggen van een VM die gebruikmaakt van Azure beheerd schijven met de Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Beheerde schijven worden verwerkt door de Azure-platform en hoeven niet de voorbereidings- of locatie om op te slaan. Zie [Azure Managed Disks overview](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Overzicht van Azure Managed Disks) voor meer informatie. 

Netwerkbronnen voor elke nieuwe virtuele machine instellen voor het maken van virtuele machines met behulp van de installatiekopie en de sjabloon (een JavaScript Object Notation of JSON,-bestand) gebruiken voor het implementeren van de vastgelegde VHD-installatiekopieën. Op deze manier kunt u een virtuele machine met de huidige softwareconfiguratie, worden dezelfde manier als die u installatiekopieën in Azure Marketplace gebruiken repliceren.

> [!TIP]
> Als u wilt maken van een kopie van uw bestaande Linux-VM met de speciale staat voor de back-up of foutopsporing, Zie [een kopie maken van een virtuele Linux-machine uitgevoerd op Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). En als u een Linux VHD uploaden van een lokale virtuele machine wilt, Zie [uploaden en Linux-VM te maken van aangepaste schijfimage](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#before-you-begin) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel

## <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u voldoet aan de volgende vereisten:

* **Azure virtuele machine gemaakt in het Resource Manager-implementatiemodel** -als u een Linux-VM nog niet hebt gemaakt, kunt u de [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), wordt de [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), of [Resource Manager-sjablonen](create-ssh-secured-vm-from-template.md). 
  
    De virtuele machine naar wens configureren. Bijvoorbeeld: [gegevensschijven toevoegen](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), toepassen van updates en toepassingen te installeren. 
* **Azure CLI** -installeren van de [Azure CLI](../../cli-install-nodejs.md) op een lokale computer.

## <a name="step-1-remove-the-azure-linux-agent"></a>Stap 1: De Azure Linux-agent verwijderen
Voer eerst de **waagent** opdracht met de **deprovision** parameter voor de Linux-VM. Deze opdracht verwijdert u bestanden en gegevens om de virtuele machine gereed is voor het generaliseren. Zie voor meer informatie de [gebruikershandleiding voor Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Verbinding maken met uw Linux-VM met behulp van een SSH-client.
2. Typ de volgende opdracht in het venster SSH:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Deze opdracht alleen uitvoeren op een virtuele machine die u van plan bent om vast te leggen als afbeelding. Er is geen garantie dat de installatiekopie wordt gewist van alle gevoelige informatie of geschikt is voor de herdistributie.
 
3. Type **y** om door te gaan. U kunt toevoegen de **-force** parameter om te voorkomen dat deze bevestigingsstap.
4. Nadat u de opdracht is voltooid, typt u **sluiten**. Deze stap wordt gesloten voor de SSH-client.

## <a name="step-2-capture-the-vm"></a>Stap 2: De virtuele machine vastleggen
Gebruik de Azure CLI generaliseren en vastleggen van de virtuele machine. In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. De namen van de voorbeeld-parameter **myResourceGroup**, **myVnet**, en **myVM**.

1. Open in uw lokale computer, de Azure CLI en [Meld u aan bij uw Azure-abonnement](../../xplat-cli-connect.md). 
2. Zorg ervoor dat u in de modus Resource Manager.
   
    ```azurecli
    azure config mode arm
    ```
3. Sluit de virtuele machine die u al gemaakt met behulp van de volgende opdracht:
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. Generaliseer de virtuele machine met de volgende opdracht:
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. Voer nu de **azure vm vastleggen** opdracht, waarmee de virtuele machine worden vastgelegd. In het volgende voorbeeld wordt de installatiekopie van het vastleggen van VHD's met namen die begint met **MyVHDNamePrefix**, en de **-t** optie geeft u een pad naar de sjabloon **MyTemplate.json**. 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > De VHD-bestanden van de installatiekopie gemaakt in hetzelfde opslagaccount die de oorspronkelijke VM gebruikt standaard. Gebruik de *hetzelfde opslagaccount* voor het opslaan van de VHD's voor nieuwe VM's u van de installatiekopie maken. 

6. Als u wilt zoeken naar de locatie van een vastgelegde installatiekopie, opent u het JSON-sjabloon in een teksteditor. In de **storageProfile**, vinden de **uri** van de **installatiekopie** zich in de **system** container. Bijvoorbeeld: de URI van de installatiekopie van de OS-schijf is vergelijkbaar met`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Stap 3: Een virtuele machine van de vastgelegde installatiekopie maken
De afbeelding met een sjabloon nu gebruiken voor het maken van een Linux-VM. Deze stappen ziet u hoe u de Azure CLI en het JSON-bestand-sjabloon die u hebt vastgelegd voor het maken van de virtuele machine in een nieuw virtueel netwerk.

### <a name="create-network-resources"></a>Maken van netwerkbronnen
De sjabloon wilt gebruiken, moet u eerst een virtueel netwerk en NIC instellen voor uw nieuwe virtuele machine. Het is raadzaam om dat het maken van een resourcegroep voor deze bronnen op de locatie waar uw VM-installatiekopie is opgeslagen. De opdrachten uitvoeren is vergelijkbaar met de volgende, waarbij namen voor uw resources en juiste Azure locatie ('centralus' in deze opdrachten):

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-the-id-of-the-nic"></a>De Id van de NIC verkrijgen
Voor het implementeren van een virtuele machine van de installatiekopie met behulp van de JSON die u hebt opgeslagen tijdens het vastleggen, moet u de Id van de NIC. U ontvangt deze met de volgende opdracht:

```azurecli
azure network nic show myResourceGroup1 myNIC
```

De **Id** in de uitvoer is vergelijkbaar met`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`

### <a name="create-a-vm"></a>Een virtuele machine maken
Voer nu de volgende opdracht voor het maken van uw virtuele machine van de vastgelegde installatiekopie van de virtuele machine. Gebruik de **-f** parameter om het pad naar het sjabloon JSON-bestand die u hebt opgeslagen.

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

In de opdrachtuitvoer wordt u gevraagd om op te geven van een nieuwe VM-naam, de admin-gebruikersnaam en wachtwoord en de Id van de NIC die u eerder hebt gemaakt.

```bash
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

Het volgende voorbeeld ziet u wat u voor een geslaagde implementatie zien:

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment to complete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-the-deployment"></a>De implementatie controleren
Nu SSH aan de virtuele machine die u hebt gemaakt om te controleren of de implementatie en begin met behulp van de nieuwe virtuele machine. Als u wilt verbinding maken via SSH, vinden de IP-adres van de virtuele machine die u hebt gemaakt met de volgende opdracht uit te voeren:

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

Het openbare IP-adres wordt vermeld in de opdrachtuitvoer. Standaard verbinding u met de Linux-VM met SSH op poort 22.

## <a name="create-additional-vms"></a>Aanvullende virtuele machines maken
Gebruik de vastgelegde installatiekopie en de sjabloon voor het implementeren van extra virtuele machines met de stappen in de vorige sectie. Andere opties voor het maken van virtuele machines van de installatiekopie met behulp van een sjabloon Quick Start of uitgevoerd zijn de **azure vm maken** opdracht.

### <a name="use-the-captured-template"></a>De vastgelegde sjabloon gebruiken
Als u de vastgelegde installatiekopie en de sjabloon, volgt (gespecificeerd in de vorige sectie):

* Zorg ervoor dat uw VM-installatiekopie in hetzelfde opslagaccount die als host fungeert voor uw VM VHD.
* Kopieer het JSON-bestand van het sjabloon en geef een unieke naam voor de besturingssysteemschijf van de nieuwe VM VHD (of VHD's). Bijvoorbeeld, in de **storageProfile**onder **vhd**in **uri**, Geef een unieke naam voor de **osDisk** VHD, vergelijkbaar met`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Maak een NIC in dezelfde of een ander virtueel netwerk.
* Maak een implementatie met de gewijzigde sjabloon JSON-bestand in de resourcegroep waarin u het virtuele netwerk hebt ingesteld.

### <a name="use-a-quickstart-template"></a>Een Quick Start-sjabloon gebruiken
Als u het instellen van het automatisch wanneer u een virtuele machine van de installatiekopie van het maakt netwerk wilt, kunt u deze resources in een sjabloon. Zie bijvoorbeeld de [101-vm-van-gebruiker-image sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) vanuit GitHub. Deze sjabloon maakt een virtuele machine van uw aangepaste installatiekopie en de benodigde virtueel netwerk, openbare IP-adres en NIC-bronnen. Zie voor een overzicht van het gebruik van de sjabloon in de Azure portal [maken van een virtuele machine uit een aangepaste installatiekopie met Resource Manager-sjabloon](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Gebruik de azure vm-opdracht maken
Meestal is het eenvoudigste Resource Manager-sjabloon gebruiken om te maken van een virtuele machine uit de afbeelding. U kunt echter de virtuele machine maken *imperatively* met behulp van de **azure vm maken** opdracht met de **-Q** (**--installatiekopie urn**) parameter. Als u deze methode gebruikt, geeft u ook de **-d** (**--os-schijf-vhd**) parameter om de locatie van het besturingssysteem-VHD-bestand voor de nieuwe virtuele machine. Dit bestand moet zich in de container VHD's van het opslagaccount waar het VHD-bestand wordt opgeslagen. De opdracht kopieert u de VHD voor de nieuwe virtuele machine automatisch omgeleid naar de **VHD's** container.

Voordat u **azure vm maken** met de installatiekopie, de volgende stappen uitvoeren:

1. Een resourcegroep maken of een bestaande resourcegroep voor de implementatie identificeren.
2. De bron van een openbare IP-adres en een NIC-resource maken voor de nieuwe virtuele machine. Zie voor stappen voor het maken van een virtueel netwerk, het openbare IP-adres en de NIC met behulp van de CLI, eerder in dit artikel. (**azure vm maken** ook een NIC kunt maken, maar u moet extra parameters voor een virtueel netwerk en subnet doorgeven.)

Voer vervolgens een opdracht die wordt doorgegeven URI's op het nieuwe besturingssysteem-VHD-bestand en de bestaande installatiekopie. In dit voorbeeld wordt een grootte Standard_A1 VM wordt gemaakt in de regio VS-Oost.

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

Voor extra opdrachtopties, voert u `azure help vm create`.

## <a name="next-steps"></a>Volgende stappen
Zie de taken in voor het beheren van uw virtuele machines met de CLI [implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en de Azure CLI](create-ssh-secured-vm-from-template.md).

