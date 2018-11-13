---
title: Quota en limieten voor schalen in uw testomgeving in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het schalen van een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 00f64deaa5e02f9bb74baaf86bfdf71690a3345d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248245"
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>Quota en limieten voor schalen in DevTest Labs
Als u in DevTest Labs werkt, zult u merken dat er bepaalde standaardlimieten voor sommige Azure-resources, die invloed kunnen zijn op de DevTest Labs-service zijn. Deze limieten worden aangeduid als **quota**.

> [!NOTE]
> De DevTest Labs-service geen quota's die zijn opgelegd. Quota's die u kunt tegenkomen zijn default-beperkingen van de algemene Azure-abonnement.

U kunt elke Azure-resource gebruiken totdat u het quotum bereikt. Elk abonnement heeft afzonderlijke quotums en gebruik per abonnement wordt bijgehouden.

Elk abonnement heeft bijvoorbeeld een standaardquotum van 20 kernen. Dus als u virtuele machines in uw testomgeving met vier kernen maakt, kunt klikt u vervolgens u alleen maken vijf virtuele machines. 

[Azure-abonnement en Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits) vindt u enkele van de meest voorkomende quota's voor Azure-resources. De resources die het meest gebruikt in een testomgeving en voor die u kunt tegenkomen quota, VM-kernen, openbare IP-adressen, de netwerkinterface, beheerde schijven, toewijzing van RBAC-rol en ExpressRoute-circuits bevatten.

## <a name="view-your-usage-and-quotas"></a>Uw gebruik en quota's weergeven
Deze stappen ziet u hoe u de huidige quota weergeven in uw abonnement voor specifieke Azure-resources, en om te zien welk percentage van de quota die u hebt gebruikt.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **facturering** in de lijst.
1. Selecteer een abonnement op de blade facturering.
4. Selecteer **gebruik + quota**.

   ![Knop gebruik en quota](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   Gebruik + quota blade wordt weergegeven, aanbieding verschillende bronnen beschikbaar zijn in dat aan het abonnement en het percentage van de quota die per resource wordt gebruikt.

   ![Quota en gebruik](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Meer resources in uw abonnement aanvragen
Als u de limiet voor een quotum hebt bereikt, de standaardlimiet van een resource in een abonnement kan worden verhoogd tot een maximum is bereikt, zoals beschreven in [Azure-abonnement en Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits).

Deze stappen ziet u hoe u aanvraagt via de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **meer Services**, selecteer **facturering**, en selecteer vervolgens **gebruik + quota**.
1. Gebruik + quota blade, selecteer de **aanvraag verhogen** knop.

   ![Knop voor verhoging aanvragen](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. Vul de vereiste gegevens op alle drie tabbladen om te voltooien en dien de aanvraag, de **nieuwe ondersteuningsaanvraag** formulier.

   ![Toename aanvraagformulier](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Inzicht krijgen in Azure-limieten en verhoogt](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) vindt u meer informatie over het contact opnemen met ondersteuning van Azure om aan te vragen een quotaverhoging.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Volgende stappen
* Verken de [DevTest Labs Azure Resource Manager QuickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
