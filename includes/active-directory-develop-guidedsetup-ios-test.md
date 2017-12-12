## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Opvragen van de Microsoft Graph-API van uw iOS-toepassing testen

Als u wilt de code in de simulator uitvoert, drukt u op **opdracht** + **R**.

![Test uw toepassing in de simulator](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Wanneer u klaar bent om te testen, selecteert u **Microsoft Graph-API aanroepen**. Wanneer u wordt gevraagd, typt u uw gebruikersnaam en wachtwoord.

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen
De eerste keer dat u zich aanmeldt bij uw toepassing, wordt u gevraagd toestemming geven om toe te staan van de toepassing toegang tot uw profiel en u aanmelden:

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, ziet u uw gebruikersprofielgegevens geretourneerd door de Microsoft Graph API-aanroep in de **logboekregistratie** sectie. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de **user.read** bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie. Andere voor Microsoft Graph-API's, evenals aangepaste API's voor uw back-end-server mogelijk extra scopes. De Microsoft Graph API vereist de **Calendars.Read** bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de **Calendars.Read** overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de **Calendars.Read** bereik voor de **acquireTokenSilent** aanroepen. 

>[!NOTE]
>De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
