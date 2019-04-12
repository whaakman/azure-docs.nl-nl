---
title: Gecombineerde registratie voor Azure AD SSPR en meervoudige verificatie (preview) - Azure Active Directory oplossen
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
ms.openlocfilehash: 40918493071fe0dd694c43e2b087a2bf7eb197d8
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59489190"
---
# <a name="troubleshooting-combined-security-information-registration-preview"></a>Het oplossen van gecombineerde security informatie registratie (preview)

De informatie in dit artikel is bedoeld voor beheerders die het oplossen van problemen die worden gerapporteerd door gebruikers van de gecombineerde registratie-ervaring.

|     |
| --- |
| Gecombineerde security informatie registratie voor meervoudige verificatie en selfservice voor wachtwoord opnieuw instellen van Azure Active Directory (Azure AD) is een openbare preview-functie van Azure AD. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.|
|     |

## <a name="audit-logs"></a>Auditlogboeken

De gebeurtenissen die zijn vastgelegd voor gecombineerde registratie zijn in de categorie verificatiemethoden in de Azure AD auditlogboeken.

![Azure AD-auditlogboeken interface weergeven inschrijving gebeurtenissen](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

De volgende tabel geeft een lijst van alle controlegebeurtenissen die worden gegenereerd door de registratie van de gecombineerde:

| Activiteit | Status | Reden | Description |
| --- | --- | --- | --- |
| De gebruiker geregistreerd alle vereiste beveiligingsgegevens | Geslaagd | De gebruiker geregistreerd alle vereiste beveiligingsgegevens. | Deze gebeurtenis treedt op wanneer een gebruiker heeft de registratie is voltooid.|
| De gebruiker geregistreerd alle vereiste beveiligingsgegevens | Fout | Registratie is geannuleerd door de gebruiker security info. | Deze gebeurtenis treedt op wanneer een gebruiker de registratie van de interrupt modus annuleert.|
| Beveiligingsgegevens voor de gebruiker is geregistreerd | Geslaagd | De gebruiker geregistreerd *methode*. | Deze gebeurtenis treedt op wanneer een gebruiker zich registreert voor een afzonderlijke methode. *Methode* mag verificator-app, telefoon, e-mailbericht, Security vragen, App wachtwoord, alternatief telefoonnummer, enzovoort.| 
| Beveiligingsgegevens van de gebruiker gecontroleerd | Geslaagd | Gebruiker is gecontroleerd op beveiligingsgegevens. | Deze gebeurtenis treedt op wanneer een gebruiker selecteert **ziet er goed uit** op de pagina beveiligingsgegevens controleren.|
| Beveiligingsgegevens van de gebruiker gecontroleerd | Fout | Kan de gebruiker niet beveiligingsgegevens te controleren. | Deze gebeurtenis treedt op wanneer een gebruiker selecteert **ziet er goed uit** Controleer op de beveiligingsgegevens pagina, maar er een storing optreedt op de back-end.|
| Beveiligingsgegevens gebruiker verwijderd | Geslaagd | Gebruiker verwijderd *methode*. | Deze gebeurtenis treedt op wanneer een gebruiker een afzonderlijke methode verwijdert. *Methode* mag verificator-app, telefoon, e-mailbericht, Security vragen, App wachtwoord, alternatief telefoonnummer, enzovoort.|
| Beveiligingsgegevens gebruiker verwijderd | Fout | Kan de gebruiker niet verwijderen *methode*. | Deze gebeurtenis treedt op wanneer een gebruiker probeert te verwijderen van een methode, maar de poging om een bepaalde reden mislukt. *Methode* mag verificator-app, telefoon, e-mailbericht, Security vragen, App wachtwoord, alternatief telefoonnummer, enzovoort.|
| Gebruiker gewijzigd standaard beveiligingsgegevens | Geslaagd | Gebruiker heeft de standaard-beveiligingsgegevens voor gewijzigd *methode*. | Deze gebeurtenis treedt op wanneer een gebruiker de standaard-methode wijzigt. *Methode* kunt melding in de Authenticator-app, een code uit mijn authenticator-app of token-aanroep + X XXXXXXXXXX, tekst worden een code naar + X XXXXXXXXX, enzovoort.|
| Gebruiker gewijzigd standaard beveiligingsgegevens | Fout | Kan de gebruiker niet wijzigen van de standaard-beveiligingsgegevens voor *methode*. | Deze gebeurtenis treedt op wanneer een gebruiker probeert te wijzigen van de standaardmethode voor, maar de poging om een bepaalde reden mislukt. *Methode* kunt melding in de Authenticator-app, een code uit mijn authenticator-app of token-aanroep + X XXXXXXXXXX, tekst worden een code naar + X XXXXXXXXX, enzovoort.|

## <a name="troubleshooting-interrupt-mode"></a>Het oplossen van problemen interrupt-modus

| Symptoom | Stappen voor probleemoplossing |
| --- | --- |
| Ik zie niet de methoden die ik verwacht te zien. | 1. Controleer of de gebruiker een Azure AD-beheerdersrol heeft. Als u Ja kiest, moet u de SSPR-beheerder beleid verschillen weergeven. <br> 2. Bepalen of de gebruiker wordt onderbroken vanwege het afdwingen van multi-factor Authentication-registratie of afdwingen van de SSPR-registratie. Zie de [stroomdiagram](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md#combined-registration-modes) onder 'Gecombineerde registratie modi' om te bepalen welke methoden moeten worden weergegeven. <br> 3. Bepalen hoe recent de multi-factor Authentication of SSPR-beleid is gewijzigd. Als de wijziging recente is, is het duurt even voordat het bijgewerkte beleid worden doorgegeven.|

## <a name="troubleshooting-manage-mode"></a>Modus voor het oplossen van beheren

| Symptoom | Stappen voor probleemoplossing |
| --- | --- |
| Ik hoef niet de optie voor het toevoegen van een bepaalde methode. | 1. Bepalen of de methode is ingeschakeld voor multi-factor Authentication of voor self-service voor Wachtwoordherstel. <br> 2. Als de methode is ingeschakeld, slaat u het beleid opnieuw en wacht 1-2 uur voordat u het opnieuw. <br> 3. Als de methode is ingeschakeld, zorg ervoor dat de gebruiker van het maximale aantal van deze methode waarmee ze zijn toegestaan voor het instellen van nog niet ingesteld.|

## <a name="disable-combined-registration"></a>Gecombineerde registratie uitschakelen

Wanneer een gebruiker een telefoonnummer en/of de mobiele app in de nieuwe registreert gecombineerde ervaring, onze stempels service een set van vlaggen (StrongAuthenticationMethods) voor deze methoden op die gebruiker. Deze functionaliteit kan de gebruiker multi-factor Authentication uitvoeren met deze methoden wanneer multi-factor Authentication vereist is.

Als een beheerder kan de Preview-versie, gebruikers via de nieuwe ervaring registreren en de beheerder vervolgens de Preview-versie schakelt, kunnen gebruikers onbewust worden geregistreerd voor meervoudige verificatie ook.

Als een gebruiker aan wie gecombineerde registratie is voltooid, gaat met de huidige pagina wachtwoordherstel (SSPR) registratie selfservice voor wachtwoord aan [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), de gebruiker wordt gevraagd om uit te voeren van multi-factor Authentication toegang te krijgen tot Deze pagina. Deze stap wordt verwacht technische vanuit het oogpunt van, maar deze is er nieuw voor gebruikers die eerder zijn geregistreerd voor SSPR alleen. Hoewel deze extra stap van de gebruiker het beveiligingspostuur verbeteren door te geven van een ander niveau van beveiliging, beheerders mogelijk wilt terugdraaien hun gebruikers zodat ze zijn niet meer kunnen multi-factor Authentication uitvoeren.  

### <a name="how-to-roll-back-users"></a>Gebruikers herstellen

Als u, als beheerder, instellen van een gebruiker multi-factor Authentication-instellingen wilt, kunt u de PowerShell-script dat is opgegeven in de volgende sectie. Het script wordt de eigenschap StrongAuthenticationMethods voor mobiele app van een gebruiker en/of telefoonnummer gewist. Als u dit script voor uw gebruikers uitvoeren, moet deze opnieuw te registreren voor meervoudige verificatie als ze deze nodig hebben. Aangeraden terugdraaien van de tests met één of twee gebruikers voordat u alle betrokken gebruikers worden teruggedraaid.

De volgende stappen krijgt u terugdraaien van een gebruiker of groep van gebruikers.

#### <a name="prerequisites"></a>Vereisten

1. Installeer de juiste Azure AD PowerShell-modules. Voer deze opdrachten om de modules te installeren in een PowerShell-venster:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Sla de lijst met betrokken gebruikersobject-id's op uw computer als een tekstbestand met één ID per regel. Noteer de locatie van het bestand.
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

Voer de volgende opdracht, biedt het script en de gebruiker bestandslocaties in een PowerShell-venster. Voer hoofdbeheerdersreferenties in wanneer hierom wordt gevraagd. Het script wordt het resultaat van elke gebruiker update-bewerking uitvoeren.

`<script location> -path <user file location>`

### <a name="disable-the-preview-experience"></a>De preview-ervaring uitschakelen

Schakel de preview-ervaring voor uw gebruikers de volgende stappen uit:

1. Meld u aan de Azure-portal als de Gebruikersbeheerder van een.
2. Ga naar **Azure Active Directory** > **gebruikersinstellingen** > **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligingsgegevens**, de kiezer ingesteld op **geen**, en selecteer vervolgens **opslaan**.

Gebruikers wordt niet meer gevraagd om u te registreren met behulp van de preview-ervaring.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de openbare preview van gecombineerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication](concept-registration-mfa-sspr-combined.md)
