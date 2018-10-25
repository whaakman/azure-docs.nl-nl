---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8d0b0c99b743461da41a518feae9c781f90c8b63
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988417"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>De Microsoft Graph-API aanroepen vanuit een toepassing in JavaScript één pagina (SPA)

Deze handleiding laat zien hoe een toepassing in JavaScript één pagina (SPA) kan zich in een persoonlijke, werk en schoolaccounts, een toegangstoken en aanroepen van de Microsoft Graph API of andere API's die tokens voor toegang vanuit de Azure Active Directory v2.0-eindpunt vereist.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![De werking van de voorbeeld-app die is gegenereerd door deze handleiding](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

De voorbeeldtoepassing die is gemaakt in deze handleiding kunt een JavaScript-SPA query uitvoeren op de Microsoft Graph API of een Web-API die tokens van Azure Active Directory v2.0-eindpunt accepteert. Voor dit scenario, nadat een gebruiker zich aanmeldt, is een toegangstoken aangevraagd en toegevoegd aan de HTTP-aanvragen via de autorisatie-header. Ophalen van tokens en vernieuwing worden afgehandeld door de Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

Deze handleiding worden de volgende bibliotheek gebruikt:

|Bibliotheek|Beschrijving|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft Authentication Library voor JavaScript-Preview|

> [!NOTE]
> *msal.js* doelen de *Azure Active Directory v2.0-eindpunt* -waarmee privé, school- als accounts aanmelden en tokens verkrijgen. De *Azure Active Directory v2.0-eindpunt* heeft [enkele beperkingen](..\articles\active-directory\develop\active-directory-v2-limitations.md).
> Om te begrijpen van de verschillen tussen de v1.0 en v2.0-eindpunten lezen de [handleiding voor vergelijking van eindpunt](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
