---
title: Rapport Riskante aanmeldingen in de Azure Active Directory-portal | Microsoft Docs
description: Meer informatie over het rapport Riskante aanmeldingen in de Azure Active Directory-portal
services: active-directory
author: MarkusVi
manager: mtillman
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 542a111ea5227db727deeac9c56b9f4b4a9f694c
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Het rapport Riskante aanmeldingen in de Azure Active Directory-portal

Met de beveiligingsrapporten in Azure Active Directory (Azure AD) krijgt u inzicht in de kans op verdachte gebruikersaccounts in uw omgeving. 

Azure AD detecteert verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Voor elke gedetecteerde activiteit wordt een record met de naam *risicogebeurtenis* gemaakt. Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie. 

De gedetecteerde risico's worden gebruikt om het volgende te berekenen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie. 

- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie [Gebruikers voor wie wordt aangegeven dat ze risico lopen](active-directory-identityprotection.md#users-flagged-for-risk) voor meer informatie.  

In [Azure Portal](https://portal.azure.com) kunt u de beveiligingsrapporten vinden op de blade **Azure Active Directory** in het gedeelte **Beveiliging**. 

![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot een beveiligingsrapport?  

Alle edities van Azure Active Directory bieden rapporten over riskante aanmeldingen.  
Het detailniveau van rapporten verschilt wel per editie: 

- Bij de edities **Azure Active Directory Free en Basic** krijgt u al een lijst met riskante aanmeldingen. 

- De editie **Azure Active Directory Premium 1** bevat een uitgebreider model waarmee u ook bepaalde onderliggende risicogebeurtenissen kunt onderzoeken die voor elk rapport zijn gedetecteerd. 

- De editie **Azure Active Directory Premium 2** biedt u de meest gedetailleerde informatie over alle onderliggende risicogebeurtenissen. Deze editie stelt u ook in staat beveiligingsbeleidsregels te configureren die automatisch op de geconfigureerde risiconiveaus reageren.



## <a name="azure-active-directory-free-and-basic-edition"></a>Gratis en Basic edities van Azure Active Directory

De edities Gratis en Basic van Azure Active Directory bieden u een lijst met riskante aanmeldingen die zijn gedetecteerd voor uw gebruikers. Dit rapport bevat:

- **Gebruiker**: de naam van de gebruiker die is gebruikt tijdens het aanmelden
- **IP**: het IP-adres van het apparaat dat is gebruikt om verbinding te maken met Azure Active Directory
- **Locatie**: de locatie die is gebruikt om verbinding te maken met Azure Active Directory
- **Tijd van aanmelden**: de tijd waarop de aanmelding heeft plaatsgevonden
- **Status**: de status van de aanmelding


![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/01.png)

Op basis van uw onderzoek van de riskante aanmelding kunt u uw feedback naar Azure Active Directory sturen door een van de volgende acties te ondernemen:

- Oplossen
- Markeren als fout-positief
- Negeren
- Opnieuw activeren

![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/21.png)

Zie voor meer informatie [Risico's handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually).

Dit rapport biedt de volgende mogelijkheden:

- Resources zoeken
- De rapportgegevens downloaden


![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a>Premium edities van Azure Active Directory

Het rapport Riskante aanmeldingen in de Azure Active Directory-portal biedt u het volgende:

- Verzamelde informatie over de gedetecteerde [risicogebeurtenistypen](active-directory-identity-protection-risk-events.md)

- Een optie voor het downloaden van het rapport


![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/456.png)


Wanneer u een risicogebeurtenis selecteert, krijgt u een gedetailleerde rapportweergave voor deze risicogebeurtenis waarmee u het volgende kunt:

- Een optie voor het configureren van een [beleid voor herstel van gebruikersrisico's](active-directory-identityprotection.md#user-risk-security-policy)  

- De detectietijdlijn voor de risicogebeurtenis bekijken  

- Een lijst met gebruikers bekijken waarvoor deze risicogebeurtenis is gedetecteerd

- [Risicogebeurtenissen handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually) of een handmatig gesloten risicogebeurtenis opnieuw activeren. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/457.png)

Wanneer u een gebruiker selecteert, krijgt u een gedetailleerde rapportweergave voor deze gebruiker waarmee u het volgende kunt:

- De weergave Alle aanmeldingen openen

- Het gebruikerswachtwoord opnieuw instellen

- Alle gebeurtenissen sluiten

- De gemelde risico's voor de gebruiker onderzoeken. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/324.png)


Selecteer in de lijst de risicogebeurtenis die u wilt onderzoeken.  
Hiermee opent u de blade **Details** voor deze risicogebeurtenis. Op de blade **Details** kunt u een [risicogebeurtenis handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually) of een handmatig gesloten risicogebeurtenis opnieuw activeren. 


![Riskante aanmeldingen](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Active Directory Identity Protection](active-directory-identityprotection.md) voor meer informatie over Azure Active Directory Identity Protection.

