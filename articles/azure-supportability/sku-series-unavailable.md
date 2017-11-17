---
title: SKU-serie is niet beschikbaar | Microsoft Docs
description: Sommige SKU-serie zijn niet beschikbaar voor het geselecteerde abonnement voor deze regio.
services: Azure Supportability
documentationcenter: 
author: stevendotwang
manager: rajatk
editor: 
ms.service: azure-supportability
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: xingwan
ms.openlocfilehash: 62964d0c5d75168226a35b25e5c256a1b57f3f81
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="region-or-sku-unavailable"></a>Regio of SKU is niet beschikbaar
In dit artikel wordt beschreven hoe los het probleem van een Azure-abonnement geen toegang hebben tot een regio of een VM-SKU.

## <a name="symptoms"></a>Symptomen

### <a name="when-deploying-a-virtual-machine-you-receive-one-of-the-following-error-messages"></a>Wanneer u een virtuele machine implementeert, wordt een van de volgende foutberichten weergegeven:
```
Code: SkuNotAvailable
Message: The requested size for resource '<resource>' is currently not available in location 
'<location>' zones '<zone>' for subscription '<subscriptionID>'. Please try another size or 
deploy to a different location or zones. See https://aka.ms/azureskunotavailable for details.
```

```
Message: Your subscription doesn’t support virtual machine creation in <location>. Choose a 
different location. Supported locations are <list of locations>
```

```
Code: NotAvailableForSubscription
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-purchasing-reserved-virtual-machine-instances-you-receive-one-of-the-following-error-messages"></a>Bij de aanschaf van gereserveerde virtuele Machine-exemplaren wordt een van de volgende foutberichten weergegeven:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-creating-a-support-request-to-increase-compute-core-quota-a-region-or-a-sku-family-is-not-available-for-selection"></a>Bij het maken van een ondersteuningsverzoek compute core quotum te verhogen, een regio of een SKU-serie is niet beschikbaar voor selectie.

## <a name="solution"></a>Oplossing
Eerst wordt aangeraden dat u rekening houden met een andere regio of SKU die voldoet aan de behoeften van uw bedrijf. Als het niet lukt om een geschikte regio of SKU te vinden, maken 'Abonnement Management' [ondersteuningsaanvraag](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) de onderstaande stappen te volgen:


- Op de pagina basisbeginselen probleem selecteren als 'Abonnement Management', selecteer het abonnement en klik op 'Volgende'.

![Blade Grondbeginselen](./media/SKU-series-unavailable/BasicsSubMgmt.png)


-   Selecteer op de pagina probleem het probleemtype als 'Andere algemene vragen'.
- In de sectie Details:
  - Geef als u zoekt voor het implementeren van virtuele machines of kopen van gereserveerde virtuele Machine-exemplaren
  - Geef de regio, SKU en het aantal exemplaren van virtuele machines die u van plan bent om te implementeren of kopen


![Probleem](./media/SKU-series-unavailable/ProblemSubMgmt.png)

-   Uw contactgegevens en klik op 'Maken'.

![Contactgegevens](./media/SKU-series-unavailable/ContactInformation.png)

## <a name="feedback"></a>Feedback
We kunnen worden altijd feedback en suggesties! Stuur ons uw [suggesties](https://feedback.azure.com/forums/266794-support-feedback). U kunt bovendien benaderen met ons opnemen via [Twitter](https://twitter.com/azuresupport) of de [MSDN-forums](https://social.msdn.microsoft.com/Forums/azure).

## <a name="learn-more"></a>Meer informatie
[Veelgestelde vragen over ondersteuning van Azure](https://azure.microsoft.com/support/faq)

