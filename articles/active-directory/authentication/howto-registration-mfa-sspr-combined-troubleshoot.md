---
title: Gecombineerde registratie voor Azure AD SSPR en MFA (preview) oplossen
description: Problemen oplossen met Azure AD multi-factor Authentication en Self-service voor wachtwoord opnieuw instellen van gecombineerde registratie (preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: b247a733c6874b023d3dc8114b0538e422baccfd
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56589011"
---
# <a name="troubleshooting-combined-security-information-registration-preview"></a>Het oplossen van gecombineerde security informatie registratie (preview)

De informatie in dit artikel kan beheerders voor het oplossen van problemen met het gecombineerde registratie-ervaring die is gerapporteerd door hun eindgebruikers dienen als richtlijn.

|     |
| --- |
| Registratie van de gecombineerde security informatie voor Azure multi-factor Authentication en Azure AD Self-service voor wachtwoord opnieuw instellen is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="audit-logs"></a>Controlelogboeken

De gebeurtenissen die zijn vastgelegd voor gecombineerde registratie zijn onder de categorie 'Verificatiemethoden' in de Azure AD auditlogboeken.

![Interface met beveiligingsvoorzieningen informatiegebeurtenissen registratie voor een nieuwe gebruiker in de map Azure AD-auditlogboeken](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

Hieronder vindt u alle controlegebeurtenissen die worden gegenereerd door de registratie van de gecombineerde:

| Activiteit | Status | Reden | Description |
| --- | --- | --- | --- |
| De gebruiker geregistreerd alle vereiste beveiligingsgegevens | Geslaagd | De gebruiker geregistreerd alle vereiste beveiligingsgegevens. | Deze gebeurtenis treedt op wanneer een gebruiker heeft de registratie is voltooid.|
| De gebruiker geregistreerd alle vereiste beveiligingsgegevens | Fout | Registratie is geannuleerd door de gebruiker security info. | Deze gebeurtenis treedt op wanneer een gebruiker de registratie van de interrupt modus annuleert.|
| Beveiligingsgegevens voor de gebruiker is geregistreerd | Geslaagd | De gebruiker geregistreerd "method". | Deze gebeurtenis treedt op wanneer een gebruiker zich registreert voor een afzonderlijke methode. "Method" mag Authenticator-app, telefoonnummer, e-mailadres, vragen over de beveiliging, App-wachtwoord, alternatief telefoonnummer, enzovoort.| 
| Beveiligingsgegevens van de gebruiker gecontroleerd | Geslaagd | Gebruiker is gecontroleerd op beveiligingsgegevens. | Deze gebeurtenis treedt op wanneer een gebruiker op 'Ziet er goed' op de pagina beveiligingsgegevens controleren.|
| Beveiligingsgegevens van de gebruiker gecontroleerd | Fout | Kan de gebruiker niet beveiligingsgegevens te controleren. | Deze gebeurtenis treedt op wanneer een gebruiker op 'Ziet er goed uit' op de beveiligingsgegevens pagina bekijken, maar er een storing optreedt in de back-end.|
| Beveiligingsgegevens gebruiker verwijderd | Geslaagd | Gebruiker heeft "method" verwijderd. | Deze gebeurtenis treedt op wanneer een gebruiker een afzonderlijke methode verwijdert. "Method" mag Authenticator-app, telefoonnummer, e-mailadres, vragen over de beveiliging, App-wachtwoord, alternatief telefoonnummer, enzovoort.|
| Beveiligingsgegevens gebruiker verwijderd | Fout | Kan de gebruiker niet verwijderen "method". | Deze gebeurtenis treedt op wanneer een gebruiker probeert te verwijderen van een methode om een bepaalde reden mislukt. "Method" mag Authenticator-app, telefoonnummer, e-mailadres, vragen over de beveiliging, App-wachtwoord, alternatief telefoonnummer, enzovoort.|
| Gebruiker gewijzigd standaard beveiligingsgegevens | Geslaagd | Standaard beveiligingsgegevens gebruiker gewijzigd in "method". | Deze gebeurtenis treedt op wanneer een gebruiker de standaard-methode wijzigt. "Method" kunt verificator-app-melding, code uit mijn authenticator-app of token-aanroep + X XXXXXXXXXX, tekst een code naar + X XXXXXXXXX, enzovoort.|
| Gebruiker gewijzigd standaard beveiligingsgegevens | Fout | Kan de gebruiker niet wijzigen in standaard beveiligingsgegevens "method". | Deze gebeurtenis treedt op wanneer een gebruiker probeert de standaardmethode wijzigen, maar om een bepaalde reden mislukt. "Method" mag melding in de Authenticator-app, een code uit mijn authenticator-app of token, aanroep + X XXXXXXXXXX, tekst een code naar + X XXXXXXXXX, enzovoort.|

## <a name="troubleshooting-interrupt-mode"></a>Het oplossen van problemen interrupt-modus

| Symptoom | Stappen voor probleemoplossing |
| --- | --- |
| Ik zie niet de methoden die ik verwacht te zien. | 1. Controleer of de gebruiker de rol van een Azure AD-beheerder heeft. Zo ja, Controleer de SSPR-beheerder beleid verschillen. <br> 2. Bepalen of de gebruiker wordt onderbroken vanwege het afdwingen van MFA-registratie of afdwingen van de SSPR-registratie. Bekijk de stroomdiagram onder gecombineerde registratie modi om te bepalen welke methoden moeten worden weergegeven. <br> 3. Bepalen hoe recent de MFA- of SSPR-beleid is gewijzigd. Als de wijziging recente is, is het duurt even voordat het bijgewerkte beleid worden doorgegeven.|

## <a name="troubleshooting-manage-mode"></a>Modus voor het oplossen van beheren

| Symptoom | Stappen voor probleemoplossing |
| --- | --- |
| Ik hoef niet de optie voor het toevoegen van een bepaalde methode. | 1. Bepalen of de methode voor MFA of voor self-service voor Wachtwoordherstel is ingeschakeld. <br> 2. Als de methode is ingeschakeld, slaat u het beleid opnieuw op en wacht 1-2 uur voordat u het opnieuw. <br> 3. Als de methode is ingeschakeld, zorg ervoor dat de gebruiker van het maximale aantal van deze methode waarmee ze zijn toegestaan voor het instellen van nog niet ingesteld.|

## <a name="disable-combined-registration"></a>Gecombineerde registratie uitschakelen

Wanneer een gebruiker hun telefoonnummer en/of de mobiele app in de nieuwe registreert gecombineerde ervaring, onze stempels service een set van vlaggen (StrongAuthenticationMethods) voor deze methoden op die gebruiker. Deze functionaliteit kan de gebruiker om uit te voeren van Azure multi-factor Authentication (MFA) met deze methoden wanneer MFA vereist is.

De methoden die gebruikers via de nieuwe ervaring registreren hebben de eigenschap StrongAuthenticationMethods is ingesteld. Als een beheerder kan de Preview-versie, gebruikers via de nieuwe ervaring registreren en de beheerder vervolgens de Preview-versie schakelt, kunnen gebruikers onbewust worden geregistreerd voor MFA ook.

Als een gebruiker die is voltooid gecombineerd registratie navigeert u naar de huidige Self-service voor wachtwoord opnieuw instellen (SSPR) registratiepagina, op [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), ze wordt gevraagd om uit te voeren MFA voordat ze krijgen deze pagina tot toegang. Deze stap is verwacht gedrag technische vanuit het oogpunt van, maar voor gebruikers die eerder zijn geregistreerd voor SSPR alleen, is deze stap een nieuw gedrag. Hoewel deze extra stap van de gebruiker het beveiligingspostuur verbeteren door te geven van een extra beveiligingsniveau, kunnen beheerders wilt terugdraaien hun gebruikers zodat ze niet meer zijn geschikt voor MFA uitvoeren.  

### <a name="how-to-roll-back-users"></a>Gebruikers herstellen

Als u als beheerder instellen van een gebruiker MFA-instellingen wilt, hebben we een PowerShell-script dat de eigenschap StrongAuthenticationMethods voor mobiele app van een gebruiker en/of telefoonnummer worden gewist gemaakt. Uitvoeren van dit script voor uw gebruikers, betekent dat ze moet u opnieuw wilt registreren voor MFA indien nodig. U wordt aangeraden terugdraaien met één of twee gebruikers testen voorafgaand aan de betrokken gebruikers worden teruggedraaid.

De volgende stappen ziet u terugdraaien van een gebruiker of groep van gebruikers:

#### <a name="prerequisites"></a>Vereisten

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

#### <a name="rollback"></a>Ongedaan maken

Voer de volgende opdracht uit na het bijwerken van de gemarkeerde locaties in een PowerShell-venster. Voer hoofdbeheerdersreferenties in wanneer hierom wordt gevraagd. Het script wordt het resultaat van elke gebruiker update-bewerking uitvoeren.

`<script location> -path <user file location>`

### <a name="disable-preview-experience"></a>Preview-ervaring uitschakelen

Als u wilt uitschakelen in de preview-ervaring voor uw gebruikers, voert u de volgende stappen uit:

1. Aanmelden bij de Azure portal aan als een globale beheerder of Gebruikerbeheerder.
2. Blader naar **Azure Active Directory**, **gebruikersinstellingen**, **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligingsgegevens**, de kiezer ingesteld op **geen** en klikt u op **opslaan**.

Gebruikers wordt niet meer gevraagd om u te registreren met behulp van de preview-ervaring.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over de openbare preview van gecombineerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication](concept-registration-mfa-sspr-combined.md)
