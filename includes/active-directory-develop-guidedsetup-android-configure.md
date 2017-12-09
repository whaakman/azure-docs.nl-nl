
## <a name="create-an-application-express"></a>Maken van een toepassing (snelle)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is ingeschakeld
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>De registratiegegevens van uw toepassing toevoegen aan uw oplossing (Geavanceerd)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4. Klik op `Add Platform`, selecteer daarna `Native Application` en klik op Opslaan
5.  Open `MainActivity` (onder `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Vervang de *[Voer de toepassing Id hier]* in de regel die begint met `final static String CLIENT_ID` met de toepassings-ID die u zojuist hebt geregistreerd:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Open `AndroidManifest.xml` (onder `app`  >  `manifests`) toevoegen van de volgende activiteiten `manifest\application` knooppunt. Hiermee registreert een `BrowserTabActivity` waarmee het besturingssysteem op uw toepassing na het voltooien van de verificatie hervatten:
</li>
</ol>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
In de `BrowserTabActivity`, Vervang `[Enter the application Id here]` met de toepassings-ID.
</li>
</ol>
