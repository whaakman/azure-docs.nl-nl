---
title: SKU-reeksen die niet beschikbaar | Microsoft Docs
description: Sommige SKU-reeksen zijn niet beschikbaar voor het geselecteerde abonnement voor deze regio.
services: Azure Supportability
documentationcenter: ''
author: stevendotwang
manager: rajatk
editor: ''
ms.service: azure-supportability
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: xingwan
ms.openlocfilehash: a57899e36a6716a6fd59cb018119c225b7396c0d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60649106"
---
# <a name="region-or-sku-unavailable"></a>Regio of SKU is niet beschikbaar
In dit artikel wordt beschreven hoe u los het probleem van een Azure-abonnement toegang kunt krijgen tot een regio of een VM-SKU.

## <a name="symptoms"></a>Symptomen

### <a name="when-deploying-a-virtual-machine-you-receive-one-of-the-following-error-messages"></a>Bij het implementeren van een virtuele machine, ontvangt u een van de volgende foutberichten:
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

### <a name="when-purchasing-reserved-virtual-machine-instances-you-receive-one-of-the-following-error-messages"></a>Bij de aanschaf van gereserveerde instanties van de virtuele Machine, ontvangt u een van de volgende foutberichten:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-creating-a-support-request-to-increase-compute-core-quota-a-region-or-a-sku-family-is-not-available-for-selection"></a>Bij het maken van een ondersteuningsaanvraag in om u te vergroot het quotum voor compute-core, is een regio of een SKU-serie niet beschikbaar voor selectie.

## <a name="solution"></a>Oplossing
Eerst wordt aangeraden dat u rekening houden met een andere regio of een SKU die voldoet aan de behoeften van uw bedrijf. Als u niet om een geschikte regio of SKU te vinden, maakt een 'Abonnementsbeheer" [ondersteuningsaanvraag](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) de onderstaande stappen te volgen:


- Op de pagina basisbeginselen probleemtype als 'Beheer van abonnementen' te selecteren, selecteert u het abonnement en klik op 'Volgende'.

![Blade Grondbeginselen](./media/SKU-series-unavailable/BasicsSubMgmt.png)


-   Selecteer het type probleem als 'Overige algemene vragen' op de pagina van het probleem.
- In de sectie Details:
  - Geef als u kijkt naar virtuele machines implementeren of koop gereserveerde instanties van de virtuele Machine
  - Geef de regio, SKU en het aantal exemplaren van virtuele machines die u van plan bent om te implementeren of kopen


![Probleem](./media/SKU-series-unavailable/ProblemSubMgmt.png)

-   Voer uw contactgegevens en klikt u op 'Maken'.

![Contactgegevens](./media/SKU-series-unavailable/ContactInformation.png)

## <a name="feedback"></a>Feedback
We zijn altijd geopend zodat u kunt feedback en suggesties. Stuur ons uw [suggesties](https://feedback.azure.com/forums/266794-support-feedback). U kunt bovendien contact met ons op via [Twitter](https://twitter.com/azuresupport) of de [MSDN-forums](https://social.msdn.microsoft.com/Forums/azure).

## <a name="learn-more"></a>Meer informatie
[Veelgestelde vragen over ondersteuning van Azure](https://azure.microsoft.com/support/faq)

