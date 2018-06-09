---
title: Het aanbieden van services in Azure-Stack | Microsoft Docs
description: Als een cloud-operator, kunt u services aanbieden aan uw gebruikers.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 042e65cfe350cb61124ed8920ae3616502e6553d
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248840"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Overzicht van services in Azure-Stack van aanbieding

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Microsoft Azure-Stack](azure-stack-poc.md) is een hybride cloud-platform waarmee u services leveren van uw datacenter. Als een serviceprovider, kunt u services aanbieden aan uw tenants. U kunt binnen een bedrijf of overheidsinstelling lokale services aanbieden aan uw werknemers. De services die u kunt leveren omvatten, maar niet beperkt tot:

- Platform als een Service (PaaS)-services zoals App Services, API-Apps, API-functies, SQL en MySQL.

U kunt zelfs combineren services om te integreren en bouwen van complexe oplossingen voor verschillende gebruikers.

Deze om services te leveren aan uw gebruikers, moet u [plannen, aanbiedingen en quota's](azure-stack-plan-offer-quota-overview.md). Uw gebruikers kunnen zich vervolgens aanmelden bij uw aanbiedingen gebruik van services.

## <a name="plan-your-service-offers"></a>Plannen van uw service-aanbiedingen

Houd rekening met de volgende punten wanneer u van plan bent uw aanbiedingen:

**Proefaanbiedingen**: U kunt Proefaanbiedingen gebruiken om aan te trekken van nieuwe gebruikers, die vervolgens een upgrade met extra services uitvoeren kunnen. Voor het maken van een proefabonnement maken een klein [basisplan](azure-stack-plan-offer-quota-overview.md#base-plan) met een optionele groter invoegtoepassing plan.

**Capaciteitsplanning**: mogelijk betrokken over gebruikers die grote hoeveelheden resources en het systeem voor alle gebruikers vertraging halen. Om prestaties te verbeteren, kunt u [uw plannen met quota's configureren](azure-stack-plan-offer-quota-overview.md#plans) op cap-gebruik.

**Providers overgedragen**: verleent u anderen aanbiedingen maken in uw omgeving. Bijvoorbeeld, als u een serviceprovider bent, kunt u [delegeren](azure-stack-delegated-provider.md) deze mogelijkheid tot uw leverancier. Of als u een organisatie, kunt u delegeren aan andere afdelingen/dochterondernemingen.

## <a name="next-steps"></a>Volgende stappen

[Maken van een aanbieding in Azure-Stack](azure-stack-create-offer.md)
