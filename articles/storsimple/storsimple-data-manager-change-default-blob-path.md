---
title: Blob-pad van de standaardwaarde wijzigen | Microsoft Docs
description: Meer informatie over het instellen van een Azure-functie de naam van een pad naar een blob wijzigen
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: cdaf991c25c23dee4f87b44142c1482bf892bcf2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58011628"
---
# <a name="change-a-blob-path-from-the-default-path"></a>Een blobpad van het standaardpad wijzigen

Wanneer u de service StorSimple Data Manager de gegevens worden getransformeerd, standaard wordt de getransformeerde blobs in een storage-container, zoals is opgegeven tijdens het maken van de doelopslagplaats. Wanneer de blobs op deze locatie binnenkomen, kunt u deze blobs verplaatsen naar een alternatieve locatie. Dit artikel wordt beschreven hoe u een Azure-functie te wijzigen van een standaardpad voor de blob-bestand en kan daarom de blobs te verplaatsen naar een andere locatie kunt instellen.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u een correct geconfigureerde taakdefinitie in uw StorSimple Data Manager-service hebt.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

Voor het maken van een Azure-functie, kunt u de volgende stappen uitvoeren:

1. Ga naar de [Azure Portal](https://portal.azure.com/).

2. Klik op **+ een resource maken**. In de **zoeken** in het vak **functie-App** en druk op **Enter**. Selecteer en klik op **functie-app** in de lijst met apps weergegeven.

    ![Typ '-functie-App' in het zoekvak in](./media/storsimple-data-manager-change-default-blob-path/search-function-app.png)

3. Klik op **Create**.

    ![De knop 'Maken' van functie-App-venster](./media/storsimple-data-manager-change-default-blob-path/create-function-app.png)

4. Op de **functie-App** configuratieblade, voer de volgende stappen uit:

    1. Geef een unieke **appnaam**.
    2. Selecteer in de vervolgkeuzelijst de **abonnement**. Dit abonnement moet hetzelfde zijn als de waarde die is gekoppeld aan uw StorSimple Data Manager-service.
    3. Selecteer **nieuw** resourcegroep.
    4. Voor de **abonnement voor webhosting** vervolgkeuzelijst **Verbruiksabonnement**.
    5. Geef een locatie waar uw functie wordt uitgevoerd. Wilt u dezelfde regio waar de service StorSimple Data Manager en het opslagaccount dat is gekoppeld aan de taakdefinitie van de zich bevinden.
    6. Selecteer een bestaand opslagaccount of maak een nieuw opslagaccount. Een storage-account wordt intern gebruikt voor de functie.

        ![Nieuwe functie-App-configuratie-gegevens invoeren](./media/storsimple-data-manager-change-default-blob-path/function-app-parameters.png)

    7. Klik op **Create**. De functie-app wordt gemaakt.
     
        ![Functie-App gemaakt](./media/storsimple-data-manager-change-default-blob-path/function-app-created.png)

5. Selecteer **functies**, en klikt u op **+ nieuwe functie**.

    ![Klik op + nieuwe functie](./media/storsimple-data-manager-change-default-blob-path/create-new-function.png)

6. Selecteer **C#** voor de taal. Selecteer in de matrix van sjabloon tegels **C#** in de **QueueTrigger-CSharp** tegel.

7. In de **wachtrijtrigger**:

    1. Voer een **naam** voor uw functie.
    2. In de **wachtrijnaam** typt u de taakdefinitienaam transformatie van gegevens.
    3. Onder **opslagaccountverbinding**, klikt u op **nieuwe**. Selecteer in de lijst met opslagaccounts, het account dat is gekoppeld aan de taakdefinitie van de. Noteer de naam van de verbinding (gemarkeerd). De naam is vereist later in de Azure-functie.

        ![Maak een nieuwe C# functie](./media/storsimple-data-manager-change-default-blob-path/new-function-parameters.png)

    4. Klik op **Create**. De **functie** wordt gemaakt.

     
10. Voer in het venster functie _.csx_ bestand.

    ![Maak een nieuwe C# functie](./media/storsimple-data-manager-change-default-blob-path/new-function-run-csx.png)
    
    Voer de volgende stappen uit.

    1. Plak de volgende code:

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

    2. Vervang **STORAGE_CONNECTIONNAME** op regel 11 met verbinding met uw opslagaccount (Zie stap 7 c).

        ![Kopieer de naam van de opslag-verbinding](./media/storsimple-data-manager-change-default-blob-path/new-function-storage-connection-name.png)

    3. **Sla** de functie.

        ![Functie opslaan](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. Toevoegen voor het voltooien van de functie één meer bestand door de volgende stappen:

    1. Klik op **bestanden bekijken**.

       ![De koppeling "Bestanden weergeven"](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    2. Klik op **+ Toevoegen**.
        
        ![De koppeling "Bestanden weergeven"](./media/storsimple-data-manager-change-default-blob-path/new-function-add-file.png)
    
    3. Type **project.json**, en druk vervolgens op **Enter**. In de **project.json** bestand, plak de volgende code:

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

    
    4. Klik op **Opslaan**.

        ![De koppeling "Bestanden weergeven"](./media/storsimple-data-manager-change-default-blob-path/new-function-project-json.png)

U kunt een Azure-functie hebt gemaakt. Deze functie wordt geactiveerd telkens wanneer die een nieuwe blob wordt gegenereerd door de taak voor gegevenstransformatie.

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md)
