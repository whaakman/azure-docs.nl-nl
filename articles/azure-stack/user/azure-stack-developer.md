---
title: Apps ontwikkelen voor Azure Stack | Microsoft Docs
description: Meer informatie over overwegingen voor ontwikkeling in maken van een prototype-toepassingen op Azure-Stack
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 4b9e0f5a264296e59579539dd929f5d75a54b62a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="develop-for-azure-stack"></a>Ontwikkelen voor Azure Stack
U kunt aan de slag ontwikkelen van toepassingen vandaag, zelfs als u geen toegang tot een Azure-Stack-omgeving hebt. Omdat Azure Stack Microsoft Azure-services die worden uitgevoerd in uw datacenter biedt, kunt u vergelijkbare hulpprogramma's en processen voor het ontwikkelen met Azure-Stack zoals u zou met Azure doen.  U kunt Azure gebruiken een Azure-Stack-omgeving emuleren met een stukje voorbereidings- en richtlijnen van de volgende onderwerpen:

* In Azure, kunt u Azure Resource Manager-sjablonen die ook worden geïmplementeerd op Azure-Stack.  Zie [sjabloon overwegingen](azure-stack-develop-templates.md) voor hulp bij het ontwikkelen van uw sjablonen om ervoor te zorgen draagbaarheid.
* Er is een delta in de beschikbaarheid van de service en serviceversiebeheer tussen Azure en Azure-Stack. U kunt de [Azure Stack-beleidsmodule](azure-stack-policy-module.md) Azure beschikbaarheid en resource servicetypen beperken tot het wat er beschikbaar is in Azure-Stack. Beperkingen van de beschikbare services kunt uw services beschikbaar zijn voor Azure-Stack afhankelijk zijn van toepassing.
* De [Azure Stack-Snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates) zijn voorbeelden van algemene scenario over het ontwikkelen van uw sjablonen zodat ze kunnen worden geïmplementeerd naar de Azure- en Azure-Stack.


