---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 4d5c8373f2a8f02ccfbe51cb970ab08744950e7e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019185"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Gebruik de instellingen die zijn opgegeven in de tabel om de webtoepassing te registreren.

![Voorbeeld van registratie-instellingen voor een nieuwe web-app](./media/active-directory-b2c-register-web-app/b2c-new-app-settings.png)

| Instelling      | Voorbeeldwaarde  | Beschrijving                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Naam** | Contoso B2C-app | Geef een **naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. | 
| **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een webtoepassing. |
| **Impliciete stroom toestaan** | Ja | Selecteer **Ja** als voor de toepassing wordt gebruikgemaakt van [Aanmelding via OpenID Connect](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) |
| **Antwoord-URL** | `https://localhost:44316` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die zijn aangevraagd voor de toepassing. Voer een [juiste](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **antwoord-URL** in. In dit voorbeeld is de app lokaal en luistert deze op poort 44316. |

Klik op **Maken** om uw toepassing te registreren.

De zojuist geregistreerde toepassing wordt weergegeven in de lijst met toepassingen voor de B2C-tenant. Selecteer de web-app in de lijst. Het eigenschappenvenster van de webtoepassing wordt weergegeven.

![Eigenschappen van webtoepassing](./media/active-directory-b2c-register-web-app/b2c-web-app-properties.png)

Noteer de globaal **unieke client-id** voor de toepassing. U gebruikt de id in de code van uw toepassing.