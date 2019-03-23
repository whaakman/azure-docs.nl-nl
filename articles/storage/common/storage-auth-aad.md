---
title: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory | Microsoft Docs
description: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ff543b7275ab05a83b1be1d156cbc6059a3b5430
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369886"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory

Azure Storage biedt ondersteuning voor verificatie en autorisatie met Azure Active Directory (AD) voor de services Blob en wachtrij. U kunt met Azure AD, op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om toegang te verlenen aan gebruikers, groepen of service-principals van toepassing. 

Verifiëren van gebruikers of toepassingen die gebruikmaken van Azure AD-referenties biedt uitstekende beveiliging en gebruiksgemak ten opzichte van andere middelen van autorisatie. Terwijl u kunt echter ook doorgaan met de gedeelde sleutel autorisatie met uw toepassingen, heeft met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel voor uw code. U kunt ook blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) om te voorzien in specifieke toegang tot resources in uw opslagaccount verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak om te beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid. Microsoft raadt u aan met behulp van Azure AD-verificatie voor uw Azure Storage-toepassingen indien mogelijk.

Verificatie en autorisatie met Azure AD-referenties is beschikbaar voor alle algemeen gebruik v2, algemeen gebruik v1 en Blob storage-accounts in alle openbare regio's. Alleen opslagaccounts die zijn gemaakt met de ondersteuning van Azure Resource Manager deployment model autorisatie van Azure AD.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Overzicht van Azure AD voor blobs en wachtrijen

De eerste stap bij het gebruik van Azure AD-integratie met Azure Storage is RBAC-rollen voor gegevens in de opslag toewijzen aan uw service-principal (een gebruiker, groep of toepassing service-principal) of de beheerde identiteiten voor Azure-resources. RBAC-rollen omvatten algemene sets machtigingen voor containers en wachtrijen. Zie voor meer informatie over het toewijzen van RBAC-rollen voor Azure Storage, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).

Voor het gebruik van Azure AD toegang verlenen aan de storage-resources in uw toepassingen, moet u een OAuth 2.0-toegangstoken aanvragen vanuit uw code. Zie voor informatie over het aanvragen van een toegangstoken en voor het toestaan van aanvragen voor Azure Storage, [verifiëren met Azure AD vanuit een Azure Storage-toepassing](storage-auth-aad-app.md). Als u van een beheerde identiteit gebruikmaakt, Zie [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md).

Azure CLI en PowerShell bieden nu ondersteuning voor aangemeld met een Azure AD-identiteit. Nadat u zich met een Azure AD-identiteit aanmeldt, is uw sessie wordt uitgevoerd onder die identiteit. Zie voor meer informatie, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-rollen voor blobs en wachtrijen

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot containers of wachtrijen omvatten. 

Wanneer een RBAC-rol is toegewezen aan een beveiligings-principal voor Azure AD, wordt de status van Azure verleent toegang tot deze resources voor deze beveiligings-principal. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Een beveiligings-principal voor Azure AD kan een gebruiker, een groep, een service-principal van toepassing zijn of een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Zie voor informatie over het toewijzen van een ingebouwde rol in Azure portal, [toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal](storage-auth-aad-rbac.md).

### <a name="access-permissions-granted-by-rbac-roles"></a>Machtigingen voor toegang verleend door RBAC-rollen 

Zie voor meer informatie over de vereiste machtigingen voor het aanroepen van Azure Storage-bewerkingen, [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="next-steps"></a>Volgende stappen

- [Toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal](storage-auth-aad-rbac.md)
- [Verifiëren met Azure Active Directory vanaf een aanvraag voor toegang tot blobs en wachtrijen](storage-auth-aad-app.md)
- [Verifiëren van toegang tot blobs en wachtrijen met beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md)
- [Een Azure AD-identiteit gebruiken voor toegang tot Azure Storage met CLI of PowerShell](storage-auth-aad-script.md)