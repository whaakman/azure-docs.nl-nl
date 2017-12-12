---
title: B2B-samenwerking gebruikersclaims toewijzen in Azure Active Directory | Microsoft Docs
description: claims toewijzing verwijzing voor Azure Active Directory B2B-samenwerking
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 439219087d0a5027c729e9d0e0ecb90bd2a9de3c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>B2B-samenwerking gebruikersclaims toewijzen in Azure Active Directory

Azure Active Directory (Azure AD) ondersteunt het aanpassen van de uitgegeven claims in het SAML-token voor B2B-samenwerking gebruikers. Wanneer een gebruiker zich bij de toepassing verifieert, verstrekt Azure AD een SAML-token aan de app die informatie (of claims) bevat over de gebruiker die uniek wordt geïdentificeerd. Dit omvat standaard gebruikersnaam, e-mailadres, de voornaam en achternaam van de gebruiker. U kunt weergeven of bewerken van de claims in het SAML-token naar de toepassing op het tabblad kenmerken verzonden.

Er zijn twee mogelijke redenen waarom moet u mogelijk de uitgegeven claims in het SAML-token bewerken.

1. De toepassing is geschreven naar een andere set claimregels URI's is vereist of claimwaarden

2. Uw toepassing vereist dat de NameIdentifier claim iets anders dan de user principal name opgeslagen in Azure Active Directory.

  ![weergave claims in SAML-token](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Bekijk voor meer informatie over het toevoegen en bewerken van claims, in dit artikel op claims aanpassing, [uitgegeven claims in het SAML-token voor vooraf geïntegreerde apps in Azure Active Directory aanpassen](develop/active-directory-saml-claims-customization.md). Voor B2B-samenwerking, gebruikers, toewijzing NameID en UPN cross-tenant worden uit veiligheidsoverwegingen voorkomen.


## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [B2bB samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)
* [Dynamische groepen en B2B-samenwerking](active-directory-b2b-dynamic-groups.md)
* [B2B-samenwerking code en PowerShell-voorbeelden](active-directory-b2b-code-samples.md)
* [SaaS-apps voor B2B-samenwerking configureren](active-directory-b2b-configure-saas-apps.md)
* [Office 365 extern delen](active-directory-b2b-o365-external-user.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
