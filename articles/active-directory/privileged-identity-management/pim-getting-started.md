---
title: Beginnen met PIM - Azure | Microsoft Docs
description: Meer informatie over het inschakelen en aan de slag met Azure AD Privileged Identity Management (PIM) in Azure portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 11/09/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09914de48df09dabd4069cd33e7acbea328fa89d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56193149"
---
# <a name="start-using-pim"></a>Beginnen met PIM

Met Azure Active Directory (Azure AD) Privileged Identity Management (PIM), die u kunt beheren, controleren en toegang binnen uw organisatie controleren. Dit bereik is inclusief toegang tot Azure-resources, Azure AD en andere Microsoft-onlineservices, zoals Office 365 en Microsoft Intune.

Dit artikel wordt beschreven hoe u kunt inschakelen en aan de slag met PIM.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van PIM, moet u een van de volgende licenties hebben:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Zie voor meer informatie, [licentie-vereisten voor het gebruik van PIM](subscription-requirements.md).

## <a name="first-person-to-use-pim"></a>Eerste persoon PIM gaat gebruiken

Als u de eerste persoon PIM gaat gebruiken in uw directory bent, u automatisch zijn toegewezen de [beveiligingsbeheerder](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) en [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rollen in de map. Alleen beheerders met bevoegdheid kunnen directory-roltoewijzingen van gebruikers in Azure AD beheren. Bovendien kunt u besluiten om uit te voeren de [beveiligingswizard](pim-security-wizard.md) deze helpt u bij de eerste ervaring van detectie en toewijzing.

## <a name="enable-pim"></a>PIM inschakelen

Als u wilt gaan met PIM in uw directory, moet u eerst PIM inschakelen.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van uw directory.

    U moet een globale beheerder met een organisatie-account (bijvoorbeeld @yourdomain.com), niet een Microsoft-account (bijvoorbeeld @outlook.com), PIM inschakelen voor een map.

1. Klik op **alle services** en zoek de **Azure AD Privileged Identity Management** service.

    ![Azure AD Privileged Identity Management in alle services](./media/pim-getting-started/pim-all-services-find.png)

1. Klik op om de PIM-snelstartgids.

1. Klik in de lijst **akkoord voor PIM**.

    ![Akkoord voor PIM](./media/pim-getting-started/consent-pim.png)

1. Klik op **Mijn identiteit verifiëren** om uw identiteit met Azure MFA te verifiëren. U wordt gevraagd om op te halen van een account.

    ![Een account kiezen](./media/pim-getting-started/pick-account.png)

1. Als u meer informatie is vereist voor verificatie, gaat u door de geleid. Zie voor meer informatie, [hulp bij het met verificatie in twee stappen](https://go.microsoft.com/fwlink/p/?LinkId=708614).

    ![Meer informatie vereist](./media/pim-getting-started/more-information-required.png)

    Bijvoorbeeld, u mogelijk gevraagd voor verificatie via de telefoon.

    ![Aanvullende beveiligingsverificatie](./media/pim-getting-started/additional-security-verification.png)

1. Nadat u het verificatieproces hebt voltooid, klikt u op de **toestemming geven** knop.

1. In het bericht dat wordt weergegeven, klikt u op **Ja** akkoord gaan met de PIM-service.

    ![Instemmen met PIM-bericht](./media/pim-getting-started/consent-pim-message.png)

## <a name="sign-up-pim-for-azure-ad-roles"></a>PIM registreren voor Azure AD-rollen

Als u PIM hebt ingeschakeld voor uw directory, moet u PIM registreren voor het beheren van Azure AD-rollen.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen**.

    ![PIM registreren voor Azure AD-rollen](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. Klik op **aanmelden**.

1. In het bericht dat wordt weergegeven, klikt u op **Ja** PIM aanmelden voor het beheren van Azure AD-rollen.

    ![PIM registreren voor Azure AD-rollen bericht](./media/pim-getting-started/sign-up-pim-message.png)

    Bij het aanmelden is voltooid, wordt de Azure AD-opties worden ingeschakeld. Mogelijk moet u de portal vernieuwen.

    Zie voor meer informatie over het detecteren en selecteert u de Azure-resources te beschermen met PIM [detecteren Azure-resources te beheren in PIM](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Navigeer naar uw taken

Nadat het PIM is ingesteld, kunt u taken voor identiteitsbeheer kunt uitvoeren.

![Taken op het hoogste niveau voor PIM - schermopname](./media/pim-getting-started/pim-quickstart-tasks.png)

| Taak + beheren | Description |
| --- | --- |
| **Mijn rollen**  | Geeft een lijst van in aanmerking komende en actieve rollen aan u toegewezen. Hier kunt u alle in aanmerking komende toegewezen rollen activeren. |
| **Mijn aanvragen** | Geeft de in behandeling zijnde aanvragen voor het activeren van in aanmerking komende roltoewijzingen. |
| **Toegang tot toepassingen** | Hiermee kunt u potentiële vertragingen verminderen en het gebruik van een rol onmiddellijk na de activering. |
| **Aanvragen goedkeuren** | Geeft een lijst weer van aanvragen voor het activeren van in aanmerking komende rollen door gebruikers in uw directory die u hebt aangewezen om goed te keuren. |
| **Toegang beoordelen** | Een lijst met actieve toegangsbeoordelingen die u zijn toegewezen om te voltooien, of u de toegang voor uzelf of iemand anders beoordeelt nu. |
| **Azure AD-rollen** | Geeft een dashboard en de instellingen voor beheerders met bevoegdheid voor het beheren van roltoewijzingen voor Azure AD-directory. Dit dashboard is uitgeschakeld voor iedereen die geen beheerder met een bevoorrechte rol is. Deze gebruikers hebben toegang tot een speciaal dashboard met de titel Mijn weergave. Het dashboard Mijn weergave bevat alleen informatie over de gebruiker die toegang heeft tot het dashboard, niet de gehele tenant. |
| **Azure-resources** | Geeft een dashboard en de instellingen voor beheerders met bevoegdheid voor het beheren van roltoewijzingen van de Azure-resource. Dit dashboard is uitgeschakeld voor iedereen die geen beheerder met een bevoorrechte rol is. Deze gebruikers hebben toegang tot een speciaal dashboard met de titel Mijn weergave. Het dashboard Mijn weergave bevat alleen informatie over de gebruiker die toegang heeft tot het dashboard, niet de gehele tenant. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Een tegel PIM aan het dashboard toevoegen

Als u wilt maken het gemakkelijker om te openen PIM, moet u een tegel PIM toevoegen aan uw Azure portal-dashboard.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Klik op **alle services** en zoek de **Azure AD Privileged Identity Management** service.

    ![Azure AD Privileged Identity Management in alle services](./media/pim-getting-started/pim-all-services-find.png)

1. Klik op om de PIM-snelstartgids.

1. Controleer **blade vastmaken aan dashboard** om de blade Snelstartgids voor PIM aan het dashboard vast te maken.

    ![Blade vastmaken aan dashboard](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    In de Azure-dashboard ziet u een tegel als volgt:

    ![PIM-snelstartgids-tegel](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Azure-resources te beheren in PIM detecteren](pim-resource-roles-discover-resources.md)
