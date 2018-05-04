---
title: Implementeren van de Azure-Stack Development Kit (ASDK) | Microsoft Docs
description: In deze zelfstudie leert u hoe de ASDK opnieuw installeren.
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
ms.openlocfilehash: fcf1abfe574dd3067f00df7c5ff2632b9cc2ec4f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-redeploy-the-asdk"></a>Zelfstudie: de ASDK opnieuw implementeren
In deze zelfstudie leert u hoe de Azure Stack Development Kit (ASDK) in een niet-productieomgeving implementeren. Omdat de ASDK bijwerken wordt niet ondersteund, moet u volledig opnieuw te implementeren om te verplaatsen naar een nieuwere versie. U kunt ook de ASDK implementeren op elk gewenst moment die u wilt beginnen vanaf het begin.

> [!IMPORTANT]
> De ASDK upgraden naar een nieuwe versie wordt niet ondersteund. U moet de ASDK op de hostcomputer van development kit elke keer dat u wilt evalueren van een nieuwere versie van Azure-Stack implementeren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure registratie verwijderen 
> * De ASDK implementeren

## <a name="remove-azure-registration"></a>Azure registratie verwijderen 
Als u uw installatie ASDK eerder bij Azure hebt geregistreerd, moet u de registratie-bron verwijderen voordat de ASDK opnieuw distribueren. Registreer opnieuw de ASDK zodat marketplace syndication wanneer u de ASDK opnieuw implementeert. Als u eerder hebt niet de ASDK hebt geregistreerd bij uw Azure-abonnement, kunt u deze sectie overslaan.

Gebruiken om de registratie-resource verwijdert, de **verwijderen AzsRegistration** cmdlet registratie van de Azure-Stack. Gebruik vervolgens de **verwijderen AzureRMRsourceGroup** cmdlet de Stack van Azure-resourcegroep verwijderen uit uw Azure-abonnement:

1. Open een PowerShell-console als beheerder op een computer die toegang tot de bevoegde eindpunt heeft. Voor de ASDK is dat de hostcomputer development kit.

2. Voer de volgende PowerShell-opdrachten voor registratie van uw installatie ASDK en verwijderen van de **azurestack** resourcegroep van uw Azure-abonnement:

  ```Powershell    
  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Provide Azure subscription admin credentials
  Add-AzureRmAccount

  # Provide ASDK admin credentials
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"

  # Unregister Azure Stack
  Remove-AzsRegistration `
      -CloudAdminCredential $YourCloudAdminCredential `
      -PrivilegedEndpoint AzS-ERCS01

  # Remove the Azure Stack resource group
  Remove-AzureRmResourceGroup -Name azurestack -Force
  ```

3. U wordt gevraagd aan te melden bij uw Azure-abonnement zowel de lokale installatie van de ASDK wanneer het script wordt uitgevoerd.
4. Wanneer het script is voltooid, ziet u berichten vergelijkbaar met de volgende voorbeelden:

    ` De-Activating Azure Stack (this may take up to 10 minutes to complete).` ` Your environment is now unable to syndicate items and is no longer reporting usage data.` ` Remove registration resource from Azure...` ` "Deleting the resource..." on target "/subscriptions/<subscription information>"` ` ********** End Log: Remove-AzsRegistration ********* `



Azure-Stack moet nu met succes worden verwijderd van uw Azure-abonnement. Bovendien moet de resourcegroep azurestack gemaakt wanneer u de ASDK geregistreerd bij Azure, ook worden verwijderd.

## <a name="redeploy-the-asdk"></a>De ASDK implementeren
Als u wilt implementeren in Azure-Stack, moet u starten via helemaal zoals hieronder wordt beschreven. De stappen zijn verschillend, afhankelijk van of u het installatiescript voor Azure-Stack (asdk installer.ps1) gebruikt voor het installeren van de ASDK.

### <a name="redeploy-the-asdk-using-the-installer-script"></a>De ASDK met het installatiescript implementeren
1. Op de computer ASDK, open een PowerShell-console met verhoogde bevoegdheid en navigeer naar het script asdk installer.ps1 in de **AzureStack_Installer** directory zich op een niet-systeemstation. Voer het script en klik op **opnieuw opstarten**.

   ![Voer het script asdk installer.ps1](media/asdk-redeploy/1.png)

2. Selecteer het basisbesturingssysteem (geen **Azure Stack**) en klik op **volgende**.

   ![Start op met de host-besturingssysteem](media/asdk-redeploy/2.png)

3. Nadat de development kit host opnieuw is opgestart in het basisbesturingssysteem, meld u aan als een lokale beheerder. Zoek en verwijder de **C:\CloudBuilder.vhdx** -bestand dat is gebruikt als onderdeel van de vorige implementatie. 

4. Herhaal de stappen die u hebt uitgevoerd naar eerste [implementeert de ASDK](asdk-deploy.md).

### <a name="redeploy-the-asdk-without-using-the-installer"></a>De ASDK implementeren zonder gebruik van het installatieprogramma
Als u het script asdk installer.ps1 niet gebruikt voor het installeren van de ASDK, moet u de hostcomputer development kit handmatig configureren voordat de ASDK opnieuw distribueren.

1. Start de systeemconfiguratie met **msconfig.exe** op de computer ASDK. Op de **Boot** tabblad het host-besturingssysteem (geen Azure-Stack), klik op **ingesteld als standaard**, en klik vervolgens op **OK**. Klik op **opnieuw** wanneer u wordt gevraagd.

      ![Stel de configuratie van de opstartinstallatiekopie](media/asdk-redeploy/4.png)

2. Nadat de development kit host opnieuw is opgestart in het basisbesturingssysteem, meld u aan als een lokale beheerder voor de hostcomputer development kit. Zoek en verwijder de **C:\CloudBuilder.vhdx** -bestand dat is gebruikt als onderdeel van de vorige implementatie. 

3. Herhaal de stappen die u hebt uitgevoerd naar eerste [implementeert de ASDK met behulp van PowerShell](asdk-deploy-powershell.md).


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Azure registratie verwijderen 
> * De ASDK implementeren

Ga naar de volgende zelfstudie voor informatie over het toevoegen van een Azure-Stack marketplace-item.

> [!div class="nextstepaction"]
> [Een Azure-Stack marketplace-item toevoegen](asdk-marketplace-item.md)




