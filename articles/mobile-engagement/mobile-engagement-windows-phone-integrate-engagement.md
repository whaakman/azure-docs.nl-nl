---
title: Windows Phone Silverlight Engagement SDK-integratie
description: Het integreren van Azure Mobile Engagement met Windows Phone Silverlight-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 72a581643ccde55f8b849c511c3365e029d7cbcb
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight Engagement SDK-integratie
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Deze procedure beschrijft de eenvoudigste manier om de Azure Mobile Engagement Analytics en bewaking van functies in uw Windows Phone Silverlight-toepassing te activeren.

De volgende stappen zijn voldoende is voor het activeren van het rapport van logboeken die nodig zijn voor alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals berekenen. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de Engagement-API (Zie [hoe u de geavanceerde Mobile Engagement API in uw Windows Phone Silverlight-app-tagging](mobile-engagement-windows-phone-use-engagement-api.md) hieronder) omdat deze statistieken afhankelijk zijn van toepassing zijn.

## <a name="supported-versions"></a>Ondersteunde versies
De SDK met Engagement Mobile voor Windows Silverlight kan alleen worden geïntegreerd in toepassingen die gericht is op:

* Windows Phone 8.0
* Windows Phone 8.1 Silverlight

> [!NOTE]
> Als u ontwikkelt voor Windows Phone 8.1 (zonder Silverlight) verwijzen naar de [universele Windows-integratie procedure](mobile-engagement-windows-store-integrate-engagement.md).
> 
> 

## <a name="install-the-mobile-engagement-silverlight-sdk"></a>De Mobile Engagement Silverlight-SDK installeren
De SDK met Engagement Mobile voor Windows Silverlight is beschikbaar als een Nuget-pakket aangeroepen *MicrosoftAzure.MobileEngagement*. U kunt het installeren van de Visual Studio Nuget Package Manager. 

## <a name="add-the-capabilities"></a>De mogelijkheden toevoegen
De Engagement SDK moet enkele mogelijkheden van de Windows Phone Silverlight-SDK goede werking.

Open uw `WMAppManifest.xml` -bestand en zorg dat de volgende mogelijkheden zijn gedeclareerd in de `Capabilities` Configuratiescherm:

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-the-engagement-sdk"></a>Initialiseren van de Engagement SDK
### <a name="engagement-configuration"></a>Engagement configuratie
De configuratie van de Engagement is gecentraliseerd in de `Resources\EngagementConfiguration.xml` -bestand van uw project.

Dit bestand op te geven bewerken:

* De verbindingsreeks voor de toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u in plaats daarvan tijdens runtime opgeven wilt, kunt u de volgende methode voordat de initialisatie van de Engagement-agent kunt aanroepen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de Azure Portal.

### <a name="engagement-initialization"></a>De initialisatie van de engagement
Wanneer u een nieuw project maakt een `App.xaml.cs` -bestand is gegenereerd. Deze klasse neemt over van `Application` en bevat veel belangrijke methoden. Dit wordt ook worden gebruikt voor het initialiseren van de Engagement SDK.

Wijzig de `App.xaml.cs`:

* Toevoegen aan uw `using` instructies:
  
      using Microsoft.Azure.Engagement;
* Invoegen `EngagementAgent.Instance.Init` in de `Application_Launching` methode:
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* Invoegen `EngagementAgent.Instance.OnActivated` in de `Application_Activated` methode:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> We afraden raden u om toe te voegen van de initialisatie van de Engagement in een andere locatie van uw toepassing. Echter wel rekening dat de `EngagementAgent.Instance.Init` methode wordt uitgevoerd op een specifieke thread en niet op de UI-thread.
> 
> 

## <a name="basic-reporting"></a>Basic-rapportage
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Aanbevolen methode: overbelasting uw `PhoneApplicationPage` klassen
Om het rapport van de logboeken die zijn vereist voor Engagement berekenen gebruikers, sessies, activiteiten, Crashes en technische statistieken activeert, kunt u gewoon aanbrengen alle uw `PhoneApplicationPage` onderliggende klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van hoe u dit doet voor een pagina van uw toepassing. U kunt hetzelfde voor alle pagina's van uw toepassing doen.

#### <a name="c-source-file"></a>C#-bronbestand
Wijzigen van uw pagina `.xaml.cs` bestand:

* Toevoegen aan uw `using` instructies:
  
      using Microsoft.Azure.Engagement;
* Vervang `PhoneApplicationPage` met `EngagementPage` :

**Zonder Engagement:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Met Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> Als uw pagina van overneemt de `OnNavigatedTo` methode, zorg ervoor dat u kunt de `base.OnNavigatedTo(e)` aanroepen. Anders wordt de activiteit niet gerapporteerd. Inderdaad, de `EngagementPage` aanroept `StartActivity` binnen de `OnNavigatedTo` methode.
> 
> 

#### <a name="xaml-file"></a>XAML-bestand
Wijzigen van uw pagina `.xaml` bestand:

* Toevoegen aan de naamruimtedeclaraties:
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* Vervang `phone:PhoneApplicationPage` met `engagement:EngagementPage` :

**Zonder Engagement:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Met Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Het standaardgedrag negeren
Standaard is de naam van de klasse van de pagina als de naam van de activiteit met zonder extra gerapporteerd. Als de klasse wordt gebruikt voor het achtervoegsel 'Pagina', Engagement wordt het ook verwijderd.

Als u het standaardgedrag voor de naam negeren wilt, gewoon toevoegen aan uw code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Als u wilt een aantal extra informatie aan de activiteit te rapporteren, kunt u dit kunt toevoegen aan uw code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van uw pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: call `StartActivity()` handmatig
Als u niet kunt of niet wilt overbelasten uw `PhoneApplicationPage` klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent` rechtstreeks methoden.

We raden aan aanroepen `StartActivity` binnen uw `OnNavigatedTo` methode van uw PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> Zorg ervoor dat u uw sessie correct beëindigen.
> 
> De SDK automatisch roept de `EndActivity` methode wanneer de toepassing wordt gesloten. Het is dus **maximaal** aanbevolen om aan te roepen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigt, en zo de **nooit** aanroepen de `EndActivity` methode. Deze methode verzendt een bericht naar de server Engagement dat de huidige gebruiker de toepassing is verdwenen en dit heeft gevolgen voor alle toepassingslogboeken.
> 
> 

## <a name="advanced-reporting"></a>Geavanceerde rapportage
U kunt rapport toepassing specifieke gebeurtenissen, fouten en taken, om dit te doen, gebruik eventueel de andere methoden gevonden in de `EngagementAgent` klasse. De API Engagement kunnen alle Engagement geavanceerde mogelijkheden gebruiken.

Zie voor meer informatie [hoe u de geavanceerde Mobile Engagement API in uw Windows Phone Silverlight-app-tagging](mobile-engagement-windows-phone-use-engagement-api.md).

## <a name="advanced-configuration"></a>Geavanceerde configuratie
### <a name="disable-automatic-crash-reporting"></a>Automatische crashrapporten uitschakelen
U kunt de automatische crash rapportagefunctie van Engagement uitschakelen. Vervolgens, als er wordt een niet-verwerkte uitzondering optreedt, Engagement is geen alles.

> [!WARNING]
> Als u van plan bent om deze functie uitschakelen, houd er rekening mee dat wanneer een niet-verwerkte crash wordt uitgevoerd in uw app, Engagement niet de crash stuurt worden **en** deze sessie en taken niet gesloten.
> 
> 

Schakel automatische crashrapporten alleen uw configuratie, afhankelijk van de manier waarop die u het gedeclareerd aanpassen:

#### <a name="from-engagementconfigurationxml-file"></a>Van `EngagementConfiguration.xml` bestand
Rapport crash ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` labels.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Van `EngagementConfiguration` object tijdens runtime
Rapport crash ingesteld op false met behulp van uw EngagementConfiguration-object.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-modus
Standaard wordt de Engagement-service-rapporten Logboeken in realtime. Als uw toepassing Logboeken heel vaak rapporteert, is het beter voor het bufferen van de logboeken en om te rapporteren ze allemaal tegelijk op een vaste tijd base (dit is de modus' burst' genoemd).

Roep de methode om dit te doen:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde in **milliseconden**. Op elk gewenst moment als u activeren, de real-time logboekregistratie wilt, roept de methode geen parameters of met de waarde 0.

De modus ' burst ' iets langere levensduur maar heeft een invloed op de Monitor Engagement: alle sessies en taken duur wordt afgerond op de drempelwaarde burst (dus sessies en korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken). Het is raadzaam een ' burst ' drempelwaarde niet langer dan 30000 (30s) gebruiken. U moet er rekening mee dat opgeslagen logboeken zijn beperkt tot 300 items. U kunt sommige logboeken verliezen als verzenden te lang is.

> [!WARNING]
> De drempelwaarde burst kan niet worden geconfigureerd op een periode van minder dan één seconde. Als u probeert te doen zodat de SDK een tracering met de fout en wordt automatisch opnieuw wordt ingesteld op de standaardwaarde wordt weergegeven dat wil zeggen, nul seconden. Dit activeert de SDK om de logboeken in realtime te rapporteren.
> 
> 

