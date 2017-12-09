
## <a name="test-your-code"></a>Testen van uw code

1. Implementeer uw code op uw apparaat/emulator.
2. Wanneer u klaar bent om uw toepassing te testen, gebruikt u een Microsoft Azure Active Directory-account (werk of school-account) of een Microsoft-account (live.com, outlook.com) aan te melden. 

    ![Uw toepassing testen](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Gebruikersnaam en wachtwoord invoeren](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen
De eerste keer dat u zich aanmeldt bij uw toepassing, wordt u gevraagd toestemming geven om toe te staan van de toepassing toegang tot uw profiel en u aanmelden: 

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, ziet u de resultaten die zijn geretourneerd door de aanroep van de Microsoft Graph API. De aanroep van de Microsoft Graph API **mij** eindpunt het profiel gebruiker https://graph.microsoft.com/v1.0/me retourneert. Zie voor een lijst met algemene Microsoft Graph-eindpunten [Microsoft Graph API-documentatie voor ontwikkelaars](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de **user.read** bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie. Andere voor Microsoft Graph-API's, evenals aangepaste API's voor uw back-end-server mogelijk extra scopes. De Microsoft Graph API vereist de **Calendars.Read** bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de **Calendars.Read** overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de **Calendars.Read** bereik voor de **acquireTokenSilent** aanroepen. 

>[!NOTE]
>De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

<!--end-collapse-->

[!INCLUDE  [Help and support](active-directory-develop-help-support-include.md)]
