---
title: 'De module services: Azure Maps gebruiken | Microsoft Docs'
description: Informatie over het gebruik van de module Azure Maps services.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: b56191bc93a91f944bb313b4ab9ad602da17dcf0
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357642"
---
# <a name="use-the-azure-maps-services-module"></a>Gebruik de module Azure Maps services

De Azure Maps Web SDK biedt een *module services*. Deze module is een helper-bibliotheek waarmee u eenvoudig de REST voor Azure-kaarten om services te gebruiken in web- of Node.js-toepassingen met behulp van JavaScript- of TypeScript.

## <a name="use-the-services-module-in-a-webpage"></a>Gebruik de module services in een webpagina

1. Maak een nieuwe HTML-bestand.
1. Laad de module Azure Maps services. U kunt dit op twee manieren laden:
    - Gebruik de wereldwijd gehoste, Azure Content Delivery Network-versie van de module Azure kaarten-services. Voeg een scriptverwijzing naar de `<head>` element van het bestand:

        ```html
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js"></script>
        ```

    - U kunt ook de broncode van Azure Maps Web SDK lokaal te laden met behulp de [azure-kaarten-rest](https://www.npmjs.com/package/azure-maps-rest) npm verpakt en deze vervolgens met uw app te hosten. Dit pakket bevat ook TypeScript-definities. Gebruik deze opdracht:
    
        > **npm install azure-kaarten-rest**
    
        Voeg een scriptverwijzing naar de `<head>` element van het bestand:

         ```html
        <script src="node_modules/azure-maps-rest/dist/js/atlas-service.min.js"></script>
         ```

1. Maakt een verificatiepijplijn. Voordat u kunt een service-eindpunt-URL-client initialiseren, moet u de pijplijn maken. Uw eigen sleutel voor Azure kaarten-account of Azure Active Directory (Azure AD)-referenties gebruiken om een Azure Maps Search-service-client te verifiëren. In dit voorbeeld wordt de Search service-URL-client worden gemaakt. 

    Als u een abonnementssleutel voor verificatie gebruiken:

    ```javascript
    // Get an Azure Maps key at https://azure.com/maps.
    var subscriptionKey = '<Your Azure Maps Key>';

    // Use SubscriptionKeyCredential with a subscription key.
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(subscriptionKey);

    // Use subscriptionKeyCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });

    // Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);
    ```

    Als u Azure AD voor verificatie gebruiken:

    ```javascript
    // Enter your Azure AD client ID.
    var clientId = "<Your Azure Active Directory Client Id>";

    // Use TokenCredential with OAuth token (Azure AD or Anonymous).
    var aadToken = await getAadToken();
    var tokenCredential = new atlas.service.TokenCredential(clientId, aadToken);

    // Create a repeating time-out that will renew the Azure AD token.
    // This time-out must be cleared when the TokenCredential object is no longer needed.
    // If the time-out is not cleared, the memory used by the TokenCredential will never be reclaimed.
    var renewToken = async () => {
    try {
      console.log("Renewing token");
      var token = await getAadToken();
      tokenCredential.token = token;
      tokenRenewalTimer = setTimeout(renewToken, getExpiration(token));
    } catch (error) {
      console.log("Caught error when renewing token");
      clearTimeout(tokenRenewalTimer);
      throw error;
    }
    }
    tokenRenewalTimer = setTimeout(renewToken, getExpiration(aadToken));

    // Use tokenCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(tokenCredential, {
    retryOptions: { maxTries: 4 } // Retry options
    });

    // Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);

    function getAadToken() {
        // Use the signed-in auth context to get a token.
        return new Promise((resolve, reject) => {
            // The resource should always be https://atlas.microsoft.com/.
            const resource = "https://atlas.microsoft.com/";
            authContext.acquireToken(resource, (error, token) => {
                if (error) {
                    reject(error);
                } else {
                    resolve(token);
                }
            });
        })
    }

    function getExpiration(jwtToken) {
        // Decode the JSON Web Token (JWT) to get the expiration time stamp.
        const json = atob(jwtToken.split(".")[1]);
        const decode = JSON.parse(json);

        // Return the milliseconds remaining until the token must be renewed.
        // Reduce the time until renewal by 5 minutes to avoid using an expired token.
        // The exp property is the time stamp of the expiration, in seconds.
        const renewSkew = 300000;
        return (1000 * decode.exp) - Date.now() - renewSkew;
    }
    ```

    Zie voor meer informatie, [verificatie met Azure Maps](azure-maps-authentication.md).

1. De volgende code maakt gebruik van de zojuist gemaakte Azure Search service-URL-client naar een adres geocode: "1 Microsoft Way, Redmond, WA". De code wordt gebruikgemaakt van de `searchAddress` functioneren en de resultaten worden weergegeven als een tabel in de hoofdtekst van de pagina.

    ```javascript
    // Search for "1 microsoft way, redmond, wa".
    searchURL.searchAddress(atlas.service.Aborter.timeout(10000), '1 microsoft way, redmond, wa').then(response => {
      var html = [];

      // Display the total results.
      html.push('Total results: ', response.summary.numResults, '<br/><br/>');

      // Create a table of the results.
      html.push('<table><tr><td></td><td>Result</td><td>Latitude</td><td>Longitude</td></tr>');

      for(var i=0;i<response.results.length;i++){
        html.push('<tr><td>', (i+1), '.</td><td>', 
          response.results[i].address.freeformAddress, 
          '</td><td>', 
          response.results[i].position.lat,
          '</td><td>', 
          response.results[i].position.lon,
          '</td></tr>');
      }

      html.push('</table>');

      // Add the resulting HTML to the body of the page.
      document.body.innerHTML = html.join('');
    });
    ```

    Dit is de volledige codevoorbeeld uitvoert:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Met behulp van de Module Services" src="//codepen.io/azuremaps/embed/zbXGMR/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/zbXGMR/'>met behulp van de Module Services</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [MapsURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.mapsurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [SearchURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [RouteURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [SubscriptionKeyCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.subscriptionkeycredential?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [TokenCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.tokencredential?view=azure-maps-typescript-latest)

Voor meer voorbeelden van code die gebruikmaken van de module services, Zie de volgende artikelen:

> [!div class="nextstepaction"]
> [Zoekresultaten weergeven op de kaart](./map-search-location.md)

> [!div class="nextstepaction"]
> [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Routebeschrijving van A naar B](./map-route.md)