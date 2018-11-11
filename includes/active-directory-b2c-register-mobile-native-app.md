---
title: bestand opnemen
description: bestand opnemen
services: active-directory-b2c
author: davidmu1
ms.service: active-directory-b2c
ms.topic: include
ms.date: 04/09/2018
ms.author: davidmu
ms.custom: include file
ms.openlocfilehash: 8363d023e89c77aabc0d123f19264c9a0758a656
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51208242"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Gebruik de instellingen die zijn opgegeven in de tabel om uw mobiele of native toepassing te registreren.

![Voorbeeld van registratie-instellingen voor nieuwe mobiele of native toepassing](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Instelling      | Voorbeeldwaarde  | Beschrijving                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Naam** | Contoso B2C-app | Geef een **naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. |
| **Systeemeigen client** | Ja | Selecteer **Ja** voor een mobiele of native toepassing. |
| **Aangepaste omleidings-URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Voer een omleidings-URI met een aangepast schema in. Zorg ervoor dat u een [goede omleidings-URI](../articles/active-directory-b2c/active-directory-b2c-app-registration.md) kiest en geen speciale tekens zoals onderstrepingstekens gebruikt. |

Klik op **Maken** om uw toepassing te registreren.

De zojuist geregistreerde toepassing wordt weergegeven in de lijst met toepassingen voor de B2C-tenant. Selecteer uw mobiele of native app in de lijst. Het eigenschappenvenster van de toepassing wordt weergegeven.

![Toepassingseigenschappen](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Noteer de globaal unieke**client-id** voor de toepassing. U gebruikt de id in de code van uw toepassing.

Als de native toepassing een web-API aanroept die is beveiligd door Azure AD B2C, voert u deze stappen uit:
   1. Maak een toepassingsgeheim door naar de blade **Sleutels** te gaan en op de knop **Sleutel genereren** te klikken. Noteer de waarde van **App-sleutel**. U gebruikt de waarde als het toepassingsgeheim in de code van uw toepassing.
   2. Klik op **API-toegang**, klik op **Toevoegen** en selecteer uw web-API en bereiken (machtigingen).

> [!NOTE]
> Een **toepassingsgeheim** is een belangrijke beveiligingsreferentie en moet op de juiste wijze worden beveiligd.
> 
