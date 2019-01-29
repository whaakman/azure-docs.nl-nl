---
title: Azure AD directory rolinstellingen in PIM configureren | Microsoft Docs
description: Informatie over het configureren van instellingen voor Azure AD directory-rol in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 28958627ee66d0586fabff30aa3e5d041fb7dd80
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196749"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Azure AD directory rolinstellingen in PIM configureren

Een beheerder met bevoorrechte rol kunt aanpassen Azure AD Privileged Identity Management (PIM) in hun organisatie, met inbegrip van de ervaring voor een gebruiker die is een in aanmerking komende roltoewijzing activeren wijzigen.

## <a name="open-role-settings"></a>Rolinstellingen openen

Volg deze stappen om de instellingen voor een Azure AD-directory-rol te openen.

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-maprollen**.

1. Klik op **Instellingen**.

    ![Azure AD-maprollen - instellingen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Klik op **rollen**.

1. Klik op de rol waarvan u de instellingen u wilt configureren.

    ![Azure AD-maprollen - instellingen voor rollen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Op de instellingenpagina voor elke rol zijn er enkele instellingen die u kunt configureren. Deze instellingen alleen van invloed op gebruikers die zijn **in aanmerking komende** toewijzingen, niet **permanente** toewijzingen.

## <a name="activations"></a>Activeringen

Gebruik de **activeringen** schuifregelaar om in te stellen de maximale tijd in uren, die een rol actief blijft voordat het verloopt. Deze waarde kan liggen tussen 1 tot 72 uur.

## <a name="notifications"></a>Meldingen

Gebruik de **meldingen** switch om op te geven of beheerders e-mailmeldingen ontvangen wanneer functies worden geactiveerd. Dit kan nuttig zijn voor het detecteren van niet-geautoriseerde of illegale activeringen zijn.

Als de waarde **inschakelen**, meldingen worden verzonden naar:

- Beheerder met bevoorrechte rol
- Beveiligingsbeheerder
- Globale beheerder

Zie voor meer informatie, [e-mailmeldingen in PIM](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Incident-/aanvraagticket

Gebruik de **Incident-/ aanvraagticket** switch om op te geven of om te vereisen in aanmerking komende beheerders op te nemen van een Ticketnummer wanneer ze hun rol activeren. Dit kan nuttig zijn wanneer u de rol toegang controles uitvoeren.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Gebruik de **multi-Factor Authentication** switch om op te geven of gebruikers hun identiteit met MFA verifiëren voordat ze hun rol kunnen activeren. Ze hebben alleen om te controleren of deze één keer per sessie niet telkens wanneer ze een rol activeren. Er zijn twee tips waarmee u rekening moet houden wanneer u MFA inschakelen:

* Gebruikers met Microsoft-account voor hun e-mailadressen (meestal @outlook.com, maar niet altijd) kan niet registreren voor Azure MFA. Als u rollen toewijzen aan gebruikers met een Microsoft-account wilt, moet u permanente beheerders toepast of MFA uitschakelen voor die rol.
* U kunt MFA niet uitschakelen voor maximaal bevoorrechte rollen voor Azure AD en Office 365. Dit is een functie veiligheid omdat deze rollen moeten zorgvuldig worden beveiligd:  
  
  * Factureringsbeheerder
  * Beheerder van de cloudtoepassing
  * Beheerder voor naleving
  * Voorwaardelijke toegang beheerder
  * CRM-servicebeheerder
  * Toegangsfiatteur voor Klanten-lockbox
  * Schrijvers van mappen
  * Exchange-beheerder
  * Globale beheerder
  * Information Protection-beheerder
  * Intune-servicebeheerder
  * Servicebeheerder van Power BI
  * Beheerder met bevoorrechte rol
  * Beveiligingsbeheerder
  * SharePoint-servicebeheerder
  * Skype voor Bedrijven-beheerder
  * Gebruikerbeheerder

Zie voor meer informatie, [multi-factor authentication (MFA) en PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Goedkeuring vereisen

Als u wilt de goedkeuring van een rol wilt activeren, volgt u deze stappen.

1. Stel de **goedkeuring vereisen** overschakelen naar **ingeschakeld**. Hiermee breidt u het deelvenster met opties voor het selecteren van de goedkeurders.

    ![Azure AD-maprollen - Settings - goedkeuring vereisen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Als u **niet** alle goedkeurders opgeeft, worden de Privileged Role Administrators worden de standaard-goedkeurders. Privileged Role Administrators nodig om goed te keuren **alle** activeringsaanvragen voor deze rol.

1. Als u wilt goedkeurders opgeven, klikt u op **fiatteurs selecteren**.

    ![Azure AD-maprollen - Settings - goedkeuring vereisen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Een of meer fiatteurs selecteren en klik vervolgens op **Selecteer**. U kunt gebruikers of groepen selecteren. Ten minste 2 fiatteurs wordt aanbevolen. Zelf goedkeuren is niet toegestaan.

    Kies de gewenste opties wordt weergegeven in de lijst met geselecteerde goedkeurders.

1. Nadat u uw al uw functie-instellingen hebt opgegeven, klikt u op **opslaan** uw wijzigingen op te slaan.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Volgende stappen

- [Azure AD-directory-rollen in PIM toewijzen](pim-how-to-add-role-to-user.md)
- [Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren](pim-how-to-configure-security-alerts.md)
