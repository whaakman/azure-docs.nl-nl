---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 0aadfd809fefa73af6869d496f6d88d6dc9df59e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269575"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Microsoft Graph API aanroepen vanuit een JavaScript-één pagina toepassing (SPA)

Deze handleiding wordt uitgelegd hoe een JavaScript één pagina toepassing (SPA) persoonlijk, werk kunt aanmelden en schoolaccounts, een toegangstoken ophalen en roept u de Microsoft Graph API of andere API's waarvoor toegangstokens van het Azure Active Directory-v2-eindpunt.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![De werking van de voorbeeld-app die is gegenereerd door deze handleiding](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

De voorbeeldtoepassing die is gemaakt door deze handleiding kunt een SPA JavaScript query uitvoeren op de Microsoft Graph API of een Web-API die tokens van Azure Active Directory-v2-eindpunt accepteert. Voor dit scenario, nadat een gebruiker zich aanmeldt, wordt een toegangstoken aangevraagd en toegevoegd aan de HTTP-aanvragen via de autorisatie-header. Token verkrijgen en verlenging worden afgehandeld door de Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheek:

|Bibliotheek|Beschrijving|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft-Verificatiebibliotheek voor JavaScript-Preview|

> [!NOTE]
> *msal.js* doelen de *Azure Active Directory-v2-eindpunt* -waardoor privé, school- als accounts aanmelden en tokens verkrijgen. De *Azure Active Directory-v2-eindpunt* heeft [enkele beperkingen](..\articles\active-directory\develop\active-directory-v2-limitations.md). Als u alleen geïnteresseerd in school- als accounts, gebruikt *adal.js* en de *V1 eindpunt*. Verschillen tussen de v1- en v2-eindpunten lezen leert de [v1-v2 vergelijking](..\articles\active-directory\develop\active-directory-v2-compare.md).

<!--end-collapse-->
