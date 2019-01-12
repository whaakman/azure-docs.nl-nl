---
title: Services in Azure Stack-aanbieding | Microsoft Docs
description: Als een cloud-operator, kunt u services aanbieden aan uw gebruikers.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 754a0002aca52462910abe50825cff0254a7068f
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244151"
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Overzicht van services in Azure Stack-aanbieding

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

[Microsoft Azure Stack](azure-stack-poc.md) is een hybride cloudplatform waarmee u services leveren vanuit uw datacenter. Als een serviceprovider kunt u services aanbieden aan uw tenants. Binnen een bedrijf of overheidsinstelling heeft, kunt u on-premises services aanbieden aan uw werknemers. 

U kunt aanbieden [Infrastructure as a Service](https://azure.microsoft.com/overview/what-is-iaas/) (IaaS)-services die uw gebruikers te maken van een computinginfrastructuur op aanvraag, ingericht en beheerd via de Azure Stack-gebruikersportal.

U kunt ook implementeren [Platform as a Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS)-services voor Azure Stack van Microsoft en andere 3e leveranciers. De services die u kunt leveren bevatten, maar niet beperkt tot:

- [Een App Service-resourceprovider aan Azure Stack toevoegen](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-overview)

- [Een SQL Server-resourceprovider toevoegen aan Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-sql-resource-provider-deploy)

- [Een resourceprovider van MySQL-Server toevoegen aan Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-mysql-resource-provider-deploy)


U kunt zelfs combineren services om te integreren en te maken van complexe oplossingen voor verschillende gebruikers.

Ze kunnen deze services bieden voor uw gebruikers, moet u [plannen, aanbiedingen en quota's](azure-stack-plan-offer-quota-overview.md). Uw gebruikers kunnen zich vervolgens aanmelden op uw aanbiedingen om de services te gebruiken.

## <a name="plan-your-service-offers"></a>Plannen van uw service-aanbiedingen

Wanneer u van plan bent uw aanbiedingen, houd rekening met de volgende punten:

**Proefaanbiedingen**: U kunt Proefaanbiedingen gebruiken voor het aantrekken van nieuwe gebruikers, die vervolgens met extra services kunnen upgraden. Voor het maken van een proefversie, maakt u een kleine [basisplan](azure-stack-plan-offer-quota-overview.md#base-plan) met een optionele grotere aanvullende plan.

**Capaciteitsplanning**: Het is mogelijk dat u zich zorgen maken over gebruikers die grote hoeveelheden van resources en het systeem voor alle gebruikers overvol ophalen. Om te helpen de prestaties, kunt u [configureren van uw abonnementen met quota](azure-stack-plan-offer-quota-overview.md#plans) op cap-gebruik.

**Gedelegeerde providers**: U kunt andere de mogelijkheid om te maken biedt in uw omgeving verlenen. Bijvoorbeeld, als u een serviceprovider bent, kunt u [delegeren](azure-stack-delegated-provider.md) deze mogelijkheid aan uw wederverkopers. Of, als u een organisatie bent, kunt u delegeren aan andere afdelingen/dochterondernemingen.

## <a name="next-steps"></a>Volgende stappen

[Een aanbieding maken in Azure Stack](azure-stack-create-offer.md)
