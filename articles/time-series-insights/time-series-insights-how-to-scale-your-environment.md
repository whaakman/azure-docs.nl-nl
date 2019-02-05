---
title: Het schalen van uw Azure Time Series Insights-omgeving | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw Azure Time Series Insights-omgeving te schalen. De Azure portal gebruiken om te worden toegevoegd of verwijderd binnen een prijs-SKU.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.custom: seodec18
ms.openlocfilehash: f6c2c8c49f34c707c7b0fb4d525337fbfa169a3e
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692422"
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Het schalen van uw Time Series Insights-omgeving

In dit artikel wordt beschreven hoe u uw Time Series Insights-omgeving met behulp van de Azure-portal van de capaciteit van uw omgeving wijzigen. Capaciteit is de vermenigvuldigingsfactor die wordt toegepast op de gegevens, capaciteit en kosten die gepaard gaan met de geselecteerde SKU. 

De Azure-portal kunt u meer of minder capaciteit binnen een bepaalde prijs-SKU. 

Echter, de prijscategorie wijzigen SKU is niet toegestaan. Bijvoorbeeld, kan niet een omgeving met een S1 prijs-SKU worden geconverteerd naar een S2 of vice versa. 


## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU inkomend tarieven en -capaciteit

| S1 SKU-capaciteit | Gegevens | Maximale opslagcapaciteit
| --- | --- | --- |
| 1 | 1 GB (1 miljoen gebeurtenissen) | 30 GB (30 miljoen gebeurtenissen) per maand |
| 10 | 10 GB (10 miljoen gebeurtenissen) | 300 GB (300 miljoen gebeurtenissen) per maand |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU inkomend tarieven en -capaciteit

| S2 SKU-capaciteit | Gegevens | Maximale opslagcapaciteit
| --- | --- | --- |
| 1 | 10 GB (10 miljoen gebeurtenissen) | 300 GB (300 miljoen gebeurtenissen) per maand |
| 10 | 100 GB (100 miljoen gebeurtenissen) | (3 miljoen gebeurtenissen) 3 TB per maand |

Capaciteiten schalen lineair, dus een S1 SKU met capaciteit 2 biedt ondersteuning voor 2 GB (2 miljoen) gebeurtenissen per dag inkomend verkeer en 60 GB (60 miljoen gebeurtenissen) per maand.

## <a name="change-the-capacity-of-your-environment"></a>De capaciteit van uw omgeving wijzigen
1. Zoek in de Azure-portal en selecteer uw Time Series Insights-omgeving. 

2. Selecteer in het menu voor uw Time Series Insights-omgeving, **configureren**.

   ![configure.png](media/scale-your-environment/configure.png)

3. Pas de **capaciteit** schuifregelaar om de capaciteit die voldoet aan de vereisten voor de tarieven voor inkomend verkeer en opslagcapaciteit te selecteren. U ziet dat de **gegevens**, **opslagcapaciteit**, en **geschatte kosten** update dynamisch om weer te geven van de gevolgen van de wijziging. 

   ![Schuifregelaar](media/scale-your-environment/slider.png)

   U kunt ook het nummer van de capaciteit vermenigvuldiger typen in het tekstvak aan de rechterkant van de schuifregelaar. 

4. Selecteer **opslaan** voor het schalen van de omgeving. De voortgangsindicator wordt weergegeven nadat de wijziging doorgevoerd, tijdelijk wordt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Controleer of de nieuwe capaciteit is voldoende om te voorkomen dat beperking](time-series-insights-diagnose-and-solve-problems.md).
