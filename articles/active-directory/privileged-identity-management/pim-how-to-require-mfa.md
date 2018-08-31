---
title: Meervoudige verificatie vereisen voor Azure AD directory-rollen in PIM | Microsoft Docs
description: Informatie over het vereisen van multi-factor authentication (MFA) voor Azure AD-directory-rollen in Azure AD Privileged Identity Management (PIM).
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
ms.openlocfilehash: 9777a7d2ed07ea2edf4bea661a5065c808de9f2b
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188350"
---
# <a name="require-multi-factor-authentication-for-azure-ad-directory-roles-in-pim"></a>Meervoudige verificatie vereisen voor Azure AD directory-rollen in PIM
Het is raadzaam dat u multi-factor authentication (MFA) voor al uw beheerders vereisen. Dit vermindert het risico van een aanval vanwege een wachtwoord is verdacht.

U kunt vereisen dat gebruikers een MFA-controle voltooien wanneer ze zich aanmelden. Het blogbericht: [MFA voor Office 365 en MFA voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) vergeleken wat is opgenomen in Office en Azure-abonnementen, met de functies die deel uitmaken van de aanbieding voor Microsoft Azure multi-factor Authentication.

U kunt ook vereisen dat gebruikers een MFA-controle voltooien wanneer ze een rol in Azure AD PIM kunt activeren. Op deze manier als de gebruiker is niet een MFA-controle uitvoeren wanneer zij is aangemeld, wordt ze gevraagd om dit te doen door PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Vereisen dat MFA in Azure AD Privileged Identity Management
Bij het beheren van identiteiten in PIM als een beheerder met bevoorrechte rol, ziet u mogelijk waarschuwingen dat MFA voor bevoegde accounts aanbeveelt. Klik op de beveiligingswaarschuwing in het dashboard PIM en er wordt een nieuwe blade geopend met een lijst van de administrator-accounts waarvoor MFA moet.  U kunt MFA vereisen door meerdere rollen selecteren en vervolgens te klikken op de **oplossen** knop, of u kunt op het beletselteken naast de afzonderlijke rollen en klik op de **oplossen** knop.

> [!IMPORTANT]
> Rechts nu werkt alleen voor Azure MFA met werk- of schoolaccounts, geen Microsoft-accounts (meestal een persoonlijk account dat wordt gebruikt voor het aanmelden bij Microsoft-services zoals Skype, Xbox, Outlook.com, enz.). Als gevolg hiervan kan iedereen met een Microsoft-account een in aanmerking komende beheerder niet omdat ze niet MFA gebruiken om te activeren van hun rollen. Als deze gebruikers gaan met het beheren van werkbelastingen met behulp van een Microsoft-account wilt, uitbreiden u ze naar de permanente beheerders nu.
> 
> 

Bovendien kunt u de MFA-vereiste voor een specifieke rol door erop te klikken in de sectie rollen van de PIM-dashboard. Klik vervolgens op **instellingen** in de rolblade en vervolgens **inschakelen** onder multi-factor authentication.

## <a name="how-azure-ad-pim-validates-mfa"></a>Hoe Azure AD PIM wordt gevalideerd met MFA
Er zijn twee opties voor het valideren van MFA wanneer een gebruiker een rol activeert.

De eenvoudigste optie is om zijn afhankelijk van de Azure MFA voor gebruikers die een bevoorrechte rol wilt activeren. U doet dit door Controleer eerst of die gebruikers zijn gelicentieerd, indien nodig, en voor Azure MFA hebt geregistreerd. Meer informatie over hoe u dit doet, wordt [aan de slag met Azure multi-factor Authentication in de cloud](../authentication/howto-mfa-getstarted.md). Het wordt aanbevolen, maar niet vereist dat u Azure AD configureren voor meervoudige verificatie afdwingen voor deze gebruikers wanneer ze zich aanmelden. Dit is omdat de MFA-controles zullen worden gemaakt door Azure AD PIM zelf.

Als gebruikers on-premises verificatie kunt u ook uw id-provider die verantwoordelijk zijn voor MFA hebt. Bijvoorbeeld, als u AD Federation Services om te vereisen dat verificatie op basis van een smartcard voordat u toegang tot Azure AD hebt geconfigureerd [cloudresources beveiligen met Azure multi-factor Authentication en AD FS](../authentication/howto-mfa-adfs.md) bevat instructies voor het configureren van AD FS voor het verzenden van claims voor Azure AD. Wanneer een gebruiker probeert om een rol te activeren, moet u Azure AD PIM accepteert dat MFA is al gevalideerd voor de gebruiker zodra deze de juiste claims ontvangt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Azure AD directory rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
- [Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren](pim-how-to-configure-security-alerts.md)
