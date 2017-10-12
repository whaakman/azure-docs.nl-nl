---
title: Een internetgerichte load balancer maken - klassieke Azure Portal| Microsoft Docs
description: Meer informatie over het maken van een internetgerichte load balancer in het klassieke implementatiemodel via de klassieke Azure Portal
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e07b6808f2401ac7b2b21e5f8816bac5a15b50b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Aan de slag met het maken van een internetgerichte load balancer (klassiek) via de klassieke Azure Portal

> [!div class="op_single_selector"]
> * [Klassieke Azure Portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Voordat u met Azure-resources gaat werken, is het belangrijk om te weten dat Azure momenteel twee implementatiemodellen heeft: Azure Resource Manager en het klassieke model. Zorg ervoor dat u begrijpt wat [implementatiemodellen en hulpprogramma's](../azure-classic-rm.md) zijn voordat u met een Azure-resource gaat werken. U kunt de documentatie voor verschillende hulpprogramma's bekijken door op de tabbladen boven aan dit artikel te klikken. Dit artikel is van toepassing op het klassieke implementatiemodel. Hier vindt u [meer informatie over hoe u een internetgerichte load balancer maakt met Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Een internetgerichte load balancer voor virtuele machines instellen

Als u netwerkverkeer van het internet wilt spreiden over de virtuele machines van een cloudservice, moet u een set met gelijke taakverdeling maken. In deze procedure wordt ervan uitgegaan dat u de virtuele machines al hebt gemaakt en dat ze zich allemaal binnen dezelfde cloudservice bevinden.

**Een set met gelijke taakverdeling voor virtuele machines configureren**

1. Klik in de klassieke Azure Portal op **Virtuele machines** en klik vervolgens op de naam van een virtuele machine in de set met gelijke taakverdeling.
2. Klik op **Eindpunten** en vervolgens op **Toevoegen**.
3. Klik op de pijl-rechts op de pagina **Een eindpunt toevoegen aan een virtuele machine**.
4. Doe het volgende op de pagina **De details van het eindpunt opgeven**:

   * Typ bij **Naam** een naam voor het eindpunt of selecteer de naam in de lijst met vooraf gedefinieerde eindpunten voor algemene protocollen.
   * Selecteer indien nodig bij **Protocol** het vereiste protocol voor het type eindpunt. Dit is TCP of UDP.
   * Typ bij **Openbare poort en particuliere poort** de poortnummers die de virtuele machine moet gebruiken (indien nodig). U kunt de particuliere poort en de firewall-regels op de virtuele machine gebruiken om verkeer om te leiden op een manier die geschikt is voor uw toepassing. De particuliere poort en de openbare poort mogen hetzelfde zijn. Voor een eindpunt voor webverkeer (HTTP) kunt u bijvoorbeeld poort 80 toewijzen aan de openbare en de particuliere poort.

5. Selecteer **Een set met gelijke taakverdeling maken** en klik vervolgens op de pijl-rechts.
6. Typ op de pagina **De set met gelijke taakverdeling configureren** een naam voor de set met gelijke taakverdeling. Wijs vervolgens de waarden toe voor het testgedrag van de Azure Load Balancer. De load balancer maakt gebruik van tests om te bepalen of de virtuele machines in de set met gelijke taakverdeling inkomend verkeer kunnen ontvangen.
7. Klik op het vinkje om het eindpunt met gelijke taakverdeling te maken. **Ja** wordt weergegeven in de kolom **Naam van set met gelijke taakverdeling** op de pagina **Eindpunten** voor de virtuele machine.
8. Klik in de portal op **Virtuele machines** en klik daarna op de naam van een extra virtuele machine in de set met gelijke taakverdeling. Klik vervolgens op **Eindpunten** en dan op **Toevoegen**.
9. Klik op de pagina **Een eindpunt toevoegen aan een virtuele machine** op **Eindpunt toevoegen aan een bestaande set met gelijke taakverdeling**, selecteer de naam van de set met gelijke taakverdeling en klik op de pijl-rechts.
10. Typ op de pagina **De details van het eindpunt opgeven** een naam voor het eindpunt en klik op het vinkje.

Herhaal stap 8-10 voor de extra virtuele machines in de set met gelijke taakverdeling.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
