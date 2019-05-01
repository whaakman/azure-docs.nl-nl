---
title: Rapporten weergeven en Logboeken in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Meer informatie over het weergeven van de gebruikersrapport van de toewijzingen op en auditlogboeken beschikbaar zijn in Azure Active Directory waar u recht op management (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: jocastel-MSFT
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60a61a581574c77a57939ea23fdadc7b060b82af
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541538"
---
# <a name="view-reports-and-logs-in-azure-ad-entitlement-management-preview"></a>Rapporten weergeven en Logboeken in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="view-resources-a-user-has-access-to"></a>Een gebruiker toegang tot heeft resources weergeven

1. Klik op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **gebruikersrapport van de toewijzingen**.

1. Klik op **gebruikers selecteren** om de geselecteerde gebruikers deelvenster te openen.

1. Zoek de gebruiker in de lijst die u wilt weergeven van de resources die ze toegang hebben.

1. Klik op de gebruiker en klik vervolgens op **Selecteer**.

    Een lijst met resources die de gebruiker toegang tot heeft wordt weergegeven. Het bevat de access-pakket, beleid en datums.

    ![Rapport over gebruikerstoewijzing](./media/entitlement-management-reports/user-assignments-report.png)

## <a name="determine-the-status-of-a-users-request"></a>De status van de aanvraag van een gebruiker bepalen

Als u meer informatie over hoe een gebruiker aangevraagd en toegang tot een package toegang ontvangen, kunt u het auditlogboek van Azure AD. In het bijzonder, kunt u de records in logboek registreren in de `EntitlementManagement` en `UserManagement` categorieën voor aanvullende informatie over de van verwerkingsstappen voor elke aanvraag.  

1. Klik op **Azure Active Directory** en klik vervolgens op **auditlogboeken**.

1. Aan de bovenkant wijzigen de **categorie** aan `EntitlementManagement` of `UserManagement`, afhankelijk van de controlerecord die u zoekt.  

1. Klik op **Toepassen**.

1. De om Logboeken te downloaden, klikt u op **downloaden**.

Als Azure AD een nieuwe aanvraag ontvangt, er wordt een controlerecord geschreven waarin de **categorie** is `EntitlementManagement` en de **activiteit** is doorgaans `User requests access package assignment`.  In het geval van een directe toewijzing gemaakt in Azure portal, de **activiteit** -veld van de controlerecord `Administrator directly assigns user to access package`, en de gebruiker die de toewijzing wordt geïdentificeerd door de **ActorUserPrincipalName**.

Azure AD wordt aanvullende controlerecords schrijven, terwijl de aanvraag uitgevoerd wordt, met inbegrip van:

| Category | Activiteit | Aanvraagstatus |
| :---- | :------------ | :------------ |
| `EntitlementManagement` | `Auto approve access package assignment request` | Aanvraag is niet vereist voor goedkeuring |
| `UserManagement` | `Create request approval` | Aanvraag is goedkeuring vereist |
| `UserManagement` | `Add approver to request approval` | Aanvraag is goedkeuring vereist |
| `EntitlementManagement` | `Approve access package assignment request` | Aanvraag goedgekeurd |
| `EntitlementManagement` | `Ready to fulfill access package assignment request` |Aanvraag goedgekeurd of er geen goedkeuring vereist |

Wanneer een gebruiker wordt toegewezen toegang, Azure AD een controlerecord voor schrijft de `EntitlementManagement` categorie met **activiteit** `Fulfill access package assignment`.  De gebruiker die de toegang hebben gekregen wordt geïdentificeerd door **ActorUserPrincipalName** veld.

Als toegang niet is toegewezen, wordt Azure AD schrijft een controlerecord voor de `EntitlementManagement` categorie met **activiteit** beide `Deny access package assignment request`, als de aanvraag is geweigerd door een goedkeurder of `Access package assignment request timed out (no approver action taken)`, als de aanvraag is een time-out voor een goedkeurder kan goedkeuren.

Wanneer de gebruiker toegang pakkettoekenning is verlopen, wordt door de gebruiker geannuleerd of verwijderd door een beheerder, en Azure AD schrijft een controlerecord voor de `EntitlementManagement` categorie met **activiteit** van `Remove access package assignment`.

## <a name="next-steps"></a>Volgende stappen

- [Problemen met Azure AD recht management oplossen](entitlement-management-troubleshoot.md)
- [Algemene scenario's](entitlement-management-scenarios.md)
