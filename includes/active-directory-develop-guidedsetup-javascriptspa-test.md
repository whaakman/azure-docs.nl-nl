## <a name="test-your-code"></a>Testen van uw code

### <a name="test-with-visual-studio"></a>Testen met Visual Studio
Als u Visual Studio, drukt u op **F5** aan uw project uitvoeren. De browser geopend met de http://<span></span>localhost: {poort} locatie en u ziet de **Microsoft Graph-API aanroepen** knop.

<p/><!-- --> 

### <a name="test-with-python-or-other-web-server"></a>Met behulp van Python-testomgeving of een andere webserver
Als u Visual Studio niet gebruikt, zorg er dan voor dat de webserver is gestart. Configureer de server om te luisteren naar een TCP-poort die gebaseerd op de locatie van uw **index.html** bestand. Voor Python, start u om te luisteren op poort door het uitvoeren van de terminal opdrachtprompt in de toepassingsmap:
 
```bash
python -m http.server 8080
```
Open de browser en typ http://<span></span>localhost:8080 of http://<span></span>localhost: {poort} waarbij **poort** is de poort die uw webserver luistert. Ziet u de inhoud van uw bestand index.html en de **Microsoft Graph-API aanroepen** knop.

## <a name="test-your-application"></a>Uw toepassing testen

Nadat het bestand index.html is geladen in de browser, selecteer **Microsoft Graph-API aanroepen**. De eerste keer dat u uw toepassing uitvoeren de browser wordt doorgestuurd naar het v2.0-eindpunt voor Microsoft Azure Active Directory (Azure AD) en u wordt gevraagd aan te melden:
 
![Aanmelden bij uw account JavaScript SPA](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen

De eerste keer dat u zich aanmeldt bij uw toepassing, wordt u gevraagd toestemming geven om toe te staan van de toepassing toegang tot uw profiel en u aanmelden:

![Geef uw toestemming voor toegang tot toepassingen](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, ziet u uw profielgegevens in de **Graph API-aanroepen reactie** vak.
 
![Verwachte resultaten vanuit Microsoft Graph API-aanroep](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

U ziet ook basisinformatie over het token dat is verkregen in de **Access Token** en **Token ID-Claims** vakken.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de **user.read** bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie. Andere voor Microsoft Graph-API's, evenals aangepaste API's voor uw back-end-server mogelijk extra scopes. De Microsoft Graph API vereist de **Calendars.Read** bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de **Calendars.Read** overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de **Calendars.Read** bereik voor de **acquireTokenSilent** aanroepen. 

>[!NOTE]
>De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

Als een back-end-API niet vereist een bereik (niet aanbevolen), kunt u de **clientId** als het bereik in de **acquireTokenSilent** en **acquireTokenRedirect** aanroepen.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
