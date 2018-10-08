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
ms.openlocfilehash: d0a20de8738e8c7b2719a9de85d5fd16aa5778cf
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829679"
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
2. Navigeer in het menu Hub op **een resource maken > netwerken > DNS-zone** openen de **DNS-zone maken** blade.

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

In de Azure-portal, gaat u naar **meer services** > **netwerken** > **DNS-zones**. Elke DNS-zone is een eigen resource en informatie zoals het aantal recordsets en naamservers kunnen worden bekeken in deze weergave. De kolom **NAAMSERVERS** bevindt zich niet in de standaardweergave. Als u wilt toevoegen, klikt u op **kolommen**, selecteer **naamservers**, en klik vervolgens op **gedaan**.

![DNS-zones weergeven](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Een DNS-zone verwijderen

Navigeer naar een DNS-zone in de portal. Op de **DNS-zone** blade, klikt u op **zone verwijderen**. U wordt vervolgens gevraagd om te bevestigen uw wilt verwijderen van de DNS-zone. Als u een DNS-zone verwijdert, verwijdert u ook alle records die zijn opgenomen in de zone.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het werken met uw DNS-Zone en -records, recentst [aan de slag met Azure DNS met behulp van de Azure-portal](dns-getstarted-portal.md).