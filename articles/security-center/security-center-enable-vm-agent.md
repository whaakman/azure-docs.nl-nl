---
title: VM-Agent in Azure Security Center inschakelen | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **VM-Agent inschakelen**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 7a059cd5c22306c42adc4c7c8fcc4e52c3ebcd00
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313568"
---
# <a name="enable-vm-agent-in-azure-security-center"></a>VM-Agent in Azure Security Center inschakelen
De VM-Agent moet worden geïnstalleerd op virtuele machines (VM's) om [verzamelen van gegevens inschakelen](security-center-enable-data-collection.md).  Azure Security Center kunt u zien welke VM's vereisen dat de VM-Agent en wordt aanbevolen dat u de VM-Agent op deze virtuele machines inschakelt.

De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie. Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **blade aanbevelingen**, selecteer **VM-Agent inschakelen**.
   ![VM-agent inschakelen][1]
2. Hiermee opent u de blade **VM-Agent ontbreekt of reageert niet**. Deze blade geeft een lijst van de virtuele machines waarvoor de VM-Agent. Volg de instructies op de blade voor het installeren van de VM-agent.
   ![VM-Agent ontbreekt][2]

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-azure-policy.md): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): leer hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
