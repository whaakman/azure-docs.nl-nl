---
title: Universele Windows-Apps bereiken SDK-integratie
description: Het integreren van Azure Mobile Engagement bereiken met universele Windows-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 9311e998e67d8d0d56da68fc9460df32ce7ce5a9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a>Universele Windows-Apps bereiken SDK-integratie
U moet volgen de procedure integratie in de [Windows universele Engagement SDK-integratie](mobile-engagement-windows-store-integrate-engagement.md) voordat u deze handleiding.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>De SDK voor het bereiken van Engagement insluiten in uw universele Windows-project
U beschikt niet over iets om toe te voegen. `EngagementReach`verwijzingen en bronnen bevinden zich al in uw project.

> [!TIP]
> U kunt afbeeldingen die zich in de `Resources` map van uw project, met name het merk-pictogram (die standaard ingesteld op het pictogram Engagement). Op de universele Apps u kunt ook verplaatsen de `Resources` map op uw gedeelde project voor het delen van de inhoud ervan tussen apps, maar u moet houden de `Resources\EngagementConfiguration.xml` bestand op de standaardlocatie, omdat deze afhankelijk platform.
> 
> 

## <a name="enable-the-windows-notification-service"></a>De Windows Notification Service inschakelen
### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x en Windows Phone 8.1 alleen
Als u wilt gebruiken de **Windows Notification Service** (WNS genoemd) in uw `Package.appxmanifest` -bestand in de `Application UI` klikt u op `All Image Assets` in het linkerdeelvenster bot. Aan de rechterkant van het vak in `Notifications`, wijzigen `toast capable` van `(not set)` naar `(Yes)`.

### <a name="all-platforms"></a>Alle platforms
U moet synchroniseren van uw app aan je Microsoft-account als de engagement-platform. Daarvoor moet u voor het maken van een account of meld u aan [windows-ontwikkelaarscentrum](https://dev.windows.com). Hierna een nieuwe toepassing maken en de SID en de geheime sleutel zoeken. Ga op uw app-instelling in op de front-end engagement `native push` en plak uw referenties. Hierna, klik met de rechtermuisknop op uw project, selecteer `store` en `Associate App with the Store...`. U hoeft de toepassing die u hebt maakt voordat te synchroniseren te selecteren.

## <a name="initialize-the-engagement-reach-sdk"></a>Initialiseren van het bereik Engagement SDK
Wijzig de `App.xaml.cs`:

* Invoegen `EngagementReach.Instance.Init` vlak na `EngagementAgent.Instance.Init` in uw `InitEngagement` methode:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  De `EngagementReach.Instance.Init` in een specifieke thread wordt uitgevoerd. U beschikt niet over het zelf te doen.

> [!NOTE]
> Als u pushmeldingen elders in uw toepassing is u [delen van het kanaal push](#push-channel-sharing) met Engagement bereiken.
> 
> 

## <a name="integration"></a>Integratie
Engagement biedt twee manieren om de Reach in-app-banner en verspreide weergaven voor aankondigingen en polls toevoegen in uw toepassing: de overlay-integratie en de handmatige integratie van web-weergaven. Beide benadering op dezelfde pagina moeten niet worden gecombineerd.

De keuze tussen de twee integratie kan op deze manier worden samengevat:

* U kunt ervoor kiezen de overlay-integratie als uw pagina's neemt al over van de Agent `EngagementPage`, het is alleen een kwestie van het vervangen van `EngagementPage` door `EngagementPageOverlay` en `xmlns:engagement="using:Microsoft.Azure.Engagement"` door `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in uw pagina's.
* U kunt de webweergaven handmatige integratie kiezen als u wilt de gebruikersinterface bereiken in de pagina's nauwkeurig te plaatsen of als u niet wilt dat een ander overnameniveau toevoegen aan uw pagina's. 

### <a name="overlay-integration"></a>Overlay-integratie
De Engagement-overlay worden dynamisch toegevoegd voor de UI-elementen waarmee Reach-campagnes in uw pagina weergegeven. Als de overlay niet aan de behoeften van uw lay-out moet u de webweergaven handmatige integratie in plaats daarvan.

In het .xaml-bestandswijziging `EngagementPage` verwijzing naar`EngagementPageOverlay`

* Toevoegen aan de naamruimtedeclaraties:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* Vervang `engagement:EngagementPage` met `engagement:EngagementPageOverlay`:

**Met EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

**Met EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Klik in het bestand .cs tag uw pagina in `EngagementPageOverlay` in plaats van `EngagementPage` en importeer `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

* Vervang `EngagementPage` met `EngagementPageOverlay`:

**Met EngagementPage:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Met EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


De overlay Engagement voegt een `Grid` element boven op de pagina bestaat uit de indeling en de twee `WebView` elementen één voor de banner en andere die voor de verspreide weergave.

U kunt de overlay elementen rechtstreeks in de `EngagementPageOverlay.cs` bestand.

### <a name="web-views-manual-integration"></a>Handmatige integratie met het web weergaven
Reach zoeken op de pagina's voor de twee naar `WebView` elementen die verantwoordelijk is voor het weergeven van de banner en de verspreide weergave. Het enige wat u hoeft te doen is het toevoegen van deze twee `WebView` elementen ergens in de pagina's, Hier volgt een voorbeeld:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


In dit voorbeeld de `WebView` elementen worden uitgerekt aanpassen aan de container die automatisch opnieuw groottes in geval van een scherm worden gedraaid of venster grootte wijzigen.

> [!WARNING]
> Het is belangrijk dat de dezelfde naming `engagement_notification_content` en `engagement_announcement_content` voor de `WebView` elementen. Reach is identificeren met hun naam. 
> 
> 

## <a name="handle-datapush-optional"></a>Ingang datapush (optioneel)
Als u wilt dat uw toepassing te kunnen ontvangen van gegevens-pushes Reach, hebt u twee gebeurtenissen van de klasse EngagementReach implementeren:

Voeg in App.xaml.cs in de constructor App() toe:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

U kunt zien dat de callback van elke methode een Booleaanse waarde retourneert. Engagement verzendt een feedback naar de back-end nadat de gegevens-push. Als de callback ONWAAR is, retourneert de `exit` feedback verzenden zijn. Anders moeten `action`. Als er geen retouraanroep is ingesteld voor de gebeurtenissen de `drop` feedback engagement worden geretourneerd.

> [!WARNING]
> Engagement is niet ontvangen veelvouden feedback voor een gegevens-push. Als u wilt meerdere handlers ingesteld op een gebeurtenis, houd er rekening mee dat de feedback met de laatste overeen wordt een verzonden. In dit geval raden we retourneert altijd dezelfde waarde om te voorkomen dat verwarrend feedback op de front-end.
> 
> 

## <a name="customize-ui-optional"></a>Aanpassen van de gebruikersinterface (optioneel)
### <a name="first-step"></a>Eerste stap
We laten u aanpassen van de gebruikersinterface van het bereik.

Om dit te doen, u moet maken van een subklasse zijn van de `EngagementReachHandler` klasse.

**Voorbeeld van Code:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Vervolgens stelt u de inhoud van de `EngagementReach.Instance.Handler` veld met het aangepaste object in uw `App.xaml.cs` klasse binnen de `App()` methode.

**Voorbeeld van Code:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> Engagement maakt standaard gebruik van een eigen implementatie van `EngagementReachHandler`.
> U hoeft te maken van uw eigen en als u doet dit, hebt u niet elke methode overschrijven. Het standaardgedrag is het selecteren van het basisobject Engagement.
> 
> 

### <a name="web-view"></a>Webweergave
Standaard Reach ingesloten resources van het DLL-bestand gebruikt om de meldingen en pagina's weer te geven.

Als u wilt een volledige aanpassing mogelijkheden bieden we alleen webweergave gebruiken. Als u aanpassen indelingen wilt, overschrijven rechtstreeks de bestanden resources `EngagementAnnouncement.html` en `EngagementNotification.html`. Engagement moet alle code in `<body></body>` correct uit te voeren. Maar u kunt de buitenste tag toevoegen `engagement_webview_area`.

U kunt echter besluiten om uw eigen bronnen te gebruiken.

U kunt onderdrukken `EngagementReachHandler` methoden in uw subklasse vertellen Engagement uw indelingen gebruiken, maar zorgt voor ingesloten het mechanisme voor engagement:

**Voorbeeld van Code:**

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


AnnouncementHTML is standaard `ms-appx-web:///Resources/EngagementAnnouncement.html`. Hiermee geeft u de html-bestand dat de inhoud van een pushbericht (tekst aankondiging, Web anoucement en Poll aankondiging) ontwerpen. AnnouncementName is `engagement_announcement_content`. Dit is de naam van het ontwerp webweergave in xaml-pagina.

NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`. Hiermee geeft u de html-bestand dat de melding van een pushbericht ontwerpen. NotfificationName is `engagement_notification_content`. Dit is de naam van het ontwerp webweergave in xaml-pagina.

### <a name="customization"></a>Aanpassing
U kunt de melding en aankondiging webweergave is dat u wilt dat als u Engagement object behouden aanpassen. Maatregelen treffen die webweergave object driemaal - de eerst in uw xaml, de tweede keer in uw bestand .cs in de methode 'setwebview()' en de derde tijd in het HTML-bestand wordt beschreven.

* In de xaml beschrijft u de huidige grafische indeling webweergave onderdeel.
* U kunt in uw bestand .cs 'setwebview()' waarin u de dimensie van de twee Webweergave (melding, aankondiging) instellen definiëren. Het is zeer effectief zijn wanneer het formaat van de toepassing.
* In het html-bestand Engagement beschrijven we de inhoud van webweergave-, ontwerp- en de elementen posities tussen elkaar.

### <a name="launch-message"></a>Bericht starten
Wanneer een gebruiker klikt op een Systeemmelding (een pop-up), Engagement, start de toepassing, de inhoud van de pushberichten laden en de pagina voor de bijbehorende campagne weergegeven.

Er is een vertraging tussen het starten van de toepassing en de weergave van de pagina (afhankelijk van de snelheid van uw netwerk).

Om aan te geven aan de gebruiker iets wordt geladen, moet u een visuele gegevens, zoals een voortgangsbalk zien of een voortgangsindicator opgeven. Engagement kan niet worden verwerkt, maar enkele handlers voor u biedt.

Voor het implementeren van de callback in App.xaml.cs in "Openbare App() {}" toevoegen:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

U kunt de callback instellen in de methode 'Openbare App() {}' van uw `App.xaml.cs` bestand, bij voorkeur voordat de `EngagementReach.Instance.Init()` aanroepen.

> [!TIP]
> Elke handler wordt aangeroepen door de UI-Thread. U hebt geen zorgen te maken wanneer u een MessageBox of iets UI-gerelateerde.
> 
> 

## <a id="push-channel-sharing"></a>Push-kanaal voor delen
Als u pushmeldingen voor een ander doel in uw toepassing hebt u de functie van de Engagement SDK voor het delen push-kanaal te gebruiken. Dit is om te voorkomen dat gemiste push.

* U kunt uw eigen push-kanaal naar de initialisatie van de Engagement bereiken opgeven. De SDK wordt gebruikt in plaats van een nieuwe aanvragen.

De initialisatie van de Engagement bereiken bijwerken met de push-kanaal in de `InitEngagement` methode van de `App.xaml.cs` bestand:

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* U kunt ook als u alleen wilt gebruiken voor de push-kanaal nadat de initialisatie van de bereiken en u een retouraanroep op Engagement bereiken instellen kunt voor de push-kanaal eenmaal is gemaakt door de SDK.

De retouraanroep ingesteld op een willekeurige plaats **nadat** de initialisatie van de bereiken:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Aangepaste schema tip
We bieden gebruik van aangepaste schema. Via frontend engagement moet worden gebruikt in uw toepassing engagement kunt u verschillende type URI verzenden. Standaardschema zoals `http, ftp, ...` zijn beheren in Windows, met een venster wordt gevraagd als ze geen standaardtoepassing op apparaat geïnstalleerd. U kunt ook uw eigen aangepaste schema maken voor uw toepassing.

De eenvoudige manier om in te stellen van een aangepast schema in uw toepassing te openen is uw `Package.appxmanifest` Ga in `Declarations` Configuratiescherm. Selecteer `Protocol` Schuif in de beschikbare declaraties vak en toe te voegen. Bewerk de `Name` veld met uw nieuwe protocol de gewenste naam.

Nu voor het gebruik van dit protocol bewerken uw `App.xaml.cs` met de `OnActivated` methode en vergeet niet initialiseren engagement hier ook:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action to do when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

