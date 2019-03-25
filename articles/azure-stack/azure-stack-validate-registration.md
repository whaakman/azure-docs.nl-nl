---
title: Valideren van de registratie van Azure voor Azure Stack | Microsoft Docs
description: Gebruik de Azure Stack-gereedheid van de vereisten voor het valideren van de registratie van Azure.
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
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: a777fc1d9052eb58bbebd319fe6cc7f42a09cb9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403609"
---
# <a name="validate-azure-registration"></a>Valideren van de registratie van Azure

Gebruik het hulpprogramma Azure Stack-gereedheid Checker (**AzsReadinessChecker**) om te valideren dat uw Azure-abonnement klaar is voor gebruik met Azure Stack. Registratie valideert voordat u begint met een Azure Stack-implementatie. De gereedheid van de vereisten gevalideerd die:

- Het Azure-abonnement met u is een ondersteund type. Abonnementen moet een Cloud Service Provider (CSP) of een Enterprise Agreement (EA).
- Het account waarmee u uw abonnement registreren bij Azure kan zich aanmelden bij Azure en is de eigenaar van een abonnement.

Zie voor meer informatie over Azure Stack-registratie, [Azure Stack registreren met Azure](azure-stack-registration.md).

## <a name="get-the-readiness-checker-tool"></a>De gereedheid van de Registercontrole ophalen

Download de nieuwste versie van **AzsReadinessChecker** uit de [PowerShell Gallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Vereisten

De volgende vereisten zijn vereist:

**De computer waarop het hulpprogramma wordt uitgevoerd:**

- Windows 10 of Windows Server 2016, met een internetverbinding.
- PowerShell 5.1 of hoger. De volgende PowerShell-cmdlet uitvoeren om te controleren of uw versie, en bekijk vervolgens de **belangrijke** en **kleine** versies:  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [PowerShell voor Azure Stack geconfigureerd](azure-stack-powershell-install.md).
- De nieuwste versie van [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker).  

**Azure Active Directory-omgeving:**

- Identificeer de gebruikersnaam en wachtwoord voor een account dat eigenaar van het Azure-abonnement dat u met Azure Stack gebruiken wilt.  
- Identificeer de abonnements-ID voor de Azure-abonnement die u wilt gebruiken.
- Identificeer de **AzureEnvironment** u wilt gebruiken. Ondersteunde waarden voor de parameter voor de omgeving zijn **AzureCloud**, **AzureChinaCloud**, of **AzureUSGovernment**, afhankelijk van welke Azure-abonnement bent u met behulp van.

## <a name="steps-to-validate-azure-registration"></a>Stappen voor het valideren van de registratie van Azure

1. Op een computer die voldoet aan de vereisten, open een verhoogde PowerShell-prompt en voer de volgende opdracht voor het installeren van **AzsReadinessChecker**:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. Uitvoeren van de PowerShell-prompt de volgende opdracht uit om `$registrationCredential` als het account dat is eigenaar van het abonnement. Vervang `subscriptionowner@contoso.onmicrosoft.com` met de naam van uw account en de tenant:

   ```powershell
   $registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"
   ```

   > [!NOTE]
   > Als een CSP, bij het gebruik van een gedeelde Services of IUR-abonnement, moet u de referenties van een gebruiker van die respectieve AAD opgeven. Meestal dit is vergelijkbaar met `subscriptionowner@iurcontoso.onmicrosoft.com`. Gebruiker moet de juiste referenties hebben, zoals beschreven in de vorige stap.

3. Voer de volgende om in te stellen van de PowerShell-prompt `$subscriptionID` als de Azure-abonnement te gebruiken. Vervang `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` met uw eigen abonnement-ID:

   ```powershell
   $subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
   ```

4. Voer de volgende opdracht om te beginnen de validatie van uw abonnement van de PowerShell-prompt:

   - Geef de waarde voor `AzureEnvironment` als **AzureCloud**, **AzureGermanCloud**, of **AzureChinaCloud**.  
   - Geef uw Azure Active Directory-beheerder en de naam van uw Azure Active Directory-tenant.

   ```powershell
   Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID
   ```

5. Nadat het hulpprogramma wordt uitgevoerd, Controleer de uitvoer. Controleer of dat de status is juist voor zowel aanmelden en de vereisten voor apparaatregistratie. Succesvolle validatie-uitvoer wordt weergegeven die vergelijkbaar is met het volgende voorbeeld:

   ```shell
   Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
   Checking Registration Requirements: OK
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsRegistrationValidation Completed
   ```

## <a name="report-and-log-file"></a>Rapport en een logboekbestand

Elke validatie wordt uitgevoerd, resultaten van deze vereistencontrole **AzsReadinessChecker.log** en **AzsReadinessCheckerReport.json**. De locatie van deze bestanden worden weergegeven, samen met de validatieresultaten in PowerShell.

Deze bestanden kunnen u helpen de validatiestatus delen voordat u Azure Stack implementeren of validatieproblemen onderzoeken. Beide bestanden behouden de resultaten van elke volgende validatiecontrole. Het rapport bevat de bevestiging van uw implementatie-team van de configuratie van de identiteit. Het logboekbestand kan helpen bij uw implementatie of ondersteuning team validatieproblemen onderzoeken.

Standaard beide bestanden worden geschreven naar **C:\Users\<gebruikersnaam > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json**.  

- Gebruik de **- OutputPath** ***&lt;pad&gt;*** parameter aan het einde van het uitvoeren vanaf de opdrachtregel om op te geven van een andere locatie.
- Gebruik de **- CleanReport** parameter aan het einde van de opdracht uitvoeren om te wissen van informatie over eerdere uitvoeringen van het hulpprogramma uit **AzsReadinessCheckerReport.json**.

Zie voor meer informatie, [Azure Stack-validatierapport](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Validatiefouten

Als een validatiecontrole mislukt, wordt informatie over de fout weergegeven in het PowerShell-venster. Het hulpprogramma registreert ook informatie naar de **AzsReadinessChecker.log** bestand.

De volgende voorbeelden geven richtlijnen op algemene validatiefouten:

### <a name="user-must-be-an-owner-of-the-subscription"></a>Gebruiker moet een eigenaar van het abonnement

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Oorzaak** -het account is niet een beheerder van het Azure-abonnement.

**Resolutie** -een account dat een beheerder van de Azure-abonnement dat wordt gefactureerd voor gebruik van de Azure Stack-implementatie gebruiken.

### <a name="expired-or-temporary-password"></a>Verlopen of tijdelijk wachtwoord

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription [subscription ID] using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change Password.
Trace ID: [Trace ID]
Correlation ID: [Correlation ID]
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Oorzaak** -het account kan niet aanmelden omdat het wachtwoord is verlopen of tijdelijk is.

**Resolutie** : Voer de volgende opdracht uit In PowerShell en volg de aanwijzingen voor het wachtwoord opnieuw instellen.

```powershell
Login-AzureRMAccount
```

U kunt ook aanmelden bij de [Azure-portal](https://portal.azure.com) als eigenaar van het account en de gebruiker gedwongen om het wachtwoord te wijzigen.

### <a name="unknown-user-type"></a>Onbekende gebruikerstype  

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription <subscription ID> using <account> failed with unknown_user_type: Unknown User Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Oorzaak** -het account kan niet aanmelden bij de opgegeven Azure Active Directory-omgeving. In dit voorbeeld **AzureChinaCloud** is opgegeven als de **AzureEnvironment**.  

**Resolutie** -bevestigen dat het account geldig voor de opgegeven Azure-omgeving is. Voer in PowerShell de volgende opdracht uit om te controleren of dat het account is geldig voor een specifieke omgeving:

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## <a name="next-steps"></a>Volgende stappen

- [Valideren van de Azure-identiteit](azure-stack-validate-identity.md)
- [Het rapport gereedheid weergeven](azure-stack-validation-report.md)
- [Aandachtspunten voor algemene Azure Stack-integratie](azure-stack-datacenter-integration.md)
