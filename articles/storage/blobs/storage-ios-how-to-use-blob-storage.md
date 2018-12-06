---
title: Het gebruik van object (Blob)-opslag i/o - Azure | Microsoft Docs
description: Sla niet-gestructureerde gegevens op in de cloud met Azure Blob Storage (objectopslag).
services: storage
author: michaelhauss
ms.service: storage
ms.devlang: objective-c
ms.topic: article
ms.date: 11/20/2018
ms.author: michaelhauss
ms.component: blobs
ms.openlocfilehash: be9254686eeb285fb4f0a5e29ba60023abee84ab
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961923"
---
# <a name="how-to-use-blob-storage-from-ios"></a>Het Blob storage gebruiken met iOS

Dit artikel leest hoe u algemene scenario's met behulp van Microsoft Azure Blob-opslag uitvoert. De voorbeelden zijn geschreven in Objective-C en gebruik de [Azure Storage-clientbibliotheek voor iOS](https://github.com/Azure/azure-storage-ios). De behandelde scenario's zijn uploaden, vermelden, downloaden en verwijderen van blobs. Zie voor meer informatie over blobs de [Vervolgstappen](#next-steps) sectie. U kunt ook downloaden de [voorbeeldapp](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) om snel het gebruik van Azure Storage in een iOS-toepassing.

Zie voor meer informatie over Blob-opslag, [Inleiding tot Azure Blob-opslag](storage-blobs-introduction.md).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Het importeren van de Azure Storage-bibliotheek voor iOS in uw toepassing
U kunt importeren met de Azure Storage-iOS-bibliotheek in uw toepassing met behulp van de [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) of door het importeren van de **Framework** bestand. CocoaPod is de aanbevolen manier zoals het integreren van de bibliotheek eenvoudiger, maar importeren uit het bestand framework is minder firewallverbinding voor uw bestaande project maakt.

Voor het gebruik van deze bibliotheek, moet u het volgende:
- iOS 8 +
- Xcode 7 +

## <a name="cocoapod"></a>CocoaPod
1. Als u dit nog niet hebt gedaan, [CocoaPods installeren](https://guides.cocoapods.org/using/getting-started.html#toc_3) op uw computer door een terminalvenster te openen en de volgende opdracht uit
    
    ```shell   
    sudo gem install cocoapods
    ```

2. Maak vervolgens een nieuw bestand met de naam in de projectmap (de map met de .xcodeproj-bestand) _Podfile_(zonder extensie). Het volgende toevoegen aan _Podfile_ en op te slaan.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. In het terminalvenster naar de projectmap en voer de volgende opdracht uit

    ```shell    
    pod install
    ```

4. Als uw .xcodeproj geopend in Xcode is, deze sluiten. Open het nieuwe project-bestand waarvoor de extensie .xcworkspace in uw projectmap. Dit is het bestand dat u gaat gebruiken van voor nu op.

## <a name="framework"></a>Framework
De andere manier gebruik van de bibliotheek is het framework handmatig bouwen:

1. Eerst, download of kloon de [azure-opslag-i/o-opslagplaats](https://github.com/azure/azure-storage-ios).
2. Ga naar de *azure-opslag-ios* -> *Lib* -> *Azure Storage Client Library*, en open `AZSClient.xcodeproj` in Xcode.
3. Wijzig bij de top-links van Xcode, het actieve schema 'Azure Storage Client Library'-"Framework".
4. Bouw het project (⌘ + B). Hiermee maakt u een `AZSClient.framework` bestand op het bureaublad.

U kunt de framework-bestand vervolgens importeren in uw toepassing door de volgende te doen:

1. Een nieuw project maken of openen van een bestaand project in Xcode.
2. Sleep en zet de `AZSClient.framework` in uw Xcode-projectnavigator.
3. Selecteer *items kopiëren indien nodig*, en klik op *voltooien*.
4. Klik op uw project in de navigatiebalk aan de linkerkant en klikt u op de *algemene* tabblad aan de bovenkant van de project-editor.
5. Onder de *Linked Frameworks and Libraries* sectie, klikt u op de knop toevoegen (+).
6. Zoek in de lijst met bibliotheken die al is opgegeven, `libxml2.2.tbd` en voeg deze toe aan uw project.

## <a name="import-the-library"></a>De bibliotheek importeren 
```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

Als u van Swift gebruikmaakt, moet u een bridging header maken en er < AZSClient/AZSClient.h > importeren:

1. Maak een headerbestand `Bridging-Header.h`, en de bovenstaande importinstructie toe te voegen.
2. Ga naar de *Build Settings* tabblad en zoek naar de *Objective-C Bridging Header*.
3. Dubbelklik op het gebied van *Objective-C Bridging Header* en het pad toevoegen aan uw header-bestand: `ProjectName/Bridging-Header.h`
4. Bouw het project (⌘ + B) om te controleren dat de bridging header is opgehaald door Xcode.
5. Start met behulp van de bibliotheek rechtstreeks in een bestand Swift, hoeven niet voor importinstructie toe.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchrone bewerkingen
> [!NOTE]
> Alle methoden die uitvoeren van een aanvraag indient voor de service zijn asynchrone bewerkingen. In de voorbeelden van code vindt u dat deze methoden een voltooiingshandler hebben. Code binnen de voltooiingshandler wordt uitgevoerd **nadat** de aanvraag is voltooid. Na het uitvoeren van de voltooiingshandler wordt code **terwijl** is de aanvraag wordt gedaan.
> 
> 

## <a name="create-a-container"></a>Een container maken
Elke blob in Azure Storage moet zich bevinden in een container. Het volgende voorbeeld ziet over het maken van een container, met de naam *newcontainer*, in uw opslagaccount als deze nog niet bestaat. Bij het kiezen van een naam op voor de container worden gelet op de hierboven genoemde naamgevingsregels.

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

U kunt bevestigen dat dit werkt door te kijken de [Microsoft Azure Storage Explorer](http://storageexplorer.com) en te verifiëren dat *newcontainer* is in de lijst met containers voor uw Storage-account.

## <a name="set-container-permissions"></a>Machtigingen van de Container instellen
De machtigingen van een container zijn geconfigureerd voor **persoonlijke** toegang standaard. Containers bieden echter een aantal verschillende opties voor toegang tot de container:

* **Persoonlijke**: Container en blob-gegevens kunnen worden gelezen door alleen de eigenaar van het account.
* **BLOB**: Blob-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de containergegevens is niet beschikbaar. Clients kunnen blobs in de container via anonieme aanvragen niet inventariseren.
* **Container**: Container en blob-gegevens kunnen worden gelezen via anonieme aanvragen. Clients kunnen opsommen en blobs in de container via anonieme aanvragen, maar kunnen de containers in het opslagaccount niet opsommen.

Het volgende voorbeeld ziet u hoe u het maken van een container met **Container** toegangsmachtigingen gebruikt, die wordt openbaar, alleen-lezen toegang toestaan voor alle gebruikers op Internet:

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
Zoals vermeld in de [Blob serviceconcepten](#blob-service-concepts) gedeelte, Blob Storage biedt drie typen blobs: blok-blobs, toevoeg-blobs en pagina-blobs. De Azure Storage-bibliotheek voor iOS ondersteunt drie typen blobs. In de meeste gevallen is een blok-blob het aangewezen type om te gebruiken.

Het volgende voorbeeld ziet u hoe u een blok-blob uit een NSString uploadt. Als een blob met dezelfde naam al in deze container bestaat, kunt u de inhoud van deze blob wordt overschreven.

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

U kunt bevestigen dat dit werkt door te kijken de [Microsoft Azure Storage Explorer](http://storageexplorer.com) en te verifiëren dat de container, *containerpublic*, bevat de blob *sampleblob*. In dit voorbeeld hebben we een openbare container gebruikt, zodat u ook controleren kunt of deze toepassing heeft gewerkt door te gaan naar de URI van de blobs:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Naast het uploaden van een blok-blob uit een NSString bestaan vergelijkbare methoden voor NSData, NSInputStream of een lokaal bestand.

## <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven
Het volgende voorbeeld ziet hoe u alle blobs in een container te vermelden. Wanneer u deze bewerking uitvoert, moet u zich ervan bewust van de volgende parameters:     

* **continuationToken** -de voortzetting van token geeft waar de lijstbewerking moet beginnen. Als er geen token is opgegeven, wordt deze blobs vanaf het begin te vermelden. Een onbeperkt aantal blobs kan worden weergegeven, van nul tot een maximum instellen. Zelfs als deze methode nul resultaten retourneert als `results.continuationToken` is niet nul is, kunnen er meer blobs op de service die niet zijn opgenomen.
* **voorvoegsel** -kunt u het voorvoegsel moet worden gebruikt voor blob-aanbieding. Alleen blobs die met dit voorvoegsel beginnen wordt weergegeven.
* **useFlatBlobListing** - zoals vermeld in de [Naming en verwijzen naar containers en blobs](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) sectie, hoewel de Blob-service een opslagschema platte is kunt u een virtuele hiërarchie door de naam van blobs met pad informatie. Niet-platte vermelding wordt echter momenteel niet ondersteund. Deze functie is binnenkort beschikbaar. Nu u deze waarde moet **Ja**.
* **blobListingDetails** -kunt u opgeven welke artikelen op te nemen wanneer blobs weergeven
  * _AZSBlobListingDetailsNone_: alleen toegezegde blobs te vermelden en metagegevens van de blob niet retourneren.
  * _AZSBlobListingDetailsSnapshots_: toegezegde blobs te vermelden en blob-momentopnamen.
  * _AZSBlobListingDetailsMetadata_: blob-metagegevens ophalen voor elke blob die wordt geretourneerd in de aanbieding.
  * _AZSBlobListingDetailsUncommittedBlobs_: doorgevoerd en niet-doorgevoerde blobs te vermelden.
  * _AZSBlobListingDetailsCopy_: kopie opnemen eigenschappen in de aanbieding.
  * _AZSBlobListingDetailsAll_: lijst met alle beschikbare toegezegde blobs, niet-doorgevoerde blobs en momentopnamen en retourneert alle metagegevens en kopieert u de status voor deze blobs.
* **maxResults** -het maximum aantal resultaten dat moet worden geretourneerd voor deze bewerking. Gebruik dan -1 niet een limiet instellen.
* **completionHandler** - het codeblok uit te voeren met de resultaten van de aanbieding-bewerking.

In dit voorbeeld wordt een Help-methode wordt gebruikt voor het recursief aanroep van de lijst met blobs methode telkens wanneer een vervolgtoken geretourneerd.

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
Het volgende voorbeeld ziet hoe u een blob te downloaden naar een NSString-object.

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
Het volgende voorbeeld ziet hoe u een blob verwijderen.

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

## <a name="delete-a-blob-container"></a>Een blob-container verwijderen
Het volgende voorbeeld ziet hoe u een container.

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
Nu dat u hebt geleerd hoe u Blob-opslag op iOS gebruiken, volgt u deze koppelingen voor meer informatie over de iOS-bibliotheek en de Storage-service.

* [Azure Storage-clientbibliotheek voor iOS](https://github.com/azure/azure-storage-ios)
* [Azure Storage-iOS-referentiedocumentatie](http://azure.github.io/azure-storage-ios/)
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Blog van het Azure Storage-team](https://blogs.msdn.com/b/windowsazurestorage)

Als u vragen over deze bibliotheek hebt, kunt u plaatsen op onze [Azure MSDN-forum](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) of [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Hebt u suggesties voor functies voor Azure Storage, kunt u plaatsen op [Azure Storage Feedback](https://feedback.azure.com/forums/217298-storage/).

