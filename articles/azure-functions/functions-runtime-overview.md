---
title: Runtime-overzicht van Azure Functions | Microsoft Docs
description: Overzicht van de Azure Functions-Runtime-Preview
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 557f071e2cd8d4f639c881274e6e74a8fb745859
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-functions-runtime-overview"></a>Overzicht van Azure Functions-Runtime

De Azure Functions-Runtime biedt een nieuwe manier om te profiteren van de eenvoud en flexibiliteit van de Azure Functions programming model lokale. Gebouwd op de dezelfde open-source hoofdmappen als Azure Functions, is Azure Functions-Runtime geïmplementeerde on-premises naar bieden een bijna identiek ontwikkeling biedt als de cloudservice.

![Preview-Portal voor Azure Functions-Runtime][1]

De Azure Functions-Runtime biedt een manier om de ervaring van Azure Functions alvorens toe te wijzen aan de cloud. Op deze manier kunnen kunnen de code-elementen die u bouwt vervolgens worden uitgevoerd met u naar de cloud wanneer u migreert.  De runtime ook wordt geopend nieuwe opties voor u, zoals het gebruik van de ongebruikte rekencapaciteit van uw lokale computers batchprocessen 's nachts uitvoeren. U kunt ook apparaten binnen uw organisatie gebruiken voorwaardelijk om gegevens te verzenden met andere systemen, zowel on-premises en in de cloud.

De Azure Functions-Runtime bestaat uit twee onderdelen:

* Beheer van Azure Functions-Runtimerol
* Azure Functions-Runtime-Werkrol

## <a name="azure-functions-management-role"></a>Azure Functions Beheerrol

De Azure Functions rol biedt een host voor het beheer van uw functies on-premises. Deze rol voert de volgende taken:

* Hosting van de Azure Functions-beheerportal, die hetzelfde account dat u ziet in de [Azure-portal](https://portal.azure.com). De portal biedt een consistente ervaring waarmee u uw functies op dezelfde manier ontwikkelen, zoals u zou in de Azure portal doen.
* Functies over meerdere functies werknemers verdeeld.
* Levert een publishing eindpunt zodat u kunt uw functies direct vanuit Microsoft Visual Studio door te downloaden en importeren van het publicatieprofiel publiceren.

## <a name="azure-functions-worker-role"></a>Azure Functions-Werkrol

De Azure Functions-werkrollen in de Windows-Containers zijn geïmplementeerd en zijn waarop uw functiecode wordt uitgevoerd.  U kunt meerdere werkrollen implementeren binnen uw organisatie en deze optie is een belangrijke manier waarin klanten kunnen maken gebruik van ongebruikte rekenkracht.  Een voorbeeld van waar spare compute in veel organisaties bestaat is machines voortdurend wordt ingeschakeld, maar niet wordt gebruikt voor grote perioden.

## <a name="minimum-requirements"></a>Minimale vereisten

U moet een machine met Windows Server 2016 of Windows 10 auteurs Update met toegang tot een exemplaar van SQL Server hebben om te beginnen met de Azure Functions-Runtime.

## <a name="next-steps"></a>Volgende stappen

Installeer de [Azure Functions-Runtime-preview](https://aka.ms/azafrdoc)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
