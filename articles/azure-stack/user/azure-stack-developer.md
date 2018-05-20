---
title: Apps ontwikkelen voor Azure Stack | Microsoft Docs
description: Ontwikkeling overwegingen voor het maken van een prototype-toepassingen op Azure-Stack
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a1b5a90ca40ce2b19186220344b22ec0ae77e34b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="develop-for-azure-stack"></a>Ontwikkelen voor Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt aan de slag ontwikkelen van toepassingen vandaag, zelfs als u geen toegang tot een Azure-Stack-omgeving hebt. Omdat Azure Stack Microsoft Azure-services die worden uitgevoerd in uw datacenter biedt, kunt u vergelijkbare hulpprogramma's en processen voor het ontwikkelen met Azure-Stack zoals u zou met Azure doen. Met voorbereiding, en de richtlijnen in de volgende onderwerpen, kunt u Azure een Azure-Stack-omgeving emuleren.

* In Azure, kunt u Azure Resource Manager-sjablonen die geïmplementeerd op Azure-Stack zijn. Zie [sjabloon overwegingen](azure-stack-develop-templates.md) voor hulp bij het ontwikkelen van sjablonen om ervoor te zorgen draagbaarheid.
* Er zijn verschillen in de beschikbaarheid van de service en serviceversiebeheer tussen Azure en Azure-Stack. U kunt de [Azure Stack-beleidsmodule](azure-stack-policy-module.md) Azure beschikbaarheid en resource servicetypen beperken tot het wat er beschikbaar is in Azure-Stack. Beperkingen van services zorgt ervoor dat uw toepassingen afhankelijk van de services beschikbaar zijn voor Azure-Stack zijn.
* De [Azure Stack-Snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates) zijn veelvoorkomende scenario voorbeelden die laten zien hoe u sjablonen die kunnen worden geïmplementeerd op Azure en Azure Stack ontwikkelen.
