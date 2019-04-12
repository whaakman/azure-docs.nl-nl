---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8c2dc41fde9387da291b6e4a6c8a6220ae62b514
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502989"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Meld u aan gebruikers en de Microsoft Graph-API aanroepen vanuit een toepassing in JavaScript één pagina (SPA)

Deze handleiding laat zien hoe een toepassing in JavaScript één pagina (SPA) kan zich aanmelden persoonlijke, werk en schoolaccounts, een toegangstoken en aanroepen van de Microsoft Graph API of andere API's waarvoor toegangstokens van het eindpunt van Microsoft identity-platform.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![Laat zien hoe de voorbeeld-app wordt gegenereerd door deze zelfstudie werkt](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

De voorbeeldtoepassing die is gemaakt in deze handleiding kunt een JavaScript-SPA query uitvoeren op de Microsoft Graph API of een Web-API die tokens van Microsoft identity-platform-eindpunt accepteert. Voor dit scenario, nadat een gebruiker zich aanmeldt, is een toegangstoken aangevraagd en toegevoegd aan de HTTP-aanvragen via de autorisatie-header. Ophalen van tokens en vernieuwing worden afgehandeld door de Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

Deze handleiding worden de volgende bibliotheek gebruikt:

|Bibliotheek|Description|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft Authentication Library voor JavaScript-Preview|

> [!NOTE]
> *msal.js* doelen de *Microsoft identity platform endpoint* -waarmee privé, school- als accounts aanmelden en tokens verkrijgen. De *Microsoft identity platform endpoint* heeft [enkele beperkingen](../articles/active-directory/develop/active-directory-v2-limitations.md).
> Om te begrijpen van de verschillen tussen de v1.0 en v2.0-eindpunten lezen de [handleiding voor vergelijking van eindpunt](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
