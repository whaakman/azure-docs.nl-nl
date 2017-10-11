---
title: DNS-zones in Azure DNS - Azure-portal beheren | Microsoft Docs
description: U kunt DNS-zones met de Azure portal beheren. Dit artikel wordt beschreven hoe u bijwerken, verwijderen en DNS-zones maken op Azure DNS
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 69a509612e6204fc93dd42bf2fe69cb165b5777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>DNS-Zones beheren in de Azure portal

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

In dit artikel laat zien hoe uw DNS-zones beheren met behulp van de Azure-portal. U kunt ook de DNS-zones met behulp van de platformoverschrijdende beheren [Azure CLI](dns-operations-dnszones-cli.md) of de Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

1. Aanmelden bij Azure Portal
2. Klik in het menu Hub en klik op **Nieuw > Netwerken >** en klik vervolgens op **DNS-zone** om de blade DNS-zone maken te openen.

    ![DNS-zone](./media/dns-operations-dnszones-portal/openzone650.png)

4. Voer op de blade **DNS-zone maken** de volgende waarden in en klik op **Maken**:


   | **Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|contoso.com|De naam van de DNS-zone|
   |**Abonnement**|[Uw abonnement]|Selecteer een abonnement waarin de DNS-zone moet worden gemaakt.|
   |**Resourcegroep**|**Nieuwe maken:** contosoDNSRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|VS - west||

> [!NOTE]
> De resourcegroep verwijst naar de locatie van de resourcegroep en heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

## <a name="list-dns-zones"></a>Lijst met DNS-zones

Navigeer in de Azure-portal naar **meer services** > **Networking** > **DNS-zones**. Elke DNS-zone, is het eigen resource, zoals het aantal recordsets en naamservers kunnen worden bekeken in deze weergave. De kolom **NAAMSERVERS** bevindt zich niet in de standaardweergave toe te voegen klikt u op **kolommen**, selecteer **naamservers** en klik op **gedaan**.

![aanbieding DNS-zones](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Een DNS-zone verwijderen

Navigeer naar een DNS-zone in de portal. Op de **DNS-zone** blade, klikt u op **zone verwijderen**. U wordt gevraagd om te bevestigen dat u willen verwijderen van de DNS-zone. Een DNS-zone verwijderen, verwijdert tevens alle records die zijn opgenomen in de zone.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het werken met uw DNS-Zone en records in via [aan de slag met Azure DNS met Azure portal](dns-getstarted-portal.md).