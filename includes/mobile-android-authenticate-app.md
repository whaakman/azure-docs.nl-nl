
1. Open het project in Android Studio.

2. In **Projectverkenner** in Android Studio, opent u de `ToDoActivity.java` -bestand en voeg de volgende importinstructies:

    ```java
    import java.util.concurrent.ExecutionException;
    import java.util.concurrent.atomic.AtomicBoolean;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;

    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
    ```

3. Voeg de volgende methode voor de **ToDoActivity** klasse:

    ```java
    // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
    public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

    private void authenticate() {
        // Login using the Google provider.
        mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the login request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // login succeeded
                    createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                    createTable();
                } else {
                    // login failed, check the error message
                    String errorMessage = result.getErrorMessage();
                    createAndShowDialog(errorMessage, "Error");
                }
            }
        }
    }
    ```

    Deze code maakt een methode voor het afhandelen van het verificatieproces Google. Een dialoogvenster geeft de ID van de geverifieerde gebruiker. U kunt alleen doorgaan op een geslaagde verificatie.

    > [!NOTE]
    > Als u van een id-provider dan Google gebruikmaakt, wijzig de waarde die is doorgegeven aan de **aanmelding** methode op een van de volgende waarden: _MicrosoftAccount_, _Facebook_, _Twitter_, of _windowsazureactivedirectory_.

4. In de **onCreate** methode, voeg de volgende regel code na de code die wordt de `MobileServiceClient` object.

    ```java
    authenticate();
    ```

    Deze aanroep start het verificatieproces.

5. Verplaatsen van de resterende code na `authenticate();` in de **onCreate** methode naar een nieuwe **createTable** methode:

    ```java
    private void createTable() {

        // Get the table instance to use.
        mToDoTable = mClient.getTable(ToDoItem.class);

        mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

        // Create an adapter to bind the items with the view.
        mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
        ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
        listViewToDo.setAdapter(mAdapter);

        // Load the items from Azure.
        refreshItemsFromTable();
    }
    ```

6. Om ervoor te zorgen omleiding werkt zoals verwacht, Voeg het volgende fragment van `RedirectUrlActivity` naar `AndroidManifest.xml`:

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}"
                android:host="easyauth.callback"/>
        </intent-filter>
    </activity>
    ```

7. Voeg `redirectUriScheme` naar `build.gradle` van uw Android-toepassing.

    ```gradle
    android {
        buildTypes {
            release {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
            debug {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
        }
    }
    ```

8. Voeg `com.android.support:customtabs:23.0.1` naar de afhankelijkheden in uw `build.gradle`:

    ```gradle
    dependencies {
        // ...
        compile 'com.android.support:customtabs:23.0.1'
    }
    ```

9. Van de **uitvoeren** menu, klikt u op **app uitvoeren** starten van de app en meld u aan met uw gekozen id-provider.

> [!WARNING]
> Het URL-schema vermeld is hoofdlettergevoelig. Zorg ervoor dat alle instanties van `{url_scheme_of_you_app}` gebruiken hetzelfde hoofdlettergebruik.

Wanneer u bent aangemeld, de app moet worden uitgevoerd zonder fouten en moet u kunnen de back-endservice doorzoeken en updates aanbrengen in de gegevens.
