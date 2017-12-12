## <a name="test-your-code"></a>Testen van uw code

Testen van uw toepassing in Visual Studio, drukt u op **F5** aan uw project uitvoeren. De **MainWindow** voor uw toepassing wordt weergegeven:

![Uw toepassing testen](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Wanneer u klaar bent om uit te voeren van uw test, gebruikt u een Microsoft Azure Active Directory-account (werk of school-account) of een Microsoft-account (live.com, outlook.com) aan te melden. De eerste keer dat u de toepassing uitvoeren die u wordt gevraagd aan te melden:

![Aanmelden bij de toepassing](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Geef toestemming voor toegang tot toepassingen
De eerste keer dat u zich aanmeldt bij uw toepassing, wordt u gevraagd toestemming geven om toe te staan van de toepassing toegang tot uw profiel en u aanmelden: 

![Geef uw toestemming voor toegang tot toepassingen](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, ziet u de gebruikersprofielgegevens geretourneerd door de aanroep van de Microsoft Graph API. De resultaten worden weergegeven in de **API aanroepen resultaten** vak. Algemene informatie over het token dat is verkregen via de aanroep van **AcquireTokenAsync** of **AcquireTokenSilentAsync** moeten worden weergegeven in de **Token Info** vak. De resultaten bevatten de volgende eigenschappen:

|Eigenschap  |Indeling  |Beschrijving |
|---------|---------|---------|
|**Naam** |Volledige naam van gebruiker |De gebruiker eerst de- en achternaam.|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.|
|**Token is verlopen** |Datum en tijd |De tijd waarop het token verloopt. MSAL breidt de verloopdatum te vernieuwen van het token indien nodig.|
|**Toegangstoken** |Reeks |De token tekenreeks die wordt verzonden naar HTTP-aanvragen waarvoor een **autorisatie** header.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de **user.read** bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd op de portal voor wachtwoordregistratie. Andere voor Microsoft Graph-API's, evenals aangepaste API's voor uw back-end-server mogelijk extra scopes. De Microsoft Graph API vereist de **Calendars.Read** bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de **Calendars.Read** overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de **Calendars.Read** bereik voor de **acquireTokenSilent** aanroepen. 

>[!NOTE]
>De gebruiker kan gevraagd om extra toestemmingen als u het aantal scopes verhogen.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
