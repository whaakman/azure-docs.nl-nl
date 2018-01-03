---
title: Azure AD-Android aan de slag | Microsoft Docs
description: Het bouwen van een Android-toepassing die met Azure AD voor aanmelden en Azure AD-aanroepen integreert beveiligd met behulp van OAuth2.0 API's.
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/30/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 619334b3ca65654fd845a62c2fc068156d94d6fc
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="azure-ad-android-getting-started"></a>Azure AD-Android aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Als u een bureaubladtoepassing ontwikkelt, kunt Azure Active Directory (Azure AD) u eenvoudig en snel u uw gebruikers verifiëren met behulp van de lokale Active Directory-accounts. Ook kunt uw toepassing veilig gebruiken voor een web-API die zijn beveiligd door Azure AD, zoals de Office 365-API of de Azure-API.

Voor Android clients die toegang moeten krijgen tot beveiligde bronnen, levert Azure AD de Active Directory Authentication Library (ADAL). Het enige doel van ADAL is gemakkelijker voor uw app toegangstokens ophalen. Om te demonstreren hoe eenvoudig het is, moet we die een takenlijst Android-toepassing bouwen:

* Krijgt toegang tot tokens voor een taak lijst-API aanroept met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* De takenlijst van een gebruiker opgehaald.
* Tekenen van gebruikers.

Om te beginnen, moet u een Azure AD-tenant kunt u gebruikers maken en een toepassing registreren. Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Stap 1: Downloaden en uitvoeren van de server Node.js REST-API TODO-voorbeeld
Het voorbeeld voor Node.js REST-API TODO wordt geschreven om specifiek te kunnen werken met onze huidige voorbeeld voor het bouwen van een één-tenant taak REST-API voor Azure AD. Dit is een vereiste voor snel starten.

Zie voor informatie over hoe dit onze bestaande voorbeelden in [Microsoft Azure Active Directory-voorbeeld REST API-Service voor Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Stap 2: Uw web-API registreren bij uw Azure AD-tenant
Active Directory ondersteunt twee soorten toepassingen toe te voegen:

- Web-API's die services aan gebruikers bieden
- Toepassingen (die worden uitgevoerd op het web of op een apparaat) die toegang hebben tot de web-API 's

In deze stap kunt u de web-API die u lokaal worden uitgevoerd voor het testen van dit voorbeeld wordt geregistreerd. Deze web-API is normaal gesproken een REST-service die wordt functionaliteit die u wilt dat een app om toegang te bieden. Azure AD kunt een willekeurig eindpunt beschermen.

We ervan uit dat u bent u registreert de waarnaar wordt verwezen eerder TODO REST-API. Maar dit werkt voor alle web-API die u wilt dat Azure Active Directory om te beschermen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. In de **Directory** kiest u de Azure AD-tenant waar u uw toepassing registreren.
3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Een beschrijvende naam voor de toepassing (bijvoorbeeld **TodoListService**), selecteer **webtoepassing en/of Web-API**, en klik op **volgende**.
6. Voer de basis-URL voor het voorbeeld voor de URL eenmalige aanmelding. Dit is standaard `https://localhost:8080`.
7. Klik op **OK** om de inschrijving te voltooien.
8. Terwijl u nog steeds in de Azure portal, Ga naar de toepassingspagina, de waarde van de toepassing-ID vinden en kopiëren. U hebt dit later nodig bij het configureren van uw toepassing.
9. Van de **instellingen** -> **eigenschappen** pagina, de app ID URI bijwerken - Voer `https://<your_tenant_name>/TodoListService`. Vervang `<your_tenant_name>` met de naam van uw Azure AD-tenant.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Stap 3: De voorbeeldtoepassing Android Native Client registreren
In dit voorbeeld moet u uw webtoepassing registreren. Hierdoor kan de toepassing om te communiceren met de zojuist hebt geregistreerd web-API. Azure AD weigert zelfs zodat uw toepassing te vragen voor aanmelden, tenzij deze is geregistreerd. Die uitmaakt deel van de beveiliging van het model.

We ervan uit dat u bent u registreert de voorbeeldtoepassing waarnaar wordt verwezen eerder. Maar deze procedure werkt voor elke app die u ontwikkelt.

> [!NOTE]
> U vraagt zich misschien af waarom u een toepassing en een web-API wilt opslaan in een tenant. Als u mogelijk hebt geraden, kunt u een app die toegang heeft tot een externe API die is geregistreerd in Azure AD van een andere tenant. Als u dat doet, wordt uw klanten wordt gevraagd om toestemming voor het gebruik van de API in de toepassing. Active Directory Authentication Library voor iOS zorgt voor deze toestemming voor u. Als we geavanceerdere functies, verkennen, ziet u dat dit een belangrijk onderdeel van het werk dat nodig is is voor toegang tot de suite van Microsoft APIs van Azure en Office, evenals andere serviceprovider. Op dit moment omdat u zowel uw web-API en uw toepassing onder dezelfde tenant geregistreerd ziet u niet om toestemming vragen. Dit is meestal het geval als u bij het ontwikkelen van een toepassing alleen voor uw eigen bedrijf te gebruiken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. In de **Directory** kiest u de Azure AD-tenant waar u uw toepassing registreren.
3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Een beschrijvende naam voor de toepassing (bijvoorbeeld **TodoListClient Android**), selecteer **systeemeigen clienttoepassing**, en klik op **volgende**.
6. Voer voor de omleidings-URI `http://TodoListClient`. Klik op **Voltooien**.
7. De waarde van de toepassing-ID vinden en kopieer het via de toepassingspagina. U hebt dit later nodig bij het configureren van uw toepassing.
8. Van de **instellingen** pagina **Required Permissions** en selecteer **toevoegen**.  Zoek en selecteer TodoListService, toevoegen de **toegang TodoListService** machtiging onder **gedelegeerde machtigingen**, en klik op **gedaan**.

Als u wilt maken met Maven, kunt u pom.xml op het hoogste niveau:

1. Deze opslagplaats klonen naar een map van uw keuze:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Volg de stappen in de [vereisten voor het instellen van uw omgeving Maven voor Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android).
3. Instellen van de SDK-19-emulator.
4. Ga naar de hoofdmap waar u de opslagplaats.
5. Deze opdracht uitvoeren:`mvn clean install`
6. Wijzig de directory in het voorbeeld snel starten:`cd samples\hello`
7. Deze opdracht uitvoeren:`mvn android:deploy android:run`

   U ziet dat de app wordt gestart.
8. Voer de referenties van gebruiker testen om te proberen.

Naast het pakket AAR worden JAR-pakketten verzonden.

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Stap 4: Download de Android ADAL en toe te voegen aan uw Eclipse-werkruimte
We hebben aangebracht u hebt meerdere mogelijkheden voor het gebruik van ADAL in uw Android-project eenvoudig:

* U kunt de broncode voor het importeren van deze bibliotheek in Eclipse en een koppeling naar uw toepassing gebruiken.
* Als u Android Studio, kunt u gebruik van de indeling van het pakket AAR en verwijzen naar de binaire bestanden.

### <a name="option-1-source-zip"></a>Optie 1: Bron Zip
Een kopie van de broncode downloaden, klikt u op **ZIP downloaden** aan de rechterkant van de pagina. U kunt [downloaden vanuit GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>Optie 2: Bron via Git
Als u de broncode van de SDK via Git, typt u:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Optie 3: Binaire bestanden via Gradle
U kunt de binaire bestanden ophalen uit de centrale opslagplaats met Maven. Het pakket AAR kan als volgt worden opgenomen in uw project in Android Studio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Optie 4: AAR via Maven
Als u de invoegtoepassing M2Eclipse gebruikt, kunt u de afhankelijkheid in uw pom.xml-bestand opgeven:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a>Optie 5: JAR-pakket in de map libs
U kunt het JAR-bestand ophalen uit de opslagplaats met Maven en neer in de **bibliotheken** map in uw project. U moet de vereiste resources aan uw project, evenals kopiëren omdat de JAR-pakketten niet opnemen.

## <a name="step-5-add-references-to-android-adal-to-your-project"></a>Stap 5: Verwijzingen naar Android ADAL toevoegen aan uw project
1. Voeg een verwijzing naar uw project en dit opgeven als een Android-bibliotheek. Als u niet zeker hoe u dit doet, krijgt u meer informatie over de [Android Studio site](http://developer.android.com/tools/projects/projects-eclipse.html).
2. De project-afhankelijkheid voor foutopsporing in de instellingen van uw project toevoegen.
3. Bijwerken van uw project AndroidManifest.xml dat moet worden opgenomen:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Een exemplaar van AuthenticationContext op uw belangrijkste activiteit maken. De details van deze aanroep zijn buiten het bereik van dit onderwerp, maar u kunt een goede start ophalen door te kijken de [Android Native Client voorbeeld](https://github.com/AzureADSamples/NativeClient-Android). In het volgende voorbeeld is SharedPreferences de standaardcache en instantie is in de vorm van `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Kopieer dit codeblok voor het afhandelen van het einde van AuthenticationActivity nadat de gebruiker referenties invoert en een autorisatiecode ontvangt:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. Als u vragen voor een token, definieert u een retouraanroep:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Ten slotte vragen voor een token met behulp van terugbellen:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Hier volgt een uitleg van de parameters:

* *resource* is vereist en de bron die u probeert te openen.
* *clientid* is vereist en afkomstig zijn van Azure AD.
* *RedirectUri* is niet vereist voor de aanroep acquireToken worden opgegeven. U kunt instellen deze als de pakketnaam van uw.
* *PromptBehavior* helpt vragen om referenties voor het overslaan van de cache en de cookie.
* *callback* wordt aangeroepen nadat de autorisatiecode worden uitgewisseld voor een token. Heeft een object van het AuthenticationResult waarvoor toegangstoken, datum verlopen en ID token-informatie.
* *acquireTokenSilent* is optioneel. U kunt deze aanroepen om te verwerken, opslaan in cache en token vernieuwen. Het bevat ook de synchronisatieversie. De cmdlet accepteert *userId* als parameter.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Met behulp van dit scenario hebt u wat u moet is geïntegreerd met Azure Active Directory. Voor meer voorbeelden van deze werken, gaat u naar de AzureADSamples / opslagplaats op GitHub.

## <a name="important-information"></a>Belangrijke informatie
### <a name="customization"></a>Aanpassing
Uw toepassingsresources kunnen project bibliotheekresources overschrijven. Dit gebeurt wanneer uw app wordt samengesteld. Daarom kunt u verificatie activiteit indeling zoals die u wilt aanpassen. Zorg ervoor dat de ID van de besturingselementen dat gebruikmaakt van ADAL (webweergave).

### <a name="broker"></a>Broker
De Microsoft Intune-bedrijfsportal-app biedt het broker-onderdeel. Het account wordt gemaakt in AccountManager. Het accounttype is 'com.microsoft.workaccount'. AccountManager kan slechts één SSO-account. Een SSO-cookie voor de gebruiker wordt gemaakt na het voltooien van de uitdaging van het apparaat voor een van de apps.

ADAL maakt gebruik van het broker-account als een gebruikersaccount is gemaakt op deze verificator en u wilt niet overslaan. U kunt de broker-gebruiker met overslaan:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

U moet een speciale RedirectUri voor het gebruik van de broker registreren. RedirectUri bevindt zich in de indeling van `msauth://packagename/Base64UrlencodedSignature`. U kunt uw RedirectUri ophalen voor uw app met behulp van het script brokerRedirectPrint.ps1 of de API-aanroep mContext.getBrokerRedirectUri. De handtekening is gerelateerd aan het ondertekenen van certificaten.

Het huidige model van de broker is voor één gebruiker. AuthenticationContext biedt de API-methode voor het ophalen van de broker-gebruiker.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

Uw app-manifest moet de volgende machtigingen hebben voor AccountManager accounts gebruiken. Zie voor meer informatie de [AccountManager informatie over de Android-site](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>Instantie-URL en AD FS
Active Directory Federation Services (AD FS) is niet herkend als productie STS, dus u moet voor het inschakelen van exemplaar van detectie en geef ' false ' in de constructor AuthenticationContext.

De URL van de instantie moet een STS-exemplaar en een [tenantnaam](https://login.microsoftonline.com/yourtenant.onmicrosoft.com).

### <a name="querying-cache-items"></a>Opvragen van cache-items
ADAL biedt een standaardcache in SharedPreferences met enkele eenvoudige cache queryfuncties. U kunt de huidige cache van AuthenticationContext krijgen met behulp van:

    ITokenCacheStore cache = mContext.getCache();

Als u wilt aanpassen, kunt u uw implementatie cache opgeven.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Waarschuwing
ADAL biedt een optie om aan te vragen gedrag opgeven. PromptBehavior.Auto wordt de gebruikersinterface weergegeven als het vernieuwingstoken dat ongeldig is en gebruikersreferenties vereist zijn. PromptBehavior.Always wordt het gebruik van de cache overslaan en altijd de UI niet weergeven.

### <a name="silent-token-request-from-cache-and-refresh"></a>Achtergrond tokenaanvraag uit de cache en vernieuwen
Een achtergrond tokenaanvraag maakt geen gebruik van de pop-gebruikersinterface en vereist geen een activiteit. Retourneert een token uit de cache beschikbaar. Als het token is verlopen, wordt deze methode probeert te vernieuwen. Als het vernieuwingstoken dat is verlopen of is mislukt, wordt er AuthenticationException.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

U kunt een synchronisatie met deze methode aanroepen. U kunt naar callback null instellen of acquireTokenSilentSync gebruiken.

### <a name="diagnostics"></a>Diagnostiek
Dit zijn de primaire bronnen met informatie voor het oplossen van problemen:

* Uitzonderingen
* Logboeken
* Netwerktracering

Houd er rekening mee dat de correlatie-id's centraal staat in de diagnostische gegevens in de bibliotheek zijn. U kunt instellen dat uw correlatie id's op basis van per aanvraag als u wilt een ADAL correleren met andere bewerkingen zijn in uw code aanvragen. Als u een correlatie-ID niet instelt, wordt de ADAL een willekeurige token genereren. Alle berichten logboekbestanden en netwerk aanroepen wordt vervolgens wordt voorzien van de correlatie-ID. De automatisch gegenereerde ID wijzigingen bij elke aanvraag.

#### <a name="exceptions"></a>Uitzonderingen
Uitzonderingen zijn de eerste diagnose. We proberen te bieden handige foutberichten. Als u een die niet handig vinden, Controleer het bestand een probleem en laat ons weten. Apparaatgegevens zoals model en SDK getal bevatten.

#### <a name="logs"></a>Logboeken
U kunt de bibliotheek voor het genereren van logboekberichten die u gebruiken kunt om u te helpen bij het analyseren van problemen. U kunt logboekregistratie configureren door het maken van de volgende oproep verzenden voor het configureren van een callback die ADAL aan de hand uit elk logboekbericht gebruiken zoals deze wordt gegenereerd.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this to log file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Berichten kunnen worden geschreven naar een aangepaste logboekbestand, zoals wordt weergegeven in de volgende code. Er is geen standaardmethode voor het ophalen van Logboeken vanaf een apparaat. Er zijn bepaalde services waarmee u kunnen met deze. U kunt ook uw eigen, zoals het verzenden van het bestand naar een server voor voorraad.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Dit zijn de registratieniveaus:
* Fout (uitzonderingen)
* Waarschuwen (waarschuwing)
* Info (doelen)
* Uitgebreid (meer informatie)

U instellen het logboekniveau als volgt:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Alle berichten in het logboek zijn verzonden naar logcat, naast eventuele aangepaste logboek retouraanroepen.
U kunt krijgen een logboek naar een bestand uit logcat als volgt:

    adb logcat > "C:\logmsg\logfile.txt"

 Zie voor meer informatie over adb opdrachten de [logcat informatie over de Android-site](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Netwerktracering
U kunt verschillende hulpprogramma's gebruiken om vast te leggen van de HTTP-verkeer dat ADAL genereert.  Dit is vooral handig als u bekend met het OAuth-protocol bent of als u nodig hebt om Microsoft of andere ondersteuningskanalen diagnostische informatie te geven.

Fiddler is de eenvoudigste hulpmiddel voor het traceren van HTTP. Gebruik de volgende koppelingen in te stellen tot correct record ADAL-netwerkverkeer. Voor een hulpprogramma zoals Fiddler of Jeroen nuttig voor tracering, moet u configureren om vast te leggen van niet-versleutelde SSL-verkeer.  

> [!NOTE]
> Traceringen gegenereerd op deze manier kunnen zeer vertrouwelijke informatie zoals toegangstokens, gebruikersnamen en wachtwoorden bevatten. Als u van productie-accounts gebruikmaakt, deze traceringen niet delen met derden. Als u nodig hebt om op te geven van een tracering voor iemand om ondersteuning krijgen, reproduceer het probleem met behulp van een tijdelijke rekening met gebruikersnamen en wachtwoorden op dat u geen rekening met het delen.

* Van de website Telerik: [instelling Up Fiddler voor Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* Vanuit GitHub: [Fiddler regels configureren voor ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Dialoogvenster modus
De methode acquireToken zonder activiteit ondersteunt een dialoogvenster.

### <a name="encryption"></a>Versleuteling
ADAL versleutelt de tokens en opslaan in SharedPreferences standaard. U kunt zoeken op de klasse StorageHelper om de details te bekijken. Android Keystore android geïntroduceerd voor 4.3 (API 18) veilige opslag van persoonlijke sleutels. ADAL gebruikt voor API 18 en hoger. Als u gebruikmaken van ADAL voor lagere SDK-versies wilt, moet u een geheime sleutel op AuthenticationSettings.INSTANCE.setSecretKey bieden.

### <a name="oauth2-bearer-challenge"></a>OAuth2 bearer uitdaging
De klasse AuthenticationParameters biedt functionaliteit om authorization_uri ophalen uit de OAuth2 bearer-uitdaging.

### <a name="session-cookies-in-webview"></a>Sessiecookies in webweergave
Android webweergave wist niet sessiecookies nadat de app is gesloten. U kunt die verwerkt met behulp van deze voorbeeldcode:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Zie voor meer informatie over cookies de [CookieSyncManager informatie over de Android-site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Resource-onderdrukkingen
De ADAL-bibliotheek bevat Engelse tekenreeksen voor de ProgressDialog berichten. Uw toepassing moet ze als u gelokaliseerde tekenreeksen wilt overschrijven.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>Dialoogvenster NTLM
ADAL-versie 1.1.0 ondersteunt een NTLM-dialoogvenster dat wordt verwerkt door de gebeurtenis onReceivedHttpAuthRequest van WebViewClient. U kunt de tekenreeksen voor het dialoogvenster met de indeling en aanpassen.

### <a name="cross-app-sso"></a>Cross-app eenmalige aanmelding
Meer informatie over [het inschakelen van eenmalige aanmelding voor cross-app voor Android met behulp van ADAL](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
