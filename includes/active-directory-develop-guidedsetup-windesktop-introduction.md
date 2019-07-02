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
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 301abe95b245603e5414eef84ce74cdc8de01d19
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509840"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>De Microsoft Graph-API aanroepen vanuit een Windows-bureaublad-app

Deze handleiding laat zien hoe een systeemeigen Windows Desktop .NET (XAML)-toepassing maakt gebruik van een toegangstoken voor het aanroepen van de Microsoft Graph API. De app ook toegang tot andere API's waarvoor toegangstokens van een Microsoft identity-platform voor ontwikkelaars v2.0-eindpunt. Dit platform heette voorheen Azure AD.

Wanneer u de handleiding hebt voltooid, zich uw toepassing een beveiligde API die gebruikmaakt van persoonlijke accounts (waaronder outlook.com, live.com en anderen) aan te roepen. De toepassing wordt ook werk en schoolaccounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.  

> [!NOTE]
> De handleiding is vereist voor Visual Studio 2015 Update 3, Visual Studio 2017 of Visual Studio 2019. Hebt u geen van deze versies? [Download gratis Visual Studio 2019](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze zelfstudie werkt](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

De voorbeeldtoepassing die u met deze handleiding maakt kunt een Windows-Desktop-toepassing die query's van de Microsoft Graph API of een Web-API die tokens van een eindpunt van Microsoft identity-platform accepteert. Voor dit scenario kunt u een token op HTTP-aanvragen via de autorisatie-header toevoegen. Microsoft Authentication Library (MSAL) wordt gebruikt voor ophalen van tokens en verlenging.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Afhandeling van ophalen van tokens voor toegang tot beveiligde Web-API 's

Nadat de gebruiker is geverifieerd, ontvangt de voorbeeld-App een token dat u gebruiken kunt om op te vragen van Microsoft Graph API of een Web-API die wordt beveiligd door Microsoft identity-platform voor ontwikkelaars.

API's, zoals Microsoft Graph vereisen een token voor het toestaan van toegang tot bepaalde resources. Bijvoorbeeld, is een token vereist voor het lezen van het profiel van een gebruiker, toegang tot de agenda van een gebruiker of het e-mailbericht verzenden. Uw toepassing kan een toegangstoken aanvragen met behulp van MSAL voor toegang tot deze resources door API-bereiken op te geven. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke aanroep die op basis van de beveiligde bron plaatsvindt.

MSAL beheert opslaan in cache en het vernieuwen van de toegangstokens voor u, zodat uw toepassing niet te hoeft.

## <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding maakt gebruik van de volgende NuGet-pakketten:

|Bibliotheek|Description|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL.NET)|
