---
title: Hoe Azure Blob storage gebruiken met iOS | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
documentationcenter: ios
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: df188021-86fc-4d31-a810-1b0e7bcd814b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: objective-c
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: f238804e6031fcf3f194695a06bf5b88733a27b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-ios"></a>Het Blob storage gebruiken met iOS
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht
In dit artikel wordt beschreven hoe u veelvoorkomende scenario's met behulp van Microsoft Azure Blob-opslag uitvoeren. De voorbeelden zijn geschreven in Objective-C en gebruik de [Azure Storage-clientbibliotheek voor iOS](https://github.com/Azure/azure-storage-ios). De scenario's worden behandeld: **uploaden**, **aanbieding**, **downloaden**, en **verwijderen** blobs. Zie voor meer informatie over blobs de [Vervolgstappen](#next-steps) sectie. U kunt ook downloaden de [voorbeeldapp](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) snel zien het gebruik van Azure Storage in een iOS-toepassing.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>De Azure Storage-bibliotheek voor iOS in uw toepassing importeren
U kunt importeren de Azure Storage-bibliotheek voor iOS in uw toepassing met behulp van de [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) of door het importeren van de **Framework** bestand. CocoaPod is de aanbevolen manier zoals het integreren van de bibliotheek eenvoudiger echter importeren uit het bestand framework is minder Tussenkomende voor uw bestaande project maakt.

Voor het gebruik van deze bibliotheek, moet u het volgende:
- iOS 8 +
- Xcode 7 +

## <a name="cocoapod"></a>CocoaPod
1. Als u dit nog niet hebt gedaan, [CocoaPods installeren](https://guides.cocoapods.org/using/getting-started.html#toc_3) op uw computer door het openen van een terminalvenster en de volgende opdracht uit te voeren
    
    ```shell   
    sudo gem install cocoapods
    ```

2. Maak vervolgens een nieuw bestand met de naam in de projectmap (de map met uw .xcodeproj-bestand) _Podfile_(zonder extensie). Het volgende toevoegen aan _Podfile_ en op te slaan.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. Navigeer naar de projectmap in een terminalvenster en voer de volgende opdracht

    ```shell    
    pod install
    ```

4. Als uw .xcodeproj geopend in Xcode is, sluiten. Open het zojuist gemaakte projectbestand wordt met de extensie .xcworkspace in uw projectmap. Dit is het bestand dat u hebt gewerkt uit voor nu.

## <a name="framework"></a>Framework
De andere manier gebruikmaken van de tapewisselaar is het framework handmatig bouwen:

1. Eerst, downloaden of te klonen de [azure-opslag-i/o-opslagplaats](https://github.com/azure/azure-storage-ios).
2. Ga naar de *azure-opslag-ios* -> *Lib* -> *Azure Storage-clientbibliotheek*, en open `AZSClient.xcodeproj` in Xcode.
3. Wijzig bij de linkerbovenhoek van Xcode, het actieve schema 'Azure Storage-clientbibliotheek'-'Framework'.
4. Bouw het project (⌘ + B). Hiermee maakt u een `AZSClient.framework` bestand op het bureaublad.

U kunt de framework-bestand vervolgens importeren in uw toepassing als volgt:

1. Een nieuw project maken of openen van een bestaand project in Xcode.
2. Slepen en neerzetten de `AZSClient.framework` in uw Xcode-projectnavigator.
3. Selecteer *items kopiëren indien nodig*, en klik op *voltooien*.
4. Klik op het project in de linkernavigatiebalk en op de *algemene* boven op het tabblad van de project-editor.
5. Onder de *gekoppelde Frameworks en bibliotheken* sectie, klikt u op de knop toevoegen (+).
6. Zoek in de lijst met bibliotheken al opgegeven voor `libxml2.2.tbd` en toe te voegen aan uw project.

## <a name="import-the-library"></a>De bibliotheek importeren 
```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

Als u van Swift gebruikmaakt, moet u een bridging header maken en er < AZSClient/AZSClient.h > importeren:

1. Maak een headerbestand `Bridging-Header.h`, en de bovenstaande importinstructie toe te voegen.
2. Ga naar de *Build Settings* tabblad en zoek naar *Objective-C Bridging Header*.
3. Dubbelklik op het gebied van *Objective-C Bridging Header* en het pad toevoegen aan uw header-bestand:`ProjectName/Bridging-Header.h`
4. Bouw het project (⌘ + B) om te controleren dat de bridging header is opgehaald door Xcode.
5. Start met behulp van de bibliotheek rechtstreeks in een bestand Swift, is niet nodig voor importinstructies.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchrone bewerkingen
> [!NOTE]
> Alle methoden die uitvoeren van een aanvraag in voor de service zijn asynchrone bewerkingen. In de codevoorbeelden vindt u dat deze methoden een handler is voltooid hebben. Code binnen de voltooiing-handler wordt uitgevoerd **nadat** de aanvraag is voltooid. Nadat de voltooiing handler wordt uitgevoerd code **terwijl** de aangevraagd.
> 
> 

## <a name="create-a-container"></a>Een container maken
Elke blob in Azure Storage moet zich bevinden in een container. Het volgende voorbeeld ziet u het maken van een container, aangeroepen *newcontainer*, in uw opslagaccount als deze niet al bestaat. Als u een naam voor de container, houd ook rekening met de hierboven genoemde naamgevingsregels worden.

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

U kunt bevestigen dat dit werkt door te kijken de [Microsoft Azure Storage Explorer](http://storageexplorer.com) en te verifiëren dat *newcontainer* is in de lijst met containers voor uw opslagaccount.

## <a name="set-container-permissions"></a>Container machtigingen instellen
Een container machtigingen zijn geconfigureerd voor **persoonlijke** wordt standaard. Containers bieden echter een aantal verschillende opties voor toegang tot de container:

* **Persoonlijke**: Container en de blob-gegevens kunnen worden gelezen door alleen de eigenaar van het account.
* **BLOB**: Blob-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de containergegevens is niet beschikbaar. Clients kunnen blobs in de container via anonieme aanvragen niet opsommen.
* **Container**: Container en de blob-gegevens kunnen worden gelezen via anonieme aanvraag. Clients kunnen inventariseren blobs in de container via anonieme aanvraag, maar kunnen containers in het opslagaccount niet inventariseren.

Het volgende voorbeeld ziet u het maken van een container met **Container** toegangsmachtigingen waardoor openbare, alleen-lezen toegang voor alle gebruikers op Internet:

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Zoals vermeld in de [Blob-service concepten](#blob-service-concepts) sectie, Blob Storage biedt drie typen blobs: blok-blobs, toevoeg-blobs en pagina-blobs. De iOS-bibliotheek van Azure Storage ondersteunt alle drie typen blobs. In de meeste gevallen is een blok-blob het aangewezen type om te gebruiken.

Het volgende voorbeeld laat zien hoe een blok-blob van een NSString uploaden. Als een blob met dezelfde naam al in deze container bestaat, wordt de inhoud van deze blob overschreven.

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob to Storage
                [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

U kunt bevestigen dat dit werkt door te kijken de [Microsoft Azure Storage Explorer](http://storageexplorer.com) en te verifiëren dat de container *containerpublic*, bevat de blob *sampleblob*. In dit voorbeeld wordt een openbare container gebruikt, zodat u ook controleren kunt of deze toepassing heeft gewerkt door te gaan naar de blobs URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Naast het uploaden van een blok-blob van een NSString bestaan vergelijkbare methoden voor NSData, NSInputStream of een lokaal bestand.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Het volgende voorbeeld ziet hoe u alle blobs in een container. Als u deze bewerking uitvoert, worden Houd ook rekening met de volgende parameters:     

* **continuationToken** -de voortzetting token geeft waar de aanbieding-bewerking moet worden gestart. Als er geen token is opgegeven, wordt er een lijst blobs vanaf het begin. Een onbeperkt aantal blobs kan worden weergegeven, van nul tot een maximale set. Zelfs als deze methode nul resultaten retourneert als `results.continuationToken` is niet nul is, kunnen er meer blobs op de service die niet zijn opgenomen.
* **voorvoegsel** -kunt u het voorvoegsel gebruiken voor blob-aanbieding. Alleen de blobs die met dit voorvoegsel beginnen wordt weergegeven.
* **useFlatBlobListing** - zoals vermeld in de [Naming en verwijzen naar containers en blobs](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) sectie, hoewel de Blob-service een opslagschema platte is, kunt u een virtuele hiërarchie door de naam van de blobs met informatie over het pad. Niet-platte aanbieding is echter momenteel niet ondersteund. Deze functie is binnenkort beschikbaar. Nu u deze waarde moet **Ja**.
* **blobListingDetails** -kunt u opgeven welke items u wilt opnemen wanneer blobs
  * _AZSBlobListingDetailsNone_: lijst alleen doorgevoerde blobs en blobmetagegevens niet retourneren.
  * _AZSBlobListingDetailsSnapshots_: lijst doorgevoerd blobs en blob-momentopnamen.
  * _AZSBlobListingDetailsMetadata_: blobmetagegevens ophalen voor elke blob geretourneerd in de aanbieding.
  * _AZSBlobListingDetailsUncommittedBlobs_: lijst doorgevoerd en niet-doorgevoerde blobs.
  * _AZSBlobListingDetailsCopy_: kopie opnemen eigenschappen in de aanbieding.
  * _AZSBlobListingDetailsAll_: lijst alle beschikbare doorgevoerd blobs, niet-doorgevoerde blobs en momentopnamen en de status van alle metagegevens en kopieert u voor deze blobs retourneren.
* **maxResults** -het maximum aantal resultaten geretourneerd voor deze bewerking. Gebruik -1 niet instellen van een limiet.
* **completionHandler** - het codeblok uit te voeren met de resultaten van de bewerking van de aanbieding.

In dit voorbeeld wordt een Help-methode wordt gebruikt voor recursief aanroep van de lijst met blobs methode telkens wanneer een vervolgtoken wordt geretourneerd.

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a>Een blob downloaden
Het volgende voorbeeld laat zien hoe een blob naar een object NSString downloaden.

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a>Een blob verwijderen
Het volgende voorbeeld laat zien hoe verwijderen van een blob.

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a>Verwijderen van een blob-container
Het volgende voorbeeld laat zien hoe een container.

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a>Volgende stappen
Nu u het gebruik van Blob Storage van iOS hebt geleerd, volgt u deze koppelingen voor meer informatie over de iOS-bibliotheek en de Storage-service.

* [Azure Storage-clientbibliotheek voor iOS](https://github.com/azure/azure-storage-ios)
* [Azure Storage iOS naslagdocumentatie](http://azure.github.io/azure-storage-ios/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](http://blogs.msdn.com/b/windowsazurestorage)

Als u vragen over deze bibliotheek hebt, gerust posten naar onze [Azure MSDN-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) of [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Als u suggesties voor functies voor Azure Storage hebt, Ga naar boeken [Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).

