---
title: Extern delen van Office 365 en Azure Active Directory B2B-samenwerking | Microsoft Docs
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: aaf7d858b45ff6b5d10846b064f40bef08bfe5e5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Extern delen van Office 365 en Azure Active Directory B2B-samenwerking

Extern delen in Office 365 (OneDrive, SharePoint Online, uniforme, groepen, enzovoort) en Azure Active Directory (Azure AD) B2B-samenwerking zijn technisch hetzelfde. Alle extern delen (met uitzondering van OneDrive/SharePoint Online), inclusief gasten in Office 365-groepen, wordt de Azure AD B2B-samenwerking uitnodiging API's al gebruikt voor het delen.

OneDrive/SharePoint Online heeft een uitnodiging voor een afzonderlijke-manager. Ondersteuning voor extern delen in OneDrive/SharePoint Online gestart voordat de ondersteuning die Azure AD zijn ontwikkeld. Na verloop van tijd OneDrive/SharePoint Online extern delen, is enkele functies samengevoegd en veel miljoenen gebruikers die gebruikmaken van het product van de ingebouwde patroon delen. Er zijn echter enkele subtiele verschillen tussen hoe extern delen OneDrive/SharePoint Online werkt en de werking van Azure AD B2B-samenwerking:

- OneDrive/SharePoint Online gebruikers toegevoegd aan de map nadat gebruikers hebben hun uitnodigingen ingewisseld. Dus voordat inwisseling er geen de gebruiker in Azure AD-portal. Als een andere site in de tussentijd nodigt van een gebruiker, wordt een nieuwe uitnodiging gegenereerd. Wanneer u Azure AD B2B-samenwerking, worden gebruikers echter toegevoegd onmiddellijk op uitnodiging zodat ze overal worden weergegeven.

- De ervaring van de inwisselcode in OneDrive/SharePoint Online er anders uit de ervaring in Azure AD B2B-samenwerking. Nadat een gebruiker een uitnodiging wordt ingewisseld, worden de ervaringen lijken.

- Azure AD B2B-samenwerking uitgenodigd gebruikers kunnen worden verzameld van OneDrive/SharePoint Online-dialoogvensters voor delen. OneDrive/SharePoint Online uitgenodigd gebruikers ook weergegeven in Azure AD wanneer ze hun uitnodigingen inwisselen.

- Voor het beheren van extern delen in OneDrive/SharePoint Online met Azure AD B2B-samenwerking ingesteld OneDrive/SharePoint Online extern delen instelling **dat alleen delen met externe gebruikers al in de map**. Gebruikers kunnen gaat u naar extern gedeelde sites en kiest uit externe deelnemers die de beheerder heeft toegevoegd. De beheerder kan de externe deelnemers via de B2B-samenwerking uitnodiging API's kunt toevoegen.

![De OneDrive/SharePoint Online extern delen van de instelling](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Volgende stappen

Lees ook onze andere artikelen over Azure AD B2B-samenwerking:

* [Wat is Azure AD B2B-samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Gebruikerseigenschappen B2B-samenwerking](active-directory-b2b-user-properties.md)
* [Een B2B-samenwerking-gebruiker toe te voegen aan een rol](active-directory-b2b-add-guest-to-role.md)
* [B2B-samenwerking uitnodigingen delegeren](active-directory-b2b-delegate-invitations.md)
* [Dynamische groepen en B2B-samenwerking](active-directory-b2b-dynamic-groups.md)
* [B2B-samenwerking code en PowerShell-voorbeelden](active-directory-b2b-code-samples.md)
* [SaaS-apps voor B2B-samenwerking configureren](active-directory-b2b-configure-saas-apps.md)
* [B2B-samenwerking gebruikerstokens](active-directory-b2b-user-token.md)
* [Gebruikersclaims voor B2B-samenwerking toewijzing](active-directory-b2b-claims-mapping.md)
* [Huidige beperkingen voor B2B-samenwerking](active-directory-b2b-current-limitations.md)
