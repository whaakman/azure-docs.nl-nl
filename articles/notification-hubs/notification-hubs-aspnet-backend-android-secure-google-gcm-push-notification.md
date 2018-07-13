---
title: Beveiligde Pushmeldingen verzenden met Azure Notification Hubs
description: Leer hoe u veilige pushmeldingen verzenden naar een Android-app van Azure. Voorbeelden van code geschreven in Java en C#.
documentationcenter: android
keywords: pushmelding, pushmeldingen, push-berichten, android-push-meldingen
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/25/2018
ms.author: dimazaid
ms.openlocfilehash: 58f6967c59a5060baa10ff83752b9c6ed08226cb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38698025"
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Beveiligde Pushmeldingen verzenden met Azure Notification Hubs
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Overzicht
> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started) voor meer informatie.
> 
> 

Push notification-ondersteuning in Microsoft Azure kunt u toegang tot een eenvoudig te gebruiken, multiplatform, uitgebreide push-bericht-infrastructuur, die aanzienlijk vereenvoudigt de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms.

Vanwege regelgeving of veiligheidsbeperkingen, soms een toepassing wilt iets opnemen in de melding dat kan niet worden verzonden via de infrastructuur voor pushmeldingen voor standard. In deze zelfstudie wordt beschreven hoe u dezelfde ervaring bereiken door het verzenden van gevoelige gegevens via een veilige, geverifieerde verbinding tussen de client Android-apparaat en de back-end.

Op hoog niveau is de stroom als volgt uit:

1. De app-back-end:
   * Winkels beveiligde nettolading in back-end-database.
   * De ID van deze melding verzendt naar de Android-apparaat (geen beveiligde gegevens wordt verzonden).
2. De app op het apparaat bij de ontvangst van de melding:
   * Het Android-apparaat neemt contact op met de back-end de nettolading van de beveiligde aanvragen.
   * De app kan de nettolading van de weergegeven als een melding op het apparaat.

Het is belangrijk te weten dat in de vorige stroom (en in deze zelfstudie), wordt ervan uitgegaan dat het apparaat een verificatietoken in de lokale opslag opslaat nadat de gebruiker zich aanmeldt. Deze aanpak garandeert een naadloze ervaring als het apparaat van de melding van de beveiligde nettolading met behulp van dit token kunt ophalen. Als uw toepassing slaat geen verificatietokens op het apparaat, of als deze tokens kunnen worden verlopen, moet een algemene melding waarin de gebruiker wordt gevraagd om de app te starten worden de apparaat-app, na ontvangst van de pushmelding weergegeven. De app vervolgens verifieert de gebruiker en de nettolading van de melding ziet.

In deze zelfstudie laat zien hoe beveiligde pushmeldingen te verzenden. Dit is gebaseerd op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) zelfstudie, dus u moet de stappen in deze zelfstudie eerst als u dat nog niet gedaan hebt.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Wijzigen van het Android-project
Nu dat u hebt gewijzigd dat uw app-back-end voor het verzenden van alleen de *ID* van een pushmelding ontvangen, moet u uw Android-app voor het verwerken van waarmee de melding en uw back-end om op te halen van het beveiligde bericht moet worden weergegeven voor de terugbelfunctie wijzigen.
U hebt voor dit doel te bereiken, om ervoor te zorgen dat uw Android-app hoe weet moet worden geverifieerd met uw back-end wanneer wordt de pushmeldingen ontvangen.

Wijzig nu de *aanmelding* stroom om op te slaan de waarde van de verificatie-header in de gedeelde voorkeuren van uw app. Analoog mechanismen kunnen worden gebruikt voor het opslaan van een verificatietoken (bijvoorbeeld: OAuth-tokens) met de app te gebruiken zonder de referenties van gebruiker.

1. Voeg in uw Android-app-project de volgende constanten toe aan de bovenkant van de **MainActivity** klasse:
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. Klik in de **MainActivity** klasse, bij te werken de `getAuthorizationHeader()` methode bevat de volgende code:
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. Voeg de volgende `import` instructies aan het begin van de **MainActivity** bestand:
   
        import android.content.SharedPreferences;

Wijzig nu de handler die wordt aangeroepen wanneer de melding is ontvangen.

1. In de **MyHandler** klasse wijzigen de `OnReceive()` methode bevat:
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. Voeg de `retrieveNotification()` methode en vervang de tijdelijke aanduiding `{back-end endpoint}` met de back-end-eindpunt dat is verkregen tijdens het implementeren van uw back-end:
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

Deze methode aanroepen van uw app-back-end om op te halen van de inhoud van meldingen met behulp van de referenties die zijn opgeslagen in de gedeelde voorkeuren en weergegeven als een normale melding. De melding ziet er voor de appgebruiker net als elke andere pushmelding.

Is het raadzaam het geval van een eigenschap van de koptekst ontbreekt verificatie- of weigerings-verwerken van de back-end. De specifieke verwerking van dergelijke gevallen is voornamelijk afhankelijk van uw doel-gebruikerservaring. Een optie is om een melding met een algemene prompt voor de gebruiker om te verifiëren om op te halen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd
Als u wilt de toepassing uitvoert, moet u de volgende acties uitvoeren:

1. Zorg ervoor dat **AppBackend** is geïmplementeerd in Azure. Als u Visual Studio gebruikt, voert u de **AppBackend** Web-API-toepassing. Een ASP.NET-webpagina wordt weergegeven.
2. In Eclipse, moet u de app uitvoeren op een fysieke Android-apparaat of de emulator.
3. Voer een gebruikersnaam en wachtwoord in de Android-app UI. Dit kunnen een willekeurige tekenreeks zijn, maar ze moeten dezelfde waarde.
4. Klik in de Android-app UI op **aanmelden**. Klik vervolgens op **pushmelding verzonden**.

