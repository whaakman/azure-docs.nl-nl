---
title: Beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal | Microsoft Docs
description: Meer informatie over het beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 411ffd9479c3f686495de21b60ef0a811915c260
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834567"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Gebruikers herstellen voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal

Met de beveiligingsrapporten in Azure Active Directory (Azure AD), kunt u de kans op verdachte gebruikersaccounts in uw omgeving te meten. Een gebruiker die is gemarkeerd voor risico's is een indicator van een gebruikersaccount dat mogelijk is aangetast.

Microsoft spant zich in om uw omgevingen veilig te houden. Als onderdeel van deze inspanningen controleert Microsoft doorlopend activiteiten die ongebruikelijk zijn of consistent met bekende aanvalspatronen. 

Als ongebruikelijke activiteiten die op niet-geautoriseerde toegang tot een aantal van uw gebruikersaccounts duiden kunnen worden gedetecteerd, ontvangt u meldingen waardoor u actie te ondernemen. Dit betekent niet dat van het Microsoft-systemen zijn aangetast.

## <a name="access-the-users-flagged-for-risk-report"></a>Gebruikers bekijken voor wie wordt aangegeven dat ze risico lopen

U kunt gebruikers die zijn gemarkeerd voor risico via bekijken de [gebruikers lopen risico rapport](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk) in Azure portal. Als u geen Azure AD hebt, kunt u zich van gratis bij [ https://aka.ms/AccessAAD ](https://aka.ms/AccessAAD). 

Van de gebruikers die zijn gemarkeerd voor risico's rapport, kunt u de volgende acties voor elke gebruiker uitvoeren:

- Een tijdelijk wachtwoord genereren
- Vereisen dat gebruikers veilig hun wachtwoord opnieuw instellen de volgende keer dat ze zich aanmelden
- Het gebruikersrisico afwijzen zonder een herstelactie uit te voeren.

Zie voor meer informatie, [gebruikers die zijn gemarkeerd voor risico's beveiligingsrapport](concept-user-at-risk.md).

### <a name="azure-ad-subscription-for-office-365-customers"></a>Azure AD-abonnement voor Office 365-klanten

U kunt ook uw Office 365-referenties gebruiken voor toegang tot de **Azure-beheercentrum**. Nadat u de toegang tot Azure AD hebt geactiveerd, wordt u doorgestuurd naar de Azure AD-portal. Bij een basisabonnement worden minder details in rapporten verstrekt. Aanvullende gegevens en analyses zijn verkrijgbaar voor abonnees van Azure Premium.

Toegang krijgen tot de **gebruikers die zijn gemarkeerd voor risico's** rapporten in de Office 365-beheercentrum:

1.  Selecteer in het navigatiemenu aan de linkerkant, **beheercentrums**. 
2.  Selecteer **Azure AD**.
3.  Meld u aan bij **Azure Active Directory-beheercentrum**.
4.  Als er een banner wordt weergegeven aan de bovenkant van de pagina met de melding dat **Bekijk de nieuwe portal**, selecteert u de koppeling.
4.  Selecteer in het navigatiemenu aan de linkerkant, **Azure Active Directory**. 
5.  Selecteer in het navigatiedeelvenster **gebruikers die zijn gemarkeerd voor risico's** uit de **Security** sectie.

## <a name="remediation-actions"></a>Herstelacties

Onderneem de volgende acties om de betreffende accounts te herstellen en uw omgeving te beveiligen:

1.  [Valideren van de juiste gegevens](https://aka.ms/MFAValid) voor multi-factor authentication en Self-service voor wachtwoord opnieuw instellen. 
2.  [Multi-factor authentication inschakelen](https://aka.ms/MFAuth) voor alle gebruikers. 
3.  Gebruik deze [herstelscript](https://aka.ms/remediate) voor elk betrokken account automatisch de volgende stappen uitvoeren: 

    a. Wachtwoord opnieuw instellen om het account, beveiligen en actieve sessies beëindigen.

    b. Verwijder gemachtigden voor een postvak.

    c. Schakel regels voor het doorsturen van e-mail voor externe domeinen uit.

    d. Verwijder de eigenschap voor het globaal doorsturen van e-mail voor het postvak uit.

    e. Schakel MFA voor het account van de gebruiker in.

    f. Stel de wachtwoordcomplexiteit voor het account in op Hoog.

    g. Schakel postvakcontrole in.

    h. Genereer een een auditlogboek voor de beheerder om te controleren.

4. Onderzoek uw Office 365-tenant en andere IT-infrastructuur, waaronder een controle van alle tenantinstellingen, gebruikersaccounts en de configuratie-instellingen per gebruiker op mogelijke wijzigingen. Controleer op de aanwezigheid van indicatoren voor persistentiemethoden, evenals indicatoren dat een indringer gebruik kan hebben gemaakt van een eerste aangrijpingspunt om VPN-referenties te verkrijgen, of toegang tot andere organisatieaccounts. 

5.  Overweeg of u overheidsinstanties, waaronder Justitie en politie moet waarschuwen als onderdeel van uw onderzoek.

Daarnaast dient u:

- Lees- en dit implementeren [richtlijnen over ongebruikelijke activiteiten](https://aka.ms/fixaccount). 
- [Inschakelen van de pijplijn audit](https://aka.ms/improvesecurity) u helpen bij het analyseren van de activiteit in uw tenant. Als u klaar bent, wordt het auditarchief gevuld met activiteitenlogboeken. Op dit moment kunt u ook gebruikmaken van de [beveiligings- en Compliancecentrum van zoek- en onderzoeksfuncties resource](https://aka.ms/sccsearch). 
- Gebruik deze [script voor het inschakelen van Postvakcontrole voor](https://aka.ms/mailboxaudit1) voor al uw accounts. 
- Bekijk voor alle postvakken de machtigingen voor gemachtigden en de regels voor het doorsturen van e-mail. U kunt dit [PowerShell-script](https://aka.ms/delegateforwardrules) gebruiken om deze taak uit te voeren. 

## <a name="next-steps"></a>Volgende stappen

* [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
* [Gebruikers die zijn gemarkeerd voor risico 's](concept-user-at-risk.md)