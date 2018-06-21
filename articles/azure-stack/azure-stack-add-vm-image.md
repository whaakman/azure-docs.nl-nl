---
title: Toevoegen en verwijderen van een VM-installatiekopie naar Azure Stack | Microsoft Docs
description: Toevoegen of verwijderen van uw organisatie aangepaste Windows of Linux-VM-installatiekopie voor tenants kunnen gebruiken.
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
ms.date: 05/24/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 714afa1da5d2c8c5695dfe33edd0257f69af149d
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287701"
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>De installatiekopie van een virtuele machine in Azure Stack beschikbaar maken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

In Azure-Stack, kunt u installatiekopieën van virtuele machines beschikbaar aan uw gebruikers. Deze installatiekopieën kunnen worden verwezen vanuit de Azure Resource Manager-sjablonen of u ze kunt toevoegen aan de Azure Marketplace-gebruikersinterface als een Marketplace-item. U kunt ofwel een formulier installatiekopie globale Azure Marketplace gebruiken of uw eigen aangepaste installatiekopie toe te voegen. U kunt een virtuele machine via de portal of de Windows PowerShell kunt toevoegen.

## <a name="add-a-vm-image-through-the-portal"></a>Toevoegen van een VM-installatiekopie via de portal

> [!NOTE]
> Met deze methode, moet u het Marketplace-item afzonderlijk maken.

Installatiekopieën moet kunnen worden verwezen door een blob storage-URI. De installatiekopie voor een Windows- of Linux-besturingssysteem in VHD-indeling (geen VHDX) voorbereiden en vervolgens de installatiekopie uploadt naar een opslagaccount in Azure of Azure-Stack. Als uw installatiekopie is al geüpload naar de blob-opslag in Azure of Azure-Stack, kunt u stap 1 overslaan.

1. [Een virtuele machine van Windows-installatiekopie uploaden naar Azure voor implementaties van Resource Manager](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) of voor een Linux-installatiekopie, volgt u de instructies die worden beschreven [implementeren Linux virtuele machines op Azure-Stack](azure-stack-linux.md). Voordat u de installatiekopie uploadt, is het belangrijk in de volgende factoren:

   - Azure-Stack biedt ondersteuning voor de vaste schijf VHD-indeling. De vaste indeling structuren de logische schijf lineair binnen het bestand, zodat de verschuiving van die schijf X wordt opgeslagen op de X-verschuiving van de blob. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Om te controleren of de schijf is opgelost, gebruikt u de [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell-opdracht.  

    > [!IMPORTANT]
    >  Azure-Stack biedt geen ondersteuning voor dynamische schijf VHD's. Formaat van een dynamische schijf die is gekoppeld aan een virtuele machine laat u de virtuele machine in een foutstatus. Om dit probleem beperken, verwijdert u de virtuele machine zonder te verwijderen van de VM-schijf, een VHD-blob in een opslagaccount. Het converteren van de VHD van een dynamische schijf naar een vaste schijf en de virtuele machine opnieuw te maken.

   * Het is efficiënter een installatiekopie uploaden naar de Stack Azure blob-opslag dan naar Azure blob-opslag omdat kost het minder tijd om de installatiekopie naar de Stack van Azure-opslagplaats voor installatiekopieën.

   * Wanneer u uploadt de [Windows VM-installatiekopie](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), Vervang door de **aanmelden bij Azure** stap met het [configureren van de Azure-Stack-operator PowerShell-omgeving](azure-stack-powershell-configure-admin.md) stap.  

   * Noteer de URI waar u de installatiekopie van het uploaden van blob-opslag. De blob storage-URI heeft de volgende indeling: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* VHD.

   * U kunt de blob anoniem toegankelijk maken, gaat u naar de storage-account blob-container waar de VHD van de VM-installatiekopie is geüpload. Selecteer **Blob**, en selecteer vervolgens **toegangsbeleid**. Eventueel, kunt u in plaats daarvan een shared access signature voor de container genereren en opnemen als onderdeel van de blob-URI.

   ![Ga naar de storage-account blobs](./media/azure-stack-add-vm-image/image1.png)

   ![Set blob toegang tot openbare](./media/azure-stack-add-vm-image/image2.png)

2. Aanmelden bij Azure Stack als operator. Selecteer in het menu **meer services** > **Resourceproviders**. Selecteer **Compute** > **VM-installatiekopieën** > **toevoegen**.

3. Onder **een VM-installatiekopie toe te voegen**, voer de uitgever, aanbieding, SKU en versie van de installatiekopie van de virtuele machine. Deze segmenten naam verwijzen naar de VM-installatiekopie in de Resource Manager-sjablonen. Zorg ervoor dat u selecteert de **besturingssysteemtype** correct waarde. Voor **OS schijf Blob-URI**, voer de Blob-URI waar de installatiekopie is geüpload. Selecteer **maken** om te beginnen met het maken van de VM-installatiekopie.

   ![Begin met het maken van de installatiekopie](./media/azure-stack-add-vm-image/image4.png)

   Wanneer de installatiekopie is gemaakt, verandert de status van de installatiekopie van virtuele machine in **geslaagd**.

4. Als u de installatiekopie van de virtuele machine meer gemakkelijk beschikbaar voor consumptie van de gebruiker in de gebruikersinterface, is een goed idee om [een Marketplace-item maken](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Verwijderen van een VM-installatiekopie via de portal

1. Open de beheerportal op [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Selecteer **Marketplace management**, en selecteer vervolgens de virtuele machine die u wilt verwijderen.

3. Klik op **Verwijderen**.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>Een VM-installatiekopie toevoegen aan de Marketplace met behulp van PowerShell

1. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).  

2. Aanmelden bij Azure Stack als operator. Zie voor instructies [aanmelden bij Azure Stack als operator](azure-stack-powershell-configure-admin.md).

3. Open PowerShell met een opdrachtprompt en voer:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  De **toevoegen AzsPlatformimage** cmdlet geeft de waarden die worden gebruikt door de Azure Resource Manager-sjablonen om te verwijzen naar de VM-installatiekopie. Mogelijke waarden zijn:
  - **publisher**  
    Bijvoorbeeld: `Canonical`  
    Het segment van de naam van uitgever van de VM-installatiekopie die gebruikers gebruiken wanneer ze de installatiekopie implementeren. Een voorbeeld is **Microsoft**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **offer**  
    Bijvoorbeeld: `UbuntuServer`  
    De aanbieding naam segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Een voorbeeld is **Windows Server**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **SKU**  
    Bijvoorbeeld: `14.04.3-LTS`  
    De SKU-naam-segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Een voorbeeld is **Datacenter2016**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **Versie**  
    Bijvoorbeeld: `1.0.0`  
    De versie van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Deze versie is in de notatie  *\#.\#. \#*. Een voorbeeld is **1.0.0**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **besturingssysteemtype**  
    Bijvoorbeeld: `Linux`  
    Het besturingssysteemtype van de afbeelding moet een **Windows** of **Linux**.  
  - **OSUri**  
    Bijvoorbeeld: `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    U kunt een blob storage-URI opgeven voor een `osDisk`.  

    Zie voor meer informatie de PowerShell-referentie voor de [toevoegen AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) cmdlet en de [nieuw DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) cmdlet.

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>Een aangepaste installatiekopie van de virtuele machine toevoegen aan de Marketplace met behulp van PowerShell

1. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).

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

3. Aanmelden bij Azure Stack als operator. Zie voor instructies [aanmelden bij Azure Stack als operator](azure-stack-powershell-configure-admin.md).

4. Een opslagaccount maken in globale Azure of Azure-Stack voor het opslaan van uw aangepaste VM-installatiekopie. Zie voor instructies [Snelstartgids: uploaden, downloaden en lijst blobs met Azure portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. De installatiekopie voor een Windows- of Linux-besturingssysteem in VHD-indeling (geen VHDX) voorbereiden, de installatiekopie uploaden naar uw storage-account en het verkrijgen van de URI waar de VM-installatiekopie kan worden opgehaald door PowerShell.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (Optioneel) U kunt een matrix van gegevensschijven als onderdeel van de VM-installatiekopie uploaden. De gegevensschijven van uw met de cmdlet New-DataDiskObject maken. PowerShell openen vanaf een opdrachtprompt en voer:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. Open PowerShell met een opdrachtprompt en voer:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Zie voor meer informatie over de cmdlet Add-AzsPlatformimage en de cmdlet New-DataDiskObject de Microsoft PowerShell [Azure Stack Operator module documentatie](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>Een VM-installatiekopie verwijderen met behulp van PowerShell

Wanneer u de installatiekopie van de virtuele machine die u hebt geüpload niet meer nodig hebt, kunt u het verwijderen van de Marketplace met behulp van de volgende cmdlet:

1. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).

2. Aanmelden bij Azure Stack als operator.

3. Open PowerShell met een opdrachtprompt en voer:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  De **verwijderen AzsPlatformImage** cmdlet geeft de waarden die worden gebruikt door de Azure Resource Manager-sjablonen om te verwijzen naar de VM-installatiekopie. Mogelijke waarden zijn:
  - **publisher**  
    Bijvoorbeeld: `Canonical`  
    Het segment van de naam van uitgever van de VM-installatiekopie die gebruikers gebruiken wanneer ze de installatiekopie implementeren. Een voorbeeld is **Microsoft**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **offer**  
    Bijvoorbeeld: `UbuntuServer`  
    De aanbieding naam segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Een voorbeeld is **Windows Server**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **SKU**  
    Bijvoorbeeld: `14.04.3-LTS`  
    De SKU-naam-segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Een voorbeeld is **Datacenter2016**. Neem geen een spatie of andere speciale tekens in dit veld.  
  - **Versie**  
    Bijvoorbeeld: `1.0.0`  
    De versie van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Deze versie is in de notatie  *\#.\#. \#*. Een voorbeeld is **1.0.0**. Neem geen een spatie of andere speciale tekens in dit veld.  
    
    Zie voor meer informatie over de cmdlet Remove-AzsPlatformImage de Microsoft PowerShell [Azure Stack Operator module documentatie](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)
