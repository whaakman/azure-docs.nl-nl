---
title: Machtigingen beheren voor bronnen per gebruiker in Azure-Stack (servicebeheerder en tenant) | Microsoft Docs
description: Informatie over het beheren van RBAC machtigingen als een servicebeheerder of tenant.
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e558f9de9bc3182bbe20ceb9d8f3f96e47fa542c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-role-based-access-control"></a>Toegangsbeheer op basis van rollen beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een gebruiker in Azure-Stack kan een lezer, de eigenaar of bijdrager voor elk exemplaar van een abonnement, resourcegroep of service zijn. Gebruiker A mogelijk bijvoorbeeld lezer gemachtigd met abonnement 1, maar de eigenaar gemachtigd tot en met 7 van de virtuele Machine.

* Lezer: Gebruiker kan alles weergeven, maar geen wijzigingen kunt aanbrengen.
* Inzender: Gebruikers kan alles beheren behalve toegang tot bronnen.
* Eigenaar: Gebruiker kan alles beheren, inclusief toegang tot bronnen.

## <a name="set-access-permissions-for-a-user"></a>Stel toegangsmachtigingen voor een gebruiker
1. Aanmelden met een account met de van eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Klik in de blade voor de resource op de **toegang** pictogram ![](media/azure-stack-manage-permissions/image1.png).
3. In de **gebruikers** blade, klikt u op **rollen**.
4. In de **rollen** blade, klikt u op **toevoegen** machtigingen voor de gebruiker toevoegen.

## <a name="next-steps"></a>Volgende stappen
[Een Azure Stack-tenant toevoegen](azure-stack-add-new-user-aad.md)

