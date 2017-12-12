# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app

Deze handleiding wordt uitgelegd hoe aanmelden bij Microsoft met behulp van een ASP.NET MVC-oplossing met een traditioneel browser gebaseerde webtoepassing met OpenID Connect implementeren. 

Aan het einde van deze handleiding, uw toepassing kan worden aanmelding modules van persoonlijke accounts (inclusief outlook.com, live.com en anderen), evenals werken accepteren en schoolaccounts zijn uit een bedrijf of organisatie die is geïntegreerd met Azure Active Directory. 

> Deze handleiding is Visual Studio 2015 Update 3 of Visual Studio 2017 vereist.  Hebt u geen deze?  [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.png)

Deze handleiding is gebaseerd op het scenario waarbij een browser toegang heeft tot een ASP.NET-website aanvragen van een gebruiker worden geverifieerd via een knop aanmelden. In dit scenario treedt de meeste werk voor het weergeven van de webpagina op aan de serverzijde.

## <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheken:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware waarmee een toepassing wilt OpenIdConnect gebruiken voor verificatie|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware waarmee een toepassing te onderhouden met behulp van cookies gebruikerssessie|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Kunnen worden uitgevoerd op IIS met de ASP.NET-aanvraag voor pipeline-toepassingen op basis van een OWIN|

