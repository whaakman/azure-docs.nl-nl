---
title: Aangepaste rollen voor Privileged Identity Management-doelinstellingen gebruiken voor Azure-resources | Microsoft Docs
description: Beschrijft hoe u aangepaste rollen voor Azure-resources met PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: fb40c59fde17c742bb9e2bbe5f5f598382248eab
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699349"
---
# <a name="use-custom-roles-to-target-privileged-identity-management-settings"></a>Aangepaste rollen voor Privileged Identity Management-doelinstellingen gebruiken

Mogelijk moet u strikte Privileged Identity Management (PIM)-instellingen toepassen op een aantal leden van een rol, terwijl er meer autonomie voor anderen. Neem bijvoorbeeld een scenario waarin huurt van uw organisatie verschillende contract gekoppeld om te helpen bij het ontwikkelen van een toepassing die wordt uitgevoerd in een Azure-abonnement.

Als de resourcebeheerder van een wilt u werknemers in aanmerking komen voor toegang zonder goedkeuring. Alle contract gekoppeld moeten echter worden goedgekeurd bij het aanvragen van toegang tot bronnen van de organisatie.

Volg de stappen die worden beschreven in de volgende sectie voor het instellen van gerichte PIM-instellingen voor Azure-resourcerollen.

## <a name="create-the-custom-role"></a>De aangepaste rol maken

Volg de stappen in voor het maken van een aangepaste rol voor een resource [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../role-based-access-control-custom-roles.md).

Wanneer u aangepaste rol maakt, bevatten een beschrijvende naam zodat u gemakkelijk te onthouden welke ingebouwde rol die u wilde dupliceren.

> [!NOTE]
> Zorg ervoor dat de aangepaste rol is een duplicaat van de ingebouwde rol die u wilt dupliceren en de ingebouwde functie komt overeen met het bereik.

## <a name="apply-pim-settings"></a>PIM-instellingen toepassen

Nadat u de rol is gemaakt in uw tenant, in de Azure portal, gaat u naar de **Privileged Identity Management - Azure-resources** deelvenster. Selecteer de resource die de rol van toepassing op.

![De 'Privileged Identity Management - Azure-resources' deelvenster](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[PIM-rollen configureren](pim-resource-roles-configure-role-settings.md) die worden toegepast op leden van de rol.

Ten slotte [rollen toewijzen](pim-resource-roles-assign-roles.md) tot de afzonderlijke groep leden die u met deze instellingen wilt richten.

## <a name="next-steps"></a>Volgende stappen

[Bekijk abonnementseigenaren en -toegang](pim-resource-roles-perform-access-review.md)
