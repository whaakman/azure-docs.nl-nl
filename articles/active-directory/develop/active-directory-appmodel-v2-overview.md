---
title: Azure Active Directory v2.0-eindpunt | Microsoft Docs
description: Een inleiding tot het bouwen van apps met zowel Microsoft-Account en Azure Active Directory aanmelden.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1e286044fb1a1b367fcac2dc14c47f68d5ed120d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Meld u aan Microsoft-Account en Azure AD-gebruikers in een enkele app
In het verleden is een app-ontwikkelaar die ondersteuning bieden voor beide persoonlijke Microsoft-accounts en -werkaccounts van Azure Active Directory wilt integreren met twee afzonderlijke systemen vereist.  De **Azure AD v2.0-eindpunt** introduceert een nieuwe authenticatie-API-versie waarmee u zich kunt aanmelden beide typen accounts met een eenvoudige integratie.  Apps die gebruikmaken van het v2.0-eindpunt kunnen ook gebruiken voor REST-API's van de [Microsoft Graph](https://graph.microsoft.io) met behulp van een type account.

## <a name="getting-started"></a>Aan de slag
Kies uw favoriete platform in de volgende lijst om een app met behulp van onze open-source bibliotheken en frameworks te bouwen.  U kunt ook kunt u onze documentatie OAuth 2.0 & OpenID Connect protocol verzend en ontvang protocolberichten rechtstreeks zonder met behulp van een auth-bibliotheek.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Wat is er nieuw
De informatie hier erg nuttig zijn bij het begrijpen van wat is & Wat is niet mogelijk met het v2.0-eindpunt.

* Meer informatie over de [typen apps kunt u met het v2.0-eindpunt](active-directory-v2-flows.md).
* Inzicht in de [beperkingen, beperkingen en beperkingen](active-directory-v2-limitations.md) met het v2.0-eindpunt.
* Bekijk deze video voor het v2.0-eindpunt overzicht:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Naslaginformatie
Deze koppelingen zijn handig voor het platform in de diepte verkennen:

* [naslaginformatie over v2.0-Protocol](active-directory-v2-protocols.md)
* [v2.0 tokenverwijzing](active-directory-v2-tokens.md)
* [naslaginformatie over v2.0-bibliotheek](active-directory-v2-libraries.md)
* [Scopes en toestemming in het v2.0-eindpunt](active-directory-v2-scopes.md)
* [Microsoft Graph](https://graph.microsoft.io)

## <a name="help--support"></a>Help en ondersteuning
Dit zijn de aanbevolen plaatsen voor hulp bij het ontwikkelen met Azure Active Directory.

* [Stack Overflow's tags voor `azure-active-directory` en `adal`](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [Feedback op Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> Als u alleen hoeft aan te melden werk- en schoolaccounts accounts van Azure Active Directory, moet u beginnen met onze [ontwikkelaarshandleiding Azure AD](active-directory-developers-guide.md).  Het v2.0-eindpunt is bedoeld voor gebruik door ontwikkelaars die expliciet hoeft aan te melden in persoonlijke Microsoft-accounts.

