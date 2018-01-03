---
title: Beveiligde Pushmeldingen verzenden met Azure Notification Hubs
description: Informatie over het veilig pushmeldingen verzendt naar een Android-app van Azure. Codevoorbeelden geschreven in Java en C#.
documentationcenter: android
keywords: push-melding, pushmeldingen, push-berichten, android pushmeldingen
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 29f8c516e611c13fb73c7edc15e7c52708c75bb0
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
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

Push notification-ondersteuning in Microsoft Azure kunt u toegang tot een push eenvoudig te gebruiken, meerdere platforms, uitgebreid bericht-infrastructuur, die aanzienlijk de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt.

Vanwege regelgeving of veiligheidsbeperkingen, soms een toepassing wilt iets opnemen in de melding die via de standaard push notification-infrastructuur kan niet worden verzonden. Deze zelfstudie wordt beschreven hoe u dezelfde ervaring bereiken door te sturen van gevoelige gegevens via een veilige en geverifieerde verbinding tussen de client Android-apparaat en de back-end voor de app.

Op een hoog niveau is de stroom als volgt uit:

1. De back-end app:
   * Winkels beveiligde nettolading in back-enddatabase.
   * De ID van deze melding verzendt naar de Android-apparaat (geen beveiligde gegevens verzonden).
2. De app op het apparaat tijdens het ontvangen van de melding:
   * Het Android-apparaat neemt contact op met de back-end de nettolading van de beveiligde aanvragen.
   * De app kan de nettolading van de weergegeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), gaan we ervan uit dat het apparaat geen verificatietoken in de lokale opslag opslaat nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze ervaring als het apparaat de melding van de beveiligde nettolading met dit token kan ophalen. Als uw toepassing geen verificatietokens opslaat op het apparaat, of als deze tokens kunnen verlopen, moet een algemene melding dat de gebruiker wordt gevraagd om de app te starten door de app apparaat bij ontvangst van de pushmelding worden weergegeven. De app vervolgens verifieert de gebruiker en toont de nettolading van de meldingen.

Deze zelfstudie laat zien hoe beveiligde pushmeldingen te verzenden. Dit is gebaseerd op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) zelfstudie, dus moet u de stappen in deze zelfstudie eerst als u dat nog niet gedaan hebt.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Wijzigen van het Android-project
Nu dat u uw back-end app verzenden gewijzigd alleen de *id* van een push-melding die u moet wijzigen van uw Android-app voor het verwerken van deze kennisgeving en terugbellen van uw back-end voor het ophalen van het beveiligde bericht moet worden weergegeven.
U hebt voor dit doel te bereiken, om ervoor te zorgen dat uw Android-app hoe zichzelf verifiëren met uw back-end weet wanneer deze de pushmeldingen ontvangt.

Er wordt nu wijzigen de *aanmelding* stroom om op te slaan van de waarde van de verificatie-header in de gedeelde voorkeuren van uw app. Vergelijkbaar mechanismen kunnen worden gebruikt voor het opslaan van een verificatietoken (bijvoorbeeld OAuth tokens) die de app gebruiken moet zonder dat de referenties van gebruiker.

1. Voeg in uw project Android-app de volgende constanten zijn aan de bovenkant van de **MainActivity** klasse:
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. Nog steeds in de **MainActivity** klasse, werkt de `getAuthorizationHeader()` methode bevat de volgende code:
   
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

Er wordt nu de handler die wordt aangeroepen wanneer de melding is ontvangen wijzigen.

1. In de **MyHandler** klasse wijzigen de `OnReceive()` methode bevat:
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. Voeg vervolgens de `retrieveNotification()` methode, vervang de tijdelijke aanduiding voor `{back-end endpoint}` met het back-end-eindpunt dat is verkregen tijdens de implementatie van uw back-end:
   
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

Deze methode aanroept van uw app-back-end voor het ophalen van de inhoud van de melding met de referenties die zijn opgeslagen in de gedeelde voorkeuren en als een normale melding weergegeven. De melding ziet er voor de gebruiker app net als andere push-melding.

Houd er rekening mee dat is het raadzaam om het geval van een eigenschap van de header ontbreekt verificatie of afwijzing verwerkt door de back-end. De specifieke verwerking van deze gevallen afhankelijk zijn van voornamelijk op de doel-gebruikerservaring. Een mogelijkheid is om een melding met een algemene prompt voor de gebruiker te verifiëren voor het ophalen van de werkelijke melding weer te geven.

## <a name="run-the-application"></a>De toepassing uitvoeren
Voor het uitvoeren van de toepassing, het volgende doen:

1. Zorg ervoor dat **AppBackend** is geïmplementeerd in Azure. Als u Visual Studio, voert u de **AppBackend** Web API-toepassing. Een ASP.NET-webpagina wordt weergegeven.
2. Voer de app op een fysieke Android-apparaat of de emulator in Eclipse.
3. Voer een gebruikersnaam en wachtwoord in de Android-app gebruikersinterface. Deze kunnen zich een willekeurige tekenreeks, maar moeten dezelfde waarde.
4. Klik in de Android-app gebruikersinterface op **aanmelden**. Klik vervolgens op **push verzenden**.

