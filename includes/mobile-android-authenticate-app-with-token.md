---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: deb94cab97bd9a402676cdc5c0239da8d07ed8b2
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440089"
---
Het vorige voorbeeld hebt u een standaard geleerd aanmelden, waarvoor de client verbinding maken met zowel de id-provider en de back-end Azure-service telkens wanneer de app wordt gestart. Deze methode is inefficiënt en u kunt problemen met betrekking tot gebruik hebben als veel klanten uw app tegelijkertijd start. Een betere benadering is het verificatietoken dat is geretourneerd door de Azure-service in de cache en probeer het gebruik van deze eerste voordat u een aanmelding op basis van een provider.

> [!NOTE]
> U kunt het token dat is uitgegeven door de back-end Azure-service, ongeacht of u van verificatie-client of de service worden beheerd gebruikmaakt in de cache. In deze zelfstudie maakt gebruik van beheerde service-verificatie.
>
>

1. Open het bestand ToDoActivity.java en voeg de volgende importinstructies toe:

    ```java
    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;
    ```

2. Toevoegen van de volgende leden aan de `ToDoActivity` klasse.

    ```java
    public static final String SHAREDPREFFILE = "temp";
    public static final String USERIDPREF = "uid";
    public static final String TOKENPREF = "tkn";
    ```

3. In het bestand ToDoActivity.java, voeg de volgende definitie voor de `cacheUserToken` methode.

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

    Deze methode worden de gebruikers-ID en het token opgeslagen in een voorkeursbestand dat is gemarkeerd als privé. Dit moet toegang tot de cache beveiligen zodat andere apps op het apparaat geen toegang tot het token. De voorkeur is sandbox voor de app. Echter, als iemand zich toegang verschaft tot het apparaat, is het mogelijk dat ze toegang tot de tokencache via andere middelen krijgen kunnen.

   > [!NOTE]
   > U kunt het token met versleuteling, verder beveiligen als token toegang tot uw gegevens wordt beschouwd als uiterst gevoelige en iemand mogelijk toegang krijgen tot het apparaat. Een volledig veilige oplossing valt buiten het bereik van deze zelfstudie, echter, en is afhankelijk van uw beveiligingsvereisten.
   >
   >

4. In het bestand ToDoActivity.java, voeg de volgende definitie voor de `loadUserTokenCache` methode.

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

5. In de *ToDoActivity.java* bestand, vervangt de `authenticate` en `onActivityResult` methoden met de volgende query, dat gebruikmaakt van een token cache. De provider voor de aanmelding wijzigen als u wilt gebruiken een ander account dan Google.

    ```java
    private void authenticate() {
        // We first try to load a token cache if one exists.
        if (loadUserTokenCache(mClient))
        {
            createTable();
        }
        // If we failed to load a token cache, sign in and create a token cache
        else
        {
            // Sign in using the Google provider.
            mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the sign-in request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // sign-in succeeded
                    createAndShowDialog(String.format("You are now signed in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                    cacheUserToken(mClient.getCurrentUser());
                    createTable();
                } else {
                    // sign-in failed, check the error message
                    String errorMessage = result.getErrorMessage();
                    createAndShowDialog(errorMessage, "Error");
                }
            }
        }
    }
    ```

6. Bouw de app en test-verificatie met behulp van een geldig account. Ten minste tweemaal wordt uitgevoerd. Tijdens de eerste keer uitvoert ontvangt u een prompt om te melden en de token cache maken. Daarna wordt elke uitvoering probeert te laden van de tokencache voor verificatie. U moet niet worden vereist aan te melden.
