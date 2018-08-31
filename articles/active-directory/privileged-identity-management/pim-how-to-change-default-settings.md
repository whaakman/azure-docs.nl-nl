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
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 20a704a0d5b61134a61b5cbf02a1c71dbc7039e1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189334"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Azure AD directory rolinstellingen in PIM configureren

Een beheerder met bevoorrechte rol kunt aanpassen Azure AD Privileged Identity Management (PIM) in hun organisatie, met inbegrip van de ervaring voor een gebruiker die is een in aanmerking komende roltoewijzing activeren wijzigen.

## <a name="open-role-settings"></a>Rolinstellingen openen

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-maprollen**.

1. Klik op **instellingen**.

    ![Azure AD-maprollen - instellingen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Klik op **rollen**.

1. Klik op de rol waarvan u de instellingen u wilt configureren.

    ![Azure AD-maprollen - instellingen voor rollen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Op de instellingenpagina voor elke rol zijn er enkele instellingen die u kunt configureren. Deze instellingen alleen van invloed op gebruikers die zijn **in aanmerking komende** toewijzingen, niet **permanente** toewijzingen.

## <a name="activations"></a>Activeringen

De **activeringen** schuifregelaar is de maximale tijd in uren, die een rol actief blijft voordat het verloopt. Deze waarde kan liggen tussen 1 tot 72 uur.

## <a name="notifications"></a>Meldingen

De **meldingen** switch kunt u kiezen of het systeem e-mailberichten worden verzonden naar beheerders die bevestigen dat ze een rol hebt geactiveerd. Dit kan nuttig zijn voor het detecteren van niet-geautoriseerde of illegale activeringen zijn.

## <a name="incidentrequest-ticket"></a>Incident-/aanvraagticket

De **Incident-/ aanvraagticket** switch kunt u kiezen of in aanmerking komende beheerders op te nemen van een Ticketnummer wanneer ze hun rol activeren. Dit kan nuttig zijn wanneer u de rol toegang controles uitvoeren.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

De **multi-Factor Authentication** switch kunt u kiezen of gebruikers hun identiteit met MFA verifiëren voordat ze hun rol kunnen activeren. Ze hebben alleen om te controleren of deze één keer per sessie niet telkens wanneer ze een rol activeren. Er zijn twee tips waarmee u rekening moet houden wanneer u MFA inschakelen:

* Gebruikers met Microsoft-account voor hun e-mailadressen (meestal @outlook.com, maar niet altijd) kan niet registreren voor Azure MFA. Als u rollen toewijzen aan gebruikers met een Microsoft-account wilt, moet u permanente beheerders toepast of MFA uitschakelen voor die rol.
* U kunt MFA niet uitschakelen voor maximaal bevoorrechte rollen voor Azure AD en Office 365. Dit is een functie veiligheid omdat deze rollen moeten zorgvuldig worden beveiligd:  
  
  * Toepassingsbeheerder
  * Application Proxy server-beheerder
  * Factureringsbeheerder  
  * Beheerder voor naleving  
  * CRM-servicebeheerder
  * Toegangsfiatteur voor klanten-LockBox
  * Schrijver van de Directory  
  * Exchange-beheerder  
  * Globale beheerder
  * Intune-servicebeheerder
  * Postvakbeheerder  
  * Laag1-ondersteuning voor partner  
  * Laag2-ondersteuning voor partner  
  * Beheerder met bevoorrechte rol
  * Beveiligingsbeheerder  
  * SharePoint-beheerder  
  * Skype voor Bedrijven-beheerder  
  * Beheerder van gebruikersaccounts  

Zie voor meer informatie over het gebruik van MFA met PIM [meervoudige verificatie vereisen voor Azure AD directory-rollen in PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Goedkeuring vereisen

De **goedkeuring vereisen** switch kunt u kiezen of u wilt de goedkeuring vereist voor deze rol kunt activeren.

1. Als u de switch instelt op **ingeschakeld**, het deelvenster met opties voor het selecteren van de goedkeurders uitgebreid.

    ![Azure AD-maprollen - Settings - goedkeuring vereisen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Als u **niet** alle goedkeurders opgeeft, worden de Privileged Role Administrators worden de standaard-goedkeurders. Privileged Role Administrators nodig om goed te keuren **alle** activeringsaanvragen voor deze rol.

1. Als u wilt goedkeurders opgeven, klikt u op **fiatteurs selecteren**.

    ![Azure AD-maprollen - Settings - goedkeuring vereisen](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Een of meer fiatteurs selecteren en klik vervolgens op **Selecteer**. U kunt gebruikers of groepen selecteren. Ten minste 2 fiatteurs wordt aanbevolen. Zelf goedkeuren is niet toegestaan.

    Kies de gewenste opties wordt weergegeven in de lijst met geselecteerde goedkeurders.

1. Nadat u uw al uw functie-instellingen hebt opgegeven, klikt u op **opslaan** uw wijzigingen op te slaan.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Volgende stappen

- [Meervoudige verificatie vereisen voor Azure AD directory-rollen in PIM](pim-how-to-require-mfa.md)
- [Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren](pim-how-to-configure-security-alerts.md)
