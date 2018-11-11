---
title: Meer informatie over de beperkingen in BizTalk Services | Microsoft Docs
description: Meer informatie over drempelwaarden voor regeling en de resulterende gedrag voor runtime voor BizTalk Services. Beperking is gebaseerd op het gebruik van geheugen en het aantal berichten. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: daab61a0ea9321b0fb918c60688215c80088e0bc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243348"
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: beperking

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services implementeert beperking van de opslagservice op basis van twee voorwaarden: geheugengebruik en het aantal gelijktijdige berichten verwerken. Dit onderwerp geeft een lijst van de drempelwaarden voor regeling en beschrijft de Runtime-gedrag wanneer een beperking voordoet.

## <a name="throttling-thresholds"></a>Drempelwaarden voor regeling
De volgende tabel bevat de bandbreedteregeling bron- en drempelwaarden:

|  | Beschrijving | Lage drempelwaarde | Hoge drempelwaarde |
| --- | --- | --- | --- |
| Geheugen |% van de totale systeem geheugen beschikbaar/PageFileBytes. <p><p>Totale beschikbare PageFileBytes is ongeveer 2 keer de hoeveelheid RAM van het systeem. |60% |70% |
| Berichtverwerking |Aantal berichten tegelijkertijd verwerken |40 * aantal kernen |100 * aantal kernen |

Wanneer een hoge drempelwaarde is bereikt, wordt Azure BizTalk Services wordt gestart om te beperken. Beperking stopt wanneer de lage drempelwaarde is bereikt. Bijvoorbeeld, wordt uw service met behulp van systeemgeheugen 65%. In dit geval wordt de service niet beperken. Uw service wordt gestart met behulp van het systeemgeheugen 70%. In dit geval is de service beperkt, en om te beperken tot 60% (lage drempelwaarde) systeemgeheugen maakt gebruik van de service blijft.

BizTalk Services van Azure houdt de status van de beperking (normale status versus beperkte staat) en de duur van de aanvragen worden beperkt.

## <a name="runtime-behavior"></a>Runtime-gedrag
Wanneer u Azure BizTalk Services in een beperking staat, gebeurt het volgende:

* Beperking is per rolinstantie. Bijvoorbeeld:<br/>
  RoleInstanceA is beperking. RoleInstanceB is geen beperking. In dit geval worden berichten in RoleInstanceB verwerkt zoals verwacht. Berichten in RoleInstanceA worden genegeerd en mislukt met de volgende fout:<br/><br/>
  **Server is bezet. Probeer het opnieuw.**<br/><br/>
* Elke pull-gegevensbron niet controleren of downloaden van een bericht. Bijvoorbeeld:<br/>
  Een pijplijn haalt berichten uit een externe FTP-bron. De rolinstantie doen het pull krijgt de status van een beperking. In dit geval stopt de pijplijn extra berichten downloaden totdat de rolinstantie beperking stopt.
* Een antwoord is verzonden naar de client, zodat de client opnieuw in het bericht indienen kan.
* U moet wachten tot de beperking opgelost is. Specifiek, moet u wachten totdat de lage drempelwaarde is bereikt.

## <a name="important-notes"></a>Belangrijke opmerkingen
* Beperking kan niet worden uitgeschakeld.
* Drempelwaarden voor regeling kan niet worden gewijzigd.
* Beperking is systeembrede geïmplementeerd.
* De Azure SQL Database-Server heeft ook ingebouwde beperking.

## <a name="additional-azure-biztalk-services-topics"></a>Aanvullende onderwerpen voor Azure BizTalk Services
* [De Azure BizTalk Services SDK installeren](https://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Zelfstudie: Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Zie ook
* [BizTalk Services: Developer, Basic, Standard en Premium-edities grafiek](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk Services: statusgrafiek voor de inrichting](https://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: back-ups maken en herstellen](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: naam en sleutel van verlener](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

