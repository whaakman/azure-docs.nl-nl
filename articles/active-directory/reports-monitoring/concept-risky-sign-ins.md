---
title: Rapport Riskante aanmeldingen in de Azure Active Directory-portal | Microsoft Docs
description: Meer informatie over het rapport Riskante aanmeldingen in de Azure Active Directory-portal
services: active-directory
author: cawrites
manager: daveba
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 629e7bf8ee7100b5927483ecfd4efa1d9223c151
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989925"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Het rapport Riskante aanmeldingen in de Azure Active Directory-portal

Azure Active Directory (Azure AD) detecteert verdachte acties die zijn gerelateerd aan uw gebruikers accounts. Voor elke gedetecteerde activiteit wordt een record met de naam **risicogebeurtenis** gemaakt. Zie [risico gebeurtenissen in azure AD](concept-risk-events.md)voor meer informatie. 

De beveiligingsrapporten zijn beschikbaar via de [Azure-portal](https://portal.azure.com). Selecteer de blade **Azure Active Directory** en ga vervolgens naar de sectie **Beveiliging**. 

Er zijn twee verschillende beveiligings rapporten die worden berekend op basis van de risico gebeurtenissen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is.

- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. 

![Riskante aanmeldingen](./media/concept-risky-sign-ins/10.png)

Zie [Het beleid voor gebruikersrisico's configureren](../identity-protection/howto-user-risk-policy.md) voor informatie over het configureren van de beleidsregels die deze risicogebeurtenissen activeren.  

## <a name="who-can-access-the-risky-sign-ins-report"></a>Wie kan het rapport Risk ante aanmeldingen openen?

De rapporten met Risk ante aanmeldingen zijn beschikbaar voor gebruikers in de volgende rollen:

- Beveiligingsbeheerder
- Globale beheerder
- Beveiligingslezer

Zie [beheerders rollen weer geven en toewijzen in azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal)voor meer informatie over het toewijzen van beheerders rollen aan een gebruiker in azure Active Directory.

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot een beveiligingsrapport?  

Alle versies van Azure AD bieden u rapporten met Risk ante aanmeldingen. Het detailniveau van rapporten verschilt wel per editie: 

- In de **Azure Active Directory Free en Basic-edities**krijgt u een lijst met Risk ante aanmeldingen. 

- Daarnaast kunt u in de editie **Azure Active Directory Premium 1** bepaalde onderliggende risicogebeurtenissen onderzoeken die voor elk rapport zijn gedetecteerd. 

- De editie **Azure Active Directory Premium 2** biedt u de meest gedetailleerde informatie over alle onderliggende risicogebeurtenissen. Deze editie stelt u ook in staat beveiligingsbeleidsregels te configureren die automatisch op de geconfigureerde risiconiveaus reageren.

## <a name="risky-sign-ins-report-for-azure-ad-free-and-basic-edition"></a>Rapport Risk ante aanmeldingen voor Azure AD Free en Basic Edition

De gratis en Basic-edities van Azure AD bieden een lijst met Risk ante aanmeldingen die zijn gedetecteerd voor uw gebruikers. Elke record bevat de volgende kenmerken:

- **Gebruiker** -de naam van de gebruiker die is gebruikt tijdens de aanmeldings bewerking.
- **IP** : het IP-adres van het apparaat dat is gebruikt om verbinding te maken met Azure Active Directory.
- **Locatie** : de locatie die wordt gebruikt om verbinding te maken met Azure Active Directory. Dit is een best mogelijke benadering op basis van traceringen, register gegevens, het terugdraaien van ups en andere informatie.
- **Tijd van aanmelden**: de tijd waarop de aanmelding heeft plaatsgevonden
- **Status**: de status van de aanmelding

![Riskante aanmeldingen](./media/concept-risky-sign-ins/01.png)

Op basis van uw onderzoek naar de Risk ante aanmelding kunt u Azure AD feedback geven door de volgende acties uit te voeren:

- Oplossen
- Markeren als fout-positief
- Negeren
- Opnieuw activeren

![Riskante aanmeldingen](./media/concept-risky-sign-ins/21.png)

Dit rapport geeft u ook een optie om het volgende te doen:

- Resources zoeken
- De rapportgegevens downloaden

![Riskante aanmeldingen](./media/concept-risky-sign-ins/93.png)

## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Rapport Risk ante aanmeldingen voor Azure AD Premium-edities

Het rapport Risk ante aanmeldingen in de Azure AD Premium-edities biedt u het volgende:

- Samengevoegde informatie over de [typen risico gebeurtenissen](concept-risk-events.md) die zijn gedetecteerd. Met de **Azure AD Premium P1 Edition**worden detecties die niet onder uw licentie vallen, weer gegeven als het aanmelden met een risico gebeurtenis **waarvoor extra risico is gedetecteerd**. Met de **Azure AD Premium P2-editie**krijgt u de meest gedetailleerde informatie over alle onderliggende detecties.

- Een optie voor het downloaden van het rapport

![Riskante aanmeldingen](./media/concept-risky-sign-ins/456.png)

Wanneer u een risicogebeurtenis selecteert, krijgt u een gedetailleerde rapportweergave voor deze risicogebeurtenis waarmee u het volgende kunt:

- Een optie voor het configureren van een [beleid voor herstel van gebruikersrisico's](../identity-protection/howto-user-risk-policy.md)  

- De detectietijdlijn voor de risicogebeurtenis bekijken  

- Een lijst met gebruikers bekijken waarvoor deze risicogebeurtenis is gedetecteerd

- Risico gebeurtenissen hand matig sluiten. 

![Riskante aanmeldingen](./media/concept-risky-sign-ins/457.png)

> [!IMPORTANT]
> Soms vindt u een risico gebeurtenis zonder een bijbehorende aanmeldings vermelding in het [aanmeld rapport](concept-sign-ins.md). Dit komt doordat identiteits beveiliging het risico evalueert voor **interactieve** en **niet-interactieve** aanmeldingen, terwijl in het rapport voor aanmeldingen alleen de interactieve aanmeldingen worden weer gegeven.

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

- [Het beleid voor gebruikersrisico's configureren](../identity-protection/howto-user-risk-policy.md)
- [How to configure the risk remediation policy](../identity-protection/howto-user-risk-policy.md) (Beleid voor herstel gebruikersrisico configureren)
- [Risicogebeurtenistypen](concept-risk-events.md)
