---
title: Privileged Identity Management voor Resources in Azure - gebruik van aangepaste rollen naar doel PIM-instellingen | Microsoft Docs
description: Beschrijft hoe u aangepaste rollen in PIM voor Azure-resources.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 9ac14125b837f45634870941e72ea63e9921ca70
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="use-custom-roles-to-target-pim-settings"></a>Gebruik aangepaste rollen naar doel PIM-instellingen

Mogelijk strenge PIM-instellingen toepassen op een aantal leden van een rol, terwijl er meer autonomie voor anderen. Neem bijvoorbeeld een scenario waarbij uw organisatie huurt sevral contract gekoppeld om te helpen bij het ontwikkelen van een toepassing die wordt uitgevoerd in een Azure-abonnement. 

Als de resourcebeheerder van een, u wilt dat werknemers van uw organisatie in aanmerking komende toegang zonder goedkeuring nodig hebben, maar alle contract gekoppeld goedkeuring vragen bij het aanvragen van activering. Volg de stappen hieronder overzicht het proces voor het beoogde PIM-instellingen voor Azure-resource-functies inschakelen.

## <a name="create-the-custom-role"></a>De aangepaste rol maken

[Gebruik deze handleiding voor het maken van een aangepaste rol voor een resource](../role-based-access-control-custom-roles.md).

Een beschrijvende naam bevatten zodat u gemakkelijk te onthouden welke ingebouwde rol die u wilde dupliceren.

>[!NOTE]
>Zorg ervoor dat de aangepaste rol is een duplicaat van de rol die u hebt aangebracht, en het bereik komt overeen met de ingebouwde rol.

## <a name="apply-pim-settings"></a>PIM-instellingen toepassen

Zodra de rol in uw tenant is gemaakt, gaat u naar PIM en selecteert u de resource die de rol is afgestemd op.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[PIM-rollen configureren](pim-resource-roles-configure-role-settings.md) die worden toegepast op deze leden

Ten slotte [rollen toewijzen](pim-resource-roles-assign-roles.md) aan de afzonderlijke groep van leden met deze instellingen wilt richten.

## <a name="next-steps"></a>Volgende stappen

[Bekijk de eigenaars van abonnement](pim-resource-roles-perform-access-review.md)
