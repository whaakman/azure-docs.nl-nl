---
title: Azure Active Directory v2.0-eindpunt | Microsoft Docs
description: Een inleiding tot het bouwen van toepassingen met aanmelding zowel Microsoft-Account en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: hirsin, jmprieur, elisol, dastrock
ms.custom: aaddev
ms.openlocfilehash: 3d71e889fccaa6de59d9a528754a5e11d0220bef
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577132"
---
# <a name="sign-in-microsoft-account-and-azure-active-directory-users-in-a-single-application"></a>Meld u aan Microsoft-Account en Azure Active Directory gebruikers in een enkele toepassing
In het verleden moesten ontwikkelaars van toepassingen die ondersteuning voor zowel persoonlijke Microsoft-accounts en -werkaccounts uit Azure Active Directory wilt integreren in twee afzonderlijke systemen. Het v2.0-eindpunt voor Azure Active Directory (Azure AD) introduceert een nieuwe verificatie-API-versie die vereenvoudigt u dit proces. De Azure AD v2.0-eindpunt kunt aanmelding vanaf beide typen accounts met behulp van een eenmalige integratie. Toepassingen die gebruikmaken van de Azure AD v2.0-eindpunt kunnen ook gebruiken voor de REST-API's van de [Microsoft Graph API](https://graph.microsoft.io) met behulp van een type account.

## <a name="getting-started"></a>Aan de slag
Kies uw favoriete platform in de volgende lijst een toepassing bouwt met behulp van de Microsoft open source-bibliotheken en frameworks. U kunt ook de OAuth 2.0 en OpenID Connect-protocollen gebruiken voor het verzenden en ontvangen protocolberichten rechtstreeks zonder met behulp van een verificatiebibliotheek.
<br />

[!INCLUDE [Azure AD v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-azure-ad-v20-endpoint"></a>Meer informatie over de Azure AD v2.0-eindpunt
Meer informatie over wat u met de Azure AD v2.0-eindpunt doen kunt:

* Ontdek de [typen toepassingen die u met de Azure AD v2.0-eindpunt maken kunt](active-directory-v2-flows.md).
* Inzicht in de [beperkingen, beperkingen en beperkingen van](active-directory-v2-limitations.md) met de Azure AD v2.0-eindpunt.
* Bekijk deze video voor een overzicht van de Azure AD v2.0-eindpunt:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Aanvullende resources
Gedetailleerde informatie over het platform van Azure AD v2.0-eindpunt verkennen:

* [Azure AD v2.0-protocollen verwijzen naar](active-directory-v2-protocols.md)
* [Azure AD v2.0 tokens verwijzing](v2-id-and-access-tokens.md)
* [Azure AD v2.0-verificatiebibliotheken verwijzen naar](active-directory-v2-libraries.md)
* [Bereiken en toestemming in de Azure AD v2.0-eindpunt](v2-permissions-and-consent.md)
* [De Microsoft Graph API](https://graph.microsoft.io)

> [!NOTE]
> Als u alleen hoeft aan te melden bij werk- en schoolaccounts accounts van Azure Active Directory, beginnen met de [Ontwikkelaarshandleiding voor Azure Active Directory](azure-ad-developers-guide.md). De Azure AD v2.0-eindpunt is bedoeld voor gebruik door ontwikkelaars die expliciet moeten aanmelden met persoonlijke Microsoft-accounts.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
