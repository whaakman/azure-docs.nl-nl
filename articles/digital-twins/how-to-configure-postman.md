---
title: Postman voor digitale dubbels voor Azure configureren | Microsoft Docs
description: Postman voor digitale dubbels voor Azure configureren
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: adgera
ms.openlocfilehash: 92ff8cb732c7c10c525d8a8ec76180cb435bd466
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53975005"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Postman voor digitale dubbels voor Azure configureren

In dit artikel wordt beschreven hoe u een Azure Active Directory (Azure AD)-toepassing voor het gebruik van de stroom voor OAuth 2.0-impliciete goedkeuring voor configureren. Vervolgens wordt het configureren van de Postman-REST-client voor het token gevolgen HTTP-aanvragen versturen naar uw beheer-API's beschreven.

## <a name="postman-summary"></a>Postman samenvatting

Aan de slag op digitale dubbels van Azure met behulp van een REST-clienthulpprogramma, zoals [Postman](https://www.getpostman.com/) voorbereiden van uw lokale omgeving voor testen. De Postman-client helpt snel complexe HTTP-aanvragen kunt maken. De bureaubladversie van de Postman-client downloaden door te gaan naar [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) is een op REST hulpprogramma waarmee belangrijke functies van HTTP-aanvraag in een handig desktop en GUI-invoegtoepassing op basis van testen. Via de Postman-client, oplossingen voor ontwikkelaars kunnen opgeven welk type HTTP-aanvraag (POST, GET, UPDATE, PATCH en DELETE), API-eindpunt voor het aanroepen en het gebruik van SSL. Postman biedt ook ondersteuning voor HTTP-aanvraagheaders toe te voegen, parameters, formuliergegevens en instanties.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Azure Active Directory voor het gebruik van de stroom voor OAuth 2.0-impliciete goedkeuring configureren

Configureer uw Azure AD-app voor het gebruik van de stroom van de impliciete goedkeuring voor OAuth 2.0.

1. Volg de stappen in [in deze snelstartgids](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) te maken van een Azure AD-toepassing van het type native modus. Of u een bestaande systeemeigen app-registratie opnieuw kunt gebruiken.

1. Onder **vereiste machtigingen**, selecteer **toevoegen** en voer **Azure digitale dubbels** onder **API-toegang toevoegen**. Als de API met uw zoekopdracht niet wordt gevonden, zoekt u in plaats daarvan **Azure Smart Spaces**. Selecteer **machtigingen verlenen > gedelegeerde machtigingen** en **gedaan**.

    ![Azure AD-app-registraties toevoegen api](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

1. Klik op **Manifest** manifest voor de toepassing voor uw app te openen. Stel *oauth2AllowImplicitFlow* naar `true`.

      ![Azure AD-impliciete stroom][1]

1. Configureer een **antwoord-URL** naar [ `https://www.getpostman.com/oauth2/callback` ](https://www.getpostman.com/oauth2/callback).

      ![Azure AD-antwoord-URL][2]

1. Kopiëren en houd de **toepassings-ID** van uw Azure AD-app. Hieronder wordt gebruikt.

## <a name="configure-the-postman-client"></a>De Postman-client configureren

Vervolgens instellen en configureren van Postman om op te halen van een Azure AD-token. Daarna moet u een geverifieerde HTTP-aanvraag voor digitale dubbels van Azure met behulp van het verkregen token:

1. Ga naar [www.getpostman.com]([https://www.getpostman.com/) om de app te downloaden.
1. Zorg ervoor dat uw **autorisatie-URL** juist is. Het duurt de indeling:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Name  | Vervangen door | Voorbeeld |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | De naam van uw tenant of organisatie | `microsoft` |

1. Selecteer de **autorisatie** tabblad **OAuth 2.0**, en selecteer vervolgens **nieuwe Access Token ophalen**.

    | Veld  | Waarde |
    |---------|---------|
    | Toekenningstype | `Implicit` |
    | URL voor terugbellen | `https://www.getpostman.com/oauth2/callback` |
    | Auth.-URL | Gebruik de **autorisatie-URL** uit stap 2 hierboven |
    | Client-id | Gebruik de **toepassings-ID** voor de Azure AD-app die is gemaakt of nieuwe bestemming krijgen uit de vorige sectie |
    | Bereik | Leeg laten |
    | Status | Leeg laten |
    | Clientverificatie | `Send as Basic Auth header` |

1. De client moet er nu uitzien zoals:

   ![Voorbeeld van de postman-client][3]

1. Selecteer **Token aanvragen**.

    >[!NOTE]
    >Als u ontvangt het foutbericht 'OAuth 2 kan niet worden voltooid', probeert u het volgende:
    > * Postman, sluit en opent u het opnieuw en probeer het opnieuw.
  
1. Schuif omlaag en selecteer **gebruik Token**.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over verificatie met de Management API's, [verifiëren met API's](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-configure-postman/implicit-flow.png
[2]: media/how-to-configure-postman/reply-url.png
[3]: media/how-to-configure-postman/postman-oauth-token.png
