---
title: Een toepassing implementeren in een schaalset voor virtuele Azure-machine | Microsoft Docs
description: Meer informatie over het implementeren van toepassingen voor Linux en Windows VM-exemplaren in een schaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 4b977a2fe9dadfe42e02063fa4fa291b9be484ac
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733131"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Implementeer uw toepassing op virtuele-machineschaalsets
Als u toepassingen wilt uitvoeren op de exemplaren van een virtuele machine (VM) in een schaalset, moet u eerst de toepassingsonderdelen en de vereiste bestanden installeren. Dit artikel bevat manieren voor het maken van een aangepaste VM-installatiekopie voor exemplaren in een schaalset instellen of installatiescripts op bestaande VM-exemplaren automatisch wordt uitgevoerd. U leert ook hoe u voor het beheren van toepassingen of updates van het besturingssysteem in een schaalset.


## <a name="build-a-custom-vm-image"></a>Een aangepaste VM-installatiekopie bouwen
Wanneer u een van de Azure-platform-installatiekopieën te maken van de exemplaren in uw schaalset gebruikt, is er geen aanvullende software geïnstalleerd of geconfigureerd. U kunt automatiseren de installatie van deze onderdelen, maar die wordt toegevoegd aan de tijd die nodig zijn voor het inrichten van VM-exemplaren in uw schaalsets. Als u veel wijzigingen in de configuratie op de VM-exemplaren toepassen, is de management overhead met deze configuratiescripts en taken.

Om te beperken het beheer van de configuratie en de tijd voor het inrichten van een virtuele machine, kunt u een aangepaste VM-installatiekopie die klaar zijn voor het uitvoeren van uw toepassing als een exemplaar is ingericht in de schaalset. Voor meer informatie over het maken en gebruiken van een aangepaste VM-installatiekopie met een schaal, raadpleegt u de volgende zelfstudies:

- [Azure-CLI](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="already-provisioned"></a>Een app installeren met de aangepaste Scriptextensie
Met de aangepaste scriptextensie kunnen scripts worden gedownload en uitgevoerd op virtuele machines in Azure. Deze uitbreiding is handig voor post-implementatieconfiguraties, software-installaties of andere configuratie-/beheertaken. Scripts kunnen worden gedownload uit Azure Storage of GitHub, of worden geleverd in Azure Portal tijdens de uitvoering van extensies. Voor meer informatie over het maken en gebruiken van een aangepaste VM-installatiekopie met een schaal, raadpleegt u de volgende zelfstudies:

- [Azure-CLI](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Azure Resource Manager-sjabloon](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Een app installeren op een Windows VM maken met PowerShell DSC
[PowerShell Desired State Configuration (DSC)](https://msdn.microsoft.com/powershell/dsc/overview) is een beheerplatform voor het definiëren van de configuratie van doelmachines. DSC-configuraties definiëren wat u wilt installeren op een computer en het configureren van de host. Een engine voor de lokale Configuration Manager (LCM) wordt uitgevoerd op elk doelknooppunt waarmee aangevraagde acties op basis van gepushte configuraties worden verwerkt.

De PowerShell DSC-extensie kunt u aanpassen van VM-exemplaren in een schaalset met PowerShell. Het volgende voorbeeld:

- Hiermee geeft u de VM-exemplaren een DSC-pakket downloaden vanuit GitHub- *https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Hiermee stelt u de extensie voor het uitvoeren van een script voor installatie- `configure-http.ps1`
- Hiermee haalt u informatie over een schaalset met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- De extensie is van toepassing op de VM-exemplaren met [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

De DSC-extensie is toegepast op de *myScaleSet* VM-exemplaren in de resourcegroep met de naam *myResourceGroup*. Voer uw eigen namen als volgt in:

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

Als het Upgradebeleid op uw schaalset *handmatige*, bijwerken van uw VM-exemplaren met [Update-AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Deze cmdlet configuratie van de bijgewerkte schaalset is van toepassing op de VM-exemplaren en uw toepassing wordt geïnstalleerd.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Een app installeren met een Linux-VM met cloud-init
[Cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html) is een veelgebruikte benadering voor het aanpassen van een Linux-VM als deze voor de eerste keer wordt opgestart. U kunt cloud-init gebruiken voor het installeren van pakketten en schrijven van bestanden, of om gebruikers en beveiliging te configureren. Als de initialisatie van de cloud-init wordt uitgevoerd tijdens het opstartproces, zijn er geen extra stappen of agents vereist om uw configuratie toe te passen.

Cloud-init werkt ook in distributies. U gebruikt bijvoorbeeld niet **apt-get install** of **yum install** om een pakket te installeren. In plaats daarvan kunt u een lijst definiëren met te installeren pakketten. Cloud-init maakt automatisch gebruik van het hulpprogramma voor systeemeigen pakketbeheer voor de distro die u selecteert.

Voor meer informatie, met inbegrip van een voorbeeld *cloud-init.txt* bestand, Zie [cloud-init gebruiken voor het aanpassen van virtuele Azure-machines](../virtual-machines/linux/using-cloud-init.md).

Een schaalset maken en gebruiken van een cloud-init-bestand, voeg de `--custom-data` parameter voor de [az vmss maken](/cli/azure/vmss) opdracht en geeft u de naam van een cloud-init-bestand. Het volgende voorbeeld wordt een schaalset met de naam *myScaleSet* in *myResourceGroup* en VM-exemplaren configureert met een bestand met de naam *cloud-init.txt*. Voer uw eigen namen als volgt in:

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


### <a name="install-applications-with-os-updates"></a>Toepassingen met updates van het besturingssysteem installeren
Wanneer er nieuwe OS releases beschikbaar zijn, u kunt gebruiken of een nieuwe aangepaste installatiekopie bouwen en [besturingssysteemupgrades implementeren](virtual-machine-scale-sets-upgrade-scale-set.md) ingesteld op een schaal. Elk VM-exemplaar is bijgewerkt naar de meest recente installatiekopie die u opgeeft. U kunt een aangepaste installatiekopie met de toepassing vooraf is geïnstalleerd, de aangepaste Scriptextensie of PowerShell DSC gebruiken om uw toepassing automatisch beschikbaar als u de upgrade uitvoert. Mogelijk moet u plannen voor het Toepassingonderhoud van als u dit proces om ervoor te zorgen dat er geen versie compatibiliteitsproblemen uitvoert.

Als u een aangepaste VM-installatiekopie met de toepassing vooraf is geïnstalleerd, kunt u de updates van toepassingen kunt integreren met een implementatiepijplijn naar de nieuwe afbeeldingen bouwen en implementeren van upgrades voor het besturingssysteem van de schaalset. Deze aanpak kunt u de pijplijn voor het kiezen van de meest recente builds van toepassing, maken en valideren van een VM-installatiekopie en vervolgens de upgrade van de VM-exemplaren in de schaalset. Als u wilt uitvoeren van een implementatiepijplijn die u bouwt en implementeert u updates van toepassingen via aangepaste VM-installatiekopieën, kunt u [een Packer-installatiekopie maken en implementeren met Azure DevOps Services](/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset), of gebruik een ander platform, zoals [Spinnaker](https://www.spinnaker.io/) of [Jenkins](https://jenkins.io/).


## <a name="next-steps"></a>Volgende stappen
Als u bouwen en implementeren van toepassingen voor uw schaalsets, kunt u bekijken de [schaal ingesteld ontwerp overzicht](virtual-machine-scale-sets-design-overview.md). Zie voor meer informatie over het beheren van uw schaalset [PowerShell gebruiken voor het beheren van uw schaalset](virtual-machine-scale-sets-windows-manage.md).
