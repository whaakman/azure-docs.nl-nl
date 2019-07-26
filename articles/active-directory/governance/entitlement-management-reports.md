---
title: Rapporten en logboeken weer geven in azure AD rechten beheer (preview)-Azure Active Directory
description: Meer informatie over het weer geven van het rapport voor gebruikers toewijzingen en controle Logboeken in Azure Active Directory rechten beheer (preview).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: ajburnle
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: d33b4751b421f5af1536af9a88d15e060ab59bdb
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489067"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>Rapporten en logboeken weer geven in het beheer van rechten van Azure AD (preview-versie)

> [!IMPORTANT]
> Azure Active Directory (Azure AD)-rechts beheer is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="view-resources-a-user-has-access-to"></a>Bronnen weer geven waartoe een gebruiker toegang heeft

1. Klik op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het linkermenu op **rapport gebruikers toewijzingen**.

1. Klik op **gebruikers selecteren** om het deel venster gebruikers selecteren te openen.

1. Zoek de gebruiker in de lijst waarin u de resources wilt weer geven waartoe ze toegang hebben.

1. Klik op de gebruiker en klik vervolgens op **selecteren**.

    Er wordt een lijst weer gegeven met bronnen waartoe de gebruiker toegang heeft. Het bevat het toegangs pakket, het beleid en de datums.

    ![Rapport over gebruikers toewijzingen](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>De status van de aanvraag van een gebruiker bepalen

Als u meer wilt weten over de manier waarop een gebruiker toegang tot een toegangs pakket heeft aangevraagd en gekregen, kunt u het Azure AD-controle logboek gebruiken. U kunt met name de logboek records in de `EntitlementManagement` categorieën en `UserManagement` gebruiken om meer informatie te krijgen over de verwerkings stappen voor elke aanvraag.  

1. Klik op **Azure Active Directory** en klik vervolgens op **audit logboeken**.

1. Wijzig bovenaan de **categorie** in ofwel `EntitlementManagement` of `UserManagement`, afhankelijk van de controle record die u zoekt.  

1. Klik op **Toepassen**.

1. Klik op **downloaden**om de logboeken te downloaden.

Wanneer Azure AD een nieuwe aanvraag ontvangt, wordt een controle record geschreven waarin de **categorie** is `EntitlementManagement` en de **activiteit** doorgaans `User requests access package assignment`.  In het geval van een directe toewijzing die in de Azure Portal is gemaakt  , is `Administrator directly assigns user to access package`het veld activiteit van de controle record en wordt de gebruiker die de toewijzing uitvoert geïdentificeerd door de **ActorUserPrincipalName**.

Azure AD schrijft extra audit records tijdens de uitvoering van de aanvraag, waaronder:

| Categorie | Activiteit | Aanvraag status |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | Aanvraag heeft geen goed keuring vereist |
| `UserManagement` | `Create request approval` | Aanvraag moet worden goedgekeurd |
| `UserManagement` | `Add approver to request approval` | Aanvraag moet worden goedgekeurd |
| `EntitlementManagement` | `Approve access package assignment request` | Aanvraag goedgekeurd |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |Aanvraag goedgekeurd of vereist geen goed keuring |

Wanneer een gebruiker toegang toegewezen krijgt, schrijft Azure AD een controle record voor de `EntitlementManagement` categorie met **activiteit** `Fulfill access package assignment`.  De gebruiker die de toegang heeft ontvangen, wordt aangeduid met het veld **ActorUserPrincipalName** .

Als er geen toegang is toegewezen, schrijft Azure AD een controle record voor de `EntitlementManagement` categorie met **activiteiten** `Deny access package assignment request`als de aanvraag is geweigerd door een fiatteur of `Access package assignment request timed out (no approver action taken)`als er een time-out is opgetreden voor de aanvraag voordat een fiatteur kan goed keuren.

Wanneer de toewijzing van het toegangs pakket van de gebruiker verloopt, wordt geannuleerd door de gebruiker of door een beheerder wordt verwijderd, schrijft Azure AD een controle record `EntitlementManagement` voor de categorie met `Remove access package assignment`de **activiteit** van.

## <a name="next-steps"></a>Volgende stappen

- [Het beheer van rechten voor Azure AD oplossen](entitlement-management-troubleshoot.md)
- [Algemene scenario's](entitlement-management-scenarios.md)
