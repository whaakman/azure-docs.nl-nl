---
title: Azure Mobile Engagement Windows universele SDK-integratie | Microsoft Docs
description: Universele integratie van Windows voor de SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: d616ad58156a19e89b3e106639a38df67cbd0abb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Windows universele SDK-integratie voor Azure Mobile Engagement
Dit document beschrijft alle integratie en configuratie van opties beschikbaar voor de Azure Mobile Engagement universele Windows SDK.

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, moet u eerst voltooien onze [15 minuten zelfstudie](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Geavanceerde functies
### <a name="reporting-features"></a>Rapportagefuncties
U kunt deze functies kunt toevoegen:

1. [Geavanceerde opties voor rapportage](mobile-engagement-windows-store-advanced-reporting.md)
2. [Geavanceerde configuratieopties](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Meldingen
[Het bereik (meldingen) integreren in uw universele Windows-app](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Tag plan implementatie:
[Het gebruik van de geavanceerde Mobile Engagement tagging API in uw universele Windows-app](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a>Releaseopmerkingen
### <a name="341-11032016"></a>3.4.1 (11/03/2016)

* Verbeteringen in stabiliteit.

Zie voor eerdere versies, de [voltooien release-opmerkingen](mobile-engagement-windows-store-release-notes.md)

## <a name="upgrade-procedures"></a>Upgradeprocedures
Als u hebt al een oudere versie van Engagement geïntegreerd in uw toepassing, hebt u de volgende punten overwegen bij het upgraden van de SDK.

Als u verschillende versies van de SDK hebt gemist, moet u wellicht verschillende procedures volgen. Zie de volledige [Procedures Upgrade](mobile-engagement-windows-store-upgrade-procedure.md). Bijvoorbeeld als u migreert van 0.10.1 naar u moet eerst Volg de procedure 'van 0.9.0 naar 0.10.1' 0.11.0 vervolgens de procedure 'van 0.10.1 naar 0.11.0'.

### <a name="from-330-to-340"></a>Van 3.3.0 naar 3.4.0
#### <a name="test-logs"></a>Test-Logboeken
De logboeken van de console die wordt geproduceerd door de SDK kunnen worden ingeschakeld/uitgeschakeld/gefilterd. Als u wilt aanpassen, werk de eigenschap `EngagementAgent.Instance.TestLogEnabled` op een van de waarden die beschikbaar is via de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a>Resources
De Reach-overlay is verbeterd. Het uitmaakt deel van de resources SDK NuGet-pakket.

Tijdens het upgraden naar de nieuwe versie van de SDK, kunt u kiezen of u behouden van de bestaande bestanden uit de overlay-map van uw resources wilt of niet:

* Als de vorige overlay u werkt of u integreert de `WebView` elementen handmatig, klikt u vervolgens kunt u bepalen dat uw afgesloten bestanden, het nog steeds werken.
* Als u wilt bijwerken naar de nieuwe overlay, vervang de gehele `overlay` map van uw resources met de nieuwe versie van de SDK-pakket (UWP-apps: na de upgrade kunt u de nieuwe map in de overlay ophalen van % USERPROFILE %\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Met behulp van de nieuwe overlay overschrijft aanpassingen op de vorige versie.
> 
> 

### <a name="upgrade-from-older-versions"></a>Upgraden van oudere versies
Zie [Procedures upgraden](mobile-engagement-windows-store-upgrade-procedure.md)

