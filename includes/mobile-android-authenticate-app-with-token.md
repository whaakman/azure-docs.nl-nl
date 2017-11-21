
Het vorige voorbeeld blijkt een standaard aanmelden, die vereist dat de client contact opnemen met zowel de id-provider en de back-end Azure service telkens wanneer de app wordt gestart. Deze methode is inefficiënt en u kunt problemen met de informatie over het gebruik hebben als veel klanten willen gelijktijdig uw app te starten. Er is een betere benadering het verificatietoken dat wordt geretourneerd door de Azure-service in de cache en probeer het eerst moet worden gebruikt deze voordat u een provider gebaseerde aanmelden.

> [!NOTE]
> U kunt het token dat is uitgegeven door de back-end Azure-service ongeacht of u van verificatie van client beheerd of service gebruikmaakt-cache. Deze zelfstudie maakt gebruik van authentication service beheerd.
>
>

1. Open het bestand ToDoActivity.java en voeg de volgende importinstructies toe:

    ```java
    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;
    ```

2. De volgende leden toevoegen aan de `ToDoActivity` klasse.

    ```java
    public static final String SHAREDPREFFILE = "temp";
    public static final String USERIDPREF = "uid";
    public static final String TOKENPREF = "tkn";
    ```

3. Voeg in het bestand ToDoActivity.java de volgende definitie voor de `cacheUserToken` methode.

    ```java
    private void cacheUserToken(MobileServiceUser user)
    {
        SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
        Editor editor = prefs.edit();
        editor.putString(USERIDPREF, user.getUserId());
        editor.putString(TOKENPREF, user.getAuthenticationToken());
        editor.commit();
    }
    ```

    Deze methode slaat de gebruikers-ID en -token in een voorkeursbestand dat is gemarkeerd als privé. Dit moet toegang tot de cache beveiligen zodat andere apps op het apparaat geen toegang tot het token. De voorkeur is sandbox voor de app. Echter, als iemand toegang tot het apparaat krijgt, is het mogelijk dat ze toegang tot de tokencache andere wijze krijgen kunnen.

   > [!NOTE]
   > U kunt het token met versleuteling, verder beschermen als token toegang tot uw gegevens wordt beschouwd als uiterst gevoelige en iemand toegang te tot het apparaat krijgen mogelijk. Een volledig veilige oplossing valt buiten het bereik van deze zelfstudie, echter, en is afhankelijk van uw beveiligingsvereisten.
   >
   >

4. Voeg in het bestand ToDoActivity.java de volgende definitie voor de `loadUserTokenCache` methode.

    ```java
    private boolean loadUserTokenCache(MobileServiceClient client)
    {
        SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
        String userId = prefs.getString(USERIDPREF, null);
        if (userId == null)
            return false;
        String token = prefs.getString(TOKENPREF, null);
        if (token == null)
            return false;

        MobileServiceUser user = new MobileServiceUser(userId);
        user.setAuthenticationToken(token);
        client.setCurrentUser(user);

        return true;
    }
    ```

5. In de *ToDoActivity.java* bestand, vervangt de `authenticate` en `onActivityResult` methoden met de volgende waarden dat gebruikmaakt van een token cache. De aanmeldingsprovider niet wijzigen als u wilt gebruiken een ander account dan Google.

    ```java
    private void authenticate() {
        // We first try to load a token cache if one exists.
        if (loadUserTokenCache(mClient))
        {
            createTable();
        }
        // If we failed to load a token cache, login and create a token cache
        else
        {
            // Login using the Google provider.
            mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }
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
                    cacheUserToken(mClient.getCurrentUser());
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

6. Bouw de app en test verificatie met een geldig account. Ten minste tweemaal wordt uitgevoerd. Tijdens de eerste keer uitvoert, moet u wordt gevraagd om te melden en de tokencache maken. Daarna wordt elke uitvoering probeert te laden van de tokencache voor verificatie. U moet niet vereist voor aanmelding.
