---
title: Een Azure-Stack marketplace-item toevoegen van een lokale bron | Microsoft Docs
description: Beschrijft hoe een installatiekopie van het lokale besturingssysteem toevoegen aan de Stack Azure Marketplace.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 676dcdea5912aed5504c386f01d6809eccf763d4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-a-local-source"></a>Zelfstudie: een Azure-Stack marketplace-item van een lokale bron toevoegen

Als een Azure-Stack-Operator, wordt het eerste wat dat u moet uitvoeren om gebruikers te implementeren van een virtuele Machine (VM) is een VM-installatiekopie toevoegen aan de Stack van Azure marketplace. Niets wordt gepubliceerd op de Stack van Azure marketplace, maar u kunt uploaden VM ISO-installatiekopieën zodat deze beschikbaar aan uw gebruikers. Gebruik deze optie als u hebt Azure Stack geïmplementeerd in een niet-verbonden scenario of in scenario's met beperkte connectiviteit.

In deze zelfstudie maakt u een Windows Server 2016 VM-installatiekopie uit downloaden de Windows Server evaluaties pagina en upload het naar de Stack van Azure marketplace.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Windows Server 2016 VM-installatiekopie toe te voegen
> * Controleer of dat de VM-installatiekopie is beschikbaar 
> * Een installatiekopie van de virtuele machine testen

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer de [Azure Stack-compatibele Azure PowerShell-modules](asdk-post-deploy.md#install-azure-stack-powershell)
- Download de [Azure Stack-hulpprogramma's](asdk-post-deploy.md#download-the-azure-stack-tools)
- Download de [ISO-installatiekopie van Windows Server 2106 virtuele machine](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016) van de evaluatie van Windows Server-pagina

## <a name="add-a-windows-server-vm-image"></a>Een Windows Server-VM-installatiekopie toe te voegen
U kunt de installatiekopie van het Windows Server 2016 publiceren naar de Stack van Azure marketplace door een eerder gedownloade ISO-installatiekopie met behulp van PowerShell toe te voegen. 

Gebruik deze optie als u hebt Azure Stack geïmplementeerd in een niet-verbonden scenario of in scenario's met beperkte connectiviteit.

1. Importeren van de Azure-Stack `Connect` en `ComputeAdmin` PowerShell-modules die zijn opgenomen in de map van de Azure-Stack-hulpprogramma's met behulp van de volgende opdrachten:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. Voer een van de volgende scripts bij de computer van de host ASDK afhankelijk van of u uw Azure-Stack-omgeving hebt geïmplementeerd met behulp van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS):

  - Opdrachten voor **Azure AD-implementaties**: 

      ```PowerShell
      # To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "https://adminmanagement.local.azurestack.external"

      # To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "https://graph.windows.net/"
      
      # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
      Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" `
        -ArmEndpoint $ArmEndpoint

      Set-AzureRmEnvironment `
        -Name "AzureStackAdmin" `
        -GraphAudience $GraphAudience

      $TenantID = Get-AzsDirectoryTenantId `
      # Replace the AADTenantName value to reflect your Azure AD tenant name.
        -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
        -EnvironmentName AzureStackAdmin

      Add-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```

  - Opdrachten voor **AD FS-implementaties**:
      
      ```PowerShell
      # To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "https://adminmanagement.local.azurestack.external"

      # To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "https://graph.local.azurestack.external/"

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

      Add-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```
   
3. De installatiekopie van het Windows Server 2016 toevoegen aan de Stack van Azure marketplace. (Vervang *fully_qualified_path_to_ISO* met het pad naar de Windows Server 2016 ISO die u hebt gedownload.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

## <a name="verify-the-marketplace-item-is-available"></a>Controleer of dat het marketplace-item beschikbaar is
Volg deze stappen om te controleren of de nieuwe VM-installatiekopie is beschikbaar in de Stack van Azure marketplace.

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer **meer services** > **Marketplace Management**. 

3. Controleer of de installatiekopie van het Windows Server 2016 Datacenter VM zijn toegevoegd.

   ![Image is gedownload van Azure](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>Een installatiekopie van de virtuele machine testen
Als een Azure-Stack-operator, kunt u de [beheerdersportal](https://adminportal.local.azurestack.external) voor het maken van een test VM voor het valideren van de installatiekopie van het beschikbaar is gesteld is. 

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).

2. Klik op **nieuwe** > **Compute** > **Windows Server 2016 Datacenter** > **maken**.  
 ![Virtuele machine maken vanuit marketplace-installatiekopie](media/asdk-marketplace-item/new-compute.png)

3. In de **basisbeginselen** blade, voer de volgende informatie en klik vervolgens op **OK**:
  - **Naam**: test-vm-1
  - **Gebruikersnaam**: AdminTestUser
  - **Wachtwoord**: AzS TestVM01
  - **Abonnement**: Accepteer de standaard Provider-abonnement
  - **Resourcegroep**: test-vm-rg
  - **Locatie**: lokale

4. In de **een grootte kiezen** blade, klikt u op **A1 standaard**, en klik vervolgens op **Selecteer**.  

5. In de **instellingen** blade, accepteer de standaardinstellingen en klik op **OK**

6. In de blade **Samenvatting** klikt u op **OK** om de virtuele machine te maken.  
> [!NOTE]
> Implementatie van de virtuele machine duurt een paar minuten.

7. Als u wilt weergeven met de nieuwe virtuele machine **virtuele machines**, zoekt u naar **test-vm-1** en klik op de naam ervan.
    ![Eerste testinstallatiekopie voor de virtuele machine weergegeven](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Resources opschonen
Nadat u hebt aangemeld bij de virtuele machine, en geverifieerd dat de testafbeelding correct werkt, moet u de test-resourcegroep verwijderen. Hierdoor wordt beperkte bronnen beschikbaar zijn naar één knooppunt ASDK-installaties vrijgemaakt.

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep, VM en alle gerelateerde resources met de volgende stappen:

1. Aanmelden bij de [ASDK-beheerportal](https://adminportal.local.azurestack.external).
2. Klik op **resourcegroepen** > **test-vm-rg** > **resourcegroep verwijderen**.
3. Type **test-vm-rg** als de naam van een resourcegroep en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Windows Server 2016 VM-installatiekopie toe te voegen
> * Controleer of dat de VM-installatiekopie is beschikbaar 
> * Een installatiekopie van de virtuele machine testen

Ga naar de volgende zelfstudie voor meer informatie over het maken van een Azure-Stack aanbieding en plannen.

> [!div class="nextstepaction"]
> [Azure-Stack IaaS-services bieden](asdk-offer-services.md)
