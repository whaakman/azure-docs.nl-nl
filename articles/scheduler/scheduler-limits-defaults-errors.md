---
title: Scheduler-limieten en standaardinstellingen
description: Scheduler-limieten en standaardinstellingen
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4be0695402b66fdb2a027bfbada0e0b26e02d36f
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378717"
---
# <a name="scheduler-limits-and-defaults"></a>Scheduler-limieten en standaardinstellingen
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Quota's voor Scheduler, limieten, standaardwaarden en vertragingen in
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>De Header x-ms-request-id
Elke aanvraag ten opzichte van de Scheduler-service retourneert een antwoordheader met de naam **x-ms-request-id**. Deze header bevat een onduidelijke waarde die de aanvraag wordt aangeduid.

Als een aanvraag voortdurend mislukken en u hebt gecontroleerd dat de aanvraag goed geformuleerd is, kunt u deze waarde mag gebruiken voor het rapporteren van de fout naar Microsoft. Opnemen in uw rapport, de waarde van de x-ms-request-id, bij benadering de tijd die het verzoek is gemaakt, de id van het abonnement, taakverzameling, en/of taak en het type bewerking waarmee is geprobeerd de aanvraag.

## <a name="see-also"></a>Zie ook
 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Hoge beschikbaarheid en betrouwbaarheid van Azure Scheduler](scheduler-high-availability-reliability.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

