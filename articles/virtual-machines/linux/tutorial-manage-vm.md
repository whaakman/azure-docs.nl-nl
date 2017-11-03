---
title: Maken en beheren van virtuele Linux-machines met de Azure CLI | Microsoft Docs
description: Zelfstudie - maken en beheren van virtuele Linux-machines met de Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: bef7f6ef13f6d31c16d40deb46f168ae52a9e61b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-linux-vms-with-the-azure-cli"></a>Maken en beheren van virtuele Linux-machines met de Azure CLI

Virtuele machines van Azure bieden een volledig worden geconfigureerd en flexibele computeromgeving. Deze zelfstudie bevat informatie over basic virtuele machine van Azure-implementatie items zoals het selecteren van een VM-grootte, een VM-installatiekopie te selecteren en implementeren van een virtuele machine. Procedures voor:

> [!div class="checklist"]
> * Maken en verbinding maken met een virtuele machine
> * Selecteer en gebruik van VM-installatiekopieën
> * Weergeven en gebruiken van specifieke VM-grootten
> * De grootte van een virtuele machine wijzigen
> * Bekijken en te begrijpen status virtuele machine


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep moet worden gemaakt voordat een virtuele machine. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* wordt gemaakt in de *eastus* regio. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

De resourcegroep is opgegeven bij het maken of wijzigen van een virtuele machine die in deze zelfstudie kan worden gezien.

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een virtuele machine met de [az vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) opdracht. 

Wanneer u een virtuele machine maakt, er zijn diverse opties beschikbaar zoals besturingssysteemkopie schijf sizing en administratieve referenties. In dit voorbeeld wordt een virtuele machine gemaakt met de naam *myVM* Ubuntu Server uitgevoerd. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

Het duurt enkele minuten voor het maken van de virtuele machine. Wanneer de virtuele machine is gemaakt, levert de Azure CLI informatie over de virtuele machine. Noteer de `publicIpAddress`, dit adres kan worden gebruikt voor toegang tot de virtuele machine... 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-to-vm"></a>Verbinding maken met virtuele machine

U kunt nu verbinding met de virtuele machine met SSH in de Azure-Cloud-Shell of vanaf uw lokale computer. Vervang het voorbeeld IP-adres met de `publicIpAddress` in de vorige stap hebt genoteerd.

```bash
ssh 52.174.34.95
```

Zodra u aangemeld bij de virtuele machine, kunt u deze kunt installeren en configureren van toepassingen. Wanneer u klaar bent, sluit u de SSH-sessie als normale:

```bash
exit
```

## <a name="understand-vm-images"></a>VM-installatiekopieën begrijpen

De Azure marketplace bevat veel afbeeldingen die kunnen worden gebruikt voor het maken van virtuele machines. Een virtuele machine is gemaakt met behulp van een installatiekopie van een virtuele Ubuntu in de vorige stappen. In deze stap wordt de Azure CLI gebruikt voor het zoeken van de marketplace voor een installatiekopie CentOS, die vervolgens wordt gebruikt voor het implementeren van een tweede virtuele machine.  

Als een lijst van de meest gebruikte afbeeldingen wilt weergeven, gebruikt de [az vm afbeeldingenlijst](/cli/azure/vm/image#list) opdracht.

```azurecli-interactive 
az vm image list --output table
```

Uitvoer van de opdracht retourneert de meest populaire VM-installatiekopieën in Azure.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

Een volledige lijst ziet door toe te voegen de `--all` argument. De lijst met afbeeldingen kan ook worden gefilterd door `--publisher` of `–-offer`. In dit voorbeeld wordt de lijst gefilterd voor alle afbeeldingen met een aanbieding die overeenkomt met *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Gedeeltelijke uitvoer:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

Voor het implementeren van een virtuele machine met de installatiekopie van een specifieke Noteer de waarde in de *Urn* kolom. Bij het opgeven van de installatiekopie van kan het versienummer van de installatiekopie worden vervangen door 'nieuwste', die de meest recente versie van de verdeling selecteert. In dit voorbeeld wordt de `--image` argument wordt gebruikt om de nieuwste versie van de installatiekopie van een CentOS 6.5 geven.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>VM-grootten begrijpen

De grootte van een virtuele machine bepaalt de hoeveelheid compute-bronnen zoals CPU en GPU geheugen die beschikbaar zijn gesteld voor de virtuele machine. Virtuele machines moet het juiste formaat voor de verwachte werkbelasting. Als de werkbelasting toeneemt, kan een bestaande virtuele machine kan worden gewijzigd.

### <a name="vm-sizes"></a>VM-grootten

De volgende tabel categoriseert grootten in gebruiksvoorbeelden.  

| Type                     | Grootten           |    Beschrijving       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7| Taakverdeling CPU-naar-geheugen. Ideaal voor dev / testen en in kleine tot middelgrote oplossingen voor toepassingen en gegevens.  |
| [Geoptimaliseerde rekenkracht](sizes-compute.md)   | FS, F             | Hoog CPU-naar-geheugen. Goede voor gemiddeld verkeer toepassingen, netwerkapparatuur en batchprocessen.        |
| [Geoptimaliseerd geheugen](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Hoge geheugen-naar-core. Ideaal voor relationele databases, middelgrote tot grote caches en in het geheugen analytics.                 |
| [Geoptimaliseerde opslag](../virtual-machines-windows-sizes-storage.md)      | Ls                | Snelle doorvoer van schijfgegevens en IO. Ideaal voor big data-, SQL- en NoSQL-databases.                                                         |
| [GPU](sizes-gpu.md)          | NV, NC            | Gespecialiseerde VMs gericht voor zware grafische weergave en het bewerken van video's.       |
| [Hoge prestaties](sizes-hpc.md) | H, A8 11          | De krachtigste CPU VMs met optionele hoge gegevensdoorvoer netwerkinterfaces (RDMA). 


### <a name="find-available-vm-sizes"></a>Zoeken naar beschikbare VM-grootten

Als een lijst met VM-grootten die beschikbaar zijn in een bepaalde regio wilt weergeven, gebruikt de [az vm-lijst-groottes](/cli/azure/vm#list-sizes) opdracht. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Gedeeltelijke uitvoer:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Virtuele machine maken met een specifieke grootte

In de vorige VM maken bijvoorbeeld is een grootte niet opgegeven, waardoor het een standaardgrootte. Een VM-grootte kan worden geselecteerd maken met behulp van tijd [az vm maken](/cli/azure/vm#create) en de `--size` argument. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>De grootte van een virtuele machine wijzigen

Nadat een virtuele machine is geïmplementeerd, kan het vergroten of verkleinen resourcetoewijzing worden gewijzigd. U kunt de huidige grootte van een virtuele machine met weergeven [az vm weergeven](/cli/azure/vm#show):

```azurecli-interactive
az vm show --resource-group myResourceGroupVM --name myVM --query hardwareProfile.vmSize
```

Grootte wijzigen van een virtuele machine, Controleer of de gewenste grootte beschikbaar op de huidige Azure-cluster is. De [az vm-vm-formaat-opties voor](/cli/azure/vm#list-vm-resize-options) opdracht retourneert de lijst met grootten. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
Als de gewenste grootte beschikbaar is, kan de virtuele machine worden gewijzigd van een status ingeschakeld op, maar deze opnieuw wordt opgestart tijdens de bewerking. Gebruik de [az vm vergroten of verkleinen]( /cli/azure/vm#resize) opdracht voor het uitvoeren van het formaat te wijzigen.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

Als de gewenste grootte niet op het huidige cluster is, moet de VM ongedaan voordat de bewerking formaat kan plaatsvinden. Gebruik de [az vm ongedaan]( /cli/azure/vm#deallocate) opdracht om te stoppen en de VM ongedaan gemaakt. Opmerking: wanneer de virtuele machine wordt ingeschakeld weer, geen gegevens op de tijdelijke schijf kan worden verwijderd. Het openbare IP-adres verandert ook tenzij een statisch IP-adres wordt gebruikt. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

Zodra de toewijzing opgeheven, kan het formaat optreden. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

Na het formaat te wijzigen, kan de virtuele machine worden gestart.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>VM-energiestatus

Een Azure VM kan een van de vele energiestatussen hebben. Deze status vertegenwoordigt de huidige status van de virtuele machine vanuit het oogpunt van de hypervisor. 

### <a name="power-states"></a>Energiestatus

| Energieniveau | Beschrijving
|----|----|
| Starting | Geeft aan dat de virtuele machine wordt gestart. |
| Running | Hiermee wordt aangegeven dat de virtuele machine wordt uitgevoerd. |
| Stopping | Hiermee wordt aangegeven dat de virtuele machine wordt gestopt. | 
| Stopped | Hiermee wordt aangegeven dat de virtuele machine is gestopt. Virtuele machines in de gestopte status nog steeds kosten compute.  |
| Toewijzing | Hiermee wordt aangegeven dat de virtuele machine wordt opgeheven. |
| De toewijzing ongedaan gemaakt | Hiermee wordt aangegeven dat de virtuele machine verwijderd uit de hypervisor maar nog steeds beschikbaar in het vlak van het besturingselement is. Virtuele machines in de status van de Deallocated kan niet worden compute-kosten in rekening. |
| - | Hiermee wordt aangegeven dat de voedingsstatus van de virtuele machine onbekend is. |

### <a name="find-power-state"></a>Energieniveau vinden

Voor het ophalen van de status van een bepaalde virtuele machine, gebruikt u de [az vm-instantieweergave ophalen](/cli/azure/vm#get-instance-view) opdracht. Zorg ervoor dat een geldige naam voor een virtuele machine en de resourcegroep opgeven. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Uitvoer:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Beheertaken

Tijdens de levenscyclus van een virtuele machine, kan u wilt uitvoeren van beheertaken, zoals starten, stoppen of een virtuele machine wordt verwijderd. Bovendien wilt u maken van scripts voor terugkerende of complexe taken automatiseren. Met de Azure CLI, kunnen veel algemene beheertaken worden uitgevoerd vanaf de opdrachtregel of in scripts. 

### <a name="get-ip-address"></a>IP-adres

Deze opdracht retourneert de persoonlijke en openbare IP-adressen van een virtuele machine.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Virtuele machine stoppen

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Virtuele machine starten

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Resourcegroep verwijderen

Verwijderen van een resourcegroep, verwijdert tevens alle resources binnen, zoals de VM, het virtuele netwerk en de schijf. De `--no-wait` parameter retourneert besturingselement op de vraag zonder te wachten totdat de bewerking is voltooid. De `--yes` parameter bevestigt dat u wilt verwijderen van de bronnen zonder een extra prompt om dit te doen.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over basic VM maken en beheren zoals het:

> [!div class="checklist"]
> * Maken en verbinding maken met een virtuele machine
> * Selecteer en gebruik van VM-installatiekopieën
> * Weergeven en gebruiken van specifieke VM-grootten
> * De grootte van een virtuele machine wijzigen
> * Bekijken en te begrijpen status virtuele machine

Ga naar de volgende zelfstudie voor meer informatie over de VM-schijven.  

> [!div class="nextstepaction"]
> [Maken en beheren van VM-schijven](./tutorial-manage-disks.md)
