---
title: Aan de slag met Android - Microsoft identity-platform | Azure
description: Hoe kan een Android-app een toegangstoken en Microsoft Graph API of API's waarvoor toegangstokens van Microsoft identity-platform aanroepen.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f2bcb7d93361ccf3d83947222ab6a3937a7eedc4
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935965"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Meld u aan gebruikers en de Microsoft Graph aanroepen vanuit een Android-app

In deze zelfstudie leert u hoe u een Android-app integreren met het Microsoft identity-platform. Uw app wordt specifiek, meld u aan een gebruiker, een toegangstoken voor het aanroepen van de Microsoft Graph API en te vragen de Microsoft Graph API.  

Wanneer u de handleiding hebt voltooid, wordt uw toepassing accepteert aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en anderen) en werk of school-accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.

## <a name="how-this-tutorial-works"></a>De werking van deze zelfstudie

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze zelfstudie werkt](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

De app in dit voorbeeld wordt Meld u aan gebruikers en gegevens ophalen in hun naam.  Deze gegevens zullen worden geopend via een beveiligde API (Microsoft Graph-API in dit geval) die is autorisatie vereist.

Met name:

* Uw app, wordt de gebruiker via een browser of de Microsoft Authenticator en Intune-bedrijfsportal aanmelden.
* De eindgebruiker accepteert de machtigingen die uw toepassing heeft aangevraagd. 
* Uw app worden een toegangstoken uitgegeven voor de Microsoft Graph API.
* Het toegangstoken worden opgenomen in de HTTP-aanvraag naar de web-API.
* Het antwoord van de Microsoft Graph worden verwerkt.

In dit voorbeeld maakt gebruik van de Microsoft Authentication library voor Android (MSAL) voor het implementeren van Auth. MSAL wordt automatisch vernieuwen van tokens, eenmalige aanmelding tussen andere apps op het apparaat leveren en beheren van het account (s).

## <a name="prerequisites"></a>Vereisten

* Android Studio maakt gebruik van deze Begeleide installatie.
* Android 16 of hoger is vereist (19 + wordt aanbevolen).

## <a name="library"></a>Bibliotheek

Deze handleiding worden de volgende verificatiebibliotheek gebruikt:

|Bibliotheek|Description|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|

## <a name="set-up-your-project"></a>Uw project instellen

In deze zelfstudie wordt een nieuw project maken. Als u wilt downloaden van de voltooide zelfstudie in plaats daarvan [downloaden van de code](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip).

### <a name="create-a-new-project"></a>Een nieuw project maken

1. Open Android Studio en selecteer **Start een nieuw Android Studio-project**.
    - Als Android Studio al geopend is, selecteert u **bestand** > **nieuw** > **nieuw Project**.
2. Laat **lege activiteit** is, selecteert u **volgende**.
3. Geef uw toepassing, stelt u de `Minimum API level` naar **API 19 of nieuwere**, treffers **voltooien**.
5. In uw `app/build.gradle`, stel de `targetedSdkVersion` tot en met 27. 

## <a name="register-your-application"></a>Uw toepassing registreren

U kunt uw toepassing registreren op twee manieren, zoals beschreven in de volgende twee secties.

### <a name="register-your-app"></a>Uw app registreren

1. Ga naar de [Azure-portal](https://aka.ms/MobileAppReg) > Selecteer `New registration`. 
2. Voer een **naam** voor uw app > `Register`. **Stel een omleidings-URI niet in deze fase**. 
3. In de `Manage` sectie, gaat u naar `Authentication` > `Add a platform` > `Android`
    - Voer de pakketnaam van uw project. Als u de code hebt gedownload, wordt deze waarde is `com.azuresamples.msalandroidapp`. 
    - Voer uw foutopsporing/ontwikkeling handtekening-hash. Gebruik de opdracht KeyTool in de portal voor het genereren van de handtekening-Hash. 
4. Raak `Configure` en op te slaan de ***MSAL configuratie*** voor later. 

## <a name="build-your-app"></a>Uw app ontwikkelen

### <a name="configure-your-android-app"></a>Uw Android-app configureren

1. Klik met de rechtermuisknop **res** > **nieuw** > **map** > **onbewerkte map voor bronnen**
2. In **app** > **res** > **onbewerkte**, maak een nieuwe JSON-bestand met de naam `auth_config.json` en plak uw ***MSAL configuratie***. Zie [MSAL-configuratie voor meer informatie](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Configuring-your-app).
   <!-- Workaround for Docs conversion bug -->
3. In **app** > **manifesten** > **AndroidManifest.xml**, voeg de `BrowserTabActivity` activiteit hieronder. Dit item kan Microsoft aanroepen om uw toepassing nadat deze de verificatie is voltooid:

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

    Opmerking: de handtekening-Hash gebruikt mag geen URL-codering de **AndroidManifest.xml**. 

4. In de **AndroidManifest.xml** en net boven de `<application>` tag, voeg de volgende machtigingen toe:

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

5. In de `BrowserTabActivity`, vervangen de ***pakketnaam*** en ***handtekening Hash*** door de waarden die zijn geregistreerd in Azure portal.

### <a name="create-the-apps-ui"></a>Maken van de app-gebruikersinterface

1. Ga naar **res** > **lay-out**, en open vervolgens **activity_main.xml**.
2. Wijzig de indeling van de activiteit van `android.support.constraint.ConstraintLayout` of andere aan `LinearLayout`.
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

1. Selecteer in Android Studio **Gradle-Scripts** > **build.gradle (Module: app)**.
2. Onder **afhankelijkheden**, plak de volgende code:

    ```gradle  
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.microsoft.identity.client:msal:0.3.+'
    ```

### <a name="use-msal"></a>Gebruik MSAL 

De volgende gedeelten wordt nog wijzigingen in de `MainAcitivty.java`. We u gaan met elke stap die nodig zijn voor het toevoegen en MSAL in uw app in uw.

#### <a name="required-imports"></a>Vereiste imports

Voeg de volgende import toe aan uw project: 

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

#### <a name="instantiating-msal"></a>Het instantiëren van MSAL 

In de `MainActivity` klasse, moeten we instantiëren MSAL samen met enkele configuraties over wat zijn app wordt doen met inbegrip van de bereiken en web-API die we willen openen. 

Kopieer de volgende variabelen in de `MainActivity`:

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

Nu u MSAL instantiëren, Kopieer de volgende code in de `onCreate(...)` methode:

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

Het bovenstaande blok van code probeert aan te melden bij de gebruikers op de achtergrond bij het openen van uw toepassing via `getAccounts(...)` en, als dit lukt, `acquireTokenSilentAsync(...)`.  In de volgende gedeelten wordt we de callbackhandler implementeren voor het geval is er geen aangemelde accounts zijn. 

#### <a name="use-msal-to-get-tokens"></a>MSAL gebruiken om op te halen van Tokens

Nu kunnen we van de app UI verwerking van logica en ophalen van tokens interactief via MSAL implementeren. 

MSAL bevat twee primaire methoden voor het ophalen van tokens: `acquireTokenSilentAsync` en `acquireToken`.  

`acquireTokenSilentAsync` Als u zich aanmeldt een gebruikers- en get-tokens zonder interactie met de gebruiker als een account aanwezig is. Als dat lukt, MSAL handoff de tokens aan uw app, als dit het mislukt parameterselectie wordt een `MsalUiRequiredException`.  Als deze uitzondering wordt gegenereerd of als u wilt dat de gebruiker om een interactieve aanmelding in ervaring (referenties, mfa of andere voorwaardelijk beleid kunnen of kunnen niet meer nodig), klikt u vervolgens kunt u `acquireToken`.  

`acquireToken` de gebruikersinterface wordt altijd weergegeven wanneer u probeert de gebruiker aanmelden en ophalen van tokens. het kan echter sessiecookies in de browser of een account in de Microsoft authenticator gebruiken om te geven van een interactieve SSO-ervaring. 

Als u wilt beginnen, maken de volgende drie UI-methoden in de `MainActivity` klasse:

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

Vervolgens voegt u een methode voor het ophalen van de huidige activiteit en op de achtergrond & interactieve callbacks verwerken toe:

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

Vervolgens, wordt ondersteuning voor toegevoegd aan onze app afmelden. 

Het is belangrijk te weten, afmelding met MSAL verwijdert alle bekende informatie over een gebruiker uit deze toepassing, maar de gebruiker heeft nog steeds een actieve sessie op hun apparaat. Als de gebruiker probeert aan te melden bij opnieuw ze interactie mogelijk te zien, maar mogelijk niet opnieuw hun referenties vanwege de apparaat-sessie actief in te voeren. 

Om toe te voegen afmelden, kopieert u de volgende methode in uw app, die alle accounts doorlopen en verwijdert u deze:

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

Nadat we hebben een token is geworden, kunnen we een aanvraag naar de Microsoft Graph API. Het toegangstoken, bevinden zich binnen de `AuthenticationResult` binnen de auth-callback `onSuccess(...)` methode. Kan een gemachtigde aanvraag, moet uw app het toegangstoken toevoegen aan de HTTP-header:

| Header-sleutel    | value                 |
| ------------- | --------------------- |
| Autorisatie | Bearer < toegangstoken > |

Om dit te doen in de code aan uw app graph aanroepen en het bijwerken van de gebruikersinterface met toevoegen van de volgende twee methoden: 

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

Meer informatie over de [Microsoft Graph API](https://graph.microsoft.com)!

#### <a name="multi-account-applications"></a>Account voor meerdere toepassingen

Deze app is gebouwd voor een scenario met één account. MSAL ondersteunt ook meerdere account-scenario's, maar hiervoor extra werk van apps. U moet de gebruikersinterface van de gebruiker selecteren welk account moet worden gebruikt voor elke actie waarvoor tokens te maken. U kunt ook uw app kunt implementeren om een heuristiek om te selecteren welk account moet worden gebruikt de `getAccounts(...)` methode. 

## <a name="test-your-app"></a>Uw app testen

### <a name="run-locally"></a>Lokaal uitvoeren

Als u de bovenstaande code hebt gevolgd, probeert te bouwen en implementeren van de app op een testapparaat of emulator. U moet zich aanmelden en tokens verkrijgen voor Azure AD of persoonlijke Microsoft-accounts. Deze app verschijnt nadat een gebruiker die zich, de gegevens die zijn geretourneerd door de Microsoft Graph `/me` eindpunt. 

Als u problemen ondervindt, gerust een probleem in dit document of in de bibliotheek MSAL openen en laat het ons weten. 

### <a name="consent-to-your-app"></a>Toestemming geven voor uw app

De eerste keer dat een gebruiker zich in uw app, wordt ze gevraagd door Microsoft identity akkoord gaan met de machtigingen die zijn aangevraagd.  Hoewel de meeste gebruikers geschikt voor stemt ermee in dat zijn, zijn sommige Azure AD-tenants toestemming van de gebruiker - beheerders om in te stemmen namens alle gebruikers waarvoor uitgeschakeld.  Ter ondersteuning van dit scenario, zorg dat voor het registreren van uw app bereiken in de Azure-portal.

## <a name="help-and-support"></a>Help en ondersteuning

Problemen met deze zelfstudie of met het Microsoft identity-platform had? Zie [Help en ondersteuning](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options)
