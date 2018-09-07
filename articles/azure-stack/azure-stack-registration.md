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
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: 52d0706177482e162d1f4bc038c967a4596fd3b3
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024855"
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure

Azure Stack registreren bij Azure, kunt u voor het downloaden van marketplace-items van Azure en het instellen van rapportage terug naar Microsoft commercegegevens. Nadat u zich registreert voor Azure Stack, gebruik wordt gerapporteerd aan Azure commerce en kunt u deze bekijken onder het abonnement dat wordt gebruikt voor de registratie.

De informatie in dit artikel beschrijft registreren geïntegreerde Azure Stack-systemen met Azure. Zie voor meer informatie over het registreren van de ASDK met Azure [Azure Stack-registratie](.\asdk\asdk-register.md) in de documentatie van ASDK.

> [!IMPORTANT]  
> Registratie is vereist voor ondersteuning van de volledige functionaliteit van de Azure Stack, waaronder items in de marketplace-aanbieding. Bovendien moet u in strijd is met Azure Stack licentievoorwaarden als u niet registreren bij het gebruik van het facturering model voor betalen als u-gebruik. Raadpleeg voor meer informatie over Azure Stack-licentieverlening modellen, de [over het aanschaffen van pagina](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Vereisten

U moet de volgende voldaan voordat u registreert:

 - Controleer uw referenties
 - De PowerShell-taalmodus instellen
 - PowerShell voor Azure Stack installeren
 - De hulpprogramma's voor Azure Stack downloaden
 - Bepalen van uw registratie-scenario

### <a name="verify-your-credentials"></a>Controleer uw referenties

Voordat u registreert Azure Stack met Azure, moet u het volgende hebben:

- De abonnements-ID voor een Azure-abonnement. Als u de ID, aanmelden bij Azure, klikt u op **alle services**. Klik vervolgens onder de **algemene** categorie, selecteer **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de abonnement-ID.

  > [!Note]  
  > Duitsland-cloudabonnementen worden momenteel niet ondersteund.

- De gebruikersnaam en wachtwoord voor een account dat eigenaar van het abonnement (MSA/2FA accounts worden ondersteund).

- Het gebruikersaccount moet een beheerder in de Azure AD-tenant waarvoor Azure Stack is geregistreerd, bijvoorbeeld `yourazurestacktenant.onmicrosoft.com`.

- De Azure Stack-resourceprovider geregistreerd (Zie de sectie Azure Stack-Resourceprovider registreren hieronder voor meer informatie).

  Als u geen Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Registreren van Azure Stack maakt geen kosten op uw Azure-abonnement.

### <a name="powershell-language-mode"></a>PowerShell-taalmodus

Als u wilt registreren is Azure Stack, de PowerShell-taalmodus moet worden ingesteld op **FullLanguageMode**.  Om te controleren dat de huidige taalmodus is ingesteld op full, opent u een PowerShell-venster met verhoogde bevoegdheid en voer de volgende PowerShell-cmdlets:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Zorg ervoor dat de uitvoer geeft als resultaat **FullLanguageMode**. Als een andere taalmodus wordt geretourneerd, registratie moet worden uitgevoerd op een andere computer of de taalmodus worden ingesteld moet op **FullLanguageMode** voordat u doorgaat.

### <a name="install-powershell-for-azure-stack"></a>PowerShell voor Azure Stack installeren

U moet de nieuwste versie van PowerShell voor Azure Stack gebruiken om u te registreren bij Azure.

Als u niet de meest recente versie niet al is geïnstalleerd, raadpleegt u [PowerShell voor Azure Stack installeren](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="download-the-azure-stack-tools"></a>De hulpprogramma's voor Azure Stack downloaden

De Azure Stack-hulpprogramma's-GitHub-opslagplaats bevat een PowerShell-modules die ondersteuning bieden voor Azure Stack-functionaliteit; met inbegrip van registratie-functionaliteit. Tijdens de registratie die u wilt importeren en gebruiken de **RegisterWithAzure.psm1** PowerShell-module, gevonden in de opslagplaats van de Azure Stack-hulpprogramma's voor het registreren van uw Azure Stack-exemplaar met Azure.

Om te controleren of u de meest recente versie gebruikt, verwijdert u alle bestaande versies van de Azure Stack-hulpprogramma's en [de nieuwste versie downloaden vanuit GitHub](azure-stack-powershell-download.md) voordat u registreert met Azure.

### <a name="determine-your-registration-scenario"></a>Bepalen van uw registratie-scenario

Uw Azure Stack-implementatie mogelijk *verbonden* of *verbroken*.

 - **Verbonden**  
 Verbonden betekent dat u kunt Azure Stack hebt geïmplementeerd, zodat deze verbinding met Internet en naar Azure maken kan. U hebt een Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) voor uw archief. Met een gekoppelde implementatie, kunt u kiezen uit twee factureringsmodellen: betalen als u-gebruik of op basis van capaciteit.
    - [Registreren van een gekoppelde Azure-Stack met Azure met de **betalen als u-gebruik** factureringsmodel](#register-connected-with-pay-as-you-go-billing)
    - [Registreren van een gekoppelde Azure-Stack met Azure met de **capaciteit** factureringsmodel](#register-connected-with-capacity-billing)

 - **De verbinding verbroken**  
 Met de niet-verbonden via de Azure-implementatie-optie, u kunt implementeren en gebruiken van Azure Stack zonder een verbinding met Internet. Met de implementatie van een niet-verbonden bent u echter beperkt tot een AD FS-identiteitsarchief en het factureringsmodel op basis van capaciteit.
    - [Registreer een niet-verbonden met Azure Stack de **capaciteit** factureringsmodel ](#register-disconnected-with-capacity-billing)

## <a name="register-connected-with-pay-as-you-go-billing"></a>Verbonden met betalen per gebruik facturering registreren

Gebruik deze stappen om Azure Stack registreren bij Azure met behulp van het facturering model voor betalen als u-gebruik.

> [!Note]  
> Al deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot het eindpunt van de bevoegde (PEP heeft). Zie voor meer informatie over de PEP [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md).

Verbonden omgevingen kunnen toegang krijgen tot het internet en Azure. Voor deze omgevingen moet u de Azure Stack-resourceprovider registreren bij Azure en configureer vervolgens uw factureringsmodel.

1. Voor het registreren van de resourceprovider van Azure Stack met Azure, PowerShell ISE starten als beheerder en gebruik de volgende PowerShell-cmdlets met de **EnvironmentName** parameter ingesteld op het juiste Azure-abonnement-type (Zie onderstaande parameters).

2. Het Azure-account die u gebruikt voor het registreren van Azure Stack toevoegen. Als u wilt het account toevoegt, worden uitgevoerd de **Add-AzureRmAccount** cmdlet. U wordt gevraagd uw globale beheerder van Azure-accountreferenties kunt invoeren en mogelijk moet u gebruikmaken van 2-factor-verificatie op basis van de configuratie van uw account.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<AzureCloud, AzureChinaCloud, or AzureUSGovernment>"
   ```

   | Parameter | Beschrijving |  
   |-----|-----|
   | EnvironmentName | De naam van de omgeving in de Azure-cloud-abonnement. Omgevingsnamen van de ondersteunde zijn **AzureCloud**, **AzureUSGovernment**, of als een Azure-abonnement van China **AzureChinaCloud**.  |

3. Als u meerdere abonnementen hebt, voert u de volgende opdracht om het abonnement te selecteren die u wilt gebruiken:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Voer de volgende opdracht om de Azure Stack resourceprovider registreren in uw Azure-abonnement:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de hulpprogramma's voor Azure Stack](#bkmk_tools). Importeren van de **RegisterWithAzure.psm1** module met behulp van PowerShell:

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Vervolgens, in dezelfde PowerShell-sessie, zorg ervoor dat u bent aangemeld bij de juiste Context van Azure PowerShell. Dit is het azure-account dat is gebruikt om de bovenstaande Azure Stack resourceprovider te registreren. Als u PowerShell om uit te voeren:

   ```PowerShell  
   Add-AzureRmAccount -Environment "<AzureCloud, AzureChinaCloud, or AzureUSGovernment>"
   ```

7. Voer in dezelfde PowerShell-sessie, het **Set AzsRegistration** cmdlet. Als u PowerShell om uit te voeren:  

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```
   Zie voor meer informatie over de cmdlet Set-AzsRegistration [registratie verwijzing](#registration-reference).

  Het proces duurt tussen 10 en 15 minuten. Wanneer de opdracht is voltooid, ziet u het bericht **'uw omgeving is nu geregistreerd en geactiveerd met behulp van de opgegeven parameters.'**

## <a name="register-connected-with-capacity-billing"></a>Verbonden zijn met capaciteit facturering registreren

Gebruik deze stappen om Azure Stack registreren bij Azure met behulp van het facturering model voor betalen als u-gebruik.

> [!Note]  
> Al deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot het eindpunt van de bevoegde (PEP heeft). Zie voor meer informatie over de PEP [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md).

Verbonden omgevingen kunnen toegang krijgen tot het internet en Azure. Voor deze omgevingen moet u de Azure Stack-resourceprovider registreren bij Azure en configureer vervolgens uw factureringsmodel.

1. Voor het registreren van de resourceprovider van Azure Stack met Azure, PowerShell ISE starten als beheerder en gebruik de volgende PowerShell-cmdlets met de **EnvironmentName** parameter ingesteld op het juiste Azure-abonnement-type (Zie onderstaande parameters).

2. Het Azure-account die u gebruikt voor het registreren van Azure Stack toevoegen. Als u wilt het account toevoegt, worden uitgevoerd de **Add-AzureRmAccount** cmdlet. U wordt gevraagd uw globale beheerder van Azure-accountreferenties kunt invoeren en mogelijk moet u gebruikmaken van 2-factor-verificatie op basis van de configuratie van uw account.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<AzureCloud, AzureChinaCloud, or AzureUSGovernment>"
   ```

   | Parameter | Beschrijving |  
   |-----|-----|
   | EnvironmentName | De naam van de omgeving in de Azure-cloud-abonnement. Omgevingsnamen van de ondersteunde zijn **AzureCloud**, **AzureUSGovernment**, of als een Azure-abonnement van China **AzureChinaCloud**.  |

3. Als u meerdere abonnementen hebt, voert u de volgende opdracht om het abonnement te selecteren die u wilt gebruiken:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Voer de volgende opdracht om de Azure Stack resourceprovider registreren in uw Azure-abonnement:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de hulpprogramma's voor Azure Stack](#bkmk_tools). Importeren van de **RegisterWithAzure.psm1** module met behulp van PowerShell:

  ```PowerShell  
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  $RegistrationName = "<unique-registration-name>"
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -AgreementNumber <EA agreement number> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
  ```
   > [!Note]  
   > U kunt uitschakelen voor gebruiksrapporten met de parameter UsageReportingEnabled voor de **Set AzsRegistration** cmdlet. Stel de parameter op onwaar. Bijvoorbeeld: ' UsageReportingEnabled
   
  Zie voor meer informatie over de cmdlet Set-AzsRegistration [registratie verwijzing](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Niet-verbonden met een capaciteit facturering registreren

Als u Azure Stack in een omgeving zonder verbinding (met geen verbinding met internet) registreert, moet u een registratie-token ophalen uit de Azure Stack-omgeving en dat token vervolgens gebruiken op een computer die verbinding kan maken met Azure en heeft [PowerShell voor Azure Stack geïnstalleerd](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Registratie van een toegangstoken ophalen uit de Azure Stack-omgeving

1. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de hulpprogramma's voor Azure Stack](#bkmk_tools). Importeren van de **RegisterWithAzure.psm1** module:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Als u het registratietoken, voer de volgende PowerShell-cmdlets:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   Zie voor meer informatie over de cmdlet Get-AzsRegistrationToken [registratie verwijzing](#registration-reference).

   > [!Tip]  
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

  > [!Note]  
  > De naam van de registratie-resource en de registratietoken voor toekomstig gebruik opslaan.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Haal een activeringscode van registratie van Azure-Resource

Vervolgens moet u een activeringscode ophalen van de registratie-resource in Azure tijdens de Register-AzsEnvironment gemaakt.

Als u de activeringscode, voer de volgende PowerShell-cmdlets:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!Tip]  
  > De activeringscode is opgeslagen in het bestand dat is opgegeven voor *$KeyOutputFilePath*. U kunt het bestandspad of de bestandsnaam naar eigen goeddunken wijzigen.

### <a name="create-an-activation-resource-in-azure-stack"></a>Een activering-Resource maken in Azure Stack

Ga terug naar de Azure Stack-omgeving met het bestand of de tekst van de activeringscode gemaakt op basis van Get-AzsActivationKey. Vervolgens maakt u een bron voor activering in Azure Stack met deze activeringssleutel. Voer de volgende PowerShell-cmdlets voor het maken van een resource activering:  

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
2. Selecteer **alle Services**, en klik vervolgens onder de **beheer** categorie, selecteer **Marketplace management** > **uit Azuretoevoegen**.

Als u ziet een lijst met items die beschikbaar zijn in Azure (zoals WordPress), wordt de activering is voltooid. Echter, in niet-verbonden omgevingen niet ziet u Azure marketplace-items in de Azure Stack marketplace.

> [!Note]  
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

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Wijzig het factureringsmodel of hoe u functies bieden

Als u wilt wijzigen van de facturering van maandabonnementen of hoe u functies voor uw installatie bieden, kunt u de registratiefunctie om in te stellen de nieuwe waarden kunt aanroepen. U hoeft niet verwijdert u eerst de huidige registratie:

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

Als u wilt verwijderen van de activering-resource in Azure Stack, voer de volgende PowerShell-cmdlets in uw Azure Stack-omgeving:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Vervolgens als u wilt verwijderen van de registratie-resource in Azure, zorg ervoor dat u gebruikmaakt van een verbonden Azure computer, zich aanmelden bij de juiste context van Azure PowerShell en voert u de juiste PowerShell-cmdlets zoals hieronder wordt beschreven.

U kunt de registratietoken gebruikt voor het maken van de resource:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Of u kunt de naam van de registratie:

  ```Powershell
  $registrationName = "AzureStack-<Cloud ID of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Registreer opnieuw met niet-verbonden stappen

U hebt nu volledig verwijderd in een niet-verbonden scenario en moet de stappen voor het registreren van een Azure Stack-omgeving in een niet-verbonden scenario.

### <a name="disable-or-enable-usage-reporting"></a>Rapportage over het gebruik in- of uitschakelen

Uitschakelen voor gebruiksrapporten met voor Azure Stack-omgevingen die gebruikmaken van een capaciteit factureringsmodel, de **UsageReportingEnabled** parameter met behulp van de **Set AzsRegistration** of de  **Get-AzsRegistrationToken** cmdlets. Azure Stack-standaard metrische gegevens over gebruik rapporten. Operators met een capaciteit gebruikt of ondersteunen van een niet-verbonden omgeving moet uitschakelen rapportage over het gebruik.

#### <a name="with-a-connected-azure-stack"></a>Met een gekoppelde Azure Stack

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
   ```

#### <a name="with-a-disconnected-azure-stack"></a>Met een niet-verbonden Azure Stack

1. Als u wilt wijzigen van het registratietoken, voer de volgende PowerShell-cmdlets:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False
   $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!Tip]  
   > De registratietoken is opgeslagen in het bestand dat is opgegeven voor *$FilePathForRegistrationToken*. U kunt het bestandspad of de bestandsnaam naar eigen goeddunken wijzigen.

2. Sla deze voor gebruik in de Azure-registratietoken verbonden machine. U kunt het bestand of de tekst van $FilePathForRegistrationToken kopiëren.

## <a name="move-a-registration-resource"></a>Een registratie-resource verplaatsen
Een registratie-resource verplaatsen tussen resourcegroepen onder hetzelfde abonnement **is** ondersteund voor alle omgevingen. Echter, een registratie-resource verplaatsen tussen abonnementen wordt alleen ondersteund voor CSP's wanneer beide abonnementen worden omgezet naar dezelfde Partner-ID. Zie voor meer informatie over het verplaatsen van resources naar een nieuwe resourcegroep [resources verplaatsen naar een nieuwe resourcegroep of abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="registration-reference"></a>Registratie-verwijzing

### <a name="set-azsregistration"></a>Set-AzsRegistration

Set-AzsRegistration kunt u Azure Stack registreren bij Azure en in- of uitschakelen van de aanbieding van items in de marketplace en rapportage over het gebruik.

Als u wilt de cmdlet uitvoert, hebt u het volgende nodig:
- Een globale Azure-abonnement van elk type.
- U moet ook worden geregistreerd Azure PowerShell met een account dat een eigenaar of bijdrager aan het abonnement.

```PowerShell
    Set-AzsRegistration [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String> [[-AzureContext]
    <PSObject>] [[-ResourceGroupName] <String>] [[-ResourceGroupLocation] <String>] [[-BillingModel] <String>]
    [-MarketplaceSyndicationEnabled] [-UsageReportingEnabled] [[-AgreementNumber] <String>] [[-RegistrationName]
    <String>] [<CommonParameters>]
   ```

| Parameter | Type | Beschrijving |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | De referenties waarmee [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam bevindt zich in de indeling **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Reeks | Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekverzameling en andere post implementatietaken. Raadpleeg voor meer informatie, de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artikel. |
| AzureContext | PSObject |  |
| ResourceGroupName | Reeks |  |
| ResourceGroupLocation | Reeks |  |
| BillingModel | Reeks | Het factureringsmodel die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: capaciteit, PayAsYouUse en ontwikkeling. |
| MarketplaceSyndicationEnabled |  |  |
| UsageReportingEnabled | Waar/onwaar | Azure Stack-standaard metrische gegevens over gebruik rapporten. Operators met een capaciteit gebruikt of ondersteunen van een niet-verbonden omgeving moet uitschakelen rapportage over het gebruik. Toegestane waarden voor deze parameter zijn: True, False. |
| AgreementNumber | Reeks |  |
| registrationName | Reeks | Instellen van een unieke naam voor de inschrijving als u het script voor de registratie op meer dan één exemplaar van Azure Stack met behulp van de dezelfde Azure-abonnement-ID. De parameter heeft een standaardwaarde van **AzureStackRegistration**. Echter, als u dezelfde naam op meer dan één exemplaar van Azure Stack gebruikt, het script mislukken. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Get-AzsRegistrationToken genereert een registratietoken van de invoerparameters.

```PowerShell  
    Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Parameter | Type | Beschrijving |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | De referenties waarmee [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam bevindt zich in de indeling **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Reeks |  Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekverzameling en andere post implementatietaken. Raadpleeg voor meer informatie, de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artikel. |
| AzureContext | PSObject |  |
| ResourceGroupName | Reeks |  |
| ResourceGroupLocation | Reeks |  |
| BillingModel | Reeks | Het factureringsmodel die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: capaciteit, PayAsYouUse en ontwikkeling. |
| MarketplaceSyndicationEnabled | Waar/onwaar |  |
| UsageReportingEnabled | Waar/onwaar | Azure Stack-standaard metrische gegevens over gebruik rapporten. Operators met een capaciteit gebruikt of ondersteunen van een niet-verbonden omgeving moet uitschakelen rapportage over het gebruik. Toegestane waarden voor deze parameter zijn: True, False. |
| AgreementNumber | Reeks |  |


## <a name="next-steps"></a>Volgende stappen

[Marketplace-items van Azure downloaden](azure-stack-download-azure-marketplace-item.md)
