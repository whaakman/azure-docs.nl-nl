---
title: Inschakelen van Netwerkbeveiligingsgroepen in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** inschakelen netwerk beveiliging groepen **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 1e034d59d8847f237fa0d4c772344d45cd618576
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Inschakelen van Netwerkbeveiligingsgroepen in Azure Security Center
Azure Security Center raadt aan dat u een netwerkbeveiligingsgroep (NSG) inschakelen als een nog niet is ingeschakeld. Nsg's bevatten een lijst met regels voor lijst ACL (Access Control) toestaan of weigeren van netwerkverkeer naar uw VM-exemplaren in een virtueel netwerk. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet. Bovendien het verkeer naar een afzonderlijke virtuele machine kan worden beperkt door een NSG rechtstreeks aan die VM koppelen verdere. Voor meer informatie over [wat is er een Netwerkbeveiligingsgroep (NSG)?](../virtual-network/virtual-networks-nsg.md)

Als u geen nsg's is ingeschakeld, Security Center twee aanbevelingen worden aan u gepresenteerd: schakelen van Netwerkbeveiligingsgroepen op subnetten en inschakelen van Netwerkbeveiligingsgroepen op virtuele machines. U kiezen welke niveau, subnet of virtuele machine toe te passen nsg's.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **Netwerkbeveiligingsgroepen inschakelen** op subnetten of op virtuele machines.
   ![Netwerkbeveiligingsgroepen inschakelen][1]
2. Hiermee opent u de blade **ontbrekende Netwerkbeveiligingsgroepen configureren** voor subnetten of voor virtuele machines, afhankelijk van de aanbeveling die u hebt geselecteerd. Selecteer een subnet of een virtuele machine voor het configureren van een NSG op.

   ![NSG voor subnet configureren][2]

   ![NSG voor de virtuele machine configureren][3]
3. Op de **netwerkbeveiligingsgroep kiezen** blade, selecteer een bestaande NSG of **nieuw** een NSG maken.

   ![Netwerkbeveiligingsgroep kiezen][4]

Als u een NSG maakt, volg de stappen in [het nsg's met de Azure portal beheren](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) maken van een NSG en beveiligingsregels voor verbindingen.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd u hoe de aanbeveling Security Center 'Netwerkbeveiligingsgroepen inschakelen' voor subnetten of virtuele machines te implementeren. Zie de volgende onderwerpen voor meer informatie over het nsg's inschakelen:

* [Wat is een netwerkbeveiligingsgroep (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Het nsg's met de Azure portal beheren](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees het laatste nieuws van de Azure-beveiliging en de informatie.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
