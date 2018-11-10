---
title: Taken die zijn toegestaan in verschillende statussen of statussen in BizTalk Services | Microsoft Docs
description: 'De acties/bewerkingen toegestaan in verschillende MABS-status: stoppen, starten, opnieuw starten, onderbreken, hervatten, verwijderen, schalen, configuratie en de back-ups maken van bijwerken'
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: bbe1288a42db307001ac778394ac410206f1df21
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228195"
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>Wat u wel en niet met behulp van de BizTalk Service-status

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Afhankelijk van de huidige status van de BizTalk service zijn er bewerkingen die u wel of niet uitvoeren op de BizTalk service.

Bijvoorbeeld, inricht u een nieuwe BizTalk service. Wanneer deze voltooid is, wordt de BizTalk-service is in `active` staat. In de status actief kunt u stoppen, onderbreken en verwijderen van de BizTalk service. Als u de BizTalk-service stoppen en stoppen is mislukt, wordt de BizTalk service gaat u naar een `StopFailed` staat. In de `StopFailed` staat, kunt u de BizTalk-service opnieuw starten. Als u een bewerking die niet is toegestaan probeert, zoals hervatten, wordt het volgende foutbericht weergegeven:

`Operation not allowed`

## <a name="view-the-possible-states"></a>De mogelijke statussen weergeven

De volgende tabellen worden de bewerkingen of bewerkingen die kunnen worden uitgevoerd wanneer de BizTalk-Service in een specifieke status is. Een ✔ betekent dat de bewerking is toegestaan in deze staat. Een lege waarde betekent dat de bewerking kan niet worden uitgevoerd in deze staat.

| Servicestatus | Starten | Stoppen | Opnieuw starten | Tijdelijk intrekken | Hervatten | Verwijderen | Schalen | Update <br/> Configuratie | Backup |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Actief |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Uitgeschakeld |  |  |  |  |  | ✔ | |  |  | 
| Uitgesteld |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Gestopt | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Service-Update is mislukt |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Zie ook
* [Wat u kunt doen in de tabbladen dashboard, bewaken en schalen in BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [Wat u krijgt met de Developer, Basic, Standard en Premium-edities in BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Back-up en herstellen van een BizTalk Service](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Beperking uitgelegd in BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [De Service Bus en Access Control verlener en -verlenersleutel sleutelwaarden voor uw BizTalk Service ophalen](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](https://go.microsoft.com/fwlink/p/?LinkID=302335)

