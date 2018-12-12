---
title: Capaciteit van de resource voor de implementatie - QnA Maker
titleSuffix: Azure Cognitive Services
description: Voordat u uw QnA Maker-service maakt, moet u bepalen welke laag van de bovenstaande services is geschikt voor u.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9e197929ce08f4e0c665f96d1c4ddbd382fdfb22
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084449"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Capaciteit voor uw implementatie QnA Maker kiezen

De QnA Maker-service heeft een afhankelijkheid op drie Azure-resources:
1.  App Service (voor de runtime)
2.  Azure Search (voor het opslaan van vragen en antwoorden supereenvoudig)
3.  App Insights (optioneel, voor het opslaan van chatlogs en telemetrie)

Voordat u uw QnA Maker-service maakt, moet u bepalen welke laag van de bovenstaande services is geschikt voor u. 

Er zijn doorgaans drie parameters die u moet overwegen:
1. **De doorvoer die u nodig hebt van de service**: Selecteer de juiste [App Plan](https://azure.microsoft.com/pricing/details/app-service/plans/) voor uw appservice op basis van uw behoeften. U kunt [omhoog schalen](https://docs.microsoft.com/azure/app-service/web-sites-scale) of u de App. Dit moet ook van invloed zijn op uw Azure Search-SKU-selectie, Zie meer informatie [hier](https://docs.microsoft.com/azure/search/search-sku-tier).

2. **Grootte en het aantal knowledge bases**: Kies de juiste [Azure zoeken SKU](https://azure.microsoft.com/pricing/details/search/) voor uw scenario. U kunt N-1 knowledge bases publiceren in een bepaalde laag, waarbij N staat voor het maximale aantal indexen toegestaan in de laag. Controleer ook de maximale grootte en het aantal documenten toegestaan per laag.

3. **Aantal documenten als bron**: de gratis SKU van de QnA Maker-management-service beperkt het aantal documenten die u via de portal en de API's naar 3 (van 1 MB grootte, elk beheren kunt). De standaard SKU heeft geen limieten voor het aantal documenten die u kunt beheren. Meer informatie [hier](https://aka.ms/qnamaker-pricing).

De volgende tabel biedt richtlijnen op hoog niveau.

|                        | QnA Maker-Management | App Service | Azure Search | Beperkingen                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experiment        | Gratis SKU             | Gratis versie   | Gratis versie    | Maximaal 2 kB's, 50 MB's publiceren  |
| Dev/Test-omgeving   | Standaard SKU         | Gedeeld      | Basic        | Publiceren tot 14 kB's, grootte van 2 GB    |
| Productie-omgeving | Standaard SKU         | Basic       | Standard     | Maximaal 49 kB's, 25 GB grootte publiceren |

Zie voor het upgraden van uw stack QnA Maker, [uw QnA Maker-service upgraden](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Upgrade uw QnA Maker-service](../How-To/upgrade-qnamaker-service.md)
