---
title: Taken die zijn toegestaan in verschillende statussen of statussen in BizTalk Services | Microsoft Docs
description: 'De acties/operations toegestaan in andere MABS status: stoppen, starten, opnieuw starten, onderbreken, hervatten, verwijderen, schalen, configuratie en back-ups maken van bijwerken'
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: 918154852cf4ee1b80792eaddb588f5b0237da7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>Wat u wel en niet met behulp van de BizTalk Service-status

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Afhankelijk van de huidige status van de BizTalk service zijn er bewerkingen die u wel of niet uitvoeren op de BizTalk service.

Bijvoorbeeld, het inrichten van een nieuwe BizTalk-service in de klassieke Azure portal. Wanneer het is voltooid, wordt de BizTalk service `active` status. In de actieve status heeft, kunt u stoppen, onderbreken en de BizTalk service verwijderen. Als u de BizTalk-service stoppen en stop mislukt en de BizTalk service gaat dan naar een `StopFailed` status. In de `StopFailed` staat, kunt u de BizTalk service opnieuw starten. De volgende fout treedt op als u een bewerking die niet zijn toegestaan, zoals hervatten:

`Operation not allowed`

## <a name="view-the-possible-states"></a>De mogelijke statussen weergeven

De volgende tabellen worden de bewerkingen of acties die kunnen worden uitgevoerd wanneer de BizTalk-Service zich in een specifieke status. Een ✔ betekent dat de bewerking is toegestaan in deze staat. Een lege waarde betekent dat de bewerking kan niet worden uitgevoerd terwijl u met die status.

| Servicestatus | Starten | Stoppen | Opnieuw starten | onderbreken | Hervatten | Verwijderen | Schalen | Update <br/> Configuratie | Back-up |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Actief |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Uitgeschakeld |  |  |  |  |  | ✔ | |  |  | 
| Onderbroken |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Stopped | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Service-Update is mislukt |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Zie ook
* [Een BizTalk Service met de klassieke Azure portal maken](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [Wat u kunt doen op de tabbladen dashboard, bewaken en schalen van BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [U krijgt met de Developer, Basic, Standard en Premium-edities in BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Het back-up en herstellen van een BizTalk Service](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Beperking uitgelegd in BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [De Service Bus- en toegangsbeheer uitgever en de verlener sleutelwaarden voor uw BizTalk Service ophalen](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302335)

