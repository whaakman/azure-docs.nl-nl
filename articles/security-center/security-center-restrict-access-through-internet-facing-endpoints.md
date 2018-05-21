---
title: Beperken van toegang via internetgerichte eindpunten in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe u de aanbeveling Azure Security Center implementeert **beperken van toegang via Internetgericht eindpunt**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 92906d31f4db21f37094f192dadd080e28cc6e8e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Beperken van toegang via internetgerichte eindpunten in Azure Security Center
Azure Security Center wordt aanbevolen dat u de toegang via internetgerichte eindpunten beperken als een van uw Netwerkbeveiligingsgroepen (nsg's) heeft een of meer regels voor binnenkomende verbindingen die toegankelijk is vanaf 'alle' IP-bronadressen. Toegang tot 'een' te openen, is het mogelijk maken aanvallers voor toegang tot uw resources. Security Center beveelt aan dat u deze regels voor binnenkomende verbindingen om toegang te beperken in bron-IP-adressen die daadwerkelijk toegang nodig bewerken.

Deze aanbeveling is voor elke niet--poort dat 'een' als bron gegenereerd.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie. Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **blade aanbevelingen**, selecteer **beperken van toegang via Internetgericht eindpunt**.

   ![Toegang via een internetgericht eindpunt beperken][1]
2. Hiermee opent u de blade **beperken van toegang via Internetgericht eindpunt**. Deze blade bevat de virtuele machines (VM's) met regels voor binnenkomende verbindingen die een mogelijk beveiligingsprobleem te maken. Selecteer een virtuele machine.

   ![Selecteer een virtuele machine][2]
3. De **NSG** blade geeft Netwerkbeveiligingsgroep informatie, gerelateerde regels voor binnenkomende verbindingen, en de bijbehorende virtuele machine. Selecteer **binnenkomende regels bewerken** om door te gaan met het bewerken van een inkomende regel.

   ![Netwerkbeveiligingsgroep-blade][3]
4. Op de **inkomende beveiligingsregels** blade selecteert u de inkomende regel om te bewerken. In dit voorbeeld gaan we selecteren **AllowWeb**.

   ![Inkomende beveiligingsregels][4]

   Opmerking: u kunt ook selecteren **regels standaard** om te zien van de set met standaardregels die zijn opgenomen in alle nsg's. De standaardregels kunnen niet worden verwijderd, maar omdat ze een lagere prioriteit worden toegewezen, kunnen ze worden overschreven door de regels die u maakt. Meer informatie over [regels standaard](../virtual-network/security-overview.md#default-security-rules).

   ![Standaardregels][5]
5. Op de **AllowWeb** blade, bewerk de eigenschappen van de binnenkomende regel zodat de **bron** is een IP-adres of een blok IP-adressen. Zie voor meer informatie over de eigenschappen van de binnenkomende regel, [NSG-regels](../virtual-network/security-overview.md#security-rules).

   ![Regel voor binnenkomende bewerken][6]

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u implementeert de aanbeveling Security Center 'Toegang beperken tot en met een internetgericht eindpunt'. Zie de volgende onderwerpen voor meer informatie over het inschakelen van regels en nsg's:

* [Wat is een netwerkbeveiligingsgroep (NSG)?](../virtual-network/security-overview.md)
* [Een netwerkbeveiligingsgroep beheren](../virtual-network/manage-network-security-group.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): leer hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
