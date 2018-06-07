---
title: Goedkeuren van toegang tot Azure Storage | Microsoft Docs
description: Meer informatie over de verschillende manieren om toegang tot Azure Storage, met inbegrip van Azure Active Directory, gedeelde sleutel verificatie of handtekeningen voor gedeelde toegang te verlenen.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 46bb332e28d7503e543ca3c99fa1099ef17f34c3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660800"
---
# <a name="authorizing-access-to-azure-storage"></a>Goedkeuren van toegang tot Azure Storage

Telkens wanneer die u toegang gegevens in uw opslagaccount tot, zendt de client een verzoek via HTTP/HTTPS naar Azure Storage. Elke aanvraag naar een beveiligde bron moet worden toegestaan, zodat de service zorgt ervoor dat de client de vereiste machtigingen heeft voor toegang tot de gegevens. Azure Storage biedt deze opties voor het verlenen van toegang voor het beveiligen van bronnen:

- **Integratie van Azure Active Directory (Azure AD) (Preview)** voor blobs en wachtrijen. Azure AD levert de op rollen gebaseerde toegangsbeheer (RBAC) voor fijnmazig controle over een client toegang tot bronnen in een opslagaccount. Zie voor meer informatie [verifiëren van aanvragen naar Azure Storage met Azure Active Directory (Preview)](storage-auth-aad.md).
- **Gedeelde sleutel autorisatie** voor blobs, bestanden, wachtrijen en tabellen. Een client met gedeelde sleutel geeft een header met elke aanvraag die is ondertekend met behulp van de toegangssleutel voor opslagaccount. Zie voor meer informatie [autoriseren met gedeelde sleutel](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key/).
- **Shared access signatures** voor blobs, bestanden, wachtrijen en tabellen. Handtekeningen voor gedeelde toegang (SAS) beperkt gedelegeerde toegang bieden tot bronnen in een opslagaccount. Beperkingen op het tijdsinterval waarvoor de handtekening ongeldig is of deze biedt flexibiliteit bij het beheren van toegang verleent machtigingen toevoegen. Zie voor meer informatie [Using shared access signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Anonieme openbare leestoegang** voor containers en blobs. Autorisatie is niet vereist. Zie voor meer informatie [anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md).  

Standaard worden alle resources in Azure Storage zijn beveiligd en zijn alleen beschikbaar voor de accounteigenaar. Hoewel u een van de autorisatie-strategieën die hierboven worden beschreven clients om toegang te verlenen tot bronnen in uw storage-account gebruiken kunt, Microsoft raadt het gebruik van Azure AD indien mogelijk voor een maximale beveiliging en eenvoudig te gebruiken. 



