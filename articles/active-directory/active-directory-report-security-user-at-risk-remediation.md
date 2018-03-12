---
title: Beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal | Microsoft Docs
description: Meer informatie over het beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ed6201e9edcef39b14b948b6b2f6e0b5da01ec60
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Gebruikers herstellen voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal

Met de beveiligingsrapporten in Azure Active Directory (Azure AD) krijgt u inzicht in de kans op verdachte gebruikersaccounts in uw omgeving. Een [gebruiker voor wie wordt aangegeven dat deze risico loopt](active-directory-identityprotection.md#users-flagged-for-risk) is een indicator van een gebruikersaccount dat mogelijk is aangetast.

Microsoft spant zich in om uw omgevingen veilig te houden. Als onderdeel van deze inspanningen controleert Microsoft doorlopend activiteiten die ongebruikelijk zijn of consistent met bekende aanvalspatronen. 


Als ongebruikelijke activiteiten wijzen op mogelijk niet-geautoriseerde toegang tot een aantal van uw gebruikersaccounts, ontvangt u meldingen waardoor u actie kunt ondernemen. Als u meldingen ontvangt, betekent dit nog niet dat van de eigen systemen van Microsoft misbruik is gemaakt.
 

## <a name="azure-active-directory-report-access"></a>Toegang tot Azure Active Directory-rapporten

U kunt gebruikers voor wie wordt aangegeven dat ze risico lopen, weergeven via een onlinerapport van Azure Active Directory. Als u geen abonnement hebt op Azure, kunt u zich gratis abonneren op [http://aka.ms/AccessAAD](http://aka.ms/AccessAAD).  
Zodra u geabonneerd bent, kunt u uw Office 365-referenties gebruiken om toegang te krijgen tot het Azure-beheercentrum. Houd er rekening mee dat bij een basisabonnement de hoeveelheid verstrekte details beperkt is. Aanvullende gegevens en analyses zijn verkrijgbaar voor abonnees van Azure Premium. Zie het [beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal](active-directory-reporting-security-user-at-risk.md) voor meer informatie.

Als u de toegang tot Azure Active Directory hebt geactiveerd, wordt u doorgestuurd naar de [Azure AD-portal](https://portal.azure.com). Als u het rapport meteen wilt inzien, klikt u op de volgende URL: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UsersAtRisk).

**Voor toegang tot het beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in het Office 365-beheercentrum:**

1.  Klik in het navigatiemenu aan de linkerkant op **Beheercentra**. 
2.  Klik op **Azure AD**.
3.  Meld u aan bij **Azure Active Directory-beheercentrum**.
4.  Als boven aan de pagina een banner wordt weergegeven met de tekst **Bekijk de nieuwe portal**, klikt u op de koppeling.
4.  Klik in het navigatiemenu aan de linkerkant op **Azure Active Directory**. 
5.  Klik in het navigatievenster, onder **Beveiliging**, op **Gebruikers voor wie wordt aangegeven dat ze risico lopen**.

Bekijk de informatie die hier wordt weergegeven. Stel het wachtwoord voor het hier vermelde account opnieuw in. 

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

- Zie [Azure Active Directory Identity Protection](active-directory-identityprotection.md) voor meer informatie over Azure Active Directory Identity Protection.

