---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 5fe9fe8ced675f68161f0df9f2665b47f9d47ac5
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55905059"
---
### <a name="server-auth"></a>Procedures: Verifiëren bij een provider (Server Flow)
Als u het verificatieproces in uw app door Mobile Apps wilt laten beheren, moet u uw app registreren bij uw id-provider. Daarna moet u in uw Azure App Service de door uw provider verstrekte toepassings-id en geheim configureren.
Zie de zelfstudie [Verificatie toevoegen aan uw app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md) voor meer informatie.

Wanneer u de id-provider hebt geregistreerd, roept u de methode `.login()` aan met de naam van uw provider. Als u bijvoorbeeld wilt aanmelden met Facebook-gebruik de volgende code:

```javascript
client.login("facebook").done(function (results) {
     alert("You are now signed in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

De geldige waarden voor de provider zijn 'aad', 'facebook', 'google', 'microsoftaccount' en 'twitter'.

> [!NOTE]
> Verificatie via Google werkt momenteel niet via Server Flow.  Voor verificatie via Google moet u een [client-flowmethode](#client-auth) gebruiken.

In dit geval beheert Azure App Service de OAuth 2.0-verificatiestroom.  Het geeft de aanmeldingspagina van de geselecteerde provider weer en genereert een App Service-verificatietoken na geslaagde aanmelding met de id-provider. De aanmeldingsfunctie retourneert na voltooiing een JSON-object dat zowel de gebruikers-id als het App Service-verificatietoken respectievelijk in de velden userId en authenticationToken weergeeft. Dit token kan worden opgeslagen in de cache en opnieuw worden gebruikt totdat het verloopt.

### <a name="client-auth"></a>Procedures: Verifiëren bij een provider (Client Flow)

Uw app kan ook afzonderlijk contact opnemen met de id-provider en het geretourneerde token vervolgens aan uw App Service verstrekken voor verificatie. Met deze clientstroom kunt u een eenmalige aanmelding bieden voor gebruikers of aanvullende gebruikersgegevens ophalen van de id-provider.

#### <a name="social-authentication-basic-example"></a>Eenvoudig voorbeeld van sociale verificatie

In dit voorbeeld wordt de client-SDK van Facebook gebruikt voor verificatie:

```javascript
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now signed in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
In dit voorbeeld wordt ervan uitgegaan dat het token dat is opgegeven door de betreffende SDK-provider, is opgeslagen in de tokenvariabele.

### <a name="auth-getinfo"></a>Procedures: Informatie over de geverifieerde gebruiker ophalen

De verificatiegegevens kunnen worden opgehaald uit het `/.auth/me`-eindpunt met een HTTP aanroep aan een willekeurige AJAX-bibliotheek.  Zorg ervoor dat u de `X-ZUMO-AUTH`-header in uw verificatietoken instelt.  Het verificatietoken wordt opgeslagen in `client.currentUser.mobileServiceAuthenticationToken`.  Als u bijvoorbeeld de API 'Ophalen' gebruikt:

```javascript
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Ophalen is beschikbaar als [npm-pakket](https://www.npmjs.com/package/whatwg-fetch), maar kan ook met een browser worden gedownload van [CDNJS](https://cdnjs.com/libraries/fetch). U kunt ook jQuery of een andere AJAX-API gebruiken voor het ophalen van de informatie.  Gegevens worden ontvangen als JSON-object.
