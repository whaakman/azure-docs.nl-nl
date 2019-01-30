---
title: Virtual Machine Scale Sets beschikbaar maken in Azure Stack | Microsoft Docs
description: Informatie over hoe een cloud-operator Virtual Machine Scale Sets kunt toevoegen aan de Azure Stack Marketplace
services: azure-stack
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 10/22/2018
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 10/22/2018
ms.openlocfilehash: 3f1c84961f2ad6bd15612917d33982ec96824257
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252265"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Virtual Machine Scale Sets beschikbaar maken in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*
  
Virtuele-machineschaalsets vormen een compute-resource van Azure Stack. U kunt deze gebruiken om te implementeren en beheren van een set identieke virtuele machines. Met alle virtuele machines op dezelfde manier geconfigureerd, hoeven schaalsets niet vooraf in te richten van virtuele machines. Het is eenvoudiger om grootschalige services die zijn gericht op big compute, big data en beperkte workloads te ontwikkelen.

In dit artikel begeleidt u door het proces van schaalsets beschikbaar maken in Azure Stack Marketplace. Nadat u deze procedure hebt voltooid, kan uw gebruikers kunnen toevoegen virtuele-machineschaalsets in de abonnementen.

Virtuele-machineschaalsets in Azure Stack zijn vergelijkbaar met de virtuele-machineschaalsets in Azure. Zie voor meer informatie de volgende video's:
* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

In Azure Stack ondersteunen schaalsets voor virtuele machines geen automatisch schalen. U kunt meer exemplaren toevoegen aan een schaalset met behulp van Resource Manager-sjablonen, CLI of PowerShell.

## <a name="prerequisites"></a>Vereisten

- **De Marketplace:** Azure Stack registreren met globale Azure, de beschikbaarheid van de items in de Marketplace. Volg de instructies in [Azure Stack registreren met Azure](azure-stack-registration.md).
- **Installatiekopie van besturingssysteem:** Voordat een virtuele-machineschaalset (VMSS) kan worden gemaakt, moet u de VM-installatiekopieën voor gebruik in de VMSS van downloaden de [Azure Stack Marketplace](azure-stack-download-azure-marketplace-item.md). De installatiekopieën moeten al aanwezig zijn voordat een gebruiker een nieuwe VMSS kunt maken. 

## <a name="use-the-azure-stack-portal"></a>De Azure Stack-portal gebruiken 

>[!IMPORTANT]  
> De informatie in deze sectie is van toepassing wanneer u Azure Stack-versie 1808 of hoger. Als uw versie 1807 of een eerdere versie, Zie [toevoegen van de virtuele-Machineschaalset opgehaald (vóór 1808)](#add-the-virtual-machine-scale-set-prior-to-version-1808).

1. Aanmelden bij de Azure Stack-portal. Ga vervolgens naar **alle services**, klikt u vervolgens **virtuele-machineschaalsets**, en klik vervolgens onder **COMPUTE**, selecteer **virtuele-machineschaalsets**. 
   ![Selecteer virtuele-machineschaalsets](media/azure-stack-compute-add-scalesets/all-services.png)

2. Selecteer maken ***virtuele-machineschaalsets***.
   ![Een virtuele-machineschaalset maken](media/azure-stack-compute-add-scalesets/create-scale-set.png)

3. Vul de lege velden, kiest u uit het vervolgkeuzemenu's voor **installatiekopie van besturingssysteemschijf**, **abonnement**, en **Exemplaargrootte**. Selecteer **Ja** voor **beheerde schijven gebruiken**. Klik vervolgens op **Maken**.
    ![Configureren en maken](media/azure-stack-compute-add-scalesets/create.png)

4. Om te zien van uw nieuwe virtuele-machineschaalset instellen, gaat u naar **alle resources**, voor naam van de virtuele-machineschaalset zoeken, en selecteer vervolgens de naam ervan in het zoekvak. 
   ![De schaalset bekijken](media/azure-stack-compute-add-scalesets/search.png)

## <a name="add-the-virtual-machine-scale-set-prior-to-version-1808"></a>De virtuele-Machineschaalset opgehaald (voorafgaand aan versie 1808) toevoegen

>[!IMPORTANT]  
> De informatie in deze sectie is van toepassing wanneer u een eerdere versie van Azure Stack dan 1808 gebruikt. Als u versie 1808 of hoger gebruikt, raadpleegt u [gebruiken de Azure Stack-portal](#use-the-azure-stack-portal).

1. Open de Azure Stack Marketplace en verbinding maken met Azure. Selecteer **Marketplace management**, klikt u vervolgens op **+ toevoegen vanuit Azure**.

    ![Marketplace-management](media/azure-stack-compute-add-scalesets/image01.png)

2. Toevoegen en downloaden van de virtuele-Machineschaalset marketplace-item.

    ![Virtuele-machineschaalset](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Bijwerken van afbeeldingen in een virtuele-Machineschaalset opgehaald

Nadat u een virtuele-machineschaalset gemaakt, kunnen gebruikers afbeeldingen in de schaalset zonder dat de schaalset opnieuw worden gemaakt met kunnen bijwerken. Het proces voor het bijwerken van een installatiekopie, is afhankelijk van de volgende scenario's:

1. Hiermee geeft u op virtuele machine implementatie schaalsetsjabloon **nieuwste** voor **versie**:  

   Wanneer de `version` is ingesteld op **nieuwste** in de `imageReference` sectie van de sjabloon voor een schaalset instellen, omhoog schalen-bewerkingen op het gebruik van scale set de nieuwste beschikbare versie van de afbeelding voor de exemplaren van de schaalset. Nadat een scale-up voltooid is, kunt u oudere exemplaren van virtuele machine scale sets verwijderen. De waarden voor `publisher`, `offer`, en `sku` ongewijzigd blijven. 

   Hiermee geeft u op de volgende JSON-voorbeeld `latest`:  

    ```json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Voordat u omhoog kunt een nieuwe installatiekopie gebruiken, moet u die nieuwe installatiekopie downloaden:  

   - Als de afbeelding op de Marketplace een nieuwere versie dan de installatiekopie in de schaalset is, downloadt u de nieuwe installatiekopie die wordt vervangen door de installatiekopie van het oudere. Nadat de installatiekopie wordt vervangen, wordt een gebruiker kunt doorgaan met het omhoog schalen. 

   - Wanneer de versie van de installatiekopie op de Marketplace hetzelfde als de afbeelding in de schaalset is, verwijdert u de installatiekopie die wordt gebruikt in de schaalset en downloadt u de nieuwe installatiekopie. Tijdens de periode tussen het verwijderen van de oorspronkelijke afbeelding en het downloaden van de nieuwe installatiekopie kan niet u omhoog schalen. 
      
     Dit proces is vereist voor resyndicate afbeeldingen die het gebruik van de indeling voor sparse-bestand, geïntroduceerd in versie 1803. 
 
2. Sjabloon voor de implementatie voor virtuele-machineschaalset **geeft geen nieuwste** voor **versie** en een uniek versienummer in plaats daarvan geeft:  

    Als u een installatiekopie met een nieuwere versie (die de versie verandert) downloadt, kan niet de schaalset omhoog schalen. Dit is standaard, zoals de versie van de installatiekopie opgegeven in de sjabloon van de schaalset moet beschikbaar zijn.  

Zie voor meer informatie, [besturingssysteemschijven en installatiekopieën](./user/azure-stack-compute-overview.md#operating-system-disks-and-images).  

## <a name="scale-a-virtual-machine-scale-set"></a>Schalen van een virtuele-machineschaalset

U kunt de schaal van een *virtuele-machineschaalset* naar groter of kleiner maken.  

1. Selecteer in de portal voor uw schaalset en selecteer vervolgens **schalen**.

2. Gebruik van de schuifregelaar om in te stellen de nieuwe niveau van schaalbaarheid voor deze virtuele-machineschaalset en klik vervolgens op **opslaan**.
     ![De schaalset](media/azure-stack-compute-add-scalesets/scale.png)

## <a name="remove-a-virtual-machine-scale-set"></a>Een virtuele-Machineschaalset verwijderen

Als u wilt verwijderen van een virtuele-Machineschaalset galerij-item, voer de volgende PowerShell-opdracht:

```PowerShell  
Remove-AzsGalleryItem
```

> [!NOTE]
> Het galerie-item kan niet direct worden verwijderd. Moet u nachttoeslagen de portal voor verschillende keren vernieuwen voordat het item wordt weergegeven als verwijderd uit de Marketplace.

## <a name="next-steps"></a>Volgende stappen

[Marketplace-items van Azure naar Azure Stack downloaden](azure-stack-download-azure-marketplace-item.md)