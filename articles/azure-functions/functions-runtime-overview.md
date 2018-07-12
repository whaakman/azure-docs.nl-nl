---
title: Overzicht van de Runtime van Azure Functions | Microsoft Docs
description: Overzicht van de Preview van Azure Functions-Runtime
services: functions
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 2be04c7037dd26755300cf8b7794678a6a958278
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38488457"
---
# <a name="azure-functions-runtime-overview-preview"></a>Azure Functions Runtime overzicht (preview)

De Azure Functions-Runtime (preview) biedt een nieuwe manier voor u om te profiteren van de eenvoud en flexibiliteit van de Azure-Functions programming model on-premises. Gebouwd op de dezelfde open-source-hoofdmappen als Azure Functions en is Azure Functions-Runtime on-premises geïmplementeerd voor een nagenoeg dezelfde ontwikkelervaring als de cloudservice.

![Azure Functions Runtime Preview-Portal][1]

De Azure Functions-Runtime biedt een manier waarop u kunt Azure Functions-ervaring voordat u vastlegt op de cloud. Op deze manier kunnen kunnen de code-elementen die u bouwt vervolgens worden uitgevoerd met u naar de cloud wanneer u migreert.  De runtime wordt ook opent u de nieuwe opties voor u, zoals het gebruik van de ongebruikte compute-kracht van uw on-premises computers batchprocessen 's nachts uitvoeren. U kunt ook apparaten binnen uw organisatie gebruiken voorwaardelijk om gegevens te verzenden met andere systemen, zowel on-premises en in de cloud.

De Azure Functions-Runtime bestaat uit twee onderdelen:

* Azure Functions Runtime Beheerrol
* Azure Functions Runtime Werkrol

## <a name="azure-functions-management-role"></a>Azure Functions-Beheerrol

De Beheerrol van Azure Functions biedt een host voor het beheer van uw functies on-premises. Deze rol worden de volgende taken uitgevoerd:

* Hosting van de Azure Functions-beheerportal, dit hetzelfde account dat u ziet is in de [Azure-portal](https://portal.azure.com). De portal biedt een consistente ervaring waarmee u uw functies ontwikkelen op dezelfde manier als in de Azure-portal.
* Functies over meerdere functies werknemers verdeeld.
* Het leveren van een publicatie-eindpunt zodat u uw functies direct vanuit Microsoft Visual Studio door te downloaden en importeren van het publicatieprofiel kunt publiceren.

## <a name="azure-functions-worker-role"></a>Azure Functions-Werkrol

De werkrollen van de Azure-functies in Windows-Containers zijn geïmplementeerd en zijn waarin uw functiecode aan te geven wordt uitgevoerd.  U kunt meerdere werkrollen implementeren binnen uw organisatie en deze optie is een belangrijke manier waarop klanten kunnen maken gebruik van ongebruikte compute-kracht.  Een voorbeeld van waar spare compute in veel organisaties bestaat is machines voortdurend wordt ingeschakeld, maar niet wordt gebruikt voor grote perioden.

## <a name="minimum-requirements"></a>Minimale vereisten

Om te beginnen met de Azure Functions-Runtime, moet u een machine met Windows Server 2016 of Windows 10 Creators Update met toegang tot een exemplaar van SQL Server hebben.

## <a name="next-steps"></a>Volgende stappen

Installeer de [Azure Functions-Runtime-preview](https://aka.ms/azafrdoc)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
