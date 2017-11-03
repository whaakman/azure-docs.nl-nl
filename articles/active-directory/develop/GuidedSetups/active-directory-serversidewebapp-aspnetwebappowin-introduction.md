---
title: Azure AD v2 ASP.NET Web Server aan de slag - Inleiding | Microsoft Docs
description: Implementeren van Microsoft-aanmeldingspagina op een ASP.NET-oplossing met een traditioneel browser gebaseerde webtoepassing met behulp van standaard OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 8062923b6270ec6253dc231a3db4333cf4666b42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app

Deze handleiding wordt uitgelegd hoe aanmelden bij Microsoft met behulp van een ASP.NET MVC-oplossing met een traditioneel browser gebaseerde webtoepassing met OpenID Connect implementeren. 

Aan het einde van deze handleiding, uw toepassing kan worden aanmelding modules van persoonlijke accounts (inclusief outlook.com, live.com en anderen), evenals werken accepteren en schoolaccounts zijn uit een bedrijf of organisatie die is geïntegreerd met Azure Active Directory. 

> Deze handleiding is Visual Studio 2015 Update 3 of Visual Studio 2017 vereist.  Hebt u geen deze?  [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Deze handleiding is gebaseerd op het scenario waarbij een browser toegang heeft tot een ASP.NET-website aanvragen van een gebruiker worden geverifieerd via een knop aanmelden. In dit scenario treedt de meeste werk voor het weergeven van de webpagina op aan de serverzijde.

## <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheken:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware waarmee een toepassing wilt OpenIdConnect gebruiken voor verificatie|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware waarmee een toepassing te onderhouden met behulp van cookies gebruikerssessie|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Kunnen worden uitgevoerd op IIS met de ASP.NET-aanvraag voor pipeline-toepassingen op basis van een OWIN|

