---
title: 'Met behulp van de module Services: Azure Maps | Microsoft Docs'
description: Informatie over het gebruik van de module Azure Maps services.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: e614758a91cb3ff02822eeeeb8ae7e80d2123e5d
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578727"
---
# <a name="using-the-azure-maps-services-module"></a>Met behulp van de module Azure Maps Services

Azure Maps Web SDK biedt een services-module die is een helper-bibliotheek waarmee u eenvoudig de services van Azure Maps REST in web- of Node.js-toepassingen met behulp van JavaScript- of TypeScript te gebruiken.

## <a name="using-the-services-module-in-a-web-page"></a>Met behulp van de module services in webpagina 's

1. Maak een nieuwe HTML-bestand.
2. Laden van de module Azure Maps Services. Dit kan worden gedaan met behulp van een van twee opties.

    a. Gebruik de wereldwijd gehoste CDN-versie van de module Azure Maps services door het toevoegen van een scriptverwijzing naar de <head> element van het bestand:
    
    ```html
    <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2"></script>
    ```
    
    b. U kunt ook laden van de Web-SDK van Azure kaarten-broncode lokaal via de [azure-kaarten-rest](https://www.npmjs.com/package/azure-maps-rest) NPM verpakt en hosten van de App met uw app. Dit pakket bevat ook TypeScript-definities.
    
    > npm install azure-kaarten-rest
    
    Voeg een scriptverwijzing naar de `<head>` element van het bestand:
    
    ```html
    <script src="node_modules/azure-maps-rest/dist/js/atlas-service.min.js"></script>
    ```

3. Voor het initialiseren van een service-eindpunt-URL-client, moet u eerst een verificatiepijplijn te maken. Uw eigen sleutel voor Azure kaarten-account of Azure Active Directory (AAD)-referenties gebruiken om de search service-client te verifiëren. In dit voorbeeld wordt de search service-URL-client worden gemaakt. Als een abonnementssleutel voor verificatie:

    ```javascript
    //Get an Azure Maps key at https://azure.com/maps
    var subscriptionKey = '<Your Azure Maps Key>';
    
    //Use SubscriptionKeyCredential with a subscription key.
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(subscriptionKey);
    
    //Use subscriptionKeyCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });
    
    //Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);
    ```
    
    Als u met behulp van Azure Active Directory (AAD) voor verificatie:

    ```javascript
    // Enter your Azure Actiuve Directory client ID.
    var clientId = "<Your Azure Active Directory Client Id>";
    
    // Use TokenCredential with OAuth token (AAD or Anonymous).
    var aadToken = await getAadToken();
    var tokenCredential = new atlas.service.TokenCredential(clientId, aadToken);
    
    // Create a repeating timeout that will renew the AAD token.
    // This timeout must be cleared once the TokenCredential object is no longer needed.
    // If the timeout is not cleared the memory used by the TokenCredential will never be reclaimed.
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
    
    // Use tokenCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(tokenCredential, {
        retryOptions: { maxTries: 4 } // Retry options
    });
    
    //Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);

    function getAadToken() {
        //Use the logged in auth context to get a token.
        return new Promise((resolve, reject) => {
            //The resource should always be https://atlas.microsoft.com/.
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
        //Decode the JWT token to get the expiration timestamp.
        const json = atob(jwtToken.split(".")[1]);
        const decode = JSON.parse(json);

        //Return the milliseconds until the token needs renewed.
        //Reduce the time until renew by 5 minutes to avoid using an expired token.
        //The exp property is the timestamp of the expiration in seconds.
        const renewSkew = 300000;
        return (1000 * decode.exp) - Date.now() - renewSkew;
    }
    ```

    Zie voor meer informatie, [verificatie met Azure Maps](azure-maps-authentication.md).

4. De volgende code gebruikt de zojuist gemaakte search service-URL client geocode een adres met behulp van "1 Microsoft manier, Redmond, WA" de `searchAddress` functioneren en de resultaten weergegeven als een tabel in de hoofdtekst van de pagina. 

    ```javascript
    //Search for "1 microsoft way, redmond, wa".
    searchURL.searchAddress(atlas.service.Aborter.timeout(10000), '1 microsoft way, redmond, wa').then(response => {
      var html = [];
      
      //Display the total results.
      html.push('Total results: ', response.summary.numResults, '<br/><br/>');
     
      //Create a table of the results.
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
      
      //Add the result HTML to the body of the page.
      document.body.innerHTML = html.join('');
    });
    ```

    Hier volgt de voorbeeldcode van de volledig uitgevoerd:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Met behulp van de Module Services" src="//codepen.io/azuremaps/embed/zbXGMR/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/zbXGMR/'>met behulp van de Module Services</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [MapsURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.mapsurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SearchURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [RouteURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SubscriptionKeyCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.subscriptionkeycredential?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TokenCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.tokencredential?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer voorbeelden van code die gebruikmaken van de module services:

> [!div class="nextstepaction"]
> [Zoekresultaten weergeven op de kaart](./map-search-location.md)

> [!div class="nextstepaction"]
> [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Routebeschrijving van A naar B](./map-route.md)