## <a name="test-your-code"></a>Testen van uw code

Als u wilt uitvoeren van uw project in Visual Studio, selecteer **F5**. Uw toepassing **MainWindow** wordt weergegeven, zoals hier wordt weergegeven:

![Uw toepassing testen](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

De eerste keer dat u de toepassing uitvoeren en selecteer de **Microsoft Graph-API aanroepen** knop, wordt u gevraagd aan te melden. Gebruik een Azure Active Directory-account (werk of school-account) of een Microsoft-account (live.com, outlook.com) te testen.

![Aanmelden bij de toepassing](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen
De eerste keer dat u zich aanmeldt bij uw toepassing ook gevraagd om toestemming geven om toe te staan van de toepassing voor toegang tot uw profiel en meld u aan u in, zoals hier wordt weergegeven: 

![Geef uw toestemming voor toegang tot toepassingen](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, ziet u de gebruikersprofielgegevens die wordt geretourneerd door de aanroep van de Microsoft Graph API. De resultaten worden weergegeven in de **API aanroepen resultaten** vak. Algemene informatie over het token dat is verkregen via de aanroep van `AcquireTokenAsync` of `AcquireTokenSilentAsync` moeten worden weergegeven in de **Token Info** vak. De resultaten bevatten de volgende eigenschappen:

|Eigenschap  |Indeling  |Beschrijving |
|---------|---------|---------|
|**Naam** |Volledige naam van gebruiker |De gebruiker eerst de- en achternaam.|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.|
|**Token is verlopen** |Datum en tijd |De tijd waarop het token verloopt. MSAL breidt de verloopdatum te vernieuwen van het token indien nodig.|
|**Toegangstoken** |Tekenreeks |De token tekenreeks die wordt verzonden naar HTTP-aanvragen waarvoor een *autorisatie-header*.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de *user.read* bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd in de Portal van de registratie van de toepassing. Andere voor Microsoft Graph-API's, evenals aangepaste API's voor uw back-end-server mogelijk extra scopes. De Microsoft Graph API vereist de *Calendars.Read* bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de *Calendars.Read* overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de *Calendars.Read* bereik voor de `acquireTokenSilent` aanroepen. 

>[!NOTE]
>De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
