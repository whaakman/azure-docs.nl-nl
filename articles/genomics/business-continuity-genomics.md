---
title: Bedrijfscontinuïteit - Microsoft-Genomics | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over hoe Microsoft Genomics ondersteuning biedt voor bedrijfscontinuïteit
keywords: bedrijfscontinuïteit, herstel na noodgevallen
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: cb3825cb89aff386c4f7c3f3b0d771d73fe637b1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Overzicht van zakelijke continuïteit met Microsoft Genomics
Dit overzicht beschrijft de mogelijkheden die Microsoft Genomics voor bedrijfscontinuïteit en noodherstel biedt. Meer informatie over opties voor het herstellen van verstoren gebeurtenissen, zoals een storing Azure-regio die leiden gegevensverlies tot kan. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Microsoft Genomics functies die ondersteuning voor bedrijfscontinuïteit 
Hoewel dit zelden voorkomt, kan een storing, wat kan leiden tot een onderbreking van de bedrijven die het kan enkele minuten tot enkele uren laatst hebben in een Azure-Datacenter. Wanneer er een storing optreedt, mislukken alle taken die momenteel worden uitgevoerd in het datacenter en taken met een secundaire regio wordt niet automatisch opnieuw door de service Microsoft Genomics. 

* Een mogelijkheid is wachttijd voor het datacenter weer online te krijgen als het datacentrum storing is via. Als de storing kort is, wordt de service Microsoft Genomics detecteert automatisch de mislukte taken en de werkstroom wordt automatisch opnieuw opgestart.

* Een andere optie is de werkstroom in een ander gegevensgebied proactief te verzenden. Microsoft Genomics implementeert instanties in verschillende [Azure-regio's](https://azure.microsoft.com/regions/services/), en elk exemplaar regiospecifieke onafhankelijk is. Als een van de exemplaren van Microsoft Genomics een regio lokale fout optreden, blijven andere regio's actieve exemplaren van Microsoft Genomics taken verwerkt. Deze overdracht naar een alternatieve regio is onder het beheer van de gebruiker en beschikbaar zijn op elk gewenst moment.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>Handmatig failover Microsoft Genomics werkstromen naar een andere regio
In het geval van een regionale datacentrum ervoor u kiezen om Microsoft Genomics werkstromen in een secundaire regio op basis van uw vereisten voor afzonderlijke gegevens onafhankelijkheid en zakelijke continuïteit te verzenden. Handmatig failover Microsoft Genomics werkstromen gebruikt u een andere regiospecifieke. Genomica account en het verzenden van de taak met de juiste regiospecifieke genomica en storage-accountreferenties.

In het bijzonder moet u naar:
* Maak een genomica-account in de secundaire regio, met de Azure portal. 
* De ingevoerde gegevens migreren naar een opslagaccount in de secundaire regio en instellen van een map voor uitvoer die in de secundaire regio.
* De werkstroom in de secundaire regio verzenden.

Wanneer de oorspronkelijke regio wordt hersteld, de service Microsoft Genomics niet de gegevens migreren vanaf de secundaire regio terug naar de oorspronkelijke regio. U kunt ervoor kiezen om te verplaatsen van de invoer en uitvoerbestanden van de secundaire regio terug naar de oorspronkelijke regio.  Als u ervoor kiest om hun gegevens te verplaatsen, dit buiten de service genomica is en alle kosten met betrekking tot zou de gegevensverplaatsing uw verantwoordelijkheid zijn. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Een mogelijke regiospecifieke onderbreking voorbereiden
Als u zich zorgen over sneller herstel in het geval van een datacentrum, zijn er een paar stappen die u ondernemen kunt om de tijd die nodig zijn voor u om uw Microsoft-Genomics werkstromen met een secundaire regio handmatig opnieuw in te verminderen:

* Een juiste secundaire regio identificeren en professionele actief een genomica-account maken in deze regio
* Dubbele uw gegevens in de primaire en secundaire regio zodat uw gegevens in de secundaire regio onmiddellijk beschikbaar is. Dit kan zijn gereed handmatig of met de [geografisch redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy) functie die beschikbaar zijn in Azure-opslag. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over uw opties voor bedrijfscontinuïteit en herstel na noodgevallen bij gebruik van de service Microsoft Genomics. Zie voor meer informatie over zakelijke continuïteit en noodherstel binnen Azure in het algemeen [Azure tolerantie technische richtlijnen.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 