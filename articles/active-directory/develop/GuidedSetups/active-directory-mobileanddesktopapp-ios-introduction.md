---
title: Azure AD v2 iOS Getting Started - Inleiding | Microsoft Docs
description: Hoe iOS (Swift)-toepassingen met een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 5fcd78eaa0c5b09b70aa973466a34556fff56071
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="call-the-microsoft-graph-api-from-an-ios-app"></a>De Microsoft Graph-API aanroepen vanuit een iOS-app

Deze handleiding wordt uitgelegd hoe een systeemeigen iOS-toepassing (Swift) kunt ophalen van een toegangstoken en bel de Microsoft Graph API of andere API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt.

Aan het einde van deze handleiding, uw toepassing kan worden aanroepen van een beveiligde API met persoonlijke accounts (inclusief outlook.com, live.com en anderen) en de werk- en schoolaccounts van een bedrijf of organisatie die Azure Active Directory heeft.

> ### <a name="pre-requisites"></a>Vereisten
> - XCode 8.x is vereist voor deze handleiding. U kunt downloaden XCode [hiervandaan](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode downloaden URL").
> - [Carthage](https://github.com/Carthage/Carthage) voor het beheer van het pakket

### <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

De voorbeeldtoepassing die is gemaakt door deze handleiding kunt een iOS-toepassing query uitvoeren op de Microsoft Graph API of een Web-API die tokens van Azure Active Directory-v2-eindpunt accepteert. Voor dit scenario wordt een token toegevoegd om HTTP-aanvragen via de autorisatie-header. Token verkrijgen en verlenging wordt verwerkt door de Microsoft Authentication Library (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Afhandeling van token verkrijgen voor toegang tot Web-API's beveiligd

Nadat de gebruiker zich verifieert, ontvangt de voorbeeldtoepassing een token dat kan worden gebruikt voor het opvragen van de Microsoft Graph API of een Web-API worden beveiligd door Microsoft Azure Active Directory-v2.

API's zoals Microsoft Graph vereisen een toegangstoken waarmee toegang tot specifieke bronnen – bijvoorbeeld een gebruikersprofiel, toegang tot de agenda gebruiker lezen of een e-mailbericht verzenden. Uw toepassing kan een toegangstoken MSAL door te geven van de API-scopes met aanvragen. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke aanroep ten opzichte van de beveiligde bron.

MSAL kunt opslaan in cache en vernieuwen toegangstokens voor u, zodat uw toepassing niet te hoeft beheren.


### <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheek:

|Bibliotheek|Beschrijving|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Preview van Microsoft verificatie-bibliotheek voor iOS|

