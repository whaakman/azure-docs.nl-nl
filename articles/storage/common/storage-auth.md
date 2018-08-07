---
title: Verlenen van toegang tot Azure Storage | Microsoft Docs
description: Meer informatie over de verschillende manieren toegang verlenen aan Azure Storage, waaronder Azure Active Directory, gedeelde sleutelverificatie of handtekeningen voor gedeelde toegang.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: ab4f9d6cbdbc047d688b57e906ea60aec6fff2fa
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39531107"
---
# <a name="authorizing-access-to-azure-storage"></a>Verlenen van toegang tot Azure Storage

Telkens wanneer die u toegang gegevens in uw opslagaccount tot, zendt de client een verzoek via HTTP/HTTPS naar Azure Storage. Elke aanvraag aan een beveiligde resource moet worden geautoriseerd, zodat de service zorgt ervoor dat de client de vereiste machtigingen heeft voor toegang tot de gegevens. Azure Storage biedt deze opties voor het verlenen van toegang om resources te beveiligen:

- **Integratie van Azure Active Directory (Azure AD) (Preview)** voor blobs en wachtrijen. Azure AD biedt op rollen gebaseerd toegangsbeheer (RBAC) voor heel nauwkeurig bepalen van een client toegang tot resources in een storage-account. Zie voor meer informatie, [verifiëren van aanvragen voor Azure Storage met behulp van Azure Active Directory (Preview)](storage-auth-aad.md).
- **Gedeelde sleutel autorisatie** voor blobs, bestanden, wachtrijen en tabellen. Een client met behulp van gedeelde sleutel geeft een koptekst met elke aanvraag die is ondertekend met behulp van de toegangssleutel voor opslagaccount. Zie voor meer informatie, [autoriseren met gedeelde sleutel](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- **Gedeelde toegangshandtekeningen** voor blobs, bestanden, wachtrijen en tabellen. Handtekeningen voor gedeelde toegang (SAS) beperkt gedelegeerde toegang bieden tot bronnen in een storage-account. Beperkingen op het tijdsinterval op dat de handtekening geldig is of op het biedt een flexibiliteit bij het beheren van toegang verleent machtigingen toevoegen. Zie voor meer informatie, [Using shared access signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Anonieme openbare leestoegang** voor containers en blobs. Autorisatie is niet vereist. Zie [Anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md) voor meer informatie.  

Standaard worden alle resources in Azure Storage zijn beveiligd en zijn alleen beschikbaar voor de accounteigenaar. Maar u een van de autorisatie-strategieën die hierboven worden beschreven clients om toegang te verlenen tot bronnen in uw storage-account gebruiken kunt, Microsoft raadt het gebruik van Azure AD indien mogelijk voor een maximale beveiliging en gebruiksgemak. 



