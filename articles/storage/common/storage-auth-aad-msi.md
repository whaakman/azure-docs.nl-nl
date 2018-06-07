---
title: Verificatie met Azure AD van een Azure VM beheerde Service-identiteit (Preview) | Microsoft Docs
description: Verificatie met Azure AD van een Azure VM beheerde Service-identiteit (Preview).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 83d3a2d973604e3b8a709b24cabcb3abba1e304c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660801"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Verifiëren met Azure AD vanaf een beheerde Azure-Service-identiteit (Preview)

Azure Storage ondersteunt verificatie met Azure Active Directory (Azure AD) [beheerde Service-identiteit](../../active-directory/managed-service-identity/overview.md). Beheerde Service-identiteit (MSI) biedt een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt MSI gebruiken om te verifiëren naar Azure Storage van toepassingen in een andere virtuele machines in Azure, functie apps en virtuele-machineschaalsets. Door MSI en gebruik van de mogelijkheden van Azure AD-verificatie, kunt u voorkomen dat opslaan van referenties met uw toepassingen die worden uitgevoerd in de cloud.  

Om machtigingen te verlenen aan een beheerde service-identiteit voor storage-containers of wachtrijen, moet u een opslagmachtigingen voor het MSI-bestand dat omvat RBAC-rol toewijzen. Zie voor meer informatie over RBAC-rollen in de opslag [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Deze preview is bedoeld voor gebruik van niet-productieve alleen. Productie service level agreements (Sla's) worden pas beschikbaar Azure AD-integratie voor Azure Storage is gedeclareerd in het algemeen beschikbaar. Als u Azure AD-integratie is nog niet ondersteund voor uw scenario, blijven de gedeelde sleutel autorisatie of SAS-tokens gebruiken in uw toepassingen. Zie voor meer informatie over de evaluatieversie [verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview)](storage-auth-aad.md).
>
> Tijdens de preview kunnen de RBAC-roltoewijzingen doorgeven maximaal vijf minuten duren.

In dit artikel laat zien hoe te verifiëren bij Azure Storage met MSI van een virtuele machine in Azure.  

## <a name="enable-msi-on-the-vm"></a>MSI op de virtuele machine inschakelen

Voordat u MSI gebruiken kunt om te verifiëren naar Azure Storage van de virtuele machine, moet u eerst MSI op de virtuele machine inschakelen. Zie voor meer informatie over het inschakelen van MSI, een van deze artikelen:

- [Azure Portal](https://docs.microsoft.com/en-us/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-service-identity/qs-configure-powershell-windows-vm.md)
- [Azure-CLI](../../active-directory/managed-service-identity/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjabloon](../../active-directory/managed-service-identity/qs-configure-template-windows-vm.md)
- [Azure-SDK 's](../../active-directory/managed-service-identity/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>De toegang van een MSI-token ophalen

Om te verifiëren met MSI, moet uw toepassing of het script een MSI-toegangstoken verkrijgen. Zie voor meer informatie over het verkrijgen van een toegangstoken, [het gebruik van een Azure VM beheerde Service identiteit (MSI) voor de aanschaf van token](../../active-directory/managed-service-identity/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>.NET-codevoorbeeld: maken van een blok-blob

In het voorbeeld wordt ervan uitgegaan dat u een MSI-toegangstoken hebt. Het toegangstoken wordt gebruikt voor het autoriseren van de identiteit van de beheerde service voor het maken van een blok-blob.

### <a name="add-references-and-using-statements"></a>Voeg verwijzingen toe en using-instructies  

In Visual Studio, installeert u de preview-versie van de Azure Storage-clientbibliotheek. Van de **extra** selecteert u **Nuget Package Manager**, klikt u vervolgens **Package Manager Console**. Typ de volgende opdracht in de console:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Voeg de volgende using-instructies toe aan uw code:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Referenties van het MSI-toegangstoken maken

Voor het maken van het blok-blob gebruikt de **TokenCredentials** class van de preview-pakket. Maken van een nieuw exemplaar van **TokenCredentials**doorgegeven in het MSI-toegangstoken die u eerder hebt verkregen:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Azure AD-integratie met Azure Storage vereist het gebruik van HTTPS voor Azure Storage-bewerkingen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de rollen RBAC voor Azure-opslag, [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).
- Zie voor meer informatie over hoe u toegang verlenen aan containers en wachtrijen van binnen uw toepassingen moeten worden opgeslagen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over hoe u aan te melden bij Azure CLI en PowerShell met een Azure AD-identiteit, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure Blobs en wachtrijen, het Azure Storage-team blogbericht, [aankondigen van de Preview van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
