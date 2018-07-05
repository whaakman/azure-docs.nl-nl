---
title: Toevoegen en verwijderen van een VM-installatiekopie naar Azure Stack | Microsoft Docs
description: Toevoegen of verwijderen van uw organisatie aangepaste Windows of Linux-VM-installatiekopie voor tenants te gebruiken.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 06/27/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 5c2088ab39e32c049ce867698e84efba759c9a87
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447333"
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>De installatiekopie van een virtuele machine beschikbaar maken in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In Azure Stack, kunt u installatiekopieën van virtuele machines beschikbaar aan uw gebruikers. Deze installatiekopieën kunnen worden verwezen door Azure Resource Manager-sjablonen of kunt u ze toevoegen aan de gebruikersinterface van de Azure Marketplace als een Marketplace-item. U kunt ofwel een installatiekopie-formulier gebruiken de globale Azure Marketplace of uw eigen aangepaste installatiekopie toevoegen. U kunt een virtuele machine met behulp van de portal of Windows PowerShell kunt toevoegen.

## <a name="add-a-vm-image-through-the-portal"></a>Toevoegen van een VM-installatiekopie via de portal

> [!NOTE]
> Met deze methode moet u de Marketplace-item afzonderlijk maken.

Afbeeldingen moeten kunnen worden verwezen door een blob storage-URI. De installatiekopie voor een Windows- of Linux-besturingssysteem in VHD-indeling (niet VHDX) voorbereiden en vervolgens de installatiekopie uploaden naar een opslagaccount in Azure of Azure Stack. Als uw installatiekopie is al geüpload naar de blob-opslag in Azure of Azure Stack, kunt u stap 1 overslaan.

1. [Een Windows VM-installatiekopie uploaden naar Azure voor Resource Manager-implementaties](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) of voor een Linux-installatiekopie, volgt u de instructies die worden beschreven in [implementeren Linux virtuele machines in Azure Stack](azure-stack-linux.md). Voordat u de installatiekopie uploadt, is het belangrijk dat u rekening houden met de volgende factoren:

   - Azure Stack biedt ondersteuning voor de vaste schijf VHD-indeling. Een vaste indeling structuren de logische schijf lineair binnen het bestand, zodat de schijf-offset X wordt opgeslagen op blob-offset X. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Om te bevestigen of de schijf is opgelost, gebruikt u de [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell-opdracht.  

    > [!IMPORTANT]
    >  Azure Stack biedt geen ondersteuning voor dynamische schijf VHD's. Formaat van een dynamische schijf die is gekoppeld aan een virtuele machine laat u de virtuele machine in een foutstatus. Risico's te beperken, door de virtuele machine te verwijderen zonder te verwijderen van de VM schijf, een VHD-blob in een storage-account. Het omzetten van de VHD van een dynamische schijf naar een vaste schijf en de virtuele machine opnieuw te maken.

   * Is het efficiënter een afbeelding uploaden naar Azure Stack blob-opslag dan naar Azure blob-opslag omdat kost het minder tijd aan de installatiekopie pushen naar de opslagplaats voor installatiekopieën van Azure Stack.

   * Wanneer u uploadt de [Windows VM-installatiekopie](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), zorg ervoor dat u vervangen door de **Meld u aan bij Azure** stap met de [configureren van de Azure Stack-operators PowerShell-omgeving](azure-stack-powershell-configure-admin.md) stap.  

   * Maak een notitie van de blob-opslag-URI waar u de installatiekopie uploadt. De URI van de blob-opslag heeft de volgende indeling: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* VHD.

   * Als u de blob anoniem toegankelijk is, gaat u naar de storage-account blob-container waarnaar de VHD van de VM-installatiekopie is geüpload. Selecteer **Blob**, en selecteer vervolgens **toegangsbeleid**. (Optioneel) kunt u in plaats daarvan een shared access signature voor de container genereren en opnemen als onderdeel van de blob-URI.

   ![Ga naar opslagaccountblobs](./media/azure-stack-add-vm-image/image1.png)

   ![Blob-toegang instellen op openbaar](./media/azure-stack-add-vm-image/image2.png)

2. Meld u met Azure Stack als operator. Selecteer in het menu **meer services**. Selecteer **Compute** > **VM-installatiekopieën** > **toevoegen**.

3. Onder **toevoegen van een VM-installatiekopie**, voer de uitgever, aanbieding, SKU en versie van de installatiekopie van de virtuele machine. Deze segmenten naam verwijzen naar de VM-installatiekopie in Resource Manager-sjablonen. Zorg ervoor dat u selecteert de **osType** correct-waarde. Voor **Blob-URI van OS-schijf**, voer de Blob-URI waarnaar de afbeelding is geüpload. Selecteer **maken** om te beginnen met het maken van de VM-installatiekopie.

   ![Begin met het maken van de installatiekopie](./media/azure-stack-add-vm-image/image4.png)

   Wanneer de installatiekopie is gemaakt, verandert de status van de installatiekopie van virtuele machine in **geslaagd**.

4. De installatiekopie van de virtuele machine meer gemakkelijk om beschikbaar te maken voor gebruik door gebruiker in de gebruikersinterface, is een goed idee om [een Marketplace-item maken](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Verwijderen van een VM-installatiekopie via de portal

1. Open het beheerportal op [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Selecteer **Marketplace management**, en selecteer vervolgens de virtuele machine die u wilt verwijderen.

3. Klik op **Verwijderen**.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>Een VM-installatiekopie toevoegen aan de Marketplace met behulp van PowerShell

> [!Note]  
> Als u toevoegt op basis van een installatiekopie van het is alleen beschikbaar voor Azure Resource Manager-sjablonen en PowerShell-implementaties. Om beschikbaar te maken van een installatiekopie van een uw gebruikers als een marketplace-item publiceren de marketplace-item met de stappen in het artikel [maken en publiceren van een Marketplace-item](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-and-publish-marketplace-item)

1. [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).  

2. Meld u met Azure Stack als een operator. Zie voor instructies [aanmelden bij Azure Stack als operator](azure-stack-powershell-configure-admin.md).

3. Open PowerShell met een opdrachtprompt en voer:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  De **toevoegen AzsPlatformimage** cmdlet Hiermee geeft u waarden die worden gebruikt door de Azure Resource Manager-sjablonen om te verwijzen naar de VM-installatiekopie. De waarden zijn:
  - **publisher**  
    Bijvoorbeeld: `Canonical`  
    Het segment van de naam van uitgever van de VM-installatiekopie die gebruikers gebruiken wanneer ze de installatiekopie implementeren. Een voorbeeld is **Microsoft**. Geen een spatie of andere speciale tekens in dit veld.  
  - **offer**  
    Bijvoorbeeld: `UbuntuServer`  
    Het segment van de naam van aanbieding van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeren. Een voorbeeld is **WindowsServer**. Geen een spatie of andere speciale tekens in dit veld.  
  - **SKU**  
    Bijvoorbeeld: `14.04.3-LTS`  
    De SKU-naam-segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeren. Een voorbeeld is **Datacenter2016**. Geen een spatie of andere speciale tekens in dit veld.  
  - **Versie**  
    Bijvoorbeeld: `1.0.0`  
    De versie van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeren. Deze versie is in de indeling  *\#.\#. \#*. Een voorbeeld is **1.0.0**. Geen een spatie of andere speciale tekens in dit veld.  
  - **besturingssysteemtype**  
    Bijvoorbeeld: `Linux`  
    Het besturingssysteemtype van de afbeelding moet een **Windows** of **Linux**.  
  - **OSUri**  
    Bijvoorbeeld: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    Kunt u een blob storage-URI voor een `osDisk`.  

    Zie voor meer informatie, de PowerShell-referentie voor de [toevoegen AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) cmdlet en de [New-DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) cmdlet.

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>Een aangepaste VM-installatiekopie toevoegen aan de Marketplace met behulp van PowerShell

1. [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).

  ```PowerShell  
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ```

2. Als u **Active Directory Federation Services**, gebruik de volgende cmdlet:

  ```PowerShell
  # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    ```

3. Meld u met Azure Stack als een operator. Zie voor instructies [aanmelden bij Azure Stack als operator](azure-stack-powershell-configure-admin.md).

4. Een storage-account maken in de globale Azure of Azure Stack voor het opslaan van uw aangepaste VM-installatiekopie. Zie voor instructies [Quickstart: blobs uploaden, downloaden, en lijst met behulp van de Azure-portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. Voorbereiden van de installatiekopie voor een Windows of Linux-besturingssysteem in VHD-indeling (de VHDX niet), de installatiekopie uploaden naar uw storage-account en de URI waar de VM-installatiekopie kan worden opgehaald door PowerShell.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (Optioneel) U kunt een matrix van gegevensschijven die als onderdeel van de VM-installatiekopie uploaden. De gegevensschijven van uw met behulp van de cmdlet New-DataDiskObject maken. Open PowerShell uit vanaf een opdrachtprompt en voer:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. Open PowerShell met een opdrachtprompt en voer:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Zie voor meer informatie over de cmdlet Add-AzsPlatformimage en de cmdlet New-DataDiskObject, de Microsoft PowerShell [module-documentatie voor Azure Stack-operators](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>Verwijderen van een VM-installatiekopie met behulp van PowerShell

Wanneer u de installatiekopie van de virtuele machine die u hebt geüpload niet langer nodig hebt, kunt u het verwijderen van de Marketplace met behulp van de volgende cmdlet:

1. [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).

2. Meld u met Azure Stack als een operator.

3. Open PowerShell met een opdrachtprompt en voer:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  De **Remove-AzsPlatformImage** cmdlet Hiermee geeft u waarden die worden gebruikt door de Azure Resource Manager-sjablonen om te verwijzen naar de VM-installatiekopie. De waarden zijn:
  - **publisher**  
    Bijvoorbeeld: `Canonical`  
    Het segment van de naam van uitgever van de VM-installatiekopie die gebruikers gebruiken wanneer ze de installatiekopie implementeren. Een voorbeeld is **Microsoft**. Geen een spatie of andere speciale tekens in dit veld.  
  - **offer**  
    Bijvoorbeeld: `UbuntuServer`  
    Het segment van de naam van aanbieding van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeren. Een voorbeeld is **WindowsServer**. Geen een spatie of andere speciale tekens in dit veld.  
  - **SKU**  
    Bijvoorbeeld: `14.04.3-LTS`  
    De SKU-naam-segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeren. Een voorbeeld is **Datacenter2016**. Geen een spatie of andere speciale tekens in dit veld.  
  - **Versie**  
    Bijvoorbeeld: `1.0.0`  
    De versie van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeren. Deze versie is in de indeling  *\#.\#. \#*. Een voorbeeld is **1.0.0**. Geen een spatie of andere speciale tekens in dit veld.  
    
    Zie voor meer informatie over de cmdlet Remove-AzsPlatformImage, de Microsoft PowerShell [module-documentatie voor Azure Stack-operators](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)
