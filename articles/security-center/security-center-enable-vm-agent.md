---
title: VM-Agent in Azure Security Center inschakelen | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** inschakelen VM Agent **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 337a7adfd93c76882a749685702bea6d1524c96a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a>VM-Agent in Azure Security Center inschakelen
De VM-Agent moet worden geïnstalleerd op virtuele machines (VM's) om [gegevensverzameling inschakelen](security-center-enable-data-collection.md).  Azure Security Center kunt u zien welke VM's vereisen de VM-Agent en beveelt aan dat u de VM-Agent op deze virtuele machines inschakelt.

De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie. Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **blade aanbevelingen**, selecteer **VM-Agent inschakelen**.
   ![VM-agent inschakelen][1]
2. Hiermee opent u de blade **VM-Agent ontbreekt of niet reageert**. Deze blade bevat de virtuele machines waarvoor de VM-Agent. Volg de instructies op de blade voor het installeren van de VM-agent.
   ![VM-Agent ontbreekt.][2]

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): leer hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
