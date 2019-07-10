---
title: Aan de slag met Android - Microsoft identity-platform | Azure
description: Hoe kan een Android-app een toegangstoken en Microsoft Graph API of API's waarvoor toegangstokens van Microsoft identity-platform aanroepen.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/09/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71c6b0d4cd664b12dbd0fbd4e9423240c8dbebb3
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723811"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Meld u aan gebruikers en de Microsoft Graph aanroepen vanuit een Android-app

In deze zelfstudie leert u hoe u een Android-app integreren met het Microsoft identity-platform. Uw app wordt een gebruiker, een toegangstoken voor het aanroepen van de Microsoft Graph API en te vragen de Microsoft Graph API.  

Wanneer u de handleiding hebt voltooid, wordt uw toepassing accepteert aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en anderen) en werk of school-accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.

## <a name="how-this-tutorial-works"></a>De werking van deze zelfstudie

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze zelfstudie werkt](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

De app in dit voorbeeld wordt Meld u aan gebruikers en gegevens ophalen in hun naam.  Deze gegevens zullen worden geopend via een beveiligde API (Microsoft Graph API) die is autorisatie vereist.

Met name:

* Uw app, wordt de gebruiker via een browser of de Microsoft Authenticator en Intune-bedrijfsportal aanmelden.
* De eindgebruiker accepteert de machtigingen die uw toepassing heeft aangevraagd. 
* Uw app worden een toegangstoken uitgegeven voor de Microsoft Graph API.
* Het toegangstoken worden opgenomen in de HTTP-aanvraag naar de web-API.
* Het antwoord van de Microsoft Graph worden verwerkt.

In dit voorbeeld maakt gebruik van de Microsoft Authentication library voor Android (MSAL) voor het implementeren van verificatie. MSAL wordt automatisch vernieuwen van tokens, eenmalige aanmelding (SSO) tussen de andere apps op het apparaat leveren en beheren van het account (s).

## <a name="prerequisites"></a>Vereisten

* Android Studio maakt gebruik van deze Begeleide installatie.
* Android 16 of hoger is vereist (19 + wordt aanbevolen).

## <a name="library"></a>Bibliotheek

Deze handleiding worden de volgende verificatiebibliotheek gebruikt:

|Bibliotheek|Description|
|---|---|
|[com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|

## <a name="create-a-project"></a>Een project maken

In deze zelfstudie wordt een nieuw project maken. Als u wilt downloaden van de voltooide zelfstudie in plaats daarvan [downloaden van de code](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip).

1. Open Android Studio en selecteer **Start een nieuw Android Studio-project**
2. Selecteer **Basic activiteit** en klikt u op **volgende**.
3. Geef uw toepassing
4. Naam van het pakket opslaan. Voert u deze later in de Azure-portal. 
5. Stelt de **minimaal API-niveau** naar **API 19** of hoger, en klikt u op **voltooien**.
6. Kies in de project-weergave **Project** open in de vervolgkeuzelijst om weer te geven van de bron- en niet-source project-bestanden, **App/build.gradle** en stel `targetSdkVersion` naar `27`.

## <a name="register-your-application"></a>Uw toepassing registreren

1. Ga naar [Azure Portal](https://aka.ms/MobileAppReg)
2. Open de [blade App-registraties](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) en klikt u op **+ nieuwe registreren**.
3. Voer een **naam** voor uw app en vervolgens, zonder in te stellen van een omleidings-URI, klikt u op **registreren**.
4. In de **beheren** sectie van het deelvenster dat verschijnt, selecteer **verificatie** >  **+ toevoegen van een platform** > **Android**.
5. Voer de pakketnaam van uw project. Als u de code hebt gedownload, wordt deze waarde is `com.azuresamples.msalandroidapp`.
6. In de **handtekening hash** sectie van de **configureren van uw Android-app** pagina, klikt u op **een ontwikkelings-handtekening Hash genereren.** en kopieer de opdracht KeyTool moet worden gebruikt voor uw platform. Opmerking: KeyTool.exe wordt geïnstalleerd als onderdeel van de Java Development Kit (JDK) en u moet ook het hulpprogramma hebt geïnstalleerd OpenSSL voor het uitvoeren van de opdracht KeyTool.
7. Voer de **handtekening hash** die worden gegenereerd door KeyTool.
8. Klik op `Configure` en sla de **MSAL configuratie** die wordt weergegeven in **Android-configuratie** pagina zodat u deze invoeren kunt bij het later opnieuw configureren van uw app.  Klik op **Gereed**.

## <a name="build-your-app"></a>Uw app ontwikkelen

### <a name="configure-your-android-app"></a>Uw Android-app configureren

1. Navigeer in Android Studio-project in het deelvenster naar **app\src\main\res**.
2. Met de rechtermuisknop op **res** en kies **nieuw** > **Directory**. Voer `raw` als de naam van de nieuwe map en klik op **OK**.
3. In **app** > **src** > **res** > **onbewerkte**, maak een nieuwe JSON-bestand met de naam `auth_config.json`en plak de MSAL-configuratie die u eerder hebt opgeslagen. Zie [MSAL-configuratie voor meer informatie](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Configuring-your-app).
   <!-- Workaround for Docs conversion bug -->
4. In **app** > **src** > **belangrijkste** > **AndroidManifest.xml**, voeg de `BrowserTabActivity`activiteit hieronder. Dit item kan Microsoft aanroepen om uw toepassing nadat deze de verificatie is voltooid:

    ```xml
    <!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
        </intent-filter>
    </activity>
    ```

    Vervang de naam van het pakket dat u hebt geregistreerd in Azure portal, de `android:host=` waarde.
    Vervang de sleutel-hash u geregistreerd in Azure portal, de `android:path=` waarde. De handtekening-Hash mag geen URL-codering.

5. In de **AndroidManifest.xml**, net boven de `<application>` tag, voeg de volgende machtigingen toe:

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

### <a name="create-the-apps-ui"></a>Maken van de app-gebruikersinterface

1. Navigeer in het venster Android Studio-project naar **app** > **src** > **belangrijkste** > **res**  >  **lay-out** en open **activity_main.xml** en open de **tekst** weergeven.
2. Wijzig de indeling van de activiteit, bijvoorbeeld `<androidx.coordinatorlayout.widget.CoordinatorLayout` naar `<androidx.coordinatorlayout.widget.LinearLayout`.
3. Voeg de `android:orientation="vertical"` eigenschap in op de `LinearLayout` knooppunt.
4. Plak de volgende code in de `LinearLayout` knooppunt, vervangen de huidige inhoud:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```

### <a name="add-msal-to-your-project"></a>MSAL toevoegen aan uw project

1. Navigeer in het venster Android Studio-project naar **app** > **src** > **build.gradle**.
2. Onder **afhankelijkheden**, plak het volgende:

    ```gradle  
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.microsoft.identity.client:msal:0.3.+'
    ```

### <a name="use-msal"></a>Gebruik MSAL

Nu wijzigingen in aanbrengen `MainActivity.java` toevoegen en gebruiken van MSAL in uw app.
Navigeer in het venster Android Studio-project naar **app** > **src** > **belangrijkste** > **java**  >  **com.example.msal**, en open `MainActivity.java`

#### <a name="required-imports"></a>Vereiste imports

Voeg de volgende invoer aan de bovenkant van `MainActivity.java`:

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
import com.microsoft.identity.client.exception.*;
```

#### <a name="instantiate-msal"></a>MSAL instantiëren

In de `MainActivity` klasse, moeten we instantiëren MSAL samen met enkele configuraties over wat zijn app wordt doen met inbegrip van de bereiken en web-API die we willen openen.

Kopieer de volgende variabelen in de `MainActivity` klasse:

```java
final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

/* UI & Debugging Variables */
private static final String TAG = MainActivity.class.getSimpleName();
Button callGraphButton;
Button signOutButton;

/* Azure AD Variables */
private PublicClientApplication sampleApp;
private IAuthenticationResult authResult;
```

Vervang de inhoud van `onCreate()` met de volgende code om het instantiëren van MSAL:

```java
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
sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);

/* Attempt to get a user and acquireTokenSilent */
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {
        if (!accounts.isEmpty()) {
            /* This sample doesn't support multi-account scenarios, use the first account */
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts */
        }
    }
});
```

De bovenstaande code probeert aan te melden bij de gebruikers op de achtergrond bij het openen van uw toepassing via `getAccounts()` en, als dit lukt, `acquireTokenSilentAsync()`.  In de volgende gedeelten wordt we de callbackhandler implementeren voor het geval is er geen aangemelde accounts zijn.

#### <a name="use-msal-to-get-tokens"></a>MSAL gebruiken om op te halen van Tokens

Nu kunnen we van de app UI verwerking van logica en ophalen van tokens interactief via MSAL implementeren.

MSAL bevat twee primaire methoden voor het ophalen van tokens: `acquireTokenSilentAsync()` en `acquireToken()`.  

`acquireTokenSilentAsync()` Als u zich aanmeldt een gebruikers- en get-tokens zonder interactie met de gebruiker als een account aanwezig is. Als dat lukt, MSAL handoff de tokens aan uw app, als dit het mislukt parameterselectie wordt een `MsalUiRequiredException`.  Als deze uitzondering wordt gegenereerd of dat u wilt dat de gebruiker een interactieve aanmelding in ervaring (referenties, mfa of andere voorwaardelijk beleid kunnen of kunnen niet meer nodig) hebt, wordt gebruik `acquireToken()`.  

`acquireToken()` Gebruikersinterface weergegeven wanneer u probeert de gebruiker aanmelden en tokens verkrijgen. Het kan echter sessiecookies in de browser of een account in de Microsoft authenticator gebruiken om de interactieve SSO-ervaring te bieden.

Maken van de volgende drie UI-methoden in de `MainActivity` klasse:

```java
/* Set the UI for successful token acquisition data */
private void updateSuccessUI() {
    callGraphButton.setVisibility(View.INVISIBLE);
    signOutButton.setVisibility(View.VISIBLE);
    findViewById(R.id.welcome).setVisibility(View.VISIBLE);
    ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
            authResult.getAccount().getUsername());
    findViewById(R.id.graphData).setVisibility(View.VISIBLE);
}

/* Set the UI for signed out account */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");

    Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
            .show();
}

/* Use MSAL to acquireToken for the end-user
 * Callback will call Graph api w/ access token & update UI
 */
private void onCallGraphClicked() {
    sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
}
```

Voeg de volgende methoden om de huidige activiteit en op de achtergrond & interactieve callbacks verwerken:

```java
public Activity getActivity() {
    return this;
}

/* Callback used in for silent acquireToken calls.
 * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
 * else errors that we need to do an interactive request.
 */
private AuthenticationCallback getAuthSilentCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
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
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");
            Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
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
```

#### <a name="use-msal-for-sign-out"></a>Gebruik MSAL voor afmelding

Vervolgens is ondersteuning toegevoegd voor afmelden.

> [!Important]
> Afmelden met MSAL alle bekende informatie over een gebruiker verwijdert uit de toepassing, maar de gebruiker heeft nog steeds een actieve sessie op hun apparaat. Als de gebruiker probeert aan te melden bij het opnieuw ze UI voor aanmelden mogelijk te zien, maar mogelijk niet naar hun referenties opnieuw invoeren omdat de sessie van het apparaat nog steeds actief is.

Om toe te voegen afmelding mogelijkheid, voegt u de volgende methode binnen de `MainActivity` klasse. Deze methode bladeren door alle accounts en verwijdert u deze:

```java
/* Clears an account's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 * User will get interactive SSO if trying to sign back-in.
 */
private void onSignOutClicked() {
    /* Attempt to get a user and acquireTokenSilent
     * If this fails we do an interactive request
     */
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (accounts.isEmpty()) {
                /* No accounts to remove */

            } else {
                for (final IAccount account : accounts) {
                    sampleApp.removeAccount(
                            account,
                            new PublicClientApplication.AccountsRemovedCallback() {
                        @Override
                        public void onAccountsRemoved(Boolean isSuccess) {
                            if (isSuccess) {
                                /* successfully removed account */
                            } else {
                                /* failed to remove account */
                            }
                        }
                    });
                }
            }

            updateSignedOutUI();
        }
    });
}
```

#### <a name="call-the-microsoft-graph-api"></a>De Microsoft Graph API aanroepen

Nadat we een token ontvangen hebben, kunnen we er een aanvraag naar de [Microsoft Graph API](https://graph.microsoft.com) het toegangstoken, bevinden zich binnen de `AuthenticationResult` binnen de auth-callback `onSuccess()` methode. Kan een gemachtigde aanvraag, moet uw app het toegangstoken toevoegen aan de HTTP-header:

| Header-sleutel    | value                 |
| ------------- | --------------------- |
| Authorization | Bearer \<toegangstoken > |

Voeg de volgende twee methoden in de `MainActivity` klasse graph aanroepen en het bijwerken van de gebruikersinterface:

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
        public Map<String, String> getHeaders() {
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

/* Sets the graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```

#### <a name="multi-account-applications"></a>Account voor meerdere toepassingen

Deze app is gebouwd voor een scenario met één account. MSAL biedt ook ondersteuning voor meerdere account scenario's, maar hiervoor extra werk van apps. U moet de gebruikersinterface van de gebruiker selecteren welk account moet worden gebruikt voor elke actie waarvoor tokens te maken. U kunt ook uw app kunt implementeren om een heuristiek om te selecteren welk account moet worden gebruikt de `getAccounts()` methode.

## <a name="test-your-app"></a>Uw app testen

### <a name="run-locally"></a>Lokaal uitvoeren

Bouw en implementeer de app voor een testapparaat of emulator. U moet zich aanmelden en tokens verkrijgen voor Azure AD of persoonlijke Microsoft-accounts.

Nadat u zich hebt aangemeld, worden de app de gegevens die zijn geretourneerd door de Microsoft Graph weergegeven `/me` eindpunt.

### <a name="consent"></a>Toestemming geven

De eerste keer dat een gebruiker zich in uw app, wordt ze gevraagd door Microsoft identity akkoord gaan met de machtigingen die zijn aangevraagd.  Hoewel de meeste gebruikers geschikt voor stemt ermee in dat zijn, zijn sommige Azure AD-tenants toestemming van de gebruiker waarvoor u beheerders om in te stemmen namens alle gebruikers uitgeschakeld. Ter ondersteuning van dit scenario, van uw app bereiken in de Azure-portal te registreren.

## <a name="get-help"></a>Help opvragen

Ga naar [Help en ondersteuning](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) als u problemen ondervindt met deze zelfstudie of met het Microsoft identity-platform.
