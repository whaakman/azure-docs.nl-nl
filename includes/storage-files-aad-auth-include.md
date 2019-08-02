---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d1b10b55481b41f42c6c872522d3dd4dd4be0e77
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68570589"
---
[Azure files](../articles/storage/files/storage-files-introduction.md) biedt ondersteuning voor verificatie op basis van identiteit via SMB (Server Message Block) via [Azure Active Directory Domain Services (AD DS)](../articles/active-directory-domain-services/overview.md). Uw virtuele Windows-machines (Vm's) die lid zijn van een domein, kunnen toegang krijgen tot Azure-bestands shares met behulp van de referenties van [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) .

U kunt Azure Files toegang op share niveau beheren voor een identiteit zoals een gebruiker of groep in azure AD met behulp van [op rollen gebaseerd toegangs beheer (RBAC)](../articles/role-based-access-control/overview.md). U kunt aangepaste RBAC-rollen definiëren die algemene sets machtigingen bevatten die worden gebruikt voor toegang tot Azure Files. Wanneer u uw aangepaste RBAC-rol toewijst aan een Azure AD-identiteit, wordt aan die identiteit toegang verleend tot een Azure-bestands share op basis van die machtigingen.

Als onderdeel van de preview ondersteunt Azure Files ook het behouden, overnemen en afdwingen van [NTFS-dacl's](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) voor alle bestanden en mappen in een bestands share. Als u gegevens kopieert van een bestands share naar Azure Files, of andersom, kunt u opgeven dat NTFS-DACL'S worden bewaard. Op deze manier kunt u back-upscenario's implementeren met behulp van Azure Files, zodat uw NTFS-DACL'S worden bewaard tussen uw on-premises bestands share en uw Cloud bestands share. 

> [!NOTE]
> - Azure AD DS-verificatie voor SMB-toegang (Server Message Block) wordt niet ondersteund voor Linux-Vm's. Alleen virtuele Windows-machines worden ondersteund.
> - Azure AD DS-verificatie voor SMB-toegang wordt niet ondersteund voor Active Directory computers die lid zijn van een domein. Overweeg het gebruik van [Azure file sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) om te beginnen met het migreren van uw gegevens naar Azure files en het afdwingen van toegangs beheer met behulp van Active Directory referenties van uw on-premises Active Directory computers die lid zijn van een domein. 
> - Azure AD DS-verificatie voor SMB-toegang is alleen beschikbaar voor opslag accounts die zijn gemaakt na 24 september 2018.
> - Azure AD DS-verificatie voor SMB-toegang en NTFS-DACL-persistentie wordt niet ondersteund in azure-bestands shares die worden beheerd door Azure File Sync.
