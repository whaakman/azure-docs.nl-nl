---
title: Verifiëren met Azure AD vanuit een Azure-VM beheerde Service-identiteit (Preview) | Microsoft Docs
description: Verifiëren met Azure AD vanuit een Azure-VM beheerde Service-identiteit (Preview).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: e20e0c412206b2a35973b192ef911bb99ed7c210
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44021860"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Verifiëren met Azure AD vanuit een Azure beheerde Service-identiteit (Preview)

Azure Storage biedt ondersteuning voor verificatie met Azure Active Directory (Azure AD) [beheerde Service-identiteit](../../active-directory/managed-identities-azure-resources/overview.md). Beheerde Service Identity (MSI) biedt een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt MSI gebruiken voor verificatie op Azure Storage vanuit toepassingen die worden uitgevoerd in virtuele machines van Azure, functie-apps, virtuele-machineschaalsets en anderen. Met behulp van MSI en gebruik te maken van de kracht van Azure AD-verificatie, kunt u voorkomen dat opslaan van referenties op met uw toepassingen die worden uitgevoerd in de cloud.  

Om machtigingen te verlenen aan een beheerde service-identiteit voor storage-containers of wachtrijen, kunt u een opslagmachtigingen voor het MSI-bestand dat RBAC-rol toewijzen. Zie voor meer informatie over RBAC-rollen in de opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> In dit voorbeeld is bedoeld voor gebruik in niet-productieomgevingen alleen. Productie service level agreements (Sla's) worden pas beschikbaar als Azure AD-integratie voor Azure Storage algemeen beschikbaar is gedeclareerd. Als Azure AD-integratie wordt nog niet ondersteund voor uw scenario, echter ook doorgaan met de gedeelde sleutel autorisatie of SAS-tokens in uw toepassingen. Zie voor meer informatie over de Preview-versie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory (Preview)](storage-auth-aad.md).
>
> Tijdens de Preview-versie duurt RBAC-roltoewijzingen tot vijf minuten worden doorgegeven.

In dit artikel laat zien hoe om te verifiëren met Azure Storage met MSI-bestand van een Azure-VM.  

## <a name="enable-msi-on-the-vm"></a>MSI-bestand op de virtuele machine inschakelen

Voordat u MSI gebruiken kunt voor verificatie op Azure Storage vanuit uw virtuele machine, moet u eerst de MSI-bestand op de virtuele machine inschakelen. Zie voor informatie over het inschakelen van MSI-bestand, een van de volgende artikelen:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjabloon](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDK 's](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>Een MSI toegang-token ophalen

Als u wilt verifiëren met MSI-bestand, moet uw toepassing of script een MSI-toegangstoken verkrijgen. Zie voor meer informatie over hoe u een toegangstoken verkrijgen, [over het gebruik van een Azure VM Managed Service Identity (MSI) voor het ophalen van tokens](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>.NET-codevoorbeeld: maken van een blok-blob

De voorbeeldcode wordt ervan uitgegaan dat u een MSI-toegangstoken hebt. Het toegangstoken wordt gebruikt voor de autorisatie van de beheerde service-identiteit voor het maken van een blok-blob.

### <a name="add-references-and-using-statements"></a>Verwijzingen toevoegen en met behulp van instructies  

In Visual Studio, installeert u de preview-versie van de Azure Storage-clientbibliotheek. Uit de **extra** in het menu **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**. Typ de volgende opdracht uit in de console:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Voeg de volgende using-instructies toe aan uw code:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Maken van referenties uit het MSI-toegangstoken

Gebruik voor het maken van de blok-blob het **TokenCredentials** klasse geleverd door de preview-pakket. Maken van een nieuw exemplaar van **TokenCredentials**aan te in het MSI-toegangstoken die u eerder hebt verkregen:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Azure AD-integratie met Azure Storage is vereist dat u HTTPS voor Azure Storage-bewerkingen gebruiken.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
- Als u wilt weten hoe u zich aanmeldt bij Azure CLI en PowerShell met een Azure AD-identiteit, Zie [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure-Blobs en wachtrijen, de blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
