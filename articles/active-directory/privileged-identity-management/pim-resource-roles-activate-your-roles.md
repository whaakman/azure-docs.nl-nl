---
title: Mijn Azure-resource-rollen in PIM activeren | Microsoft Docs
description: Informatie over het activeren van uw Azure-resource-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59bce2c61db5838bb21a29757d4e354311ecffd5
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666244"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Mijn Azure-resource-rollen in PIM activeren

Met behulp van Azure AD Privileged Identity Management (PIM), kunnen in aanmerking komende rolleden voor de Azure-resources plannen dat activering voor een toekomstige datum en tijd. Ze kunnen ook een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders) selecteren.

In dit artikel geldt voor leden die willen hun Azure-resource-rollen in PIM activeren.

## <a name="activate-a-role"></a>Een rol activeren

Wanneer u nodig hebt om de rol van een Azure-resource te, kunt u activering aanvragen met behulp van de **mijn rollen** navigatieoptie in PIM.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**. Zie voor meer informatie over het toevoegen van de PIM-tegel aan uw dashboard [beginnen met PIM](pim-getting-started.md).

1. Klik op **mijn rollen** voor een overzicht van uw in aanmerking komende Azure AD-directory-rollen en functies van Azure-resource.

    ![Azure AD-directory-rollen en functies van Azure-resource - rollen](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. In de **Azure-resourcerollen** lijst, het vinden van de rol die u wilt activeren.

    ![Azure-resourcerollen - mijn lijst met gebruikersrollen](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Klik op **activeren** om het deelvenster activeren te openen.

1. Als uw rol is vereist voor multi-factor authentication (MFA), klikt u op **Verifieer uw identiteit voordat u doorgaat**. U moet slechts één keer per sessie geverifieerd.

    ![Controleer of met MFA voor rolactivering](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Klik op **Mijn identiteit verifiëren** en volg de instructies voor aanvullende beveiligingsverificatie.

    ![Aanvullende beveiligingsverificatie](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Als u een verminderde bereik opgeven wilt, klikt u op **bereik** het filterdeelvenster Resource te openen.

    Het is beste alleen aanvragen van toegang tot de bronnen die u nodig. U kunt de resourcegroepen of resources die u toegang tot hebt opgeven op het filterdeelvenster Resource.

    ![Activeren - resourcefilter](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Geef zo nodig een begintijd aangepaste activering. Het lid wordt geactiveerd na de geselecteerde periode.

1. In de **reden** voert u de reden voor de aanvraag voor activering.

    ![Voltooide activeren deelvenster](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Klik op **activeren**.

    Als de rol geen goedkeuring is vereist, wordt deze nu geactiveerd en de rol wordt weergegeven in de lijst met actieve rollen. Als de [rol is goedkeuring vereist](pim-resource-roles-approval-workflow.md) wilt activeren, een melding wordt weergegeven in de rechterbovenhoek van uw browser waarin de aanvraag is in afwachting van goedkeuring.

    ![Aanvraag in behandeling melding](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>De status van de aanvragen bekijken

U ziet de status van de in behandeling zijnde aanvragen om te activeren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **mijn aanvragen** aanvragen voor een overzicht van uw Azure AD directory-rol en de rol van Azure-resource.

    ![Azure AD-directory-rollen en functies van Azure-resource - mijn aanvragen](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Schuif naar rechts om weer te geven de **de Status van wijzigingsaanvragen** kolom.

## <a name="use-a-role-immediately-after-activation"></a>Gebruik een rol onmiddellijk na de activering

Vanwege de caching, treden niet onmiddellijk activeringen op in Azure portal zonder te vernieuwen. Als u nodig hebt om te beperken van de mogelijkheid van vertragingen na het activeren van een rol, kunt u de **toegang tot toepassingen** pagina in de portal. Toepassingen die via deze pagina meteen controleren of er nieuwe roltoewijzingen.

1. Open Azure AD Privileged Identity Management.

1. Klik op de **toegang tot toepassingen** pagina.

    ![Toegang tot PIM toepassingen - schermafbeelding](./media/pim-resource-roles-activate-your-roles/pim-application-access.png)

1. Klik op **Azure-resources** opnieuw openen van de portal op de **alle resources** pagina.

    Als u deze koppeling klikt, wordt u geforceerd vernieuwen en er is een controle voor nieuwe Azure-resource-roltoewijzingen.

## <a name="cancel-a-pending-request"></a>Een aanvraag in behandeling annuleren

Als u geen activering van een rol waarvoor goedkeuring wordt vereist, kunt u een aanvraag in behandeling op elk gewenst moment annuleren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **mijn aanvragen**.

1. Voor de rol die u wilt annuleren, klikt u op de **annuleren** koppeling.

    Wanneer u op Annuleren klikt, wordt de aanvraag geannuleerd. Voor het activeren van de rol opnieuw, moet u een nieuwe aanvraag voor activering te verzenden.

   ![Aanvraag in behandeling annuleren](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource-rollen in PIM verlengen of vernieuwen](pim-resource-roles-renew-extend.md)
- [Mijn Azure AD-directory-rollen in PIM activeren](pim-how-to-activate-role.md)