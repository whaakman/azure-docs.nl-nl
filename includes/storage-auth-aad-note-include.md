---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 50217829be7aff3bcdbf417f19fff9f1fd7c6901
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372662"
---
> [!NOTE]
> - RBAC-roltoewijzingen duurt maximaal twee minuten worden doorgegeven.
>
> U moet HTTPS gebruiken voor het autoriseren van blob- en wachtrijservices bewerkingen met een OAuth-token.
>
> - De Azure-portal biedt nu ondersteuning voor Azure AD-referenties gebruiken om te lezen en schrijven van blob en wachtrijgegevens, als onderdeel van de Preview-versie uitbrengen. Toegang tot gegevens voor blob- en wachtrijservices in Azure portal een gebruiker moet worden toegewezen Azure Resource Manager **lezer** rol, naast de juiste rol voor toegang tot blob of een wachtrij. Zie voor meer informatie, [toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal](../articles/storage/common/storage-auth-aad-rbac.md). 
> 
> - [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gebruikt momenteel de sleutel van uw opslagaccount voor het openen van blob- en wachtrijservices. Als de sleutel niet beschikbaar is, wordt Azure AD-verificatie gebruikt voor toegang tot blobs. Autorisatie van Azure AD is momenteel niet ondersteund voor wachtrijen. 
>
> - Azure Files biedt ondersteuning voor verificatie met Azure AD via SMB voor domein-VM's alleen (preview). Zie voor meer informatie over het gebruik van Azure AD voor Azure Files via SMB, [overzicht van Azure Active Directory-verificatie voor Azure Files (preview) via SMB](../articles/storage/files/storage-files-active-directory-overview.md).