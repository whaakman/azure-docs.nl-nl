
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Microsoft Graph API aanroepen vanuit een Android-app

Deze handleiding wordt uitgelegd hoe een systeemeigen Android-toepassing kunt ophalen van een toegangstoken en bel de Microsoft Graph API of andere API's waarvoor toegangstokens van het Azure Active Directory-v2-eindpunt.

Wanneer u de handleiding hebt voltooid, is uw toepassing worden geaccepteerd aanmeldingen van persoonlijke (waaronder outlook.com, live.com en anderen) en werk- en schoolaccounts accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory. De toepassing wordt vervolgens aanroepen voor een API die wordt beveiligd door het Azure Active Directory-v2-eindpunt.  

## <a name="how-this-sample-works"></a>De werking van dit voorbeeld
![De werking van dit voorbeeld](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

De voorbeeldtoepassing die u met deze handleiding maakt is gebaseerd op een scenario waarbij een Android-toepassing wordt gebruikt voor het zoeken van een Web-API die tokens van het Azure Active Directory-v2-eindpunt (Microsoft Graph API, in dit geval accepteert). Voor dit scenario wordt het token verkregen in uw toepassing toegevoegd op HTTP-aanvragen via de autorisatie-header. De Microsoft Authentication Library (MSAL) verwerkt de token verkrijgen en verlenging voor u.

## <a name="prerequisites"></a>Vereisten
* Deze installatie begeleide is gericht op Android Studio, maar ook alle andere Android-toepassing-ontwikkelomgeving aanvaardbaar is. 
* Android SDK 21 of hoger is vereist (25 SDK wordt aanbevolen).
* Google Chrome of een webbrowser die gebruikmaakt van aangepaste tabbladen is vereist voor deze release van MSAL voor Android.

> [!NOTE]
> Google Chrome is niet opgenomen in Visual Studio-Emulator voor Android. Het is raadzaam dat u deze code op een Emulator met 25 API of een afbeelding met API-21 of hoger met Google Chrome geïnstalleerd testen.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Afhandeling van token verkrijgen voor toegang tot Web-API's beveiligd

Nadat de gebruiker is geverifieerd, ontvangt de voorbeeldtoepassing een toegangstoken dat kan worden gebruikt voor Microsoft Graph API of een Web-API die wordt beveiligd door Azure Active Directory-v2 opvragen.

API's zoals Microsoft Graph vereisen een toegangstoken dat toegang tot specifieke bronnen. Bijvoorbeeld, is een toegangstoken vereist voor het lezen van het gebruikersprofiel, toegang tot de agenda van een gebruiker of per e-mail. Uw toepassing kan een toegangstoken aanvragen met behulp van MSAL voor toegang tot deze bronnen door te geven van de API-scopes. Deze toegangstoken wordt vervolgens toegevoegd aan de HTTP-autorisatie-header voor elke oproep die wordt gemaakt op basis van de beveiligde bron. 

MSAL beheert opslaan in cache en het vernieuwen van de toegangstokens voor u, zodat uw toepassing niet te hoeft.

## <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende bibliotheken:

|Bibliotheek|Beschrijving|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft-Verificatiebibliotheek (MSAL)|
