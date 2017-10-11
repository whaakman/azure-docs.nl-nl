---
title: 'Beveiligingsproblemen die worden gedetecteerd door Azure Active Directory: Identity Protection | Microsoft Docs'
description: 'Overzicht van de beveiligingsproblemen die worden gedetecteerd door Azure Active Directory: Identity Protection.'
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 364873ff54099a6123e40b12e819d1745751f285
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Beveiligingsproblemen die worden gedetecteerd door Azure Active Directory: Identity Protection
Zwakke plekken zijn zwakke punten in uw omgeving die door een aanvaller kan worden misbruikt. U wordt aangeraden dat u deze beveiligingslekken ter verbetering van de beveiligingsstatus van uw organisatie, en voorkomen dat aanvallers deze nu aanvalt.


![beveiligingsproblemen](./media/active-directory-identityprotection-vulnerabilities/101.png "beveiligingsproblemen")



De volgende secties bieden u een overzicht van de kwetsbaarheden die zijn gerapporteerd door Identity Protection.

## <a name="multi-factor-authentication-registration-not-configured"></a>Multi-factor authentication-registratie niet geconfigureerd
Deze kwetsbaarheid helpt u bij de implementatie van Azure multi-factor Authentication in uw organisatie beheren. 

Azure multi-factor authentication-Server biedt een tweede beveiligingslaag voor verificatie van gebruiker. Deze u toegang tot gegevens en toepassingen beveiligt en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Sterke verificatie via een bereik van eenvoudige verificatie-opties levert: telefoonoproep, tekstbericht of mobiele app melding of verificatie van code en 3rd party OATH-tokens.

Het is raadzaam dat u Azure multi-factor Authentication voor gebruikersaanmelding nodig hebt. Multi-factorauthenticatie speelt een belangrijke rol in de beleidsregels voor voorwaardelijke toegang op basis van risico's beschikbaar via Identity Protection.

Zie voor meer informatie [wat is Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Niet-beheerde cloud-apps
Deze kwetsbaarheid helpt u identificeren onbeheerde cloud-apps in uw organisatie.

In moderne ondernemingen zijn IT-afdelingen vaak niet op de hoogte van alle cloud-toepassingen die van gebruikers in hun organisatie gebruikmaken voor hun werk. Het is gemakkelijk om te zien waarom beheerders zou twijfels hebt over niet-geautoriseerde toegang tot zakelijke gegevens, mogelijk gegevenslekken en andere beveiligingsrisico's. 

We raden aan uw organisatie te implementeren Cloud App Discovery voor het detecteren van niet-beheerde cloud-toepassingen en voor het beheren van deze toepassingen met Azure Active Directory.

Zie voor meer informatie [zoeken naar niet-beheerde cloud-toepassingen met Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

## <a name="security-alerts-from-privileged-identity-management"></a>Beveiligingswaarschuwingen van Privileged Identity Management
Deze kwetsbaarheid helpt u bij het opsporen en oplossen van waarschuwingen over bevoegde identiteiten in uw organisatie.  

Als u wilt dat gebruikers kunnen bevoorrechte bewerkingen uitvoeren, moeten organisaties gebruikers tijdelijke of permanente bevoegde om toegang te verlenen in Azure AD, Azure of Office 365-bronnen of andere SaaS-apps. Elk van deze bevoegde gebruikers verhoogt de kwetsbaarheid van uw organisatie. Deze kwetsbaarheid kunt u gebruikers identificeren met onnodige bevoegdheden gebruiken en onderneem gepaste actie te verlagen of elimineren het risico dat ze vormen. 

Het is raadzaam om uw organisatie gebruikmaakt van Azure AD Privileged Identity Management te beheren, beheren en bewaken bevoegde identiteiten en toegang krijgen tot bronnen in Azure AD, evenals andere Microsoft online services zoals Office 365 of Microsoft Intune.

Zie voor meer informatie [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

