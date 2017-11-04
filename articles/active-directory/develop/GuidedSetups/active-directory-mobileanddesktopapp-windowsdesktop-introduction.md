---
title: Azure AD v2 Windows Desktop aan de slag - Inleiding | Microsoft Docs
description: Hoe Windows Desktop .NET (XAML) toepassingen een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
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
ms.openlocfilehash: 4a695c00fce4deb02261ba58ec95469746bb1486
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Microsoft Graph API aanroepen vanuit een Windows-bureaublad-app

Deze handleiding wordt uitgelegd hoe een systeemeigen Windows Desktop .NET (XAML)-toepassing kunt ophalen van een toegangstoken en Microsoft Graph API of andere API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt niet aanroepen.

Aan het einde van deze handleiding, uw toepassing kan worden aanroepen van een beveiligde API met persoonlijke accounts (inclusief outlook.com, live.com en anderen) en de werk- en schoolaccounts van een bedrijf of organisatie die Azure Active Directory heeft.  

> Deze handleiding is Visual Studio 2015 Update 3 of Visual Studio 2017 vereist.  Hebt u geen deze? [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

De voorbeeldtoepassing die is gemaakt door deze handleiding kunt een Windows-bureaublad toepassing Microsoft Graph API of een Web-API die tokens van Azure Active Directory-v2-eindpunt accepteert opvragen. Voor dit scenario wordt een token toegevoegd om HTTP-aanvragen via de autorisatie-header. Token verkrijgen en verlenging wordt verwerkt door de Microsoft Authentication Library (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Afhandeling van token verkrijgen voor toegang tot Web-API's beveiligd

Nadat de gebruiker zich verifieert, ontvangt de voorbeeldtoepassing een token dat kan worden gebruikt voor Microsoft Graph API of een Web-API worden beveiligd door Microsoft Azure Active Directory-v2 query.

API's zoals Microsoft Graph vereisen een toegangstoken waarmee toegang tot specifieke bronnen – bijvoorbeeld een gebruikersprofiel, toegang tot de agenda gebruiker lezen of een e-mailbericht verzenden. Uw toepassing kan aanvragen van een toegangstoken MSAL gebruiken voor toegang tot deze bronnen door te geven van de API-scopes. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke aanroep ten opzichte van de beveiligde bron. 

MSAL kunt opslaan in cache en vernieuwen toegangstokens voor u, zodat uw toepassing niet te hoeft beheren.


### <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding bevat de volgende NuGet-pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft-Verificatiebibliotheek (MSAL)|

