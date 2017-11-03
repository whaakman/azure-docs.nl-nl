---
title: Virtuele-machineschaalsets beschikbaar maken in Azure-Stack
description: Meer informatie over hoe een cloudbeheerder virtuele-machineschaalset kunt toevoegen aan de Stack Azure Marketplace
services: azure-stack
author: anjayajodha
ms.service: azure-stack
ms.topic: article
ms.date: 9/25/2017
ms.author: anajod
keywords: 
ms.openlocfilehash: 31aeb963bdf4fd32712bc6f29f64060ec1c77cb8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Virtuele-machineschaalsets beschikbaar maken in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Virtuele-machineschaalsets zijn een Azure-Stack compute-resource. U kunt deze gebruiken om te implementeren en beheren van een reeks identieke virtuele machines. Alle virtuele machines geconfigureerd dezelfde zijn,-schaalsets niet vooraf inrichten van virtuele machines vereist. Het is gemakkelijker te bouwen van grootschalige services die zijn gericht op big compute, big data en beperkte workloads.

In dit onderwerp leidt u door het proces schaalsets beschikbaar te maken in de Stack Azure Marketplace. Nadat u deze procedure hebt voltooid, kan uw gebruikers kunnen toevoegen virtuele-machineschaalset wordt ingesteld op hun abonnementen.

Virtuele-machineschaalsets op Azure-Stack lijken op virtuele-machineschaalsets in Azure. Zie voor meer informatie de volgende video's:
* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Virtuele-Machineschaalsets ondersteunen op Azure-Stack niet automatisch schalen. U kunt meer exemplaren toevoegen aan een schaal ingesteld met de Stack van Azure portal, Resource Manager-sjablonen of PowerShell.

## <a name="prerequisites"></a>Vereisten
* **PowerShell en hulpprogramma 's**

   Installeren en geconfigureerde PowerShell voor Azure-Stack en de Azure-Stack-hulpprogramma's. Zie [leren werken met PowerShell in Azure Stack](azure-stack-powershell-configure-quickstart.md).

   Nadat u de Azure-Stack-hulpprogramma's geïnstalleerd, zorg ervoor dat u de volgende PowerShell-module importeren (pad relatief de. \ComputeAdmin map in de map AzureStack-hulpprogramma's-master):

        Import-Module .\AzureStack.ComputeAdmin.psm1

* **Installatiekopie van besturingssysteem**

   Als u hebt een besturingssysteemkopie toegevoegd aan uw Azure-Stack Marketplace, Zie [de Windows Server 2016 VM-installatiekopie toevoegen aan de Stack van Azure marketplace](azure-stack-add-default-image.md).

   Voor Linux-ondersteuning, Ubuntu Server 16.04 downloaden en toe te voegen met behulp van ```Add-AzsVMImage``` met de volgende parameters: ```-publisher "Canonical" -offer "UbuntuServer" -sku "16.04-LTS"```.

## <a name="add-the-virtual-machine-scale-set"></a>Toevoegen van de virtuele-machineschaalset

De volgende PowerShell-script voor uw omgeving bewerken en voer vervolgens het toevoegen van een virtuele-machineschaalset ingesteld op de Stack Azure Marketplace. 

``$User``is het account waarmee u verbinding maken met de beheerdersportal. Bijvoorbeeld serviceadmin@contoso.onmicrosoft.com.

```
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds
Select-AzureRmProfile -Profile $AzsEnvContext

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
```

## <a name="remove-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset verwijderen

Als u wilt verwijderen van een virtuele machine schalen set galerij-item, voer de volgende PowerShell-opdracht:

    Remove-AzsVMSSGalleryItem

> [!NOTE]
> Het galerie-item kan niet direct worden verwijderd. Wellicht moet u de portal verschillende keren vernieuwen voordat het wordt verwijderd uit de Marketplace.


## <a name="next-steps"></a>Volgende stappen
[Veelgestelde vragen over Azure-Stack](azure-stack-faq.md)

