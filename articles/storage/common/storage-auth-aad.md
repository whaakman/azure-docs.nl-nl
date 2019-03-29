---
title: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory | Microsoft Docs
description: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/28/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e72400a759b976b1a2a6864b2fa7d7d91e16c62f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619285"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory

Azure Storage biedt ondersteuning voor verificatie en autorisatie met Azure Active Directory (AD) voor de services Blob en wachtrij. U kunt met Azure AD, op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om toegang te verlenen aan gebruikers, groepen of service-principals van toepassing. 

Verifiëren van gebruikers of toepassingen die gebruikmaken van Azure AD-referenties biedt uitstekende beveiliging en gebruiksgemak ten opzichte van andere middelen van autorisatie. Terwijl u kunt echter ook doorgaan met de gedeelde sleutel autorisatie met uw toepassingen, heeft met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel voor uw code. U kunt ook blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) om te voorzien in specifieke toegang tot resources in uw opslagaccount verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak om te beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid. Microsoft raadt u aan met behulp van Azure AD-verificatie voor uw Azure Storage-toepassingen indien mogelijk.

Verificatie en autorisatie met Azure AD-referenties is beschikbaar voor alle algemeen gebruik v2, algemeen gebruik v1 en Blob storage-accounts in alle openbare regio's. Alleen opslagaccounts die zijn gemaakt met de ondersteuning van Azure Resource Manager deployment model autorisatie van Azure AD.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Overzicht van Azure AD voor blobs en wachtrijen

De eerste stap bij het gebruik van Azure AD-integratie met Azure Storage is RBAC-rollen voor gegevens in de opslag toewijzen aan uw service-principal (een gebruiker, groep of toepassing service-principal) of de beheerde identiteiten voor Azure-resources. RBAC-rollen omvatten algemene sets machtigingen voor containers en wachtrijen. Zie voor meer informatie over het toewijzen van RBAC-rollen voor Azure Storage, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).

Voor het gebruik van Azure AD toegang verlenen aan de storage-resources in uw toepassingen, moet u een OAuth 2.0-toegangstoken aanvragen vanuit uw code. Zie voor informatie over het aanvragen van een toegangstoken en voor het toestaan van aanvragen voor Azure Storage, [verifiëren met Azure AD vanuit een Azure Storage-toepassing](storage-auth-aad-app.md). Als u van een beheerde identiteit gebruikmaakt, Zie [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md).

Azure CLI en PowerShell bieden nu ondersteuning voor aangemeld met een Azure AD-identiteit. Nadat u zich met een Azure AD-identiteit aanmeldt, is uw sessie wordt uitgevoerd onder die identiteit. Zie voor meer informatie, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-rollen voor blobs en wachtrijen

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot blob- en wachtrijservices gegevens omvatten. U kunt ook aangepaste rollen voor toegang tot blob- en wachtrijservices gegevens definiëren.

Wanneer een RBAC-rol is toegewezen aan een beveiligings-principal voor Azure AD, wordt de status van Azure verleent toegang tot deze resources voor deze beveiligings-principal. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Een beveiligings-principal voor Azure AD kan een gebruiker, een groep, een service-principal van toepassing zijn of een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Zie voor informatie over het toewijzen van een ingebouwde RBAC voor Azure Storage-resources, een van de volgende onderwerpen:

- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac-portal.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van Azure CLI](storage-auth-aad-rbac-cli.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van PowerShell](storage-auth-aad-rbac-powershell.md)

### <a name="access-permissions-granted-by-rbac-roles"></a>Machtigingen voor toegang verleend door RBAC-rollen 

Zie voor meer informatie over de vereiste machtigingen voor het aanroepen van Azure Storage-bewerkingen, [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="resource-scope"></a>Resource-bereik

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="next-steps"></a>Volgende stappen

- [Ondersteuning voor Azure Storage voor Azure Active Directory gebaseerde toegang algemeen beschikbaar](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
- [Verifiëren met Azure Active Directory vanaf een aanvraag voor toegang tot blobs en wachtrijen](storage-auth-aad-app.md)
- [Verifiëren van toegang tot blobs en wachtrijen met beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md)
- Azure Files biedt ondersteuning voor verificatie met Azure AD via SMB voor domein-VM's alleen (preview). Zie voor meer informatie over het gebruik van Azure AD voor Azure Files via SMB, [overzicht van Azure Active Directory-verificatie voor Azure Files (preview) via SMB](../files/storage-files-active-directory-overview.md).