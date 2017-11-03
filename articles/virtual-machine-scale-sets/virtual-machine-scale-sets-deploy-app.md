---
title: Een toepassing implementeert op een virtuele machine van Azure-schaalset | Microsoft Docs
description: Meer informatie over het implementeren van toepassingen voor Linux en Windows virtuele machine-exemplaren in een schaalset
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou
ms.openlocfilehash: 7e03d5e2bbdb1b3b206fa7fa455f7dce7951f02b
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Implementeren van uw toepassing op virtuele-machineschaalsets
Om toepassingen op de virtuele machine (VM)-exemplaren in een schaalset uitvoeren, moet u eerst de toepassingsonderdelen en de vereiste bestanden wilt installeren. Dit artikel bevat manieren voor het maken van een aangepaste VM-installatiekopie voor de instanties in een schaal ingesteld of installatiescripts automatisch wordt uitgevoerd op een bestaande VM-exemplaren. U leert ook hoe voor het beheren van de toepassing of updates voor het besturingssysteem op een scale-set.


## <a name="build-a-custom-vm-image"></a>Een aangepaste VM-installatiekopie bouwen
Wanneer u een van de Azure-platform-installatiekopieën maken van de exemplaren in de schaalset gebruikt, is geen extra software geïnstalleerd of geconfigureerd. U kunt automatiseren de installatie van deze onderdelen echter die wordt toegevoegd aan de tijd die nodig zijn voor het inrichten van VM-exemplaren op uw scale-sets. Als u veel configuratiewijzigingen voor de VM-instanties toepassen, is het beheer van de overhead van die configuratiescripts en taken.

Als u het beheer van de configuratie en de tijd voor het inrichten van een virtuele machine, kunt u een aangepaste VM-installatiekopie dat gereed is voor uw toepassing uitvoeren zodra u een exemplaar is ingericht in de schaalset. Het algehele proces voor het maken van een aangepaste VM-installatiekopie voor scale set instanties zijn als volgt:

1. Om een aangepaste VM-installatiekopie voor uw scale set exemplaren bouwt, u maken en zich aanmelden bij een virtuele machine, en vervolgens installeren en configureren van de toepassing. U kunt verpakker definiëren en bouwen van een [Linux](../virtual-machines/linux/build-image-with-packer.md) of [Windows](../virtual-machines/windows/build-image-with-packer.md) VM-installatiekopie. Of u kunt handmatig maken en configureren van de virtuele machine:

    - Maak een Linux-VM met de [Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md), [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md), of de [portal](../virtual-machines/linux/quick-create-portal.md).
    - Maken van een Windows-VM met de [Azure PowerShell](../virtual-machines/windows/quick-create-powershell.md), wordt de [Azure CLI 2.0](../virtual-machines/windows/quick-create-cli.md), of de [portal](../virtual-machines/windows/quick-create-portal.md).
    - Meld u aan bij een [Linux](../virtual-machines/linux/mac-create-ssh-keys.md#use-the-ssh-key-pair) of [Windows](../virtual-machines/windows/connect-logon.md) VM.
    - Installeer en configureer de toepassingen en hulpmiddelen die nodig zijn. Als u specifieke versies van een bibliotheek of runtime nodig, een aangepaste installatiekopie van de virtuele machine kunt u definiëren van een versie en 

2. Vastleggen van uw virtuele machine met de [Azure CLI 2.0](../virtual-machines/linux/capture-image.md) of [Azure PowerShell](../virtual-machines/windows/capture-image.md). Deze stap maakt de aangepaste VM-installatiekopie die wordt gebruikt voor de instanties in een schaalset vervolgens te implementeren.

3. [Maken van een schaalset](virtual-machine-scale-sets-create.md) en geef de aangepaste VM-installatiekopie in de voorgaande stappen hebt gemaakt.


## <a name="already-provisioned"></a>Een app installeren met de extensie voor aangepaste scripts
De aangepaste Scriptextensie downloads en scripts worden uitgevoerd op Azure Virtual machines. Deze uitbreiding is nuttig voor post-implementatieconfiguratie, software-installatie of een andere configuratie / beheertaak. Scripts kunnen worden gedownload van Azure storage of GitHub, of naar de Azure portal tijdens runtime extensie.

De aangepaste scriptextensie kan worden geïntegreerd met Azure Resource Manager-sjablonen en kan ook worden uitgevoerd met de Azure CLI, PowerShell, Azure-portal of de REST-API van Azure virtuele Machine. 

Zie voor meer informatie de [overzicht van de aangepaste Scriptextensie](../virtual-machines/windows/extensions-customscript.md).


### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
PowerShell maakt gebruik van een hashtabel voor het opslaan van het bestand te downloaden en de opdracht uit te voeren. Het volgende voorbeeld:

- Hiermee geeft u de VM-exemplaren voor het downloaden van een script vanuit GitHub - *https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1*
- Hiermee stelt u de uitbreiding voor het uitvoeren van een script voor installatie-`powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1`
- Hiermee haalt u informatie over een instellen met schaal [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- De extensie is van toepassing op de VM-instanties met [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

De aangepaste Scriptextensie wordt toegepast op de *myScaleSet* VM-exemplaren in de resourcegroep met de naam *myResourceGroup*. Voer uw eigen namen:

```powershell
# Define the script for your Custom Script Extension to run
$customConfig = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Als het Upgradebeleid op uw schaalset *handmatige*, bijwerken van uw VM-exemplaren met [Update AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Deze cmdlet geldt de configuratie van de bijgewerkte scale set voor VM-exemplaren en uw toepassing wordt geïnstalleerd.


### <a name="use-azure-cli-20"></a>Azure CLI 2.0 gebruiken
Als u wilt de aangepaste Scriptextensie met de Azure CLI gebruiken, moet u een JSON-bestand dat definieert welke bestanden verkrijgen en de opdrachten uit te voeren maken. Deze JSON-definities kunnen opnieuw worden gebruikt met scale set implementaties om toe te passen consistente toepassing wordt geïnstalleerd.

Maak een bestand met de naam in uw huidige shell *customConfig.json* en plak de volgende configuratie. Maak bijvoorbeeld het bestand in de Cloud-Shell niet op uw lokale machine. U kunt een editor die u wilt gebruiken. Voer `sensible-editor cloudConfig.json` voor het maken van het bestand en een overzicht van beschikbare editors.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

De configuratie van de aangepaste Scriptextensie toepassen op de VM-exemplaren in uw instellen met schaal [az vmss extensie set](/cli/azure/vmss/extension#set). Het volgende voorbeeld wordt de *customConfig.json* configuratie van de *myScaleSet* VM-exemplaren in de resourcegroep met de naam *myResourceGroup*. Voer uw eigen namen:

```azurecli
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroup \
    --vmss-name myScaleSet \
    --settings @customConfig.json
```

Als het Upgradebeleid op uw schaalset *handmatige*, bijwerken van uw VM-exemplaren met [az vmss update-exemplaren](/cli/azure/vmss#update-instances). Deze cmdlet geldt de configuratie van de bijgewerkte scale set voor VM-exemplaren en uw toepassing wordt geïnstalleerd.


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Een app installeren op een Windows-VM met PowerShell DSC
[PowerShell Desired State Configuration (DSC)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) is een beheerplatform voor het definiëren van de configuratie van de doelcomputers. DSC-configuraties definiëren wat u wilt installeren op een computer en het configureren van de host. Een lokale Configuration Manager (LCM)-engine wordt uitgevoerd op elk doelknooppunt waarmee aangevraagde acties op basis van pushed configuraties worden verwerkt.

De PowerShell DSC-uitbreiding kunt u VM-exemplaren in een instellen met PowerShell schaal aanpassen. Het volgende voorbeeld:

- Hiermee geeft u de VM-exemplaren voor het downloaden van een DSC-pakket van GitHub - *https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip*
- Hiermee stelt u de uitbreiding voor het uitvoeren van een script voor installatie-`configure-http.ps1`
- Hiermee haalt u informatie over een instellen met schaal [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- De extensie is van toepassing op de VM-instanties met [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

De DSC-uitbreiding is toegepast op de *myScaleSet* VM-exemplaren in de resourcegroep met de naam *myResourceGroup*. Voer uw eigen namen:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Als het Upgradebeleid op uw schaalset *handmatige*, bijwerken van uw VM-exemplaren met [Update AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Deze cmdlet geldt de configuratie van de bijgewerkte scale set voor VM-exemplaren en uw toepassing wordt geïnstalleerd.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Een app installeren voor een Linux-VM met cloud-init
[Cloud-init](https://cloudinit.readthedocs.io/latest/) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud init gebruiken voor het installeren van pakketten en bestanden schrijven of om gebruikers en beveiliging te configureren. Als de initialisatie van de cloud wordt uitgevoerd tijdens het opstartproces, zijn er geen extra stappen of vereist agents naar uw configuratie toe te passen.

Cloud-init werkt ook via distributies. Bijvoorbeeld, u niet gebruikt **apt get-installatie** of **yum installeren** om een pakket te installeren. In plaats daarvan kunt u een lijst met pakketten te installeren. Het hulpprogramma voor systeemeigen pakket cloud init automatisch gebruikt voor de distro die u selecteert.

Voor meer informatie, waaronder een voorbeeld *cloud init.txt* bestand, Zie [cloud init gebruiken voor het aanpassen van Azure Virtual machines](../virtual-machines/linux/using-cloud-init.md).

Toevoegen als u wilt een schaalset maken en gebruiken van een cloud-init-bestand, de `--custom-data` -parameter voor de [az vmss maken](/cli/azure/vmss#create) opdracht in en geef de naam van een cloud-init-bestand. Het volgende voorbeeld wordt een set met de naam scale *myScaleSet* in *myResourceGroup* en VM-instanties configureert met een bestand met de naam *cloud init.txt*. Voer uw eigen namen:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="install-applications-as-a-set-scales-out"></a>Toepassingen installeren als een set uitgeschaald
-Schaalsets kunnen u Verhoog het aantal VM-exemplaren die uw toepassing uitvoeren. Deze scale-out proces worden gestart handmatig of automatisch op basis van de metrische gegevens zoals CPU of geheugen gebruik.

Als u een aangepaste Scriptextensie op de schaalaanpassingsset toegepast, wordt de toepassing is geïnstalleerd op elk nieuwe VM-exemplaar. Als de scale-set is gebaseerd op een aangepaste installatiekopie met de toepassing vooraf is geïnstalleerd, wordt elke nieuwe VM-instantie wordt geïmplementeerd in een bruikbaar staat. 

Als de VM-instanties van de schaal set container hosts, kunt u de aangepaste Scriptextensie ophalen en uitvoeren hoeft installatiekopieën van de container. De aangepaste scriptextensie kan ook de nieuwe VM-instantie registreren met een orchestrator, zoals Azure Container Service.


## <a name="deploy-application-updates"></a>Toepassingsupdates implementeren
Als u uw toepassingscode, -bibliotheken of pakketten bijwerkt, kunt u de meest recente toestand van de toepassing pushen naar VM-exemplaren in een schaalset. Als u de aangepaste Scriptextensie, updates voor uw toepassing gebruikt en niet automatisch geïmplementeerd. Wijzig de configuratie aangepast Script zoals verwijzen naar een script voor installatie met de naam van een bijgewerkte versie. In een eerder voorbeeld de aangepaste Scriptextensie maakt gebruik van een script met de naam *automate_nginx.sh* als volgt:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Eventuele wijzigingen die u voor uw toepassing worden niet blootgesteld aan de aangepaste Scriptextensie tenzij die wijzigingen script installeert. Een aanpak is het opnemen van een versienummer dat stappen met uw toepassing worden vrijgegeven. De aangepaste scriptextensie kan nu verwijzen naar *automate_nginx_v2.sh* als volgt:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx_v2.sh"],
  "commandToExecute": "./automate_nginx_v2.sh"
}
```

De aangepaste Scriptextensie is nu wordt uitgevoerd op de VM-instanties om toe te passen, de meest recente toepassingsupdates.


### <a name="install-applications-with-os-updates"></a>Toepassingen met updates voor het besturingssysteem installeren
Wanneer nieuwe OS-versies beschikbaar zijn, u kunt gebruiken of een nieuwe aangepaste installatiekopie bouwen en [upgrades voor het besturingssysteem implementeren](virtual-machine-scale-sets-upgrade-scale-set.md) ingesteld op een schaal. Elk exemplaar van de virtuele machine is bijgewerkt naar de meest recente installatiekopie die u opgeeft. U kunt een aangepaste installatiekopie met de toepassing vooraf is geïnstalleerd, de extensie voor aangepaste scripts of PowerShell DSC gebruiken om uw toepassing automatisch beschikbaar als u de upgrade uitvoert. Mogelijk moet u plannen voor het Toepassingonderhoud van de zoals u dit proces om ervoor te zorgen dat er geen versie compatibiliteitsproblemen zijn uitvoeren.

Als u een aangepaste installatiekopie van de virtuele machine met de toepassing vooraf is geïnstalleerd, kunt u de toepassingsupdates kan integreren met een pipeline implementatie kunnen de nieuwe afbeeldingen bouwen en implementeren van upgrades voor het besturingssysteem op de scale-set. Deze aanpak kunt de pijplijn voor de meest recente builds van toepassing kunnen worden opgepikt, maken en valideren van een VM-installatiekopie en vervolgens de upgrade van de VM-exemplaren in de schaalset. U kunt gebruiken voor het uitvoeren van een implementatie-pijplijn die is gebaseerd en toepassingsupdates implementeert via aangepaste VM-installatiekopieën, [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Spinnaker](https://www.spinnaker.io/), of [Jenkins](https://jenkins.io/) .


## <a name="next-steps"></a>Volgende stappen
Bij het bouwen en implementeren van toepassingen voor uw schaalsets, kunt u controleren de [Scale ingesteld ontwerp overzicht](virtual-machine-scale-sets-design-overview.md). Zie voor meer informatie over het beheren van uw scale set [Gebruik PowerShell voor het beheren van uw scale set](virtual-machine-scale-sets-windows-manage.md).
