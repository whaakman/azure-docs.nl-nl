---
title: Windows universele Apps Engagement SDK-integratie
description: Het integreren van Azure Mobile Engagement met universele Windows-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 56a382a348609df1d1d308aeac39f47ca82ac4c8
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows universele Apps Engagement SDK-integratie
> [!div class="op_single_selector"]
> * [Universeel Windows](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Deze procedure beschrijft de eenvoudigste manier Engagement Analytics en bewaking van functies in uw universele Windows-toepassing te activeren.

De volgende stappen zijn voldoende is voor het activeren van het rapport van logboeken die nodig zijn voor alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals berekenen. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de Engagement-API (Zie [hoe u de geavanceerde Mobile Engagement API in uw universele Windows-app-tagging](mobile-engagement-windows-store-use-engagement-api.md) aangezien deze statistieken afhankelijk van de toepassing zijn.

## <a name="supported-versions"></a>Ondersteunde versies
De Mobile Engagement SDK voor Windows universele Apps kunnen alleen worden geïntegreerd in Windows Runtime en universele Windows-Platform-toepassingen die gericht is op:

* Windows 8
* Windows 8.1
* Windows Phone 8.1
* Windows 10 (desktop en mobiel families)

> [!NOTE]
> Als u voor Windows Phone Silverlight ontwikkelt vervolgens naar verwijzen de [Windows Phone Silverlight-integratie procedure](mobile-engagement-windows-phone-integrate-engagement.md).
> 
> 

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>De Mobile Engagement universele Apps SDK installeren
### <a name="all-platforms"></a>Alle platforms
De Mobile Engagement SDK voor universele Windows-App is beschikbaar als een Nuget-pakket aangeroepen *MicrosoftAzure.MobileEngagement*. U kunt het installeren van de Visual Studio Nuget Package Manager.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x en Windows Phone 8.1
NuGet implementeert automatisch de SDK-resources in de `Resources` map in de hoofdmap van uw toepassingsproject.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 Universal Windows Platform-toepassingen
NuGet, distribueert de SDK-resources in uw UWP-toepassing nog niet automatisch. U hebben moet dit handmatig doen totdat de implementatie van resources wordt teruggeplaatst in NuGet:

1. Open de Verkenner.
2. Navigeer naar de volgende locatie (**x.x.x** is de versie van Engagement u installeert): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3. Slepen en neerzetten de **Resources** map vanuit de Verkenner naar de hoofdmap van uw project in Visual Studio.
4. Selecteer uw project in Visual Studio en activeren de **weergeven van alle bestanden** pictogram boven de **Solution Explorer**.
5. Sommige bestanden zijn niet opgenomen in het project. Voor het importeren van deze in één keer Klik met de rechtermuisknop op de **Resources** map **uitsluiten van project** en vervolgens een andere Klik met de rechtermuisknop op de **Resources** map **opnemen in het project** opnieuw ook de hele map. Alle bestanden uit de **Resources** map nu zijn opgenomen in uw project.

## <a name="add-the-capabilities"></a>De mogelijkheden toevoegen
De Engagement SDK moet enkele mogelijkheden van de Windows SDK goede werking.

Open uw `Package.appxmanifest` -bestand en zorg dat de volgende mogelijkheden worden gedefinieerd:

* `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>Initialiseren van de Engagement SDK
### <a name="engagement-configuration"></a>Engagement configuratie
De configuratie van de Engagement is gecentraliseerd in de `Resources\EngagementConfiguration.xml` -bestand van uw project.

Dit bestand op te geven bewerken:

* De verbindingsreeks voor de toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u in plaats daarvan tijdens runtime opgeven wilt, kunt u de volgende methode voordat de initialisatie van de Engagement-agent kunt aanroepen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de Azure Portal.

### <a name="engagement-initialization"></a>De initialisatie van de engagement
Wanneer u een nieuw project maakt een `App.xaml.cs` -bestand is gegenereerd. Deze klasse neemt over van `Application` en bevat veel belangrijke methoden. Dit wordt ook worden gebruikt voor het initialiseren van de Engagement SDK.

Wijzig de `App.xaml.cs`:

* Toevoegen aan uw `using` instructies:
  
      using Microsoft.Azure.Engagement;
* Een methode voor het delen van de initialisatie van de Engagement eenmaal voor alle aanroepen definiëren:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* Roep `InitEngagement` in de `OnLaunched` methode:
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* Wanneer uw toepassing wordt gestart met een aangepast schema, een andere toepassing of de opdrachtregel en vervolgens de `OnActivated` methode wordt aangeroepen. U moet ook de Engagement SDK starten wanneer uw app wordt geactiveerd. Om dit te doen overschrijven `OnActivated` methode:
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> We afraden raden u om toe te voegen van de initialisatie van de Engagement in een andere locatie van uw toepassing.
> 
> 

## <a name="basic-reporting"></a>Basic-rapportage
### <a name="recommended-method-overload-your-page-classes"></a>Aanbevolen methode: overbelasting uw `Page` klassen
Om het rapport van de logboeken die zijn vereist voor Engagement berekenen gebruikers, sessies, activiteiten, Crashes en technische statistieken activeert, kunt u gewoon aanbrengen alle uw `Page` onderliggende klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van hoe u dit doet voor een pagina van uw toepassing. U kunt hetzelfde voor alle pagina's van uw toepassing doen.

#### <a name="c-source-file"></a>C#-bronbestand
Wijzigen van uw pagina `.xaml.cs` bestand:

* Toevoegen aan uw `using` instructies:
  
      using Microsoft.Azure.Engagement;
* Vervang `Page` met `EngagementPage`:

**Zonder Engagement:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Met Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> Als uw pagina de methode `OnNavigatedTo` negeert, zorg er dan voor dat u `base.OnNavigatedTo(e)` aanroept. Anders wordt de activiteit niet gerapporteerd (de `EngagementPage` roept `StartActivity` aan binnen de methode `OnNavigatedTo`).
> 
> 

#### <a name="xaml-file"></a>XAML-bestand
Wijzigen van uw pagina `.xaml` bestand:

* Toevoegen aan de naamruimtedeclaraties:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* Vervang `Page` met `engagement:EngagementPage`:

**Zonder Engagement:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Met Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Standaard overschrijven
Standaard is de naam van de klasse van de pagina als de naam van de activiteit met zonder extra gerapporteerd. Als de klasse wordt gebruikt voor het achtervoegsel 'Pagina', Engagement wordt het ook verwijderd.

Als u onderdrukken van het standaard-gedrag voor de naam wilt, gewoon toevoegen aan uw code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Als u rapporteren enkele extra informatie over met de activiteit wilt, kunt u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van uw pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: call `StartActivity()` handmatig
Als u niet kunt of niet wilt overbelasten uw `Page` klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent` rechtstreeks methoden.

Het is raadzaam om aan te roepen `StartActivity` binnen uw `OnNavigatedTo` methode van uw pagina.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Zorg ervoor dat u uw sessie correct beëindigen.
> 
> De universele Windows SDK automatisch roept de `EndActivity` methode wanneer de toepassing wordt gesloten. Het is dus **maximaal** aanbevolen om aan te roepen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigt, en zo de **nooit** aanroepen de `EndActivity` methode deze methode verzendt naar Engagement server dat de huidige gebruiker de toepassing heeft verlaten, deze van invloed is op alle toepassingslogboeken.
> 
> 

## <a name="advanced-reporting"></a>Geavanceerde rapportage
U kunt rapport toepassing specifieke gebeurtenissen, fouten en taken, om dit te doen, gebruik eventueel de andere methoden gevonden in de `EngagementAgent` klasse. De API Engagement kunnen alle Engagement geavanceerde mogelijkheden gebruiken.

Zie voor meer informatie [hoe u de geavanceerde Mobile Engagement API in uw universele Windows-app-tagging](mobile-engagement-windows-store-use-engagement-api.md).

## <a name="advanced-configuration"></a>Geavanceerde configuratie
### <a name="disable-automatic-crash-reporting"></a>Automatische crashrapporten uitschakelen
U kunt de automatische crash rapportagefunctie van Engagement uitschakelen. Vervolgens, als er wordt een niet-verwerkte uitzondering optreedt, Engagement is geen alles.

> [!WARNING]
> Als u van plan bent om deze functie uitschakelen, houd er rekening mee dat wanneer een niet-verwerkte crash wordt uitgevoerd in uw app, Engagement niet de crash stuurt worden **en** de sessie en taken niet gesloten.
> 
> 

Schakel automatische crashrapporten alleen uw configuratie, afhankelijk van de manier waarop die u het gedeclareerd aanpassen:

#### <a name="from-engagementconfigurationxml-file"></a>Van `EngagementConfiguration.xml` bestand
Rapport crash ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` labels.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Van `EngagementConfiguration` object tijdens runtime
Rapport crash ingesteld op false met behulp van uw EngagementConfiguration-object.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-modus
Standaard wordt de Engagement-service-rapporten Logboeken in realtime. Als uw toepassing Logboeken heel vaak rapporteert, is het beter voor het bufferen van de logboeken en om te rapporteren ze allemaal tegelijk op een vaste tijd base (dit is de modus' burst' genoemd).

Roep de methode om dit te doen:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde in **milliseconden**. Op elk gewenst moment als u activeren, de real-time logboekregistratie wilt, roept de methode geen parameters of met de waarde 0.

De modus ' burst ' iets langere levensduur maar heeft een invloed op de Monitor Engagement: alle sessies en taken duur wordt afgerond op de drempelwaarde burst (dus sessies en korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken). Het is raadzaam een ' burst ' drempelwaarde niet langer dan 30000 (30s) gebruiken. U moet er rekening mee dat opgeslagen logboeken zijn beperkt tot 300 items. U kunt sommige logboeken verliezen als verzenden te lang is.

> [!WARNING]
> De drempelwaarde burst kan niet worden geconfigureerd op een lager dan 1s periode. Als u probeert te doen, wordt de SDK een tracering met de fout wordt weergegeven en wordt automatisch opnieuw ingesteld op de standaardwaarde, dat wil zeggen, 0s. Dit activeert de SDK om de logboeken in realtime te rapporteren.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

