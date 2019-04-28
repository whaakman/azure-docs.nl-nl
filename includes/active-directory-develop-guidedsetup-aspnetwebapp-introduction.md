---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 86113246fab399e8364653198e9a6971317e3f2c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60298178"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aanmelding met Microsoft toevoegen aan een ASP.NET-web-app

Deze handleiding laat zien hoe u aanmelden met Microsoft met behulp van een ASP.NET MVC-oplossing met een traditionele web browser-gebaseerde toepassing met behulp van OpenID Connect implementeren.

Aan het einde van deze handleiding, is uw toepassing mogelijk om te accepteren van aanmeldingen van persoonlijke accounts (waaronder outlook.com, live.com en andere) evenals werk en schoolaccounts van een bedrijf of organisatie die is geïntegreerd met Azure Active Directory.

> Deze handleiding is vereist voor Visual Studio 2017.  Is dit niet het geval?  U kunt [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![Laat zien hoe de voorbeeld-app wordt gegenereerd door deze zelfstudie werkt](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

De voorbeeldtoepassing die is gemaakt in deze handleiding is gebaseerd op het scenario waarbij een gebruiker de browser gebruikt voor toegang tot een ASP.NET-website aanvragen van een gebruiker via een knop aanmelden wordt geverifieerd. In dit scenario vindt het grootste gedeelte van het werk om de webpagina weer te geven plaats aan de serverzijde.

## <a name="libraries"></a>Bibliotheken

Deze handleiding maakt gebruik van de volgende bibliotheken:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware die een toepassing in staat stelt om OpenIdConnect te gebruiken voor verificatie|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware die een toepassing in staat stelt om een gebruikerssessie te onderhouden met behulp van cookies|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Maakt het mogelijk om OWIN-toepassingen uit te voeren op IIS met behulp van de ASP.NET-aanvraagpijplijn|
