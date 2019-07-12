---
title: Vermijd serviceonderbrekingen in Azure Stream Analytics-taken
description: In dit artikel bevat instructies over het maken van uw Stream Analytics-taken bijwerken robuuste.
services: stream-analytics
author: jseb225
ms.author: sidram
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: e38f8a923daa210d8aa5b56631e5f8157d4b3f70
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620869"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Betrouwbaarheid van de Stream Analytics-taken te garanderen tijdens de service-updates

Onderdeel van een volledig beheerde service die wordt is de mogelijkheid om nieuwe servicefunctionaliteit en verbeteringen in een hoog tempo te introduceren. Stream Analytics kan als gevolg hiervan, een service-update implementeren op basis van wekelijkse (of vaker) hebben. Ongeacht hoeveel tests worden uitgevoerd is er nog steeds een risico dat een bestaande, actieve taak is het vanwege de introductie van een bug verbroken mogelijk. Als u essentiële taken kritiek moeten taken, deze risico's worden vermeden. U verkleint dit risico door volgende Azure **[gekoppelde regio](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Hoe dit probleem oplossen met Azure gekoppelde regio's?

Stream Analytics-taken in gekoppelde regio's worden bijgewerkt in afzonderlijke batches wordt gegarandeerd. Er is als gevolg hiervan een voldoende tijdsverschil tussen de updates voor potentiële problemen identificeren en ze herstellen.

_Met uitzondering van Centraal-India_ (waarvan de gekoppelde regio Zuid-India, heeft geen Stream Analytics aanwezigheid), de implementatie van een update voor Stream Analytics wordt niet uitgevoerd op hetzelfde moment in een set van gekoppelde regio's. Implementaties in meerdere regio's **in dezelfde groep** optreden **op hetzelfde moment**.

In het artikel over **[beschikbaarheid en gekoppelde regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** is de meest actuele informatie over welke regio's zijn gekoppeld.

Het verdient aanbeveling om te implementeren identieke taken voor beide gekoppelde regio's. Vervolgens moet u [bewaken van deze taken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) om te worden geïnformeerd wanneer er iets onverwachts gebeurt. Als een van deze ends van taken een [status mislukt](https://docs.microsoft.com/azure/stream-analytics/job-states) nadat de update van een Stream Analytics-service, u kunt contact opnemen met klantondersteuning om u te helpen bij het identificeren van de hoofdoorzaak te achterhalen. U moet ook een failover downstream consumenten aan de orde taakuitvoer.

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Stream Analytics query language-referentie](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Stream Analytics management REST API-naslaginformatie](https://msdn.microsoft.com/library/azure/dn835031.aspx)
