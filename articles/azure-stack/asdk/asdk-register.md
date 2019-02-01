---
title: De ASDK registreren bij Azure | Microsoft Docs
description: Beschrijft hoe u Azure Stack registreren bij Azure, marketplace-syndicatie en rapportage over het gebruik.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 5daf60669d6e647fc1060bb5ac172b777c9559c5
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55512292"
---
# <a name="azure-stack-registration"></a>Azure Stack-registratie
U kunt de installatie van de Azure Stack Development Kit (ASDK) registreren met Azure marketplace-items van Azure downloaden en het instellen van rapportage terug naar Microsoft commercegegevens. Registratie is vereist voor ondersteuning van de volledige functionaliteit van Azure Stack, met inbegrip van marketplace-syndicatie. Registratie is vereist om in te schakelen om te testen van belangrijke Azure Stack-functionaliteit, zoals marketplace-syndicatie en rapportage over het gebruik. Nadat u Azure Stack hebt geregistreerd, wordt gebruik gerapporteerd aan Azure commerce. U kunt het zien onder het abonnement dat u voor de registratie gebruikt. Echter ASDK gebruikers niet in rekening gebracht voor het gebruik die ze rapporteren.

Als u uw ASDK niet registreert, ziet u mogelijk een **activering vereist** waarschuwing die aangeeft voor het registreren van uw Azure Stack Development Kit. Dit gedrag is verwacht.

## <a name="prerequisites"></a>Vereisten
Voordat u deze instructies voor het registreren van de ASDK met Azure, zorg ervoor dat u hebt de PowerShell voor Azure Stack geïnstalleerd en de hulpprogramma's voor Azure Stack downloaden, zoals beschreven in de [post-implementatieconfiguratie](asdk-post-deploy.md) artikel.

Bovendien de taalmodus PowerShell moet zijn ingesteld op **FullLanguageMode** op de computer gebruikt voor het registreren van de ASDK met Azure. Om te controleren dat de huidige taalmodus is ingesteld op full, open een verhoogde PowerShell-venster en voer de volgende PowerShell-opdrachten:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Zorg ervoor dat de uitvoer geeft als resultaat **FullLanguageMode**. Als een andere taalmodus wordt geretourneerd, registratie moet worden uitgevoerd op een andere computer of de taalmodus worden ingesteld moet op **FullLanguageMode** voordat u doorgaat.

De Azure AD-account gebruikt voor de registratie moet toegang hebben tot het Azure-abonnement en machtigingen voor het maken van toepassingen met identiteit en service-principals in de map die is gekoppeld aan dat abonnement. Wordt u aangeraden dat u Azure Stack registreren bij Azure-beheer met minimale bevoegdheden [het maken van een service-account moet worden gebruikt voor registratie](..\azure-stack-registration-role.md) in plaats van met de referenties van de globale beheerder.

## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure
Volg deze stappen voor het registreren van de ASDK met Azure.

> [!NOTE]
> Al deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot het eindpunt van de bevoegdheden heeft. Voor de ASDK is dat de hostcomputer development kit.

1. Open een PowerShell-console als beheerder.  

2. Voer de volgende PowerShell-opdrachten voor het registreren van uw installatie ASDK met Azure. U moet zich aanmelden bij uw Azure-abonnement zowel de lokale ASDK-installatie. Als u een Azure-abonnement hebt hebt, u kunt [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Registreren van Azure Stack maakt geen kosten op uw Azure-abonnement.<br><br>Een unieke naam voor de registratie wordt ingesteld tijdens het uitvoeren van de **Set AzsRegistration** cmdlet. De **RegistrationName** parameter heeft een standaardwaarde van **AzureStackRegistration**. Echter, als u dezelfde naam op meer dan één exemplaar van Azure Stack gebruikt, het script mislukken.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "<environment name>"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    # Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    # If you have multiple subscriptions, run the following command to select the one you want to use:
    # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription
    
    # Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$true
    ```
3. Wanneer het script is voltooid, moet u dit bericht ziet: **Uw omgeving is nu geregistreerd en geactiveerd met behulp van de opgegeven parameters.**

    ![Uw omgeving is nu geregistreerd](media/asdk-register/1.PNG)


## <a name="register-in-disconnected-environments"></a>Registreren in niet-verbonden omgevingen
Als u Azure Stack in een omgeving zonder verbinding (met geen verbinding met internet) registreert, moet u een registratie-token ophalen uit de Azure Stack-omgeving en dat token vervolgens gebruiken op een computer die verbinding kan maken met Azure om te registreren en maken van een activering resource voor uw omgeving ASDK.
 
 > [!IMPORTANT]
 > Voordat u deze instructies voor het registreren van Azure Stack, zorg ervoor dat u hebt PowerShell voor Azure Stack is geïnstalleerd en de hulpprogramma's voor Azure Stack downloaden, zoals beschreven in de [post-implementatieconfiguratie](asdk-post-deploy.md) artikel op zowel de ASDK-host computer en de computer met internettoegang waarmee verbinding wordt gemaakt naar Azure en registreren.

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Registratie van een toegangstoken ophalen uit de Azure Stack-omgeving
Op de hostcomputer ASDK start PowerShell als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u de hulpprogramma's voor Azure Stack hebt gedownload. Gebruik de volgende PowerShell-opdrachten voor het importeren van de **RegisterWithAzure.psm1** -module en gebruik vervolgens de **Get-AzsRegistrationToken** cmdlet voor het ophalen van het registratietoken:  

   ```PowerShell  
   # Import the registration module that was downloaded with the GitHub tools
   Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

   # Create registration token
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
   # File path to save the token. This example saves the file as C:\RegistrationToken.txt.
   $FilePathForRegistrationToken = "$env:SystemDrive\RegistrationToken.txt"
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $CloudAdminCred `
   -UsageReportingEnabled:$false `
   -PrivilegedEndpoint AzS-ERCS01 `
   -BillingModel Development `
   -MarketplaceSyndicationEnabled:$false `
   -TokenOutputFilePath $FilePathForRegistrationToken
   ```

Sla dit registratietoken voor gebruik op de computer met internetverbinding. U kunt het bestand of de tekst van het bestand dat is gemaakt door de parameter $FilePathForRegistrationToken kopiëren.

### <a name="connect-to-azure-and-register"></a>Verbinding maken met Azure en registreren
Op internet verbonden computer de volgende PowerShell-opdrachten gebruiken voor het importeren van de **RegisterWithAzure.psm1** module en gebruik vervolgens de **registreren AzsEnvironment** cmdlet om te registreren met het gebruik van Azure de registratietoken dat u zojuist hebt gemaakt en een unieke registratie-naam:  

  ```PowerShell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

U kunt ook kunt u de **Get-inhoud** cmdlet om te verwijzen naar een bestand met uw registratietoken:

  ```PowerShell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure 
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Wanneer de registratie is voltooid, ziet u een bericht dat lijkt op **uw Azure Stack-omgeving is nu geregistreerd bij Azure.**

> [!IMPORTANT]
> Sluit het PowerShell-venster niet. 

Sla de registratietoken en registratie-resourcenaam voor toekomstig gebruik.

### <a name="retrieve-an-activation-key-from-the-azure-registration-resource"></a>Haal een activeringscode van de registratie van Azure-resource

Nog steeds met behulp van de computer met internetverbinding **en de dezelfde PowerShell-consolevenster**, een activeringscode ophalen van de registratie-resource gemaakt wanneer u met Azure hebt geregistreerd.

Als u de activeringscode, voer de volgende PowerShell-opdrachten, gebruikt u dezelfde unieke registratie-naam van waarde die u hebt opgegeven bij het registreren met Azure in de vorige stap:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  # File path to save the activation key. This example saves the file as C:\ActivationKey.txt.
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName `
  -KeyOutputFilePath $KeyOutputFilePath
  ```
### <a name="create-an-activation-resource-in-azure-stack"></a>Een activering-Resource maken in Azure Stack

Ga terug naar de Azure Stack-omgeving met het bestand of de tekst van de activeringscode gemaakt op basis van **Get-AzsActivationKey**. Voer de volgende PowerShell-opdrachten om te maken van een activering-resource in Azure Stack met deze activeringssleutel:   

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1
  
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

U kunt ook kunt u de **Get-inhoud** cmdlet om te verwijzen naar een bestand met uw registratietoken:

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  # This example uses the C:\ActivationKey.txt file.
  $ActivationKey = Get-Content -Path "$env:SystemDrive\Activationkey.txt"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Wanneer de activering is voltooid, ziet u een bericht dat lijkt op **uw omgeving het proces voor registratie en -activering is voltooid.**

## <a name="verify-the-registration-was-successful"></a>Controleer of dat de registratie is geslaagd

U kunt de **regiobeheer** tegel om te controleren of de Azure Stack-registratie geslaagd is. Deze tegel is beschikbaar op de standaard-dashboard in de beheerdersportal.

1. Aanmelden bij de [Azure Stack-beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer in het Dashboard, **regiobeheer**.

    [ ![De tegel beheer regio](media/asdk-register/admin1sm.png "regio management tegel") ](media/asdk-register/admin1.png#lightbox)

3. Selecteer **eigenschappen**. Deze blade ziet u de status en details van uw omgeving. De status kan zijn **geregistreerde** of **niet geregistreerd**. Als geregistreerd, ook ziet u de ID van de Azure-abonnement dat u gebruikt voor het registreren van uw Azure Stack, samen met de registratie-resourcegroep en de naam.

## <a name="move-a-registration-resource"></a>Een registratie-resource verplaatsen
Een registratie-resource verplaatsen tussen resourcegroepen onder hetzelfde abonnement **is** ondersteund. Zie voor meer informatie over het verplaatsen van resources naar een nieuwe resourcegroep [resources verplaatsen naar een nieuwe resourcegroep of abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Volgende stappen

- [Een Azure Stack marketplace-item toevoegen](../azure-stack-marketplace.md)
