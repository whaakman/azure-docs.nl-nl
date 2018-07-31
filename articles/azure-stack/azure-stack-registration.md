---
title: Registratie van Azure voor Azure Stack-geïntegreerde systemen | Microsoft Docs
description: Beschrijft het proces van de registratie van Azure voor implementaties met meerdere knooppunten Azure Stack Azure-verbinding.
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
ms.date: 07/30/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: e73cd24064f2b0d6197a69251b55639d41e3212c
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39357745"
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure

Registreren van [Azure Stack](azure-stack-poc.md) met Azure kunt u voor het downloaden van marketplace-items van Azure en het instellen van rapportage terug naar Microsoft commercegegevens. Nadat u zich registreert voor Azure Stack, gebruik wordt gerapporteerd aan Azure commerce en kunt u deze bekijken onder het abonnement dat wordt gebruikt voor de registratie.

> [!IMPORTANT]  
> Registratie is vereist voor ondersteuning van de volledige functionaliteit van Azure Stack, met inbegrip van marketplace-syndicatie. Bovendien moet u in strijd is met Azure Stack licentievoorwaarden als u niet registreren bij het gebruik van het facturering model voor betalen als u-gebruik. Raadpleeg voor meer informatie over Azure Stack-licentieverlening modellen, de [over het aanschaffen van pagina](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Vereisten

Voordat u registreert Azure Stack met Azure, moet u het volgende hebben:

- De abonnements-ID voor een Azure-abonnement. Als u de ID, aanmelden bij Azure, klikt u op **meer services** > **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de Abonnements-ID.

  > [!NOTE]
  > Duitsland en US Government-cloud-abonnementen worden momenteel niet ondersteund.

- De gebruikersnaam en wachtwoord voor een account dat eigenaar van het abonnement (MSA/2FA accounts worden ondersteund).
- De Azure Stack-resourceprovider geregistreerd (Zie de sectie Azure Stack-Resourceprovider registreren hieronder voor meer informatie).

Als u geen Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Registreren van Azure Stack maakt geen kosten op uw Azure-abonnement.

### <a name="powershell-language-mode"></a>PowerShell-taalmodus

Als u wilt registreren is Azure Stack, de PowerShell-taalmodus moet worden ingesteld op **FullLanguageMode**.  Om te controleren dat de huidige taalmodus is ingesteld op full, open een verhoogde PowerShell-venster en voer de volgende PowerShell-opdrachten:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Zorg ervoor dat de uitvoer geeft als resultaat **FullLanguageMode**. Als een andere taalmodus wordt geretourneerd, registratie moet worden uitgevoerd op een andere computer of de taalmodus worden ingesteld moet op **FullLanguageMode** voordat u doorgaat.

### <a name="bkmk_powershell"></a>PowerShell voor Azure Stack installeren

U moet de nieuwste versie van PowerShell voor Azure Stack gebruiken om u te registreren bij Azure.

Als u nog niet is geïnstalleerd, [PowerShell voor Azure Stack installeren](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="bkmk_tools"></a>De hulpprogramma's voor Azure Stack downloaden

De Azure Stack-hulpprogramma's-GitHub-opslagplaats bevat een PowerShell-modules die ondersteuning bieden voor Azure Stack-functionaliteit; met inbegrip van registratie-functionaliteit. Tijdens de registratie die u wilt importeren en gebruiken van de RegisterWithAzure.psm1 PowerShell-module gevonden in de opslagplaats van de Azure Stack-hulpprogramma's voor het registreren van uw Azure Stack-exemplaar met Azure.

Om te controleren of u de meest recente versie gebruikt, verwijdert u alle bestaande versies van de Azure Stack-hulpprogramma's en [de nieuwste versie downloaden vanuit GitHub](azure-stack-powershell-download.md) voordat u registreert met Azure.

## <a name="register-azure-stack-in-connected-environments"></a>Azure Stack in verbonden omgevingen registreren

Verbonden omgevingen kunnen toegang krijgen tot het internet en Azure. Voor deze omgevingen moet u de Azure Stack-resourceprovider registreren bij Azure en configureer vervolgens uw factureringsmodel.

> [!NOTE]
> Al deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot het eindpunt van de bevoegdheden heeft.

### <a name="register-the-azure-stack-resource-provider"></a>Registreer de resourceprovider van Azure Stack

Voor het registreren van de resourceprovider van Azure Stack met Azure, PowerShell ISE starten als beheerder en gebruik de volgende PowerShell-opdrachten met de **EnvironmentName** parameter ingesteld op het juiste Azure-abonnement-type (Zie onderstaande parameters).

1. Het Azure-account die u gebruikt voor het registreren van Azure Stack toevoegen. Als u wilt het account toevoegt, worden uitgevoerd de **Add-AzureRmAccount** cmdlet. U wordt gevraagd uw globale beheerder van Azure-accountreferenties kunt invoeren en mogelijk moet u gebruikmaken van 2-factor-verificatie op basis van de configuratie van uw account.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parameter | Beschrijving |  
   |-----|-----|
   | EnvironmentName | De naam van de omgeving in de Azure-cloud-abonnement. Omgevingsnamen van de ondersteunde zijn **AzureCloud** of, als met een Azure-abonnement van China **AzureChinaCloud**.  |
   |  |  |

2. Als u meerdere abonnementen hebt, voert u de volgende opdracht om het abonnement te selecteren die u wilt gebruiken:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Voer de volgende opdracht om de Azure Stack resourceprovider registreren in uw Azure-abonnement:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Azure Stack registreren bij Azure met behulp van het facturering model voor betalen als u-gebruik

Gebruik deze stappen om Azure Stack registreren bij Azure met behulp van het facturering model voor betalen als u-gebruik.

1. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de hulpprogramma's voor Azure Stack](#bkmk_tools). Importeren van de **RegisterWithAzure.psm1** module met behulp van PowerShell:

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Vervolgens, in dezelfde PowerShell-sessie, zorg ervoor dat u bent aangemeld bij de juiste Context van Azure PowerShell. Dit is het azure-account dat is gebruikt om de bovenstaande Azure Stack resourceprovider te registreren. Als u PowerShell om uit te voeren:

   ```PowerShell  
   Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>"
   ```

3. Voer in dezelfde PowerShell-sessie, het **Set AzsRegistration** cmdlet. Als u PowerShell om uit te voeren:  

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```

  |Parameter|Beschrijving|
  |-----|-----|
  |PrivilegedEndpointCredential|De referenties waarmee [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam bevindt zich in de indeling **AzureStackDomain\CloudAdmin**.|
  |PrivilegedEndpoint|Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekverzameling en andere post implementatietaken. Raadpleeg voor meer informatie, de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artikel.|
  |BillingModel|Het factureringsmodel die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: capaciteit, PayAsYouUse en ontwikkeling.|
  | registrationName | Instellen van een unieke naam voor de inschrijving als u het script voor de registratie op meer dan één exemplaar van Azure Stack met behulp van de dezelfde Azure-abonnement-ID. De parameter heeft een standaardwaarde van **AzureStackRegistration**. Echter, als u dezelfde naam op meer dan één exemplaar van Azure Stack gebruikt, het script mislukken. |

  Het proces duurt tussen 10 en 15 minuten. Wanneer de opdracht is voltooid, ziet u het bericht **'uw omgeving is nu geregistreerd en geactiveerd met behulp van de opgegeven parameters.'**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure Stack registreren bij Azure met behulp van het factureringsmodel voor capaciteit

Volg de instructies die worden gebruikt voor het registreren met behulp van het factureringsmodel betalen als u-gebruik, maar de overeenkomstnummer waaronder capaciteit is aangeschaft toevoegen en wijzig de **BillingModel** parameter **capaciteit**. Alle andere parameters zijn niet gewijzigd.

Als u PowerShell om uit te voeren:

```PowerShell  
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
$RegistrationName = "<unique-registration-name>"
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity `
    -RegistrationName $RegistrationName
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Azure Stack in niet-verbonden omgevingen registreren
Als u Azure Stack in een omgeving zonder verbinding (met geen verbinding met internet) registreert, moet u een registratie-token ophalen uit de Azure Stack-omgeving en dat token vervolgens gebruiken op een computer die verbinding kan maken met Azure en heeft [PowerShell voor Azure Stack geïnstalleerd](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Registratie van een toegangstoken ophalen uit de Azure Stack-omgeving

1. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de hulpprogramma's voor Azure Stack](#bkmk_tools). Importeren van de **RegisterWithAzure.psm1** module:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Als u het registratietoken, voer de volgende PowerShell-opdrachten:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!TIP]  
   > De registratietoken is opgeslagen in het bestand dat is opgegeven voor *$FilePathForRegistrationToken*. U kunt het bestandspad of de bestandsnaam naar eigen goeddunken wijzigen.

3. Sla deze voor gebruik in de Azure-registratietoken verbonden machine. U kunt het bestand of de tekst van $FilePathForRegistrationToken kopiëren.

### <a name="connect-to-azure-and-register"></a>Verbinding maken met Azure en registreren

Op de computer die is verbonden met Internet, moet u de dezelfde stappen voor het importeren van de module RegisterWithAzure.psm1 en zich aanmelden bij de juiste context van Azure Powershell uitvoeren. Roep vervolgens registreren AzsEnvironment. Geef de registratietoken te registreren bij Azure. Als u meer dan één exemplaar van Azure Stack met behulp van de dezelfde Azure-abonnements-ID registreert, moet u een unieke registratie-naam opgeven. Voer de volgende cmdlet uit:

  ```PowerShell  
  $registrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  -RegistrationName $RegistrationName
  ```

U kunt de cmdlet Get-inhoud (optioneel) gebruiken om te verwijzen naar een bestand met uw registratietoken:

  ```PowerShell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken -RegistrationName $RegistrationName
  ```

  > [!NOTE]  
  > De naam van de registratie-resource en de registratietoken voor toekomstig gebruik opslaan.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Haal een activeringscode van registratie van Azure-Resource

Vervolgens moet u een activeringscode ophalen van de registratie-resource in Azure tijdens de Register-AzsEnvironment gemaakt.

Als u de activeringscode, voer de volgende PowerShell-opdrachten:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!TIP]  
  > De activeringscode is opgeslagen in het bestand dat is opgegeven voor *$KeyOutputFilePath*. U kunt het bestandspad of de bestandsnaam naar eigen goeddunken wijzigen.

### <a name="create-an-activation-resource-in-azure-stack"></a>Een activering-Resource maken in Azure Stack

Ga terug naar de Azure Stack-omgeving met het bestand of de tekst van de activeringscode gemaakt op basis van Get-AzsActivationKey. Vervolgens maakt u een bron voor activering in Azure Stack met deze activeringssleutel. Voer de volgende PowerShell-opdrachten voor het maken van een resource activering:  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

U kunt de cmdlet Get-inhoud (optioneel) gebruiken om te verwijzen naar een bestand met uw registratietoken:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Controleer of de registratie van de Azure Stack

Volg deze stappen om te controleren of Azure Stack is geregistreerd met Azure.

1. Meld u aan met de Azure Stack [beheerdersportal](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;regio >. &lt;FQDN-naam >*.
2. Selecteer **meer Services** > **Marketplace Management** > **toevoegen vanuit Azure**.

Als u ziet een lijst met items die beschikbaar zijn in Azure (zoals WordPress), wordt de activering is voltooid. Echter, in niet-verbonden omgevingen niet ziet u Azure marketplace-items in de Azure Stack marketplace.

> [!NOTE]
> Nadat de registratie is voltooid, wordt de actieve waarschuwing voor het registreren van niet langer worden weergegeven.

## <a name="renew-or-change-registration"></a>Vernieuwen of wijzigen van inschrijving

### <a name="renew-or-change-registration-in-connected-environments"></a>Vernieuwen of wijzigen van de registratie in verbonden omgevingen

U moet bijwerken of vernieuwen van de inschrijving in de volgende omstandigheden:

- Nadat u uw jaarlijkse op basis van capaciteit-abonnement te verlengen.
- Wanneer u uw factureringsmodel wijzigen.
- Wanneer u wijzigingen (knooppunten toevoegen of verwijderen) schalen voor facturering op basis van capaciteit.

#### <a name="change-the-subscription-you-use"></a>Wijzigen van het abonnement dat u gebruikt

Als u wilt wijzigen van het abonnement dat u gebruikt, moet u eerst uitvoeren de **Remove-AzsRegistration** cmdlet, zorg er dan voor u zijn aangemeld bij de juiste context van Azure PowerShell en voer ten slotte **Set AzsRegistration**  met een gewijzigd parameters:

  ```PowerShell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Wijzigen van de facturering model of syndication-functies

Als u wilt de facturering van maandabonnementen of syndication-functies voor uw installatie wijzigen, kunt u de registratiefunctie om in te stellen de nieuwe waarden kunt aanroepen. U hoeft niet verwijdert u eerst de huidige registratie:

  ```PowerShell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Vernieuwen of wijzigen van de registratie in niet-verbonden omgevingen

U moet bijwerken of vernieuwen van de inschrijving in de volgende omstandigheden:

- Nadat u uw jaarlijkse op basis van capaciteit-abonnement te verlengen.
- Wanneer u uw factureringsmodel wijzigen.
- Wanneer u wijzigingen (knooppunten toevoegen of verwijderen) schalen voor facturering op basis van capaciteit.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Verwijder de resource activering uit Azure Stack

U moet eerst de resource activering verwijderen uit Azure Stack en vervolgens de registratie-resource in Azure.  

Als u wilt verwijderen van de activering-resource in Azure Stack, de volgende PowerShell-opdrachten worden uitgevoerd in uw Azure Stack-omgeving:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Vervolgens als u wilt verwijderen van de registratie-resource in Azure, zorg ervoor dat u gebruikmaakt van een verbonden Azure computer, zich aanmelden bij de juiste context van Azure PowerShell, en de juiste PowerShell-opdrachten uitvoeren, zoals hieronder wordt beschreven.

U kunt de registratietoken gebruikt voor het maken van de resource:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Of u kunt de naam van de registratie:

  ```Powershell
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Registreer opnieuw met niet-verbonden stappen

U hebt nu volledig verwijderd in een niet-verbonden scenario en moet de stappen voor het registreren van een Azure Stack-omgeving in een niet-verbonden scenario.

## <a name="next-steps"></a>Volgende stappen

[Marketplace-items van Azure downloaden](azure-stack-download-azure-marketplace-item.md)
