---
title: De Azure portal gebruiken voor toegang tot blob of gegevens - Azure Storage in de wachtrij
description: Wanneer u toegang tot blob of wachtrijgegevens met behulp van de Azure portal, de portal maakt aanvragen naar Azure Storage op de achtergrond. Deze aanvragen voor Azure Storage kunnen worden geverifieerd en geautoriseerd met behulp van uw Azure AD-account of de toegangssleutel voor opslagaccount.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/19/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: b56cbfbc9dfde8b1a7d43d55ee85c34fde385902
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58846388"
---
# <a name="use-the-azure-portal-to-access-blob-or-queue-data"></a>Gebruik de Azure-portal voor het openen van blob of een wachtrij

Wanneer u toegang krijgen tot blob of wachtrij gegevens met de [Azure-portal](https://portal.azure.com), de portal maakt aanvragen naar Azure Storage op de achtergrond. Deze aanvragen voor Azure Storage kunnen worden geverifieerd en geautoriseerd met behulp van uw Azure AD-account of de toegangssleutel voor opslagaccount. De portal wordt aangegeven welke verificatiemethode die u gebruikt, en kunt u om over te schakelen tussen de twee als u de juiste machtigingen hebt.  

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Vereiste machtigingen voor toegang tot gegevens voor blob of een wachtrij

Afhankelijk van hoe u verifiëren van toegang tot blob of een wachtrij-gegevens in Azure portal wilt, moet u specifieke machtigingen. Deze machtigingen zijn in de meeste gevallen geboden via op rollen gebaseerd toegangsbeheer (RBAC). Zie voor meer informatie over RBAC [wat is op rollen gebaseerd toegangsbeheer (RBAC)?](../../role-based-access-control/overview.md).

### <a name="account-access-key"></a>Toegangssleutel

Voor toegang tot blob- en wachtrijservices gegevens door de toegangssleutel voor het account, hebt u een RBAC-rol die aan u toegewezen met de actie RBAC **Microsoft.Storage/storageAccounts/listkeys/action**. Deze rol RBAC is mogelijk een ingebouwde of een aangepaste rol. Ingebouwde rollen die ondersteuning bieden voor **Microsoft.Storage/storageAccounts/listkeys/action** opnemen:

- De Azure Resource Manager [eigenaar](../../role-based-access-control/built-in-roles.md#owner) rol
- De Azure Resource Manager [Inzender](../../role-based-access-control/built-in-roles.md#contributor) rol
- De [Inzender voor Opslagaccounts](../../role-based-access-control/built-in-roles.md#storage-account-contributor) rol

Wanneer u probeert te krijgen tot blob of een wachtrij gegevens in Azure portal, de portal controleert eerst of u een rol met toegewezen bent **Microsoft.Storage/storageAccounts/listkeys/action**. Als u een rol met deze actie is toegewezen, klikt u vervolgens de portal maakt gebruik van de accountsleutel voor toegang tot gegevens voor blob- en wachtrijservices. Als u niet een rol met deze actie toegewezen bent, klikt u vervolgens de portal geprobeerd toegang tot gegevens via uw Azure AD-account.

> [!NOTE]
> De beheerdersrollen klassiek abonnement servicebeheerder en Medebeheerder omvatten het equivalent van de Azure Resource Manager [eigenaar](../../role-based-access-control/built-in-roles.md#owner) rol. De **eigenaar** rol bevat alle acties, met inbegrip van de **Microsoft.Storage/storageAccounts/listkeys/action**, zodat een gebruiker met een van deze beheerdersrollen ook toegang blob- en wachtrijservices gegevens met tot de accountsleutel. Zie voor meer informatie, [klassiek abonnement beheerdersrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="azure-ad-account"></a>Azure AD-account

Voor toegang tot blob of een wachtrij gegevens vanuit de Azure-portal met behulp van uw Azure AD-account, moeten beide van de volgende instructies geldt voor u zijn:

- U bent de Azure Resource Manager toegewezen [lezer](../../role-based-access-control/built-in-roles.md#reader) rol, ten minste binnen het bereik op het niveau van het storage-account of hoger. De **lezer** rol, de meest beperkte machtigingen hebben, maar ook een andere Azure Resource Manager-rol die toegang tot beheer van de resources van opslagaccount verleent acceptabel is.
- U bent toegewezen ofwel een ingebouwde of aangepaste rol die toegang tot de blobs of wachtrijgegevens biedt.

De **lezer** roltoewijzing of een andere Azure Resource Manager-roltoewijzing is nodig zodat de gebruiker kan weergeven en storage-account management-resources in de Azure-portal te navigeren. De RBAC-rollen die toegang tot gegevens voor blob of een wachtrij verlenen Verleen geen toegang tot opslag account management-resources. Voor toegang tot blob of een wachtrij gegevens in de portal, moet de gebruiker machtigingen om te navigeren van resources voor storage-account. Zie voor meer informatie over deze vereiste [toewijzen van de rol van lezer voor portaltoegang](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

De ingebouwde rollen die ondersteuning bieden voor toegang tot uw gegevens blob of een wachtrij zijn onder andere:

- [Storage Blob Gegevenseigenaar](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Voor POSIX toegangsbeheer voor Azure Data Lake Storage Gen2 (preview).
- [Gegevensbijdrager voor Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Machtigingen voor lezen/schrijven/verwijderen van blobs.
- [Gegevenslezer voor Opslagblob](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Alleen-lezen machtigingen voor blobs.
- [Gegevensbijdrager voor wachtrij](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Machtigingen voor lezen/schrijven/verwijderen voor wachtrijen.
- [Gegevenslezer voor Opslagwachtrij](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): Alleen-lezen machtigingen voor wachtrijen.
    
Aangepaste rollen kunnen verschillende combinaties van dezelfde machtigingen geleverd door de ingebouwde rollen ondersteunen. Zie voor meer informatie over het maken van aangepaste RBAC-rollen [aangepaste rollen voor Azure-resources](../../role-based-access-control/custom-roles.md) en [roldefinities voor Azure-resources begrijpen](../../role-based-access-control/role-definitions.md).

> [!NOTE]
> Wachtrijen met de rol van een klassiek abonnement beheerder aanbieding wordt niet ondersteund. Om weer te geven in de wachtrij geplaatst, een gebruiker moet zijn toegewezen aan de Azure Resource Manager **lezer** rol, de **gegevenslezer voor Opslagwachtrij** rol of de **Gegevensbijdrager voor wachtrij** de rol.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>Navigeer naar blobs of wachtrijen in de portal

Als u wilt inzien blob of een wachtrij in de portal, gaat u naar de **overzicht** voor uw opslagaccount en klik op de koppelingen voor **Blobs** of **wachtrijen**. U kunt ook u kunt naar de **Blob-service** en **Queue-service** secties in het menu. 

![Navigeer naar de blob of een wachtrij gegevens in Azure portal](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>De huidige verificatiemethode bepalen

Wanneer u naar een container of een wachtrij navigeert, wordt de Azure-portal geeft aan of u momenteel gebruikmaakt van de toegangssleutel of uw Azure AD-account om te verifiëren.

De voorbeelden in deze sectie geven toegang tot een container en de blobs, maar de portal het bericht wordt weergegeven wanneer u toegang tot een wachtrij en de berichten, of wachtrijen lijst.

### <a name="account-access-key"></a>Toegangssleutel

Als u tijdens de verificatie met behulp van de toegangssleutel, ziet u **toegangssleutel** opgegeven als de verificatiemethode in de portal:

![Momenteel toegang hebben tot containergegevens met de accountsleutel](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Als u wilt overschakelen op het gebruik van Azure AD-account, klikt u op de koppeling in de afbeelding gemarkeerd. Hebt u de juiste machtigingen via de RBAC-rollen die aan u zijn toegewezen, moet u mogelijk zijn om door te gaan. Echter, als u niet over de juiste machtigingen, ziet u een foutbericht weergegeven zoals in de volgende:

![Fout die wordt weergegeven als Azure AD-account toegang wordt niet ondersteund](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

U ziet dat geen blobs moeten worden weergegeven in de lijst als uw Azure AD-account beschikt niet over machtigingen om ze te bekijken. Klik op de **overschakelen naar de toegangssleutel** koppeling met de toegangssleutel voor de verificatie opnieuw uit.

### <a name="azure-ad-account"></a>Azure AD-account

Als u tijdens de verificatie met behulp van uw Azure AD-account, ziet u **Azure AD-gebruikersaccount** opgegeven als de verificatiemethode in de portal:

![Momenteel toegang hebben tot containergegevens met Azure AD-account](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Als u wilt overschakelen op het gebruik van de toegangssleutel, klikt u op de koppeling in de afbeelding gemarkeerd. Als u toegang tot de accountsleutel hebt, klikt u vervolgens kunt u moet zich om door te gaan. Echter, als u toegang tot de accountsleutel ontbreken, ziet u een foutbericht weergegeven zoals in de volgende:

![Fout die wordt weergegeven als u geen toegang tot accountsleutel hebt](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

U ziet dat geen blobs in de lijst worden weergegeven als u geen toegang tot de sleutels. Klik op de **overschakelen naar de Azure AD-gebruikersaccount** koppeling naar uw Azure AD-account gebruiken voor verificatie opnieuw uit.

## <a name="next-steps"></a>Volgende stappen

- [Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory](storage-auth-aad.md)
- [Toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal](storage-auth-aad-rbac-portal.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van Azure CLI](storage-auth-aad-rbac-cli.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van PowerShell](storage-auth-aad-rbac-powershell.md)
