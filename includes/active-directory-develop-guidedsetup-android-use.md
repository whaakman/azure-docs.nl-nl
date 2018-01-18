
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Gebruik MSAL voor een token voor de Microsoft Graph-API

1.  Onder **app** > **java** > **{domein}. { AppName}**Open `MainActivity`. 
2.  Voeg de volgende import toe:

    ```java
    import android.app.Activity;
    import android.content.Intent;
    import android.util.Log;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    import android.widget.Toast;
    import com.android.volley.*;
    import com.android.volley.toolbox.JsonObjectRequest;
    import com.android.volley.toolbox.Volley;
    import org.json.JSONObject;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import com.microsoft.identity.client.*;
    ```

3. Vervang de `MainActivity` klasse met de volgende code:

    ```java
    public class MainActivity extends AppCompatActivity {

        final static String CLIENT_ID = "[Enter the application Id here]";
        final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
        final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

        /* UI & Debugging Variables */
        private static final String TAG = MainActivity.class.getSimpleName();
        Button callGraphButton;
        Button signOutButton;

        /* Azure AD Variables */
        private PublicClientApplication sampleApp;
        private AuthenticationResult authResult;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            callGraphButton = (Button) findViewById(R.id.callGraph);
            signOutButton = (Button) findViewById(R.id.clearCache);

            callGraphButton.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    onCallGraphClicked();
                }
            });

            signOutButton.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    onSignOutClicked();
                }
            });

    /* Configure your sample app and save state for this activity */
            sampleApp = null;
            if (sampleApp == null) {
                sampleApp = new PublicClientApplication(
                        this.getApplicationContext(),
                        CLIENT_ID);
            }

    /* Attempt to get a user and acquireTokenSilent
    * If this fails we do an interactive request
    */
            List<User> users = null;

            try {
                users = sampleApp.getUsers();

                if (users != null && users.size() == 1) {
            /* We have 1 user */

                    sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
                } else {
            /* We have no user */

            /* Let's do an interactive request */
                    sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
                }
            } catch (MsalClientException e) {
                Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

            } catch (IndexOutOfBoundsException e) {
                Log.d(TAG, "User at this position does not exist: " + e.toString());
            }

        }

    //
    // App callbacks for MSAL
    // ======================
    // getActivity() - returns activity so we can acquireToken within a callback
    // getAuthSilentCallback() - callback defined to handle acquireTokenSilent() case
    // getAuthInteractiveCallback() - callback defined to handle acquireToken() case
    //

        public Activity getActivity() {
            return this;
        }

        /* Callback method for acquireTokenSilent calls 
        * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
        * else errors that we need to do an interactive request.
        */
        private AuthenticationCallback getAuthSilentCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call Graph now */
                    Log.d(TAG, "Successfully authenticated");

                /* Store the authResult */
                    authResult = authenticationResult;

                /* call graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
                    } else if (exception instanceof MsalServiceException) {
                    /* Exception when communicating with the STS, likely config issue */
                    } else if (exception instanceof MsalUiRequiredException) {
                    /* Tokens expired or no session, retry with interactive */
                    }
                }

                @Override
                public void onCancel() {
                /* User cancelled the authentication */
                    Log.d(TAG, "User cancelled login.");
                }
            };
        }

        /* Callback used for interactive request.  If succeeds we use the access
            * token to call the Microsoft Graph. Does not check cache
            */
        private AuthenticationCallback getAuthInteractiveCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call graph now */
                    Log.d(TAG, "Successfully authenticated");
                    Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

                /* Store the auth result */
                    authResult = authenticationResult;

                /* call Graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
                    } else if (exception instanceof MsalServiceException) {
                    /* Exception when communicating with the STS, likely config issue */
                    }
                }

                @Override
                public void onCancel() {
                /* User cancelled the authentication */
                    Log.d(TAG, "User cancelled login.");
                }
            };
        }

        /* Set the UI for successful token acquisition data */
        private void updateSuccessUI() {
            callGraphButton.setVisibility(View.INVISIBLE);
            signOutButton.setVisibility(View.VISIBLE);
            findViewById(R.id.welcome).setVisibility(View.VISIBLE);
            ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                    authResult.getUser().getName());
            findViewById(R.id.graphData).setVisibility(View.VISIBLE);
        }

        /* Use MSAL to acquireToken for the end-user
        * Callback will call Graph api w/ access token & update UI
        */
        private void onCallGraphClicked() {
            sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
        }

        /* Handles the redirect from the System Browser */
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
        }

    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie
#### <a name="get-a-user-token-interactively"></a>Een gebruiker interactief token ophalen
Het aanroepen van de `AcquireTokenAsync` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich aanmelden in interactief de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze wellicht ook aan te melden bij een achtergrond te verkrijgen van een token mislukt (bijvoorbeeld wanneer een gebruiker het wachtwoord is verlopen).

#### <a name="get-a-user-token-silently"></a>Een gebruiker achtergrond token ophalen
De `AcquireTokenSilentAsync` methode token acquisities van organisaties en vernieuwingen zonder tussenkomst van de gebruiker worden verwerkt. Na `AcquireTokenAsync` wordt uitgevoerd voor de eerste keer `AcquireTokenSilentAsync` is de gebruikelijke methode gebruiken om toegang te verkrijgen van tokens die toegang beveiligde bronnen voor volgende aanroepen tot omdat aanroepen naar aanvragen of vernieuwen van tokens op de achtergrond worden gemaakt.

Uiteindelijk de `AcquireTokenSilentAsync` methode mislukken. Oorzaak van de fout is mogelijk dat de gebruiker heeft zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem doordat een interactieve actie kan worden omgezet, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

* Er kan een aanroep van tegen `AcquireTokenAsync` onmiddellijk. Deze aanroep resulteert in vraagt de gebruiker aan te melden. Dit patroon wordt meestal gebruikt in de on line toepassingen wanneer er geen beschikbare offline inhoud voor de gebruiker is. De steekproef die worden gegenereerd door deze Begeleide instelprocedure volgt dit patroon die u kunt zien in actie de eerste keer dat u het voorbeeld uitvoert. 
    * Omdat er geen gebruiker heeft de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering gegenereerd. 
    * De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync`, wat ertoe leidt de gebruiker aan te melden. 

* Het kan in plaats daarvan een visuele indicatie opleveren voor gebruikers die een interactief aanmelden vereist is, zodat ze het juiste moment aan te melden kunnen selecteren. Of de toepassing opnieuw kunt `AcquireTokenSilentAsync` later. Dit patroon wordt vaak gebruikt als gebruikers andere toepassingsfunctionaliteit van de zonder onderbreking--bijvoorbeeld gebruiken kunnen wanneer u offline inhoud is beschikbaar in de toepassing. In dit geval bepalen gebruikers wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens te vernieuwen. U kunt ook de toepassing opnieuw kunt bepalen `AcquireTokenSilentAsync` wanneer het netwerk is hersteld nadat u hebt tijdelijk niet beschikbaar zijn. 
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>De Microsoft Graph-API aanroepen via het token dat u zojuist hebt verkregen
Voeg de volgende methoden in de `MainActivity` klasse:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In deze voorbeeldtoepassing `callGraphAPI` aanroepen `getAccessToken` en maakt vervolgens een HTTP `GET` -aanvraag in voor een resource die is een token vereist en retourneert de inhoud. Deze methode wordt het token verkregen in de HTTP-autorisatie-header. Voor dit voorbeeld wordt de resource is de Microsoft Graph API *mij* eindpunt waarin informatie over het profiel van de gebruiker.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Afmelden instellen

Voeg de volgende methoden in de `MainActivity` klasse:

```java
/* Clears a user's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt to get a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set the UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Meer informatie over gebruikers afmelden

De `onSignOutClicked` methode in de bovenstaande code worden gebruikers verwijderd uit de cache van de gebruiker MSAL, waardoor effectief MSAL vergeten van de huidige gebruiker, zodat een toekomstige aanvraag voor een token verkrijgen slaagt alleen als interactief zijn wordt gemaakt.

Hoewel de toepassing in dit voorbeeld één gebruikers ondersteunt, ondersteunt MSAL scenario's waarbij meerdere accounts kunnen worden ondertekend op hetzelfde moment. Een voorbeeld is een e-mailtoepassing waar een gebruiker heeft meerdere accounts.
<!--end-collapse-->
