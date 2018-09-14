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
ms.date: 05/23/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 5fe16ecb2725cf306b5b57c9d45d8601581a3ece
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578889"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Gebruikers herstellen voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal

Met de beveiligingsrapporten in Azure Active Directory (Azure AD) krijgt u inzicht in de kans op verdachte gebruikersaccounts in uw omgeving. Een gebruiker die is gemarkeerd voor risico's is een indicator van een gebruikersaccount dat mogelijk is aangetast.

Microsoft spant zich in om uw omgevingen veilig te houden. Als onderdeel van deze inspanningen controleert Microsoft doorlopend activiteiten die ongebruikelijk zijn of consistent met bekende aanvalspatronen. 


Als ongebruikelijke activiteiten wijzen op mogelijk niet-geautoriseerde toegang tot een aantal van uw gebruikersaccounts, ontvangt u meldingen waardoor u actie kunt ondernemen. Als u meldingen ontvangt, betekent dit nog niet dat van de eigen systemen van Microsoft misbruik is gemaakt.
 

## <a name="access-the-users-flagged-for-risk-report"></a>Gebruikers bekijken voor wie wordt aangegeven dat ze risico lopen

U kunt gebruikers voor wie wordt aangegeven dat ze risico lopen, bekijken via het [bijbehorende rapport](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk) in Azure Active Directory (AD). Als u geen abonnement hebt op Azure AD, kunt u zich gratis abonneren via het eenmalige proces op [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD). In dit rapport kunt u verschillende acties uitvoeren, zoals:

- Een tijdelijk wachtwoord genereren
- Vereisen dat gebruikers veilig hun wachtwoord opnieuw instellen de volgende keer dat ze zich aanmelden
- Het gebruikersrisico afwijzen zonder een herstelactie uit te voeren.

Zie het [Beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal](concept-user-at-risk.md) voor meer informatie.

### <a name="azure-ad-subscription-for-office-365-customers"></a>Azure AD-abonnement voor Office 365-klanten

Zodra u geabonneerd bent, kunt u uw Office 365-referenties gebruiken om toegang te krijgen tot het Azure-beheercentrum. Nadat u de toegang tot Azure AD hebt geactiveerd, wordt u doorgestuurd naar de Azure AD-portal. Bij een basisabonnement worden minder details in rapporten verstrekt. Aanvullende gegevens en analyses zijn verkrijgbaar voor abonnees van Azure Premium.


**Voor toegang tot het beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in het Office 365-beheercentrum:**

1.  Klik in het navigatiemenu aan de linkerkant op **Beheercentra**. 
2.  Klik op **Azure AD**.
3.  Meld u aan bij **Azure Active Directory-beheercentrum**.
4.  Als boven aan de pagina een banner wordt weergegeven met de tekst **Bekijk de nieuwe portal**, klikt u op de koppeling.
4.  Klik in het navigatiemenu aan de linkerkant op **Azure Active Directory**. 
5.  Klik in het navigatievenster, onder **Beveiliging**, op **Gebruikers voor wie wordt aangegeven dat ze risico lopen**.

Bekijk de informatie die hier wordt weergegeven. Stel het wachtwoord voor het vermelde account opnieuw in. 

## <a name="remediation-actions"></a>Herstelacties

Onderneem de volgende acties om de betreffende accounts te herstellen en uw omgeving te beveiligen:

1.  [Valideer](http://aka.ms/MFAValid) de juiste informatie voor meervoudige verificatie en selfservice voor wachtwoordherstel. 
2.  [Activeer](http://aka.ms/MFAuth) meervoudige verificatie voor alle gebruikers. 
3.  Met behulp van dit [herstelscript](http://aka.ms/remediate) kunt u voor elk betrokken account automatisch de volgende stappen uitvoeren: 

    a. Stel het wachtwoord opnieuw in om het account te beveiligen en actieve sessies af te sluiten.

    b. Verwijder gemachtigden voor een postvak.

    c. Schakel regels voor het doorsturen van e-mail voor externe domeinen uit.

    d. Verwijder de eigenschap voor het globaal doorsturen van e-mail voor het postvak uit.

    e. Schakel MFA voor het account van de gebruiker in.

    f. Stel de wachtwoordcomplexiteit voor het account in op Hoog.

    g. Schakel postvakcontrole in.

    h. Genereer een auditlogboek ter beoordeling van de beheerder.

4. Onderzoek uw Office 365-tenant en andere IT-infrastructuur, waaronder een controle van alle tenantinstellingen, gebruikersaccounts en de configuratie-instellingen per gebruiker op mogelijke wijzigingen. Controleer op de aanwezigheid van indicatoren voor persistentiemethoden, evenals indicatoren dat een indringer gebruik kan hebben gemaakt van een eerste aangrijpingspunt om VPN-referenties te verkrijgen, of toegang tot andere organisatieaccounts. 

5.  Overweeg bij uw onderzoek ook of u overheidsinstanties, waaronder de politie, moet waarschuwen.

Daarnaast dient u:

- Deze [richtlijnen](http://aka.ms/fixaccount) over ongebruikelijke activiteiten te lezen en implementeren. 
- [De auditpijplijn](http://aka.ms/improvesecurity) in te schakelen voor de analyse van de activiteiten met betrekking tot uw tenancy. Als u klaar bent, wordt het auditarchief gevuld met alle activiteitenlogboeken. Op dat moment kunt u ook gebruikmaken van [Security and Compliance Center’s Search and Investigation](http://aka.ms/sccsearch) (Doorzoeken en onderzoeken in het Centrum voor beveiliging en naleving). 
- Gebruik dit [script](http://aka.ms/mailboxaudit1) om postvakcontrole in te schakelen voor al uw accounts. 
- Bekijk voor alle postvakken de machtigingen voor gemachtigden en de regels voor het doorsturen van e-mail. U kunt dit [PowerShell-script](http://aka.ms/delegateforwardrules) gebruiken om deze taak uit te voeren. 



## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) voor meer informatie over Azure Active Directory Identity Protection.

