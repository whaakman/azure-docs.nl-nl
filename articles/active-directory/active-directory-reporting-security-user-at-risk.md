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
ms.openlocfilehash: 04f15384a7cd0fa03300acdf159d371569ecf9fc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="users-flagged-for-risk-security-report-in-the-azure-active-directory-portal"></a>Beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal

Met de beveiligingsrapporten in Azure Active Directory (Azure AD) krijgt u inzicht in de kans op verdachte gebruikersaccounts in uw omgeving. 

Azure Active Directory detecteert verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Voor elke gedetecteerde activiteit wordt een record met de naam *risicogebeurtenis* gemaakt. Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie. 

De gedetecteerde risico's worden gebruikt om het volgende te berekenen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie. 

- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie [Gebruikers voor wie wordt aangegeven dat ze risico lopen](active-directory-identityprotection.md#users-flagged-for-risk) voor meer informatie.  

In Azure Portal kunt u de beveiligingsrapporten vinden op de blade **Azure Active Directory** in het gedeelte **Beveiliging**.  

![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot een beveiligingsrapport?  

Alle edities van Azure Active Directory bieden rapporten over gebruikers voor wie wordt aangegeven dat ze risico lopen.  
Het detailniveau van rapporten verschilt wel per editie: 

- In de edities **Azure Active Directory Free en Basic** hebt u toegang tot een lijst die gebruikers bevat voor wie wordt aangegeven dat ze risico lopen. 

- De editie **Azure Active Directory Premium 1** bevat een uitgebreider model waarmee u ook bepaalde onderliggende risicogebeurtenissen kunt onderzoeken die voor elk rapport zijn gedetecteerd. 

- De editie **Azure Active Directory Premium 2** biedt u de meest gedetailleerde informatie over alle onderliggende risicogebeurtenissen. Deze editie stelt u ook in staat beveiligingsbeleidsregels te configureren die automatisch op de geconfigureerde risiconiveaus reageren.



## <a name="azure-active-directory-free-and-basic-edition"></a>Gratis en Basic edities van Azure Active Directory

Het rapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de gratis en Basic-editie van Azure Active Directory biedt een lijst gebruikersaccounts die mogelijk zijn aangetast. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/03.png)

Wanneer u op een gebruiker klikt, wordt de betreffende blade met gebruikersgegevens geopend.
Controleer de aanmeldgeschiedenis van gebruikers die risico lopen en stel het wachtwoord indien nodig opnieuw in.

![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/46.png)


Dit rapport biedt de volgende mogelijkheden:

- Het rapport downloaden

- Gebruikers zoeken

![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a>Premium edities van Azure Active Directory

Het rapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory Premium-edities biedt u het volgende:

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


Als u een risicogebeurtenis wilt onderzoeken, selecteert u de gebeurtenis in de lijst om de bijbehorende blade **Details** te openen. Op de blade **Details** kunt u een [risicogebeurtenis handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually) of een handmatig gesloten risicogebeurtenis opnieuw activeren. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Active Directory Identity Protection](active-directory-identityprotection.md) voor meer informatie over Azure Active Directory Identity Protection.

