---
title: Geconvergeerde registratie uitschakelen voor Azure AD SSPR en MFA (openbare preview)
description: Azure AD multi-factor Authentication uitschakelen en de self-service voor wachtwoord opnieuw instellen van inschrijving (openbare preview)
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: 6a51b1a3050b37fdcc822006f9e25d6662c65fb2
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426349"
---
# <a name="disable-azure-ad-converged-registration-public-preview"></a>Schakel Azure AD geconvergeerde registratie (openbare preview)

Wanneer een gebruiker hun telefoonnummer en/of de mobiele app in de nieuwe registreert geconvergeerd ervaring, onze stempels service een set van vlaggen (StrongAuthenticationMethods) voor deze methoden op die gebruiker. Deze functionaliteit kan de gebruiker om uit te voeren van Azure multi-factor Authentication (MFA) met deze methoden wanneer MFA vereist is.

De methoden die gebruikers via de nieuwe ervaring registreren hebben de eigenschap StrongAuthenticationMethods is ingesteld. Dit probleem doet zich ook voor als openbare preview-versie beschikbaar is. Als een beheerder kan de Preview-versie, gebruikers via de nieuwe ervaring registreren en de beheerder vervolgens de Preview-versie schakelt, kunnen gebruikers onbewust worden geregistreerd voor MFA ook.

Als een gebruiker die is voltooid geconvergeerd registratie navigeert u naar de registratiepagina van de huidige SSPR op [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), ze wordt gevraagd om uit te voeren MFA voordat ze krijgen deze pagina tot toegang. Deze stap is verwacht gedrag technische vanuit het oogpunt van, maar voor gebruikers die eerder zijn geregistreerd voor SSPR alleen, is deze stap een nieuw gedrag. Hoewel deze extra stap van de gebruiker het beveiligingspostuur verbeteren door te geven van een extra beveiligingsniveau, kunnen beheerders wilt terugdraaien hun gebruikers zodat ze niet meer zijn geschikt voor MFA uitvoeren.  

## <a name="how-to-roll-back-users"></a>Gebruikers herstellen

Als u als beheerder instellen van een gebruiker MFA-instellingen wilt, hebben we een PowerShell-script dat de eigenschap StrongAuthenticationMethods voor mobiele app van een gebruiker en/of telefoonnummer worden gewist gemaakt. Uitvoeren van dit script voor uw gebruikers, betekent dat ze moet u opnieuw wilt registreren voor MFA indien nodig. U wordt aangeraden terugdraaien met één of twee gebruikers testen voorafgaand aan de betrokken gebruikers worden teruggedraaid.

De volgende stappen ziet u terugdraaien van een gebruiker of groep van gebruikers:

### <a name="pre-requisites"></a>Vereisten

1. U moet de juiste Azure AD PowerShell-modules installeren. Voer deze opdrachten om de modules te installeren in een PowerShell-venster:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Sla de lijst met betrokken gebruikersobject-ID /-id's op uw computer als een tekstbestand met één ID per regel. Noteer de locatie van het bestand.
1. Sla het volgende script op uw computer en noteer de locatie van het script:

```powershell
<# 
//********************************************************
//*                                                      *
//*   Copyright (C) Microsoft. All rights reserved.      *
//*                                                      *
//********************************************************
#>

param($path)

# Define Remediation Fn
function RemediateUser {

    param  
    (
        $ObjectId
    )

    $user = Get-MsolUser -ObjectId $ObjectId

    Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

    $hasMfaRelyingParty = $false
    foreach($p in $user.StrongAuthenticationRequirements)
    {
        if ($p.RelyingParty -eq "*")
        {
            $hasMfaRelyingParty = $true
            Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
        }
    }

    if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
    {
        Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
        Write-Host "Rolling back user ..." -ForegroundColor Yellow
        Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
        Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
    }
    else
    {
        Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
    }

    Write-Host ""
    Start-Sleep -Milliseconds 750
}

# Connect
Import-Module MSOnline
Connect-MsolService

foreach($line in Get-Content $path)
{
    RemediateUser -ObjectId $line
}
```

### <a name="rollback"></a>Ongedaan maken

Voer de volgende opdracht uit na het bijwerken van de gemarkeerde locaties in een PowerShell-venster. Voer hoofdbeheerdersreferenties in wanneer hierom wordt gevraagd. Het script wordt het resultaat van elke gebruiker update-bewerking uitvoeren.

`<script location> -path <user file location>`

## <a name="disable-preview-experience"></a>Preview-ervaring uitschakelen

Als u wilt uitschakelen in de preview-ervaring voor uw gebruikers, voert u de volgende stappen uit:

1. Aanmelden bij de Azure portal aan als een globale beheerder of Gebruikerbeheerder.
2. Blader naar **Azure Active Directory**, **gebruikersinstellingen**, **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligingsgegevens**, de kiezer ingesteld op **geen** en klikt u op **opslaan**.

Gebruikers wordt niet meer gevraagd om u te registreren met behulp van de preview-ervaring.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de openbare preview van geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication](concept-registration-mfa-sspr-converged.md)