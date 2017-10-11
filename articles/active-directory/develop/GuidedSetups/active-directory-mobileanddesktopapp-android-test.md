---
title: Azure AD v2 Android aan de slag - testen | Microsoft Docs
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Testen van uw code

1. Implementeer uw code op uw apparaat/emulator.
2. Wanneer u klaar bent om te testen, gebruikt u een Microsoft Azure Active Directory (organisatieaccount) of een Microsoft-Account (live.com, outlook.com)-account aan te melden. 

![Schermopname van een steekproef](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Aanmelden](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Toestemming
De eerste keer dat een gebruiker zich bij uw toepassing aanmeldt ze krijgt een scherm toestemming vergelijkbaar met de hieronder, waar ze moeten expliciet accepteren: 

![Toestemming](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Verwachte resultaten
U ziet de resultaten van een aanroep naar Microsoft Graph API 'me' eindpunt dat wordt gebruikt op het gebruikersprofiel - https://graph.microsoft.com/v1.0/me verkrijgen. Voor een lijst met algemene Microsoft Graph-eindpunten, raadpleegt u dit [artikel](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de `user.read` bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard in elke toepassing die wordt geregistreerd op onze portal voor wachtwoordregistratie. Sommige andere API's voor Microsoft Graph evenals aangepaste API's voor uw back-endserver moet mogelijk extra scopes. Bijvoorbeeld: voor het bereik van Microsoft Graph `Calendars.Read` is vereist voor een lijst met agenda's van de gebruiker. Voor toegang tot de agenda van de gebruiker in een context van een toepassing, moet u toevoegen de `Calendars.Read` overgedragen machtigingen voor de toepassingsregistratie-informatie en voeg vervolgens de `Calendars.Read` bereik voor de `acquireTokenSilentAsync` aanroepen. De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

<!--end-collapse-->
