---
title: Beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal | Microsoft Docs
description: Meer informatie over het beveiligingsrapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure Active Directory-portal
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9a7a3877970d5ecf3b86471b94fbb1bf6e5efbb4
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621068"
---
# <a name="users-flagged-for-risk-report-in-the-azure-portal"></a>Rapport met gebruikers die risico lopen in de Azure-portal

Azure Active Directory (Azure AD) detecteert verdachte activiteit in verband met uw gebruikersaccounts. Voor elke gedetecteerde activiteit wordt een record met de naam [risicogebeurtenis](concept-risk-events.md) gemaakt.

De beveiligingsrapporten zijn beschikbaar via de [Azure-portal](https://portal.azure.com). Selecteer de blade **Azure Active Directory** en ga vervolgens naar de sectie **Beveiliging**. 

De gedetecteerde risico's worden gebruikt om het volgende te berekenen:

- **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. 

- **Gebruikers voor wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. 

Zie [Het beleid voor gebruikersrisico's configureren](../identity-protection/howto-user-risk-policy.md) voor informatie over het configureren van de beleidsregels die deze risicogebeurtenissen activeren. 

![Riskante aanmeldingen](./media/concept-user-at-risk/10.png)


## <a name="what-azure-ad-license-do-you-need-to-access-the-users-at-risk-report"></a>Welke Azure AD-licentie heb ik nodig voor toegang tot het rapport met gebruikers die risico lopen?  

Alle edities van Azure Active Directory bieden rapporten over gebruikers voor wie wordt aangegeven dat ze risico lopen. Het detailniveau van rapporten verschilt wel per editie: 

- In de edities **Azure Active Directory Free en Basic** hebt u toegang tot een lijst met gebruikers voor wie wordt aangegeven dat ze risico lopen. 

- Daarnaast kunt u in de editie **Azure Active Directory Premium 1** bepaalde onderliggende risicogebeurtenissen onderzoeken die voor elk rapport zijn gedetecteerd. 

- De editie **Azure Active Directory Premium 2** biedt u de meest gedetailleerde informatie over alle onderliggende risicogebeurtenissen. Deze editie stelt u ook in staat beveiligingsbeleidsregels te configureren die automatisch op de geconfigureerde risiconiveaus reageren.


## <a name="users-at-risk-report-for-azure-ad-free-and-basic-editions"></a>Rapport met gebruikers die risico lopen voor de gratis en Basic-editie van Azure AD

Het rapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de gratis en Basic-editie van Azure AD biedt een lijst gebruikersaccounts die mogelijk zijn aangetast. 

![Riskante aanmeldingen](./media/concept-user-at-risk/03.png)

Wanneer u op een gebruiker klikt, wordt de betreffende blade met gebruikersgegevens geopend. Controleer de aanmeldgeschiedenis van gebruikers die risico lopen en stel het wachtwoord indien nodig opnieuw in.

![Riskante aanmeldingen](./media/concept-user-at-risk/46.png)


Dit rapport biedt de volgende mogelijkheden:

- Het rapport downloaden

- Gebruikers zoeken

![Riskante aanmeldingen](./media/concept-user-at-risk/16.png)


## <a name="users-at-risk-report-for-azure-ad-premium-editions"></a>Rapport met gebruikers die risico lopen voor Premium-edities van Azure AD

Het rapport over gebruikers voor wie wordt aangegeven dat ze risico lopen in de Azure AD Premium-edities biedt u het volgende:

- Een lijst met gebruikersaccounts die mogelijk zijn aangetast 

- Verzamelde informatie over de gedetecteerde [risicogebeurtenistypen](concept-risk-events.md)

- Een optie voor het downloaden van het rapport

- Een optie voor het configureren van een [beleid voor herstel van gebruikersrisico's](../identity-protection/howto-user-risk-policy.md)  

![Riskante aanmeldingen](./media/concept-user-at-risk/71.png)

Wanneer u een gebruiker selecteert, krijgt u een gedetailleerde rapportweergave voor deze gebruiker waarmee u het volgende kunt:

- De weergave Alle aanmeldingen openen

- Het gebruikerswachtwoord opnieuw instellen

- Alle gebeurtenissen sluiten

- De gemelde risico's voor de gebruiker onderzoeken. 

![Riskante aanmeldingen](./media/concept-user-at-risk/324.png)

Als u een risicogebeurtenis wilt onderzoeken, selecteert u de gebeurtenis in de lijst om de bijbehorende blade **Details** te openen. Op de blade **Details** kunt u een risicogebeurtenis handmatig sluiten of een handmatig gesloten risicogebeurtenis opnieuw activeren. 

![Riskante aanmeldingen](./media/concept-user-at-risk/325.png)


## <a name="next-steps"></a>Volgende stappen

- [Het beleid voor gebruikersrisico's configureren](../identity-protection/howto-user-risk-policy.md)
- [How to configure the risk remediation policy](../identity-protection/howto-user-risk-policy.md) (Beleid voor herstel gebruikersrisico configureren)
- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

