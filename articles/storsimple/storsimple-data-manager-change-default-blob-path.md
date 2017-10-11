---
title: Blob-pad van de standaardwaarde wijzigen | Microsoft Docs
description: Meer informatie over het instellen van een Azure-functie naam wijzigen van een blob-bestandspad (afgeschermd voorbeeld)
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 057d4d7370207859617eb63238bf425bfa6d3e16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="change-a-blob-path-from-the-default-path-private-preview"></a>Wijzig het blobpad van een het standaardpad (afgeschermd voorbeeld)

In dit artikel wordt beschreven hoe een Azure-functie instellen voor het wijzigen van een standaard blob-bestandspad. 

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u de taakdefinitie van een die correct is geconfigureerd in de bron van een hybride gegevens binnen een resourcegroep.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

Voor het maken van een Azure-functie, het volgende doen:

1. Ga naar de [Azure Portal](http://portal.azure.com/).

2. Klik boven in het linkerdeelvenster op **nieuw**. 

3. In de **Search** in het vak **functie-App**, en druk op Enter.

    ![Typ 'Functie-App' in het vak Zoeken](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. In de **resultaten** lijst, klikt u op **functie-App**.

    ![Selecteer een resource voor de functie-app in de lijst met resultaten](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    De **functie-App** venster wordt geopend.

5. Klik op **Create**.

    ![De knop functie-App venster 'Maken'](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. Op de **functie-App** blade van de configuratie van de volgende handelingen uit:

    a. In de **appnaam** typt u de appnaam.
    
    b. In de **abonnement** typt u de naam van het abonnement.

    c. Onder **resourcegroep**, klikt u op **nieuw**, en typ vervolgens de naam van de resourcegroep.

    d. In de **die als host fungeert van plan bent** in het vak **verbruik plannen**.

    e. In de **locatie** typt u de locatie.

    f. Onder **opslagaccount**, selecteer een bestaand opslagaccount of maak een nieuw opslagaccount. Een opslagaccount wordt intern gebruikt voor de functie.

    ![Voer de nieuwe functie-App-configuratiegegevens](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Klik op **Create**.  
    De functie-app wordt gemaakt.

8. Klik in het linkerdeelvenster op **meer services**, en vervolgens de volgende handelingen uit:
    
    a. In de **Filter** in het vak **App services**.
    
    b. Klik op **App Services**. 

    ![De koppeling 'Meer services' in het linkerdeelvenster](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. Klik op de naam van de functie-app in de lijst van app-services.  
    De functie app-pagina wordt geopend.

10. Klik in het linkerdeelvenster op **nieuwe functie**, en vervolgens de volgende handelingen uit: 

    a. In de **taal** selecteert **C#**.
    
    b. Selecteer in de matrix van sjabloon tegels **QueueTrigger CSharp**.

    c. In de **naam van de functie** typt u een naam voor de functie.

    d. In de **wachtrijnaam** typt u de naam van uw data transformation taak definitie.

    e. Onder **Storage-account verbinding**, klikt u op **nieuwe**, en selecteer vervolgens het account dat overeenkomt met de taak gegevens transformatie.  
        Noteer de naam van de verbinding. De naam van de is later in de Azure-functie vereist.

       ![Maak een nieuwe C#-functie](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Klik op **Create**.  
    De **functie** venster wordt geopend.

11. In de **functie** venster uitvoeren _.csx_ bestand en voert u het volgende:

    a. Plak de volgende code:

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip to copy the blob to new container, if source blob doesn't exist
            log.Info($"The specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy to new container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    b. Vervang **STORAGE_CONNECTIONNAME** op regel 11 met de storage-account verbinding (Zie punt 8 c).

    c. Klik linksboven, **opslaan**.

    ![De functie opslaan](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. Voor het voltooien van de functie toevoegen één meer bestand als volgt:

    a. Klik op **bestanden bekijken**.

       ![De koppeling 'Bestanden weergeven'](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Klik op **Add**.
    
    c. Type **project.json**, en druk op Enter.
    
    d. In de **project.json** bestand, plak de volgende code:

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    e. Klik op **Opslaan**.

U hebt een Azure-functie gemaakt. Deze functie wordt telkens wanneer die een nieuwe blob wordt gegenereerd door de taak Gegevenstransformatie geactiveerd.

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md)
