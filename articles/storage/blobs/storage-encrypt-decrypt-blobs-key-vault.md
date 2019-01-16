---
title: 'Zelfstudie: Blobs in Azure Storage met behulp van Azure Key Vault versleutelen en ontsleutelen | Microsoft Docs'
description: Het versleutelen en ontsleutelen van een blob met behulp van versleuteling op de client voor Microsoft Azure Storage met Azure Key Vault.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: tamram
ms.component: blobs
ms.openlocfilehash: 213190863702ec5a7f2ae764c8e2d892764740f9
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332197"
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Zelfstudie: Blobs in Microsoft Azure Storage met behulp van Azure Key Vault versleutelen en ontsleutelen
## <a name="introduction"></a>Inleiding
Deze zelfstudie wordt uitgelegd hoe u het gebruik van versleuteling van de client-side-opslag met Azure Key Vault. Dit helpt u bij het versleutelen en ontsleutelen van een blob in een consoletoepassing met behulp van deze technologieën.

**Geschatte tijdsduur:** 20 minuten

Zie voor informatie over Azure Key Vault, [wat is Azure Key Vault?](../../key-vault/key-vault-whatis.md).

Zie voor informatie over client-side encryption voor Azure Storage, [Client-Side-versleuteling en Microsoft Azure Storage in Azure Key Vault](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een Azure Storage-account
* Visual Studio 2013 of hoger
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>Overzicht van de client-side-versleuteling
Zie voor een overzicht van de client-side encryption voor Azure Storage, [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Hier volgt een korte beschrijving van de werking van versleuteling aan de clientzijde:

1. De Azure Storage client-SDK genereert een sleutel met versleuteling van inhoud (CEK), dit een symmetrische sleutel van één permanente gebruiken is.
2. Gegevens van de klant is versleuteld met behulp van deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met behulp van de sleutel van versleutelingssleutel (KEK-sleutel). De KEK kan wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden lokaal beheerd of die zijn opgeslagen in Azure Key Vault. De opslag-client zichzelf heeft nooit toegang tot de KEK-sleutel. Deze roept alleen de belangrijkste wrapping-algoritme die wordt geleverd door Key Vault. Klanten kunnen kiezen voor aangepaste providers voor sleutel verpakken/uitpakken als ze willen gebruiken.
4. De versleutelde gegevens wordt vervolgens naar de Azure Storage-service geüpload.

## <a name="set-up-your-azure-key-vault"></a>Uw Azure Key Vault instellen
Om door te gaan met deze zelfstudie, moet u de volgende stappen uitvoert, worden beschreven in de zelfstudie [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md):

* Een sleutelkluis maken.
* Een sleutel of geheim toevoegen aan de sleutelkluis.
* Een toepassing registreren met Azure Active Directory.
* Toestaan dat de toepassing de sleutel of geheim te gebruiken.

Noteer de ClientID en ClientSecret die zijn gegenereerd tijdens het registreren van een toepassing met Azure Active Directory.

Beide sleutels maken in de key vault. We ervan uitgaan dat voor de rest van de zelfstudie die u hebt de volgende namen hebben gebruikt: ContosoKeyVault en TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Maak een consoletoepassing met pakketten en AppSettings
Maak een nieuwe consoletoepassing in Visual Studio.

Vereiste nuget-pakketten in de Package Manager-Console toevoegen.

```
Install-Package WindowsAzure.Storage
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

AppSettings aan het App.Config toevoegen.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Voeg de volgende `using` richtlijnen en zorg ervoor dat u een verwijzing naar de System.Configuration toevoegen aan het project.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;        
using System.IO;
```

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Voeg een methode om een token aan uw consoletoepassing toe
De volgende methode wordt gebruikt door Key Vault-klassen die u nodig hebt om te verifiëren voor toegang tot uw key vault.

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a>Toegang tot opslag en Key Vault in uw programma
Voeg de volgende code in de Main-functie.

```csharp
// This is standard code to interact with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// The Resolver object is used to interact with Key Vault for Azure Storage.
// This is where the GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> Key Vault objectmodellen
> 
> Het is belangrijk te weten dat er eigenlijk twee Key Vault objectmodellen rekening mee moet houden: een is gebaseerd op de REST-API (KeyVault-naamruimte) en de andere is een uitbreiding voor client-side-versleuteling.
> 
> De Key Vault-Client communiceert met de REST-API en JSON Web sleutels en geheimen voor de twee typen objecten die zijn opgenomen in Key Vault begrijpt.
> 
> De Key Vault-extensies zijn klassen die lijken speciaal gemaakt voor client-side-versleuteling in Azure Storage. Ze bevatten een interface voor sleutels (IKey) en klassen die is gebaseerd op het concept van een sleutel-Resolver. Er zijn twee implementaties van IKey die u moet weten: RSAKey en SymmetricKey. Nu ze overeenkomen met de dingen die zijn opgenomen in een Key Vault optreden, maar op dit moment ze onafhankelijk klassen zijn (zodat de sleutel en het geheim dat is opgehaald door de Key Vault-Client niet alle IKey).
> 
> 

## <a name="encrypt-blob-and-upload"></a>Blob Versleutel en upload
Voeg de volgende code voor het versleutelen van een blob en upload het naar uw Azure storage-account. De **ResolveKeyAsync** methode die wordt gebruikt als resultaat een IKey.

```csharp
// Retrieve the key that you created previously.
// The IKey that is returned here is an RsaKey.
// Remember that we used the names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using the UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

> [!NOTE]
> Als u de constructor BlobEncryptionPolicy bekijkt, ziet u dat het een sleutel en/of een resolver kunt accepteren. Let erop dat op dit moment kunt u een conflictoplosser voor versleuteling heeft momenteel geen ondersteuning voor een standaard-sleutel.
> 
> 

## <a name="decrypt-blob-and-download"></a>Blob ontsleutelen en te downloaden
Ontsleuteling is in feite wanneer met behulp van de klassen conflictoplosser zinvol zijn. De ID van de sleutel die wordt gebruikt voor versleuteling is gekoppeld aan de blob in de metagegevens, dus er is geen reden voor u kunt de sleutel ophalen en vergeet niet dat de koppeling tussen het sleutel- en blob. U hoeft alleen om ervoor te zorgen dat de sleutel in Key Vault blijft.   

De persoonlijke sleutel van een RSA-sleutel blijft in Key Vault, dus voor ontsleuteling optreden, de versleutelde sleutel van de metagegevens van de blob met de CEK worden verzonden naar de Key Vault voor ontsleuteling.

Voeg de volgende voor het ontsleutelen van de blob die u net hebt geüpload.

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Er zijn een aantal andere soorten resolvers om eenvoudiger sleutelbeheer, met inbegrip van: AggregateKeyResolver en CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Gebruik Key Vault-geheimen
De manier waarop het gebruik van een geheim met client-side encryption is via de klasse SymmetricKey omdat een geheim in feite een symmetrische sleutel is. Maar zoals eerder vermeld, een geheim in Key Vault niet kan worden toegewezen aan een SymmetricKey precies. Er zijn een paar dingen om te begrijpen:

* De sleutel in een SymmetricKey heeft een vaste lengte zijn: 128, 192, 256, 384 of 512 bits.
* De sleutel in een SymmetricKey moet Base64-gecodeerd.
* Een Key Vault-geheim dat wordt gebruikt als een SymmetricKey moet een inhoudstype van 'application/octet-stream' in Key Vault.

Hier volgt een voorbeeld in PowerShell voor het maken van een geheim in de Sleutelkluis die kunnen worden gebruikt als een SymmetricKey.
Houd er rekening mee dat de vastgelegde waarde, $key, voor demonstratiedoeleinden doel alleen is. In uw eigen code moet u deze sleutel te genereren.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

In uw consoletoepassing, kunt u dezelfde aanroep als voordat dit geheim als een SymmetricKey ophalen.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
Dat is alles. Veel plezier!

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van Microsoft Azure Storage met C# [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Zie voor meer informatie over de REST-API voor Blob [REST API voor Blob Service](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Voor de meest recente informatie over Microsoft Azure Storage, gaat u naar de [Blog van Microsoft Azure Storage-Team](https://blogs.msdn.com/b/windowsazurestorage/).
