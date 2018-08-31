---
title: Beginnen met PIM - Azure | Microsoft Docs
description: Leer hoe u aan de slag met Azure AD Privileged Identity Management (PIM) in Azure portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 5b3bff27821964648713b02589c941c99e3eb03d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190086"
---
# <a name="start-using-pim"></a>Beginnen met PIM

Met Azure Active Directory (Azure AD) Privileged Identity Management (PIM), die u kunt beheren, controleren en toegang binnen uw organisatie controleren. Dit bereik is inclusief toegang tot Azure-resources, Azure AD en andere Microsoft-onlineservices, zoals Office 365 en Microsoft Intune.

In dit artikel leest u hoe u de PIM-app van Azure AD toevoegt aan uw Azure Portal-dashboard.

## <a name="first-person-to-use-pim"></a>Eerste persoon PIM gaat gebruiken

Als u de eerste persoon PIM gaat gebruiken in uw directory bent, u automatisch zijn toegewezen de [beveiligingsbeheerder](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) en [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rollen in de map. Alleen beheerders met bevoegdheid kunnen directory-roltoewijzingen van gebruikers in Azure AD beheren. Bovendien kunt u de [beveiligingswizard](pim-security-wizard.md) uitvoeren. Deze helpt u bij de eerste ervaring met detectie en toewijzing.

## <a name="add-pim-tile-to-the-dashboard"></a>PIM-tegel aan het dashboard toevoegen

Als u wilt maken het gemakkelijker om te openen PIM, moet u een tegel PIM toevoegen aan uw Azure portal-dashboard.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw directory.

1. Klik op **alle services** en zoek de **Azure AD Privileged Identity Management** service.

    ![Azure AD Privileged Identity Management in alle services](./media/pim-getting-started/pim-all-services-find.png)

1. Klik op om de PIM-snelstartgids.

1. Controleer **blade vastmaken aan dashboard** om de blade Snelstartgids voor PIM aan het dashboard vast te maken.

    ![Blade vastmaken aan dashboard](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    In de Azure-dashboard ziet u een tegel als volgt:

    ![PIM-snelstartgids-tegel](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="navigate-to-your-tasks"></a>Navigeer naar uw taken

Nadat het PIM is ingesteld, kunt u deze blade kunt gebruiken om uit te voeren taken voor identiteitsbeheer.

![Taken op het hoogste niveau voor PIM - schermopname](./media/pim-getting-started/pim-quickstart-tasks.png)

| Taak + beheren | Beschrijving |
| --- | --- |
| **Mijn rollen**  | Geeft een lijst van in aanmerking komende en actieve rollen aan u toegewezen. Hier kunt u alle in aanmerking komende toegewezen rollen activeren. |
| **Mijn aanvragen** | Geeft de in behandeling zijnde aanvragen voor het activeren van in aanmerking komende roltoewijzingen. |
| **Toegang tot toepassingen** | Hiermee kunt u potentiële vertragingen verminderen en het gebruik van een rol onmiddellijk na de activering. |
| **Aanvragen goedkeuren** | Geeft een lijst weer van aanvragen voor het activeren van in aanmerking komende rollen door gebruikers in uw directory die u hebt aangewezen om goed te keuren. |
| **Toegang beoordelen** | Een lijst met actieve toegangsbeoordelingen die u zijn toegewezen om te voltooien, of u de toegang voor uzelf of iemand anders beoordeelt nu. |
| **Azure AD-maprollen** | Geeft een dashboard en de instellingen voor beheerders met bevoegdheid voor het beheren van roltoewijzingen voor Azure AD-directory. Dit dashboard is uitgeschakeld voor iedereen die geen beheerder met een bevoorrechte rol is. Deze gebruikers hebben toegang tot een speciaal dashboard met de titel Mijn weergave. Het dashboard Mijn weergave bevat alleen informatie over de gebruiker die toegang heeft tot het dashboard, niet de gehele tenant. |
| **Azure-resources** | Geeft een dashboard en de instellingen voor beheerders met bevoegdheid voor het beheren van roltoewijzingen van de Azure-resource. Dit dashboard is uitgeschakeld voor iedereen die geen beheerder met een bevoorrechte rol is. Deze gebruikers hebben toegang tot een speciaal dashboard met de titel Mijn weergave. Het dashboard Mijn weergave bevat alleen informatie over de gebruiker die toegang heeft tot het dashboard, niet de gehele tenant. |

## <a name="next-steps"></a>Volgende stappen

- [Mijn Azure AD-directory-rollen in PIM activeren](pim-how-to-activate-role.md)
- [Mijn Azure-resource-rollen in PIM activeren](pim-resource-roles-activate-your-roles.md)
