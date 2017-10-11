---
title: Meer informatie over het beperken van in BizTalk Services | Microsoft Docs
description: Meer informatie over bandbreedtebeperking drempelwaarden en de resulterende gedrag van de runtime voor BizTalk Services. Bandbreedtebeperking is gebaseerd op het gebruik van geheugen en het aantal berichten. MABS, WABS
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 145e7470bbc01c676a1fb5856c0f9a8726e667fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: beperking

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services implementeert service bandbreedtebeperking op basis van twee voorwaarden: geheugengebruik en het aantal gelijktijdige berichten verwerken. Dit onderwerp geeft een lijst van de bandbreedteregeling drempelwaarden en beschrijft het runtimegedrag wanneer er een voorwaarde bandbreedteregeling optreedt.

## <a name="throttling-thresholds"></a>Beperking van de drempelwaarden
De volgende tabel bevat de drempelwaarden en bandbreedteregeling bron:

|  | Beschrijving | Lage drempelwaarde | Hoge drempelwaarde |
| --- | --- | --- | --- |
| Geheugen |% van totale systeem geheugen beschikbaar/PageFileBytes. <p><p>Totaal aantal beschikbare PageFileBytes is ongeveer 2 keer de hoeveelheid RAM van het systeem. |60% |70% |
| Berichtverwerking |Aantal berichten tegelijkertijd verwerken |40 * aantal kernen |100 * aantal kernen |

Wanneer een hoge drempelwaarde is bereikt, begint de Azure BizTalk Services om te beperken. Beperking stopt wanneer de lage drempelwaarde is bereikt. Bijvoorbeeld, is uw service met behulp van 65% systeemgeheugen. In dit geval heeft de service niet beperken. Uw service wordt gestart met behulp van het systeemgeheugen 70%. In dit geval kan de service beperkt en blijft totdat de service geheugen (lage drempelwaarde) 60 gebruikt % beperken.

Azure BizTalk Services houdt de status van de bandbreedteregeling (normale status versus beperkte status) en de duur van de bandbreedteregeling.

## <a name="runtime-behavior"></a>Runtime-gedrag
Wanneer u Azure BizTalk Services in een bandbreedteregeling staat, gebeurt het volgende:

* Beperking is per rolexemplaar. Bijvoorbeeld:<br/>
  RoleInstanceA is beperking. RoleInstanceB is geen beperking. In deze situatie verwerkt berichten in RoleInstanceB zoals verwacht. Berichten in RoleInstanceA worden verwijderd en mislukt met de volgende fout:<br/><br/>
  **Server is bezet. Probeer het opnieuw.**<br/><br/>
* Een pull-bronnen geen pollen of downloaden van een bericht. Bijvoorbeeld:<br/>
  Een pijplijn haalt berichten uit een externe FTP-bron. De rolinstantie tijdens het doorzoeken van de pull opgehaald in een status bandbreedteregeling. In dit geval is stopt de pijplijn aanvullende berichten downloaden totdat de rolinstantie niet meer beperking.
* Een antwoord wordt verzonden naar de client, zodat de client opnieuw van het bericht indienen kunt.
* U moet wachten totdat de beperking opgelost is. In het bijzonder moet u wachten tot de lage drempelwaarde is bereikt.

## <a name="important-notes"></a>Belangrijke opmerkingen
* Beperking kan niet worden uitgeschakeld.
* Drempelwaarden beperking kan niet worden gewijzigd.
* Beperking, is de hele systeem geïmplementeerd.
* De Azure SQL Database-Server heeft ook ingebouwde beperking.

## <a name="additional-azure-biztalk-services-topics"></a>Aanvullende onderwerpen voor Azure BizTalk Services
* [De Azure BizTalk Services SDK installeren](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Zelfstudies: Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zie ook
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: Inrichten met behulp van Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: statusgrafiek voor de inrichting](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: back-ups maken en herstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: naam en sleutel van verlener](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

