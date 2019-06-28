---
title: Regionale Azure-quotum aanvragen verhogen | Microsoft Docs
description: Aanvragen voor het regionale quotum verhogen
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7ac6dce9aec1c363fa9772caabad9ce542d43888
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310654"
---
# <a name="total-regional-vcpu-limit-increase"></a>De algehele regionale vCPU-limiet verhogen 

Resource Manager-vCPU-quota voor virtuele machines en virtuele-machineschaalsets worden afgedwongen op twee lagen voor elk abonnement, in elke regio. 

De eerste laag is de **totaal aantal regionale vcpu's limiet** (voor alle VM-serie), en de tweede laag is de **per VM-reeks vcpu's beperken** (zoals de D-serie vcpu's). Telkens wanneer die een nieuwe virtuele machine kan worden geïmplementeerd, is moet de som van de nieuwe en bestaande vcpu's gebruik voor die VM-reeks het vCPU-quotum goedgekeurd voor die specifieke VM-reeks niet overschrijden. Het totaal aantal nieuwe en bestaande vCPU-aantal geïmplementeerd op alle VM-reeksen mag bovendien niet meer dan het totaal aantal regionale Vcpu-quotum goedgekeurd voor het abonnement. Als een van deze quota worden overschreden, wordt de VM-implementatie niet toegestaan.
U kunt een verhoging van de quotumlimiet voor uw vcpu's voor de VM-reeks aanvragen vanuit Azure portal. De limiet van het totaal aantal regionale vcpu's wordt een toename van het quotum-serie VM's automatisch evenveel. 

Wanneer een nieuw abonnement wordt gemaakt, de standaard-totaal aantal regionale vcpu's mogelijk niet gelijk zijn aan de som van de standaard vCPU-quota's voor alle afzonderlijke VM-reeksen. Dit kan resulteren in een abonnement voldoende quotum voor elke afzonderlijke VM-reeks die u wilt implementeren, maar er is onvoldoende quotum voor het totaal aantal regionale vcpu's voor alle implementaties. In dit geval moet u een verzoek indienen om de limiet van totaal aantal regionale vcpu's expliciet verhogen. Totaal aantal regionale vcpu's beperken niet langer zijn dan de som van de goedgekeurde quotum voor alle VM-reeksen voor de regio.

Meer informatie over quota's in de [virtuele machine vCPU-quota pagina](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) en [Azure-abonnement en Servicelimieten](https://aka.ms/quotalimits) pagina. 

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



