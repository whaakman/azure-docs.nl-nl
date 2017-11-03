---
title: Azure Mobile Engagement Windows Phone Silverlight-SDK overzicht | Microsoft Docs
description: Overzicht van de Windows Phone Silverlight-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: c4e8ceee4104c3d3a6c3e6b79322ba1cf8463b22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight-SDK overzicht voor Azure Mobile Engagement
Begin hier de details ophalen over het integreren van Azure Mobile Engagement in een Windows Phone Silverlight-App. Als u u het eerst proberen wilt, controleert u of u onze [15 minuten zelfstudie](mobile-engagement-windows-phone-get-started.md).

Klik om te zien de [SDK-inhoud](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Integratie procedures
1. Begin hier: [Mobile Engagement integreren in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-integrate-engagement.md)
2. Voor meldingen: [Reach (meldingen) integreren in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Tag plan implementatie: [hoe u de geavanceerde Mobile Engagement tagging API in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Releaseopmerkingen
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
Onderdeel van de *MicrosoftAzure.MobileEngagement* Nuget-pakket **v3.4.1**

* Verbeteringen in stabiliteit.

Zie voor een eerdere versie de [voltooien release-opmerkingen](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Upgradeprocedures
Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, hebt u de volgende punten overwegen bij het upgraden van de SDK.

U moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Zie de volledige [Procedures Upgrade](mobile-engagement-windows-phone-upgrade-procedure.md). Bijvoorbeeld als u migreert van 0.10.1 naar u moet eerst Volg de procedure 'van 0.9.0 naar 0.10.1' 0.11.0 vervolgens de procedure 'van 0.10.1 naar 0.11.0'.

### <a name="from-200-to-330"></a>Van 2.0.0 naar 3.3.0
#### <a name="test-logs"></a>Test-Logboeken
De logboeken van de console die wordt geproduceerd door de SDK kunnen worden ingeschakeld/uitgeschakeld/gefilterd. Werk de eigenschap voor het aanpassen van dit `EngagementAgent.Instance.TestLogEnabled` op een van de waarde in de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Upgraden van oudere versies
Zie [Procedures upgraden](mobile-engagement-windows-phone-upgrade-procedure.md)

