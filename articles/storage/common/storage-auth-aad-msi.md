---
title: Verifiëren van toegang tot blobs en wachtrijen met Azure Active Directory beheerde identiteiten voor Azure-resources - Azure Storage | Microsoft Docs
description: Azure Blob- en Queue storage biedt ondersteuning voor Azure Active Directory-verificatie met beheerde identiteiten voor Azure-resources. U kunt beheerde identiteiten voor Azure-resources gebruiken om te verifiëren van toegang tot blobs en wachtrijen van toepassingen die worden uitgevoerd in virtuele machines van Azure, functie-apps, virtuele-machineschaalsets en anderen. Met behulp van beheerde identiteiten voor Azure-resources en gebruik te maken van de kracht van Azure AD-verificatie, kunt u voorkomen dat opslaan van referenties op met uw toepassingen die worden uitgevoerd in de cloud.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 4372045590938df701dd00e58a111215f6e8e56d
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369646"
---
# <a name="authenticate-access-to-blobs-and-queues-with-managed-identities-for-azure-resources"></a>Verifiëren van toegang tot blobs en wachtrijen met beheerde identiteiten voor Azure-Resources

Azure Blob en Queue storage ondersteuning voor verificatie met Azure Active Directory (Azure AD) [beheerde identiteiten voor een Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md). Beheerde identiteiten voor Azure-resources van toegang tot blobs verifiëren kunnen en wachtrijen met Azure AD-referenties van de toepassingen die worden uitgevoerd in virtuele Azure-machines (VM's), functie-apps, virtuele-machineschaalsets en anderen. Met behulp van beheerde identiteiten voor Azure-resources en gebruik te maken van de kracht van Azure AD-verificatie, kunt u voorkomen dat opslaan van referenties op met uw toepassingen die worden uitgevoerd in de cloud.  

Om machtigingen te verlenen aan een beheerde identiteit aan een blob-container of een wachtrij, kunt u de rol van een op basis van de rol beheer (RBAC) toewijzen aan de beheerde identiteit die machtigingen voor de resource op het juiste bereik omvat. Zie voor meer informatie over RBAC-rollen in de opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md). 

In dit artikel laat zien hoe om te verifiëren naar Azure Blob of Queue storage met een beheerde identiteit van een Azure-VM.  

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="enable-managed-identities-on-a-vm"></a>Beheerde identiteiten op een virtuele machine inschakelen

Voordat u beheerde identiteiten voor Azure-Resources gebruiken kunt om te verifiëren van toegang tot blobs en wachtrijen van de virtuele machine, moet u eerst beheerde identiteiten inschakelen voor Azure-Resources op de virtuele machine. Voor informatie over het inschakelen van beheerde identiteiten voor Azure-Resources, Zie een van de volgende artikelen:

- [Azure-portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjabloon](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDK 's](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="assign-an-rbac-role-to-an-azure-ad-managed-identity"></a>Een RBAC-rol toewijzen aan een Azure AD beheerde identiteit

Als u wilt een beheerde identiteit van uw Azure Storage-toepassing verifiëren, moet u eerst op basis van rollen (RBAC) instellingen voor toegangsbeheer voor die beheerde identiteit configureren. Azure Storage definieert RBAC-rollen die machtigingen voor containers en wachtrijen omvatten. Wanneer de RBAC-rol is toegewezen aan een beheerde identiteit, die de beheerde identiteit krijgt toegang tot die resource. Zie voor meer informatie, [beheren toegangsrechten tot Azure BLOB Storage en Queue gegevens met RBAC](storage-auth-aad-rbac.md).

## <a name="get-a-managed-identity-access-token"></a>Een beheerde identiteit toegangstoken ophalen

Als u wilt verifiëren met een beheerde identiteit, moet uw toepassing of script een beheerde identiteit-toegangstoken verkrijgen. Zie voor meer informatie over hoe u een toegangstoken verkrijgen, [over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM aan een toegangstoken verkrijgen](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>Voorbeeld van .NET-code: Een blok-blob maken

De voorbeeldcode wordt ervan uitgegaan dat u een toegangstoken beheerde identiteit hebt. Het toegangstoken wordt gebruikt voor de autorisatie van de beheerde identiteit om te maken van een blok-blob.

### <a name="add-references-and-using-statements"></a>Verwijzingen toevoegen en met behulp van instructies  

In Visual Studio, installeert u de Azure Storage-clientbibliotheek. Uit de **extra** in het menu **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**. Typ de volgende opdracht uit in de console:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage  
```

Voeg de volgende using-instructies toe aan uw code:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-managed-identity-access-token"></a>Referenties maken van het toegangstoken beheerde identiteit

Gebruik voor het maken van de blok-blob het **TokenCredentials** klasse. Maken van een nieuw exemplaar van **TokenCredentials**aan te in het toegangstoken voor beheerde identiteit die u eerder hebt verkregen:

```dotnet
// Create storage credentials from your managed identity access token.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Azure AD-integratie met Azure Storage is vereist dat u HTTPS voor Azure Storage-bewerkingen gebruiken.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
- Zie voor informatie over het aanmelden bij Azure CLI en PowerShell met een Azure AD-identiteit, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell](storage-auth-aad-script.md).
