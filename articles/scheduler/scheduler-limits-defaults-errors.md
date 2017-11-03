---
title: Scheduler-limieten en de standaardinstellingen
description: Scheduler-limieten en de standaardinstellingen
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: db6b1c196cb468f41c7a7ce34758de346b522abb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="scheduler-limits-and-defaults"></a>Scheduler-limieten en de standaardinstellingen
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Scheduler-quota, limieten, standaardwaarden en vertragingen
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>De Header x-ms-aanvraag-id
Elke aanvraag ten opzichte van de Scheduler-service retourneert een antwoordheader genaamd**x-ms-aanvraag-id**. Deze koptekst bevat een ondoorzichtige waarde die een unieke identificatie van de aanvraag.

Als een aanvraag consequent mislukt en u hebt gecontroleerd dat de aanvraag goed geformuleerd is, kunt u deze waarde mag gebruiken voor het rapporteren van de fout naar Microsoft. In het rapport bevatten de waarde van de x-ms-aanvraag-id, bij benadering de tijd die de aanvraag is ingediend, de id van het abonnement, taakverzameling, en/of de taak en het type van de bewerking waarmee is geprobeerd de aanvraag.

## <a name="see-also"></a>Zie ook
 [Wat is Scheduler?](scheduler-intro.md)

 [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Aan de slag met behulp van Scheduler in Azure Portal](scheduler-get-started-portal.md)

 [Plannen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Naslaginformatie over Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Hoge beschikbaarheid en betrouwbaarheid van Azure Scheduler](scheduler-high-availability-reliability.md)

 [Azure Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)

