---
title: Netwerkbeveiligingsgroepen in Azure Security Center inschakelen | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **Netwerkbeveiligingsgroepen inschakelen**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: f70faff478da9840a6c40b2f1360aec2cf3aecda
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235646"
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Netwerkbeveiligingsgroepen in Azure Security Center inschakelen
Azure Security Center raadt aan dat u een netwerkbeveiligingsgroep (NSG) inschakelen als een nog niet is ingeschakeld. Nsg's bevatten een lijst van lijst met ACL (Access Control)-regels toestaan of weigeren van netwerkverkeer naar uw VM-exemplaren in een Virtueelnetwerk. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet. Bovendien verkeer naar een afzonderlijke virtuele machine kan worden beperkt verder door een NSG rechtstreeks naar die virtuele machine koppelen. Voor meer informatie over meer [wat is er een Netwerkbeveiligingsgroep (NSG)?](../virtual-network/security-overview.md)

Als u geen nsg's ingeschakeld, Security Center twee aanbevelingen worden aan u gepresenteerd: Netwerkbeveiligingsgroepen inschakelen op subnetten en Netwerkbeveiligingsgroepen inschakelen op virtuele machines. U kiezen welk niveau, subnet of VM, om toe te passen nsg's.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **Netwerkbeveiligingsgroepen inschakelen** op subnetten of virtuele machines.
   ![Netwerkbeveiligingsgroepen inschakelen][1]
2. Hiermee opent u de blade **ontbrekende Netwerkbeveiligingsgroepen configureren** voor subnetten of virtuele machines, afhankelijk van de aanbeveling die u hebt geselecteerd. Selecteer een subnet of een virtuele machine voor het configureren van een NSG op.

   ![NSG voor subnet configureren][2]

   ![NSG voor virtuele machine configureren][3]
3. Op de **netwerkbeveiligingsgroep kiezen** blade, selecteert u een bestaande NSG of **nieuw** aan een NSG te maken.

   ![Netwerkbeveiligingsgroep kiezen][4]

Als u een NSG te maken, volgt u de stappen in [beheren van een netwerkbeveiligingsgroep](../virtual-network/manage-network-security-group.md) maken een Netwerkbeveiligingsgroep en beveiligingsregels voor verbindingen.

## <a name="see-also"></a>Zie ook
In dit artikel laat zien hoe u de aanbeveling van Security Center 'Netwerkbeveiligingsgroepen inschakelen' voor subnetten of virtuele machines implementeert. Zie de volgende onderwerpen voor meer informatie over het inschakelen van nsg's:

* [Wat is een netwerkbeveiligingsgroep (NSG)?](../virtual-network/security-overview.md)
* [Een netwerkbeveiligingsgroep beheren](../virtual-network/manage-network-security-group.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) --de meest recente Azure-beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
