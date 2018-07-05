---
title: Aangepaste rollen voor Privileged Identity Management-doelinstellingen gebruiken voor Azure-resources | Microsoft Docs
description: Beschrijft hoe u aangepaste rollen voor Azure-resources met PIM gebruiken.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: f086d8038e6d27990c49749438ee05e3e39a5aec
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442906"
---
# <a name="use-custom-roles-to-target-privileged-identity-management-settings"></a>Aangepaste rollen voor Privileged Identity Management-doelinstellingen gebruiken

U moet mogelijk strikte Privileged Identity Management (PIM)-instellingen toepassen op sommige leden van een rol, terwijl er meer autonomie voor anderen. U hebt een scenario waarin huurt uw organisatie verschillende contract wordt om te helpen bij de ontwikkeling van een toepassing die wordt uitgevoerd in een Azure-abonnement.

Als de resourcebeheerder van een wilt u werknemers komen in aanmerking om toegang te krijgen zonder goedkeuring. Alle contract wordt moeten echter worden goedgekeurd bij het aanvragen van toegang tot resources van de organisatie.

Volg de stappen in de volgende sectie voor het instellen van gerichte PIM-instellingen voor Azure resource-rollen.

## <a name="create-the-custom-role"></a>De aangepaste rol maken

Volg de stappen in voor het maken van een aangepaste rol voor een resource, [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../role-based-access-control-custom-roles.md).

Wanneer u de aangepaste rol maakt, bevatten een beschrijvende naam, zodat u gemakkelijk welke ingebouwde rol die u wilde onthouden kunt te dupliceren.

> [!NOTE]
> Zorg ervoor dat de aangepaste rol is een duplicaat van de ingebouwde rol die u wilt dupliceren, en dat het bereik overeenkomt met de ingebouwde rol.

## <a name="apply-pim-settings"></a>PIM-instellingen toepassen

Nadat de rol is gemaakt in uw tenant, in de Azure-portal, gaat u naar de **Privileged Identity Management - Azure-resources** deelvenster. Selecteer de resource die de rol is van toepassing op.

![De 'Privileged Identity Management - Azure-resources' deelvenster](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[Configureren van instellingen voor de sitesysteemrol PIM](pim-resource-roles-configure-role-settings.md) die gelden voor deze leden van de rol.

Ten slotte [rollen toewijzen](pim-resource-roles-assign-roles.md) aan afzonderlijke leden die u wilt targeten met deze instellingen.

## <a name="next-steps"></a>Volgende stappen

[Abonnementseigenaren lees- en toegangsbeheer](pim-resource-roles-perform-access-review.md)
