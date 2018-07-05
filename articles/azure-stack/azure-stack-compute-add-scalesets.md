---
title: Virtual Machine Scale Sets beschikbaar maken in Azure Stack | Microsoft Docs
description: Informatie over hoe een cloud-operator Virtual Machine Scale Sets kunt toevoegen aan de Azure Stack Marketplace
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 06/05/2018
ms.author: brenduns
ms.reviewer: kivenkat
ms.openlocfilehash: ddde2e6bad8a373df405ac05e78a5dbccd0257fc
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "34800637"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Virtual Machine Scale Sets beschikbaar maken in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Virtuele-machineschaalsets vormen een compute-resource van Azure Stack. U kunt deze gebruiken om te implementeren en beheren van een set identieke virtuele machines. Met alle virtuele machines geconfigureerd dezelfde schaalsets vereisen vooraf inrichten van virtuele machines. Het is eenvoudiger om grootschalige services die zijn gericht op big compute, big data en beperkte workloads te ontwikkelen.

In dit artikel begeleidt u door het proces van schaalsets beschikbaar maken in Azure Stack Marketplace. Nadat u deze procedure hebt voltooid, kan uw gebruikers kunnen toevoegen virtuele-machineschaalsets in de abonnementen.

Virtuele-machineschaalsets in Azure Stack zijn, zoals virtuele-machineschaalsets in Azure. Zie voor meer informatie de volgende video's:
* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

In Azure Stack ondersteuning virtuele-machineschaalsets geen voor automatisch schalen. U kunt meer exemplaren toevoegen aan een schaalset met behulp van Resource Manager-sjablonen, CLI of PowerShell.

## <a name="prerequisites"></a>Vereisten

- **Marketplace-syndicatie**  
    Azure Stack registreren met globale Azure Marketplace-publicatie inschakelen. Volg de instructies in [Azure Stack registreren met Azure](azure-stack-registration.md).
- **Installatiekopie van besturingssysteem**  
    Als u een installatiekopie van besturingssysteem nog niet hebt toegevoegd in de Azure Stack Marketplace, Zie [een Azure Stack marketplace-item toevoegen van Azure](asdk/asdk-marketplace-item.md).

## <a name="add-the-virtual-machine-scale-set"></a>Toevoegen van de virtuele-Machineschaalset

1. Open de Azure Stack Marketplace en verbinding maken met Azure. Selecteer **Marketplace management**> **+ toevoegen vanuit Azure**.

    ![Marketplace-management](media/azure-stack-compute-add-scalesets/image01.png)

2. Toevoegen en downloaden van de virtuele-Machineschaalset marketplace-item.

    ![Virtuele-machineschaalset](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Bijwerken van afbeeldingen in een virtuele-Machineschaalset opgehaald

Nadat u een virtuele-machineschaalset gemaakt, kunnen gebruikers afbeeldingen in de schaalset zonder dat de schaalset opnieuw worden gemaakt met kunnen bijwerken. Het proces voor het bijwerken van een installatiekopie, is afhankelijk van de volgende scenario's:

1. Sjabloon voor de implementatie voor virtuele-machineschaalset **Hiermee geeft u de meest recente** voor *versie*:  

   Wanneer de *versie* is ingesteld als **nieuwste** in de *imageReference* sectie van de sjabloon voor een schaal ingesteld, bewerkingen voor het gebruik van scale set opschalen met de nieuwste beschikbare versie Stel exemplaren van de afbeelding voor de schaal. Nadat een omhoog schalen voltooid is, kunt u oudere exemplaren van virtuele machine scale sets verwijderen.  (De waarden voor *publisher*, *bieden*, en *sku* ongewijzigd blijven). 

   Hier volgt een voorbeeld van het opgeven van *nieuwste*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Voordat u omhoog schalen kan een nieuwe installatiekopie kunt gebruiken, moet u die nieuwe installatiekopie downloaden:  

   - Als de afbeelding op de Marketplace is een nieuwere versie dan de installatiekopie in de schaalset: de nieuwe installatiekopie die wordt vervangen door de installatiekopie van het oudere downloaden. Nadat de installatiekopie wordt vervangen, wordt een gebruiker kunt doorgaan met het omhoog schalen. 

   - Wanneer de versie van de installatiekopie op de Marketplace is hetzelfde als de afbeelding in de schaalset: verwijderen van de afbeelding die wordt gebruikt in de schaalset en downloadt u de nieuwe installatiekopie. Tijdens de periode tussen het verwijderen van de oorspronkelijke afbeelding en het downloaden van de nieuwe installatiekopie kan niet u omhoog schalen. 
      
     Dit proces is vereist voor resyndicate afbeeldingen die het gebruik van de indeling voor sparse-bestand, geïntroduceerd in versie 1803. 
 

2. Sjabloon voor de implementatie voor virtuele-machineschaalset **geeft geen nieuwste** voor *versie* en een uniek versienummer in plaats daarvan geeft:  

    Als u een installatiekopie met een nieuwere versie (die de versie verandert) downloadt, kan niet de schaalset omhoog schalen. Dit is standaard als de versie van de installatiekopie opgegeven in de sjabloon van de schaalset moet beschikbaar zijn.  

Zie voor meer informatie, [besturingssysteemschijven en installatiekopieën](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Een virtuele-Machineschaalset verwijderen

Als u wilt verwijderen van een virtuele machine schalen set galerie-item, de volgende PowerShell-opdracht uitvoeren:

```PowerShell  
    Remove-AzsGalleryItem
````

> [!NOTE]
> Het galerie-item kan niet direct worden verwijderd. Moet u nachttoeslagen de portal voor verschillende keren vernieuwen voordat het item wordt weergegeven als verwijderd uit de Marketplace.

## <a name="next-steps"></a>Volgende stappen
[Veelgestelde vragen over Azure Stack](azure-stack-faq.md)