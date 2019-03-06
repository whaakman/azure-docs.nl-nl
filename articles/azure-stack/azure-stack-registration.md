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
ms.date: 03/04/2019
ms.author: jeffgilb
ms.reviewer: brbartle
ms.lastreviewed: 03/04/2019
ms.openlocfilehash: 12edea505ba3b0c8009512a52e3eea9ecea5bb26
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57405195"
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure

Azure Stack registreren bij Azure, kunt u voor het downloaden van marketplace-items van Azure en het instellen van rapportage terug naar Microsoft commercegegevens. Nadat u zich registreert voor Azure Stack, gebruik wordt gerapporteerd aan Azure commerce en kunt u deze bekijken in de Azure facturering abonnements-ID gebruiken voor registratie.

De informatie in dit artikel beschrijft registreren geïntegreerde Azure Stack-systemen met Azure. Zie voor meer informatie over het registreren van de ASDK met Azure [Azure Stack-registratie](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-register) in de documentatie van ASDK.

> [!IMPORTANT]  
> Registratie is vereist voor ondersteuning van de volledige functionaliteit van de Azure Stack, waaronder items in de marketplace-aanbieding. Bovendien moet u in strijd is met Azure Stack licentievoorwaarden als u niet registreren bij het gebruik van het facturering model voor betalen als u-gebruik. Raadpleeg voor meer informatie over Azure Stack-licentieverlening modellen, de [over het aanschaffen van pagina](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Vereisten

De volgende handelingen uit in plaats moet u voordat u registreert:

 - Uw referenties controleren
 - De PowerShell-taalmodus instellen
 - PowerShell voor Azure Stack installeren
 - De hulpprogramma's voor Azure Stack downloaden
 - Bepalen van uw registratie-scenario

### <a name="verify-your-credentials"></a>Uw referenties controleren

Voordat u registreert Azure Stack met Azure, moet u het volgende hebben:

- De abonnements-ID voor een Azure-abonnement. Alleen EA, CSP of CSP van gedeelde services abonnementen worden ondersteund voor registratie. CSP's nodig hebt om te bepalen of u wilt [gebruikt u een abonnement CSP of APSS](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription).<br><br>Als u de ID, aanmelden bij Azure, klikt u op **alle services**. Klik vervolgens onder de **algemene** categorie, selecteer **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de abonnement-ID.

  > [!Note]  
  > Duitsland-cloudabonnementen worden momenteel niet ondersteund.

- De gebruikersnaam en wachtwoord voor een account dat eigenaar van het abonnement.

- Het gebruikersaccount moet toegang hebben tot het Azure-abonnement en machtigingen hebben voor het maken van toepassingen met identiteit en service-principals in de map die is gekoppeld aan dat abonnement. Wordt u aangeraden dat u Azure Stack registreren bij Azure beheer van minimale bevoegdheden. Zie voor meer informatie over het maken van een aangepaste roldefinitie die de toegang tot uw abonnement voor registratie beperkt [maakt u een rol registratie voor Azure Stack](azure-stack-registration-role.md).

- De Azure Stack-resourceprovider geregistreerd (Zie de volgende sectie van de Azure Stack-Resourceprovider registreren voor meer informatie).

Na de registratie is Azure Active Directory-globale beheerder toestemming niet vereist. Bepaalde bewerkingen is echter mogelijk de referentie van de globale beheerder. Bijvoorbeeld, een resource provider installatieprogramma script of een nieuwe functie die een machtiging worden verleend. U kunt tijdelijk reactiveren van het account algemeen beheerder-bevoegdheden of gebruik een afzonderlijke globale beheerder-account dat is eigenaar van de *providerabonnement standaard*.

De gebruiker die Azure Stack registreert is de eigenaar van de service-principal in Azure Active Directory. Alleen de gebruiker die zich hebben geregistreerd van Azure Stack kunt wijzigen van de registratie van de Azure Stack. Als een gebruiker niet-beheerders die geen eigenaar van de van registratieservice-principal probeert te registreren of Azure Stack opnieuw te registreren, kunnen ze een 403-antwoord optreden. Een 403-antwoord geeft aan dat de gebruiker heeft onvoldoende machtigingen om de bewerking te voltooien.

Als u geen Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Registreren van Azure Stack maakt geen kosten op uw Azure-abonnement.

### <a name="powershell-language-mode"></a>PowerShell-taalmodus

Als u wilt registreren is Azure Stack, de PowerShell-taalmodus moet worden ingesteld op **FullLanguageMode**.  Om te controleren dat de huidige taalmodus is ingesteld op full, opent u een PowerShell-venster met verhoogde bevoegdheid en voer de volgende PowerShell-cmdlets:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Zorg ervoor dat de uitvoer geeft als resultaat **FullLanguageMode**. Als een andere taalmodus wordt geretourneerd, registratie moet worden uitgevoerd op een andere computer of de taalmodus worden ingesteld moet op **FullLanguageMode** voordat u doorgaat.

### <a name="install-powershell-for-azure-stack"></a>PowerShell voor Azure Stack installeren

Gebruik de nieuwste versie van PowerShell voor Azure Stack te registreren bij Azure.

Als de meest recente versie niet al is geïnstalleerd, raadpleegt u [PowerShell voor Azure Stack installeren](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

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

### <a name="determine-a-unique-registration-name-to-use"></a>Bepaal de naam van een unieke registratie gebruiken 
Wanneer u Azure Stack met Azure hebt geregistreerd, moet u een unieke registratie-naam opgeven. Een eenvoudige manier om uw Azure Stack-abonnement koppelen aan een registratie van Azure, is met uw Azure Stack **Cloud-ID**. 

> [!NOTE]
> Azure Stack-registraties met behulp van het factureringsmodel op basis van capaciteit, moet de unieke naam wijzigen bij het opnieuw registreren nadat deze jaarlijkse abonnementen zijn verlopen, tenzij u [verwijderen van de registratie van de verlopen](azure-stack-registration.md#change-the-subscription-you-use) en opnieuw te registreren bij Azure.

Om te bepalen van de Cloud-ID voor uw Azure Stack-implementatie, opent u PowerShell als beheerder op een computer die u kunt toegang tot het eindpunt met bevoegde, voer de volgende opdrachten uit en noteer de **CloudID** waarde: 

```powershell
Run: Enter-PSSession -ComputerName <privileged endpoint computer name> -ConfigurationName PrivilegedEndpoint
Run: Get-AzureStackStampInformation 
```

## <a name="register-connected-with-pay-as-you-go-billing"></a>Verbonden met betalen per gebruik facturering registreren

Gebruik deze stappen om Azure Stack registreren bij Azure met behulp van het facturering model voor betalen als u-gebruik.

> [!Note]  
> Al deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot het eindpunt van de bevoegde (PEP heeft). Zie voor meer informatie over de PEP [met behulp van het eindpunt van de bevoegde in Azure Stack](azure-stack-privileged-endpoint.md).

Verbonden omgevingen kunnen toegang krijgen tot het internet en Azure. Voor deze omgevingen moet u de Azure Stack-resourceprovider registreren bij Azure en configureer vervolgens uw factureringsmodel.

1. Voor het registreren van de resourceprovider van Azure Stack met Azure, PowerShell ISE starten als beheerder en gebruik de volgende PowerShell-cmdlets met de **EnvironmentName** parameter ingesteld op het juiste Azure-abonnement-type (Zie onderstaande parameters).

2. Het Azure-account die u gebruikt voor het registreren van Azure Stack toevoegen. Als u wilt het account toevoegt, worden uitgevoerd de **Add-AzureRmAccount** cmdlet. U wordt gevraagd de referenties van uw Azure-account in te voeren en mogelijk moet u gebruikmaken van 2-factor-verificatie op basis van de configuratie van uw account.

   ```PowerShell
   Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parameter | Description |  
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

5. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u de hulpprogramma's voor Azure Stack hebt gedownload. Importeren van de **RegisterWithAzure.psm1** module met behulp van PowerShell:

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Vervolgens, in dezelfde PowerShell-sessie, zorg ervoor dat u bent aangemeld bij de juiste Context van Azure PowerShell. Dit is de Azure-account dat is gebruikt voor het eerder de Azure Stack-resourceprovider registreren. Als u PowerShell om uit te voeren:

   ```PowerShell  
   Connect-AzureRmAccount -Environment "<environment name>"
   ```

   | Parameter | Description |  
   |-----|-----|
   | EnvironmentName | De naam van de omgeving in de Azure-cloud-abonnement. Omgevingsnamen van de ondersteunde zijn **AzureCloud**, **AzureUSGovernment**, of als een Azure-abonnement van China **AzureChinaCloud**.  |

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

2. Het Azure-account die u gebruikt voor het registreren van Azure Stack toevoegen. Als u wilt het account toevoegt, worden uitgevoerd de **Add-AzureRmAccount** cmdlet. U wordt gevraagd de referenties van uw Azure-account in te voeren en mogelijk moet u gebruikmaken van 2-factor-verificatie op basis van de configuratie van uw account.

   ```PowerShell  
   Connect-AzureRmAccount -Environment "<environment name>"
   ```

   | Parameter | Description |  
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

5. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u de hulpprogramma's voor Azure Stack hebt gedownload. Importeren van de **RegisterWithAzure.psm1** module met behulp van PowerShell:

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
   > [!Note]  
   > U kunt uitschakelen voor gebruiksrapporten met de parameter UsageReportingEnabled voor de **Set AzsRegistration** cmdlet door de parameter op onwaar. 
   
  Zie voor meer informatie over de cmdlet Set-AzsRegistration [registratie verwijzing](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Niet-verbonden met een capaciteit facturering registreren

Als u Azure Stack in een omgeving zonder verbinding (met geen verbinding met internet) registreert, moet u een registratie-token ophalen uit de Azure Stack-omgeving en dat token vervolgens gebruiken op een computer die verbinding kan maken met Azure en PowerShell voor Azure Stack is geïnstalleerd.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Registratie van een toegangstoken ophalen uit de Azure Stack-omgeving

1. PowerShell ISE starten als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u de hulpprogramma's voor Azure Stack hebt gedownload. Importeren van de **RegisterWithAzure.psm1** module:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Als u het registratietoken, voer de volgende PowerShell-cmdlets:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -UsageReportingEnabled:$False -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   Zie voor meer informatie over de cmdlet Get-AzsRegistrationToken [registratie verwijzing](#registration-reference).

   > [!Tip]  
   > De registratietoken is opgeslagen in het bestand dat is opgegeven voor *$FilePathForRegistrationToken*. U kunt het bestandspad of de bestandsnaam naar eigen goeddunken wijzigen.

3. Sla deze voor gebruik in de Azure-registratietoken verbonden machine. U kunt het bestand of de tekst van $FilePathForRegistrationToken kopiëren.

### <a name="connect-to-azure-and-register"></a>Verbinding maken met Azure en registreren

Op de computer die is verbonden met Internet, moet u de dezelfde stappen voor het importeren van de module RegisterWithAzure.psm1 en zich aanmelden bij de juiste context van Azure Powershell uitvoeren. Roep vervolgens registreren AzsEnvironment. Geef de registratietoken te registreren bij Azure. Als u meer dan één exemplaar van Azure Stack met behulp van de dezelfde Azure-abonnements-ID registreert, moet u een unieke registratie-naam opgeven. Voer de volgende cmdlet uit:

  ```PowerShell  
  $RegistrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $RegistrationToken -RegistrationName $RegistrationName
  ```

U kunt de cmdlet Get-inhoud (optioneel) gebruiken om te verwijzen naar een bestand met uw registratietoken:

  ```PowerShell  
  $RegistrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $RegistrationToken -RegistrationName $RegistrationName
  ```

  > [!Note]  
  > De naam van de registratie-resource en de registratietoken voor toekomstig gebruik opslaan.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Haal een activeringscode van registratie van Azure-Resource

Vervolgens moet u een activeringscode ophalen van de registratie-resource in Azure tijdens de Register-AzsEnvironment gemaakt.

Als u de activeringscode, voer de volgende PowerShell-cmdlets:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
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

U kunt de **regiobeheer** tegel om te controleren of de Azure Stack-registratie geslaagd is. Deze tegel is beschikbaar op de standaard-dashboard in de beheerportal. De status kan geregistreerd of niet geregistreerd. Als geregistreerd, ook ziet u de ID van de Azure-abonnement dat u gebruikt voor het registreren van uw Azure-Stack samen met de registratie-resourcegroep en de naam.

1. Aanmelden bij de [Azure Stack-beheerportal](https://adminportal.local.azurestack.external).

2. Selecteer in het Dashboard, **regiobeheer**.

3. Selecteer **eigenschappen**. Deze blade ziet u de status en details van uw omgeving. De status kan zijn **geregistreerde** of **niet geregistreerd**.

    [![De tegel beheer regio](media/azure-stack-registration/admin1sm.png "regio management tegel")](media/azure-stack-registration/admin1.png#lightbox)

    Als u geregistreerd, voegt u de eigenschappen:
    
    - **Registratie abonnements-ID**: De Azure-abonnement-ID geregistreerd en die is gekoppeld aan Azure Stack
    - **Registratie-resourcegroep**: De Azure-resourcegroep in het gekoppelde abonnement met de Azure Stack-resources.

4. De Azure portal gebruiken om de Azure Stack-app-registraties weer te geven. Aanmelden bij de Azure-portal met een account dat is gekoppeld aan het abonnement dat u gebruikt voor het registreren van Azure Stack. Overschakelen naar de tenant die is gekoppeld aan Azure Stack.
5. Navigeer naar **Azure Active Directory > App-registraties > alle toepassingen weergeven**.

    ![App-registraties](media/azure-stack-registration/app-registrations.png)

    Azure Stack-app-registraties worden voorafgegaan door **Azure Stack**.

U kunt ook kunt u controleren of uw registratie geslaagd is met behulp van de Marketplace-management-functie. Als u een lijst met items voor de marketplace in de Marketplace-beheerblade ziet, is uw registratie is gelukt. Echter in niet-verbonden omgevingen zal u niet mogelijk om te zien van marketplace-items in beheer van Marketplace.

> [!NOTE]
> Nadat de registratie is voltooid, wordt de actieve waarschuwing voor het registreren van niet langer worden weergegeven. In niet-verbonden scenario's ziet u een bericht waarin wordt gevraagd u te registreren en activeren van uw Azure Stack, zelfs als u hebt geregistreerd voor beheer van Marketplace.

## <a name="renew-or-change-registration"></a>Vernieuwen of wijzigen van inschrijving

### <a name="renew-or-change-registration-in-connected-environments"></a>Vernieuwen of wijzigen van de registratie in verbonden omgevingen

U moet bijwerken of vernieuwen van de inschrijving in de volgende omstandigheden:

- Nadat u uw jaarlijkse op basis van capaciteit-abonnement te verlengen.
- Wanneer u uw factureringsmodel wijzigen.
- Wanneer u wijzigingen (knooppunten toevoegen of verwijderen) schalen voor facturering op basis van capaciteit.

#### <a name="change-the-subscription-you-use"></a>Wijzigen van het abonnement dat u gebruikt

Als u wilt wijzigen van het abonnement dat u gebruikt, moet u eerst uitvoeren de **Remove-AzsRegistration** cmdlet, zorg er dan voor u zijn aangemeld bij de juiste context van Azure PowerShell en voer ten slotte **Set AzsRegistration**  met een gewijzigde parameters waaronder \<factureringsmodel\>:

  ```PowerShell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Wijzig het factureringsmodel of hoe u functies bieden

Als u wilt wijzigen van de facturering van maandabonnementen of hoe u functies voor uw installatie bieden, kunt u de registratiefunctie om in te stellen de nieuwe waarden kunt aanroepen. U hoeft niet verwijdert u eerst de huidige registratie:

  ```PowerShell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
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
  $RegistrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $RegistrationToken
  ```

Of u kunt de naam van de registratie:

  ```Powershell
  $RegistrationName = "AzureStack-<unique-registration-name>"
  Unregister-AzsEnvironment -RegistrationName $RegistrationName
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

| Parameter | Type | Description |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | De referenties waarmee [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam bevindt zich in de indeling **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | String | Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekverzameling en andere post implementatietaken. Raadpleeg voor meer informatie, de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artikel. |
| AzureContext | PSObject |  |
| ResourceGroupName | String |  |
| ResourceGroupLocation | String |  |
| BillingModel | String | Het factureringsmodel die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: Capaciteit, PayAsYouUse en ontwikkeling. |
| MarketplaceSyndicationEnabled | Waar/onwaar | Hiermee bepaalt u of de marketplace-management-functie beschikbaar in de portal is. Ingesteld op true als registreren met een internetverbinding. Ingesteld op ONWAAR als registreren in niet-omgeving verbonden. Voor niet-verbonden registraties de [offline syndication hulpprogramma](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) kan worden gebruikt voor het downloaden van items voor de marketplace. |
| UsageReportingEnabled | Waar/onwaar | Azure Stack-standaard metrische gegevens over gebruik rapporten. Operators met een capaciteit gebruikt of ondersteunen van een niet-verbonden omgeving moet uitschakelen rapportage over het gebruik. Toegestane waarden voor deze parameter zijn: Waar, ONWAAR. |
| AgreementNumber | String |  |
| registrationName | String | Instellen van een unieke naam voor de inschrijving als u het script voor de registratie op meer dan één exemplaar van Azure Stack met behulp van de dezelfde Azure-abonnement-ID. De parameter heeft een standaardwaarde van **AzureStackRegistration**. Echter, als u dezelfde naam op meer dan één exemplaar van Azure Stack gebruikt, mislukt het script. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Get-AzsRegistrationToken genereert een registratietoken van de invoerparameters.

```PowerShell  
Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Parameter | Type | Description |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | De referenties waarmee [toegang tot het eindpunt met bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam bevindt zich in de indeling **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | String |  Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekverzameling en andere post implementatietaken. Raadpleeg voor meer informatie, de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artikel. |
| AzureContext | PSObject |  |
| ResourceGroupName | String |  |
| ResourceGroupLocation | String |  |
| BillingModel | String | Het factureringsmodel die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: Capaciteit, PayAsYouUse en ontwikkeling. |
| MarketplaceSyndicationEnabled | Waar/onwaar |  |
| UsageReportingEnabled | Waar/onwaar | Azure Stack-standaard metrische gegevens over gebruik rapporten. Operators met een capaciteit gebruikt of ondersteunen van een niet-verbonden omgeving moet uitschakelen rapportage over het gebruik. Toegestane waarden voor deze parameter zijn: Waar, ONWAAR. |
| AgreementNumber | String |  |

## <a name="registration-failures"></a>Registratiefouten

Mogelijk ziet u een van de onderstaande fouten tijdens het registreren van uw Azure Stack:
1. Kan de verplichte hardware-informatie voor $hostName niet ophalen. Controleer de fysieke host en connectiviteit en probeer het opnieuw uitvoeren registratie.
2. Kan geen verbinding maken met $hostName naar informatie over hardware -. fysieke host en -connectiviteit controleren en probeer het opnieuw uitvoeren registratie.

Oorzaak: Dit is meestal omdat we proberen te verkrijgen van hardware-informatie zoals de UUID, Bios CPU van hosts en de poging tot activering en kon niet worden vanwege het feit dat verbinding maakt met de fysieke host.

## <a name="next-steps"></a>Volgende stappen

[Marketplace-items van Azure downloaden](azure-stack-download-azure-marketplace-item.md)
