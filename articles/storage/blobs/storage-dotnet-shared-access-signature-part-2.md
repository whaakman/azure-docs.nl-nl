---
title: Maken en een shared access signature (SAS) gebruiken met Azure Blob-opslag | Microsoft Docs
description: Deze zelfstudie leert u over het maken van handtekeningen voor gedeelde toegang voor gebruik met Blob-opslag, en u deze consumeert in uw client-toepassingen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.devlang: dotnet
ms.date: 05/15/2017
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 5dcb9f16b589b8332d5fcf35c9d8b4cd914460f2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58012543"
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Shared Access Signatures, deel 2: Maken en een SAS gebruiken met Blob-opslag

[Deel 1](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) shared access signatures (SAS) van deze zelfstudie verkend en beschreven aanbevolen procedures voor het gebruik ervan. Deel 2 ziet u hoe te genereren en vervolgens handtekeningen voor gedeelde toegang gebruiken met Blob-opslag. De voorbeelden zijn geschreven in C# en de Azure Storage-clientbibliotheek voor .NET gebruiken. De voorbeelden in deze zelfstudie:

* Genereren van een shared access signature voor een container
* Genereren van een shared access signature voor een blob
* Een opgeslagen toegangsbeleid voor het beheren van handtekeningen voor resources van een container maken
* Testen van de handtekeningen voor gedeelde toegang in een clienttoepassing

## <a name="about-this-tutorial"></a>Over deze zelfstudie
In deze zelfstudie maken we twee consoletoepassingen die laten zien van het maken en gebruiken van handtekeningen voor gedeelde toegang voor containers en blobs:

**Toepassing 1**: De management-toepassing. Genereert een shared access signature voor een container en een blob. De toegangssleutel voor opslagaccount bevat in de broncode.

**Toepassing 2**: De clienttoepassing. Toegang tot container- en blobnaam resources met behulp van de handtekeningen voor gedeelde toegang gemaakt met de eerste toepassing. Maakt gebruik van alleen de handtekeningen voor gedeelde toegang tot toegang tot container en blobbronnen--doet *niet* de toegangssleutel voor opslagaccount bevatten.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Deel 1: Maak een consoletoepassing voor het genereren van handtekeningen voor gedeelde toegang
Eerst voor zorgen dat u de Azure Storage-clientbibliotheek voor .NET geïnstalleerd hebt. U kunt installeren de [NuGet-pakket](https://nuget.org/packages/WindowsAzure.Storage/ "NuGet-pakket") met de meest recente assembly's voor de clientbibliotheek. Dit is de aanbevolen methode om ervoor te zorgen dat u de meest recente oplossingen hebt. U kunt ook de clientbibliotheek downloaden als onderdeel van de meest recente versie van de [Azure SDK voor .NET](https://azure.microsoft.com/downloads/).

Maak een nieuwe Windows-consoletoepassing in Visual Studio en noem dit **GenerateSharedAccessSignatures**. Verwijzingen toevoegen aan [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) en [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) met behulp van een van de volgende methoden:

* Gebruik de [NuGet-Pakketbeheer](https://docs.nuget.org/consume/installing-nuget) in Visual Studio. Selecteer **Project** > **NuGet-pakketten beheren**, zoek online naar elk pakket (Microsoft.WindowsAzure.ConfigurationManager en WindowsAzure.Storage) en deze installeren.
* U kunt ook deze assembly's in de installatie van de Azure SDK en verwijzingen naar deze toevoegen:
  * Microsoft.WindowsAzure.Configuration.dll
  * Microsoft.WindowsAzure.Storage.dll

Voeg het volgende toe aan de bovenkant van het bestand Program.cs **met behulp van** richtlijnen:

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Bewerk het bestand app.config zodat deze een configuratie-instelling met een verbindingsreeks die naar uw storage-account verwijst bevat. Het bestand app.config moet op deze als volgt uitzien:

```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
  </appSettings>
</configuration>
```

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Genereren van een shared access signature URI voor een container
Begint met, voegen we een methode voor het genereren van een shared access signature voor een nieuwe container. De handtekening is in dit geval niet gekoppeld aan een opgeslagen toegangsbeleid, zodat zij werkzaam in de URI de informatie die de verlooptijd en de machtigingen die hij verleent aangeeft.

Eerst, voeg code toe aan de **Main()** methode voor het machtigen van toegang tot uw storage-account en een nieuwe container maken:

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls to the methods created below here...

    //Require user input before closing the console window.
    Console.ReadLine();
}
```

Vervolgens voegt u een methode die de shared access signature voor de container genereert en retourneert de handtekening URI toe:

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set the expiry time and permissions for the container.
    //In this case no start time is specified, so the shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

Voeg de volgende regels toe aan de onderkant van de **Main()** voordat de aanroep van methode **Console.ReadLine()**, om aan te roepen **GetContainerSasUri()** en schrijven van de handtekening URI naar de consolevenster:

```csharp
//Generate a SAS URI for the container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

Compileren en uitvoeren als u wilt uitvoeren van de shared access signature URI voor de nieuwe container. De URI zijn die vergelijkbaar is met het volgende:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

Nadat u de code hebt uitgevoerd, zijn de shared access signature die u hebt gemaakt voor de container zijn geldig voor de volgende 24 uur. De handtekening een client een machtiging verleend voor blobs weergeven in de container en het schrijven van nieuwe blobs naar de container.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Genereren van een shared access signature URI voor een blob
Vervolgens schrijven we met vergelijkbare code voor het maken van een nieuwe blob in de container en een shared access signature genereren voor het. Deze shared access signature is niet gekoppeld aan een opgeslagen toegangsbeleid, zodat deze de begintijd, verlooptijd en machtigingsinformatie in de URI bevat.

Voeg een nieuwe methode die wordt een nieuwe blob gemaakt en geschreven tekst, en vervolgens genereert een shared access signature en retourneert de handtekening URI toe:

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set the expiry time and permissions for the blob.
    //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
    //The shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

Aan de onderkant van de **Main()** methode, voeg de volgende regels om aan te roepen **GetBlobSasUri()**, voordat de aanroep van **Console.ReadLine()**, en de shared access signature schrijven De URI in het consolevenster:

```csharp
//Generate a SAS URI for a blob within the container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

Compileren en uitvoeren als u wilt uitvoeren van de shared access signature URI voor de nieuwe blob. De URI zijn die vergelijkbaar is met het volgende:

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-the-container"></a>Een opgeslagen toegangsbeleid voor de container maken
Nu gaan we een opgeslagen toegangsbeleid maken op de container, zodat de beperkingen voor alle handtekeningen voor gedeelde toegang die gekoppeld aan deze zijn definieert.

In de vorige voorbeelden, we hebben opgegeven de begintijd (impliciet of expliciet), de verlooptijd en de machtigingen op de shared access signature URI zelf. In de volgende voorbeelden Geef deze op het opgeslagen toegangsbeleid en niet volgens de shared access signature. In dat geval kan we deze beperkingen zonder opnieuw uitgeven van de shared access signature worden gewijzigd.

Het is mogelijk dat een of meer van de beperkingen met betrekking tot de shared access signature en de rest van de opgeslagen toegangsbeleid. U kunt de begintijd, verlooptijd en machtigingen alleen opgeven in één locatie of in de andere. U kunt geen bijvoorbeeld machtigingen opgeven op de shared access signature en geef ze ook op het opgeslagen toegangsbeleid.

Wanneer u een opgeslagen toegangsbeleid aan een container toevoegen, moet u de bestaande machtigingen van de container ophalen, de nieuw toegangsbeleid toevoegen en vervolgens machtigingen van de container instellen.

Voeg een nieuwe methode die u maakt een nieuwe opgeslagen toegangsbeleid in een container en retourneert de naam van het beleid toe:

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get the container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

Aan de onderkant van de **Main()** voordat de aanroep van methode **Console.ReadLine()**, voeg de volgende regels om te wissen eerste alle bestaande beleidsregels voor toegang en roep vervolgens de  **CreateSharedAccessPolicy()** methode:

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on the container, which may be optionally used to provide constraints for
//shared access signatures on the container and the blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

Wanneer u het toegangsbeleid voor een container wist, moet u eerst de bestaande machtigingen van de container, schakelt u de machtigingen vervolgens, de machtigingen opnieuw instellen.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Een shared access signature URI voor de container die gebruikmaakt van een toegangsbeleid genereren
Vervolgens maken we een andere shared access signature voor de container die we eerder, maar deze keer die we de handtekening koppelen aan het opgeslagen toegangsbeleid dat wordt gemaakt in het vorige voorbeeld gemaakt.

Voeg een nieuwe methode voor het genereren van een andere shared access signature voor de container toe:

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate the shared access signature on the container. In this case, all of the constraints for the
    //shared access signature are specified on the stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

Aan de onderkant van de **Main()** voordat de aanroep van methode **Console.ReadLine()**, voeg de volgende regels om aan te roepen de **GetContainerSasUriWithPolicy** methode:

```csharp
//Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Een Shared Access Signature URI in de Blob die gebruikmaakt van een toegangsbeleid genereren
Ten slotte toevoegen we een vergelijkbare manier voor het maken van een andere blob en genereren van een shared access signature die is gekoppeld aan een opgeslagen toegangsbeleid.

Voeg een nieuwe methode voor het maken van een blob en een shared access signature genereren:

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature. " +
    "A stored access policy defines the constraints for the signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate the shared access signature on the blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

Aan de onderkant van de **Main()** voordat de aanroep van methode **Console.ReadLine()**, voeg de volgende regels om aan te roepen de **GetBlobSasUriWithPolicy** methode:

```csharp
//Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

De **Main()** methode moet er nu uitzien als volgt in zijn geheel toe. Uitvoeren om te schrijven van de shared access signature URI's in het consolevenster, kopieer en plak deze in een tekstbestand voor gebruik in het tweede gedeelte van deze zelfstudie.

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

Wanneer u de GenerateSharedAccessSignatures consoletoepassing uitvoert, ziet u uitvoer die vergelijkbaar is met de volgende. Dit zijn de handtekeningen voor gedeelde toegang u in deel 2 van de zelfstudie.

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Deel 2: Maak een consoletoepassing voor het testen van de handtekeningen voor gedeelde toegang
Als u wilt testen van de handtekeningen voor gedeelde toegang gemaakt in de vorige voorbeelden, maken we een tweede consoletoepassing die gebruikmaakt van de handtekeningen bewerkingen uit te voeren op de container en op een blob.

> [!NOTE]
> Als er meer dan 24 uur zijn verstreken nadat u het eerste deel van de zelfstudie hebt voltooid, is de handtekeningen die u hebt gegenereerd niet langer geldig. In dit geval moet u de code uitvoeren in de eerste consoletoepassing voor het genereren van nieuwe gedeelde toegangshandtekeningen voor gebruik in het tweede gedeelte van de zelfstudie.
>

Maak een nieuwe Windows-consoletoepassing in Visual Studio en noem dit **ConsumeSharedAccessSignatures**. Verwijzingen toevoegen aan [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) en [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), zoals u eerder hebt gedaan.

Voeg het volgende toe aan de bovenkant van het bestand Program.cs **met behulp van** richtlijnen:

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

In de hoofdtekst van de **Main()** methode, voeg de volgende tekenreeks constanten toe, wijzigen de waarden in de handtekeningen voor gedeelde toegang die u in deel 1 van de zelfstudie.

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Een methode om te proberen met behulp van een shared access signature containers toevoegen
Vervolgens voegt we een methode die een bepaalde container-bewerkingen met behulp van een shared access signature voor de container wordt getest. De shared access signature wordt gebruikt om een verwijzing naar de container, het verifiëren van toegang tot de container op basis van de handtekening alleen retourneert.

Voeg de volgende methode toe aan Program.cs:

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with the SAS provided.

    //Return a reference to the container using the SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list to store blob URIs returned by a listing operation on the container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob to the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
        blob.UploadText(blobContent);

        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //List operation: List the blobs in the container.
    try
    {
        foreach (ICloudBlob blob in container.ListBlobs())
        {
            blobList.Add(blob);
        }
        Console.WriteLine("List operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("List operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Get a reference to one of the blobs in the container and read it.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        MemoryStream msRead = new MemoryStream();
        msRead.Position = 0;
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            Console.WriteLine(msRead.Length);
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    Console.WriteLine();

    //Delete operation: Delete a blob in the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

Update de **Main()** methode om aan te roepen **UseContainerSAS()** met beide van de handtekeningen voor gedeelde toegang hebt gemaakt op de container:

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Een methode om te proberen de blob-bewerkingen met behulp van een shared access signature toevoegen
Ten slotte toevoegen we een methode die bepaalde blob-bewerkingen met behulp van een shared access signature voor de blob test. In dit geval gebruiken we de constructor **CloudBlockBlob(String)** aan te in de shared access signature, om terug te keren naar de blob verwijst. Er is geen andere verificatie vereist. deze gebaseerd op de handtekening alleen.

Voeg de volgende methode toe aan Program.cs:

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using the SAS provided.

    //Return a reference to the blob using the SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob to the container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            blob.UploadFromStream(msWrite);
        }
        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Read the contents of the blob.
    try
    {
        MemoryStream msRead = new MemoryStream();
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            msRead.Position = 0;
            using (StreamReader reader = new StreamReader(msRead, true))
            {
                string line;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Delete operation: Delete the blob.
    try
    {
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

Update de **Main()** methode om aan te roepen **UseBlobSAS()** met beide van de handtekeningen voor gedeelde toegang die u hebt gemaakt in de blob:

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

Voer de consoletoepassing uit en bekijk de uitvoer om te zien welke bewerkingen zijn toegestaan voor welke handtekeningen. De uitvoer in het consolevenster ziet er ongeveer als volgt:

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a>Volgende stappen

* [Shared Access Signatures, deel 1: Inzicht in het SAS-Model](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Anonieme leestoegang tot containers en blobs beheren](storage-manage-access-to-resources.md)
* [Toegang delegeren met een shared access signature (REST-API)](https://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Maak kennis met de tabel en wachtrij SAS](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
