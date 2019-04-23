---
title: Verifiëren van toegang tot blobs en wachtrijen met beheerde identiteiten voor Azure-resources - Azure Storage | Microsoft Docs
description: Azure Blob- en Queue storage ondersteuning voor Azure Active Directory-verificatie met beheerde identiteiten voor Azure-resources. U kunt beheerde identiteiten voor Azure-resources gebruiken om te verifiëren van toegang tot blobs en wachtrijen van toepassingen die worden uitgevoerd in virtuele machines van Azure, functie-apps, virtuele-machineschaalsets en anderen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 9209161f9c9e34320b1388e0e1edbd5069e73727
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60148846"
---
# <a name="authenticate-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Verifiëren van toegang tot blobs en wachtrijen met Azure Active Directory en beheerde identiteiten voor Azure-Resources

Azure Blob en Queue storage ondersteuning voor verificatie met Azure Active Directory (Azure AD) [beheerde identiteiten voor een Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md). Beheerde identiteiten voor Azure-resources toegang verleent kunnen tot blob- en wachtrijgegevens met Azure AD-referenties van de toepassingen die worden uitgevoerd in virtuele Azure-machines (VM's), functie-apps, virtuele-machineschaalsets en andere services. Met behulp van beheerde identiteiten voor Azure-resources samen met Azure AD-verificatie, kunt u voorkomen dat opslaan van referenties op met uw toepassingen die worden uitgevoerd in de cloud.  

In dit artikel laat zien hoe om toegang te verlenen tot gegevens voor blob of een wachtrij met een beheerde identiteit van een Azure-VM. 

## <a name="enable-managed-identities-on-a-vm"></a>Beheerde identiteiten op een virtuele machine inschakelen

Voordat u beheerde identiteiten voor Azure-Resources gebruiken kunt om toegang tot blobs en wachtrijen van de virtuele machine te verlenen, moet u eerst beheerde identiteiten inschakelen voor Azure-Resources op de virtuele machine. Voor informatie over het inschakelen van beheerde identiteiten voor Azure-Resources, Zie een van de volgende artikelen:

- [Azure-portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjabloon](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDK 's](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-an-azure-ad-managed-identity"></a>Machtigingen toewijzen aan een Azure AD beheerde identiteit

Als u wilt toestaan dat een aanvraag naar de Blob of Queue-service van een beheerde identiteit in uw Azure Storage-toepassing, moet u eerst op basis van rollen (RBAC) instellingen voor toegangsbeheer voor die beheerde identiteit configureren. Azure Storage definieert RBAC-rollen die machtigingen voor blob- en wachtrijservices gegevens omvatten. Wanneer de RBAC-rol is toegewezen aan een beheerde identiteit, wordt de beheerde identiteit verleend aan deze machtigingen blob of een wachtrij gegevens op het juiste bereik. 

Zie een van de volgende artikelen voor meer informatie over het toewijzen van RBAC-rollen:

- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](storage-auth-aad-rbac-portal.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van Azure CLI](storage-auth-aad-rbac-cli.md)
- [Toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC met behulp van PowerShell](storage-auth-aad-rbac-powershell.md)

## <a name="authorize-with-a-managed-identity-access-token"></a>Toestaan dat met het toegangstoken van een beheerde identiteit

Om aanvragen te verifiëren op basis van Blob en Queue storage met een beheerde identiteit, moet uw toepassing of script een OAuth-token verkrijgen. De [Microsoft Azure App Authentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) -clientbibliotheek voor .NET (preview) vereenvoudigt het proces van aanschaf en het vernieuwen van een token vanuit uw code.

De App-verificatie-clientbibliotheek beheert automatisch verificatie. De bibliotheek maakt gebruik van de referenties van de ontwikkelaar om te verifiëren tijdens de lokale ontwikkeling. Met de ontwikkelaarsreferenties van tijdens het ontwikkelen van lokale is veiliger omdat u niet hoeft te maken van Azure AD-referenties of referenties tussen ontwikkelaars delen. Wanneer de oplossing later wordt geïmplementeerd naar Azure, schakelt u de bibliotheek automatisch met referenties van de toepassing.

Voor het gebruik van de App-verificatiebibliotheek in een Azure Storage-toepassing, installeert u de meest recente preview-pakket van [Nuget]((https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)), evenals de meest recente versie van de [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Voeg de volgende **met behulp van** instructies gebruikt om uw code uit:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.Auth;
```

De App-verificatiebibliotheek biedt de **AzureServiceTokenProvider** klasse. Een exemplaar van deze klasse kan worden doorgegeven aan een callbackfunctie die wordt een token opgehaald en vervolgens vernieuwt het token voordat deze verloopt.

Het volgende voorbeeld wordt een token opgehaald en gebruikt voor het maken van een nieuwe blob en vervolgens de dezelfde token wordt gebruikt om te lezen van de blob.

```csharp
const string blobName = "https://storagesamples.blob.core.windows.net/sample-container/blob1.txt";

// Get the initial access token and the interval at which to refresh it.
AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
var tokenAndFrequency = TokenRenewerAsync(azureServiceTokenProvider, 
                                            CancellationToken.None).GetAwaiter().GetResult();

// Create storage credentials using the initial token, and connect the callback function 
// to renew the token just before it expires
TokenCredential tokenCredential = new TokenCredential(tokenAndFrequency.Token, 
                                                        TokenRenewerAsync,
                                                        azureServiceTokenProvider, 
                                                        tokenAndFrequency.Frequency.Value);

StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a blob using the storage credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName), 
                                            storageCredentials);

// Upload text to the blob.
blob.UploadTextAsync(string.Format("This is a blob named {0}", blob.Name));

// Continue to make requests against Azure Storage. 
// The token is automatically refreshed as needed in the background.
do
{
    // Read blob contents
    Console.WriteLine("Time accessed: {0} Blob Content: {1}", 
                        DateTimeOffset.UtcNow, 
                        blob.DownloadTextAsync().Result);

    // Sleep for ten seconds, then read the contents of the blob again.
    Thread.Sleep(TimeSpan.FromSeconds(10));
} while (true);
```

De terugbelmethode controleert de verlooptijd van het token en vernieuwt deze indien nodig:

```csharp
private static async Task<NewTokenAndFrequency> TokenRenewerAsync(Object state, CancellationToken cancellationToken)
{
    // Specify the resource ID for requesting Azure AD tokens for Azure Storage.
    const string StorageResource = "https://storage.azure.com/";  

    // Use the same token provider to request a new token.
    var authResult = await ((AzureServiceTokenProvider)state).GetAuthenticationResultAsync(StorageResource);

    // Renew the token 5 minutes before it expires.
    var next = (authResult.ExpiresOn - DateTimeOffset.UtcNow) - TimeSpan.FromMinutes(5);
    if (next.Ticks < 0)
    {
        next = default(TimeSpan);
        Console.WriteLine("Renewing token...");
    }

    // Return the new token and the next refresh time.
    return new NewTokenAndFrequency(authResult.AccessToken, next);
}
```

Zie voor meer informatie over de App-verificatiebibliotheek [Service-naar-serviceverificatie naar Azure Key Vault met behulp van .NET](../../key-vault/service-to-service-authentication.md). 

Zie voor meer informatie over het verkrijgen van een toegangstoken, [over het gebruik van beheerde identiteiten voor Azure-resources op een Azure-VM aan een toegangstoken verkrijgen](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

> [!NOTE]
> Om aanvragen te verifiëren op basis van gegevens voor blob of een wachtrij met Azure AD, moet u HTTPS gebruiken voor deze aanvragen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC-rollen voor Azure-opslag, [beheren-toegangsrechten aan opslag van gegevens met RBAC](storage-auth-aad-rbac.md).
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met opslagtoepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over Azure CLI en PowerShell-opdrachten uitvoeren met Azure AD-referenties, [uitvoeren op Azure CLI of PowerShell-opdrachten met Azure AD-referenties voor toegang tot gegevens voor blob of een wachtrij](storage-auth-aad-script.md).