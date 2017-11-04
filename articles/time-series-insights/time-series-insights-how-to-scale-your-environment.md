---
title: Schalen van uw omgeving Azure Time Series Insights | Microsoft Docs
description: In deze zelfstudie wordt beschreven hoe u uw omgeving Azure Time Series Insights schalen
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: ba6bd1ab05bb7e24dd1bc307218e7a772fbde601
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Schalen van uw omgeving Time Series Insights

Deze zelfstudie wordt beschreven hoe uw omgeving Time Series Insights te schalen.

> [!NOTE]
> Opschaling van de verschillende typen sku is niet toegestaan. Een omgeving met een Sku S1 kan niet worden geconverteerd naar een S2-omgeving.

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU inkomend tarieven en capaciteit

| S1 SKU-capaciteit | Snelheid van inkomende gegevens | Maximale opslagcapaciteit
| --- | --- | --- |
| 1 | 1 GB (1 miljoen gebeurtenissen) | 30 GB (30 miljoen gebeurtenissen) per maand |
| 10 | 10 GB (10 miljoen gebeurtenissen) | 300 GB (300 miljoen gebeurtenissen) per maand |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU inkomend tarieven en capaciteit

| S2 SKU-capaciteit | Snelheid van inkomende gegevens | Maximale opslagcapaciteit
| --- | --- | --- |
| 1 | 10 GB (10 miljoen gebeurtenissen) | 300 GB (300 miljoen gebeurtenissen) per maand |
| 10 | 100 GB (100 miljoen gebeurtenissen) | 3 TB (3 miljard gebeurtenissen) per maand |

Capaciteitswaarden evenredig, zodat een sku S1 capaciteit 2 2 GB (2 miljoen) gebeurtenissen per dag inkomend en 60 GB (60 miljoen gebeurtenissen) per maand ondersteunt.

## <a name="changing-the-capacity-of-your-environment"></a>Het wijzigen van de capaciteit van uw omgeving

1. Selecteer de omgeving waarvan capaciteit die u wilt wijzigen in de Azure-portal.
1. Klik onder instellingen configureren.
1. Gebruik de schuifregelaar capaciteit om de capaciteit die voldoet aan de vereisten voor de tarieven van toegangsroutes en de opslagcapaciteit te selecteren.

## <a name="next-steps"></a>Volgende stappen

* Controleer of de nieuwe capaciteit om te voorkomen dat beperking voldoende is. Zie voor meer informatie de *uw omgeving kan worden opgehaald beperkt* sectie [hier](time-series-insights-diagnose-and-solve-problems.md).