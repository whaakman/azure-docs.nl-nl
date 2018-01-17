---
title: "Azure registratie voor Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: Beschrijft het registratieproces Azure voor implementaties met meerdere knooppunten verbonden met een Azure-Stack Azure.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 03a6ce77eed16cbc5d2fe46094b0b6ac7fbc022e
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure
U kunt Azure-Stack registreren met Azure marketplace-items van Azure downloaden en voor het instellen van rapportage terug naar Microsoft commerce-gegevens. Nadat u Azure-Stack geregistreerd, wordt informatie over het gebruik gerapporteerd aan Azure commerce. U kunt dit zien onder het abonnement dat u voor registratie gebruikt.

> [!IMPORTANT]
> Registratie is verplicht als u ervoor het facturering model pay-as-gebruik kiest. Anders kunt u zich in een overschrijding van de licentievoorwaarden van de implementatie van Azure-Stack zoals Gebruik anders niet gerapporteerd.

## <a name="before-you-register-azure-stack-with-azure"></a>Voordat u Azure-Stack met Azure registreren
Voordat u registreert Stack van Azure met Azure, moet u het volgende hebben:

- De abonnement-ID voor een Azure-abonnement. Als u de ID, zich aanmelden bij Azure, klikt u op **meer services** > **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de Abonnements-ID. 

  > [!NOTE]
  > China, Duitsland en US government-cloudabonnementen zijn momenteel niet ondersteund. 

- De gebruikersnaam en het wachtwoord voor een account dat is eigenaar van het abonnement (MSA/2FA accounts worden ondersteund)
- *Niet vereist vanaf Azure Stack 1712 updateversie (180106.1)*: de Azure AD voor de Azure-abonnement. U kunt deze map vinden in Azure door de muiswijzer op uw avatar in de rechterbovenhoek van de Azure portal. 
- De Azure-Stack-resourceprovider geregistreerd (Zie de sectie Azure Stack-Resourceprovider registreren hieronder voor meer informatie)

Als u geen een Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Azure-Stack registreren maakt geen kosten op uw Azure-abonnement.

### <a name="bkmk_powershell"></a>Installeer PowerShell voor Azure Stack
U moet het systeem registreren met Azure met de meest recente PowerShell voor Azure-Stack.

Als u nog niet is geïnstalleerd, [Installeer PowerShell voor Azure-Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Download de Azure-Stack-tools
De Azure-Stack extra GitHub-opslagplaats bevat PowerShell-modules die ondersteuning bieden voor Azure-Stack functionaliteit; inclusief registratie-functionaliteit. Tijdens de registratie moet u importeren en gebruiken van de module PowerShell RegisterWithAzure.psm1 aangetroffen in de opslagplaats van de Azure-Stack hulpprogramma's voor uw Azure-Stack-exemplaar registreren bij Azure. 

```powershell
# Change directory to the root directory. 
cd \

# Download the tools archive.
  invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
  expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
  cd AzureStack-Tools-master
```

## <a name="register-azure-stack-in-connected-environments"></a>Azure-Stack in verbonden omgevingen registreren
Verbonden omgevingen hebben toegang tot het internet en Azure. Voor deze omgevingen moet u de Azure-Stack-resourceprovider registreren bij Azure en configureer vervolgens het factureringsmodel.

### <a name="register-the-azure-stack-resource-provider"></a>De Stack van Azure resourceprovider registreren
Als u wilt de Azure-Stack-resourceprovider registreren met Azure, Powershell ISE start als beheerder en gebruik de volgende PowerShell-opdrachten. Deze opdrachten wordt:
- Vraagt u zich aanmelden als een eigenaar van het Azure-abonnement moet worden gebruikt en stel de `EnvironmentName` -parameter voor **AzureCloud**.
- Registreer de provider van de Azure-resource **Microsoft.AzureStack**.

PowerShell om uit te voeren:

```powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack 
```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Azure-Stack registreren bij Azure met behulp van het facturering model pay-as-gebruik
Gebruik de volgende stappen om Azure-Stack registreren bij Azure met behulp van het facturering model pay-as-gebruik.

Start PowerShell ISE als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de Azure-Stack-hulpprogramma's](#bkmk_tools). Importeer de **RegisterWithAzure.psm1** module met behulp van PowerShell: 

PowerShell om uit te voeren:

```powershell
Import-Module .\RegisterWithAzure.psm1
```
Voer vervolgens in dezelfde PowerShell-sessie de **Set AzsRegistration** cmdlet. Wanneer dit wordt gevraagd om referenties, geeft u de eigenaar van het Azure-abonnement.  

PowerShell om uit te voeren:

```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Azure subscription owner>  -Message "Enter the cloud domain credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -BillingModel PayAsYouUse
```

|Parameter|Beschrijving|
|-----|-----|
|CloudAdminCredential|PowerShell-object dat referentie-informatie (gebruikersnaam en wachtwoord) voor de eigenaar van het Azure-abonnement bevat.|
|PrivilegedEndpoint|Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekgegevens verzameld en andere post implementatietaken. Raadpleeg voor meer informatie de [met behulp van het eindpunt van de bevoegde](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint#access-the-privileged-endpoint) artikel.|
|BillingModel|Het facturering model die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn: capaciteit, PayAsYouUse en ontwikkeling.|

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure-Stack registreren bij Azure met behulp van het facturering Capaciteitsmodel
Volg de instructies voor het registreren met behulp van het facturering model pay-as-gebruik gebruikt, maar het overeenkomstnummer waaronder capaciteit is gekocht toevoegen en wijzig de `BillingModel` -parameter voor **capaciteit**. Alle andere parameters zijn niet gewijzigd.

PowerShell om uit te voeren:
```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Azure subscription owner>  -Message "Enter the cloud domain credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Azure-Stack in niet-verbonden omgevingen registreren 
*De informatie in deze sectie geldt die begint met de updateversie Azure Stack 1712 (180106.1) en wordt niet ondersteund in eerdere versies.*

Als u Azure-Stack in een omgeving zonder verbinding (met zonder internetverbinding) registreert, moet u een registratie-token ophalen uit de Stack van Azure-omgeving en dat token vervolgens gebruiken op een computer die verbinding kan maken met Azure en heeft [PowerShell voor Azure-Stack geïnstalleerd](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Een registratie-token ophalen uit de Stack van Azure-omgeving
  1. Als u het registratietoken, voer de volgende PowerShell-opdrachten:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > De registratietoken is opgeslagen in het bestand dat is opgegeven voor *$env:SystemDrive\RegistrationToken.txt*.

  2. Sla dit registratietoken voor gebruik op de Azure verbonden computer.


### <a name="connect-to-azure-and-register"></a>Verbinding maken met Azure en registreren
Start PowerShell ISE als beheerder en navigeer naar de **registratie** map in de **AzureStack-hulpprogramma's-master** directory gemaakt wanneer u [gedownload van de Azure-Stack-hulpprogramma's](#bkmk_tools). Importeer de **RegisterWithAzure.psm1** module: 

PowerShell om uit te voeren:
```powershell
Import-Module .\RegisterWithAzure.psm1
```
Geef vervolgens de registratietoken registreren bij Azure in dezelfde PowerShell-sessie:

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
> Sla de resourcenaam registratie of het registratietoken voor toekomstig gebruik.

## <a name="verify-azure-stack-registration"></a>Azure-Stack registratie controleren
Volg deze stappen om te controleren dat Azure Stack met succes is geregistreerd bij Azure.
1. Aanmelden bij de Azure-Stack [beheerdersportal](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https &#58; / / adminportal. *&lt;regio >. &lt;FQDN-naam >*.
2. Klik op **meer Services** > **Marketplace Management** > **toevoegen vanuit Azure**.

Als u ziet een lijst met items die beschikbaar is via Azure (zoals WordPress), wordt de activering is geslaagd.

> [!NOTE]
> Nadat de registratie voltooid is, wordt de actieve waarschuwing voor het registreren van niet langer weergegeven.

## <a name="renew-or-change-registration"></a>Vernieuw of inschrijving wijzigen
U wilt bijwerken of vernieuwen van uw registratie in de volgende omstandigheden:
- Nadat u uw capaciteit gebaseerde jaarlijkse abonnement verlengen.
- Wanneer u het factureringsmodel wijzigt.
- Wanneer u wijzigingen (knooppunten toevoegen of verwijderen) schalen voor facturering op basis van capaciteit.

### <a name="change-the-subscription-you-use"></a>Wijzigen van het abonnement dat u gebruikt
Als u wilt wijzigen van het abonnement u wilt gebruiken, moet u eerst uitvoeren de **verwijderen AzsRegistration** cmdlet, zorg er vervolgens voor u zijn aangemeld bij de juiste context van de Azure PowerShell en voer ten slotte **Set AzsRegistration**  gewijzigd met een parameters:

```powershell
Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
Set-AzureRmContext -SubscriptionId $NewSubscriptionId
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

### <a name="change-the-billing-model-or-syndication-features"></a>Wijzig de facturering model of syndication-functies
Als u wilt de facturering model of de functies voor uw installatie syndicatie wilt wijzigen, kunt u de registratiefunctie voor het instellen van de nieuwe waarden kunt aanroepen. U hoeft niet verwijdert u eerst de huidige registratie: 

```powershell
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

## <a name="remove-a-registered-resource"></a>Verwijderen van een geregistreerde bron
Als u wilt verwijderen van een registratie, dan moet u **UnRegister AzsEnvironment** cmdlet en geeft u de naam van de registratie-resource of de registratie van de token u hebt gebruikt voor **registreren-AzsEnvironment**.

Een registratie met de naam van een bron verwijderen:

```Powershell    
UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
```
Een registratie met een registratietoken verwijderen:

```Powershell
$registrationToken = "*Your copied registration token*"
UnRegister-AzsEnvironment -RegistrationToken $registrationToken
```

