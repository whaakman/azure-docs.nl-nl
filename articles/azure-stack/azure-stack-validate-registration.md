---
title: Azure registratie valideert voor Azure-Stack | Microsoft Docs
description: Gebruik de controle van Azure Stack gereedheid voor het valideren van Azure registratie.
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
ms.openlocfilehash: 84eb1c08cc3f9ef104e2eb0b96ed397315c3f374
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="validate-azure-registration"></a>Azure registratie valideert 
Gebruik het hulpprogramma Azure Stack gereedheid Checker (AzsReadinessChecker) om te valideren dat uw Azure-abonnement gereed is voor gebruik met Azure-Stack. Registratie valideert voordat u begint met de implementatie van een Azure-Stack. De controle van de gereedheid van de evalueert:
- Het Azure-abonnement dat u gebruikt, is een ondersteund type. Abonnementen moet een Cloud Service Provider (CSP) of een Enterprise Agreement (EA). 
- Het account waarmee u uw abonnement te registreren met Azure kunt aanmelden bij Azure en is de eigenaar van een abonnement. 

Zie voor meer informatie over het registreren van de Azure-Stack [Azure Stack registreren met Azure](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen
Download de nieuwste versie van het hulpprogramma Azure Stack gereedheid Checker (AzsReadinessChecker) is beschikbaar op [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Vereisten
De volgende vereisten moeten aanwezig zijn.

**De computer waarop het hulpprogramma wordt uitgevoerd:**
 - Windows 10 of Windows Server 2016 met een internetverbinding.
 - PowerShell 5.1 of hoger. Voer de volgende PowerShell-cmd en bekijk vervolgens te controleren of uw versie, de *belangrijke* versie en *secundaire* versies:  

    >`$PSVersionTable.PSVersion` 
 - Configureer [PowerShell voor Azure Stack](azure-stack-powershell-install.md). 
 - Download de nieuwste versie van [Microsoft Azure-Stack gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.  

**Azure Active Directory-omgeving:**
 - Identificeer de gebruikersnaam en het wachtwoord voor een account dat is eigenaar van het Azure-abonnement dat u met Azure-Stack gebruiken wilt.  
 - Identificeer de abonnements-ID voor de Azure-abonnement die u wilt gebruiken. 
 - Identificeren van de AzureEnvironment die u wilt gebruiken: *AzureCloud*, *AzureGermanCloud*, of *AzureChinaCloud*.

## <a name="validate-azure-registration"></a>Azure registratie valideert
1. Op een computer die voldoet aan de vereisten, open een administratieve PowerShell-prompt en voer de volgende opdracht voor het installeren van de AzsReadinessChecker.
    > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. Voer de volgende om in te stellen van de PowerShell-prompt *$registrationCredential* als het account dat is eigenaar van het abonnement.   Vervang *subscriptionowner@contoso.onmicrosoft.com* aan de tenant en uw account. 
    > `$registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"`

3. Voer de volgende om in te stellen van de PowerShell-prompt *$subscriptionID* als het Azure-abonnement u wilt gebruiken. Vervang *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* met uw eigen abonnement-ID.  
     > `$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` 

4. Voer de volgende voor het starten van de validatie van uw abonnement van de PowerShell-prompt 
   - Geef de waarde voor AzureEnvironment als *AzureCloud*, *AzureGermanCloud*, of *AzureChinaCloud*.  
   - Geef uw Azure Active Directory-beheerder en de naam van uw Azure Active Directory-Tenant. 

   > `Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer dat de status OK is voor aanmelding en de vereisten voor registratie. Een geslaagde validatie wordt weergegeven zoals in de volgende afbeelding:  
![validatie van uitvoeren](./media/azure-stack-validate-registration/registration-validation.png)


## <a name="report-and-log-file"></a>Rapport en een logboekbestand
Elke validatie wordt uitgevoerd, geregistreerd resultaten in het logboek **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden wordt weergegeven met de validatieresultaten in PowerShell. 

Deze bestanden kunt u de validatiestatus delen voordat u Azure-Stack implementeren of validatie problemen onderzoeken. Beide bestanden bewaard de resultaten van elke latere validatiecontrole. Het rapport bevat de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kunt u uw implementatie of ondersteuning team validatieproblemen onderzoeken. 

Standaard worden beide bestanden geschreven naar *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Gebruik de **- OutputPath** ***&lt;pad&gt;*** parameter aan het einde van de uitgevoerde opdrachtregel op te geven van een andere locatie.   
 - Gebruik de **- CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen van gegevens uit *AzsReadinessCheckerReport.json*.  over vorige van het hulpprogramma kan worden uitgevoerd. Voor meer informatie [Azure Stack validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Mislukte gegevensvalidatie
Als een validatiecontrole mislukt, wordt informatie over de fout weergegeven in het PowerShell-venster. Het hulpprogramma registreert ook informatie naar de AzsReadinessChecker.log.

De volgende voorbeelden op validatiefouten algemene richtlijnen te bieden.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Gebruiker moet een eigenaar van het abonnement   
![eigenaar van het abonnement](./media/azure-stack-validate-registration/subscription-owner.png)
**oorzaak** -het account is geen beheerder van de Azure-abonnement.   

**Resolutie** -een account gebruiken dat een beheerder van de Azure-abonnement dat wordt gefactureerd voor gebruik van de Azure-Stack-implementatie.


### <a name="expired-or-temporary-password"></a>Verlopen of tijdelijke wachtwoord 
![verlopen wachtwoord](./media/azure-stack-validate-registration/expired-password.png)
**oorzaak** -het account kan niet aanmelden omdat het wachtwoord is verlopen of tijdelijk is.     

**Resolutie** - In PowerShell uitvoeren en volg de aanwijzingen voor het wachtwoord opnieuw instellen. 
  > `Login-AzureRMAccount` 

U kunt ook de aanmelding bij https://portal.azure.com als het account en de gebruiker gedwongen het wachtwoord te wijzigen.


### <a name="microsoft-accounts-are-not-supported-for-registration"></a>Microsoft-accounts worden niet ondersteund voor registratie  
![niet-ondersteunde account](./media/azure-stack-validate-registration/unsupported-account.png)
**oorzaak** -een Microsoft-account (zoals Outlook.com of Hotmail.com) is opgegeven.  Deze accounts worden niet ondersteund.

**Resolutie** -gebruik een account en abonnement vanuit een Cloud Service Provider (CSP) of een Enterprise Agreement (EA). 


### <a name="unknown-user-type"></a>Onbekende gebruikerstype  
![Onbekend gebruiker](./media/azure-stack-validate-registration/unknown-user.png)
**oorzaak** -het account kan niet aanmelden bij de opgegeven Azure Active Directory-omgeving. In dit voorbeeld *AzureChinaCloud* is opgegeven als de *AzureEnvironment*.  

**Resolutie** -Controleer of de account ongeldig voor de opgegeven Azure-omgeving is. Voer in PowerShell de volgende om te controleren of dat het account is geldig voor een specifieke omgeving.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Volgende stappen
[Azure identiteit valideren](azure-stack-validate-identity.md)
[weergeven van het rapport gereedheid](azure-stack-validation-report.md)
[aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)

