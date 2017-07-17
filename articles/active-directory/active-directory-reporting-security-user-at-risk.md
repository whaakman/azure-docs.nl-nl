---
title: Beveiligingsrapport over gebruikers die risico lopen in de Azure Active Directory-portal | Microsoft Docs
description: Kom meer te weten over het beveiligingsrapport over gebruikers die risico lopen in de Azure Active Directory-portal
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 01ecb98c02b2a01007c7f76805d4db4b7aeee1f0
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017

---
# Het beveiligingsrapport over gebruikers die risico lopen in de Azure Active Directory-portal
<a id="users-at-risk-security-report-in-the-azure-active-directory-portal" class="xliff"></a>

Met de beveiligingsrapporten in Azure Active Directory (Azure AD) krijgt u inzicht in de kans op verdachte gebruikersaccounts in uw omgeving. 

Azure Active Directory detecteert verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Voor elke gedetecteerde activiteit wordt een record met de naam *risicogebeurtenis* gemaakt. Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie. 

De gedetecteerde risico's worden gebruikt om het volgende te berekenen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie. 

- **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie [Gebruikers van wie wordt aangegeven dat ze risico lopen](active-directory-identityprotection.md#users-flagged-for-risk) voor meer informatie.  

In Azure Portal kunt u de beveiligingsrapporten vinden op de blade **Azure Active Directory** in het gedeelte **Beveiliging**.  

![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/10.png)

## Gratis en Basic edities van Azure Active Directory
<a id="azure-active-directory-free-and-basic-edition" class="xliff"></a>

Het rapport over gebruikers die risico lopen in de gratis en Basic-editie van Azure Active Directory biedt een lijst gebruikersaccounts die mogelijk zijn aangetast. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/03.png)

Wanneer u op een gebruiker klikt, wordt de betreffende blade met gebruikersgegevens geopend.
Controleer de aanmeldgeschiedenis van gebruikers die risico lopen en stel het wachtwoord indien nodig opnieuw in.

![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/46.png)

## Premium edities van Azure Active Directory
<a id="azure-active-directory-premium-editions" class="xliff"></a>

Het rapport over gebruikers die risico lopen in de Azure Active Directory Premium-edities biedt u het volgende:

- Een [lijst met gebruikersaccounts](active-directory-identityprotection.md#users-flagged-for-risk) die mogelijk zijn aangetast 

- Verzamelde informatie over de gedetecteerde [risicogebeurtenistypen](active-directory-identity-protection-risk-events.md)

- Een optie voor het downloaden van het rapport

- Een optie voor het configureren van een [beleid voor herstel van gebruikersrisico's](active-directory-identityprotection.md#user-risk-security-policy)  


![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/71.png)

Wanneer u een gebruiker selecteert, krijgt u een gedetailleerde rapportweergave voor deze gebruiker waarmee u het volgende kunt:

- De weergave Alle aanmeldingen openen

- Het gebruikerswachtwoord opnieuw instellen

- Alle gebeurtenissen sluiten

- De gemelde risico's voor de gebruiker onderzoeken. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/324.png)


Selecteer in de lijst de risicogebeurtenis die u wilt onderzoeken.  
Hiermee opent u de blade **Details** voor deze risicogebeurtenis. Op de blade **Details** kunt u een [risicogebeurtenis handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually) of een handmatig gesloten risicogebeurtenis opnieuw activeren. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/325.png)



## Volgende stappen
<a id="next-steps" class="xliff"></a>

- Zie [Azure Active Directory Identity Protection](active-directory-identityprotection.md) voor meer informatie over Azure Active Directory Identity Protection.


