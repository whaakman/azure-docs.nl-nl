---
title: De maandelijkse trend in de testomgeving geschatte kosten weergeven in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over de Azure DevTest Labs maandelijkse geschatte kosten trend van grafiek.
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>De maandelijkse trend in de testomgeving geschatte kosten weergeven in Azure DevTest Labs
De beheerfunctie van kosten van DevTest Labs helpt u bij de kosten van uw testomgeving bijhouden. In dit artikel laat zien hoe u de **maandelijkse geschatte kosten Trend** grafiek voor de huidige kalendermaand geschatte kosten-to-date en de geschatte kosten van de laatste van de maand van de huidige kalendermaand weergeven. In dit artikel leert u hoe de maandelijkse geschatte kosten trend grafiek weergeven in de Azure-portal.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>De grafiek maandelijkse geschatte kosten Trend weergeven
Als u wilt weergeven van de grafiek maandelijkse geschatte kosten Trend, de volgende stappen uit: 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
3. Selecteer de gewenste testomgeving uit de lijst van labs.   
4. Selecteer op de labblade **instellingen**.
5. Op de testomgeving **instellingen** blade Selecteer **Lab kosten trend**.
6. De volgende Schermafbeelding toont een voorbeeld van een grafiek kosten. 
   
    ![Kosten grafiek](./media/devtest-lab-configure-cost-management/graph.png)

De **geschatte kosten** waarde is de huidige kalendermaand geschatte kosten-to-date. De **geschatte kosten** de geschatte kosten voor de hele huidige kalendermaand is berekend met behulp van de kosten van het testlab voor de afgelopen vijf dagen.

De kosten zijn afgerond naar het dichtstbijzijnde gehele getal. Bijvoorbeeld: 

* 5.01 Rondt maximaal 6 
* 5.50 Rondt maximaal 6
* 5.99 Rondt maximaal 6

Als deze boven het diagram, raken de kosten die u in de grafiek ziet worden *geschatte* via [betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) tarieven aanbieden.
Daarnaast volgen *niet* opgenomen in de kostenberekening:

* CSP en Dreamspark abonnementen worden momenteel niet ondersteund als Azure DevTest Labs gebruikt de [Azure facturering API's](../billing/billing-usage-rate-card-overview.md) voor het berekenen van de testomgeving kosten, die geen ondersteuning biedt voor CSP of Dreamspark abonnementen.
* De tarieven van de aanbieding. Op dit moment kunnen kunnen we geen gebruik van de tarieven van de aanbieding (die wordt weergegeven onder uw abonnement) dat u hebt onderhandeld met Microsoft of Microsoft partners. We gebruiken de tarieven voor betalen per gebruik.
* Uw belastingen
* Uw kortingen
* Uw factureringsvaluta. Op dit moment wordt de kosten van het testlab alleen weergegeven in valuta USD.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten
* [Twee meer dingen te houden van uw kosten op bijhouden in DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Waarom kosten drempelwaarden?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele dingen om te proberen het volgende:

* [Labbeleidsregels definiëren](devtest-lab-set-lab-policy.md) -informatie over het instellen van de verschillende beleidsregels gebruikt om te bepalen hoe uw lab en bijbehorende virtuele machines worden gebruikt. 
* [Maken van aangepaste installatiekopie](devtest-lab-create-template.md) : wanneer u een virtuele machine, maakt u een basis, kan dit een aangepaste installatiekopie of een Marketplace-installatiekopie opgeven. In dit artikel laat zien hoe een aangepaste installatiekopie maken van een VHD-bestand.
* [Configureren van installatiekopieën van Marketplace](devtest-lab-configure-marketplace-images.md) - DevTest Labs ondersteunt het maken van virtuele machines op basis van Azure Marketplace-installatiekopieën. Dit artikel wordt beschreven hoe u kunt opgeven die, indien aanwezig, Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving.
* [Een virtuele machine maken in een testomgeving](devtest-lab-add-vm-with-artifacts.md) -ziet u hoe u een virtuele machine maken van een basisinstallatiekopie (een aangepaste of Marketplace), en het werken met artefacten in uw virtuele machine.

