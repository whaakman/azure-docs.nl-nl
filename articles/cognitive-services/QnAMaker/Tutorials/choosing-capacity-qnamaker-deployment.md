---
title: Het kiezen van de capaciteit voor uw implementatie QnA Maker - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: een handleiding voor het kiezen van de capaciteit voor uw implementatie QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b0219b9f7dbbee52406dab9d808134fa2e2a689d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345301"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Capaciteit voor uw implementatie QnA Maker kiezen

De Maker Writer-service heeft een afhankelijkheid op drie Azure-resources:
1.  App Service (voor de runtime)
2.  Azure Search (voor het opslaan van QnAs)
3.  App Insights (optioneel, voor het opslaan van chatlogs en telemetrie)

Voordat u uw QnA Maker-service maakt, moet u bepalen welke lagen van de bovenstaande services geschikt is voor u. 

Er zijn doorgaans drie parameters, moet u rekening houden:
1. **De doorvoer die u nodig hebt van de service**: Selecteer de relevante [App Plan](https://azure.microsoft.com/en-in/pricing/details/app-service/plans/) voor uw App-service op basis van uw behoeften. U kunt [opschalen](https://docs.microsoft.com/en-us/azure/app-service/web-sites-scale) of u de App. Dit moet ook van invloed zijn op uw selectie Azure Search SKU Zie om meer details [hier](https://docs.microsoft.com/en-us/azure/search/search-sku-tier).

2. **Grootte en het aantal kennis basissen**: Kies de juiste [Azure zoeken SKU](https://azure.microsoft.com/en-in/pricing/details/search/) voor uw scenario. U kunt N-1 kennis basissen publiceren in een bepaalde laag, waarbij N staat voor de maximale indexen toegestaan in de laag. Controleer ook de maximale grootte en het aantal documenten dat is toegestaan per laag.

3. **Het aantal documenten als bronnen**: de gratis SKU van de beheerservice QnA Maker beperkt het aantal documenten die u via de portal en de API's op 3 (van 1 MB grootte, elke beheren kunt). De standaard SKU heeft geen beperkingen voor het aantal documenten die u kunt beheren. Meer informatie [hier](https://aka.ms/qnamaker-pricing).

De volgende tabel vindt u enkele richtlijnen op hoog niveau.

|                        | QnA Maker-Management | App Service | Azure Search | Beperkingen                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experiment        | Beschikbare SKU             | Gratis versie   | Gratis versie    | Publiceren van maximaal 2 kB, grootte 50 MB  |
| De omgeving ontwikkelen en testen   | Standaard SKU         | Gedeeld      | Basic        | Publiceren van maximaal 4 kB, de grootte van 2GB    |
| Productie-omgeving | Standaard SKU         | Basic       | Standard     | Maximaal 49 kB, 25 GB grootte publiceren |

Zie voor het upgraden van uw QnA Maker-stack [Upgrade uw service QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Upgrade uw QnA Maker-service](../How-To/upgrade-qnamaker-service.md)
