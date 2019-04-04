---
title: Problemen oplossen met bewerkingslogboeken BizTalk-Services | Microsoft Docs
description: Problemen oplossen met bewerkingslogboeken BizTalk-Services. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 35c30ec97d4c033a687204b926d2f686c52be069
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916017"
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Problemen oplossen met bewerkingslogboeken

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="what-are-the-operation-logs"></a>Wat zijn de logboeken voor bewerkingen
Logboeken voor bewerkingen is een Management Services-functie waarmee u om historische logboeken van de bewerkingen die worden uitgevoerd op uw Azure-services, inclusief BizTalk Services weer te geven. Hiermee kunt u historische gegevens met betrekking tot bewerkingen op uw BizTalk Service-abonnement tot 180 dagen geleden bekijken.

> [!NOTE]
> Deze functie bevat alleen logboeken voor beheerbewerkingen op BizTalk Services, zoals wanneer de service is gestart, een back-maximaal, enzovoort gemaakt. Deze bewerkingen worden bijgehouden met behulp van de [BizTalk Service REST-API's](/previous-versions/azure/reference/dn232347(v=azure.100)). Zie voor een volledige lijst met bewerkingen die zijn getraceerd door Management Services, [bewerkingen bijgehouden met behulp van Azure Management Services](#bizops).<br/><br/>
> De logboeken voor activiteiten met betrekking tot de BizTalk Service runtime (zoals bericht verwerkt door bruggen, enzovoort.), wordt dit niet vastgelegd. Als u deze logboeken, gebruikt u de weergave bijhouden in de BizTalk Services-portal. Zie voor meer informatie, [berichten traceren](/previous-versions/azure/hh949805(v=azure.100)).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>BizTalk Services-Logboeken voor bewerkingen weergeven
1. Selecteer in de portal **beheerservices**, en selecteer vervolgens de **Bewerkingslogboeken** tabblad.
2. U kunt de logboeken op basis van verschillende parameters, zoals het abonnement, datumbereik, servicetype (bijvoorbeeld BizTalk Services), servicenaam of de status van de bewerking (geslaagd, mislukt) filteren.
3. Selecteer het vinkje om de gefilterde lijst weer te geven. De volgende afbeelding ziet u activiteiten met betrekking tot testbiztalkservice: ![Logboeken van de bewerking weergeven][ViewLogs] 
4. Als u wilt meer weergeven over een bepaalde bewerking, selecteer de rij en klikt u op **Details** in de taakbalk onder.

## <a name="bizops"></a>Bewerkingen die worden bijgehouden met behulp van Azure-beheerservices
De volgende tabel bevat de bewerkingen die worden bijgehouden met behulp van de Azure Management-Services:

| Naam van bewerking | Taak |
| --- | --- |
| CreateBizTalkService |Bewerking is een nieuwe BizTalk Service maken |
| DeleteBizTalkService |Bewerking voor het verwijderen van een BizTalk Service |
| RestartBizTalkService |Bewerking is een BizTalk-Service opnieuw starten |
| StartBizTalkService |Bewerking voor het starten van een BizTalk-Service |
| StopBizTalkService |Bewerking is een BizTalk-Service stoppen |
| DisableBizTalkService |Bewerking uit te schakelen van een BizTalk Service |
| EnableBizTalkService |Bewerking voor het inschakelen van een BizTalk Service |
| BackupBizTalkService |Bewerking voor het back-up van een BizTalk Service |
| RestoreBizTalkService |Bewerking is een BizTalk Service opgegeven back-up terugzetten |
| SuspendBizTalkService |Bewerking is een BizTalk Service onderbreken |
| ResumeBizTalkService |Bewerking voor het hervatten van een BizTalk-Service |
| ScaleBizTalkService |Bewerking is een BizTalk Service omhoog of omlaag schalen |
| ConfigUpdateBizTalkService |Bewerking voor het bijwerken van de configuratie van een BizTalk Service |
| ServiceUpdateBizTalkService |Bewerking voor het upgraden en downgraden van een BizTalk Service naar een andere versie |
| PurgeBackupBizTalkService |Bewerking voor het opschonen van de back-ups van de BizTalk Service buiten de bewaarperiode liggen |

## <a name="see-also"></a>Zie ook
* [Backup BizTalk Service](https://go.microsoft.com/fwlink/p/?LinkID=325584)
* [BizTalk-Service te herstellen vanuit back-up](https://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](https://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Inrichten](https://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Diagram van de Status van inrichting](https://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Tabbladen dashboard, bewaken en schalen](https://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Beperking](https://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Naam en sleutel van verlener](https://go.microsoft.com/fwlink/p/?LinkID=303941)
* [De Azure BizTalk Services SDK gaan gebruiken](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

