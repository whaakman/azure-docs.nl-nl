---
title: Beveiligingsrapportage in de edities Gratis en Basic van Azure Active Directory - preview | Microsoft Docs
description: Bevat een lijst met de diverse beschikbare rapporten voor Azure Active Directory-previews
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: fa5a6dab1e76d62956cbfdd9b8b0f64c014696db
ms.openlocfilehash: c1a3953c79cfac9d6f55b38971ee2b79ff4244eb


---
# <a name="security-reporting-in-the-azure-active-directory-free-and-basic-edition---preview"></a>Beveiligingsrapportage in de edities Gratis en Basic van Azure Active Directory - preview

Met de beveiligingsrapporten in de [preview](active-directory-preview-explainer.md) van Azure Active Directory krijgt u inzicht in de kans op verdachte gebruikersaccounts in uw omgeving. 

Azure Active Directory detecteert verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Voor elke gedetecteerde activiteit wordt een record met de naam *risicogebeurtenis* gemaakt. Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie. 

De gedetecteerde risico's worden gebruikt om het volgende te berekenen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie. 

- **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie [Gebruikers van wie wordt aangegeven dat ze risico lopen](active-directory-identityprotection.md#users-flagged-for-risk) voor meer informatie.  


## <a name="risky-sign-ins-report"></a>Rapport Riskante aanmeldingen

De edities Gratis en Basic van Azure Active Directory bieden u een lijst met riskante aanmeldingen die zijn gerapporteerd voor uw gebruikers. Het rapport Riskante gebeurtenissen geeft u informatie over:

- **Gebruiker**: de naam van de gebruiker die is gebruikt tijdens het aanmelden
- **IP**: het IP-adres van het apparaat dat is gebruikt om verbinding te maken met Azure Active Directory
- **Locatie**: de locatie die is gebruikt om verbinding te maken met Azure Active Directory
- **Tijd van aanmelden**: de tijd waarop de aanmelding heeft plaatsgevonden
- **Status**: de status van de aanmelding

Dit rapport geeft u een optie voor het downloaden van de rapportgegevens.

![Rapportage](./media/active-directory-reporting-security-azure-portal-free-basic/01.png)

Op basis van uw onderzoek van de riskante aanmelding kunt u uw feedback naar Azure Active Directory sturen door een van de volgende acties te ondernemen:

- Oplossen
- Markeren als fout-positief
- Negeren
- Opnieuw activeren

![Rapportage](./media/active-directory-reporting-security-azure-portal-free-basic/21.png)

Zie voor meer informatie [Risico's handmatig sluiten](active-directory-identityprotection.md#closing-risk-events-manually).


## <a name="users-at-risk-report"></a>Rapport Gebruikers die risico lopen

De Gratis editie van Azure Active Directory biedt u een lijst van gebruikersaccounts die mogelijk zijn aangetast. 


![Rapportage](./media/active-directory-reporting-security-azure-portal-free-basic/03.png)

Wanneer u op een gebruiker in de lijst klikt, wordt de betreffende blade met gebruikersgegevens geopend.
Controleer de aanmeldgeschiedenis van gebruikers die risico lopen en stel het wachtwoord zonodig opnieuw in.

![Rapportage](./media/active-directory-reporting-security-azure-portal-free-basic/46.png)



## <a name="next-steps"></a>Volgende stappen

- Zie de [Rapportagehandleiding voor Azure Active Directory](active-directory-reporting-guide.md) voor meer informatie over rapporteren in Azure Active Directory.
- Zie [Azure Active Directory Identity Protection](active-directory-identityprotection.md) voor meer informatie over Azure Active Directory Identity Protection.




<!--HONumber=Jan17_HO3-->


