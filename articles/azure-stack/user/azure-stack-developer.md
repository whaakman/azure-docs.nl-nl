---
title: Apps ontwikkelen voor Azure Stack | Microsoft Docs
description: Overwegingen bij de ontwikkeling van prototypen toepassingen in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 51f3861d53ac5dac80b53fad9a4efe7b276807fe
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065406"
---
# <a name="develop-for-azure-stack"></a>Ontwikkelen voor Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt aan de slag vandaag, het ontwikkelen van toepassingen, zelfs als u geen toegang tot een Azure Stack-omgeving. Omdat Azure Stack Microsoft Azure-services die worden uitgevoerd in uw datacenter biedt, kunt u dezelfde hulpprogramma's en processen te ontwikkelen met betrekking tot Azure Stack, zoals u zou met Azure doen.

## <a name="development-considerations"></a>Overwegingen bij de ontwikkeling

Enige voorbereiding en overeenkomstig de richtlijnen in de volgende onderwerpen, kunt u Azure aan een Azure Stack-omgeving emuleren.

* In Azure, kunt u Azure Resource Manager-sjablonen die geïmplementeerd met Azure Stack zijn. Zie [sjabloon overwegingen met betrekking tot](azure-stack-develop-templates.md) voor hulp bij het ontwikkelen van sjablonen om ervoor te zorgen draagbaarheid.
* Er zijn verschillen in de beschikbaarheid van de service en serviceversiebeheer tussen Azure en Azure Stack. U kunt de [Azure Stack-beleidsmodule](azure-stack-policy-module.md) om te beperken van de beschikbaarheid en resource typen Azure-service tot wat er beschikbaar is in Azure Stack. Beperken van services, zorgt u ervoor dat uw toepassingen zijn afhankelijk van de services beschikbaar zijn voor Azure Stack.
* De [Snelstartsjablonen voor Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates) worden algemene scenario voorbeelden die laten hoe zien het ontwikkelen van sjablonen die kunnen worden geïmplementeerd op Azure en Azure Stack.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het ontwikkelen van Azure Stack:

* [Azure Resource Manager sjabloon aanbevolen procedures](azure-stack-develop-templates.md)
* [Azure Stack-Quickstart-sjablonen op GitHub](https://github.com/Azure/AzureStack-QuickStart-Templates)