---
title: Externe gebruikers uitnodigen en Azure-resource-rollen in PIM toewijzen | Microsoft Docs
description: Leer hoe u externe gebruikers uitnodigen en Azure-resource-rollen in Azure AD Privileged Identity Management (PIM) toe te wijzen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/29/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: a90d0d3d3f484044a0ffbab7a3c24a76c40aa74c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208273"
---
# <a name="invite-external-users-and-assign-azure-resource-roles-in-pim"></a>Externe gebruikers uitnodigen en Azure-resource-rollen in PIM toewijzen

Azure Active Directory (Azure AD) business-to-business (B2B) is een verscheidenheid aan functies in Azure AD waarmee organisaties om samen te werken met externe gebruikers en leveranciers met behulp van een account. Wanneer u B2B met Azure AD Privileged Identity Management (PIM) combineert, kunt u blijven uw vereisten voor naleving en governance toepassen voor externe gebruikers. Bijvoorbeeld, kunt u deze functies PIM voor Azure-resources met externe gebruikers:

- Toegang tot specifieke Azure-resources toewijzen
- Just-in-time-toegang inschakelen
- Toewijzing duur en einddatum opgeven
- MFA vereisen bij actieve toewijzing of activering
- Toegangsbeoordelingen uitvoeren
- Gebruikmaken van waarschuwingen en auditlogboeken

In dit artikel wordt beschreven hoe u een externe gebruiker aan uw directory uitnodigen en hun toegang tot Azure-resources met PIM beheren.

## <a name="when-would-you-invite-external-users"></a>Als zou u externe gebruikers uitnodigen?

Hier volgen enkele scenario's met voorbeelden wanneer u externe gebruikers voor uw directory uitnodigen kan:

- Toestaan dat een externe zelfstandige leverancier met alleen een e-mailaccount voor toegang tot uw Azure-resources voor een project.
- Toestaan dat een externe partner in een grote organisatie die gebruikmaakt van on-premises Active Directory Federation Services toegang tot uw toepassing kosten.
- Toestaan ondersteuningstechnici niet in uw organisatie (zoals Microsoft-ondersteuning) tijdelijk toegang te krijgen tot uw Azure-resource voor het oplossen van problemen.

## <a name="how-does-external-collaboration-using-b2b-work"></a>Hoe gaat de externe samenwerking met B2B-werk?

Wanneer u B2B gebruikt, kunt u een externe gebruiker uitnodigen aan uw directory. De externe gebruiker wordt weergegeven in uw directory, maar de gebruiker heeft geen referenties gekoppeld. Wanneer een externe gebruiker heeft om te worden geverifieerd, moeten zij worden geverifieerd in hun oorspronkelijke directory en niet in uw directory. Dit betekent dat als de externe gebruiker heeft niet langer toegang tot hun oorspronkelijke directory, ze automatisch geen toegang meer tot uw directory. Bijvoorbeeld, als de externe gebruiker hun organisatie verlaat, ze automatisch geen toegang meer tot alle bronnen die u hebt gedeeld met deze in uw directory zonder dat u hoeft verder niets te doen. Zie voor meer informatie over B2B [wat is er toegang van gastgebruikers in Azure Active Directory B2B?](../b2b/what-is-b2b.md).

![B2B en externe gebruiker](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-external-collaboration-settings"></a>Controleer de instellingen voor externe samenwerking

Als u wilt controleren of dat u kunt externe gebruikers uitnodigen in uw directory, Controleer uw instellingen voor externe samenwerking.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. Klik op **Azure Active Directory** > **gebruikersinstellingen**.

1. Klik op **instellingen voor externe samenwerking beheren**.

    ![Instellingen voor externe samenwerking](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Zorg ervoor dat de **beheerders en gebruikers in de rol van gastuitnodiger kunnen uitnodigingen versturen** is ingesteld op **Ja**.

## <a name="invite-an-external-user-and-assign-a-role"></a>Een externe gebruiker uitnodigen en een rol toewijzen

Met PIM, kunt u een externe gebruiker uitnodigen en zodat ze in aanmerking voor een Azure-resource-rol net als een gebruiker lid.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) of [beheerder van gebruikersaccounts](../users-groups-roles/directory-assign-admin-roles.md#user-account-administrator) rol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Gebruik de **resourcefilter** om de lijst van beheerde resources te filteren.

1. Klik op de resource die u beheren wilt, zoals een resource, resourcegroep, abonnement of beheergroep.

    U moet het bereik ingesteld op alleen wat de externe gebruiker nodig heeft.

1. Klik onder beheren, op **rollen** voor een overzicht van de rollen voor Azure-resources.

    ![Azure-resources-rollen](./media/pim-resource-roles-external-users/resources-roles.png)

1. Klik op de minimale rol die de gebruiker nodig hebt.

    ![Geselecteerde rol](./media/pim-resource-roles-external-users/selected-role.png)

1. Klik op de functiepagina **lid toevoegen** om de nieuwe Toewijzingsdeelvenster te openen.

1. Klik op **lid of groep selecteren**.

    ![Lid of groep selecteren](./media/pim-resource-roles-external-users/select-member-group.png)

1. Als u wilt een externe gebruiker uitnodigen, klikt u op **uitnodigen**.

    ![Een gast uitnodigen](./media/pim-resource-roles-external-users/invite-guest.png)

1. Nadat u een externe gebruiker hebt opgegeven, klikt u op **uitnodigen**.

    De externe gebruiker moet worden toegevoegd als lid van een geselecteerde.

1. Selecteer in het deelvenster van een lid of een groep, klikt u op **Selecteer**.

1. Selecteer het toewijzingstype en de duur in het deelvenster van de instellingen van het lidmaatschap.

    ![Lidmaatschapsinstellingen](./media/pim-resource-roles-external-users/membership-settings.png)

1. Klik op om de toewijzing **gedaan** en vervolgens **toevoegen**.

    De roltoewijzing van de externe gebruiker wordt weergegeven in de lijst van uw rol.

    ![Roltoewijzing voor de externe gebruiker](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-an-external-user"></a>Rol als een externe gebruiker activeren

Als een externe gebruiker moet u eerst de uitnodiging aan voor de Azure AD-directory accepteren en mogelijk uw rol kunt activeren.

1. Open het e-mailbericht met de uitnodiging voor uw directory. Het e-mailbericht ziet eruit als het volgende.

    ![E-mailuitnodiging](./media/pim-resource-roles-external-users/email-invite.png)

1. Klik op de **aan de slag** koppeling in het e-mailbericht.

1. Controleer de machtigingen en klik op **accepteren**.

    ![Machtigingen controleren](./media/pim-resource-roles-external-users/invite-accept.png)

1. U mogelijk gevraagd een gebruiksrechtovereenkomst te accepteren en opgeven of u wilt aangemeld blijven.

    De Azure-portal wordt geopend. Als u zich alleen in aanmerking voor een rol, geen u toegang tot bronnen.

1. Als u wilt uw rol kunt activeren, opent u het e-mailbericht met uw rol koppeling activeren. Het e-mailbericht ziet eruit als het volgende.

    ![E-mailuitnodiging](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Klik op **rol activeren** uw in aanmerking komende rollen in PIM openen.

    ![Mijn rollen - in aanmerking komende](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. Onder de actie, klikt u op de **activeren** koppeling.

    Afhankelijk van de rolinstellingen moet u bepaalde informatie voor het activeren van de functie opgeven.

1. Nadat u de instellingen voor de rol hebt opgegeven, klikt u op **activeren** om de rol te activeren.

    ![Rol activeren](./media/pim-resource-roles-external-users/activate-role.png)

    Tenzij de beheerder vereist is voor uw aanvraag goedkeurt, hebt u toegang tot de opgegeven resources.

## <a name="view-activity-for-an-external-user"></a>Activiteit voor een externe gebruiker weergeven

Net als een gebruiker lid vindt u auditlogboeken om bij te houden wat externe gebruikers doen.

1. Als beheerder, opent u PIM en selecteer de resource die is gedeeld.

1. Klik op **resourcecontrole** om de activiteit voor die bron weer te geven. Hieronder ziet u een voorbeeld van de activiteit voor een resourcegroep.

    ![Resourcecontrole](./media/pim-resource-roles-external-users/audit-resource.png)

1. Als u wilt de activiteit voor de externe gebruiker weergeven, klikt u op **Azure Active Directory** > **gebruikers** > externe gebruiker.

1. Klik op **auditlogboeken** om te zien van de auditlogboeken voor de map. Indien nodig, kunt u filters opgeven.

    ![Directorycontrole](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Wat is de toegang van gastgebruikers in Azure Active Directory B2B?](../b2b/what-is-b2b.md)
