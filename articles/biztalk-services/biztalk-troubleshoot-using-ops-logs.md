---
title: BizTalk Services met behulp van bewerkingslogboeken oplossen | Microsoft Docs
description: Problemen oplossen met bewerkingslogboeken BizTalk-Services. MABS, WABS
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c0c83361f94ffd9c30d7fcc551ff4b85ad7d6fa5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Problemen oplossen met bewerkingslogboeken

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-the-operation-logs"></a>Wat zijn de logboeken van de bewerking
Bewerkingslogboeken is een beschikbaar in de klassieke Azure portal waarmee u historische logboeken van de bewerkingen die worden uitgevoerd op uw Azure-services, met inbegrip van BizTalk Services Management Services-functie. Hiermee kunt u historische gegevens met betrekking tot beheerbewerkingen op uw BizTalk Service-abonnement tot 180 dagen weergeven.

> [!NOTE]
> Deze functie bevat alleen de logboeken voor beheerbewerkingen op BizTalk Services, zoals wanneer de service is gestart, een back-up, enzovoort gemaakt. Deze bewerkingen worden bijgehouden, ongeacht of ze worden uitgevoerd vanuit de klassieke Azure portal of met behulp van de [BizTalk Service REST-API's](http://msdn.microsoft.com/library/azure/dn232347.aspx). Zie voor een volledige lijst van bewerkingen die worden bijgehouden met beheerservices [bewerkingen bijgehouden met behulp van Azure Management Services](#bizops).<br/><br/>
> De logboeken voor activiteiten met betrekking tot de BizTalk Service-runtime (zoals bericht verwerkt door bruggen, enzovoort.), wordt dit niet vastgelegd. Gebruik deze om Logboeken te raadplegen, de weergave van het bijhouden van de BizTalk Services-portal. Zie voor meer informatie [berichten bijhouden](http://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>BizTalk Services Bewerkingslogboeken weergeven
1. Selecteer in de klassieke Azure portal **beheerservices**, en selecteer vervolgens de **Bewerkingslogboeken** tabblad.
2. U kunt de logboeken op basis van verschillende parameters zoals abonnement, datumbereik, servicetype (bijvoorbeeld BizTalk Services), servicenaam of status van de bewerking (geslaagd, mislukt) filteren.
3. Selecteer het vinkje om de gefilterde lijst weer te geven. De volgende afbeelding toont activiteiten met betrekking tot testbiztalkservice: ![bewerkingslogboeken weergeven][ViewLogs] 
4. Als u wilt bekijken over een specifieke bewerking, selecteer de rij en klikt u op **Details** in de taakbalk onder.

## <a name="bizops"></a>Bewerkingen bijgehouden met Azure Management-Services
De volgende tabel bevat de bewerkingen die worden bijgehouden met de Azure Management-Services:

| De naam van bewerking | Taak |
| --- | --- |
| CreateBizTalkService |Bewerking is een nieuwe BizTalk Service maken |
| DeleteBizTalkService |Bewerking is een BizTalk Service verwijderen |
| RestartBizTalkService |Om een BizTalk Service opnieuw te starten |
| StartBizTalkService |Bewerking is een BizTalk Service starten |
| StopBizTalkService |Bewerking is een BizTalk Service stoppen |
| DisableBizTalkService |Bewerking uit te schakelen van een BizTalk Service |
| EnableBizTalkService |Bewerking is een BizTalk Service inschakelen |
| BackupBizTalkService |Bewerking back-up een BizTalk Service maken |
| RestoreBizTalkService |De bewerking een BizTalk Service van de opgegeven back-up herstellen |
| SuspendBizTalkService |Bewerking voor het onderbreken van een BizTalk Service |
| ResumeBizTalkService |Bewerking is een BizTalk Service hervatten |
| ScaleBizTalkService |Bewerking is een BizTalk Service omhoog of omlaag schalen |
| ConfigUpdateBizTalkService |Bewerking voor het bijwerken van de configuratie van een BizTalk Service |
| ServiceUpdateBizTalkService |Bewerking bij te werken of een BizTalk Service naar een andere versie downgraden |
| PurgeBackupBizTalkService |Bewerking voor het opschonen van de back-ups van de BizTalk Service buiten de bewaarperiode |

## <a name="see-also"></a>Zie ook
* [Back-up van BizTalk-Service](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk Service back-up terugzetten](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Inrichten met behulp van Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: statusgrafiek voor de inrichting](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: beperking](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: naam en sleutel van verlener](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

