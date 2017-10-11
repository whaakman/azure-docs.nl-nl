---
title: Azure Resource Manager core quotum verhogen aanvragen | Microsoft Docs
description: Azure Resource Manager core quotum verhogen aanvragen
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: cb6c5b3e86f126d4110d1cd29d8c9891e356e414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-core-quota-increase-requests"></a>Resource Manager core quotum verhogen aanvragen

Resource Manager core quota worden afgedwongen op het regioniveau en de SKU-familie niveau.
Meer informatie over hoe quota's worden toegepast op de [Azure-abonnement en Servicelimieten](http://aka.ms/quotalimits) pagina.
Voor meer informatie over de SKU-Families, kunt u kosten en prestaties vergelijken op de [prijzen van virtuele Machines](http://aka.ms/pricingcompute) pagina.

Voor een verhoging aanvragen, maakt u een ondersteuningsaanvraag quotum voor kernen in de Azure portal [https://portal.azure.com](https://portal.azure.com).

> [!NOTE]
> Meer informatie over hoe [Maak een ondersteuningsaanvraag](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) in de Azure portal

1. Selecteer type probleem als 'Target' en quotumtype als 'Kernen' op de pagina nieuwe ondersteuning aanvraag.

    ![De blade grondbeginselen quotum](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. Selecteer implementatiemodel als 'Resource Manager' en selecteer een locatie.

    ![Quota voor probleem-blade](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Selecteer de SKU-Families waarvoor verhogen.

    ![SKU-serie geselecteerd](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Voer de nieuwe beperkingen die u wilt dat op het abonnement.

    ![Nieuwe quotumaanvraag SKU](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Een regel verwijderen, schakelt u de SKU van de SKU-familie dropdown of klik op het pictogram 'x' negeren.
Na het invoeren van de gewenste quota voor elke SKU-serie, klikt u op 'Volgende' op de pagina probleem stap om door te gaan met het maken van de aanvraag voor ondersteuning.
