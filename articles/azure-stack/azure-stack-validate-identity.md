---
title: Valideren van de Azure-identiteit voor Azure Stack | Microsoft Docs
description: Gebruik de Azure Stack-gereedheid van de vereisten om Azure-identiteit te valideren.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/04/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/04/2018
ms.openlocfilehash: 4fb636a91389309b44f2308efec1a6c257c41078
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242583"
---
# <a name="validate-azure-identity"></a>Valideren van de Azure-identiteit 
Gebruik het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) om te valideren dat uw Azure Active Directory (Azure AD) klaar is voor gebruik met Azure Stack. Uw Azure identity-oplossing te valideren voordat u begint met een Azure Stack-implementatie.  

De gereedheid van vereisten wordt gevalideerd:
 - Azure Active Directory (Azure AD) als een id-provider voor Azure Stack.
 - De Azure AD-account dat u van plan bent te gebruiken kunt als een globale beheerder van uw Azure Active Directory aanmelden. 

Validatie garandeert dat uw omgeving klaar is voor Azure Stack voor het opslaan van informatie over gebruikers, toepassingen, groepen en service-principals van Azure Stack in uw Azure AD is.

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen
Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid van de Registercontrole (AzsReadinessChecker) van de [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Vereisten
De volgende vereisten moeten worden voldaan.

**De computer waarop het hulpprogramma wordt uitgevoerd:**
 - Windows 10 of Windows Server 2016, met een internetverbinding.
 - PowerShell 5.1 of hoger. Voer de volgende PowerShell-opdracht om te controleren of uw versie, en bekijk vervolgens de *belangrijke* versie en *kleine* versies:  

   > `$PSVersionTable.PSVersion`
 - Configureer [PowerShell voor Azure Stack](azure-stack-powershell-install.md). 
 - De nieuwste versie van [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

**Azure Active Directory-omgeving:**
 - Identificeer de Azure AD-account u wilt gebruiken voor Azure Stack en zorg ervoor dat het een Azure Active Directory globale beheerder is.
 - Geef de naam van uw Azure AD-Tenant. Naam van de tenant moet de *primaire* domeinnaam voor uw Azure Active Directory. Bijvoorbeeld, *contoso.onmicrosoft.com*. 
 - Identificeer de Azure-omgeving u gebruikt. Ondersteunde waarden voor de parameter voor de omgeving zijn AzureCloud, AzureChinaCloud of AzureUSGovernment, afhankelijk van welke Azure-abonnement u gebruikt.

## <a name="validate-azure-identity"></a>Valideren van de Azure-identiteit 
1. Open een PowerShell-prompt met beheerdersrechten op een computer die voldoet aan de vereisten, en voer de volgende opdracht voor het installeren van de AzsReadinessChecker:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. Voer de volgende om in te stellen van de PowerShell-prompt *$serviceAdminCredential* als de servicebeheerder voor uw Azure AD-Tenant.  Vervang *serviceadmin@contoso.onmicrosoft.com* met uw account en de tenant. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. Voer de volgende voor het starten van de validatie van uw Azure AD vanuit de PowerShell-prompt. 
   - Geef de waarde van de omgeving voor Azure-omgeving. Ondersteunde waarden voor de parameter voor de omgeving zijn AzureCloud, AzureChinaCloud of AzureUSGovernment, afhankelijk van welke Azure-abonnement u gebruikt.  
   - Geef uw Azure Active Directory-Tenantnaam vervangen *contoso.onmicrosoft.com*. 

   > `Invoke-AzsAzureIdentityValidation -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment <environment name> -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer of de status **OK** voor installatie-eisen. Een succesvolle validatie wordt weergegeven zoals in de volgende afbeelding: 
 
```PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: OK

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```


## <a name="report-and-log-file"></a>Rapport en een logboekbestand
Elke validatie wordt uitgevoerd, resultaten van deze vereistencontrole **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell.

Deze bestanden kunnen u helpen de validatiestatus delen voordat u Azure Stack implementeren of validatieproblemen onderzoeken.  Beide bestanden behouden de resultaten van elke volgende validatiecontrole. Het rapport bevat de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kan helpen bij uw implementatie of ondersteuning team validatieproblemen onderzoeken. 

Standaard beide bestanden worden geschreven naar *C:\Users\<gebruikersnaam > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Gebruik de **- OutputPath** ***&lt;pad&gt;*** parameter aan het einde van het uitvoeren vanaf de opdrachtregel om op te geven van een andere locatie.   
 - Gebruik de **- CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen van gegevens uit *AzsReadinessCheckerReport.json*.  over eerdere uitvoeringen van het hulpprogramma. 

Voor meer informatie, [Azure Stack-validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Validatiefouten
Als een validatiecontrole mislukt, wordt informatie over de fout weergegeven in het PowerShell-venster. Het hulpprogramma wordt ook informatie in de AzsReadinessChecker.log geregistreerd.

De volgende voorbeelden geven richtlijnen op algemene validatiefouten.

### <a name="expired-or-temporary-password"></a>Verlopen of tijdelijk wachtwoord 
 
```PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail 
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The password for account  has expired or is a temporary password that needs to be reset before continuing. Run Login-AzureRMAccount, login with  credentials and follow the prompts to reset.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```
**Oorzaak** -het account kan niet aanmelden omdat het wachtwoord is verlopen of tijdelijk is.     

**Resolutie** : Voer In PowerShell de volgende en volg de aanwijzingen voor het wachtwoord opnieuw instellen.  
> `Login-AzureRMAccount`

U kunt ook, meld u aan bij https://portal.azure.com als het account en de gebruiker gedwongen om het wachtwoord te wijzigen.
### <a name="unknown-user-type"></a>Onbekende gebruikerstype 
 
```PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail 
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
Unknown user type detected. Check the account  is valid for AzureChinaCloud
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```
**Oorzaak** -het account zich niet aanmelden bij de opgegeven Azure Active Directory (AADDirectoryTenantName). In dit voorbeeld *AzureChinaCloud* is opgegeven als de *AzureEnvironment*.

**Resolutie** -bevestigen dat het account geldig voor de opgegeven Azure-omgeving is. Voer in PowerShell het volgende om te controleren of dat het account is geldig voor een specifieke omgeving:   Login-AzureRmAccount – EnvironmentName AzureChinaCloud 
### <a name="account-is-not-an-administrator"></a>Account is niet een beheerder 
 
```PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail 
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The Service Admin account you entered 'admin@contoso.onmicrosoft.com' is not an administrator of the Azure Active Directory tenant 'contoso.onmicrosoft.com'.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**Oorzaak** -Hoewel het account zich aanmelden kan, het account is niet een beheerder van de Azure Active Directory (AADDirectoryTenantName).  

**Resolutie** -Meld u aan bij https://portal.azure.com als het account, gaat u naar **Azure Active Directory** > **gebruikers** > *selecteert u de gebruiker*  >  **Maprol**, en controleer vervolgens of de gebruiker een **hoofdbeheerder**.  Als het account een gebruiker is, gaat u naar **Azure Active Directory** > **aangepast domein** namen en bevestigt u dat de naam u hebt opgegeven voor *AADDirectoryTenantName* is gemarkeerd als de primaire domeinnaam op voor deze map.  In dit voorbeeld is dat *contoso.onmicrosoft.com*. 

Azure Stack is vereist dat de domeinnaam de primaire domeinnaam is.

## <a name="next-steps"></a>Volgende stappen
[Valideren van de registratie van Azure](azure-stack-validate-registration.md)  
[Het rapport gereedheid weergeven](azure-stack-validation-report.md)  
[Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)  

