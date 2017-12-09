
## <a name="setting-up-your-web-server-or-project"></a>Instellen van uw webserver of -project

> Voorkeur voor het downloaden van dit voorbeeld project in plaats daarvan? 
> - [De Visual Studio-project downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> of
> - [De project-bestanden downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) voor een lokale webserver, zoals Python
>
> En ga vervolgens verder met de [configuratiestap](#create-an-application-express) voor het configureren van het codevoorbeeld voordat deze wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten
Een lokale webserver zoals [Python http.server](https://www.python.org/downloads/), [HTTP-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core), of IIS Express integratie met [Visual Studio 2017](https://www.visualstudio.com/downloads/) Deze begeleide setup uit te voeren is vereist. 

Instructies in deze handleiding zijn gebaseerd op zowel Python en Visual Studio 2017, maar gerust ontwikkelomgeving of webserver.

## <a name="create-your-project"></a>Uw project maken 

> ### <a name="option-1-visual-studio"></a>Optie 1: Visual Studio 
> Als u met behulp van Visual Studio en een nieuw project maakt, volgt u onderstaande stappen voor het maken van een nieuwe Visual Studio-oplossing:
> 1.    In Visual Studio:`File` > `New` > `Project`
> 2.    Onder `Visual C#\Web`, selecteer`ASP.NET Web Application (.NET Framework)`
> 3.    Naam van uw toepassing en klik op *OK*
> 4.    Onder `New ASP.NET Web Application`, selecteer`Empty`

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a>Optie 2: Python / andere webservers
> Zorg ervoor dat u hebt geïnstalleerd [Python](https://www.python.org/downloads/), volg de onderstaande stappen:
> - Maak een map voor het hosten van uw toepassing.


## <a name="create-your-single-page-applications-ui"></a>De gebruikersinterface van uw toepassing één pagina maken
1.  Maak een *index.html* -bestand voor de SPA JavaScript. Als u Visual Studio gebruikt, selecteert u het project (basismap project), klik met de rechtermuisknop en selecteer: `Add`  >  `New Item`  >  `HTML page` en noem deze index.html
2.  Voeg de volgende code naar de pagina:
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling the page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn to reference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.3/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
