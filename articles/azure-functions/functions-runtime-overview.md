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
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: cb98d5f2aaa526555820c15ba5a32fb7e78ffc5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
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

* Hosting van de Azure Functions-beheerportal, dit is de hetzelfde account dat u ziet in de [Azure-portal](https://portal.azure.com). Hiermee kunt u het ontwikkelen van uw functies op dezelfde manier als in de Azure-portal.
* Functies over meerdere functies werknemers verdeeld.
* Levert een publishing eindpunt zodat u uw functies direct vanuit Microsoft Visual Studio kunt publiceren.

## <a name="azure-functions-worker-role"></a>Azure Functions-Werkrol

De Azure Functions-werkrollen zijn geïmplementeerd in Windows-Containers en dit is waar uw functiecode wordt uitgevoerd.  U kunt meerdere werkrollen implementeren binnen uw organisatie en is een belangrijke manier waarin klanten kunnen maken gebruik van ongebruikte rekenkracht.

## <a name="minimum-requirements"></a>Minimale vereisten

Aan de slag met de Azure Functions-Runtime moet u een machine met hebben **Windows Server 2016 of Windows 10 auteurs Update** met toegang tot een **SQL Server** exemplaar.

## <a name="next-steps"></a>Volgende stappen

Installeer de [Azure Functions-Runtime-preview](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
