---
title: Aangepaste rollen gebruiken voor Azure-resources in PIM | Microsoft Docs
description: Informatie over het gebruik van aangepaste rollen voor Azure-resources in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 2dc3729f533b6f4b70cb3d83cc295ca1afcda5f3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197174"
---
# <a name="use-custom-roles-for-azure-resources-in-pim"></a>Gebruik aangepaste rollen voor Azure-resources in PIM

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

- [Azure-resource rolinstellingen in PIM configureren](pim-resource-roles-configure-role-settings.md)
- [Aangepaste rollen in Azure](../../role-based-access-control/custom-roles.md)
