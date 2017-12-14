---
title: Upgradeprocedures van Windows universele Apps SDK
description: Upgradeprocedures van Windows universele Apps SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 2bd2329026057a4a23295880d0d304e4e8effd1c
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a>Upgradeprocedures van Windows universele Apps SDK
Als u hebt al een oudere versie van Engagement geïntegreerd in uw toepassing, hebt u de volgende punten overwegen bij het upgraden van de SDK.

U moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Bijvoorbeeld als u migreert van 0.10.1 naar u moet eerst Volg de procedure 'van 0.9.0 naar 0.10.1' 0.11.0 vervolgens de procedure 'van 0.10.1 naar 0.11.0'.

## <a name="from-330-to-340"></a>Van 3.3.0 naar 3.4.0
### <a name="test-logs"></a>Test-Logboeken
De logboeken van de console die wordt geproduceerd door de SDK kunnen worden ingeschakeld/uitgeschakeld/gefilterd. Werk de eigenschap voor het aanpassen van dit `EngagementAgent.Instance.TestLogEnabled` op een van de waarde in de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Resources
De Reach-overlay is verbeterd. Het uitmaakt deel van de resources SDK NuGet-pakket.

Tijdens een upgrade uitvoeren naar de nieuwe versie van de SDK u kiezen kunt of u behouden van de bestaande bestanden uit de overlay-map van uw resources wilt of niet:

* Als de vorige overlay u werkt of u integreert de `WebView` elementen handmatig vervolgens u bepalen kunt om uw bestaande bestanden te houden, er nog steeds werken. 
* Als u bijwerken naar de nieuwe overlay wilt, vervangt het gehele `overlay` map van uw resources met de nieuwe versie van de SDK-pakket (UWP-apps: na de upgrade kunt u de nieuwe map in de overlay ophalen van % USERPROFILE %\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Met behulp van de nieuwe overlay overschrijft aanpassingen op de vorige versie.
> 
> 

## <a name="from-320-to-330"></a>Van 3.2.0 naar 3.3.0
### <a name="resources"></a>Resources
Deze stap betreft alleen de aangepaste resources. Als u de resources die worden geleverd door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze back-up voordat u de upgrade en pas uw aanpassingen op bijgewerkte bronnen.

## <a name="from-310-to-320"></a>Van 3.1.0 naar 3.2.0
### <a name="resources"></a>Resources
Deze stap betreft alleen de aangepaste resources. Als u de resources die worden geleverd door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze back-up voordat u de upgrade en pas uw aanpassingen op bijgewerkte bronnen.

### <a name="webview-integration"></a>Webweergave-integratie
Enkele verbeteringen aan verschillende apparaten vormfactoren zijn geïntroduceerd in deze versie. Zorg ervoor dat de integratie van de webweergave overeenkomen met het volgende:

In uw pagina-(XAML):

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

En in uw bestand gekoppeld .cs:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-to-300"></a>Van 2.0.0 naar 3.0.0
### <a name="resources"></a>Resources
Deze stap betreft alleen de aangepaste resources. Als u de resources die worden geleverd door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze back-up voordat u de upgrade en pas uw aanpassingen op bijgewerkte bronnen.

## <a name="from-111-to-200"></a>Van 1.1.1 naar 2.0.0
De volgende beschrijft het migreren van een SDK-integratie van de service van Capptain SAS Capptain in een app die is aangedreven door Azure Mobile Engagement. 

> [!IMPORTANT]
> Capptain en Mobile Engagement zijn niet dezelfde services en de procedure die hieronder wordt alleen uitgelegd hoe u voor het migreren van de client-app. Migreren van de SDK in de app wordt niet uw gegevens migreren van de servers Capptain naar de Mobile Engagement-servers
> 
> 

Als u vanaf een eerdere versie migreert, raadpleegt u de Capptain-website om te migreren naar 1.1.1 eerst en vervolgens de volgende procedure toepassen

### <a name="nuget-package"></a>Nuget-pakket
Vervang **Capptain.WindowsPhone** door **MicrosoftAzure.MobileEngagement** Nuget-pakket.

### <a name="applying-mobile-engagement"></a>Mobile Engagement toepassen
De SDK gebruikt de term `Engagement`. U moet uw project zodat deze overeenkomt met deze wijziging bijwerken.

U moet uw huidige Capptain nuget-pakket te verwijderen. Houd rekening met dat uw wijzigingen in de map Capptain bronnen worden verwijderd. Als u wilt behouden die bestanden vervolgens een kopie maken van deze.

Daarna het nieuwe Microsoft Azure Engagement nuget-pakket te installeren op uw project. U vindt deze rechtstreeks op [nuget website]. of hier index. Deze bewerking vervangt alle resources bestanden die worden gebruikt door Engagement en voegt u het nieuwe Engagement dll-bestand toe aan de projectverwijzingen.

U moet uw projectverwijzingen door Capptain DLL verwijzingen te verwijderen. Als u dit niet doet, wordt de versie van Capptain conflict veroorzaken en fouten gebeurt.

Als u Capptain resources hebt aangepast, uw oude bestanden inhoud kopiëren en plak deze in de nieuwe Engagement-bestanden. Houd er rekening mee dat zowel xaml en cs-bestanden moeten worden bijgewerkt.

Wanneer deze stappen zijn voltooid. hoeft u alleen oude Capptain verwijzingen door de nieuwe Engagement verwijzingen moeten worden vervangen.

1. Alle Capptain naamruimten moeten worden bijgewerkt.
   
    Vóór de migratie:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Na de migratie:
   
        using Microsoft.Azure.Engagement;
2. Alle Capptain klassen met 'Capptain' moeten 'Engagement' bevatten.
   
    Vóór de migratie:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Na de migratie:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. Voor xaml-bestanden wijzigen Capptain naamruimte en kenmerken ook.
   
    Vóór de migratie:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Na de migratie:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Overlay pagina wijzigingen
   
   > [!IMPORTANT]
   > Overlay ook gewijzigd. De nieuwe naamruimte is `Microsoft.Azure.Engagement.Overlay`. Er moet worden gebruikt in xaml- en cs-bestanden. Bovendien `CapptainGrid` is naam `EngagementGrid`, `capptain_notification_content` en `capptain_announcement_content` zijn benoemde `engagement_notification_content` en `engagement_announcement_content`.
   > 
   > 
   
    Voor de overlay:
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    Wordt deze:
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. Voor de andere resources zoals Capptain afbeeldingen en HTML-bestanden, houd er rekening mee dat ze ook zijn gewijzigd voor het gebruik van 'Engagement'.

### <a name="project-declaration"></a>Project-declaratie
Op Package.appxmanifest `File Type Associations` is bijgewerkt op basis van:

* capptain\_bereiken\_inhoud engagement\_bereiken\_inhoud
* capptain\_logboek\_bestand engagement\_logboek\_bestand

### <a name="application-id--sdk-key"></a>Toepassings-ID / SDK-sleutel
Engagement maakt gebruik van een verbindingsreeks. U hoeft niet te geven van een toepassings-ID en een SDK-sleutel met Mobile Engagement, hoeft u een verbindingsreeks opgeven. U kunt deze functie instellen op uw EngagementConfiguration-bestand.

De configuratie van de Engagement kan worden ingesteld in uw `Resources\EngagementConfiguration.xml` -bestand van uw project.

Dit bestand op te geven bewerken:

* De verbindingsreeks voor de toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u in plaats daarvan tijdens runtime opgeven wilt, kunt u de volgende methode voordat de initialisatie van de Engagement-agent kunt aanroepen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de Azure Portal.

### <a name="items-name-change"></a>Wijziging van items
Alle items met de naam *capptain* naam hebt gegeven *engagement*. Op dezelfde manier voor *Capptain* naar *Engagement*.

Voorbeelden van veelgebruikte Capptain items:

* CapptainConfiguration EngagementConfiguration nu met de naam
* CapptainAgent EngagementAgent nu met de naam
* CapptainReach EngagementReach nu met de naam
* CapptainHttpConfig EngagementHttpConfig nu met de naam
* GetCapptainPageName GetEngagementPageName nu met de naam

Houd er rekening mee dat rename ook van invloed op overschreven methoden.

