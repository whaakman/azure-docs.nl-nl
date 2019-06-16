---
title: Controlegeschiedenis voor Azure AD-rollen in PIM - Azure Active Directory weergeven | Microsoft Docs
description: Informatie over het weergeven van de controlegeschiedenis voor Azure AD-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/10/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8061cff8d39db66cb22a5650c7688657aa8b3554
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053925"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>Controlegeschiedenis voor Azure AD-rollen in PIM weergeven

U kunt de controlegeschiedenis voor Azure Active Directory (Azure AD) Privileged Identity Management (PIM) gebruiken om te zien van alle roltoewijzingen en activeringen binnen de afgelopen 30 dagen voor alle bevoorrechte rollen. Als u zien van de volledige controlegeschiedenis van activiteit in uw directory wilt, met inbegrip van de beheerder, eindgebruikers en synchronisatieactiviteit, kunt u de [Azure Active Directory-beveiliging en activiteit rapporten](../reports-monitoring/overview-reports.md).

## <a name="view-audit-history"></a>Controlegeschiedenis weergeven

Volg deze stappen om de controlegeschiedenis voor Azure AD-rollen weer te geven.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen**.

1. Klik op **controlegeschiedenis van de adreslijstrollen**.

    Afhankelijk van de controlegeschiedenis wordt een kolomdiagram weergegeven samen met de totale activeringen, max activeringen per dag en gemiddelde activeringen per dag.

    ![Controlegeschiedenis van de adreslijstrollen](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Aan de onderkant van de pagina wordt een tabel met informatie over elke actie in de beschikbare controlegeschiedenis weergegeven. De kolommen hebben de volgende betekenis:

    | Kolom | Description |
    | --- | --- |
    | Time | Wanneer de actie is opgetreden. |
    | Aanvrager | De gebruiker die heeft aangevraagd van de rolactivering of wijzigen. Als de waarde **Azure System**, controleert u de controlegeschiedenis voor Azure voor meer informatie. |
    | Bewerking | Acties die door de aanvrager. Acties kunnen toewijzen, toewijzing verwijderen, activeren, uitschakelen of AddedOutsidePIM opnemen. |
    | Lid | De gebruiker die is geactiveerd of toegewezen aan een rol. |
    | Rol | De rol toegewezen of geactiveerd door de gebruiker. |
    | Redeneren | De tekst die in het veld reden is ingevoerd tijdens de activering. |
    | vervaldatum | Wanneer een geactiveerde rol is verlopen. Geldt alleen voor in aanmerking komende roltoewijzingen. |

1. Als u wilt de controlegeschiedenis sorteren, klikt u op de **tijd**, **actie**, en **rol** knoppen.

## <a name="filter-audit-history"></a>Controlegeschiedenis filteren

1. Aan de bovenkant van de pagina controle van geschiedenis weergegeven, klikt u op de **Filter** knop.

    De **Grafiekparameters bijwerken** deelvenster wordt weergegeven.

1. In **tijdsbereik**, selecteer een tijdsbereik.

1. In **rollen**, toevoegen markeringstekens voor de functies die u wilt weergeven.

    ![Bijwerken van het deelvenster parameters van grafiek](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Klik op **gedaan** om de gefilterde controlegeschiedenis weer te geven.

## <a name="next-steps"></a>Volgende stappen

- [Activiteiten bekijken en controlegeschiedenis voor Azure-resource-rollen in PIM](azure-pim-resource-rbac.md)
