---
title: Vermijd serviceonderbrekingen in Azure Stream Analytics-taken
description: In dit artikel bevat instructies over het maken van uw Stream Analytics-taken bijwerken robuuste.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 7375fb2763ad83e049b1ef30a623f164e059a792
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090802"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Betrouwbaarheid van de Stream Analytics-taken te garanderen tijdens de service-updates

Onderdeel van een volledig beheerde service die wordt is de mogelijkheid om nieuwe servicefunctionaliteit en verbeteringen in een hoog tempo te introduceren. Stream Analytics kan als gevolg hiervan, een service-update implementeren op basis van wekelijkse (of vaker) hebben. Ongeacht hoeveel tests worden uitgevoerd is er nog steeds een risico dat een bestaande, actieve taak is het vanwege de introductie van een bug verbroken mogelijk. Voor klanten die essentiële taken voor streaming verwerkt uitvoeren moeten deze risico's worden vermeden. Een mechanisme voor klanten gebruiken kunnen om dit risico te beperken van Azure is **[gekoppelde regio](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Hoe dit probleem oplossen met Azure gekoppelde regio's?

Stream Analytics-taken in gekoppelde regio's worden bijgewerkt in afzonderlijke batches wordt gegarandeerd. Er is als gevolg hiervan een voldoende tijdsverschil tussen de updates voor het identificeren van mogelijke belangrijke fouten en ze herstellen.

_Met uitzondering van Centraal-India_ (waarvan de gekoppelde regio Zuid-India, heeft geen Stream Analytics aanwezigheid), de implementatie van een update voor Stream Analytics wordt niet uitgevoerd op hetzelfde moment in een set van gekoppelde regio's. Implementaties in meerdere regio's **in dezelfde groep** optreden **op hetzelfde moment**.

In het artikel over **[beschikbaarheid en gekoppelde regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** is de meest actuele informatie over welke regio's zijn gekoppeld.

Klanten wordt aangeraden om te implementeren identieke taken voor beide gekoppelde regio's. Naast de Stream Analytics interne bewakingsmogelijkheden, ook klanten wordt aangeraden voor het bewaken van de taken als **beide** productietaken zijn. Als een einde wordt vastgesteld dat het gevolg zijn van de Stream Analytics-service-update, escaleren op de juiste wijze en downstream consumenten aan de orde taakuitvoer failover. Escalatie ter ondersteuning van wordt voorkomen dat de gekoppelde regio worden beïnvloed door de nieuwe implementatie- en onderhouden van de integriteit van de gekoppelde taken.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Stream Analytics query language-referentie](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)
