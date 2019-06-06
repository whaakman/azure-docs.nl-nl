---
title: Azure Resource Manager-vCPU-quotum aanvragen verhogen | Microsoft Docs
description: Azure Resource Manager-vCPU-aanvragen voor het verhogen van quota
author: sowmyavenkat86
ms.author: svenkat
ms.date: 05/09/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9d19d1a993d574777aa630b9c58f2472b94716cd
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479386"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Resource Manager-vCPU-aanvragen voor het verhogen van quota

Resource Manager-vCPU-quota voor virtuele machines en virtuele-machineschaalsets worden afgedwongen op twee lagen voor elk abonnement, in elke regio. 

De eerste laag is de limiet van het totaal aantal regionale vcpu's (voor alle VM-serie) en de tweede laag is de limiet van de VM-reeks vcpu's (zoals de D-serie vcpu's). Telkens wanneer die een nieuwe virtuele machine kan worden geïmplementeerd, is de som van de nieuwe en bestaande vcpu's gebruik voor die VM-reeks mag niet groter zijn dan het vCPU-quotum voor die specifieke VM-serie verdere goedgekeurd, het aantal van de totale vCPU's geïmplementeerd op alle VM-reeksen mag niet groter zijn dan het totaal aantal regionale Vcpu-quotum  goedgekeurd voor het abonnement. Als een van deze quota worden overschreden, wordt de VM-implementatie niet toegestaan.
U kunt een verhoging van de quotumlimiet voor uw vcpu's voor de VM-reeks aanvragen vanuit Azure portal. De limiet van het totaal aantal regionale vcpu's wordt een toename van het quotum-serie VM's automatisch evenveel. 

Wanneer een nieuw abonnement wordt gemaakt, de standaard-totaal aantal regionale vcpu's mag niet gelijk zijn aan de som van de standaard vCPU-quota's voor alle afzonderlijke VM-serie. Hiermee kan leiden tot een abonnement met voldoende quotum voor elke afzonderlijke VM-reeks die u wilt implementeren , maar het abonnement beschikt niet over voldoende quotum voor totaal aantal regionale vcpu's voor alle implementaties. In dit geval moet u een aanvraag indient bij de totale verhoging voor de regio de limiet expliciet verhogen. Totaal aantal regionale vcpu's beperken niet langer zijn dan de som van de goedgekeurde quotum voor alle VM-reeksen voor de regio.

Meer informatie over quota's in de [virtuele machine vCPU-quota pagina](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) en [Azure-abonnement en Servicelimieten](https://aka.ms/quotalimits) pagina. 

U kunt nu een verhoging aanvragen via **Help en ondersteuning** blade of de **gebruik + Quota** -blade in de portal. 

## <a name="request-quota-increase-using-the-help--support-blade"></a>Aanvraag quotum toename gebruiken de **Help en ondersteuning** blade

Volg de onderstaande instructies om te maken van een ondersteuningsaanvraag via Azure 'Help + ondersteuning' blade beschikbaar in de Azure portal. 

1. Van https://portal.azure.com, selecteer **Help en ondersteuning**.

![Help en ondersteuning](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Selecteer **Nieuwe ondersteuningsaanvraag**. 

![Nieuw ondersteuningsverzoek](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Kies in de vervolgkeuzelijst van het type probleem **limieten voor Service en -abonnement (quota)** .

![Probleem type vervolgkeuzelijst](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

![Abonnement selecteren newSR](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Selecteer **Compute - VM (kernen vcpu's) voor uw abonnement wordt verhoogd** in **quotumtype** vervolgkeuzelijst. 

![Quotumtype selecteren](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. In **Details van probleem**, bieden aanvullende informatie over het proces uw aanvraag door te klikken op **vindt u informatie**.

![Geef details](./media/resource-manager-core-quotas-request/provide-details.png)

7. In de **quotumgegevens** deelvenster, het implementatiemodel selecteren en selecteer een locatie.

![Quotumgegevens DM](./media/resource-manager-core-quotas-request/quota-details.png)

8. Selecteer de **SKU-families** waarvoor een verhoging. 

![SKU-familie](./media/resource-manager-core-quotas-request/sku-family.png)

9. Geef de nieuwe limieten die u wilt dat voor het abonnement. Als u wilt verwijderen van een regel, schakel het selectievakje van de SKU in de SKU-familie vervolgkeuzelijst of klik op het pictogram 'x' negeren. Na het invoeren van de gewenste quotum voor elke SKU-familie, klikt u op **opslaan en doorgaan** in het deelvenster met details quotum om door te gaan met het maken van de aanvraag voor ondersteuning.

![Nieuwe grenzen](./media/resource-manager-core-quotas-request/new-limits.png)


## <a name="request-quota-increase-at-subscription-level-using-usages--quota"></a>Verhoging van quota op abonnementsniveau met behulp van het gebruik + Quota

Volg de instructies hieronder gebruiken voor het maken van een ondersteuningsaanvraag via Azure 'Gebruik + quota' blade beschikbaar in de Azure portal. 

1. Van https://portal.azure.com, selecteer **abonnementen**.

![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

![Abonnement selecteren](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecteer **gebruik + quota**

![Gebruik en quota's selecteren](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Selecteer in de rechterbovenhoek **verhoging**.

![Verhoging aanvragen](./media/resource-manager-core-quotas-request/request-increase.png)

5. Selecteer **verhoogt de Compute-VM (kernen vcpu's) voor uw abonnement** als het type offerte. 

![Formulier invullen](./media/resource-manager-core-quotas-request/forms.png)
   
6. In de **quotumgegevens** deelvenster, het implementatiemodel selecteren en selecteer een locatie.

![Quotum probleem blade](./media/resource-manager-core-quotas-request/problemstep.png)

7. Selecteer de **SKU-Families** waarvoor een verhoging.

![SKU-reeksen geselecteerd](./media/resource-manager-core-quotas-request/sku-family.png)

8. Geef de nieuwe limieten die u wilt dat voor het abonnement. Als u wilt verwijderen van een regel, schakel het selectievakje van de SKU in de SKU-familie vervolgkeuzelijst of klik op het pictogram 'x' negeren. Na het invoeren van de gewenste quotum voor elke SKU-familie, klikt u op **opslaan en doorgaan** op de pagina van het probleem stap om door te gaan met het maken van de aanvraag voor ondersteuning.

![Nieuwe quotumaanvraag-SKU](./media/resource-manager-core-quotas-request/new-limits.png)


