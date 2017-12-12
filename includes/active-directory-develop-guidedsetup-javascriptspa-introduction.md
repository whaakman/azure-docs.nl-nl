# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Microsoft Graph API aanroepen vanuit een JavaScript-één pagina toepassing (SPA)

Deze handleiding wordt uitgelegd hoe een JavaScript één pagina toepassing (SPA) persoonlijk, werk kunt aanmelden en schoolaccounts, een toegangstoken ophalen en roept u de Microsoft Graph API of andere API's waarvoor toegangstokens van het Azure Active Directory-v2-eindpunt.

### <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van de voorbeeld-app die is gegenereerd door deze handleiding](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

De voorbeeldtoepassing die is gemaakt door deze handleiding kunt een SPA JavaScript query uitvoeren op de Microsoft Graph API of een Web-API die tokens van Azure Active Directory-v2-eindpunt accepteert. Voor dit scenario, nadat een gebruiker zich aanmeldt, wordt een toegangstoken aangevraagd en toegevoegd aan de HTTP-aanvragen via de autorisatie-header. Token verkrijgen en verlenging worden afgehandeld door de Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheek:

|Bibliotheek|Beschrijving|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft-Verificatiebibliotheek voor JavaScript-Preview|

> [!NOTE]
> *msal.js* doelen de *Azure Active Directory-v2-eindpunt* -waardoor privé, school- als accounts aanmelden en tokens verkrijgen. De *Azure Active Directory-v2-eindpunt* heeft [enkele beperkingen](..\articles\active-directory\develop\active-directory-v2-limitations.md). Als u alleen geïnteresseerd in school- als accounts, gebruikt *adal.js* en de *V1 eindpunt*. Verschillen tussen de v1- en v2-eindpunten lezen leert de [v1-v2 vergelijking](..\articles\active-directory\develop\active-directory-v2-compare.md).

<!--end-collapse-->
