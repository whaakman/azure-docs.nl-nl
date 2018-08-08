---
title: Over het beheren van instellingen voor rolactivering | Microsoft Docs
description: Informatie over het wijzigen van de standaardinstellingen voor bevoegde identiteiten met de extensie van Azure Active Directory Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4ca74c001ba379b287c0c9799d90336eb187b2c2
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619597"
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Over het beheren van instellingen voor rolactivering in Azure AD Privileged Identity Management
Een beheerder met bevoorrechte rol kunt aanpassen Azure AD Privileged Identity Management (PIM) in hun organisatie, met inbegrip van de ervaring voor een gebruiker die is een in aanmerking komende roltoewijzing activeren wijzigen.

## <a name="manage-the-role-activation-settings"></a>De instellingen voor rolactivering beheren
1. Ga naar de [Azure-portal](https://portal.azure.com) en selecteer de **Azure AD Privileged Identity Management** app vanuit het dashboard.
2. Selecteer **bevoorrechte rollen beheren** > **instellingen** > **bevoorrechte rollen**.
3. Selecteer de rol waarvan u de instellingen u wilt beheren.

Er zijn een aantal instellingen die u kunt configureren op de instellingenpagina voor elke rol. Deze instellingen zijn alleen van invloed op gebruikers die in aanmerking komende beheerders, niet-permanente beheerders.

**Activeringen**: de tijd in uren, die een rol actief blijft voordat het verloopt. Dit kan zijn van 1 tot 72 uur.

**Meldingen**: U kunt kiezen of het systeem e-mailberichten verzendt naar beheerders bevestigen dat ze een rol hebt geactiveerd. Dit kan nuttig zijn voor het detecteren van niet-geautoriseerde of illegale activeringen zijn.

**Incident-/ Aanvraagticket**: U kunt wel of niet vereisen dat in aanmerking komende beheerders op te nemen van een Ticketnummer wanneer ze hun rol activeren. Dit kan nuttig zijn wanneer u de rol toegang controles uitvoeren.

**Multi-Factor Authentication**: U kunt wel of niet vereisen dat gebruikers hun identiteit met MFA verifiëren voordat ze hun rol kunnen activeren. Ze hebben alleen om te controleren of deze één keer per sessie niet telkens wanneer ze een rol activeren. Er zijn twee tips waarmee u rekening moet houden wanneer u MFA inschakelen:

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

Zie voor meer informatie over het gebruik van MFA met PIM [hoe u kunt MFA vereisen](pim-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

