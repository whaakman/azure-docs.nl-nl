---
title: .NET SDK gebruiken voor Microsoft Azure StorSimple Data Manager-taken | Microsoft Docs
description: Informatie over het gebruik van .NET SDK StorSimple Data Manager-taken starten
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: d15a5cbda2f0c2a363b40e94c38fed6631aa81b5
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="use-the-net-sdk-to-initiate-data-transformation"></a>De .net SDK gebruiken om te zetten van gegevenstransformatie

## <a name="overview"></a>Overzicht

Dit artikel wordt uitgelegd hoe u de functie voor transformatie van gegevens in de StorSimple Data Manager-service kunt gebruiken voor het transformeren van gegevens van de StorSimple-apparaat. De getransformeerde gegevens worden vervolgens die door andere Azure-services in de cloud gebruikt.

U kunt een transformatie-taak op twee manieren starten:

 - De .NET SDK gebruiken
 - Gebruik Azure Automation-runbook
 
 Dit artikel wordt uitgelegd hoe u een voorbeeld .NET-consoletoepassing een transformatie-taak starten en te volgen voor voltooiing maakt. Voor meer informatie over het initiëren van gegevenstransformatie via Automation, gaat u naar [gebruik Azure Automation-runbook trigger data transformation baan](storsimple-data-manager-job-using-automation.md).

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt voordat u begint:
*   Een computer met:

    - Visual Studio 2012, 2013 of 2015 2017.

    - Azure Powershell. [Download de Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Een juist geconfigureerde taakdefinitie in StorSimple Data Manager binnen een resourcegroep.
*   Alle vereiste dll. Download deze DLL-bestanden van de [GitHub-opslagplaats](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   [`Get-ConfigurationParams.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1)script van de GitHub-opslagplaats.

## <a name="step-by-step-procedure"></a>Stapsgewijze instructies

De volgende stappen voor het gebruik van .NET starten van een transformatie-taak uitvoeren.

1. Voor het ophalen van de configuratieparameters, voer de volgende stappen uit:
    1. Download de `Get-ConfigurationParams.ps1` uit het script GitHub-opslagplaats in `C:\DataTransformation` locatie.
    1. Voer de `Get-ConfigurationParams.ps1` script vanaf de GitHub-opslagplaats. Typ de volgende opdracht:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        U kunt geen waarden doorgeven voor de ActiveDirectoryKey en AppName.

2. Dit script levert de volgende waarden:
    * Client-id
    * Tenant-id
    * Active Directory-sleutel (zelfde als een dat hierboven is opgegeven)
    * Abonnements-id

        ![De uitvoer van het script voor configuratie-parameters](media/storsimple-data-manager-dotnet-jobs/get-config-parameters.png)

3. Met behulp van Visual Studio 2012, 2013 of 2015, en maak een C# .NET-consoletoepassing.

    1. Start **Visual Studio 2012-2013-2015**.
    1. Selecteer **bestand > Nieuw > Project**.

        ![Een 1-project maken](media/storsimple-data-manager-dotnet-jobs/create-new-project-7.png)        
    2. Selecteer **geïnstalleerd > Sjablonen > Visual C# > consoletoepassing**.
    3. Voer **DataTransformationApp** voor de **naam**.
    4. Selecteer **C:\DataTransformation** voor de **locatie**.
    6. Klik op **OK** om het project aan te maken.

        ![Een 2-project maken](media/storsimple-data-manager-dotnet-jobs/create-new-project-1.png)

4.  Voeg nu alle dll's aanwezig zijn in de [dll's map](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) als **verwijzingen** in het project dat u hebt gemaakt. Als u wilt toevoegen in de dll-bestanden, het volgende doen:

    1. In Visual Studio, gaat u naar **weergave > Solution Explorer**.
    2. Klik op de pijl naar links van Data Transformation App-project. Klik op **verwijzingen** en klik vervolgens met de rechtermuisknop op **verwijzing toevoegen**.
    
        ![DLL's 1 toevoegen](media/storsimple-data-manager-dotnet-jobs/create-new-project-4.png)

    3. Blader naar de locatie van de map pakketten, selecteert u alle dll's en op **toevoegen**, en klik vervolgens op **OK**.

        ![DLL's 2 toevoegen](media/storsimple-data-manager-dotnet-jobs/create-new-project-6.png)

5. Voeg de volgende **using**-instructies toe aan het bronbestand (Program.cs) in het project.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```
    
6. De volgende code initialiseert het taakexemplaar voor transformatie van gegevens. Voeg deze in de **Main-methode**. Vervang de waarden van configuratieparameters als u eerder hebt verkregen. Sluit de waarden van **Resourcegroepnaam** en **ResourceName**. De **ResourceGroupName** is gekoppeld met het gegevensbeheer van StorSimple waarop de taakdefinitie is geconfigureerd. De **ResourceName** is de naam van uw StorSimple Data Manager-service.

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);
    ```
   
7. Geef de parameters waarmee de taakdefinitie moet worden uitgevoerd

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);
    ```

    (OR)

    Als u wijzigen van de definitie van taakparameters tijdens runtime wilt, voegt u de volgende code:

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    ```

8. Voeg de volgende code om te activeren van een transformatie-taak op de taakdefinitie na de initialisatie. Plug-in de betreffende **definitie taaknaam**.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);
    Console.WriteLine("jobid: ", jobId);
    Console.ReadLine();

    ```
    Zodra de code is geplakt, moet u de oplossing bouwen. Hier volgt een schermopname van het codefragment initialiseren van het exemplaar van data transformation taak.

   ![Codefragment transformatie-taak gegevens initialiseren](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet-1.png)

9. Deze taak de gegevens die overeenkomt met de hoofdmap worden omgezet en bestand filtert op het StorSimple-volume en plaatst deze in de opgegeven container/bestandsshare. Wanneer een bestand wordt omgezet, wordt een bericht toegevoegd aan een opslagwachtrij (in hetzelfde opslagaccount als de container/bestandsshare) met dezelfde naam als de taakdefinitie. Dit bericht kan worden gebruikt als een trigger initiëren verdere verwerking van het bestand.

10. Zodra de taak is geactiveerd, kunt u de volgende code voor het bijhouden van de taak voor voltooiing. Het is niet verplicht om toe te voegen deze code voor de taak uitvoeren.

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```
 Hier volgt een schermopname van het gehele codevoorbeeld gebruikt voor het activeren van de taak met .NET.

 ![Volledige codefragment voor het activeren van een .NET-taak](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet.png)

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
