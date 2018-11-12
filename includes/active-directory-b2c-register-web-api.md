---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: fcd71f74e0b00934958828024094773e42496b66
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51017267"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Gebruik de instellingen die zijn opgegeven in de tabel, om de web-API te registreren.

![Voorbeeld van registratie-instellingen voor een nieuwe web-API](./media/active-directory-b2c-register-web-api/b2c-new-web-api-settings.png)

| Instelling      | Voorbeeldwaarde  | Beschrijving                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Naam** | Contoso B2C-API | Voer een **Naam** in voor de toepassing waarmee de API wordt beschreven voor consumenten. | 
| **Web-app / web-API opnemen** | Ja | Selecteer **Ja** voor een web-API. |
| **Impliciete stroom toestaan** | Ja | Selecteer **Ja** als voor de toepassing wordt gebruikgemaakt van [Aanmelding via OpenID Connect](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) |
| **Antwoord-URL** | `https://localhost:44316/` | Antwoord-URL's zijn eindpunten waarop Azure AD B2C tokens retourneert die zijn aangevraagd voor de toepassing. In dit voorbeeld is de web-API lokaal en luistert deze op poort 44316. |
| **App-id-URI** | api | De app-id-URI is de id die wordt gebruikt voor de web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. |

Klik op **Maken** om uw toepassing te registreren.

De zojuist geregistreerde toepassing wordt weergegeven in de lijst met toepassingen voor de B2C-tenant. Selecteer de web-API in de lijst. Eigenschappendeelvenster voor de API wordt weergegeven.

![Eigenschappen van de web-API](./media/active-directory-b2c-register-web-api/b2c-web-api-properties.png)

Noteer de globaal unieke **client-id voor de toepassing**. U gebruikt de id in de code van uw toepassing.