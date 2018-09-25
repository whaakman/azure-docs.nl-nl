---
title: DNS-zones in Azure DNS - Azure-portal beheren | Microsoft Docs
description: U kunt DNS-zones met behulp van de Azure-portal beheren. In dit artikel wordt beschreven hoe u bijwerken, verwijderen en het maken van DNS-zones in Azure DNS
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: victorh
ms.openlocfilehash: a9325dff84635955600bc78687ec0156495ae893
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954378"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Over het beheren van DNS-Zones in Azure portal

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [CLI van Azure classic](dns-operations-dnszones-cli-nodejs.md)
> * [Azure-CLI](dns-operations-dnszones-cli.md)

Dit artikel ziet u hoe u uw DNS-zones beheren met behulp van de Azure-portal. U kunt ook beheren met uw DNS-zones met behulp van de platformoverschrijdende [Azure CLI](dns-operations-dnszones-cli.md) of de Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

1. Aanmelden bij Azure Portal
2. Klik in het menu Hub en klikt u op **een resource maken > netwerken >** en klik vervolgens op **DNS-zone** om het maken van DNS-zone-blade te openen.

    ![DNS-zone](./media/dns-operations-dnszones-portal/openzone650.png)

4. Voer op de blade **DNS-zone maken** de volgende waarden in en klik op **Maken**:


   | **Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|contoso.com|De naam van de DNS-zone|
   |**Abonnement**|[Uw abonnement]|Selecteer een abonnement waarin de DNS-zone moet worden gemaakt.|
   |**Resourcegroep**|**Nieuwe maken:** contosoDNSRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|US - west||

> [!NOTE]
> De resourcegroep verwijst naar de locatie van de resourcegroep en heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

## <a name="list-dns-zones"></a>DNS-zones vermelden

In de Azure-portal, gaat u naar **meer services** > **netwerken** > **DNS-zones**. Elke DNS-zone is het eigen resource, informatie zoals het aantal recordsets en naamservers kunnen worden bekeken in deze weergave. De kolom **NAAMSERVERS** bevindt zich niet in de standaardweergave, toe te voegen klikt u op **kolommen**, selecteer **naamservers** en klikt u op **gedaan**.

![DNS-zones weergeven](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Een DNS-zone verwijderen

Navigeer naar een DNS-zone in de portal. Op de **DNS-zone** blade, klikt u op **zone verwijderen**. U wordt gevraagd om te bevestigen uw wilt verwijderen van de DNS-zone. Als u een DNS-zone verwijdert, verwijdert u ook alle records die zijn opgenomen in de zone.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het werken met uw DNS-Zone en -records, recentst [aan de slag met Azure DNS met behulp van de Azure-portal](dns-getstarted-portal.md).