## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>De Microsoft Authentication Library (MSAL) gebruiken voor de gebruiker van de aanmeldingspagina

1.  Maak een bestand met de naam `app.js`. Als u Visual Studio gebruikt, selecteert u het project (basismap project), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `JavaScript File`:
2.  Voeg de volgende code naar uw `app.js` bestand:

```javascript
// Graph API endpoint to show user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used to obtain the access token to read user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue to display user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call the Microsoft Graph API and display the results on the page. Sign the user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user to sign in via loginRedirect.
        // This will redirect user to the Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // The call to loginRedirect above frontloads the consent to query Graph API during the sign-in.
        // If you want to use dynamic consent, just remove the graphAPIScopes from loginRedirect call.
        // As such, user will be prompted to give consent when requested access to a resource that 
        // he/she hasn't consented before. In the case of this application - 
        // the first time the Graph API call to obtain user's profile is executed.
    } else {
        // If user is already signed in, display the user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API to show the user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order to call the Graph API, an access token needs to be acquired.
        // Try to acquire the token used to query Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After the access token is acquired, call the Web API, sending the acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If the acquireTokenSilent() method fails, then acquire the token interactively via acquireTokenRedirect().
                // In this case, the browser will redirect user back to the Azure Active Directory v2 Endpoint so the user 
                // can reenter the current username/ password and/ or give consent to new permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get the token silently, the Graph API call results will be made and results will be displayed in the page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });
    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() to show results.
 * @param {string} errorDesc - If error occur, the error message
 * @param {object} token - The token received from login
 * @param {object} error - The error string
 * @param {string} tokenType - The token type: For loginRedirect, tokenType = "id_token". For acquireTokenRedirect, tokenType:"access_token".
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in the page
 * @param {string} endpoint - the endpoint used for the error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

Nadat een gebruiker op de *'Microsoft Graph API aanroepen'* knop voor de eerste keer `callGraphApi` methodeaanroepen `loginRedirect` aan te melden als de gebruiker. Deze methode resulteert in het omleiden van de gebruiker de *Microsoft Azure Active Directory-v2-eindpunt* te vragen en referenties van de gebruiker te valideren. Als gevolg van een geslaagde aanmelden, de gebruiker wordt omgeleid naar de oorspronkelijke *index.html* pagina en een token wordt ontvangen, verwerkt door `msal.js` en de gegevens in het token in de cache wordt opgeslagen. Dit token wordt ook wel de *token ID* en algemene informatie over de gebruiker, zoals de weergavenaam van de gebruiker bevat. Als u wilt dat alle gegevens die door dit token voor andere doeleinden te gebruiken, moet u ervoor zorgen dat dit token is gevalideerd door uw back-endserver om te waarborgen dat het token is uitgegeven voor een geldige gebruiker voor uw toepassing.

De SPA die worden gegenereerd door deze handleiding maakt geen gebruik van het token ID rechtstreeks – in plaats daarvan wordt aangeroepen `acquireTokenSilent` en/of `acquireTokenRedirect` aan te schaffen een *toegangstoken* gebruikt voor het zoeken van de Microsoft Graph API. Als u een steekproef die het valideert het token ID moet, Bekijk [dit](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 voorbeeld") voorbeeldtoepassing in GitHub – het voorbeeld maakt gebruik van een ASP .NET-web-API voor validatie van tokens.

#### <a name="getting-a-user-token-interactively"></a>Een gebruiker ophalen interactief token

Na de initiële aanmelden, u niet wilt dat de gebruikers vragen om andere referenties telkens wanneer ze nodig hebben om aan te vragen van een token voor toegang tot een bron – dus *acquireTokenSilent* de meeste gevallen moet worden gebruikt voor het verkrijgen van tokens. Er zijn echter situaties die u nodig hebt om af te dwingen gebruikers werken met Azure Active Directory-v2-eindpunt: voorbeelden zijn:
-   Gebruikers willen hun referenties opnieuw invoeren omdat het wachtwoord is verlopen
-   Uw toepassing vraagt toegang tot een resource die de gebruiker toestemming geven moet aan
-   Tweeledige verificatie is vereist

Het aanroepen van de *acquireTokenRedirect(scope)* ertoe leiden dat gebruikers omleiden naar het Azure Active Directory-v2-eindpunt (of *acquireTokenPopup(scope)* resultaat op een pop-upvenster) waar gebruikers moeten communiceren met door het bevestigen van hun referenties, het toestemming verlenen aan de vereiste resource of voltooien van rekening de twee te houden met verificatie.

#### <a name="getting-a-user-token-silently"></a>Een gebruiker ophalen achtergrond token
De ` acquireTokenSilent` methode verwerkt token acquisities van organisaties en verlenging zonder tussenkomst van de gebruiker. Na `loginRedirect` (of `loginPopup`) wordt uitgevoerd voor de eerste keer `acquireTokenSilent` is de methode die vaak worden gebruikt voor het verkrijgen van tokens die worden gebruikt voor toegang tot beveiligde bronnen voor volgende aanroepen - aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden aangebracht.
`acquireTokenSilent`kan mislukken in sommige gevallen – bijvoorbeeld van de gebruiker het wachtwoord is verlopen. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

1.  Voer een aanroep naar `acquireTokenRedirect` onmiddellijk, wat ertoe leidt de gebruiker aan te melden. Dit patroon wordt meestal gebruikt in toepassingen met online wanneer er geen niet-geverifieerde inhoud in de toepassing beschikbaar is voor de gebruiker is. Dit patroon maakt gebruik van de steekproef die worden gegenereerd door deze Begeleide installatie.

2. Toepassingen kunnen ook een visuele indicatie maken voor de gebruiker die een interactief aanmelden is vereist, zodat de gebruiker het juiste moment aan te melden kunt selecteren of de toepassing opnieuw kunt `acquireTokenSilent` op een later tijdstip. Dit wordt meestal gebruikt wanneer de gebruiker andere functies van de toepassing gebruiken kunt zonder wordt onderbroken: er bijvoorbeeld niet-geverifieerde inhoud beschikbaar is in de toepassing is. In dit geval wordt bepalen de gebruiker wanneer ze aanmelden willen voor toegang tot de beveiligde bron, of om de verouderde gegevens te vernieuwen.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>De Microsoft Graph API met behulp van het token dat u zojuist hebt verkregen aanroepen

Voeg de volgende code naar uw `app.js` bestand:

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used to display the results
 * @param {object} showTokenElement = HTML element used to display the RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in the page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in the page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In de voorbeeldtoepassing die is gemaakt door deze handleiding worden de `callWebApiWithToken()` methode wordt gebruikt voor het maken van een HTTP `GET` aanvragen op basis van een beveiligde bron die is een token vereist en vervolgens de inhoud naar de aanroeper wordt geretourneerd. Met deze methode wordt het token verkregen in de *HTTP-autorisatie-header*. Voor de voorbeeldtoepassing die is gemaakt door deze handleiding, de bron is de Microsoft Graph API *mij* eindpunt – waarin informatie over het profiel van de gebruiker.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Een methode voor het ondertekenen van de gebruiker toevoegen

Voeg de volgende code naar uw `app.js` bestand:

```javascript
/**
 * Sign-out the user
 */
function signOut() {
    userAgentApplication.logout();
}
```
