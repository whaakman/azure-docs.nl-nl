---
title: 'Zelfstudie: Blobs versleutelen en ontsleutelen in Azure Storage met Azure Sleutelkluis | Microsoft Docs'
description: Het versleutelen en ontsleutelen van een blob met client-side '-codering voor Microsoft Azure Storage met Azure Sleutelkluis.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: adhurwit
ms.openlocfilehash: 405ccb44c9daf8d555946e6c68ef318ed2b82505
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Zelfstudie: Blobs versleutelen en ontsleutelen in Microsoft Azure Storage met Azure Sleutelkluis
## <a name="introduction"></a>Inleiding
Deze zelfstudie wordt beschreven hoe u het gebruik van de versleuteling van de client-side '-opslag met Azure Sleutelkluis. Dit helpt u bij het versleutelen en ontsleutelen van een blob in een consoletoepassing met behulp van deze technologieën.

**Geschatte duur:** 20 minuten

Zie voor informatie over Azure Sleutelkluis [wat is Azure Sleutelkluis?](../../key-vault/key-vault-whatis.md).

Zie voor informatie over client-side '-versleuteling voor Azure Storage, [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="prerequisites"></a>Vereisten
U hebt het volgende nodig om deze zelfstudie te voltooien:

* Een Azure Storage-account
* Visual Studio 2013 of hoger
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>Overzicht van client-side '-versleuteling
Zie voor een overzicht van client-side '-versleuteling voor Azure Storage [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Hier volgt een korte beschrijving van de werking van versleuteling aan de clientzijde:

1. De client-SDK van Azure Storage genereert een inhoud versleutelingssleutel (CEK), die een symmetrische sleutel met een eenmalig gebruik is.
2. Gegevens van de klant is versleuteld met behulp van deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met behulp van de belangrijkste versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden beheerd lokaal of opgeslagen in Azure Sleutelkluis. De opslag-client zichzelf heeft nooit toegang tot de KEK. Het aanroept zojuist de belangrijkste wrapping algoritme die wordt geleverd door Sleutelkluis. Klanten kunnen kiezen voor aangepaste providers voor sleutel wrapping/uitpakken als ze willen gebruiken.
4. De versleutelde gegevens is vervolgens naar de Azure Storage-service geüpload.

## <a name="set-up-your-azure-key-vault"></a>Instellen van uw Azure Sleutelkluis
Om verder te gaan met deze zelfstudie moet u de volgende stappen uitvoert, worden beschreven in de zelfstudie [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md):

* Een sleutelkluis maken.
* Een sleutel of geheim toevoegen aan de sleutelkluis.
* Registreert een toepassing met Azure Active Directory.
* De autorisatie voor de toepassing sleutel of geheim te gebruiken.

Noteer de ClientID en ClientSecret die zijn gegenereerd tijdens het registreren van een toepassing met Azure Active Directory.

Beide sleutels maken in de sleutelkluis. We ervan uitgaan dat voor de rest van de zelfstudie of u de volgende namen gebruikt: ContosoKeyVault en TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Maak een consoletoepassing met pakketten en AppSettings
Maak een nieuwe consoletoepassing in Visual Studio.

Benodigde nuget-pakketten in de Package Manager-Console toevoegen.

```
Install-Package WindowsAzure.Storage

// This is the latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

AppSettings toevoegen aan de App.Config.

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

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Een methode om een token aan uw consoletoepassing toevoegen
De volgende methode wordt gebruikt door de Sleutelkluis-klassen die nodig zijn om te verifiëren voor toegang tot de sleutelkluis.

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

## <a name="access-storage-and-key-vault-in-your-program"></a>Toegang tot opslag en Sleutelkluis in uw programma
Voeg de volgende code in de functie Main.

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
> Netwerkobjectmodellen voor Sleutelkluis
> 
> Het is belangrijk te weten dat er daadwerkelijk twee Sleutelkluis objectmodellen rekening houden met zijn: een is gebaseerd op de REST-API (KeyVault-naamruimte) en de andere is een uitbreiding voor client-side '-versleuteling.
> 
> De Sleutelkluis Client communiceert met de REST-API en JSON Web sleutels en geheimen voor de twee soorten bewerkingen die zijn opgenomen in de Sleutelkluis begrijpt.
> 
> De Sleutelkluis extensies zijn klassen die lijken speciaal voor client-side '-versleuteling in Azure Storage. Ze bevatten een interface voor sleutels (IKey) en klassen op basis van het concept van een sleutel-Resolver. Er zijn twee implementaties van IKey die u nodig hebt: RSAKey en SymmetricKey. Nu ze overeenkomen met de bewerkingen die zijn opgenomen in een Sleutelkluis gebeuren op dit moment zijn echter onafhankelijke klassen (zodat de sleutel en het geheim dat is opgehaald door de kluis sleutel Client niet IKey implementeert).
> 
> 

## <a name="encrypt-blob-and-upload"></a>BLOBs versleutelen en uploaden
Voeg de volgende code voor het versleutelen van een blob en upload het naar uw Azure storage-account. De **ResolveKeyAsync** retourneert een IKey methode die wordt gebruikt.

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
> Als u de constructor BlobEncryptionPolicy bekijkt, ziet u dat deze een sleutel-en/of een resolver kan accepteren. Let die op dit moment kunt u een conflictoplosser voor versleuteling heeft momenteel geen ondersteuning bieden voor een standaardsleutel.
> 
> 

## <a name="decrypt-blob-and-download"></a>Blob ontsleutelen en te downloaden
Ontsleuteling is in feite wanneer met de omzetter klassen zinvol zijn. De ID van de sleutel die wordt gebruikt voor versleuteling is gekoppeld aan de blob in de metagegevens, zodat er geen reden voor het ophalen van de sleutel en de koppeling tussen sleutel en blob onthouden. U hoeft alleen om ervoor te zorgen dat de sleutel in de Sleutelkluis blijft.   

De persoonlijke sleutel van een RSA-sleutel blijft in de Sleutelkluis, zodat voor ontsleuteling om te worden uitgevoerd, wordt de versleutelde sleutel van de blobmetagegevens van de die het CEK bevat naar Sleutelkluis voor ontsleuteling verzonden.

Voeg de volgende voor het ontsleutelen van de blob die u zojuist hebt geüpload.

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Er zijn een aantal andere soorten resolvers Sleutelbeheer om gemakkelijker te maken, met inbegrip van: AggregateKeyResolver en CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Gebruik Sleutelkluis geheimen
De manier om het gebruik van een geheim met versleuteling aan clientzijde is via de klasse SymmetricKey omdat een geheim in wezen een symmetrische sleutel is. Maar zoals eerder vermeld, een geheim in de Sleutelkluis niet wordt toegewezen aan een SymmetricKey exact. Er zijn een aantal dingen te begrijpen:

* De sleutel in een SymmetricKey moet een vaste lengte: 128, 192, 256, 384 of 512 bits.
* De sleutel in een SymmetricKey moet Base64-gecodeerd.
* Een Sleutelkluis geheim dat wordt gebruikt als een SymmetricKey moet een Type inhoud van 'application/octet-stream' in de Sleutelkluis.

Hier volgt een voorbeeld in PowerShell voor het maken van een geheim in de Sleutelkluis die kunnen worden gebruikt als een SymmetricKey.
Houd er rekening mee dat de vastgelegde waarde, $key, alleen daartoe demonstratie is. In uw eigen code moet u deze sleutel te genereren.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

U kunt dezelfde aanroep als voordat dit geheim als een SymmetricKey ophalen gebruiken in uw consoletoepassing.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
Dat is alles. Veel plezier!

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van Microsoft Azure Storage met C# [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Zie voor meer informatie over de REST-API van Blob [REST-API van Blob](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Voor de nieuwste informatie over Microsoft Azure Storage, gaat u naar de [Blog van Microsoft Azure Storage-Team](http://blogs.msdn.com/b/windowsazurestorage/).
