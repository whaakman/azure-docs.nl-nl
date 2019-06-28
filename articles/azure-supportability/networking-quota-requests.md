---
title: Bestandsgroottelimiet verhoogd netwerken | Microsoft Docs
description: Netwerken bestandsgroottelimiet verhoogd
author: anavinahar
ms.author: anavin
ms.date: 06/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 48d7e9cc4a3034e149901931f2addbc7df78e2bc
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297228"
---
# <a name="networking-limit-increase"></a>Netwerken bestandsgroottelimiet verhoogd

Als u wilt weergeven van uw huidige netwerken gebruik en quota, vindt u de **gebruik + Quota** blade in Azure portal. U kunt ook gebruik [CLI](https://docs.microsoft.com//cli/azure/network?view=azure-cli-latest#az-network-list-usages), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkusage?view=azurermps-6.13.0) of de [netwerkgebruik API](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/listusage) om uw netwerkgebruik en de limieten te bekijken.

U kunt een verhoging aanvragen via **Help en ondersteuning** blade of de **gebruik + Quota** -blade in de portal.

## <a name="request-networking-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Verhoging netwerken quotum op abonnement niveau met de **Help en ondersteuning** blade

Volg de onderstaande instructies om te maken van een ondersteuningsaanvraag via Azure 'Help + ondersteuning' blade beschikbaar in de Azure portal. 

1. Van https://portal.azure.com, selecteer **Help en ondersteuning**.

    ![Help en ondersteuning](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Selecteer **Nieuwe ondersteuningsaanvraag**. 

    ![Nieuw ondersteuningsverzoek](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Kies in de vervolgkeuzelijst van het type probleem **limieten voor Service en -abonnement (quota)** .

    ![Probleem type vervolgkeuzelijst](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

    ![Abonnement selecteren newSR](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Selecteer **netwerken** in **quotumtype** vervolgkeuzelijst. 

    ![Quotumtype selecteren](./media/networking-quota-request/select-quota-type-network.png)

6. In **Details van probleem**, bieden aanvullende informatie over het proces uw aanvraag door te klikken op **vindt u informatie**.

    ![Geef details](./media/resource-manager-core-quotas-request/provide-details.png)

7. In de **quotumgegevens** deelvenster, selecteert u implementatie-model, een locatie en de Resources die u wilt een toename van aanvragen.

    ![Quotumgegevens DM](./media/networking-quota-request/quota-details-network.png)

8.  Geef de nieuwe limieten die u wilt dat voor het abonnement. Als u wilt verwijderen van een regel, schakel het selectievakje van de Resource in de vervolgkeuzelijst Resource of klik op het pictogram 'x' negeren. Na het invoeren van de gewenste quotum voor elke Resource, klikt u op **opslaan en doorgaan** in het deelvenster met details quotum om door te gaan met het maken van de aanvraag voor ondersteuning.

    ![Nieuwe grenzen](./media/networking-quota-request/network-new-limits.png)


## <a name="request-networking-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Verhoging van de netwerken op het niveau met abonnement aanvragen **gebruik + Quota** blade

Volg de instructies hieronder gebruiken voor het maken van een ondersteuningsaanvraag via Azure 'Gebruik + quota' blade beschikbaar in de Azure portal. 

1. Van https://portal.azure.com, selecteer **abonnementen**.

    ![Subscriptions](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecteer het abonnement waarvoor het quotum moet worden verhoogd.

    ![Abonnement selecteren](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecteer **gebruik + quota**

    ![Gebruik en quota's selecteren](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Selecteer in de rechterbovenhoek **verhoging**.

    ![Verhoging aanvragen](./media/resource-manager-core-quotas-request/request-increase.png)

5. Volg de stappen vanaf stap 3 van de *verhoging aanvragen toegang op abonnementsniveau* sectie met behulp van de **Help en ondersteuning** blade sectie

## <a name="about-networking-limits"></a>Over de limieten voor netwerken

Zie voor meer informatie over de limieten voor netwerken, de [netwerksectie](../azure-subscription-service-limits.md#networking-limits) van de pagina limieten of onze Veelgestelde vragen over limieten voor het netwerk