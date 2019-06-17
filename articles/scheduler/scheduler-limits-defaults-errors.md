---
title: Limieten, quota en drempelwaarden in Azure Scheduler
description: Meer informatie over limieten, quota, standaardwaarden en beperken van drempelwaarden voor Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 478afb20f3dabec74d66d00bec325408ce6604fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64713736"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Limieten, quota en drempelwaarden beperken in Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, dat buiten gebruik wordt gesteld. [Probeer in plaats daarvan Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) als u taken wilt plannen. 

## <a name="limits-quotas-and-thresholds"></a>Limieten, quota en drempelwaarden

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>de header x-ms-request-id

Elke aanvraag ten opzichte van de Scheduler-service retourneert een antwoordheader met de naam **x-ms-request-id**. Deze header bevat een onduidelijke waarde die de aanvraag wordt aangeduid. Dus als een aanvraag consistent mislukt, en u vastgesteld dat de aanvraag is juist opgemaakt, kunt u rapporteren de fout naar Microsoft door te geven de **x-ms-request-id** antwoord-header-waarde en met inbegrip van deze gegevens: 

* De **x-ms-request-id** waarde
* Bij benadering de tijd wanneer de aanvraag is gedaan 
* De id's voor de Azure-abonnement, de taakverzameling en de taak 
* Het type bewerking waarmee is geprobeerd de aanvraag

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Azure Scheduler-concepten, -terminologie en -entiteitenhiërarchie](scheduler-concepts-terms.md)
