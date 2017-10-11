---
title: Android webweergave Bridge met systeemeigen Mobile Engagement Android SDK
description: Beschrijft hoe u maakt een brug tussen webweergave met Javascript en de systeemeigen Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: cf272f3f-2b09-41b1-b190-944cdca8bba2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4fc7b3c81747ec80974a99084eeb1acc311f11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="bridge-android-webview-with-native-mobile-engagement-android-sdk"></a>Android webweergave Bridge met systeemeigen Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Android-brug](mobile-engagement-bridge-webview-native-android.md)
> * [iOS-brug](mobile-engagement-bridge-webview-native-ios.md)
> 
> 

Sommige mobiele apps zijn ontworpen als een hybride-app waarin de app zelf is ontwikkeld met behulp van systeemeigen Android-ontwikkeling, maar sommige of zelfs alle van de schermen worden weergegeven binnen een webweergave Android. U kunt nog steeds Mobile Engagement Android SDK verbruiken binnen deze apps en deze zelfstudie wordt beschreven hoe gaat over dit te doen. De onderstaande voorbeeldcode is gebaseerd op de Android-documentatie [hier](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript). Hierin wordt beschreven hoe deze gedocumenteerde benadering kan worden gebruikt voor het implementeren van hetzelfde voor Mobile Engagement Android SDK van veelgebruikte methoden zodat een webweergave in een hybride-app ook aanvragen Houd gebeurtenissen, taken, fouten, app-info terwijl ze via onze Android SDK sluizen kan initiëren. 

1. Ten eerste moet u ervoor zorgen dat u hebt doorlopen onze [zelfstudie aan de slag](mobile-engagement-android-get-started.md) voor het integreren van de Mobile Engagement Android SDK in uw hybride-app. Als u dit doen, uw `OnCreate` methode ziet er als volgt.  
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("<Mobile Engagement Conn String>");
            EngagementAgent.getInstance(this).init(engagementConfiguration);
        }
2. Controleren of dat uw hybride-app een scherm met een webweergave is geïnstalleerd op. De code voor deze zijn vergelijkbaar met het volgende waar we een lokaal HTML-bestand worden geladen **Sample.html** in de webweergave in de `onCreate` methode van het scherm. 
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
        }
   
        protected void onCreate(Bundle savedInstanceState) {
            ...
            ...
            SetWebView();
        }
3. Maak nu een brug bestand met de naam **WebAppInterface** met behulp van Mobile Engagement Android SDK-methoden die Hiermee maakt u een wrapper voor sommige vaak gebruikt de `@JavascriptInterface` benadering wordt beschreven in de [Android documentatie](https://developer.android.com/guide/webapps/webview.html#BindingJavaScript):
   
        import android.content.Context;
        import android.os.Bundle;
        import android.util.Log;
        import android.webkit.JavascriptInterface;
   
        import com.microsoft.azure.engagement.EngagementAgent;
   
        import org.json.JSONArray;
        import org.json.JSONObject;
   
        public class WebAppInterface {
            Context mContext;
   
            /** Instantiate the interface and set the context */
            WebAppInterface(Context c) {
                mContext = c;
            }
   
            @JavascriptInterface
            public void sendEngagementEvent(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendEvent(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void startEngagementJob(String name, String extras ){
                EngagementAgent.getInstance(mContext).startJob(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void endEngagementJob(String name){
                EngagementAgent.getInstance(mContext).endJob(name);
            }
   
            @JavascriptInterface
            public void sendEngagementError(String name, String extras ){
                EngagementAgent.getInstance(mContext).sendError(name, ParseExtras(extras));
            }
   
            @JavascriptInterface
            public void sendEngagementAppInfo(String appInfo){
                EngagementAgent.getInstance(mContext).sendAppInfo(ParseExtras(appInfo));
            }
   
            public Bundle ParseExtras(String input) {
                Bundle extras = new Bundle();
   
                try {
                    JSONObject jObject = new JSONObject(input);
                    extras.putString(jObject.names().getString(0),
                            jObject.get(jObject.names().getString(0)).toString());
                } catch (Exception e) {
                    e.printStackTrace();
                }
                return extras;
            }
        }  
4. Nadat we de bovenstaande bridge-bestand hebt gemaakt, moeten we ervoor te zorgen dat deze gekoppeld aan onze webweergave is. Dit gebeurt, moet u uw `SetWebview` methode zodat deze er als volgt:
   
        private void SetWebView() {
            WebView myWebView = (WebView) findViewById(R.id.webview);
            myWebView.loadUrl("file:///android_asset/Sample.html");
            WebSettings webSettings = myWebView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            myWebView.addJavascriptInterface(new WebAppInterface(this), "EngagementJs");
        }
5. In het bovenstaande codefragment we aangeroepen `addJavascriptInterface` onze klasse bridge koppelen aan onze webweergave en ook een ingang aangeroepen gemaakt **EngagementJs** de methoden uit het bestand bridge aanroepen. 
6. Maak nu het volgende bestand aangeroepen **Sample.html** in uw project in een map genaamd **activa** die in de webweergave wordt geladen en waar noemen we de methoden uit het bestand bridge.
   
        <!doctype html>
        <html>
            <head>
                <style type='text/css'>
                    html { font-family:Helvetica; color:#222; }
                    h1 { color:steelblue; font-size:22px; margin-top:16px; }
                    h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
                </style>
   
                <script type="text/javascript">
   
                    window.onerror = function(err)
                    {
                      log('window.onerror: ' + err);
                    }
   
                    send = function(inputId)
                    {
                        var input = document.getElementById(inputId);
                        if(input)
                        {
                            var value = input.value;
                            // Example of how extras info can be passed with the Engagement logs
                            var extras = '{"CustomerId":"MS290011"}';
   
                            if(value && value.length > 0)
                            {
                                switch(inputId)
                                {
                                    case "event":
                                    EngagementJs.sendEngagementEvent(value, extras);
                                    break;
   
                                    case "job":
                                    EngagementJs.startEngagementJob(value, extras);
                                    window.setTimeout( function()
                                    {
                                      EngagementJs.endEngagementJob(value);
                                    }, 10000 );
                                    break;
   
                                    case "error":
                                    EngagementJs.sendEngagementError(value, extras);
                                    break;
   
                                    case "appInfo":
                                    EngagementJs.sendEngagementAppInfo({"customer_name":value});
                                    break;
                                }
                            }
                        }
                    }
                </script>
            </head>
            <body>
                <h1>Bridge Tester</h1>
                <div id='engagement'>
                    <h2>Event</h2>
                    <input type="text" id="event" size="35">
                    <button onclick="send('event')">Send</button>
   
                    <h2>Job</h2>
                    <input type="text" id="job" size="35">
                    <button onclick="send('job')">Send</button>
   
                    <h2>Error</h2>
                    <input type="text" id="error" size="35">
                    <button onclick="send('error')">Send</button>
   
                    <h2>AppInfo</h2>
                    <input type="text" id="appInfo" size="35">
                    <button onclick="send('appInfo')">Send</button>
                </div>
            </body>
        </html>
7. Houd rekening met de volgende punten over het bovenstaande HTML-bestand:
   
   * Het bevat een set van invoervakken waarin u gegevens moet worden gebruikt als namen voor de gebeurtenis, taak, fout, AppInfo kunt opgeven. Wanneer u op de knop ernaast klikt, wordt een aanroep Javascript die uiteindelijk de methoden aanroept vanuit het bestand bridge om door te geven deze aanroep van Mobile Engagement Android SDK. 
   * We zijn labels op sommige statische extra gegevens op de gebeurtenissen, taken en zelfs fouten ter illustratie van hoe deze kan worden uitgevoerd. Deze extra gegevens als een JSON-tekenreeks die wordt verzonden als u zoeken in de `WebAppInterface` bestand wordt geparseerd en plaatsen in een Android `Bundle` en samen met het verzenden van gebeurtenissen, taken, fouten wordt doorgegeven. 
   * Een Mobile Engagement-taak wordt gestart met de naam die u opgeeft in het invoervak voor tien seconden uitgevoerd en afsluiten. 
   * Een Mobile Engagement appinfo of tag wordt doorgegeven met 'customer_name' als de statische sleutel en de waarde die u hebt ingevoerd in de invoerclaimset als de waarde voor het label. 
8. Voer de app en u ziet het volgende. Nu een naam opgeven voor een testgebeurtenis als volgt uit en klik op **verzenden** eronder. 
   
    ![][1]
9. Nu wanneer u naar de **Monitor** tabblad van uw app en kijk onder **Events -> Details**, ziet u deze gebeurtenis weergegeven samen met de statische app-gegevens die we sturen. 
   
   ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-android/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-android/event-output.png
