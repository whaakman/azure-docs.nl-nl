---
title: Bevoegde toegang beveiligen in Azure AD | Microsoft Docs
description: "Een onderwerp met uitleg over de strategieën voor het beveiligen van bevoegde toegang in Azure, Azure Active Directory en Microsoft Online Services."
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: e1bc0f27b14beef91b4deb68dc625d75195445fb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a>Bevoegde toegang beveiligen in Azure AD
Bevoegde toegang beveiligen is een belangrijke eerste stap ter bescherming van zakelijke activa in een moderne organisatie. Bevoegde accounts zijn accounts dat beheren en het beheer van IT-systemen. Cyberbeveiliging aanvallers gericht zijn op deze accounts toegang krijgen tot gegevens en systemen van een organisatie. Als u wilt beveiligen bevoorrechte toegang, moet u de accounts en systemen van het risico worden blootgesteld aan een kwaadwillende gebruiker isoleren.

Meer gebruikers zijn bevoorrechte om toegang te krijgen via cloudservices gestart. Dit kunnen bijvoorbeeld globale beheerders van Office365, Azure-abonnementbeheerders en gebruikers die beheerderstoegang op de SaaS-apps of virtuele machines hebben.

Microsoft raadt u Volg deze roadmap op [bevoegde toegang beveiligen](https://technet.microsoft.com/library/mt631194.aspx).

Deze principes toepassen voor klanten met Azure Active Directory, Office 365 of andere Microsoft-services en toepassingen, of gebruikersaccounts worden beheerd en geverifieerd door Active Directory of in Azure Active Directory. De volgende secties bevatten meer informatie over Azure AD-functies voor de ondersteuning van bevoegde toegang beveiligen.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication
Voor een verhoging van de beveiliging van de verificatie door de beheerder moet u de verificatie in twee stappen voordat u verleent rechten vereisen. Verificatie in twee stappen is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede beveiligingslaag op gebruikersaanmeldingen en transacties.

Azure multi-factor Authentication (MFA) is oplossing van Microsoft in twee stappen verificatie, welke helpt beveiliging toegang tot gegevens en toepassingen en te voldoen aan de gebruiker vragen voor een eenvoudig proces aanmelden. Sterke verificatie via een bereik van eenvoudige verificatie-opties zoals levert:

- telefoongesprekken
- SMS-berichten
- mobiele app-meldingen
- mobiele app verificatiecodes
- OATH-tokens van derde partij

Zie de volgende video voor een overzicht van de werking van Azure multi-factor Authentication:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

Zie voor meer informatie [MFA voor Office 365 en MFA voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Tijdsgebonden bevoegdheden
Sommige organisaties kunnen het gebeuren dat ze te veel gebruikers in maximaal bevoorrechte rollen hebben. Een gebruiker kan zijn toegevoegd aan de rol voor een bepaalde activiteit, zoals aanmelden voor een service, maar deze rechten niet vaak later gebruiken.

Om te verlagen van de blootstellingstijd van bevoegdheden en vergroot de zichtbaarheid van het gebruik ervan, gebruikers beperken tot alleen nemen over hun bevoegdheden 'just in time' (Just in time), of deze rollen toewijzen voor een kortere duur met vertrouwen, de machtigingen automatisch worden ingetrokken. Voor Azure Active Directory, Azure-Resources (Preview) en Microsoft Online Services, kunt u [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).

![PIM-dashboard][2]

## <a name="attack-detection"></a>Aanvalsdetectie
[Azure Active Directory: Identity Protection](../active-directory-identityprotection.md) biedt een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Op basis van de risico's, berekent Identity Protection het risiconiveau van een gebruiker voor elke gebruiker, zodat u kunt beleid op basis van een risico voor het automatisch beveiligen van de identiteit van uw organisatie configureren. Dit beleid, samen met andere besturingselementen voorwaardelijke toegang is verstrekt door Azure Active Directory en EMS, kunnen automatisch worden voorkomen dat de gebruiker of suggesties met wachtwoord opnieuw instellen en afdwingen van multi-factor authentication-server.

![Azure AD-identiteitsbeveiliging][3]

## <a name="conditional-access"></a>Voorwaardelijke toegang
Met voorwaardelijk toegangsbeheer controleert de Azure Active Directory de specifieke voorwaarden die u bij het verifiëren van een gebruiker, alvorens deze toegang tot een toepassing kiezen. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang te krijgen tot de toepassing.

## <a name="related-articles"></a>Verwante artikelen:
* Schakel [Azure multi-factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Schakel [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Schakel [Azure AD Identity Protection](../active-directory-identityprotection.md)
* Schakel [voorwaardelijk toegangsbeheer](../active-directory-conditional-access-azure-portal.md)

Voor meer informatie over het bouwen van een volledige beveiliging roadmap, Zie de sectie 'verantwoordelijkheden van de klant en roadmap' van de [Microsoft Cloud Security voor Enterprise-architecten](http://aka.ms/securecustomer) document. Voor meer informatie over Microsoft-services om te helpen bij deze onderwerpen bezighouden, neem contact op met uw Microsoft-vertegenwoordiger of gaat u naar onze [Cybersecurity oplossingen pagina](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
