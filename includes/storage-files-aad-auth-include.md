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
ms.openlocfilehash: ff2ed5abbf2ce67a0b96a5da450b83832403db1f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269352"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) biedt ondersteuning voor verificatie op basis van identiteit via SMB (Server Message Block) (preview) via [Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/overview.md). Uw domein Windows virtuele machines (VM's) kunnen toegang krijgen tot Azure-bestandsshares met behulp van [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md) referenties. 

U kunt Azure Files-share op toegang tot een identiteit, zoals een gebruiker, groep in Azure AD met beheren [op rollen gebaseerd toegangsbeheer (RBAC)](../articles/role-based-access-control/overview.md). U kunt aangepaste RBAC-rollen die algemene sets machtigingen die worden gebruikt voor toegang tot Azure Files omvatten definiëren. Wanneer u uw aangepaste RBAC-rol naar een Azure AD-identiteit die identiteit toegang tot een Azure-bestandsshare op basis van deze machtigingen krijgt.

Als onderdeel van de Preview-versie, Azure Files biedt ook ondersteuning te behouden, overnemen en afdwingen van [NTFS DACL's](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) op alle bestanden en mappen in een bestandsshare. Als u gegevens van een bestandsshare naar Azure Files kopiëren of vice versa, u dat opgeven kunt worden NTFS DACL's bijgehouden. Op deze manier kunt u back-upscenario's met behulp van Azure Files, behoud van uw DACLS NTFS tussen uw on-premises-bestandsshare en de bestandsshare voor cloud implementeren. 

> [!NOTE]
> - Azure AD Domain Services-verificatie voor SMB-toegang wordt niet ondersteund voor virtuele Linux-machines. Alleen virtuele Windows-machines worden ondersteund.
> - Azure AD Domain Services-verificatie voor SMB-toegang wordt niet ondersteund voor de computer van Active Directory-domein is gekoppeld. In de tussentijd kunt u overwegen om gebruik te [Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) om te beginnen met het migreren van uw gegevens naar Azure Files en doorgaan met het afdwingen van toegangsbeheer met het AD-credentails van uw on-premises AD domein-machines. 
> - Azure AD Domain Services-verificatie voor SMB-toegang is alleen beschikbaar voor opslagaccounts die zijn gemaakt na 24 September 2018.
> - Azure AD Domain Services-verificatie voor SMB-toegang en NTFS DACL permanente wordt niet ondersteund op Azure-bestandsshares worden beheerd door Azure File Sync-Service. 
