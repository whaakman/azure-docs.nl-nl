## <a name="test-your-code"></a>Testen van uw code

Testen van uw toepassing in Visual Studio, drukt u op **F5** aan uw project uitvoeren. De browser geopend met de http://<span></span>localhost: {poort} locatie en u ziet de **aanmelden met Microsoft** knop. Selecteer de knop om het proces aanmelden te starten.

Wanneer u klaar bent om uit te voeren van uw test, gebruiken een Microsoft Azure Active Directory (Azure AD)-account (werk of school-account) of een persoonlijk Microsoft-account (<span>live.</span> COM of <span>outlook.</span> com) aan te melden.

![Meld u aan met Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Aanmelden bij je Microsoft-account](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Resultaten van de toepassing weergeven
Nadat u zich aanmeldt, wordt de gebruiker wordt omgeleid naar de startpagina van uw website. De startpagina van is de HTTPS-URL die is opgegeven in uw toepassing registratie-informatie in de Registratieportal voor Microsoft-toepassing. De introductiepagina bevat een welkomstbericht ' Hello \<gebruiker >, ' een koppeling om af te melden en een koppeling om de claims van de gebruiker weer te geven. De koppeling voor de gebruiker claims bladert naar de **autoriseren** domeincontroller die u eerder hebt gemaakt.

### <a name="browse-to-see-the-users-claims"></a>Ga naar de claims van de gebruiker
Als u wilt zien van de gebruiker claims, selecteer de koppeling om te bladeren naar de weergave van de domeincontroller die is alleen beschikbaar voor geverifieerde gebruikers.

#### <a name="view-the-claims-results"></a>Bekijk de resultaten van claims
Nadat u de weergave controller bladert, ziet u een tabel met de belangrijkste eigenschappen voor de gebruiker:

|Eigenschap |Waarde |Beschrijving |
|---|---|---|
|**Naam** |Volledige naam van gebruiker | De gebruiker eerst de- en achternaam.
|**Gebruikersnaam** |gebruiker<span>@domain.com</span> | De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.
|**Onderwerp** |Onderwerp |Een tekenreeks die een unieke identificatie van de gebruiker op het web.|
|**Tenant-ID** |GUID | Een **guid** die Azure AD-organisatie van de gebruiker een unieke vertegenwoordigt.|

Bovendien ziet u een tabel van alle claims die zich in de verificatieaanvraag. Zie voor meer informatie de [lijst met claims die zich in een Azure AD-ID Token](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Test de toegang tot een methode die een kenmerk autoriseren (optioneel)
Voor het testen van toegang tot de **autoriseren** controller voor claims van de gebruiker als anonieme gebruiker, als volgt te werk:
1. Selecteer de koppeling naar de gebruiker afmelden en de afmelden proces te voltooien.
2. Typ in uw browser http://<span></span>localhost: {poort} / geverifieerde toegang tot uw domeincontroller die is beveiligd met de **autoriseren** kenmerk.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Verwachte resultaten na toegang tot een beveiligde domeincontroller
U wordt gevraagd om te verifiëren voor de weergave beveiligde domeincontroller gebruiken.

## <a name="additional-information"></a>Aanvullende informatie

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Beveiligen van uw gehele website
Beveiligen van uw gehele website in de **Global.asax** bestand, het toevoegen van de **AuthorizeAttribute** kenmerk de **GlobalFilters** filteren de **toepassing _Starten** methode:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-sign-in-access-to-your-application"></a>-Toegang tot uw toepassing beperken
Standaard persoonlijke accounts, zoals outlook.com, live.com en anderen zich kunnen aanmelden bij uw toepassing. Werk- of schoolaccount accounts in organisaties die zijn geïntegreerd met Azure AD kunnen zich ook aanmelden standaard.

Gebruiker aanmelden om toegang te beperken voor uw toepassing, zijn diverse opties beschikbaar.

#### <a name="restrict-access-to-a-single-organization"></a>Beperk de toegang tot een enkele organisatie
U kunt aanmelden toegang beperken voor uw toepassing kan alleen de gebruikersaccounts die zich in een enkele organisatie met Azure AD:
1. In de **web.config** bestand, wijzig de waarde voor de **Tenant** parameter. Wijzig de waarde van **algemene** naar de tenantnaam van de organisatie, zoals **contoso.onmicrosoft.com**.
2. In uw **OWIN opstarten** klasse, stelt u de **ValidateIssuer** argument voor **true**.

#### <a name="restrict-access-to-a-list-of-organizations"></a>Toegang tot een lijst met organisaties beperken
U kunt-toegang beperken tot alleen gebruikersaccounts die in een Azure AD-organisatie die zich in de lijst met toegestane organisaties zijn:
1. In de **web.config** bestand in uw **OWIN opstarten** klasse, stelt u de **ValidateIssuer** argument voor **true**.
2. Stel de waarde van de **ValidIssuers** parameter aan de lijst met toegestane organisaties.

#### <a name="use-a-custom-method-to-validate-issuers"></a>Gebruik een aangepaste methode voor het valideren van uitgevers van certificaten
U kunt implementeren een aangepaste methode voor het valideren van uitgevers van certificaten met behulp van de **IssuerValidator** parameter. Lees voor meer informatie over het gebruik van deze parameter over de [TokenValidationParameters klasse](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) op MSDN.

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
