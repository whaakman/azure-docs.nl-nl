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
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7e7e9d078bf9339beb2ad5ac53ea858e843242ce
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293620"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Meld u aan gebruikers en de Microsoft Graph aanroepen vanuit een Android-app

In deze zelfstudie leert u hoe u een Android-toepassing bouwen en integreren van Microsoft identity-platform. Deze app wordt specifiek, meld u aan een gebruiker, een toegangstoken voor het aanroepen van de Microsoft Graph API en maken van een algemene aanvraag naar de Microsoft Graph API.  

Wanneer u de handleiding hebt voltooid, wordt uw toepassing accepteert aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en anderen) en werk of school-accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory. 

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>De werking van de voorbeeld-app die is gegenereerd door deze handleiding
![De werking van dit voorbeeld](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

De app in dit voorbeeld wordt Meld u aan gebruikers en gegevens ophalen in hun naam.  Deze gegevens kan worden bereikt via een externe API (Microsoft Graph-API in dit geval) die is autorisatie vereist en die ook wordt beveiligd door de Microsoft identity-platform. 

Meer specifiek, 
* Uw app wordt gestart, een webpagina aan te melden bij de gebruiker.
* Uw app worden een toegangstoken uitgegeven voor de Microsoft Graph API.
* Het toegangstoken worden opgenomen in de HTTP-aanvraag naar de web-API.
* Het antwoord van de Microsoft Graph worden verwerkt. 

In dit voorbeeld maakt gebruik van de Microsoft Authentication library voor Android (MSAL) te coördineren en te helpen met verificatie MSAL wordt automatisch vernieuwen van tokens, eenmalige aanmelding bieden tussen andere apps op het apparaat, bij het beheren van het account (s) en de meeste gevallen van voorwaardelijke toegang verwerken. 

## <a name="prerequisites"></a>Vereisten
* Deze Begeleide installatie maakt gebruik van Android Studio-3.0. 
* Android 21 of hoger is vereist (25 + wordt aanbevolen).
* Google Chrome of een webbrowser die gebruikmaakt van aangepaste tabbladen is vereist voor deze versie van MSAL voor Android.

## <a name="library"></a>Bibliotheek

Deze handleiding worden de volgende verificatiebibliotheek gebruikt:

|Bibliotheek|Beschrijving|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|
