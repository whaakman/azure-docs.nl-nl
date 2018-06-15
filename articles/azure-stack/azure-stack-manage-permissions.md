---
title: Machtigingen beheren voor bronnen per gebruiker in Azure-Stack (servicebeheerder en tenant) | Microsoft Docs
description: Informatie over het beheren van RBAC machtigingen als een servicebeheerder of tenant.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: fenila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: mabrigg
ms.reviewer: thomas.roettinger
ms.openlocfilehash: 0e50ea44ebb0b0a7285dab04666dd55cad480c6a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
ms.locfileid: "29385633"
---
# <a name="manage-role-based-access-control"></a>Toegangsbeheer op basis van rollen beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een gebruiker in Azure-Stack kan een lezer, de eigenaar of bijdrager voor elk exemplaar van een abonnement, resourcegroep of service zijn. Gebruiker A mogelijk bijvoorbeeld Lezersmachtigingen aan één abonnement hebt, maar de van eigenaarsmachtigingen voor de virtuele Machine zeven hebben.

 - Lezer: Gebruiker kan alles weergeven, maar geen wijzigingen kunt aanbrengen.
 - Inzender: Gebruikers kan alles beheren behalve toegang tot bronnen.
 - Eigenaar: Gebruiker kan alles beheren, inclusief toegang tot bronnen.

## <a name="set-access-permissions-for-a-user"></a>Stel toegangsmachtigingen voor een gebruiker

1. Aanmelden met een account met de van eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Klik in de blade voor de resource op de **toegang** pictogram ![](media/azure-stack-manage-permissions/image1.png).
3. In de **gebruikers** blade, klikt u op **rollen**.
4. In de **rollen** blade, klikt u op **toevoegen** machtigingen voor de gebruiker toevoegen.

## <a name="set-access-permissions-for-a-universal-group"></a>Stel toegangsmachtigingen voor een universele groep 

> [!Note]  
Alleen van toepassing op Active Directory Federated Services (AD FS).

1. Aanmelden met een account met de van eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Klik in de blade voor de resource op de **toegang** pictogram ![](media/azure-stack-manage-permissions/image1.png).
3. In de **gebruikers** blade, klikt u op **rollen**.
4. In de **rollen** blade, klikt u op **toevoegen** machtigingen voor de universele groep Active Directory-groep toevoegen.

## <a name="next-steps"></a>Volgende stappen
[Een Azure Stack-tenant toevoegen](azure-stack-add-new-user-aad.md)

