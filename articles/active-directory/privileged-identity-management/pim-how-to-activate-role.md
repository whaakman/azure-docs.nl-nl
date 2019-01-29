---
title: Mijn Azure AD-directory-rollen in PIM activeren | Microsoft Docs
description: Informatie over het activeren van Azure AD-directory-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 75c8952b1fa7003b6a2c12c7e3e6be4983a25bba
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174105"
---
# <a name="activate-my-azure-ad-directory-roles-in-pim"></a>Mijn Azure AD-directory-rollen in PIM activeren

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) vereenvoudigt de manier waarop ondernemingen bevoegde toegang tot resources in Azure AD en andere Microsoft online services zoals Office 365 en Microsoft Intune beheren.  

Als u hebt aangebracht in aanmerking voor een beheerdersrol, betekent dit dat u die rol kunt activeren wanneer u nodig hebt bevoegde acties uit te voeren. Bijvoorbeeld als u Office 365-functies van tijd tot tijd beheert, beheerders met bevoegdheid van uw organisatie mogelijk niet moet u een permanente globale beheerder, omdat die rol te invloed is op andere services. In plaats daarvan moeten deze u in aanmerking voor Azure AD-rollen, zoals Exchange Online-beheerder. U kunt aanvragen om te activeren die rol wanneer u de bevoegdheden nodig hebt, en vervolgens beheerdersbevoegdheden voor een vooraf vastgestelde periode hebt u.

Dit artikel is bedoeld voor beheerders die u nodig hebt om hun Azure AD directory-rol in PIM te activeren.

## <a name="activate-a-role"></a>Een rol activeren

Als u uitvoeren op een Azure AD-directory-rol wilt, kunt u activering aanvragen met behulp van de **mijn rollen** navigatieoptie in PIM.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Open **Azure AD Privileged Identity Management**. Zie voor meer informatie over het toevoegen van de PIM-tegel aan uw dashboard [beginnen met PIM](pim-getting-started.md).

1. Klik op **Azure AD-maprollen**.

1. Klik op **mijn rollen** voor een overzicht van uw in aanmerking komende Azure AD-maprollen.

    ![Azure AD-maprollen - mijn rollen](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Zoek een rol die u wilt activeren.

    ![Azure AD-maprollen - mijn lijst met gebruikersrollen](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Klik op **activeren** openen van het detailvenster van de rol activeren.

1. Als uw rol is vereist voor multi-factor authentication (MFA), klikt u op **Verifieer uw identiteit voordat u doorgaat**. U moet slechts één keer per sessie geverifieerd.

    ![Controleer of met MFA voor rolactivering](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Klik op **Mijn identiteit verifiëren** en volg de instructies voor aanvullende beveiligingsverificatie.

    ![Aanvullende beveiligingsverificatie](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Klik op **activeren** om de activering deelvenster te openen.

    ![Activering van deelvenster](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Geef zo nodig een begintijd aangepaste activering.

1. Geef de duur van de activering.

1. In de **activering reden** voert u de reden voor de aanvraag voor activering. Sommige rollen moeten u een Ticketnummer problemen opgeven.

    ![Voltooide activering deelvenster](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Klik op **Activeren**.

    Als de rol geen goedkeuring is vereist, wordt deze geactiveerd en toegevoegd aan de lijst met actieve rollen. Als u de rol meteen gebruiken wilt, volgt u de stappen in de volgende sectie.

    Als de [rol is goedkeuring vereist](./azure-ad-pim-approval-workflow.md) wilt activeren, een melding wordt weergegeven in de rechterbovenhoek van uw browser waarin de aanvraag is in afwachting van goedkeuring.

    ![Aanvraag in behandeling melding](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Gebruik een rol onmiddellijk na de activering

Als u een rol in PIM kunt activeren, gaat de ten minste tien minuten voordat u kunt toegang krijgen de gewenste beheerdersportal tot of functies in een specifieke werkbelasting uitvoeren. Als u wilt afdwingen dat een update van uw machtigingen, gebruiken de **toegang tot toepassingen** pagina zoals beschreven in de volgende stappen uit.

1. Open Azure AD Privileged Identity Management.

1. Klik op de **toegang tot toepassingen** pagina.

    ![Toegang tot de PIM-toepassing](./media/pim-how-to-activate-role/pim-application-access.png)

1. Klik op de **Azure Active Directory** koppeling naar de portal openen op de **alle gebruikers** pagina.

    Als u deze koppeling klikt, kunt u uw huidige token vervalt en afdwingen dat de Azure-portal een nieuw token met uw bijgewerkte machtigingen te verkrijgen.

## <a name="view-the-status-of-your-requests"></a>De status van de aanvragen bekijken

U ziet de status van de in behandeling zijnde aanvragen om te activeren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **Azure AD-maprollen**.

1. Klik op **mijn aanvragen** voor een overzicht van de aanvragen.

    ![Azure AD-maprollen - mijn aanvragen](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Een rol deactiveren

Wanneer een rol is geactiveerd, wordt deze automatisch uitgeschakeld wanneer de tijdslimiet (in aanmerking komende duur) is bereikt.

Als u uw beheerderstaken vroeg hebt voltooid, kunt u ook een rol handmatig in Azure AD Privileged Identity Management deactiveren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **Azure AD-maprollen**.

1. Klik op **mijn rollen**.

1. Klik op **actieve rollen** om te zien van de lijst met actieve rollen.

1. Vinden van de rol die u klaar bent en klik vervolgens op **deactiveren**.

## <a name="cancel-a-pending-request"></a>Een aanvraag in behandeling annuleren

Als u geen activering van een rol waarvoor goedkeuring wordt vereist, kunt u een aanvraag in behandeling op elk gewenst moment annuleren.

1. Open Azure AD Privileged Identity Management.

1. Klik op **Azure AD-maprollen**.

1. Klik op **mijn aanvragen**.

1. Voor de rol die u wilt annuleren, klikt u op de **annuleren** knop.

    Wanneer u op Annuleren klikt, wordt de aanvraag geannuleerd. Voor het activeren van de rol opnieuw, moet u een nieuwe aanvraag voor activering te verzenden.

   ![Aanvraag in behandeling annuleren](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Problemen oplossen

### <a name="permissions-not-granted-after-activating-a-role"></a>Machtigingen niet na het activeren van een rol

Als u een rol in PIM kunt activeren, gaat de ten minste tien minuten voordat u kunt toegang krijgen de gewenste beheerdersportal tot of functies in een specifieke werkbelasting uitvoeren. Als u wilt afdwingen dat een update van uw machtigingen, gebruiken de **toegang tot toepassingen** pagina zoals eerder beschreven in [gebruik van een rol onmiddellijk na de activering](#use-a-role-immediately-after-activation).

Zie voor aanvullende stappen voor probleemoplossing, [verhoogde machtigingen voor het oplossen van](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx).

## <a name="next-steps"></a>Volgende stappen

- [Mijn Azure-resource-rollen in PIM activeren](pim-resource-roles-activate-your-roles.md)
