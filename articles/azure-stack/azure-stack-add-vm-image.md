---
title: Een VM-installatiekopie toevoegen aan Azure-Stack | Microsoft Docs
description: Toevoegen van uw organisatie aangepaste Windows of Linux-VM-installatiekopie voor tenants kunnen gebruiken.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/17/2018
ms.author: mabrigg
ms.openlocfilehash: 0ba0bc4e8350a65a95dc41788c93d5c89fc48334
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="make-a-custom-virtual-machine-image-available-in-azure-stack"></a>De installatiekopie van een aangepaste virtuele machine in Azure Stack beschikbaar maken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

In Azure-Stack kunnen operators aangepaste virtuele machine installatiekopieën beschikbaar maken voor gebruikers. Deze installatiekopieën kunnen worden verwezen vanuit de Azure Resource Manager-sjablonen of u ze kunt toevoegen aan de Azure Marketplace-gebruikersinterface als een Marketplace-item.

## <a name="add-a-vm-image-to-marketplace-by-using-powershell"></a>Een VM-installatiekopie in Marketplace toevoegen met behulp van PowerShell

Voer de volgende vereisten uitvoeren vanuit de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) of vanuit een Windows-externe client, bent u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

1. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).  

2. Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  

3. Voorbereiden van een Windows- of Linux-besturingssysteeminstallatiekopie virtuele harde schijf in VHD-indeling (gebruik niet VHDX-indeling).

   * Voor Windows-installatiekopieën voor instructies over het voorbereiden van de afbeelding Zie [een virtuele machine van Windows-installatiekopie uploaden naar Azure voor implementaties van Resource Manager](../virtual-machines/windows/upload-generalized-managed.md).

   * Zie voor Linux-installatiekopieën, [implementeren Linux virtuele machines op Azure-Stack](azure-stack-linux.md). Voltooi de stappen voor het voorbereiden van de afbeelding of gebruiken van een installatiekopie van een bestaande Azure Stack Linux zoals beschreven in het artikel.    

   Azure-Stack biedt ondersteuning voor de vaste schijf VHD-indeling. De vaste indeling structuren de logische schijf lineair binnen het bestand, zodat de verschuiving van die schijf X wordt opgeslagen op de X-verschuiving van de blob. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Om te controleren of de schijf is opgelost, gebruikt u de [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell-opdracht.  

   > [!IMPORTANT]
   >  Azure-Stack biedt geen ondersteuning voor dynamische schijf VHD's. Formaat van een dynamische schijf die is gekoppeld aan een virtuele machine laat u de virtuele machine in een foutstatus. Om dit probleem beperken, verwijdert u de virtuele machine zonder te verwijderen van de VM-schijf, een VHD-blob in een opslagaccount. Vervolgens de VHD van een dynamische schijf converteren naar een vaste schijf en de virtuele machine opnieuw te maken.

Als u wilt de installatiekopie toevoegen aan de Stack Azure Marketplace, moet u de volgende stappen uitvoeren:

1. Importeer de modules Connect en ComputeAdmin:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1
   ```

2. Aanmelden bij uw Azure-Stack-omgeving. Voer een van de volgende scripts, afhankelijk van of u uw Azure-Stack-omgeving hebt geïmplementeerd met behulp van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS). (De Azure AD vervangen `tenantName`, `GraphAudience` eindpunt, en `ArmEndpoint` waarden in overeenstemming met de configuratie van uw omgeving.)

    * **Azure Active Directory**. Gebruik de volgende cmdlet:

      ```PowerShell
      # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "<Resource Manager endpoint for your environment>"

      # For Azure Stack Development Kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "<GraphAuidence endpoint for your environment>"

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

      Login-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID
      ```

   * **Active Directory Federatieservices**. Gebruik de volgende cmdlet:

        ```PowerShell
        # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
        $ArmEndpoint = "<Resource Manager endpoint for your environment>"

        # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
        $GraphAudience = "<GraphAuidence endpoint for your environment>"

        # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
        Add-AzureRMEnvironment `
          -Name "AzureStackAdmin" `
          -ArmEndpoint $ArmEndpoint

        Set-AzureRmEnvironment `
          -Name "AzureStackAdmin" `
          -GraphAudience $GraphAudience `
          -EnableAdfsAuthentication:$true

        $TenantID = Get-AzsDirectoryTenantId `
          -ADFS `
          -EnvironmentName AzureStackAdmin

        Login-AzureRmAccount `
          -EnvironmentName "AzureStackAdmin" `
          -TenantId $TenantID
        ```

3. De VM-installatiekopie toevoegen door het aanroepen van de `Add-AzsVMImage` cmdlet. In de `Add-AzsVMImage` cmdlet, geef `osType` als Windows of Linux. De uitgever, aanbieding, SKU en versie voor de VM-installatiekopie opnemen. Zie voor meer informatie over de toegestane parameters [Parameters](#parameters). De parameters worden gebruikt door Azure Resource Manager-sjablonen om te verwijzen naar de VM-installatiekopie. Het volgende voorbeeld wordt het script:

  ```powershell
  Add-AzsVMImage `
    -publisher "Canonical" `
    -offer "UbuntuServer" `
    -sku "14.04.3-LTS" `
    -version "1.0.0" `
    -osType Linux `
    -osDiskLocalPath 'C:\Users\AzureStackAdmin\Desktop\UbuntuServer.vhd' `
  ```

De opdracht doet het volgende:

* Wordt geverifieerd op de Stack van Azure-omgeving.
* De lokale VHD naar een nieuwe tijdelijke opslagaccount geüpload.
* Voegt de VM-installatiekopie toe aan de opslagplaats voor VM-installatiekopieën.
* Maakt een Marketplace-item.

Om te controleren of de opdracht is geslaagd, in de portal, gaat u naar de Marketplace. Controleer of de VM-installatiekopie is beschikbaar in de **virtuele Machines** categorie.

![VM-installatiekopie is toegevoegd](./media/azure-stack-add-vm-image/image5.PNG)

## <a name="remove-a-vm-image-by-using-powershell"></a>Een VM-installatiekopie verwijderen met behulp van PowerShell

Wanneer u de installatiekopie van de virtuele machine die u hebt geüpload niet meer nodig hebt, kunt u het verwijderen van de Marketplace met behulp van de volgende cmdlet:

```powershell
Remove-AzsVMImage `
  -publisher "Canonical" `
  -offer "UbuntuServer" `
  -sku "14.04.3-LTS" `
  -version "1.0.0" `
```

## <a name="parameters"></a>Parameters

| Parameter | Beschrijving |
| --- | --- |
| **publisher** |Het segment van de naam van uitgever van de VM-installatiekopie die gebruikers gebruiken wanneer ze de installatiekopie implementeren. Een voorbeeld is **Microsoft**. Neem geen een spatie of andere speciale tekens in dit veld. |
| **offer** |De aanbieding naam segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Een voorbeeld is **Windows Server**. Neem geen een spatie of andere speciale tekens in dit veld. |
| **sku** |De SKU-naam-segment van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Een voorbeeld is **Datacenter2016**. Neem geen een spatie of andere speciale tekens in dit veld. |
| **Versie** |De versie van de VM-installatiekopie die gebruikers gebruiken wanneer ze de VM-installatiekopie implementeert. Deze versie is in de notatie  *\#.\#. \#*. Een voorbeeld is **1.0.0**. Neem geen een spatie of andere speciale tekens in dit veld. |
| **osType** |Het besturingssysteemtype van de afbeelding moet een **Windows** of **Linux**. |
| **osDiskLocalPath** |Het lokale pad naar de schijf met het besturingssysteem VHD die u als een VM-installatiekopie naar Azure-Stack uploaden wilt. |
| **dataDiskLocalPaths** |Een optionele matrix van de lokale paden voor gegevensschijven dat als onderdeel van de VM-installatiekopie kunnen worden geüpload. |
| **CreateGalleryItem** |Een Booleaanse vlag waarmee wordt bepaald of het maken van een item in de Marketplace. Standaard is ingesteld op **true**. |
| **titel** |De weergavenaam van de Marketplace-item. Standaard is ingesteld op de `Publisher-Offer-Sku` waarde van de VM-installatiekopie. |
| **Beschrijving** |De beschrijving van de Marketplace-item. |
| **location** |De locatie waar de installatiekopie van de virtuele machine moet worden gepubliceerd. Deze waarde is standaard ingesteld op **lokale**.|
| **osDiskBlobURI** |(Optioneel) Dit script accepteert ook een Blob storage-URI voor `osDisk`. |
| **dataDiskBlobURIs** |(Optioneel) Dit script accepteert ook een matrix van URI's van de Blob-opslag voor gegevensschijven toevoegen aan de installatiekopie. |

## <a name="add-a-vm-image-through-the-portal"></a>Toevoegen van een VM-installatiekopie via de portal

> [!NOTE]
> Met deze methode, moet u het Marketplace-item afzonderlijk maken.

Installatiekopieën moet kunnen worden verwezen door een Blob storage-URI. De installatiekopie voor een Windows- of Linux-besturingssysteem in VHD-indeling (geen VHDX) voorbereiden en vervolgens de installatiekopie uploadt naar een opslagaccount in Azure of Azure-Stack. Als uw installatiekopie is al geüpload naar de Blob-opslag in Azure of Azure-Stack, kunt u stap 1 overslaan.

1. [Een virtuele machine van Windows-installatiekopie uploaden naar Azure voor implementaties van Resource Manager](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) of voor een Linux-installatiekopie, volgt u de instructies die worden beschreven [implementeren Linux virtuele machines op Azure-Stack](azure-stack-linux.md). Voordat u de installatiekopie uploadt, is het belangrijk in de volgende factoren:

   * Azure-Stack biedt ondersteuning voor de vaste schijf VHD-indeling. De vaste indeling structuren de logische schijf lineair binnen het bestand, zodat de verschuiving van die schijf X wordt opgeslagen op de X-verschuiving van de blob. Een kleine voettekst aan het einde van de blob beschrijft de eigenschappen van de VHD. Om te controleren of de schijf is opgelost, gebruikt u de [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell-opdracht.  

    > [!IMPORTANT]
   >  Azure-Stack biedt geen ondersteuning voor dynamische schijf VHD's. Formaat van een dynamische schijf die is gekoppeld aan een virtuele machine laat u de virtuele machine in een foutstatus. Om dit probleem beperken, verwijdert u de virtuele machine zonder te verwijderen van de VM-schijf, een VHD-blob in een opslagaccount. Het converteren van de VHD van een dynamische schijf naar een vaste schijf en de virtuele machine opnieuw te maken.

   * Het is efficiënter een installatiekopie uploaden naar Azure Stack Blob-opslag dan naar Azure Blob-opslag omdat kost het minder tijd om de installatiekopie naar de Stack van Azure-opslagplaats voor installatiekopieën.

   * Wanneer u uploadt de [Windows VM-installatiekopie](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), Vervang door de **aanmelden bij Azure** stap met het [configureren van de Azure-Stack-operator PowerShell-omgeving](azure-stack-powershell-configure-admin.md) stap.  

   * Noteer de URI waar u de installatiekopie van het uploaden van Blob-opslag. De Blob storage-URI heeft de volgende indeling:  *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* VHD.

   * U kunt de blob anoniem toegankelijk maken, gaat u naar de storage-account blob-container waar de VHD van de VM-installatiekopie is geüpload. Selecteer **Blob**, en selecteer vervolgens **toegangsbeleid**. Eventueel, kunt u in plaats daarvan een shared access signature voor de container genereren en opnemen als onderdeel van de blob-URI.

   ![Ga naar de storage-account blobs](./media/azure-stack-add-vm-image/image1.png)

   ![Set blob toegang tot openbare](./media/azure-stack-add-vm-image/image2.png)

2. Aanmelden bij Azure Stack als operator. Selecteer in het menu **meer services** > **Resourceproviders**. Selecteer **Compute** > **VM-installatiekopieën** > **toevoegen**.

3. Onder **een VM-installatiekopie toe te voegen**, voer de uitgever, aanbieding, SKU en versie van de installatiekopie van de virtuele machine. Deze segmenten naam verwijzen naar de VM-installatiekopie in de Resource Manager-sjablonen. Zorg ervoor dat u selecteert de **besturingssysteemtype** correct waarde. Voor **OS schijf Blob-URI**, voer de Blob-URI waar de installatiekopie is geüpload. Selecteer **maken** om te beginnen met het maken van de VM-installatiekopie.

   ![Begin met het maken van de installatiekopie](./media/azure-stack-add-vm-image/image4.png)

   Wanneer de installatiekopie is gemaakt, verandert de status van de installatiekopie van virtuele machine in **geslaagd**.

4. Als u de installatiekopie van de virtuele machine meer gemakkelijk beschikbaar voor consumptie van de gebruiker in de gebruikersinterface, is een goed idee om [een Marketplace-item maken](azure-stack-create-and-publish-marketplace-item.md).

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)
