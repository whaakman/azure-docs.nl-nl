
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>De Microsoft Graph-API aanroepen vanuit een iOS-toepassing

Deze handleiding wordt beschreven hoe een systeemeigen iOS-toepassing (Swift) API's waarvoor toegangstokens van het v2.0-eindpunt voor Microsoft Azure Active Directory (Azure AD) kunt aanroepen. De handleiding wordt uitgelegd hoe toegangstokens ophalen en deze gebruiken in aanroepen van de Microsoft Graph-API en andere API's.

Nadat u de oefeningen in deze handleiding hebt voltooid, kunt uw toepassing een beveiligde API aanroepen vanuit een bedrijf of organisatie die Azure AD heeft. Uw toepassing kunt maken van beveiligde API-aanroepen met behulp van persoonlijke accounts, zoals outlook.com, live.com en anderen, evenals accounts werk- of schoolaccount.

## <a name="prerequisites"></a>Vereisten
- XCode versie 8.x is vereist voor het voorbeeld dat is gemaakt in deze handleiding. U kunt downloaden XCode van de [iTunes website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode downloaden URL").
- De [Carthage](https://github.com/Carthage/Carthage) Afhankelijkheidsbeheer is vereist voor pakket-beheer.

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

In deze handleiding kan de voorbeeldtoepassing een iOS-toepassing query uitvoeren op de Microsoft Graph API of een web-API die tokens van het Azure AD v2.0-eindpunt accepteert. In dit scenario voor een token wordt toegevoegd aan de HTTP-aanvragen via de **autorisatie** header. Token verkrijgen en verlenging worden afgehandeld door de Microsoft Authentication Library (MSAL).


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Ingang token verkrijgen voor toegang tot beveiligde web-API 's

Nadat de gebruiker zich verifieert, ontvangt de voorbeeldtoepassing een token. Het token wordt gebruikt voor het zoeken van de Microsoft Graph API of een web-API die wordt beveiligd door het Azure AD v2.0-eindpunt.

API's, zoals Microsoft Graph vereisen een toegangstoken dat toegang tot specifieke bronnen. Tokens zijn vereist voor het lezen van het gebruikersprofiel, toegang tot de agenda van een gebruiker, een e-mailbericht verzenden, enzovoort. Uw toepassing kan een toegangstoken aanvragen door MSAL en geven API scopes. Het toegangstoken is toegevoegd aan de HTTP **autorisatie** -header voor elke aanroep die wordt gemaakt op basis van de beveiligde bron.

MSAL kunt opslaan in cache en vernieuwen toegangstokens voor u, zodat uw toepassing niet te hoeft beheren.


## <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheek:

|Bibliotheek|Beschrijving|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Preview van Microsoft verificatie-bibliotheek voor iOS|

