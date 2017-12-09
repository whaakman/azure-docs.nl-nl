
## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw app

In deze stap moet u de Client-ID toevoegen aan uw project.

1.  Open `MainActivity` (onder `app`  >  `java`  >   *`{host}.{namespace}`* )
2.  Vervang de regel die begint met `final static String CLIENT_ID` met:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Open:`app` > `manifests` > `AndroidManifest.xml`
4. Voeg de volgende activiteiten `manifest\application` knooppunt. In dit register een `BrowserTabActivity` waarmee het besturingssysteem op uw toepassing na het voltooien van de verificatie hervatten:

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

### <a name="what-is-next"></a>Wat is de volgende

[Testen en valideren](active-directory-develop-guidedsetup-android-test.md)
