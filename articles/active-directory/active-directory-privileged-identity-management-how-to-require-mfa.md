---
title: Meervoudige verificatie vereisen | Microsoft Docs
description: Ontdek hoe meervoudige authenticatie (MFA) voor bevoegde identiteiten met de extensie Azure Active Directory Privileged Identity Management.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 1e3dc4ad-3a6a-4a52-8417-3ca4f84ae05c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: faee62bdaca3f80fdd8f6be8aaf28c881314333a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>MFA in Azure AD Privileged Identity Management vereisen
Het is raadzaam dat u multi-factor authentication (MFA) voor al uw beheerders vereist. Dit vermindert het risico van een aanval als gevolg van een wachtwoord waarmee is geknoeid.

U kunt vereisen dat gebruikers een challenge MFA voltooid wanneer ze zich aanmelden. Het blogbericht [MFA voor Office 365 en MFA voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) vergelijkt wat is opgenomen in de Office- en Azure-abonnementen, met de functies die zijn opgenomen in de Microsoft Azure multi-factor Authentication-aanbieding.

U kunt ook vereisen dat gebruikers een challenge MFA voltooien bij het activeren van een rol in Azure AD PIM. Op deze manier als de gebruiker kan een challenge MFA niet hebt voltooid wanneer ze zich aangemeld, wordt ze gevraagd om dit te doen door PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Het vereisen van MFA in Azure AD Privileged Identity Management
Bij het beheren van identiteiten in PIM als een beheerder met bevoorrechte rol, ziet u waarschuwingen die u kunt het beste MFA voor bevoegde accounts. Klik op de beveiligingswaarschuwing in het dashboard PIM en een nieuwe blade geopend met een lijst met de administratoraccounts die MFA te vereisen.  U kunt MFA vereisen door meerdere rollen selecteren en vervolgens te klikken op de **los** knop, of u kunt op de weglatingstekens naast afzonderlijke rollen en klik op de **los** knop.

> [!IMPORTANT]
> Rechtermuisknop nu werkt alleen voor Azure MFA met werk of schoolaccounts, geen Microsoft-accounts (meestal een persoonlijk account die wordt gebruikt om aan te melden bij Microsoft-services zoals Skype, Xbox, Outlook.com, enz.). Daarom kan iedereen met een Microsoft-account een in aanmerking komende beheerder niet omdat ze MFA niet gebruiken voor het activeren van hun rollen. Als deze gebruikers om door te gaan met het beheren van werkbelastingen met een Microsoft-account nodig hebt, worden de bevoegdheden ze voor permanente beheerders nu.
> 
> 

Bovendien kunt u de MFA-vereiste voor een specifieke rol door erop te klikken in het gedeelte van de functies van de PIM-dashboard. Klik vervolgens op **instellingen** in de rolblade en klikt u vervolgens selecteren **inschakelen** onder multi-factor authentication-server.

## <a name="how-azure-ad-pim-validates-mfa"></a>Hoe Azure AD PIM wordt gevalideerd met MFA
Er zijn twee opties voor het valideren van MFA wanneer een gebruiker een rol activeren.

De eenvoudigste optie is te vertrouwen op de Azure MFA voor gebruikers die een bevoorrechte rol wilt activeren. U doet dit door Controleer eerst of die gebruikers in licentie gegeven zijn, indien nodig, en hebt geregistreerd voor Azure MFA. Meer informatie over hoe u dit doet zich in [aan de slag met Azure multi-factor Authentication in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Het wordt aanbevolen, maar niet vereist, dat u Azure AD MFA afdwingen voor deze gebruikers wanneer ze zich aanmelden als u wilt configureren. Dit is omdat de MFA-controles worden uitgevoerd per Azure AD PIM zelf.

Als gebruikers lokale verificatie kunt u ook hebt id-provider is verantwoordelijk voor MFA. Bijvoorbeeld, als u AD Federation Services om te vereisen verificatie op basis van een smartcard voor de toegang tot Azure AD hebt geconfigureerd [cloudresources beveiligen met Azure multi-factor Authentication en AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) bevat instructies voor het configureren van AD FS verzendt u claims die naar Azure AD. Wanneer een gebruiker probeert een rol wilt activeren, accepteert Azure AD PIM dat MFA is al gevalideerd voor de gebruiker wanneer deze de juiste claims ontvangt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

