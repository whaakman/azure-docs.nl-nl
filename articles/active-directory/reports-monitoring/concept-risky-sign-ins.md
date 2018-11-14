---
title: Rapport Riskante aanmeldingen in de Azure Active Directory-portal | Microsoft Docs
description: Meer informatie over het rapport Riskante aanmeldingen in de Azure Active Directory-portal
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2e4406a75ea1d9f1968d994ae2294b39ca7613d5
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623856"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Het rapport Riskante aanmeldingen in de Azure Active Directory-portal

Azure Active Directory (Azure AD) detecteert verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Voor elke activiteit wordt gedetecteerde er een record met de naam een **risicogebeurtenis** wordt gemaakt. Zie voor meer informatie, [Azure AD-risicogebeurtenissen](concept-risk-events.md). 

U hebt toegang tot de beveiligingsrapporten in de [Azure-portal](https://portal.azure.com) door het selecteren van de **Azure Active Directory** blade en vervolgens te navigeren naar de **Security** sectie. 

Er zijn twee verschillende beveiligingsrapporten die worden berekend op basis van de risicogebeurtenissen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is.

- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. 

![Riskante aanmeldingen](./media/concept-risky-sign-ins/10.png)

Zie voor informatie over het configureren van de beleidsregels die van deze risicogebeurtenissen activeren, [het configureren van het beleid voor gebruikersrisico's](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Wie toegang heeft tot het rapport riskante aanmeldingen?

De rapporten over riskante aanmeldingen zijn beschikbaar voor gebruikers in de volgende rollen:

- Beveiligingsbeheerder
- Globale beheerder
- Beveiligingslezer

Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [weergeven en toewijzen beheerdersrollen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot een beveiligingsrapport?  

Alle edities van Azure AD bieden rapporten over riskante aanmeldingen. Het detailniveau van rapporten verschilt wel per editie: 

- In de **Azure Active Directory gratis en Basic edities**, krijgt u een lijst met riskante aanmeldingen. 

- Bovendien de **Azure Active Directory Premium 1** edition kunt u enkele van de onderliggende risicogebeurtenissen die zijn gedetecteerd voor elk rapport bekijken. 

- De editie **Azure Active Directory Premium 2** biedt u de meest gedetailleerde informatie over alle onderliggende risicogebeurtenissen. Deze editie stelt u ook in staat beveiligingsbeleidsregels te configureren die automatisch op de geconfigureerde risiconiveaus reageren.

## <a name="risky-sign-ins-report-for-azure-ad-free-and-basic-edition"></a>Rapport riskante aanmeldingen voor Azure AD gratis en basic-editie

De Azure AD gratis en basic edities bieden u een lijst met riskante aanmeldingen die zijn gedetecteerd voor uw gebruikers. Elke record bevat de volgende kenmerken:

- **Gebruiker**: de naam van de gebruiker die is gebruikt tijdens het aanmelden
- **IP**: het IP-adres van het apparaat dat is gebruikt om verbinding te maken met Azure Active Directory
- **Locatie**: de locatie die is gebruikt om verbinding te maken met Azure Active Directory
- **Tijd van aanmelden**: de tijd waarop de aanmelding heeft plaatsgevonden
- **Status**: de status van de aanmelding

![Riskante aanmeldingen](./media/concept-risky-sign-ins/01.png)

Op basis van uw onderzoek van de riskante aanmelding, kunt u feedback geven aan Azure AD door de volgende acties te ondernemen:

- Oplossen
- Markeren als fout-positief
- Negeren
- Opnieuw activeren

![Riskante aanmeldingen](./media/concept-risky-sign-ins/21.png)

Dit rapport bevat ook een optie voor het:

- Resources zoeken
- De rapportgegevens downloaden

![Riskante aanmeldingen](./media/concept-risky-sign-ins/93.png)


## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Rapport riskante aanmeldingen voor Azure AD premium-edities

Het rapport riskante aanmeldingen in de Azure AD premium-edities biedt u met:

- Verzamelde informatie over de gedetecteerde [risicogebeurtenistypen](concept-risk-events.md)

- Een optie voor het downloaden van het rapport

![Riskante aanmeldingen](./media/concept-risky-sign-ins/456.png)

Wanneer u een risicogebeurtenis selecteert, krijgt u een gedetailleerde rapportweergave voor deze risicogebeurtenis waarmee u het volgende kunt:

- Een optie voor het configureren van een [beleid voor herstel van gebruikersrisico's](../identity-protection/howto-user-risk-policy.md)  

- De detectietijdlijn voor de risicogebeurtenis bekijken  

- Een lijst met gebruikers bekijken waarvoor deze risicogebeurtenis is gedetecteerd

- Risicogebeurtenissen handmatig sluiten. 

![Riskante aanmeldingen](./media/concept-risky-sign-ins/457.png)

Wanneer u een gebruiker selecteert, krijgt u een gedetailleerde rapportweergave voor deze gebruiker waarmee u het volgende kunt:

- De weergave Alle aanmeldingen openen

- Het gebruikerswachtwoord opnieuw instellen

- Alle gebeurtenissen sluiten

- De gemelde risico's voor de gebruiker onderzoeken. 

![Riskante aanmeldingen](./media/concept-risky-sign-ins/324.png)

Selecteer in de lijst de risicogebeurtenis die u wilt onderzoeken.  
Hiermee opent u de blade **Details** voor deze risicogebeurtenis. Op de blade **Details** kunt u een risicogebeurtenis handmatig sluiten of een handmatig gesloten risicogebeurtenis opnieuw activeren. 

![Riskante aanmeldingen](./media/concept-risky-sign-ins/325.png)

## <a name="next-steps"></a>Volgende stappen

- [Het configureren van het beleid voor gebruikersrisico 's](../identity-protection/howto-user-risk-policy.md)
- [Het configureren van het beleid voor het herstel van gebruikersrisico](../identity-protection/howto-user-risk-policy.md)
- [Risicogebeurtenistypen](concept-risk-events.md)