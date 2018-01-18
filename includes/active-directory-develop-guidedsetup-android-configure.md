
## <a name="register-your-application"></a>Uw toepassing registreren
U kunt uw toepassing op twee manieren registreren, zoals beschreven in de volgende twee secties.

### <a name="option-1-express-mode"></a>Optie 1: Snelle modus
U kunt snel uw toepassing registreren als volgt:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  In de **toepassingsnaam** Voer een naam voor uw toepassing.

3. Zorg ervoor dat de **Begeleide instelprocedure** selectievakje is geselecteerd en selecteer vervolgens **maken**.

4. Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus
Uw toepassing registreren en de registratiegegevens van uw toepassing toevoegen aan uw oplossing, kunt u het volgende:
1. Als u al uw toepassing nog niet hebt geregistreerd, gaat u naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app).
2. In de **toepassingsnaam** Voer een naam voor uw toepassing. 

3. Zorg ervoor dat de **Begeleide instelprocedure** selectievakje is uitgeschakeld en selecteer vervolgens **maken**.

4. Selecteer **toevoegen Platform**, selecteer **systeemeigen toepassing**, en selecteer vervolgens **opslaan**.

5. Onder **app** > **java** > **{host}. { naamruimte}**Open `MainActivity`. 

6.  Vervang *[Voer de toepassing Id hier]* in de volgende regel met de toepassings-ID die u zojuist hebt geregistreerd:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Onder **app** > **manifesten**, open de *AndroidManifest.xml* bestand.

8. In de `manifest\application` knooppunt, de volgende activiteit toevoegen. In dat geval registers doet een `BrowserTabActivity` activiteit waarmee het besturingssysteem op uw toepassing hervatten nadat de verificatie is voltooid:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
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
9. In de `BrowserTabActivity` knooppunt vervangen `[Enter the application Id here]` met de toepassings-ID.
