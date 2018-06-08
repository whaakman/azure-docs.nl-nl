---
title: Een toepassing implementeert op een virtuele machine van Azure-schaalset | Microsoft Docs
description: Meer informatie over het implementeren van toepassingen voor Linux en Windows virtuele machine-exemplaren in een schaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: bbbe677b0a0d47147ace41ff5a229282f80bbf1b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839512"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Implementeren van uw toepassing op virtuele-machineschaalsets
Als u toepassingen wilt uitvoeren op de exemplaren van een virtuele machine (VM) in een schaalset, moet u eerst de toepassingsonderdelen en de vereiste bestanden installeren. Dit artikel bevat manieren voor het maken van een aangepaste VM-installatiekopie voor de instanties in een schaal ingesteld of installatiescripts automatisch wordt uitgevoerd op een bestaande VM-exemplaren. U leert ook hoe voor het beheren van de toepassing of updates voor het besturingssysteem op een scale-set.


## <a name="build-a-custom-vm-image"></a>Een aangepaste VM-installatiekopie bouwen
Wanneer u een van de Azure-platform-installatiekopieën maken van de exemplaren in de schaalset gebruikt, is geen extra software geïnstalleerd of geconfigureerd. U kunt automatiseren de installatie van deze onderdelen echter die wordt toegevoegd aan de tijd die nodig zijn voor het inrichten van VM-exemplaren op uw scale-sets. Als u veel configuratiewijzigingen voor de VM-instanties toepassen, is het beheer van de overhead van die configuratiescripts en taken.

Als u het beheer van de configuratie en de tijd voor het inrichten van een virtuele machine, kunt u een aangepaste VM-installatiekopie dat gereed is voor uw toepassing uitvoeren zodra u een exemplaar is ingericht in de schaalset. Voor meer informatie over het maken en gebruiken van een aangepaste installatiekopie van de virtuele machine met een schaal, raadpleegt u de volgende zelfstudies:

- [Azure CLI 2.0](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="already-provisioned"></a>Een app installeren met de extensie voor aangepaste scripts
Met de aangepaste scriptextensie kunnen scripts worden gedownload en uitgevoerd op virtuele machines in Azure. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. Voor meer informatie over het maken en gebruiken van een aangepaste installatiekopie van de virtuele machine met een schaal, raadpleegt u de volgende zelfstudies:

- [Azure CLI 2.0](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Azure Resource Manager-sjabloon](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Een app installeren op een Windows-VM met PowerShell DSC
[PowerShell Desired State Configuration (DSC)](https://msdn.microsoft.com/powershell/dsc/overview) is een beheerplatform voor het definiëren van de configuratie van de doelcomputers. DSC-configuraties definiëren wat u wilt installeren op een computer en het configureren van de host. Een lokale Configuration Manager (LCM)-engine wordt uitgevoerd op elk doelknooppunt waarmee aangevraagde acties op basis van pushed configuraties worden verwerkt.

De PowerShell DSC-uitbreiding kunt u VM-exemplaren in een instellen met PowerShell schaal aanpassen. Het volgende voorbeeld:

- Hiermee geeft u de VM-exemplaren voor het downloaden van een DSC-pakket van GitHub: *https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Hiermee stelt u de uitbreiding voor het uitvoeren van een script voor installatie- `configure-http.ps1`
- Hiermee haalt u informatie over een instellen met schaal [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- De extensie is van toepassing op de VM-instanties met [Update AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

De DSC-uitbreiding is toegepast op de *myScaleSet* VM-exemplaren in de resourcegroep met de naam *myResourceGroup*. Voer uw eigen namen:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip";
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
[Cloud-init](https://cloudinit.readthedocs.io/latest/) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud-init gebruiken voor het installeren van pakketten en schrijven van bestanden, of om gebruikers en beveiliging te configureren. Als de initialisatie van de cloud-init wordt uitgevoerd tijdens het opstartproces, zijn er geen extra stappen of agents vereist om uw configuratie toe te passen.

Cloud-init werkt ook in distributies. U gebruikt bijvoorbeeld niet **apt-get install** of **yum install** om een pakket te installeren. In plaats daarvan kunt u een lijst definiëren met te installeren pakketten. Cloud-init maakt automatisch gebruik van het hulpprogramma voor systeemeigen pakketbeheer voor de distro die u selecteert.

Voor meer informatie, waaronder een voorbeeld *cloud init.txt* bestand, Zie [cloud init gebruiken voor het aanpassen van Azure Virtual machines](../virtual-machines/linux/using-cloud-init.md).

Toevoegen als u wilt een schaalset maken en gebruiken van een cloud-init-bestand, de `--custom-data` -parameter voor de [az vmss maken](/cli/azure/vmss#az_vmss_create) opdracht in en geef de naam van een cloud-init-bestand. Het volgende voorbeeld wordt een set met de naam scale *myScaleSet* in *myResourceGroup* en VM-instanties configureert met een bestand met de naam *cloud init.txt*. Voer uw eigen namen:

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


### <a name="install-applications-with-os-updates"></a>Toepassingen met updates voor het besturingssysteem installeren
Wanneer nieuwe OS-versies beschikbaar zijn, u kunt gebruiken of een nieuwe aangepaste installatiekopie bouwen en [upgrades voor het besturingssysteem implementeren](virtual-machine-scale-sets-upgrade-scale-set.md) ingesteld op een schaal. Elk exemplaar van de virtuele machine is bijgewerkt naar de meest recente installatiekopie die u opgeeft. U kunt een aangepaste installatiekopie met de toepassing vooraf is geïnstalleerd, de extensie voor aangepaste scripts of PowerShell DSC gebruiken om uw toepassing automatisch beschikbaar als u de upgrade uitvoert. Mogelijk moet u plannen voor het Toepassingonderhoud van de zoals u dit proces om ervoor te zorgen dat er geen versie compatibiliteitsproblemen zijn uitvoeren.

Als u een aangepaste installatiekopie van de virtuele machine met de toepassing vooraf is geïnstalleerd, kunt u de toepassingsupdates kan integreren met een pipeline implementatie kunnen de nieuwe afbeeldingen bouwen en implementeren van upgrades voor het besturingssysteem op de scale-set. Deze aanpak kunt de pijplijn voor de meest recente builds van toepassing kunnen worden opgepikt, maken en valideren van een VM-installatiekopie en vervolgens de upgrade van de VM-exemplaren in de schaalset. Voor het uitvoeren van een pijplijn implementatie dat bouwt en toepassingsupdates via aangepaste installatiekopieën voor virtuele machine implementeert, kunt u [een verpakker installatiekopie maken en implementeren met Visual Studio Team Services](/vsts/pipelines/apps/cd/azure/deploy-azure-scaleset), of gebruik een ander platform, zoals [Spinnaker ](https://www.spinnaker.io/) of [Jenkins](https://jenkins.io/).


## <a name="next-steps"></a>Volgende stappen
Bij het bouwen en implementeren van toepassingen voor uw schaalsets, kunt u controleren de [Scale ingesteld ontwerp overzicht](virtual-machine-scale-sets-design-overview.md). Zie voor meer informatie over het beheren van uw scale set [Gebruik PowerShell voor het beheren van uw scale set](virtual-machine-scale-sets-windows-manage.md).
