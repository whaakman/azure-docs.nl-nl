---
title: Toegang beperken via internetgerichte eindpunten in Azure Security Center | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **toegang via een Internetgericht eindpunt beperken**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 487b9d8d02909913f251ae7547d75669fa90f8d9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313297"
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Toegang beperken via internetgerichte eindpunten in Azure Security Center
Azure Security Center wordt aanbevolen dat u de toegang via internetgerichte eindpunten beperken als een van uw Netwerkbeveiligingsgroepen (nsg's) heeft een of meer binnenkomende regels waarmee toegang via 'een' bron-IP-adres. Toegang tot 'een' te openen, mogelijk kwaadwillende personen om toegang te krijgen tot uw resources. Security Center wordt aanbevolen dat u deze regels voor binnenkomende verbindingen om te beperken van toegang tot bron-IP-adressen die toegang nodig hebt.

Deze aanbeveling wordt gegenereerd voor elke niet-web-poort dat 'een' als bron.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie. Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **blade aanbevelingen**, selecteer **toegang via een Internetgericht eindpunt beperken**.

   ![Toegang via een internetgericht eindpunt beperken][1]
2. Hiermee opent u de blade **toegang via een Internetgericht eindpunt beperken**. Deze blade geeft een lijst van de virtuele machines (VM's) met binnenkomende regels die een mogelijk beveiligingsprobleem te maken. Selecteer een virtuele machine.

   ![Selecteer een virtuele machine][2]
3. De **NSG** blade wordt weergegeven, Network Security Group-informatie, verwante binnenkomende regels en de bijbehorende virtuele machine. Selecteer **binnenkomende regels bewerken** om door te gaan met het bewerken van een binnenkomende regel.

   ![Network Security Group-blade][3]
4. Op de **inkomende beveiligingsregels** blade selecteert u de binnenkomende regel bewerken. In dit voorbeeld gaan we selecteren **AllowWeb**.

   ![Inkomende beveiligingsregels][4]

   Opmerking: u kunt ook selecteren **standaardregels** om te zien van de set met standaardregels die door alle nsg's zijn opgenomen. De standaardregels kunnen niet worden verwijderd, maar omdat ze een lagere prioriteit worden toegewezen, kunnen ze worden overschreven door de regels die u maakt. Meer informatie over [standaardregels](../virtual-network/security-overview.md#default-security-rules).

   ![Standaardregels][5]
5. Op de **AllowWeb** blade, bewerk de eigenschappen van de binnenkomende regel zodat de **bron** is een IP-adres of een blok IP-adressen. Zie voor meer informatie over de eigenschappen van de binnenkomende regel, [NSG-regels](../virtual-network/security-overview.md#security-rules).

   ![Regel voor inkomende bewerken][6]

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center 'Toegang beperken via internetgerichte eindpunt'. Zie de volgende onderwerpen voor meer informatie over het inschakelen van Netwerkbeveiligingsgroepen en regels:

* [Wat is een netwerkbeveiligingsgroep (NSG)?](../virtual-network/security-overview.md)
* [Een netwerkbeveiligingsgroep beheren](../virtual-network/manage-network-security-group.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-azure-policy.md): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md): leer hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
