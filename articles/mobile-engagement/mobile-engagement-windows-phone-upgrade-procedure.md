---
title: Windows Phone Silverlight-SDK upgradeprocedures
description: Windows Phone Silverlight-SDK upgradeprocedures voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 633bf79a3bcaa9c97a5c70e3b362fd928178dcce
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight-SDK upgradeprocedures
Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, hebt u de volgende punten overwegen bij het upgraden van de SDK.

U moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Bijvoorbeeld als u migreert van 0.10.1 naar u moet eerst Volg de procedure 'van 0.9.0 naar 0.10.1' 0.11.0 vervolgens de procedure 'van 0.10.1 naar 0.11.0'.

## <a name="from-200-to-330"></a>Van 2.0.0 naar 3.3.0
### <a name="test-logs"></a>Test-Logboeken
De logboeken van de console die wordt geproduceerd door de SDK kunnen worden ingeschakeld/uitgeschakeld/gefilterd. Werk de eigenschap voor het aanpassen van dit `EngagementAgent.Instance.TestLogEnabled` op een van de waarde in de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

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

Daarna het nieuwe Microsoft Azure Engagement nuget-pakket te installeren op uw project. U vindt deze op [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Deze bewerking vervangt alle resources bestanden die worden gebruikt door Engagement en voegt u het nieuwe Engagement dll-bestand toe aan de projectverwijzingen.

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
4. Voor de andere resources zoals Capptain afbeeldingen, houd er rekening mee dat ze ook zijn gewijzigd voor het gebruik van 'Engagement'.

### <a name="application-id--sdk-key"></a>Toepassings-ID / SDK-sleutel
Engagement maakt gebruik van een verbindingsreeks. U hoeft niet te geven van een toepassings-ID en een SDK-sleutel met Mobile Engagement, hoeft u een verbindingsreeks opgeven. U kunt deze functie instellen op uw EngagementConfiguration-bestand.

De configuratie van de Engagement kan worden ingesteld in uw `Resources\EngagementConfiguration.xml` -bestand van uw project.

Dit bestand op te geven bewerken:

* De verbindingsreeks voor de toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u in plaats daarvan tijdens runtime opgeven wilt, kunt u de volgende methode voordat de initialisatie van de Engagement-agent kunt aanroepen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de Azure-Portal.

### <a name="items-name-change"></a>Wijziging van items
Alle items met de naam *capptain* naam hebt gegeven *engagement*. Op dezelfde manier voor *Capptain* naar *Engagement*.

Voorbeelden van veelgebruikte Capptain items:

* CapptainConfiguration EngagementConfiguration nu met de naam
* CapptainAgent EngagementAgent nu met de naam
* CapptainReach EngagementReach nu met de naam
* CapptainHttpConfig EngagementHttpConfig nu met de naam
* GetCapptainPageName GetEngagementPageName nu met de naam

Houd er rekening mee dat rename ook van invloed op overschreven methoden.

