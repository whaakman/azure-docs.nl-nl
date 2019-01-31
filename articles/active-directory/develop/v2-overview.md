---
title: Over v2.0 | Azure
description: Meer informatie over het v2.0-eindpunt en -platform.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1f707351f5f5887155148b58d5ed145dde785b40
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55095409"
---
# <a name="about-v20"></a>Over v2.0

Het v2.0-eindpunt en -platform verkeren in de preview-fase en worden voortdurend verbeterd. De JavaScript-SPA-scenario's (Single Page Application) zijn gereed en we nodigen u dan ook uit om MSAL.js te gebruiken voor het bouwen van browsertoepassingen en ons feedback te geven, zodat we de status kunnen wijzigen van preview in algemene beschikbaarheid (GA).

> [!NOTE]
> De functies voor MSAL Android, iOS en .NET zijn nog in ontwikkeling. U kunt ze gebruiken voor het bouwen van toepassingen en ons feedback geven.

De [App-registraties (preview)](quickstart-register-app.md)-ervaring in de Azure-portal is aanzienlijk bijgewerkt en omvat nu al uw met ADAL of MSAL gebouwde toepassingen. Daarnaast is de bruikbaarheid verbeterd.

In het verleden moesten toepassingsontwikkelaars die zowel persoonlijke Microsoft-accounts als werkaccounts wilden ondersteunen vanuit Azure Active Directory (Azure AD) hun toepassingen integreren in twee afzonderlijke systemen. Het v2.0-eindpunt en -platform biedt een API-versie voor verificatie die dit proces vereenvoudigt. Deze maakt aanmelding vanaf beide typen accounts mogelijk met behulp van een eenmalige integratie. Toepassingen die het v2.0-eindpunt gebruiken, kunnen met een van beide accounttypen ook gebruikmaken van de REST API's van de [Microsoft Graph API](https://developer.microsoft.com/graph).

## <a name="getting-started"></a>Aan de slag

Kies uw favoriete platform in de volgende lijst om een toepassing te bouwen met behulp van de open-source bibliotheken en frameworks van Microsoft:

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>Meer informatie over het v2.0-eindpunt en -platform

Meer informatie over wat u met het Azure AD v2.0-eindpunt kunt doen:

* Ontdek de [typen toepassingen die u met het Azure AD v2.0-eindpunt kunt maken](v2-app-types.md).
* Inzicht in de [beperkingen](active-directory-v2-limitations.md) met het Azure AD v2.0-eindpunt.

## <a name="additional-resources"></a>Aanvullende resources

Gedetailleerde informatie over v2.0 verkennen:

* [Over het Microsoft Identity Platform](about-microsoft-identity-platform.md)
* [Naslaginformatie over v2.0-protocollen](active-directory-v2-protocols.md)
* [Naslaginformatie voor Access-tokens](access-tokens.md)
* [Naslaginformatie voor id-tokens](id-tokens.md)
* [Naslaginformatie over v2.0-verificatiebibliotheken](reference-v2-libraries.md)
* [Machtigingen en toestemming in v2.0](v2-permissions-and-consent.md)
* [Microsoft Graph API](https://developer.microsoft.com/graph)

> [!NOTE]
> Als u zich vanuit Azure Active Directory alleen hoeft aan te melden bij een werk- of schoolaccount, begin dan met de [Gids voor Azure AD-ontwikkelaars](v1-overview.md). Het v2.0-eindpunt is bedoeld voor gebruik door ontwikkelaars die expliciet persoonlijke Microsoft-accounts moeten kunnen aanmelden.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
