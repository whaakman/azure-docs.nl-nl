---
title: Gasten uitnodigen en toewijzen van Azure-resource-rollen in PIM - Azure Active Directory | Microsoft Docs
description: Leer hoe u externe gastgebruikers uitnodigen en Azure-resource-rollen in Azure AD Privileged Identity Management (PIM) toe te wijzen.
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
ms.date: 03/13/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68c5e5e2ed0d3ec767a239439476a98bac73bcb4
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576876"
---
# <a name="invite-guest-users-and-assign-azure-resource-roles-in-pim"></a>Gastgebruikers uitnodigen en Azure-resource-rollen in PIM toewijzen

Azure Active Directory (Azure AD) business-to-business (B2B) is een verscheidenheid aan functies in Azure AD waarmee organisaties om samen te werken met externe gastgebruikers (gasten) en leveranciers met behulp van een account. Wanneer u B2B met Azure AD Privileged Identity Management (PIM) combineert, kunt u blijven uw vereisten voor naleving en governance toepassen voor gasten. Bijvoorbeeld, kunt u deze functies PIM voor Azure identity-taken met gasten:

- Toegang tot specifieke Azure-resources toewijzen
- Just-in-time-toegang inschakelen
- Toewijzing duur en einddatum opgeven
- MFA vereisen bij actieve toewijzing of activering
- Toegangsbeoordelingen uitvoeren
- Gebruikmaken van waarschuwingen en auditlogboeken

In dit artikel wordt beschreven hoe u een gast voor uw organisatie uitnodigen en hun toegang tot Azure-resources met behulp van Privileged Identity Management beheren.

## <a name="when-would-you-invite-guests"></a>Als zou u gasten uitnodigen?

Hier volgen enkele scenario's met voorbeelden wanneer u gasten voor uw organisatie uitnodigen mogelijk:

- Toestaan dat een externe zelfstandige leverancier met alleen een e-mailaccount voor toegang tot uw Azure-resources voor een project.
- Toestaan dat een externe partner in een grote organisatie die gebruikmaakt van on-premises Active Directory Federation Services toegang tot uw toepassing kosten.
- Toestaan ondersteuningstechnici niet in uw organisatie (zoals Microsoft-ondersteuning) tijdelijk toegang te krijgen tot uw Azure-resource voor het oplossen van problemen.

## <a name="how-does-collaboration-using-b2b-guests-work"></a>Hoe gaat de samenwerking met behulp van B2B gasten werk?

Wanneer u B2B-samenwerking, kunt u een externe gebruiker uitnodigen voor uw organisatie als gast. De Gast lijkt te zijn in uw organisatie, maar de Gast geen referenties gekoppeld. Wanneer een Gast heeft moeten worden geverifieerd, moeten zij worden geverifieerd in hun oorspronkelijke organisatie en niet in uw organisatie. Dit betekent dat als de Gast heeft niet langer toegang tot hun thuisorganisatie, ze ook geen toegang meer tot uw organisatie. Bijvoorbeeld, als de Gast de organisatie verlaat, ze automatisch geen toegang meer tot alle bronnen die u hebt gedeeld met hen in Azure AD zonder dat u hoeft verder niets te doen. Zie voor meer informatie over B2B [wat is er toegang van gastgebruikers in Azure Active Directory B2B?](../b2b/what-is-b2b.md).

![B2B- en Gast](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-guest-collaboration-settings"></a>Controleer de instellingen van de Gast-samenwerking

Als u wilt controleren of dat u kunt gasten uitnodigen in uw organisatie, moet u uw samenwerking gastinstellingen controleren.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. Klik op **Azure Active Directory** > **gebruikersinstellingen**.

1. Klik op **instellingen voor externe samenwerking beheren**.

    ![Instellingen voor externe samenwerking](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Zorg ervoor dat de **beheerders en gebruikers in de rol van gastuitnodiger kunnen uitnodigingen versturen** is ingesteld op **Ja**.

## <a name="invite-a-guest-and-assign-a-role"></a>Een gast uitnodigen en een rol toewijzen

Met PIM, kunt u een gast uitnodigen en zodat ze in aanmerking voor een Azure-resource-rol net als een gebruiker lid.

1. Aanmelden bij [Azure-portal](https://portal.azure.com/) met een gebruiker die lid is van de [beheerder met bevoorrechte rol](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) of [Gebruikerbeheerder](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) rol.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure-resources**.

1. Gebruik de **resourcefilter** om de lijst van beheerde resources te filteren.

1. Klik op de resource die u beheren wilt, zoals een resource, resourcegroep, abonnement of beheergroep.

    U moet het bereik ingesteld op alleen wat de Gast nodig heeft.

1. Klik onder beheren, op **rollen** voor een overzicht van de rollen voor Azure-resources.

    ![Azure-resources-rollen](./media/pim-resource-roles-external-users/resources-roles.png)

1. Klik op de minimale rol die de gebruiker nodig hebt.

    ![Geselecteerde rol](./media/pim-resource-roles-external-users/selected-role.png)

1. Klik op de functiepagina **lid toevoegen** om de nieuwe Toewijzingsdeelvenster te openen.

1. Klik op **lid of groep selecteren**.

    ![Lid of groep selecteren](./media/pim-resource-roles-external-users/select-member-group.png)

1. Als u wilt een gast uitnodigen, klikt u op **uitnodigen**.

    ![Een gast uitnodigen](./media/pim-resource-roles-external-users/invite-guest.png)

1. Nadat u een gast hebt geselecteerd, klikt u op **uitnodigen**.

    De gast moet worden toegevoegd als lid van een geselecteerde.

1. In de **lid of groep selecteren** deelvenster, klikt u op **Selecteer**.

1. In de **Lidmaatschapsinstellingen** deelvenster, selecteert u het toewijzingstype en de duur.

    ![Lidmaatschapsinstellingen](./media/pim-resource-roles-external-users/membership-settings.png)

1. Klik op om de toewijzing **gedaan** en vervolgens **toevoegen**.

    De roltoewijzing van de Gast worden weergegeven in de lijst van uw rol.

    ![Roltoewijzing voor gast](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-a-guest"></a>Rol als gast activeren

Als een externe gebruiker moet u eerst om te accepteren van de uitnodiging voor uw organisatie Azure AD en mogelijk uw rol kunt activeren.

1. Open het e-mailbericht met de uitnodiging. Het e-mailbericht ziet eruit als het volgende.

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

## <a name="view-activity-for-a-guest"></a>Activiteiten weergeven voor een gast

Net als een gebruiker lid vindt u auditlogboeken om bij te houden wat gasten doen.

1. Als beheerder, opent u PIM en selecteer de resource die is gedeeld.

1. Klik op **resourcecontrole** om de activiteit voor die bron weer te geven. Hieronder ziet u een voorbeeld van de activiteit voor een resourcegroep.

    ![Resourcecontrole](./media/pim-resource-roles-external-users/audit-resource.png)

1. Klik op om de activiteit voor de Gast **Azure Active Directory** > **gebruikers** > de naam van de Gast.

1. Klik op **auditlogboeken** om te zien van de auditlogboeken voor de organisatie. Indien nodig, kunt u filters opgeven.

    ![controle van de organisatie](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-beheerdersrollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Wat is de toegang van gastgebruikers in Azure Active Directory B2B?](../b2b/what-is-b2b.md)
