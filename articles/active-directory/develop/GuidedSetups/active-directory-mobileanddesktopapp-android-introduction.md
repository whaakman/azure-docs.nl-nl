---
title: Azure AD v2 Android aan de slag - Inleiding | Microsoft Docs
description: Hoe een Android-app kunt ophalen van een toegangstoken en Microsoft Graph API of API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt aanroepen
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
ms.openlocfilehash: d933781713456f73aa76db557fdf35672dfb2a29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Microsoft Graph API aanroepen vanuit een Android-app

Deze handleiding wordt uitgelegd hoe een systeemeigen Android-toepassing kunt ophalen van een toegangstoken en Microsoft Graph API of andere API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt niet aanroepen.

Aan het einde van deze handleiding, uw toepassing kan worden aanroepen van een beveiligde API met persoonlijke accounts (inclusief outlook.com, live.com en anderen) en de werk- en schoolaccounts van een bedrijf of organisatie die Azure Active Directory heeft.  

### <a name="how-this-sample-works"></a>De werking van dit voorbeeld
![De werking van dit voorbeeld](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Het voorbeeld dat is gemaakt door deze handleiding is gebaseerd op een scenario waarbij een Android-toepassing wordt gebruikt voor het zoeken van een Web-API die tokens van Azure Active Directory v2-eindpunt: in dit geval Microsoft Graph API accepteert. Voor dit scenario wordt een token toegevoegd om HTTP-aanvragen via de autorisatie-header. Token verkrijgen en verlenging wordt verwerkt door de Microsoft Authentication Library (MSAL).

### <a name="pre-requisites"></a>Vereisten
* Deze Begeleide instelprocedure is gericht op Android Studio, maar ook alle andere Android-toepassing-ontwikkelomgeving aanvaardbaar is. 
* Android SDK 21 of hoger is vereist (25 SDK wordt aanbevolen).
* Google Chrome of een webbrowser met de tabbladen voor aangepaste is vereist voor deze release van Microsoft Authentication Library (MSAL) voor Android.

> Opmerking: Google Chrome is niet opgenomen in Visual Studio-Emulator voor Android. We raden u aan deze code op een Emulator met 25 API of een afbeelding met API-21 of hoger die met Google Chrome geïnstalleerd testen.


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a>Hoe moet worden verwerkt token verkrijgen voor toegang tot een beveiligde Web-API

Nadat de gebruiker zich verifieert, ontvangt de voorbeeldtoepassing een token dat kan worden gebruikt voor Microsoft Graph API of een Web-API worden beveiligd door Microsoft Azure Active Directory-v2 query.

API's zoals Microsoft Graph vereisen een toegangstoken waarmee toegang tot specifieke bronnen – bijvoorbeeld een gebruikersprofiel, toegang tot de agenda gebruiker lezen of een e-mailbericht verzenden. Uw toepassing kan aanvragen van een toegangstoken MSAL gebruiken voor toegang tot deze bronnen door te geven van de API-scopes. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke aanroep ten opzichte van de beveiligde bron. 

MSAL kunt opslaan in cache en vernieuwen toegangstokens voor u, zodat uw toepassing niet te hoeft beheren.

### <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheken:

|Bibliotheek|Beschrijving|
|---|---|
|[com.Microsoft.Identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft-Verificatiebibliotheek (MSAL)|
