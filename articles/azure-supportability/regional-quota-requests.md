---
title: Regionale Azure-quotum aanvragen verhogen | Microsoft Docs
description: Aanvragen voor het regionale quotum verhogen
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 4c5524cf450959db7055ca5d032c81f79ebac077
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083078"
---
# <a name="total-regional-vcpu-limit-increase"></a>Totaal aantal regionale vCPU bestandsgroottelimiet verhoogd 

U kunt nu een verhoging aanvragen via **Help en ondersteuning** blade of de **gebruik + Quota** -blade in de portal. 

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Totaal aantal regionale vcpu's verhoging op bestandsserverniveau met abonnement aanvragen de **Help en ondersteuning** blade

Volg de onderstaande instructies om te maken van een ondersteuningsaanvraag via Azure 'Help + ondersteuning' blade beschikbaar in de Azure portal. 

1. Van https://portal.azure.com, selecteer **Help en ondersteuning**.

![Help en ondersteuning](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Selecteer **Nieuwe ondersteuningsaanvraag**. 

![Nieuw ondersteuningsverzoek](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Kies in de vervolgkeuzelijst van het type probleem **limieten voor Service en -abonnement (quota)** .

![Probleem type vervolgkeuzelijst](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

![Abonnement selecteren newSR](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Selecteer **andere aanvragen** in **quotumtype** vervolgkeuzelijst.

![QuotaType](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. In **Details** deelvenster bevatten aanvullende informatie aan de hand van het voorbeeld hieronder om te verwerken van uw aanvraag en doorgaan met het maken van de aanvraag. 
    1.  **Implementatiemodel** – Geef de Resource Manager
    2.  **Gevraagde regio** – Geef de vereiste regio bijvoorbeeld VS-Oost 2
    3.  **Nieuwe limiet waarde** – nieuwe limiet voor de regio opgeven. Dit mag niet groter zijn dan de som van de goedgekeurde quotum voor afzonderlijke SKU-families voor dit abonnement

![QuotaDetails](./media/resource-manager-core-quotas-request/regional-details.png)

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-usages--quota-blade"></a>Totaal aantal regionale vcpu's verhoging op bestandsserverniveau met abonnement aanvragen de **gebruik + Quota** blade

Volg de instructies hieronder gebruiken voor het maken van een ondersteuningsaanvraag via Azure 'Gebruik + quota' blade beschikbaar in de Azure portal. 

1. Van https://portal.azure.com, selecteer **abonnementen**.

![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

![Abonnement selecteren](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecteer **gebruik + quota**

![Gebruik en quota's selecteren](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Selecteer in de rechterbovenhoek **verhoging**.

![Verhoging aanvragen](./media/resource-manager-core-quotas-request/request-increase.png)

5. Selecteer **andere aanvragen** in **quotumtype** vervolgkeuzelijst.

![QuotaType](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. In **Details** deelvenster bevatten aanvullende informatie aan de hand van het voorbeeld hieronder om te verwerken van uw aanvraag en doorgaan met het maken van de aanvraag. 
    1.  **Implementatiemodel** – Geef de Resource Manager
    2.  **Gevraagde regio** – Geef de vereiste regio bijvoorbeeld VS-Oost 2
    3.  **Nieuwe limiet waarde** – nieuwe limiet voor de regio opgeven. Dit mag niet groter zijn dan de som van de goedgekeurde quotum voor afzonderlijke SKU-families voor dit abonnement

![QuotaDetails](./media/resource-manager-core-quotas-request/regional-details.png)



