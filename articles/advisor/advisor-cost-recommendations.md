---
title: Azure Advisor kosten aanbevelingen | Microsoft Docs
description: Gebruik Azure Advisor om te optimaliseren van de kosten van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 51320d93689da3e37c0946d8877b27a11793d9c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-cost-recommendations"></a>Advisor kosten aanbevelingen

Advisor helpt u te optimaliseren en reduceren uw algehele Azure hoeven te besteden aan met het vaststellen van niet-actieve- en onderbenutte resources. U kunt ophalen kosten aanbevelingen van de **kosten** op het dashboard Advisor.

![Tabblad van Advisor kosten](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optimaliseren door het formaat onderbenutte exemplaren te besteden aan de virtuele machine 
Hoewel bepaalde toepassingsscenario's leiden minder gebruik standaard tot kunnen, kunt u vaak geld besparen door het beheer van de grootte en het nummer van uw virtuele machines. Advisor bewaakt het gebruik van uw virtuele machine 14 dagen en identificeert dan laag gebruik virtuele machines. Virtuele machines waarvan CPU-gebruik 5 is % of minder en netwerkgebruik is 7 MB of minder voor vier of meer dagen worden beschouwd als laag gebruik virtuele machines.

Advisor ziet u de geschatte kosten van u wilt doorgaan met het uitvoeren van uw virtuele machine, zodat u kunt deze afsluiten of het formaat.  

![Advisor kosten aanbevelingen voor het vergroten of verkleinen van virtuele machines](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Een kostenbesparende oplossing gebruiken voor het beheren van prestatiedoelstellingen van meerdere SQL-databases
Advisor identificeert SQL server-exemplaren die u profiteren kunnen van het maken van pools voor elastische databases. Pools voor elastische databases bieden een eenvoudige, rendabele oplossing voor het beheren van de prestatiedoelstellingen van meerdere databases die verschillende gebruikspatronen hebben. Zie voor meer informatie over Azure elastische pools [wat is er een Azure elastische pool?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Advisor kosten aanbevelingen voor pools voor elastische databases](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Toegang tot de aanbevelingen in Azure Advisor kosten

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik in het linkerdeelvenster op **meer services**.

3. Klik in het deelvenster service menu onder **bewaking en beheer**, klikt u op **Azure Advisor**.  
 De Advisor-dashboard wordt weergegeven.

4. Klik op het dashboard Advisor de **kosten** tabblad.

5. Selecteer het abonnement waarvoor u wilt ontvangen van aanbevelingen en klik vervolgens op **aanbevelingen krijgen**.

> [!NOTE]
> Voor toegang tot de aanbevelingen Advisor te ontvangen, moet u eerst *registreren van uw abonnement* met Advisor. Een abonnement is geregistreerd als een *abonnement eigenaar* start van de Advisor-dashboard en klikt op de **aanbevelingen krijgen** knop. Dit is een *eenmalige bewerking*. Nadat het abonnement is geregistreerd, kunt u de aanbevelingen van Advisor als openen *eigenaar*, *Inzender*, of *lezer* voor een abonnement, resourcegroep of een specifieke bron.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor aanbevelingen:
* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag](advisor-get-started.md)
* [Aanbevelingen van Advisor-prestaties](advisor-cost-recommendations.md)
* [Hoge beschikbaarheid aanbevelingen Advisor te ontvangen](advisor-cost-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-cost-recommendations.md)
