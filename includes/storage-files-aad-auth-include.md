---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/22/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 64751e0fcbf9a2255964d0de673e2cc2020ceb9a
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50254557"
---
[Azure Files](../articles/storage/files/storage-files-introduction.md) biedt ondersteuning voor verificatie op basis van identiteit via SMB (Server Message Block) (preview) via [Azure Active Directory (Azure AD) Domain Services](../articles/active-directory-domain-services/active-directory-ds-overview.md). Uw domein Windows virtuele machines (VM's) kunnen toegang krijgen tot Azure-bestandsshares met behulp van [Azure AD](../articles/active-directory/fundamentals/active-directory-whatis.md) referenties. 

Azure AD verifieert een identiteit, zoals een gebruiker, groep of service-principal met [op rollen gebaseerd toegangsbeheer (RBAC)](../articles/role-based-access-control/overview.md). U kunt aangepaste RBAC-rollen die algemene sets machtigingen die worden gebruikt voor toegang tot Azure Files omvatten definiëren. Wanneer u uw aangepaste RBAC-rol naar een Azure AD-identiteit die identiteit toegang tot een Azure-bestandsshare op basis van deze machtigingen krijgt.

Als onderdeel van de Preview-versie, Azure Files biedt ook ondersteuning te behouden, overnemen en afdwingen van [NTFS DACL's](https://technet.microsoft.com/library/2006.01.howitworksntfs.aspx) op alle bestanden en mappen in een bestandsshare. Als u gegevens van een bestandsshare naar Azure Files kopiëren of vice versa, u dat opgeven kunt worden NTFS DACL's bijgehouden. Op deze manier kunt u back-upscenario's met behulp van Azure Files, behoud van uw DACLS NTFS tussen uw on-premises-bestandsshare en de bestandsshare voor cloud implementeren. 

> [!NOTE]
> - Azure AD-verificatie via SMB wordt niet ondersteund voor virtuele Linux-machines voor de preview-versie. Alleen Windows Server VM's worden ondersteund.
> - Azure AD-verificatie via SMB wordt niet ondersteund voor on-premises computers toegang tot Azure Files.
> - Azure AD-verificatie is alleen beschikbaar voor opslagaccounts die zijn gemaakt na 24 September 2018.
> - Azure AD-verificatie via het SMB- en NTFS ACL permanente wordt niet ondersteund op Azure-bestandsshares worden beheerd door Azure File Sync-Service. 
