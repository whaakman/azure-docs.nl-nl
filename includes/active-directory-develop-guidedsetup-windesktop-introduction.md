# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Microsoft Graph API aanroepen vanuit een Windows-bureaublad-app

Deze handleiding wordt uitgelegd hoe een systeemeigen Windows Desktop .NET (XAML)-toepassing kunt ophalen van een toegangstoken en bel de Microsoft Graph API of andere API's waarvoor toegangstokens van een Azure Active Directory-v2-eindpunt.

Wanneer u de handleiding hebt voltooid, zich de toepassing een beveiligde API die gebruikmaakt van persoonlijke accounts (inclusief outlook.com, live.com en anderen) aan te roepen. De toepassing wordt ook werk en schoolaccounts zijn uit een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.  

> [!NOTE] 
> De handleiding voor vereist Visual Studio 2015 Update 3 of Visual Studio 2017.  Geen een van deze versies? [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/).

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

De voorbeeldtoepassing die u met deze handleiding maakt kunt een Windows-bureaublad toepassing waarmee een query op de Microsoft Graph API of een Web-API die tokens van een Azure Active Directory-v2-eindpunt accepteert. In dit scenario kunt u een token op HTTP-aanvragen via de autorisatie-header toevoegen. Microsoft Authentication Library (MSAL) verwerkt token verkrijgen en verlenging.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Afhandeling van token verkrijgen voor toegang tot Web-API's beveiligd

Nadat de gebruiker is geverifieerd, ontvangt de voorbeeldtoepassing een token dat kan worden gebruikt voor Microsoft Graph API of een Web-API die wordt beveiligd door Azure Active Directory-v2 opvragen.

API's zoals Microsoft Graph vereisen een token dat toegang tot specifieke bronnen. Bijvoorbeeld, is een token vereist voor het lezen van het gebruikersprofiel, toegang tot de agenda van een gebruiker of per e-mail. Uw toepassing kan een toegangstoken aanvragen met behulp van MSAL voor toegang tot deze bronnen door te geven van de API-scopes. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke oproep die wordt gemaakt op basis van de beveiligde bron. 

MSAL beheert opslaan in cache en het vernieuwen van de toegangstokens voor u, zodat uw toepassing niet te hoeft.

## <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding bevat de volgende NuGet-pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft-Verificatiebibliotheek (MSAL)|

