---
title: Vermijd serviceonderbrekingen in Azure Stream Analytics-taken
description: Dit artikel bevat instructies over het maken van uw Stream Analytics-taken werken robuuste.
services: stream-analytics
author: jseb225
manager: kfile
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 47ccfe99d2ee6576dbb70324eb383f52d2a1b2e7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Stream Analytics-taak betrouwbaarheid garanderen tijdens de service-updates

Deel van een volledig beheerde service wordt is de mogelijkheid om te introduceren van nieuwe service-functionaliteit en verbeteringen in een snelle tempo. Stream Analytics kan hierdoor een service-update implementeert op basis van wekelijkse (of vaker) hebben. Ongeacht hoeveel tests worden uitgevoerd nog er steeds het risico bestaat dat een bestaande, actieve taak is het vanwege de introductie van een fout verbroken mogelijk. Voor klanten die kritieke streaming verwerking taken uitvoeren, moeten deze risico's worden vermeden. Een mechanisme klanten gebruiken kunnen om dit risico te verminderen van Azure is **[gekoppelde regio](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** model. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Hoe gekoppelde Azure-regio adresseren van dit te voorkomen?

Stream Analytics-taken in de gekoppelde regio's worden bijgewerkt in afzonderlijke batches wordt gegarandeerd. Er is als gevolg hiervan een voldoende tijdsverschil tussen de updates te identificeren mogelijke recente fouten en ze herstellen.

_Met uitzondering van centrale India_ (waarvan gekoppelde regio, Zuid, India geen Stream Analytics aanwezigheid), de implementatie van een update voor Stream Analytics gebeurt niet op hetzelfde moment in een set van gekoppelde regio's. Implementaties in meerdere regio's **in dezelfde groep** optreden **op hetzelfde moment**.

Het artikel over **[beschikbaarheid en gekoppelde regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** heeft de meest actuele informatie op welke regio's zijn gekoppeld.

Klanten wordt aangeraden om te implementeren identiek taken voor beide gekoppelde regio's. Naast de Stream Analytics interne bewakingsmogelijkheden, ook klanten wordt aangeraden om te controleren van de taken als **beide** zijn productietaken. Als een einde wordt geïdentificeerd als een resultaat van de Stream Analytics-service-update, escaleren op de juiste wijze en downstream consumenten aan de orde taakuitvoer failover. Escalatie ter ondersteuning van wordt voorkomen dat de gekoppelde regio wordt beïnvloed door de nieuwe implementatie en de integriteit van de gekoppelde taken beheren.
