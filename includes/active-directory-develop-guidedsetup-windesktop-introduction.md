---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 9f25734259ce34caa0f0d7e844fc985f953b2795
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842888"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>De Microsoft Graph-API aanroepen vanuit een Windows-bureaublad-app

Deze handleiding laat zien hoe een systeemeigen Windows Desktop .NET (XAML)-toepassing kan een toegangstoken en de Microsoft Graph API of andere API's waarvoor toegangstokens van een Azure Active Directory v2-eindpunt aanroepen.

Wanneer u de handleiding hebt voltooid, zich uw toepassing een beveiligde API die gebruikmaakt van persoonlijke accounts (waaronder outlook.com, live.com en anderen) aan te roepen. De toepassing wordt ook werk en schoolaccounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.  

> [!NOTE] 
> De handleiding is vereist voor Visual Studio 2015 Update 3 of Visual Studio 2017.  Hebt u niet een van deze versies? [Download gratis Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding

![De werking van deze handleiding](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

De voorbeeldtoepassing die u met deze handleiding maakt kunt een Windows-Desktop-toepassing die query's van de Microsoft Graph API of een Web-API die tokens van een Azure Active Directory v2-eindpunt accepteert. Voor dit scenario kunt u een token op HTTP-aanvragen via de autorisatie-header toevoegen. Microsoft Authentication Library (MSAL) wordt gebruikt voor ophalen van tokens en verlenging.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Afhandeling van ophalen van tokens voor toegang tot beveiligde Web-API 's

Nadat de gebruiker is geverifieerd, ontvangt de voorbeeld-App een token dat kan worden gebruikt om op te vragen van Microsoft Graph API of een Web-API die wordt beveiligd door Azure Active Directory v2.

API's, zoals Microsoft Graph vereisen een token voor het toestaan van toegang tot bepaalde resources. Bijvoorbeeld, is een token vereist voor het lezen van het profiel van een gebruiker, toegang tot de agenda van een gebruiker of het e-mailbericht verzenden. Uw toepassing kan een toegangstoken aanvragen met behulp van MSAL voor toegang tot deze resources door API-bereiken op te geven. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke aanroep die op basis van de beveiligde bron plaatsvindt. 

MSAL beheert opslaan in cache en het vernieuwen van de toegangstokens voor u, zodat uw toepassing niet te hoeft.

## <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding maakt gebruik van de volgende NuGet-pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL)|

