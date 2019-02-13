---
title: Eenmalige wachtwoordcode verificatie voor gastgebruikers voor B2B - Azure Active Directory | Microsoft Docs
description: Het gebruik van de eenmalige wachtwoordcode e-mailadres voor verificatie van B2B-gastgebruikers zonder de noodzaak voor een Microsoft-account.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 1/25/2019
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9493f7ee3278bb42dc21574cd008fbe2f4376a1
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56185142"
---
# <a name="email-one-time-passcode-authentication-preview"></a>E-mailbericht eenmalige wachtwoordcode verificatie (preview)

|     |
| --- |
| Eenmalige wachtwoordcode per e-mail is een openbare preview-functie van Azure Active Directory. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.|
|     |

De functie voor e-mailbericht eenmalige wachtwoordcode verifieert B2B-gastgebruikers wanneer ze niet worden geverifieerd via een andere manier, bijvoorbeeld Azure AD, een Microsoft-account (MSA) of Google federation. Met verificatie van de eenmalige wachtwoordcode is er niet nodig om een Microsoft-account te maken. Wanneer de gastgebruiker een uitnodiging wordt ingewisseld of toegang heeft tot een gedeelde bron, kunnen ze een tijdelijke code, die wordt verzonden naar het e-mailadres aanvragen. Ze voert deze code om door te gaan met het aanmelden.

Deze functie is momenteel beschikbaar voor Preview-versie (Zie [inschrijving voor de Preview-versie](#opting-in-to-the-preview) hieronder). Na de Preview-versie, wordt deze functie ingeschakeld standaard voor alle tenants.

> [!NOTE]
> Eenmalige wachtwoordcode gebruikers moeten aanmelden met een koppeling met de context van de tenant (bijvoorbeeld `https://myapps.microsoft.com/?tenantid=<tenant id>` of `https://portal.azure.com/<tenant id>`, of in geval van een geverifieerd domein `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`). Directe koppelingen naar toepassingen en bronnen ook werken, zolang ze de context van de tenant zijn. Gastgebruikers ook kunnen zijn op dit moment kan niet aanmelden met behulp van eindpunten waarvoor geen tenant-context. Bijvoorbeeld, met behulp van `https://myapps.microsoft.com`, `https://portal.azure.com`, of het algemene Teams-eindpunt resulteert in een fout. 

## <a name="user-experience-for-one-time-passcode-guest-users"></a>Gebruikerservaring voor gastgebruikers eenmalige wachtwoordcode
Met verificatie van de eenmalige wachtwoordcode, kan de gastgebruiker de uitnodiging inwisselen door een directe koppeling te klikken of met behulp van de uitnodiging per e-mail. In beide gevallen moet een bericht in de browser geeft aan dat een code naar e-mailadres van de gastgebruiker verzonden. De gastgebruiker selecteert **code naartoe sturen**:
 
   ![Toegangsdeelvensters beheren app](media/one-time-passcode/otp-send-code.png)
 
Een wachtwoordcode wordt verzonden naar e-mailadres van de gebruiker. De gebruiker de wachtwoordcode van het e-mailbericht opgehaald en ingevoerd in het browservenster:
 
   ![Toegangsdeelvensters beheren app](media/one-time-passcode/otp-enter-code.png)
 
De gastgebruiker wordt nu geverifieerd en ze kunnen zien van de gedeelde bron of doorgaan met aanmelden. 

> [!NOTE]
> Eenmalige wachtwoordcodes zijn geldig gedurende 30 minuten. Na 30 minuten, dat specifieke eenmalige wachtwoordcode is niet meer geldig en moet een nieuwe aanvraag van de gebruiker. Gebruikerssessies verlopen na 24 uur. De gastgebruiker ontvangt een nieuwe wachtwoordcode na die tijd, wanneer ze toegang krijgen de resource tot. Verlopen van de sessie biedt extra beveiliging, met name wanneer een gastgebruiker hun bedrijf verlaat of niet langer toegang nodig heeft.

## <a name="when-does-a-guest-user-get-a-one-time-passcode"></a>Wanneer een eenmalige wachtwoordcode in een gastgebruiker krijgen?

Wanneer een gastgebruiker een uitnodiging wordt ingewisseld, of een koppeling naar een resource die is gedeeld met hen gebruikt, wordt er een eenmalige wachtwoordcode als:
- Ze hebben niet een Azure AD-account 
- Ze hebben niet een Microsoft-account 
- De uitnodigende tenant niet ingesteld Google Federatie voor @gmail.com en @googlemail.com gebruikers 

Op het moment van inschrijving is er geen indicatie dat de gebruiker die u nodigen eenmalige wachtwoordcode verificatie gebruikt. Maar wanneer de gastgebruiker zich aangemeld heeft, verificatie van de eenmalige wachtwoordcode wordt de alternatieve methode als er geen andere verificatiemethoden kunnen worden gebruikt. 

U kunt gastgebruikers die worden geverifieerd met eenmalige wachtwoordcodes in Azure portal door te gaan naar weergeven **Azure Active Directory** > **organisatie relaties**  >   **Gebruikers van andere organisaties**.

![Eenmalige wachtwoordcode gebruikers weergeven in Azure portal waarbij bron gelijk is aan OTP](media/one-time-passcode/otp-users.png)

> [!NOTE]
> Wanneer een gebruiker een eenmalige wachtwoordcode wordt ingewisseld en later een MSA, Azure AD-account of andere federatieve-account verkrijgt, blijven deze gewoon worden geverifieerd met een eenmalige wachtwoordcode. Als u bijwerken hun verificatiemethode wilt, kunt u hun gastgebruikersaccount verwijderen en ze kunt uitnodigen.

### <a name="example"></a>Voorbeeld
Gastgebruiker alexdoe@gmail.com wordt uitgenodigd voor Fabrikam, waarvoor geen Google federation instellen. Alex beschikt niet over een Microsoft-account. Hij ontvangt een eenmalige wachtwoordcode voor verificatie.

## <a name="opting-in-to-the-preview"></a>Inschrijving voor de Preview-versie 
Het duurt een paar minuten voor de actie opt-in te voeren. Hierna wordt alleen de zojuist uitgenodigde gebruikers die voldoen aan de bovenstaande voorwaarden eenmalige wachtwoordcode verificatie gebruiken. Gastgebruikers die eerder ingewisseld een uitnodiging wordt echter ook doorgaan met de dezelfde methode voor verificatie.

### <a name="to-opt-in-using-the-azure-ad-portal"></a>Aanmelden met behulp van de Azure AD-portal
1.  Aanmelden bij de [Azure-portal](https://portal.azure.com/) als een globale beheerder van Azure AD.
2.  Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3.  Onder **beheren**, selecteer **organisatie relaties**.
4.  Selecteer **instellingen**.
5.  Onder **e eenmalige wachtwoordcode voor gasten (Preview) inschakelen**, selecteer **Ja**.
 
### <a name="to-opt-in-using-powershell"></a>Aanmelden met behulp van PowerShell

U moet eerst de meest recente versie van de Azure AD PowerShell voor Graph-module (AzureADPreview) installeren. Vervolgens bepaalt u of beleid voor B2B al bestaan en voer de juiste opdrachten uit.

#### <a name="prerequisite-install-the-latest-azureadpreview-module"></a>Voorwaarde: Installeer de nieuwste versie van de AzureADPreview-module
Controleer eerst welke modules die u hebt geïnstalleerd. Open PowerShell als een gebruiker met verhoogde bevoegdheid (Uitvoeren als administrator) en voer de volgende opdracht uit:
 
```powershell  
Get-Module -ListAvailable AzureAD*
```

Als de AzureADPreview-module zonder bericht verschijnt dat aangeeft dat er een nieuwere versie bestaat, bent u klaar. Anders doet u op basis van de uitvoer, het volgende:

- Als er geen resultaten worden geretourneerd, voert u de volgende opdracht uit om de AzureADPreview-module te installeren:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Als alleen de AzureAD-module wordt weergegeven in de resultaten, voer de volgende opdrachten uit om de AzureADPreview-module installeren: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- Als alleen de AzureADPreview-module wordt weergegeven in de resultaten, maar u een bericht ontvangt dat aangeeft dat er een latere versie, voer de volgende opdrachten uit om de module bij te werken: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
  ```

Mogelijk krijgt u een opdrachtprompt dat u de module vanuit een niet-vertrouwde opslagplaats installeert. Dit gebeurt als u de opslagplaats PSGallery eerder niet hebt ingesteld als een vertrouwde opslagplaats. Druk op **Y** om de module te installeren.

#### <a name="check-for-existing-policies-and-opt-in"></a>Controleren op bestaande beleidsregels en opt-in

Controleer vervolgens om te zien als een B2BManagementPolicy momenteel door het uitvoeren van de volgende bestaat:

```powershell 
$currentpolicy =  Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
$currentpolicy -ne $null
```
- Als de uitvoer ONWAAR is, bestaat het beleid nog niet. Maak een nieuwe B2BManagementPolicy en meldt zich aan voor de Preview-versie door het uitvoeren van de volgende:

   ```powershell 
   $policyValue=@("{`"B2BManagementPolicy`":{`"PreviewPolicy`":{`"Features`":[`"OneTimePasscode`"]}}}")
   New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true
   ```

- Als de uitvoer waar is, wordt het beleid B2BManagementPolicy momenteel bestaat. Voor het bijwerken van het beleid en meldt zich aan voor de Preview-versie, voert u het volgende:
  
   ```powershell 
   $policy = $currentpolicy.Definition | ConvertFrom-Json
   $features=[PSCustomObject]@{'Features'=@('OneTimePasscode')}; $policy.B2BManagementPolicy | Add-Member 'PreviewPolicy' $features -Force; $policy.B2BManagementPolicy
   $updatedPolicy = $policy | ConvertTo-Json -Depth 3
   Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
   ```

## <a name="opting-out-of-the-preview-after-opting-in"></a>Wanneer u geen gebruik van de Preview-versie nadat in
Het duurt een paar minuten voor de actie opt-out pas van kracht. Als u de Preview-versie uitschakelt, wordt alle gastgebruikers die een eenmalige wachtwoordcode hebt ingewisseld wordt pas weer aan te melden. U kunt de gastgebruiker verwijderen en kunt u de gebruiker om te kunnen aanmelden met een andere verificatiemethode uitnodigen.

### <a name="to-turn-off-the-preview-using-the-azure-ad-portal"></a>De Preview-versie met behulp van de Azure AD-portal uitschakelen
1.  Aanmelden bij de [Azure-portal](https://portal.azure.com/) als een globale beheerder van Azure AD.
2.  Selecteer in het navigatiedeelvenster **Azure Active Directory**.
3.  Onder **beheren**, selecteer **organisatie relaties**.
4.  Selecteer **instellingen**.
5.  Onder **e eenmalige wachtwoordcode voor gasten (Preview) inschakelen**, selecteer **Nee**.

### <a name="to-turn-off-the-preview-using-powershell"></a>De Preview-versie met behulp van PowerShell uitschakelen
De meest recente AzureADPreview-module installeren als u dit nog niet hebt (Zie [vereiste: Installeer de meest recente AzureADPreview-module](#prerequisite-install-the-latest-azureadpreview-module) hierboven). Controleer vervolgens of dat het beleid van de Preview-versie eenmalige wachtwoordcode momenteel door het uitvoeren van de volgende bestaat:

```powershell 
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
($currentPolicy -ne $null) -and ($currentPolicy.Definition -like "*OneTimePasscode*")
```

Als de uitvoer waar is, opt-out voor de Preview-versie door het uitvoeren van de volgende:

```powershell 
$policy = $currentpolicy.Definition | ConvertFrom-Json
$policy.B2BManagementPolicy.PreviewPolicy.Features = $policy.B2BManagementPolicy.PreviewPolicy.Features.Where({$_ -ne "OneTimePasscode"})
$updatedPolicy = $policy | ConvertTo-Json -Depth 3
Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
```

