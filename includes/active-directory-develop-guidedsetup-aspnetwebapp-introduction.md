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
ms.openlocfilehash: cf6604a0e22ca72c8aabd0603e42469cc71c9680
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app

Deze handleiding wordt uitgelegd hoe aanmelden bij Microsoft met behulp van een ASP.NET MVC-oplossing met een traditioneel browser gebaseerde webtoepassing met OpenID Connect implementeren. 

Aan het einde van deze handleiding, uw toepassing kan worden aanmelding modules van persoonlijke accounts (inclusief outlook.com, live.com en anderen), evenals werken accepteren en schoolaccounts zijn uit een bedrijf of organisatie die is geïntegreerd met Azure Active Directory. 

> Deze handleiding is Visual Studio 2015 Update 3 of Visual Studio 2017 vereist.  Hebt u geen deze?  [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![De werking van deze handleiding](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.png)

De voorbeeldtoepassing die is gemaakt door deze handleiding is gebaseerd op het scenario waarbij een gebruiker de browser gebruikt voor toegang tot een ASP.NET-website aanvragen van een gebruiker worden geverifieerd via een knop aanmelden. In dit scenario treedt de meeste werk voor het weergeven van de webpagina op aan de serverzijde.

## <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheken:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware waarmee een toepassing wilt OpenIdConnect gebruiken voor verificatie|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware waarmee een toepassing te onderhouden met behulp van cookies gebruikerssessie|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Kunnen worden uitgevoerd op IIS met de ASP.NET-aanvraag voor pipeline-toepassingen op basis van een OWIN|

