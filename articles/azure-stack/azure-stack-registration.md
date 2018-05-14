---
title: Azure registratie voor Azure-Stack geïntegreerd systemen | Microsoft Docs
description: Beschrijft het registratieproces Azure voor implementaties met meerdere knooppunten verbonden met een Azure-Stack Azure.
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
ms.date: 05/11/2018
ms.author: jeffgilb
ms.reviewer: avishwan
ms.openlocfilehash: f34c4697439685ce6ea0ce3f2c7e954ee81b5079
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure
Registreren van [Azure Stack](azure-stack-poc.md) met Azure kunt u voor het downloaden van marketplace-items van Azure en voor het instellen van rapportage terug naar Microsoft commerce-gegevens. Nadat u Azure-Stack geregistreerd, gebruik is gerapporteerd aan Azure commerce en kunt u deze bekijken in het abonnement dat u gebruikt voor registratie. 

> [!IMPORTANT]
> Registratie is vereist voor volledige functionaliteit van de Azure-Stack, met inbegrip van marketplace syndication ondersteuning. Bovendien kunt u zich in een overschrijding van de Azure-Stack licentievoorwaarden als u niet registreert bij gebruik van het facturering model pay-as-gebruik. Raadpleeg voor meer informatie over Azure-Stack licentieverlening van modellen, de [aanschaffen pagina](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Vereisten
Voordat u registreert Stack van Azure met Azure, moet u het volgende hebben:

- De abonnement-ID voor een Azure-abonnement. Als u de ID, zich aanmelden bij Azure, klikt u op **meer services** > **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de Abonnements-ID. 

  > [!NOTE]
  > Duitsland en -US Government cloudabonnementen zijn momenteel niet ondersteund.

- De gebruikersnaam en het wachtwoord voor een account dat is eigenaar van het abonnement (MSA/2FA accounts worden ondersteund).
- De Azure-Stack-resourceprovider geregistreerd (Zie de sectie Azure Stack-Resourceprovider registreren hieronder voor meer informatie).

Als u geen een Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Azure-Stack registreren maakt geen kosten op uw Azure-abonnement.

### <a name="bkmk_powershell"></a>Installeer PowerShell voor Azure Stack
U moet de meest recente PowerShell voor Azure-Stack gebruiken om te registreren met Azure.

Als u nog niet is geïnstalleerd, [Installeer PowerShell voor Azure-Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Download de Azure-Stack-tools
De Azure-Stack extra GitHub-opslagplaats bevat PowerShell-modules die ondersteuning bieden voor Azure-Stack functionaliteit; inclusief registratie-functionaliteit. Tijdens het registratieproces die u wilt importeren en gebruiken van de module PowerShell RegisterWithAzure.psm1 gevonden in de opslagplaats van de Azure-Stack hulpprogramma's voor uw Azure-Stack-exemplaar registreren bij Azure. 

Om te controleren of u de nieuwste versie gebruikt, verwijdert u alle bestaande versies van de Azure-Stack-hulpprogramma's en [de nieuwste versie downloaden vanuit GitHub](azure-stack-powershell-download.md) voordat u registreert met Azure.

## <a name="register-azure-stack-in-connected-environments"></a>Azure-Stack in verbonden omgevingen registreren
Verbonden omgevingen hebben toegang tot het internet en Azure. Voor deze omgevingen moet u de Azure-Stack-resourceprovider registreren bij Azure en configureer vervolgens het factureringsmodel.

> [!NOTE]
> Alle deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot de bevoegde eindpunt heeft. 

### <a name="register-the-azure-stack-resource-provider"></a>De Stack van Azure resourceprovider registreren
Als u wilt de Azure-Stack-resourceprovider registreren met Azure, PowerShell ISE start als beheerder en gebruik de volgende PowerShell-opdrachten met de **EnvironmentName** parameter ingesteld op het juiste Azure-abonnementstype (Zie onderstaande parameters). 

1. De Azure-account waarmee u Azure-Stack registreert toevoegen. Uitvoeren als u wilt toevoegen op het account, de **Add-AzureRmAccount** cmdlet. U wordt gevraagd de referenties van uw globale beheerder van Azure-account invoeren en wellicht 2-factor-verificatie op basis van de configuratie van uw account te gebruiken.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parameter | Beschrijving |  
   |-----|-----|
   | EnvironmentName | De naam van de omgeving in de Azure-cloud-abonnement. De omgevingsnamen van de ondersteunde zijn **AzureCloud** of, als een China Azure-abonnement met **AzureChinaCloud**.  |
   |  |  |

2. Als u meerdere abonnementen hebt, voert u de volgende opdracht om te selecteren die dat u wilt gebruiken:  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Voer de volgende opdracht om de Azure-Stack-resourceprovider registreren in uw Azure-abonnement:

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Azure-Stack registreren bij Azure met behulp van het facturering model pay-as-gebruik
Volg deze stappen om Azure-Stack registreren bij Azure met behulp van het facturering model pay-as-gebruik.

1. Start PowerShell ISE als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de Azure-Stack-hulpprogramma's](#bkmk_tools). Importeer de **RegisterWithAzure.psm1** module met behulp van PowerShell: 

  ```powershell
  Import-Module .\RegisterWithAzure.psm1
  ```

2. Vervolgens in dezelfde PowerShell-sessie, zorg ervoor dat u bent aangemeld bij de juiste Context van Azure PowerShell. Dit is de azure-account dat is gebruikt voor het registreren van de bovenstaande Azure-Stack resourceprovider. PowerShell om uit te voeren: 

  ```powershell 
  Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>" 
  ``` 

3. Voer in dezelfde PowerShell-sessie de **Set AzsRegistration** cmdlet. PowerShell om uit te voeren:  

  ```powershell
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse
  ```

  |Parameter|Beschrijving|
  |-----|-----|
  |PrivilegedEndpointCredential|De referenties waarmee [toegang tot de bevoegde eindpunt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam is in de notatie **AzureStackDomain\CloudAdmin**.|
  |PrivilegedEndpoint|Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekgegevens verzameld en andere post implementatietaken. Raadpleeg voor meer informatie de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) artikel.|
  |BillingModel|Het facturering model die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: capaciteit, PayAsYouUse en ontwikkeling.|
  |  |  |

  Het proces duurt tussen 10 en 15 minuten. Als de opdracht is voltooid, ziet u het bericht **'uw omgeving is nu geregistreerd en geactiveerd met behulp van de opgegeven parameters.'**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure-Stack registreren bij Azure met behulp van het facturering Capaciteitsmodel
Volg de instructies voor het registreren met behulp van het facturering model pay-as-gebruik gebruikt, maar het overeenkomstnummer waaronder capaciteit is gekocht toevoegen en wijzig de **BillingModel** -parameter voor **capaciteit**. Alle andere parameters zijn niet gewijzigd.

PowerShell om uit te voeren:

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Azure-Stack in niet-verbonden omgevingen registreren 
*De informatie in deze sectie geldt die begint met de updateversie Azure Stack 1712 (180106.1) en wordt niet ondersteund in eerdere versies.*

Als u Azure-Stack in een omgeving zonder verbinding (met zonder internetverbinding) registreert, moet u een registratie-token ophalen uit de Stack van Azure-omgeving en dat token vervolgens gebruiken op een computer die verbinding kan maken met Azure en heeft [PowerShell voor Azure-Stack geïnstalleerd](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Een registratie-token ophalen uit de Stack van Azure-omgeving

1. Start PowerShell ISE als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de Azure-Stack-hulpprogramma's](#bkmk_tools). Importeer de **RegisterWithAzure.psm1** module:  

  ```powershell 
  Import-Module .\RegisterWithAzure.psm1 
  ``` 

2. Als u het registratietoken, voer de volgende PowerShell-opdrachten:  

  ```Powershell
  $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
  $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
  ```
  
  > [!TIP]  
  > De registratietoken is opgeslagen in het bestand dat is opgegeven voor *$FilePathForRegistrationToken*. U kunt het bestandspad of de bestandsnaam naar eigen inzicht wijzigen. 

3. Sla dit registratietoken voor gebruik op de Azure verbonden computer. U kunt het bestand of de tekst van $FilePathForRegistrationToken kopiëren.


### <a name="connect-to-azure-and-register"></a>Verbinding maken met Azure en registreren
Op de computer die is verbonden met internet, voert u dezelfde stappen voor het importeren van de module RegisterWithAzure.psm1 en meld u aan in de juiste context van de Azure Powershell. Vervolgens registreren AzsEnvironment aanroepen en geef het inschrijvingstoken van de registratie te registreren met Azure:

  ```Powershell  
  $registrationToken = "<Your Registration Token>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```
Desgewenst kunt u de cmdlet Get-inhoud om te verwijzen naar een bestand met uw registratietoken:

  ```Powershell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```
  > [!NOTE]  
  > De naam van de registratie-resource en de registratietoken voor toekomstig gebruik opslaan.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Een activeringscode ophalen van de registratie van de Azure-Resource 
Vervolgens moet u een activeringscode ophalen van de registratie-resource gemaakt in Azure tijdens registratie AzsEnvironment. 
 
Als u de activeringssleutel, voer de volgende PowerShell-opdrachten:  

  ```Powershell 
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>" 
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt" 
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath 
  ``` 
  > [!TIP]   
  > De activeringscode wordt opgeslagen in het bestand dat is opgegeven voor *$KeyOutputFilePath*. U kunt het bestandspad of de bestandsnaam naar eigen inzicht wijzigen. 

### <a name="create-an-activation-resource-in-azure-stack"></a>Een activering-Resource maken in Azure-Stack 
Terug naar de Stack van Azure-omgeving met het bestand of de tekst van de activeringscode gemaakt op basis van Get-AzsActivationKey. Vervolgens maakt u een resource activering in Azure-Stack met deze activeringssleutel. De bron van een activering de volgende PowerShell-opdrachten maken:  

  ```Powershell 
  $ActivationKey = "<activation key>" 
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey 
  ``` 

Desgewenst kunt u de cmdlet Get-inhoud om te verwijzen naar een bestand met uw registratietoken: 

  ```Powershell   
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>' 
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey 
  ``` 

## <a name="verify-azure-stack-registration"></a>Azure-Stack registratie controleren
Volg deze stappen om te controleren dat Azure Stack met succes is geregistreerd bij Azure.
1. Aanmelden bij de Azure-Stack [beheerdersportal](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;regio >. &lt;FQDN-naam >*.
2. Klik op **meer Services** > **Marketplace Management** > **toevoegen vanuit Azure**.

Als u ziet een lijst met items die beschikbaar is via Azure (zoals WordPress), wordt de activering is geslaagd. Echter, in niet-verbonden omgevingen worden er geen Azure marketplace-items in de Stack van Azure marketplace.

> [!NOTE]
> Nadat de registratie voltooid is, wordt de actieve waarschuwing voor het registreren van niet langer weergegeven.

## <a name="renew-or-change-registration"></a>Vernieuw of inschrijving wijzigen
### <a name="renew-or-change-registration-in-connected-environments"></a>Vernieuw of inschrijving in verbonden omgevingen wijzigen
U wilt bijwerken of vernieuwen van uw registratie in de volgende omstandigheden:
- Nadat u uw capaciteit gebaseerde jaarlijkse abonnement verlengen.
- Wanneer u het factureringsmodel wijzigt.
- Wanneer u wijzigingen (knooppunten toevoegen of verwijderen) schalen voor facturering op basis van capaciteit.

#### <a name="change-the-subscription-you-use"></a>Wijzigen van het abonnement dat u gebruikt
Als u wilt wijzigen van het abonnement u wilt gebruiken, moet u eerst uitvoeren de **verwijderen AzsRegistration** cmdlet, zorg er vervolgens voor u zijn aangemeld bij de juiste context van de Azure PowerShell en voer ten slotte **Set AzsRegistration**  gewijzigd met een parameters:

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Wijzig de facturering model of syndication-functies
Als u wilt de facturering model of de functies voor uw installatie syndicatie wilt wijzigen, kunt u de registratiefunctie voor het instellen van de nieuwe waarden kunt aanroepen. U hoeft niet verwijdert u eerst de huidige registratie: 

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Vernieuwen of wijzigen van de registratie in niet-verbonden omgevingen 
U wilt bijwerken of vernieuwen van uw registratie in de volgende omstandigheden: 
- Nadat u uw capaciteit gebaseerde jaarlijkse abonnement verlengen. 
- Wanneer u het factureringsmodel wijzigt. 
- Wanneer u wijzigingen (knooppunten toevoegen of verwijderen) schalen voor facturering op basis van capaciteit. 

#### <a name="remove-the-activation-resource-from-azure-stack"></a>De resource activering uit Azure Stack verwijderen 
U moet eerst de resource activering verwijderen uit de Stack van Azure en de resource registratie in Azure.  

Voor het verwijderen van de resource activering in Azure-Stack, voer de volgende PowerShell-opdrachten in uw Azure-Stack-omgeving:  

  ```Powershell 
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint 
  ``` 

Vervolgens voor het verwijderen van de bron van de registratie in Azure, zorg ervoor dat u op een verbonden Azure computer aanmelden bij de juiste context van de Azure PowerShell en voer de juiste PowerShell-opdrachten, zoals hieronder wordt beschreven.

U kunt het registratietoken gebruikt voor het maken van de resource:  

  ```Powershell 
  $registrationToken = "<registration token>" 
  Unregister-AzsEnvironment -RegistrationToken $registrationToken 
  ``` 
  
Of u kunt de naam van de registratie: 

  ```Powershell 
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>" 
  Unregister-AzsEnvironment -RegistrationName $registrationName 
  ``` 

### <a name="re-register-using-disconnected-steps"></a>Opnieuw registreren met behulp van niet-verbonden stappen 
U hebt nu volledig registratie is verwijderd in een scenario met niet-verbonden en moet de stappen voor het registreren van een Azure-Stack-omgeving in een scenario met niet-verbonden. 

## <a name="next-steps"></a>Volgende stappen

[Downloaden van marketplace-items van Azure](azure-stack-download-azure-marketplace-item.md)
