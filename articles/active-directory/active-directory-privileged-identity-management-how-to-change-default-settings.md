---
title: Het beheren van serverfunctie-instellingen voor activering | Microsoft Docs
description: Informatie over het wijzigen van de standaardinstellingen voor bevoegde identiteiten met de extensie Azure Active Directory Privileged Identity Management.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2cf361dc879915c316e7ee907b5c70b5907459c2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Het beheren van instellingen voor de sitesysteemrol activering in Azure AD Privileged Identity Management
Een beheerder met bevoorrechte rol kunt aanpassen Azure AD Privileged Identity Management (PIM) in hun organisatie, inclusief het wijzigen van de ervaring voor een gebruiker die een in aanmerking komende roltoewijzing is geactiveerd.

## <a name="manage-the-role-activation-settings"></a>De instellingen van de activering beheren
1. Ga naar de [Azure-portal](https://portal.azure.com) en selecteer de **Azure AD Privileged Identity Management** app vanuit het dashboard.
2. Selecteer **bevoorrechte rollen beheren** > **instellingen** > **bevoorrechte rollen**.
3. Selecteer de rol waarvoor u de instellingen u wilt beheren.

Er zijn een aantal instellingen die u kunt configureren op de instellingenpagina voor elke rol. Deze instellingen zijn alleen van invloed op gebruikers die in aanmerking komende beheerders, niet-permanente beheerders zijn.

**Activeringen**: de tijd in uren, die een rol actief blijft voordat het verloopt. Dit kan zijn tussen 1 en 72 uur.

**Meldingen**: U kunt kiezen of het systeem e-mailberichten verzendt naar het admins bevestigen dat ze een rol hebben geactiveerd. Dit kan handig zijn voor het detecteren van niet-geautoriseerde of illegale activeringen zijn.

**Ticket incident/aanvraag**: U kunt al dan niet in aanmerking komende beheerders een Ticketnummer nemen bij het activeren van hun rol nodig. Dit kan nuttig zijn wanneer u de rol toegang audits uitvoert.

**Multi-Factor Authentication**: U kunt kiezen of gebruikers hun identiteit verifiëren met MFA voordat ze hun rollen kunnen activeren. Ze hebben alleen om te controleren of deze eenmaal per sessie niet elke keer dat ze een rol activeren. Er zijn twee tips rekening moet houden wanneer u MFA inschakelen:

* Gebruikers met Microsoft-account voor hun e-mailadressen (meestal @outlook.com, maar niet altijd) kan niet registreren voor Azure MFA. Als u wilt toewijzen van rollen aan gebruikers met Microsoft-accounts, moet u doen permanente beheerders of MFA uitschakelen voor die rol.
* U kunt MFA niet uitschakelen voor maximaal bevoorrechte rollen voor Azure AD en Office365. Dit is een functie veiligheid omdat deze rollen moeten zorgvuldig worden beveiligd:  
  
  * Toepassingsbeheerder
  * Toepassingsbeheerder Proxy server
  * Financieel medewerker  
  * Naleving beheerder  
  * CRM-servicebeheerder
  * Klant LockBox toegang goedkeurder
  * Schrijver van de Directory  
  * Exchange-beheerder  
  * Globale beheerder
  * Intune-servicebeheerder
  * Postvak beheerder  
  * Partner tier1-ondersteuning  
  * Partner tier2-ondersteuning  
  * Beheerder met bevoorrechte rol   
  * Beveiligingsbeheerder  
  * SharePoint-beheerder  
  * Skype voor bedrijven-beheerder  
  * De beheerder van gebruiker  

Zie voor meer informatie over het gebruik van MFA met PIM [hoe MFA vereisen](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

