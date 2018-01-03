---
title: Quota en limieten schalen in uw lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het schalen van een testomgeving in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: v-craic
ms.openlocfilehash: 7ea022dfb39154ec86c26856030111beee672bd6
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>Schaal quota en limieten in DevTest Labs
Als u in DevTest Labs werkt, kunt u wellicht opgevallen dat er zijn bepaalde standaardlimieten voor sommige Azure-resources die de service DevTest Labs kunnen beïnvloeden. Deze limieten worden aangeduid als **quota**.

> [!NOTE]
> De service DevTest Labs opleggen niet geen quota's. Quota's die u kunt tegenkomen zijn default-beperkingen van de algehele Azure-abonnement.

U kunt elke Azure-resource gebruiken totdat u het quotum bereikt. Elk abonnement heeft een afzonderlijke quota's en gebruik per abonnement wordt bijgehouden.

Elk abonnement heeft bijvoorbeeld een standaardquotum van 20 kernen. Dus als u virtuele machines in uw testomgeving met vier cores maakt, kunt klikt u vervolgens u alleen maken vijf virtuele machines. 

[Azure-abonnement en Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits) vindt u enkele van de meest voorkomende quota voor Azure-resources. De bronnen worden meestal gebruikt in een testomgeving en voor die u kunt tegenkomen quota, VM kernen, openbare IP-adressen, netwerkinterface, beheerde schijven, RBAC roltoewijzing en ExpressRoute-circuits bevatten.

## <a name="view-your-usage-and-quotas"></a>Uw gebruik en quota's weergeven
Deze stappen ziet u hoe de huidige quota in uw abonnement voor specifieke Azure-resources bekijken en om te zien welk percentage van elke quota die u hebt gebruikt.

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **facturering** uit de lijst.
1. Selecteer een abonnement op de blade facturering.
4. Selecteer **gebruik + quota**.

   ![Knop voor informatie over het gebruik en quota 's](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   De informatie over het gebruik + quota blade wordt weergegeven, aanbieding verschillende bronnen beschikbaar zijn in het abonnement en het percentage van de quota voor de per resource wordt gebruikt.

   ![Quota's en gebruik](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Aanvragen meer resources in uw abonnement
Als u een quotum-limiet bereikt, de standaardlimiet van een resource in een abonnement kan worden verhoogd tot een maximumlimiet zoals beschreven in [Azure-abonnement en Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits).

Deze stappen ziet u hoe u kunt een verhoging van het quotum via aanvragen de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **meer Services**, selecteer **facturering**, en selecteer vervolgens **gebruik + quota**.
1. Gebruik + quota blade, selecteer de **aanvragen verhogen** knop.

   ![Knop voor toename van aanvragen](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. Om te voltooien en dien de aanvraag, vul de vereiste gegevens op alle drie tabbladen van de **nieuw ondersteuningsverzoek** formulier.

   ![Toename aanvraagformulier](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Informatie over Azure-limieten en toeneemt](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) vindt u meer informatie over het contact opnemen met ondersteuning van Azure om aan te vragen een verhoging van het quotum.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Volgende stappen
* Verken de [DevTest Labs Azure Resource Manager QuickStart sjablonengalerie](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
