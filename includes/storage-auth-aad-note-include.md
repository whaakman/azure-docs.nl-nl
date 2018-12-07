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
ms.openlocfilehash: 0182df40a4e7815560a85e60fe9062ccd8001c18
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978847"
---
> [!NOTE]
> - De Preview-versie van Azure AD-verificatie voor blobs en wachtrijen is bedoeld voor gebruik in alleen niet-productieomgevingen. Productie service level agreements (Sla's) zijn momenteel niet beschikbaar. Als Azure AD-verificatie wordt nog niet ondersteund voor uw scenario, echter ook doorgaan met de gedeelde sleutel autorisatie of SAS-tokens in uw toepassingen.
>
> - Tijdens de Preview-versie duurt RBAC-roltoewijzingen tot vijf minuten worden doorgegeven.
>
> - U moet HTTPS gebruiken voor het autoriseren van blob- en wachtrijservices bewerkingen met een OAuth-token.
>
> - De Azure-portal biedt nu ondersteuning voor Azure AD-referenties gebruiken om te lezen en schrijven van blob en wachtrijgegevens, als onderdeel van de Preview-versie uitbrengen.
> 
> - [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gebruikt momenteel de sleutel van uw opslagaccount voor het openen van blob- en wachtrijservices. OAuth-toegang wordt ondersteund voor blobs.
>
> - Azure Files biedt ondersteuning voor verificatie met Azure AD via SMB voor domein-VM's alleen (preview). Zie voor meer informatie over het gebruik van Azure AD voor Azure Files via SMB, [overzicht van Azure Active Directory-verificatie voor Azure Files (preview) via SMB](../articles/storage/files/storage-files-active-directory-overview.md).