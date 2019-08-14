---
title: Netwerk beveiligings groepen inschakelen in Azure Security Center | Microsoft Docs
description: Dit document bevat informatie over het implementeren van de Azure Security Center aanbeveling **netwerk beveiligings groepen inschakelen**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f53ed853-ffaf-4530-a019-1906ba6f341b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 14b7cc8f8162574380ca21ac515af8b7d3d5ded9
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "60911415"
---
# <a name="enable-network-security-groups-in-azure-security-center"></a>Netwerk beveiligings groepen inschakelen in Azure Security Center
Azure Security Center wordt aangeraden een netwerk beveiligings groep (NSG) in te scha kelen als deze nog niet is ingeschakeld. Nsg's bevatten een lijst met regels voor Access Control lijst (ACL) waarmee netwerk verkeer naar uw VM-exemplaren in een Virtual Network wordt toegestaan of geweigerd. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet. Daarnaast kan het verkeer naar een afzonderlijke virtuele machine verder worden beperkt door een NSG rechtstreeks aan die VM te koppelen. Zie [Wat is een netwerk beveiligings groep (NSG)?](../virtual-network/security-overview.md) voor meer informatie.

Als Nsg's niet is ingeschakeld, biedt Security Center twee aanbevelingen voor u: Netwerk beveiligings groepen inschakelen op subnetten en netwerk beveiligings groepen op virtuele machines inschakelen. U kiest welk niveau, subnet of virtuele machine u wilt gebruiken om Nsg's toe te passen.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. Op de Blade **aanbevelingen** selecteert u **netwerk beveiligings groepen inschakelen** op subnetten of op virtuele machines.
   ![Netwerkbeveiligingsgroepen inschakelen][1]
2. Hiermee opent u de Blade voor het **configureren van ontbrekende netwerk beveiligings groepen** voor subnetten of voor virtuele machines, afhankelijk van de aanbeveling die u hebt geselecteerd. Selecteer een subnet of een virtuele machine voor het configureren van een NSG op.

   ![NSG voor subnet configureren][2]

   ![NSG voor VM configureren][3]
3. Selecteer op de Blade **netwerk beveiligings groep kiezen** een bestaande NSG of selecteer **nieuwe maken** om een NSG te maken.

   ![Netwerk beveiligings groep kiezen][4]

Als u een NSG maakt, volgt u de stappen in [een netwerk beveiligings groep beheren](../virtual-network/manage-network-security-group.md) om een NSG te maken en beveiligings regels in te stellen.

## <a name="see-also"></a>Zie ook
In dit artikel wordt uitgelegd hoe u de Security Center aanbeveling ' netwerk beveiligings groepen inschakelen ' voor subnetten of virtuele machines kunt implementeren. Ga voor meer informatie over het inschakelen van Nsg's naar het volgende:

* [Wat is een netwerkbeveiligingsgroep (NSG)?](../virtual-network/security-overview.md)
* [Een netwerk beveiligings groep beheren](../virtual-network/manage-network-security-group.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in azure Security Center](security-center-recommendations.md) : Ontdek hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligings status controleren in azure Security Center](security-center-monitoring.md) --meer informatie over het controleren van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligings blog](https://blogs.msdn.com/b/azuresecurity/) : krijg het meest recente Azure-beveiligings nieuws en-informatie.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
