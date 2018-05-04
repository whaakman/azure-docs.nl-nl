---
title: Zorg virtuele-machineschaalsets beschikbaar zijn in Azure-Stack | Microsoft Docs
description: Meer informatie over hoe een cloud-operator virtuele-machineschaalset kunt toevoegen aan de Stack Azure Marketplace
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.topic: article
ms.date: 04/06/2018
ms.author: brenduns
ms.reviewer: anajod
keywords: ''
ms.openlocfilehash: cdabd2a9d336cdd8ac83d27460fe129c45b7e1c6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Virtuele-machineschaalsets beschikbaar maken in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Virtuele-machineschaalsets zijn een Azure-Stack compute-resource. U kunt deze gebruiken om te implementeren en beheren van een reeks identieke virtuele machines. Alle virtuele machines geconfigureerd dezelfde zijn,-schaalsets niet vooraf inrichten van virtuele machines vereist. Het is gemakkelijker te bouwen van grootschalige services die zijn gericht op big compute, big data en beperkte workloads.

In dit artikel begeleidt u bij het proces schaalsets beschikbaar te maken in de Stack Azure Marketplace. Nadat u deze procedure hebt voltooid, kan uw gebruikers kunnen toevoegen virtuele-machineschaalset wordt ingesteld op hun abonnementen.

Virtuele-machineschaalsets op Azure-Stack lijken op virtuele-machineschaalsets in Azure. Zie voor meer informatie de volgende video's:
* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Op Azure-Stack ondersteuning virtuele-machineschaalsets geen voor automatisch schalen. U kunt meer exemplaren toevoegen aan een schaal ingesteld met de Stack van Azure portal, Resource Manager-sjablonen of PowerShell.

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

``$User`` is het account waarmee u verbinding maken met de beheerdersportal. Bijvoorbeeld serviceadmin@contoso.onmicrosoft.com.

```
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
```

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Afbeeldingen in een virtuele-machineschaalset bijwerken 
Nadat u een virtuele-machineschaalset gemaakt, kunnen gebruikers afbeeldingen in de schaal zonder de schaal instelt opnieuw worden gemaakt met bijwerken. Het proces voor het bijwerken van een installatiekopie, is afhankelijk van de volgende scenario's:

1. Virtuele-machineschaalset implementatiesjabloon **Hiermee geeft u de meest recente** voor *versie*:  

   Wanneer de *versie* is ingesteld als **nieuwste** in de *imageReference* gedeelte van de sjabloon voor een scale-ingesteld, bewerkingen op het gebruik van scale set opschalen met de nieuwste versie Stel de exemplaren van de afbeelding voor de schaal. Nadat een schaal van voltooid is, kunt u oudere sets exemplaren van de virtuele machine scale verwijderen.  (De waarden voor *publisher*, *bieden*, en *sku* ongewijzigd blijven). 

   Hieronder volgt een voorbeeld van het opgeven van *nieuwste*:  

          "imageReference": {
             "publisher": "[parameters('osImagePublisher')]",
             "offer": "[parameters('osImageOffer')]",
             "sku": "[parameters('osImageSku')]",
             "version": "latest"
             }

   Voordat de schaal van een nieuwe installatiekopie kunt gebruiken, moet u die nieuwe installatiekopie te downloaden:  

   - Wanneer de installatiekopie op de Marketplace is een nieuwere versie dan de installatiekopie in de schaalset: nieuwe installatiekopie die wordt vervangen door de installatiekopie van het oudere downloaden. Nadat de installatiekopie wordt vervangen, wordt een gebruiker kunt doorgaan met het omhoog schalen. 

   - Als de versie van de installatiekopie op de Marketplace is hetzelfde als de afbeelding in de schaalset: de afbeelding die wordt gebruikt in de schaalset verwijderen en vervolgens de nieuwe installatiekopie te downloaden. Tijdens de tijd tussen het verwijderen van de originele installatiekopie en het downloaden van de nieuwe installatiekopie, kunt geen u opschalen. 
      
     Dit proces is vereist voor resyndicate afbeeldingen die het gebruik van de sparse bestandsindeling, geïntroduceerd in versie 1803. 
 

2. Virtuele-machineschaalset implementatiesjabloon **geeft geen recente** voor *versie* en geeft een versienummer in plaats daarvan:  

     Als u een afbeelding met een nieuwere versie (die de beschikbare versie wijzigt) downloadt, kan niet de schaalaanpassingsset omhoog schalen. Dit is standaard als de image-versie die is opgegeven in de sjabloon van de set schaal moet beschikbaar zijn.  

Zie voor meer informatie [besturingssysteem schijven en installatiekopieën](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset verwijderen

Als u wilt verwijderen van een virtuele machine schalen set galerij-item, voer de volgende PowerShell-opdracht:

    Remove-AzsVMSSGalleryItem

> [!NOTE]
> Het galerie-item kan niet direct worden verwijderd. Moet u nachttoeslagen de portal verschillende keren vernieuwen voordat het item wordt verwijderd uit de Marketplace.


## <a name="next-steps"></a>Volgende stappen
[Veelgestelde vragen over Azure-Stack](azure-stack-faq.md)

