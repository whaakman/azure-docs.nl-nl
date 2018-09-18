---
title: Zakelijke continuïteit - Microsoft Genomics | Microsoft Docs
titleSuffix: Azure
description: Dit overzicht beschrijft de mogelijkheden met Microsoft Genomics voor zakelijke continuïteit en herstel na noodgevallen. Meer informatie over opties voor het herstellen van storingen, zoals een storing Azure-regio, dat leiden gegevensverlies tot kan.
keywords: bedrijfscontinuïteit, herstel na noodgevallen
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: be678648ab93fcbdfd0a0baa1b01dcb273060ce2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729449"
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Overzicht van bedrijfscontinuïteit met Microsoft Genomics
Dit overzicht beschrijft de mogelijkheden met Microsoft Genomics voor zakelijke continuïteit en herstel na noodgevallen. Meer informatie over opties voor het herstellen van storingen, zoals een storing Azure-regio, dat leiden gegevensverlies tot kan. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Microsoft Genomics-functies die ondersteuning voor zakelijke continuïteit 
Hoewel zeldzaam, kan een Azure-Datacenter een storing, wat kan leiden tot een bedrijfsonderbreking die enkele minuten tot enkele uren kan duren hebben. Wanneer een storing optreedt, alle taken die momenteel wordt uitgevoerd in het datacenter mislukken en de Microsoft Genomics-service niet automatisch wordt opnieuw indienen taken naar een secundaire regio. 

* Een optie is na afloop van het datacenter weer online komt wanneer de onderbreking datacentrum ligt boven de. Als de storing kort is, de Microsoft Genomics-service detecteert automatisch de mislukte taken en de werkstroom wordt automatisch opnieuw gestart.

* Een andere optie is de werkstroom in een andere gegevensregio proactief te verzenden. Microsoft Genomics-exemplaren in verschillende implementeert [Azure-regio's](https://azure.microsoft.com/regions/services/), en elk exemplaar regiospecifieke is onafhankelijk. Als een van de Microsoft Genomics-exemplaren een lokale regio-fout optreedt, blijft andere regio's bij het uitvoeren van exemplaren van Microsoft Genomics om taken te verwerken. Deze overdracht naar een andere regio is onder het beheer van de gebruiker en beschikbaar zijn op elk gewenst moment.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>Handmatig Microsoft Genomics workflows voor failover naar een andere regio
In het geval van een regionale storing in het datacenter, kan u ervoor kiezen om in te dienen werkstromen van de Microsoft Genomics in een secundaire regio, op basis van uw vereisten voor afzonderlijke gegevens soevereiniteit en zakelijke continuïteit. Handmatig failover Microsoft Genomics werkstromen gebruikt u een andere regiospecifieke. Genomics-account en het verzenden van de taak met de juiste regiospecifieke Genomics en referenties van het opslagaccount.

Specifiek, gaat u te werk:
* Maak een Genomics-account in de secundaire regio, met behulp van de Azure portal. 
* De ingevoerde gegevens migreren naar een opslagaccount in de secundaire regio, en stelt u een map voor uitvoer in de secundaire regio.
* De werkstroom in de secundaire regio verzenden.

Wanneer de oorspronkelijke regio wordt hersteld, de Microsoft Genomics-service niet gemigreerd door de gegevens van de secundaire regio naar de oorspronkelijke regio. U kunt ervoor kiezen om te verplaatsen van de invoer en uitvoerbestanden van de secundaire regio terug naar oorspronkelijke regio.  Als u er ook voor kiezen om hun gegevens te verplaatsen, dit buiten de Genomics-service is en alle kosten met betrekking tot is de verplaatsing van gegevens is uw verantwoordelijkheid. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Voorbereiden voor een mogelijke regiospecifiek-storing
Als u zich zorgen maken over snellere herstelmogelijkheid in het geval van een storing in het datacenter, zijn er enkele stappen die u ondernemen kunt om de tijd die nodig is voor u om uw werkstromen met Microsoft Genomics naar een secundaire regio handmatig opnieuw in te verminderen:

* Een juiste secundaire regio te identificeren en pro-actief een Genomics-account maken in deze regio
* Dubbele uw gegevens in de primaire en secundaire regio, zodat uw gegevens onmiddellijk beschikbaar in de secundaire regio zijn. Dit kan worden gedaan handmatig of via de [geografisch redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy) beschikbare functie in Azure storage. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over uw opties voor bedrijfscontinuïteit en herstel na noodgevallen bij het gebruik van de service Microsoft Genomics. Zie voor meer informatie over bedrijfscontinuïteit en herstel na noodgevallen in Azure in het algemeen [technische richtlijnen voor flexibiliteit van Azure.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 