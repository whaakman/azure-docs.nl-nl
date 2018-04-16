---
title: Azure Resource Manager vCPU quotum verhogen aanvragen | Microsoft Docs
description: Azure Resource Manager vCPU quotum verhogen aanvragen
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: c22a6dde0067385a1bf8d889cc76178bb44dd0ac
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Resource Manager vCPU quotum verhogen aanvragen

Resource Manager vCPU quota worden afgedwongen op het regioniveau en de SKU-familie niveau.
Meer informatie over hoe quota's worden toegepast op de [Azure-abonnement en Servicelimieten](http://aka.ms/quotalimits) pagina.
Voor meer informatie over de SKU-Families, kunt u kosten en prestaties vergelijken op de [prijzen van virtuele Machines](http://aka.ms/pricingcompute) pagina.

Als u een verhoging aanvragen, maakt een ondersteuningsaanvraag quotum voor vcpu's in de Azure portal [https://portal.azure.com](https://portal.azure.com).

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
