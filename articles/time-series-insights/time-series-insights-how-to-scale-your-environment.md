---
title: Schalen van uw omgeving Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe uw omgeving Azure Time Series Insights te schalen. Gebruik de Azure-portal toevoegen aan of aftrekken capaciteit binnen een prijscategorie SKU.
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: edcd9561778998c4df09cc5014f8b8ba81c0e369
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Schalen van uw omgeving Time Series Insights

In dit artikel wordt beschreven hoe de capaciteit van uw omgeving uw Time Series Insights-omgeving met de Azure portal te wijzigen. De capaciteit is de vermenigvuldiger toegepast op de snelheid van inkomende, opslagcapaciteit en kosten die zijn gekoppeld aan de geselecteerde SKU. 

U kunt de Azure-portal vergroten of verkleinen van capaciteit binnen een bepaalde prijscategorie SKU. 

Echter, als u de prijscategorie wijzigt SKU is niet toegestaan. Bijvoorbeeld, kan niet een omgeving met een S1 SKU prijzen worden geconverteerd naar een S2 of vice versa. 


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

Capaciteitswaarden evenredig, zodat een SKU S1 capaciteit 2 2 GB (2 miljoen) gebeurtenissen per dag inkomend en 60 GB (60 miljoen gebeurtenissen) per maand ondersteunt.

## <a name="change-the-capacity-of-your-environment"></a>De capaciteit van uw omgeving wijzigen
1. In de Azure portal, zoek en selecteer uw Time Series Insights-omgeving. 

2. Selecteer in het menu voor uw omgeving Time Series Insighs **configureren**.

   ![Configure.PNG](media/scale-your-environment/configure.png)

3. Pas de **capaciteit** schuifregelaar om de capaciteit die voldoet aan de vereisten voor de tarieven van toegangsroutes en de opslagcapaciteit te selecteren. U ziet de **inkomend snelheid**, **opslagcapaciteit**, en **geschatte kosten** update dynamisch aan het weergeven van de gevolgen van de wijziging. 

   ![Schuifregelaar](media/scale-your-environment/slider.png)

   U kunt ook het nummer van de capaciteit vermenigvuldiger typen in het tekstvak aan de rechterkant van de schuifregelaar. 

4. Selecteer **opslaan** schalen van de omgeving. De voortgangsindicator wordt weergegeven nadat de wijziging doorgevoerd, tijdelijk worden is. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Controleer of de nieuwe capaciteit is voldoende om te voorkomen dat beperking](time-series-insights-diagnose-and-solve-problems.md).
