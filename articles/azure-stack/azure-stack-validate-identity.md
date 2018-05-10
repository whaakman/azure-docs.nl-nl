---
title: Azure identiteit te verifiëren voor Azure Stack | Microsoft Docs
description: De Stack gereedheid van de controle van Azure gebruiken om Azure identiteit te valideren.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: fe5e7281cbe01ad11f667729df344f91ef1327d2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="validate-azure-identity"></a>Azure identiteit valideren 
Gebruik het hulpprogramma Azure Stack gereedheid Checker (AzsReadinessChecker) om te valideren dat uw Azure Active Directory (Azure AD) klaar is voor gebruik met Azure-Stack. Uw Azure identiteitsoplossing valideren voordat u begint met de implementatie van een Azure-Stack.  

De controle van de gereedheid van de evalueert:
 - Azure Active Directory (Azure AD) als een id-provider voor Azure-Stack.
 - Het Azure AD-account dat u wilt gebruiken, kan zich aanmelden als globale beheerder van uw Azure Active Directory. 

Validatie zorgt ervoor dat uw omgeving gereed is voor Azure-Stack voor het opslaan van informatie over gebruikers, toepassingen, groepen en service-principals uit Azure-Stack in uw Azure AD.

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen
Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid Checker (AzsReadinessChecker) van de [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Vereisten
De volgende vereisten moeten aanwezig zijn.

**De computer waarop het hulpprogramma wordt uitgevoerd:**
 - Windows 10 of Windows Server 2016 met een internetverbinding.
 - PowerShell 5.1 of hoger. Voer de volgende PowerShell-cmd en bekijk vervolgens te controleren of uw versie, de *belangrijke* versie en *secundaire* versies:  

   > `$PSVersionTable.PSVersion`
 - Configureer [PowerShell voor Azure Stack](azure-stack-powershell-install.md). 
 - De nieuwste versie van [Microsoft Azure-Stack gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

**Azure Active Directory-omgeving:**
 - De Azure AD-account u wilt gebruiken voor Azure-Stack en controleer of het een Azure Active Directory-hoofdbeheerder identificeren.
 - Geef de naam van uw Azure AD-Tenant. Naam van de tenant moet de *primaire* domeinnaam voor uw Azure Active Directory. Bijvoorbeeld: *contoso.onmicrosoft.com*. 
 - Identificeren van de AzureEnvironement die u wilt gebruiken: *AzureCloud*, *AzureGermanCloud*, of *AzureChinaCloud*.

## <a name="validate-azure-identity"></a>Azure identiteit valideren 
1. Op een computer die voldoet aan de vereisten, open een administratieve PowerShell een opdrachtprompt en voer vervolgens de volgende opdracht voor het installeren van de AzsReadinessChecker:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. Voer de volgende om in te stellen van de PowerShell-prompt *$serviceAdminCredential* als de servicebeheerder voor uw Azure AD-Tenant.  Vervang *serviceadmin@contoso.onmicrosoft.com* aan de tenant en uw account. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. Voer de volgende voor het starten van de validatie van uw Azure AD van de PowerShell-prompt. 
   - Geef de waarde voor AzureEnvironment als *AzureCloud*, *AzureGermanCloud*, of *AzureChinaCloud*.  
   - Geef uw Azure Active Directory-Tenantnaam vervangen *contoso.onmicrosoft.com*. 

   > `Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer de status is **OK** voor aanmelding en de installatievereisten voldoet. Een geslaagde validatie wordt weergegeven zoals in de volgende afbeelding: 
 
![validatie van uitvoeren](./media/azure-stack-validate-identity/validation.png)


## <a name="report-and-log-file"></a>Rapport en een logboekbestand
Elke validatie wordt uitgevoerd, geregistreerd resultaten in het logboek **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell.

Deze bestanden kunt u de validatiestatus delen voordat u Azure-Stack implementeren of validatie problemen onderzoeken.  Beide bestanden bewaard de resultaten van elke latere validatiecontrole. Het rapport bevat de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kunt u uw implementatie of ondersteuning team validatieproblemen onderzoeken. 

Standaard worden beide bestanden geschreven naar *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Gebruik de **- OutputPath** ***&lt;pad&gt;*** parameter aan het einde van de uitgevoerde opdrachtregel op te geven van een andere locatie.   
 - Gebruik de **- CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen van gegevens uit *AzsReadinessCheckerReport.json*.  over vorige van het hulpprogramma kan worden uitgevoerd. 

Voor meer informatie [Azure Stack validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Mislukte gegevensvalidatie
Als een validatiecontrole mislukt, wordt informatie over de fout weergegeven in het PowerShell-venster. Het hulpprogramma registreert ook informatie naar de AzsReadinessChecker.log.

De volgende voorbeelden op validatiefouten algemene richtlijnen te bieden.

### <a name="expired-or-temporary-password"></a>Verlopen of tijdelijke wachtwoord 
 
![wachtwoord verlopen](./media/azure-stack-validate-identity/expired-password.png)
**oorzaak** -het account kan niet aanmelden omdat het wachtwoord is verlopen of tijdelijk is.     

**Resolutie** : Voer de volgende In PowerShell en volg de aanwijzingen voor het wachtwoord opnieuw instellen.  
> `Login-AzureRMAccount`

U kunt ook de aanmelding bij https://portal.azure.com als het account en de gebruiker gedwongen het wachtwoord te wijzigen.
### <a name="unknown-user-type"></a>Onbekende gebruikerstype 
 
![Onbekende gebruiker](./media/azure-stack-validate-identity/unknown-user.png)
**oorzaak** -het account kan niet aanmelden bij de opgegeven Azure Active Directory (AADDirectoryTenantName). In dit voorbeeld *AzureChinaCloud* is opgegeven als de *AzureEnvironment*.

**Resolutie** -Controleer of de account ongeldig voor de opgegeven Azure-omgeving is. Voer in PowerShell de volgende om te controleren of het account is geldig voor een specifieke omgeving: Login-AzureRmAccount – EnvironmentName AzureChinaCloud 
### <a name="account-is-not-an-administrator"></a>Account is geen beheerder 
 
![niet-beheerder](./media/azure-stack-validate-identity/not-admin.png)

**Oorzaak** -Hoewel het account zich aanmelden kan, het account is niet een beheerder van de Azure Active Directory (AADDirectoryTenantName).  

**Resolutie** -aanmelding in https://portal.azure.com het account, gaat u naar **Azure Active Directory** > **gebruikers** > *selecteert u de gebruiker*  >  **Functie directory**, en controleer vervolgens of de gebruiker een **hoofdbeheerder**.  Als het account een gebruiker is, gaat u naar **Azure Active Directory** > **aangepaste domeinen** namen en Bevestig dat de naam u hebt opgegeven voor *AADDirectoryTenantName* is Als de primaire domeinnaam voor deze directory gemarkeerd.  In dit voorbeeld is dat *contoso.onmicrosoft.com*. 

Azure-Stack is vereist dat de domeinnaam de naam van het primaire domein is.

## <a name="next-steps"></a>Volgende stappen
[Azure registratie valideert](azure-stack-validate-registration.md)  
[Het rapport gereedheid weergeven](azure-stack-validation-report.md)  
[Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)  

