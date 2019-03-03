---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5d0e919159569e7512b6e7dc0458a4a3e397943b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57251955"
---
> [!NOTE]
> - De Preview-versie van Azure AD-verificatie voor blobs en wachtrijen is bedoeld voor gebruik in alleen niet-productieomgevingen. Productie service level agreements (Sla's) zijn momenteel niet beschikbaar. Als Azure AD-verificatie wordt nog niet ondersteund voor uw scenario, echter ook doorgaan met de gedeelde sleutel autorisatie of SAS-tokens in uw toepassingen.
>
> - Tijdens de Preview-versie duurt RBAC-roltoewijzingen tot vijf minuten worden doorgegeven.
>
> - U moet HTTPS gebruiken voor het autoriseren van blob- en wachtrijservices bewerkingen met een OAuth-token.
>
> - De Azure-portal biedt nu ondersteuning voor Azure AD-referenties gebruiken om te lezen en schrijven van blob en wachtrijgegevens, als onderdeel van de Preview-versie uitbrengen. Voor toegang tot gegevens voor blob- en wachtrijservices in Azure portal, moet de Azure Resource Manager-lezer RBAC-rol, naast de juiste preview-functie voor toegang tot blob of een wachtrij op een gebruiker worden toegewezen. Zie voor meer informatie, [toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal (preview)](../articles/storage/common/storage-auth-aad-rbac.md). 
> 
> - [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gebruikt momenteel de sleutel van uw opslagaccount voor het openen van blob- en wachtrijservices. OAuth-toegang wordt ondersteund voor blobs.
>
> - Azure Files biedt ondersteuning voor verificatie met Azure AD via SMB voor domein-VM's alleen (preview). Zie voor meer informatie over het gebruik van Azure AD voor Azure Files via SMB, [overzicht van Azure Active Directory-verificatie voor Azure Files (preview) via SMB](../articles/storage/files/storage-files-active-directory-overview.md).