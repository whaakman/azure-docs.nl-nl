---
title: Toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory autoriseren | Microsoft Docs
description: Verleent toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 8033dda4059a52cea2b775fc8765a9f2a91b96dd
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67302415"
---
# <a name="authorize-access-to-azure-blobs-and-queues-using-azure-active-directory"></a>Toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory autoriseren

Azure Storage ondersteunt het gebruik van Azure Active Directory (AD) om aanvragen naar de Blob en Queue-opslag te autoriseren. Met Azure AD, kunt u op rollen gebaseerd toegangsbeheer (RBAC) voor het verlenen van machtigingen aan een beveiligings-principal die mogelijk een gebruiker, groep of toepassing service-principal. De beveiligings-principal is geverifieerd door Azure AD om terug te keren een OAuth 2.0-token. Het token kan worden gebruikt om een aanvraag voor toegang tot een resource in de wachtrij- of Blob-opslag.

Autoriseren van gebruikers of toepassingen die gebruikmaken van een OAuth 2.0 dat wordt geretourneerd door Azure AD biedt uitstekende beveiliging en gebruiksgemak ten opzichte van de gedeelde sleutel autorisatie en shared access signatures (SAS). Met Azure AD is er niet nodig voor het opslaan van de toegangssleutel met uw code en het risico potentiële beveiligingsproblemen. Terwijl u kunt echter ook doorgaan met de gedeelde sleutel autorisatie met uw toepassingen, heeft met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel voor uw code. U kunt ook blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) om te voorzien in specifieke toegang tot resources in uw opslagaccount verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak om te beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid. Microsoft raadt u aan met behulp van Azure AD-verificatie met uw Azure Storage-toepassingen indien mogelijk.

Autorisatie met Azure AD is beschikbaar voor alle voor algemeen gebruik en Blob storage-accounts in alle openbare regio's en nationale clouds. Alleen opslagaccounts die zijn gemaakt met de ondersteuning van Azure Resource Manager deployment model autorisatie van Azure AD.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Overzicht van Azure AD voor blobs en wachtrijen

Wanneer een beveiligings-principal (een gebruiker, groep of toepassing) probeert te krijgen tot een bron-blob of een wachtrij, moet de aanvraag worden geautoriseerd, tenzij het een blob beschikbaar voor anonieme toegang. Met Azure AD is toegang tot een resource een proces in twee stappen. Ten eerste de beveiligings-principal-id is geverifieerd en een OAuth 2.0-token wordt geretourneerd. Het token wordt vervolgens doorgegeven als onderdeel van een aanvraag naar de Blob of Queue-service en gebruikt door de service toegang verlenen aan de opgegeven resource.

De stap van de verificatie is vereist dat een toepassing een OAuth 2.0-toegangstoken tijdens runtime aanvragen. Als een toepassing wordt uitgevoerd op binnen een Azure-entiteit, zoals een Azure-VM, een virtuele-machineschaalset of een Azure Functions-app, kan gebruiken een [beheerde identiteit](../../active-directory/managed-identities-azure-resources/overview.md) aan toegang tot blobs of wachtrijen. Zie voor informatie over het toestaan van aanvragen door een beheerde identiteit voor de service Azure Blob of een wachtrij, [verleent toegang tot blobs en wachtrijen met Azure Active Directory en beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md).

De stap van de autorisatie is vereist dat een of meer RBAC-rollen worden toegewezen aan de beveiligings-principal. Azure Storage biedt RBAC-rollen die algemene sets machtigingen voor blob- en wachtrijservices gegevens omvatten. De rollen die zijn toegewezen aan een beveiligingsprincipal bepalen de machtigingen die de principal heeft. Zie voor meer informatie over het toewijzen van RBAC-rollen voor Azure Storage, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).

Systeemeigen toepassingen en webtoepassingen die aanvragen naar de service Azure Blob of een wachtrij versturen kunnen ook verleent voor toegang met Azure AD. Zie voor meer informatie over het aanvragen van een toegangstoken en deze gebruiken om aanvragen voor gegevens voor blob of een wachtrij te autoriseren, [toegang verlenen tot Azure Storage met Azure AD vanuit een Azure Storage-toepassing](storage-auth-aad-app.md).

## <a name="assigning-rbac-roles-for-access-rights"></a>RBAC-rollen voor toegangsrechten toewijzen

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot blob- en wachtrijservices gegevens omvatten. U kunt ook aangepaste rollen voor toegang tot blob- en wachtrijservices gegevens definiëren.

Wanneer een RBAC-rol is toegewezen aan een beveiligings-principal voor Azure AD, wordt de status van Azure verleent toegang tot deze resources voor deze beveiligings-principal. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Een beveiligings-principal voor Azure AD kan een gebruiker, een groep, een service-principal van toepassing zijn of een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="built-in-rbac-roles-for-blobs-and-queues"></a>Ingebouwde RBAC-rollen voor blobs en wachtrijen

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Zie voor meer informatie over een ingebouwde RBAC-rol toewijzen aan een beveiligings-principal, een van de volgende artikelen:

- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac-portal.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van Azure CLI](storage-auth-aad-rbac-cli.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van PowerShell](storage-auth-aad-rbac-powershell.md)

Voor meer informatie over hoe u de ingebouwde rollen voor Azure Storage zijn gedefinieerd, Zie [begrijpen roldefinities](../../role-based-access-control/role-definitions.md#management-and-data-operations). Zie voor meer informatie over het maken van aangepaste RBAC-rollen [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../../role-based-access-control/custom-roles.md).

### <a name="access-permissions-for-data-operations"></a>Toegangsmachtigingen voor gegevensbewerkingen

Zie voor meer informatie over de machtigingen die nodig zijn om aan te roepen specifieke Blob of een wachtrij servicebewerkingen, [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

## <a name="resource-scope"></a>Resource-bereik

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="access-data-with-an-azure-ad-account"></a>Toegang tot gegevens met een Azure AD-account

Toegang tot blob of een wachtrij gegevens via de Azure portal, PowerShell of Azure CLI kan worden geautoriseerd met behulp van de gebruiker Azure AD-account of met behulp van de toegangssleutels van account (gedeelde sleutel autorisatie).

### <a name="data-access-from-the-azure-portal"></a>Toegang tot de gegevens vanuit Azure portal

De Azure-portal kunt gebruiken op uw Azure AD-account of de toegangssleutels van account voor toegang tot gegevens voor blob- en wachtrijservices in Azure storage-account. Welke autorisatieschema maakt gebruik van de Azure-portal, is afhankelijk van de RBAC-rollen die aan u zijn toegewezen.

Wanneer u probeert te krijgen tot gegevens voor blob of een wachtrij, de Azure-portal controleert eerst of u een RBAC-rol met toegewezen bent **Microsoft.Storage/storageAccounts/listkeys/action**. Als u een rol met deze actie is toegewezen, gebruikt de Azure-portal de accountsleutel voor toegang tot blob- en wachtrijservices gegevens via gedeelde sleutel autorisatie. Als u niet een rol met deze actie toegewezen bent, klikt u vervolgens wil de Azure-portal toegang tot gegevens via uw Azure AD-account.

Gegevens uit Azure portal met uw Azure AD-account, moet u machtigingen voor toegang tot blobopslag-en wachtrij voor toegang tot blob of een wachtrij en u moet ook machtigingen om te navigeren door de resources van het opslagaccount in Azure portal. De ingebouwde rollen die is geleverd door Azure Storage verlenen toegang tot blob- en wachtrijservices resources, maar ze niet verlenen aan machtigingen voor de resources van opslagaccount. Om deze reden toegang tot de portal ook moet de toewijzing van een Azure Resource Manager-rol, zoals de [lezer](../../role-based-access-control/built-in-roles.md#reader) rol binnen het bereik van het niveau van het storage-account of hoger. De **lezer** rol, de meest beperkte machtigingen hebben, maar ook een andere Azure Resource Manager-rol die toegang tot beheer van de resources van opslagaccount verleent acceptabel is. Zie voor meer informatie over hoe u machtigingen toewijzen aan gebruikers voor toegang tot gegevens in Azure portal met een Azure AD-account, [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac-portal.md).

De Azure portal wordt aangegeven welke autorisatieschema is in gebruik wanneer u gaat u naar een container of een wachtrij. Zie voor meer informatie over toegang tot gegevens in de portal, [gebruik van Azure portal voor toegang tot blob of een wachtrij gegevens](storage-access-blobs-queues-portal.md).

### <a name="data-access-from-powershell-or-azure-cli"></a>Gegevenstoegang vanuit PowerShell of Azure CLI

Azure CLI en PowerShell ondersteuning voor aanmelden met Azure AD-referenties. Nadat u zich hebt aangemeld, is uw sessie wordt uitgevoerd onder deze referenties. Zie voor meer informatie, [uitvoeren op Azure CLI of PowerShell-opdrachten met Azure AD-referenties voor toegang tot gegevens voor blob of een wachtrij](storage-auth-aad-script.md).

## <a name="azure-ad-authorization-over-smb-for-azure-files"></a>Azure AD-autorisatie via SMB voor Azure Files

Azure Files biedt ondersteuning voor verificatie met Azure AD via SMB voor domein-VM's alleen (preview). Zie voor meer informatie over het gebruik van Azure AD voor Azure Files via SMB, [overzicht van Azure Active Directory-autorisatie via SMB voor Azure Files (preview)](../files/storage-files-active-directory-overview.md).

## <a name="next-steps"></a>Volgende stappen

- [Verleent toegang tot blobs en wachtrijen met Azure Active Directory en beheerde identiteiten voor Azure-Resources](storage-auth-aad-msi.md)
- [Verifiëren met Azure Active Directory vanaf een aanvraag voor toegang tot blobs en wachtrijen](storage-auth-aad-app.md)
- [Ondersteuning voor Azure Storage voor Azure Active Directory gebaseerde toegang algemeen beschikbaar](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
