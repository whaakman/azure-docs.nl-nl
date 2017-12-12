---
title: De standaardinstallatiekopie van de virtuele machine toevoegen aan de Stack Azure Marketplace | Microsoft Docs
description: De standaardinstallatiekopie van Windows Server 2016 VM toevoegen aan de Stack Azure Marketplace.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2849E53F-3D58-48A5-8007-3238FC39F630
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: mabrigg
ms.openlocfilehash: ed62f2f8441220eb37aea7f4c848702e9821698b
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>De Windows Server 2016 VM-installatiekopie toevoegen aan de Stack Azure Marketplace

Standaard zijn er geen installatiekopieën van virtuele machines beschikbaar in de Stack Azure Marketplace. Een Azure-Stack-operator moet een afbeelding toevoegen aan de Marketplace gebruikers toegang hebben. U kunt de installatiekopie van het Windows Server 2016 toevoegen aan de Stack Azure Marketplace met behulp van een van de volgende methoden:

* [De installatiekopie te downloaden vanuit Azure Marketplace](#add-the-image-by-downloading-it-from-the-azure-marketplace). Gebruik deze optie als u in een scenario voor verbonden werkt en u uw Azure-Stack-exemplaar bij Azure hebt geregistreerd.

* [De installatiekopie toevoegen met behulp van PowerShell](#add-the-image-by-using-powershell). Gebruik deze optie als u hebt Azure Stack geïmplementeerd in een niet-verbonden scenario of in scenario's met beperkte connectiviteit.

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>De installatiekopie toevoegen downloaden via Azure Marketplace

1. Azure-Stack implementeren en vervolgens weer aanmelden bij uw Azure-Stack Development Kit.

2. Selecteer **meer services** > **Marketplace Management** > **toevoegen uit Azure**. 

3. Zoeken of zoeken naar de **Windows Server 2016 Datacenter – Eval** installatiekopie en selecteer vervolgens **downloaden**.

   ![Afbeelding van Azure downloaden](media/azure-stack-add-default-image/download-image.png)

Wanneer het downloaden is voltooid, is de afbeelding onder **Marketplace Management**. De installatiekopie van het is ook beschikbaar onder **virtuele Machines**.

## <a name="add-the-image-by-using-powershell"></a>De installatiekopie toevoegen met behulp van PowerShell

### <a name="prerequisites"></a>Vereisten 

Voer de volgende vereisten uitvoeren vanuit de [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) of vanuit een Windows-externe client, bent u [verbonden via VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

1. Installeer [Azure Stack-compatibele Azure PowerShell-modules](azure-stack-powershell-install.md).  

2. Download de [hulpprogramma's voor het werken met Azure-Stack](azure-stack-powershell-download.md).  

3. Op de pagina Windows Server evaluaties [download de evaluatieversie van Windows Server 2016](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016). Wanneer u wordt gevraagd, selecteert u de ISO-versie van de download. Noteer het pad naar de downloadlocatie, die verderop in de stappen in dit artikel wordt gebruikt. Deze stap is verbinding met internet vereist.  

### <a name="add-the-image-to-the-azure-stack-marketplace"></a>De installatiekopie toevoegen aan de Stack Azure Marketplace
   
1. De Azure-Stack Connect en ComputeAdmin modules importeren met behulp van de volgende opdrachten:

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
     -EnvironmentName "AzureStackAdmin" 

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```
   
3. De installatiekopie van het Windows Server 2016 toevoegen aan de Stack Azure Marketplace. (Vervang *fully_qualified_path_to_ISO* met het pad naar de Windows Server 2016 ISO die u hebt gedownload.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

Om ervoor te zorgen dat de Windows Server 2016 VM-installatiekopie de meest recente cumulatieve update heeft, omvatten de `IncludeLatestCU` parameter tijdens het uitvoeren van de `New-AzsServer2016VMImage` cmdlet. Voor informatie over de toegestane parameters voor de `New-AzsServer2016VMImage` cmdlet, Zie [Parameters](#parameters). Het duurt ongeveer een uur voor het publiceren van de installatiekopie naar de Stack Azure Marketplace. 

## <a name="parameters"></a>Parameters

|Nieuwe AzsServer2016VMImage parameters|Vereist?|Beschrijving|
|-----|-----|------|
|ISOPath|Ja|Het volledig gekwalificeerde pad naar de gedownloade ISO van Windows Server 2016.|
|Net35|Nee|De runtime .NET 3.5 installeert op de installatiekopie van het Windows Server 2016. Deze waarde is standaard ingesteld op **true**.|
|Versie|Nee|Hiermee geeft u **Core**, **volledige**, of **beide** installatiekopieën van Windows Server 2016. Deze waarde is standaard ingesteld op **volledige**.|
|VHDSizeInMB|Nee|Hiermee stelt u de grootte (in MB) van de installatiekopie van het VHD moet worden toegevoegd aan uw Azure-Stack-omgeving. Deze waarde is standaard ingesteld op 40.960 MB.|
|CreateGalleryItem|Nee|Hiermee geeft u op of een Marketplace-item moet worden gemaakt voor de installatiekopie van Windows Server 2016. Deze waarde is standaard ingesteld op **true**.|
|location |Nee |Hiermee geeft u de locatie waarop de installatiekopie van het Windows Server 2016 moet worden gepubliceerd.|
|IncludeLatestCU|Nee|De meest recente cumulatieve update voor Windows Server 2016 geldt voor de nieuwe VHD.|
|CUUri |Nee |Stelt de Windows Server 2016 cumulatieve update uitvoeren vanaf een specifieke URI. |
|CUPath |Nee |De Windows Server 2016 sets cumulatieve update uitvoeren vanaf een lokaal pad. Deze optie is handig als u het Azure-Stack-exemplaar in een omgeving zonder verbinding hebt geïmplementeerd.|

## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine inrichten](azure-stack-provision-vm.md)
