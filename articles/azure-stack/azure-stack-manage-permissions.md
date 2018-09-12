---
title: Beheren van machtigingen voor resources per gebruiker in Azure Stack (service-beheerder en tenant) | Microsoft Docs
description: Informatie over het beheren van RBAC-machtigingen als een servicebeheerder of tenant.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: ab61e1f892f46ad36df741b7a72afcfcbaa0ed87
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44376932"
---
# <a name="manage-role-based-access-control"></a>Toegangsbeheer op basis van rollen beheren

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Een gebruiker in Azure Stack is een lezer, de eigenaar of Inzender voor elk exemplaar van een abonnement, resourcegroep of -service. Gebruiker A kan bijvoorbeeld reader-machtigingen aan één abonnement hebt, maar eigenaarsmachtigingen hebben voor de virtuele Machine zeven.

 - Lezer: Gebruiker kan alles weergeven, maar geen wijzigingen kunt aanbrengen.
 - Inzender: Gebruiker kan alles beheren behalve toegang tot bronnen.
 - Eigenaar: Gebruiker kan alles beheren, inclusief toegang tot bronnen.

## <a name="set-access-permissions-for-a-user"></a>Stel toegangsmachtigingen voor een gebruiker

1. Meld u aan met een account met eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Klik op de blade voor de resource op de **toegang** pictogram ![](media/azure-stack-manage-permissions/image1.png).
3. In de **gebruikers** blade, klikt u op **rollen**.
4. In de **rollen** blade, klikt u op **toevoegen** machtigingen voor de gebruiker toevoegen.

## <a name="set-access-permissions-for-a-universal-group"></a>Stel toegangsmachtigingen voor een universele groep 

> [!Note]  
Alleen van toepassing op Active Directory Federated Services (AD FS).

1. Meld u aan met een account met eigenaarsmachtigingen voor de resource die u wilt beheren.
2. Klik op de blade voor de resource op de **toegang** pictogram ![](media/azure-stack-manage-permissions/image1.png).
3. In de **gebruikers** blade, klikt u op **rollen**.
4. In de **rollen** blade, klikt u op **toevoegen** machtigingen voor de universele groep Active Directory-groep toevoegen.

## <a name="next-steps"></a>Volgende stappen
[Een Azure Stack-tenant toevoegen](azure-stack-add-new-user-aad.md)

