---
title: Azure per VM vCPU-quotum aanvragen verhogen | Microsoft Docs
description: per VM-vCPU-quotum verhogen aanvragen
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: f921b4a95c1b0cfb29d84c0bacc17d268af6e6c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082818"
---
# <a name="vm-series-vcpu-limit-increase"></a>VM-reeks vCPU bestandsgroottelimiet verhoogd

U kunt nu een verhoging aanvragen via **Help en ondersteuning** blade of de **gebruik + Quota** -blade in de portal. 

## <a name="request-per-vm-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Per VM vCPU verhoging op bestandsserverniveau met abonnement vragen om de **Help en ondersteuning** blade

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


## <a name="request-per-vm-vcpu-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Aanvraag per VM vCPU verhoging op bestandsserverniveau met abonnement **gebruik + Quota** blade

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

