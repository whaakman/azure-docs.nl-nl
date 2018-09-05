---
title: Multi-factor authentication (MFA) en PIM - Azure | Microsoft Docs
description: Meer informatie over hoe multi-factor authentication (MFA) in Azure AD Privileged Identity Management (PIM) wordt gevalideerd.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: cfa092e8aebe92192ecca8aec2721282e603cc5b
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669271"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Multi-factor authentication (MFA) en PIM

Het is raadzaam dat u multi-factor authentication (MFA) voor alle uw beheerders vereist. Dit vermindert het risico van een aanval vanwege een wachtwoord is verdacht.

U kunt vereisen dat gebruikers een MFA-controle voltooien wanneer ze zich aanmelden. U kunt ook vereisen dat gebruikers een MFA-controle voltooien wanneer ze een rol in Azure AD Privileged Identity Management (PIM) activeert. Op deze manier als de gebruiker is niet een MFA-controle uitvoeren wanneer zij is aangemeld, wordt ze gevraagd om dit te doen door PIM.

> [!IMPORTANT]
> Rechts nu werkt alleen voor Azure MFA met werk- of schoolaccounts, geen Microsoft-accounts (meestal een persoonlijk account dat wordt gebruikt voor het aanmelden bij Microsoft-services zoals Skype, Xbox, Outlook.com, enz.). Als gevolg hiervan kan iedereen met een Microsoft-account een in aanmerking komende beheerder niet omdat ze niet MFA gebruiken om te activeren van hun rollen. Als deze gebruikers gaan met het beheren van werkbelastingen met behulp van een Microsoft-account wilt, uitbreiden u ze naar de permanente beheerders nu.

## <a name="how-pim-validates-mfa"></a>Hoe PIM wordt gevalideerd met MFA

Er zijn twee opties voor het valideren van MFA wanneer een gebruiker een rol activeert.

De eenvoudigste optie is om zijn afhankelijk van de Azure MFA voor gebruikers die een bevoorrechte rol wilt activeren. U doet dit door Controleer eerst of die gebruikers zijn gelicentieerd, indien nodig, en voor Azure MFA hebt geregistreerd. Zie voor meer informatie over het implementeren van Azure MFA [implementeren van cloud-gebaseerde Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md). Het wordt aanbevolen, maar niet vereist dat u Azure AD configureren voor meervoudige verificatie afdwingen voor deze gebruikers wanneer ze zich aanmelden. Dit is omdat de MFA-controle wordt gesteld door PIM zelf.

Als gebruikers on-premises verificatie kunt u ook uw id-provider die verantwoordelijk zijn voor MFA hebt. Bijvoorbeeld, als u AD Federation Services om te vereisen dat verificatie op basis van een smartcard voordat u toegang tot Azure AD hebt geconfigureerd [cloudresources beveiligen met Azure multi-factor Authentication en AD FS](../authentication/howto-mfa-adfs.md) bevat instructies voor het configureren van AD FS voor het verzenden van claims voor Azure AD. Wanneer een gebruiker probeert om een rol te activeren, moet u PIM accepteert dat MFA is al gevalideerd voor de gebruiker zodra deze de juiste claims ontvangt.

## <a name="next-steps"></a>Volgende stappen

- [Azure AD directory rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
