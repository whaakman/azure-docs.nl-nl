---
title: Windows Universal geavanceerde Reporting met MobileApps Engagement
description: Het integreren van Azure Mobile Engagement met universele Windows-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: feac309db1ffce0945012e293bfc1df417aed876
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Geavanceerde Reporting met de Windows universele Apps Engagement SDK
> [!div class="op_single_selector"]
> * [Universeel Windows](mobile-engagement-windows-store-advanced-reporting.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Dit onderwerp beschrijft aanvullende scenario's voor rapportage in uw universele Windows-toepassing. Deze scenario's omvatten opties die u toepassen op de app gemaakt kunt in de [aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) zelfstudie.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Voordat u deze zelfstudie begint, moet u eerst voltooien de [aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) zelfstudie, namelijk opzettelijk direct en eenvoudig. Deze zelfstudie bevat informatie over aanvullende opties die u kunt kiezen uit.

## <a name="specifying-engagement-configuration-at-runtime"></a>Configuratie van de engagement tijdens runtime opgeven
De configuratie van de Engagement is gecentraliseerd in de `Resources\EngagementConfiguration.xml` -bestand van uw project, namelijk waarin deze is opgegeven in de [aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) onderwerp.

Maar u kunt dit ook opgeven tijdens runtime: u kunt de volgende methode voordat de initialisatie van de agent Engagement aanroepen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Aanbevolen methode: overbelasting uw `Page` klassen
Voor het activeren van de rapportage van de logboeken die zijn vereist voor Engagement gebruikers, sessies, activiteiten, Crashes en technische statistieken berekenen, moet u alle uw `Page` onderliggende klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van een pagina van uw toepassing. U kunt hetzelfde voor alle pagina's van uw toepassing doen.

### <a name="c-source-file"></a>C#-bronbestand
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
> Als uw pagina de methode `OnNavigatedTo` negeert, zorg er dan voor dat u `base.OnNavigatedTo(e)` aanroept. Anders wordt de activiteit is niet gerapporteerd (de `EngagementPage` aanroepen `StartActivity` binnen de `OnNavigatedTo` methode).
> 
> 

### <a name="xaml-file"></a>XAML-bestand
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

### <a name="override-the-default-behaviour"></a>Standaard overschrijven
Standaard is de naam van de klasse van de pagina als de naam van de activiteit met zonder extra gerapporteerd. Als de klasse wordt gebruikt voor het achtervoegsel 'Pagina', Engagement, wordt deze verwijderd.

U kunt het standaardgedrag voor de naam overschrijven, voeg deze code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Voeg deze code worden gerapporteerd extra informatie met uw activiteit:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van uw pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: call `StartActivity()` handmatig
Als u niet kunt of niet wilt overbelasten uw `Page` klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent` rechtstreeks methoden.

We raden aan aanroepen `StartActivity` binnen uw `OnNavigatedTo` methode van uw pagina.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Zorg ervoor dat u uw sessie correct beëindigen.
> 
> De universele Windows SDK automatisch roept de `EndActivity` methode wanneer de toepassing wordt gesloten. Het is dus **maximaal** aanbevolen om aan te roepen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigt, en zo de **nooit** aanroepen de `EndActivity` methode. Deze methode meldt de Engagement-server aan dat de huidige gebruiker de toepassing, wat is van invloed op alle toepassingslogboeken heeft verlaten.
> 
> 

## <a name="advanced-reporting"></a>Geavanceerde rapportage
Desgewenst kunt u rapporteren toepassingsspecifieke gebeurtenissen, fouten en taken, doet u gebruik van de andere methoden gevonden in de `EngagementAgent` klasse. De API Engagement staat het gebruik van geavanceerde mogelijkheden voor alle Engagement.

Zie voor meer informatie [hoe u de geavanceerde Mobile Engagement API in uw universele Windows-app-tagging](mobile-engagement-windows-store-use-engagement-api.md).

