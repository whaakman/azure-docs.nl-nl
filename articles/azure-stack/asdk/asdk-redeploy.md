---
title: Opnieuw implementeren van de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: In dit artikel leert u hoe u de ASDK opnieuw moet installeren.
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
ms.topic: article
ms.custom: ''
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: ecdfb0f062ebad903d35401a2f5303d3e6d04b5d
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037376"
---
# <a name="redeploy-the-asdk"></a>De ASDK opnieuw implementeren
In dit artikel leert u hoe u de Azure Stack Development Kit (ASDK) in een niet-productieomgeving opnieuw implementeren. Omdat de ASDK upgraden wordt niet ondersteund, moet u helemaal opnieuw te implementeren om te verplaatsen naar een nieuwere versie. U kunt ook de ASDK herimplementatie op elk moment dat u alleen wilt helemaal opnieuw beginnen.

> [!IMPORTANT]
> De ASDK upgraden naar een nieuwe versie wordt niet ondersteund. U moet de ASDK op de hostcomputer development kit telkens wanneer die u wilt evalueren van een nieuwere versie van Azure Stack implementeren.

## <a name="remove-azure-registration"></a>Verwijderen van registratie van Azure 
Als u de installatie van de ASDK eerder met Azure hebt geregistreerd, moet u de registratie-resource verwijderen voordat u de ASDK opnieuw te implementeren. Registreer opnieuw de ASDK om in te schakelen van de beschikbaarheid van de items in de marketplace wanneer u de ASDK opnieuw implementeert. Als u eerder hebt niet de ASDK hebt geregistreerd bij uw Azure-abonnement, kunt u deze sectie overslaan.

Als u wilt verwijderen van de registratie-resource, gebruikt u de **Remove-AzsRegistration** cmdlet registratie van Azure Stack. Vervolgens gebruikt u de **Remove-AzureRMRsourceGroup** cmdlet voor het verwijderen van de Azure Stack-resourcegroep van uw Azure-abonnement:

1. Open een PowerShell-console als beheerder op een computer die toegang tot het eindpunt van de bevoegdheden heeft. Voor de ASDK is dat de hostcomputer development kit.

2. Voer de volgende PowerShell-opdrachten voor de installatie van de ASDK de registratie ongedaan maken en verwijderen van de **azurestack** resourcegroep van uw Azure-abonnement:

  ```Powershell    
  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Provide Azure subscription admin credentials
  Add-AzureRmAccount

  # Provide ASDK admin credentials
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"

  # Unregister Azure Stack
  Remove-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01

  # Remove the Azure Stack resource group
  Remove-AzureRmResourceGroup -Name azurestack -Force
  ```

3. U wordt gevraagd of u zich aanmeldt bij uw Azure-abonnement zowel de lokale installatie van de ASDK wanneer het script wordt uitgevoerd.
4. Wanneer het script is voltooid, ziet u berichten die vergelijkbaar is met de volgende voorbeelden:

    ` De-Activating Azure Stack (this may take up to 10 minutes to complete).` ` Your environment is now unable to syndicate items and is no longer reporting usage data.`
    ` Remove registration resource from Azure...`
    ` "Deleting the resource..." on target "/subscriptions/<subscription information>"`
    ` ********** End Log: Remove-AzsRegistration ********* `



Azure Stack is worden nu bij uw Azure-abonnement ongedaan gemaakt. Bovendien moet de resourcegroep azurestack gemaakt wanneer u de ASDK geregistreerd bij Azure, ook worden verwijderd.

## <a name="deploy-the-asdk"></a>De ASDK implementeren
Als u wilt implementeren in Azure Stack, moet u starten via helemaal zoals hieronder wordt beschreven. De stappen zijn verschillend, afhankelijk van of u het installatiescript voor Azure Stack (asdk-installer.ps1) gebruikt voor het installeren van de ASDK.

### <a name="redeploy-the-asdk-using-the-installer-script"></a>De ASDK met behulp van het installatiescript opnieuw implementeren
1. Op de computer ASDK, open een PowerShell-console met verhoogde bevoegdheid en navigeer naar het script asdk installer.ps1 in de **AzureStack_Installer** directory zich op een niet-systeemstation. Voer het script uit en klikt u op **opnieuw opstarten**.

   ![Voer het script asdk installer.ps1](media/asdk-redeploy/1.png)

2. Selecteer het basisbesturingssysteem (niet **Azure Stack**) en klikt u op **volgende**.

   ![Starten met het hostbesturingssysteem](media/asdk-redeploy/2.png)

3. Nadat de development kit host opnieuw is opgestart in het besturingssysteem, moet u zich aanmelden als een lokale beheerder zijn. Zoek en verwijder de **C:\CloudBuilder.vhdx** -bestand dat is gebruikt als onderdeel van de vorige implementatie. 

4. Herhaal de stappen die u hebt gemaakt naar eerste [implementeren de ASDK](asdk-install.md).

### <a name="redeploy-the-asdk-without-using-the-installer"></a>De ASDK implementeren zonder gebruik van het installatieprogramma
Als u het script asdk installer.ps1 niet hebt gebruikt voor het installeren van de ASDK, moet u handmatig opnieuw configureren de hostcomputer development kit voordat de ASDK opnieuw te implementeren.

1. Start de systeemconfiguratie door te voeren **msconfig.exe** op de computer ASDK. Op de **Boot** tabblad, selecteert u het besturingssysteem van de host-computer (niet Azure Stack), klikt u op **ingesteld als standaard**, en klik vervolgens op **OK**. Klik op **opnieuw** wanneer hierom wordt gevraagd.

      ![Configuratie van de opstartinstallatiekopie instellen](media/asdk-redeploy/4.png)

2. Nadat de development kit host opnieuw is opgestart in het besturingssysteem, moet u zich aanmelden als een lokale beheerder voor de hostcomputer development kit. Zoek en verwijder de **C:\CloudBuilder.vhdx** -bestand dat is gebruikt als onderdeel van de vorige implementatie. 

3. Herhaal de stappen die u hebt gemaakt naar eerste [implementeren de ASDK met behulp van PowerShell](asdk-deploy-powershell.md).


## <a name="next-steps"></a>Volgende stappen
[Boeken ASDK implementatietaken](asdk-post-deploy.md)




