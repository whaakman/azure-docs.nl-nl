---
title: Azure AD-Android aan de slag | Microsoft Docs
description: Het bouwen van een Android-toepassing die met Azure AD voor aanmelden en Azure AD-aanroepen integreert beveiligd met behulp van OAuth2.0 API's.
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/30/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 25a908c542bf8fdd8008841a1865cdfb40d847fc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-ad-android-getting-started"></a>Azure AD-Android aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Als u een bureaubladtoepassing ontwikkelt, kunt Azure Active Directory (Azure AD) u eenvoudig en snel u uw gebruikers verifiëren met behulp van de lokale Active Directory-accounts. Ook kunt uw toepassing veilig gebruiken voor een web-API die zijn beveiligd door Azure AD, zoals de Office 365-API of de Azure-API.

Voor Android clients die toegang moeten krijgen tot beveiligde bronnen, levert Azure AD de Active Directory Authentication Library (ADAL). Het enige doel van ADAL is gemakkelijker voor uw app toegangstokens ophalen. Om te demonstreren hoe eenvoudig het is, moet we die een takenlijst Android-toepassing bouwen:

* Krijgt toegang tot tokens voor het aanroepen van een taak lijst API met [OAuth 2.0-verificatieprotocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
* De takenlijst van een gebruiker opgehaald.
* Tekenen van gebruikers.

Om te beginnen, moet u een Azure AD-tenant kunt u gebruikers maken en een toepassing registreren. Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Stap 1: Downloaden en uitvoeren van de server Node.js REST-API TODO-voorbeeld
Het voorbeeld voor Node.js REST-API TODO wordt geschreven om specifiek te kunnen werken met onze huidige voorbeeld voor het bouwen van een één-tenant taak REST-API voor Azure AD. Dit is een vereiste voor snel starten.

Zie voor informatie over hoe dit onze bestaande voorbeelden in [Azure Active Directory-voorbeeld REST API-Service voor Node.js](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Stap 2: Uw web-API registreren bij uw Azure AD-tenant
Active Directory ondersteunt twee soorten toepassingen toe te voegen:

- Web-API's die services aan gebruikers bieden
- Toepassingen (die worden uitgevoerd op het web of op een apparaat) die toegang hebben tot de web-API 's

In deze stap kunt u de web-API die u lokaal worden uitgevoerd voor het testen van dit voorbeeld wordt geregistreerd. Deze web-API is normaal gesproken een REST-service die wordt functionaliteit die u wilt dat een app om toegang te bieden. Azure AD kunt een willekeurig eindpunt beschermen.

We ervan uit dat u bent u registreert de waarnaar wordt verwezen eerder TODO REST-API. Maar dit werkt voor alle web-API die u wilt dat Azure Active Directory om te beschermen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. In de **Directory** kiest u de Azure AD-tenant waar u uw toepassing registreren.
3. Klik op **alle services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
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
3. Klik op **alle services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Een beschrijvende naam voor de toepassing (bijvoorbeeld **TodoListClient Android**), selecteer **systeemeigen clienttoepassing**, en klik op **volgende**.
6. Voer voor de omleidings-URI `http://TodoListClient`. Klik op **Voltooien**.
7. De waarde van de toepassing-ID vinden en kopieer het via de toepassingspagina. U hebt dit later nodig bij het configureren van uw toepassing.
8. Van de **instellingen** pagina **Required Permissions** en selecteer **toevoegen**.  Zoek en selecteer TodoListService, toevoegen de **toegang TodoListService** machtiging onder **gedelegeerde machtigingen**, en klik op **gedaan**.

Als u wilt maken met Maven, kunt u pom.xml op het hoogste niveau:

1. Deze opslagplaats klonen naar een map van uw keuze:

  `$ git clone https://github.com/Azure-Samples/active-directory-android.git`  
2. Volg de stappen in de [vereisten voor het instellen van uw omgeving Maven voor Android](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Maven).
3. Instellen van de SDK-19-emulator.
4. Ga naar de hoofdmap waar u de opslagplaats.
5. Deze opdracht uitvoeren: `mvn clean install`
6. Wijzig de directory in het voorbeeld snel starten: `cd samples\hello`
7. Deze opdracht uitvoeren: `mvn android:deploy android:run`

   U ziet dat de app wordt gestart.
8. Voer de referenties van gebruiker testen om te proberen.

Naast het pakket AAR worden JAR-pakketten verzonden.

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Stap 4: Download de Android ADAL en toe te voegen aan uw Eclipse-werkruimte
We hebben aangebracht u hebt meerdere mogelijkheden voor het gebruik van ADAL in uw Android-project eenvoudig:

* U kunt de broncode voor het importeren van deze bibliotheek in Eclipse en een koppeling naar uw toepassing gebruiken.
* Als u Android Studio, kunt u gebruik van de indeling van het pakket AAR en verwijzen naar de binaire bestanden.

### <a name="option-1-source-zip"></a>Optie 1: Bron Zip
Een kopie van de broncode downloaden, klikt u op **ZIP downloaden** aan de rechterkant van de pagina. U kunt [downloaden vanuit GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android/releases).

### <a name="option-2-source-via-git"></a>Optie 2: Bron via Git
Als u de broncode van de SDK via Git, typt u:

    git clone https://github.com/AzureAD/azure-activedirectory-library-for-android.git
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

4. Een exemplaar van AuthenticationContext op uw belangrijkste activiteit maken. De details van deze aanroep zijn buiten het bereik van dit onderwerp, maar u kunt een goede start ophalen door te kijken de [Android Native Client voorbeeld](https://github.com/AzureAD/azure-activedirectory-library-for-android). In het volgende voorbeeld is SharedPreferences de standaardcache en instantie is in de vorm van `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`:

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

### <a name="broker"></a>Broker
De Intune-bedrijfsportal of Microsoft Authenticator-app biedt het broker-onderdeel. Het account wordt gemaakt in AccountManager. Het accounttype is 'com.microsoft.workaccount'. AccountManager kan slechts één SSO-account. Een SSO-cookie voor de gebruiker wordt gemaakt na het voltooien van de uitdaging van het apparaat voor een van de apps.

Voor meer informatie over het configureren met behulp van een broker, bekijk de [broker wiki-artikel](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Broker). 

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

#### <a name="errors--exceptions"></a>Fouten en uitzonderingen
Uitzonderingen zijn de eerste diagnose. We proberen te bieden handige foutberichten. Als u een die niet handig vinden, Controleer het bestand een probleem en laat ons weten. Apparaatgegevens zoals model en SDK getal bevatten.

Voor meer informatie over welke fouten uw app moet verwerken, bekijk de [aanbevolen procedures voor foutafhandeling](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-adal-error-handling). 

#### <a name="logs"></a>Logboeken
U kunt de bibliotheek voor het genereren van logboekberichten die u gebruiken kunt om u te helpen bij het analyseren van problemen. U kunt logboekregistratie configureren door het maken van de volgende oproep verzenden voor het configureren van een callback die ADAL aan de hand uit elk logboekbericht gebruiken zoals deze wordt gegenereerd.

Inschakelen van logboekregistratie, bekijk de [logboekregistratie wiki-artikel](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Logging).

### <a name="session-cookies-in-webview"></a>Sessiecookies in webweergave
Android webweergave wist niet sessiecookies nadat de app is gesloten. U kunt die verwerkt met behulp van deze voorbeeldcode:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Zie voor meer informatie over cookies de [CookieSyncManager informatie over de Android-site](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="ntlm-dialog-box"></a>Dialoogvenster NTLM
ADAL-versie 1.1.0 ondersteunt een NTLM-dialoogvenster dat wordt verwerkt door de gebeurtenis onReceivedHttpAuthRequest van WebViewClient. U kunt de tekenreeksen voor het dialoogvenster met de indeling en aanpassen.

### <a name="cross-app-sso"></a>Cross-app eenmalige aanmelding
Meer informatie over [het inschakelen van eenmalige aanmelding voor cross-app voor Android met behulp van ADAL](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
