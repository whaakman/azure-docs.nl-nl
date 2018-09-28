---
title: Over v2.0 | Azure
description: Meer informatie over het v2.0-eindpunt en -platform.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: adcb1b15ed52e6954846ea09be0a87a118222c10
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989581"
---
# <a name="about-v20"></a>Over v2.0

Het v2.0-eindpunt en -platform verkeren in de preview-fase en worden voortdurend verbeterd. De JavaScript-SPA-scenario's (Single Page Application) zijn gereed en we nodigen u dan ook uit om MSAL.js te gebruiken voor het bouwen van browsertoepassingen en ons feedback te geven, zodat we de status kunnen wijzigen van preview in algemene beschikbaarheid (GA).

> [!NOTE]
> De functies voor MSAL Android, iOS en .NET zijn nog in ontwikkeling. U kunt ze gebruiken voor het bouwen van toepassingen en ons feedback geven.

De ontwikkelaarservaring van Azure Portal is aanzienlijk bijgewerkt en omvat nu al uw toepassingen die zijn gebouwd met ADAL of MSAL. Daarnaast is de bruikbaarheid verbeterd.

In het verleden moesten toepassingsontwikkelaars die zowel persoonlijke Microsoft-accounts als werkaccounts wilden ondersteunen vanuit Azure Active Directory (Azure AD) hun toepassingen integreren in twee afzonderlijke systemen. Het v2.0-eindpunt en -platform biedt een API-versie voor verificatie die dit proces vereenvoudigt. Deze maakt aanmelding vanaf beide typen accounts mogelijk met behulp van een eenmalige integratie. Toepassingen die het v2.0-eindpunt gebruiken, kunnen met een van beide accounttypen ook gebruikmaken van de REST API's van de [Microsoft Graph API](https://graph.microsoft.io).

## <a name="getting-started"></a>Aan de slag

Kies uw favoriete platform in de volgende lijst om een toepassing te bouwen met behulp van de open-source bibliotheken en frameworks van Microsoft. U kunt ook de protocollen OAuth 2.0 en OpenID Connect gebruiken om protocolberichten rechtstreeks te verzenden en te ontvangen, zonder een verificatiebibliotheek te gebruiken.

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>Meer informatie over het v2.0-eindpunt en -platform

Meer informatie over wat u met het Azure AD v2.0-eindpunt kunt doen:

* Ontdek de [typen toepassingen die u met het Azure AD v2.0-eindpunt kunt maken](v2-app-types.md).
* Inzicht in de [beperkingen](active-directory-v2-limitations.md) met het Azure AD v2.0-eindpunt.
* Bekijk deze video voor een overzicht van het Azure AD v2.0-eindpunt:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Aanvullende resources

Gedetailleerde informatie over v2.0 verkennen:

* [Naslaginformatie over v2.0-protocollen](active-directory-v2-protocols.md)
* [Naslaginformatie voor Access-tokens](access-tokens.md)
* [Naslaginformatie over `id_tokens`](id-tokens.md)
* [Naslaginformatie over v2.0-verificatiebibliotheken](reference-v2-libraries.md)
* [Bereiken en toestemming in v2.0](v2-permissions-and-consent.md)
* [De Microsoft Graph API](https://graph.microsoft.io)

> [!NOTE]
> Als u zich vanuit Azure Active Directory alleen hoeft aan te melden bij een werk- of schoolaccount, begin dan met de [Gids voor Azure AD-ontwikkelaars](azure-ad-developers-guide.md). Het v2.0-eindpunt is bedoeld voor gebruik door ontwikkelaars die expliciet persoonlijke Microsoft-accounts moeten kunnen aanmelden.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
