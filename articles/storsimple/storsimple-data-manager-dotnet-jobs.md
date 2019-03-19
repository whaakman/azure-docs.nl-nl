---
title: .NET SDK gebruiken voor Microsoft Azure StorSimple Data Manager-taken | Microsoft Docs
description: Leer hoe u .NET SDK gebruiken om te starten van StorSimple Data Manager-taken
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
ms.openlocfilehash: 80f01a926b94deebab59f8ef91bfc36a4600b5f0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57885047"
---
# <a name="use-the-net-sdk-to-initiate-data-transformation"></a>De .NET SDK gebruiken om te gegevenstransformatie starten

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u de functie voor transformatie van gegevens in de StorSimple Data Manager-service kunt gebruiken om gegevens van StorSimple-apparaat te transformeren. De getransformeerde gegevens wordt vervolgens gebruikt door andere Azure-services in de cloud.

U kunt een taak voor gegevenstransformatie op twee manieren starten:

- De .NET SDK gebruiken
- Gebruik Azure Automation-runbook
 
  Dit artikel wordt uitgelegd hoe u een voorbeeld van .NET-consoletoepassing om te starten van een taak voor gegevenstransformatie en vervolgens bijhouden voor de voltooiing maakt. Voor meer informatie over het starten van de gegevenstransformatie van via Automation, gaat u naar [gebruik Azure Automation-runbook aan taken voor gegevenstransformatie trigger](storsimple-data-manager-job-using-automation.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u hebt:
*   Een computer met:

    - Visual Studio 2012, 2013, 2015 of 2017.

    - Azure Powershell. [Azure Powershell downloaden](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Een juist geconfigureerde taakdefinitie in StorSimple Data Manager binnen een resourcegroep.
*   De vereiste dll. Download deze DLL-bestanden uit de [GitHub-opslagplaats](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   [`Get-ConfigurationParams.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) het script uit de GitHub-opslagplaats.

## <a name="step-by-step-procedure"></a>Stapsgewijze procedure

De volgende stappen uitvoeren om .NET gebruiken om te starten van een taak voor gegevenstransformatie.

1. Als u wilt ophalen van de parameters voor de configuratie, moet u de volgende stappen uitvoeren:
    1. Download de `Get-ConfigurationParams.ps1` van het script van de GitHub-opslagplaats in `C:\DataTransformation` locatie.
    1. Voer de `Get-ConfigurationParams.ps1` script uit de GitHub-opslagplaats. Typ de volgende opdracht:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        U kunt geen waarden doorgeven voor de ActiveDirectoryKey en AppName.

2. Dit script levert de volgende waarden:
    * Client-id
    * Tenant-id
    * Active Directory-sleutel (hetzelfde als de domeincontroller dat hierboven is opgegeven)
    * Abonnements-id

        ![De uitvoer van het script voor configuratie-parameters](media/storsimple-data-manager-dotnet-jobs/get-config-parameters.png)

3. Met behulp van Visual Studio 2012, 2013 of 2015, maken een C# .NET-consoletoepassing.

    1. Start **Visual Studio 2012/2013/2015**.
    1. Selecteer **Bestand > Nieuw > Project**.

        ![Maak een project 1](media/storsimple-data-manager-dotnet-jobs/create-new-project-7.png)        
    2. Selecteer **geïnstalleerd > Sjablonen > Visual C# > consoletoepassing**.
    3. Voer **DataTransformationApp** voor de **naam**.
    4. Selecteer **C:\DataTransformation** voor de **locatie**.
    6. Klik op **OK** om het project aan te maken.

        ![Maak een project 2](media/storsimple-data-manager-dotnet-jobs/create-new-project-1.png)

4. Voeg nu toe alle dll's aanwezig zijn in de [dll-bestanden map](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) als **verwijzingen** in het project dat u hebt gemaakt. Als u wilt toevoegen de dll-bestanden, voert u het volgende:

   1. Ga in Visual Studio naar **weergave > Solution Explorer**.
   2. Klik op de pijl naar links van Data Transformation App-project. Klik op **verwijzingen** en klik vervolgens met de rechtermuisknop op **verwijzing toevoegen**.
    
       ![DLL-bestanden 1 toevoegen](media/storsimple-data-manager-dotnet-jobs/create-new-project-4.png)

   3. Blader naar de locatie van de map pakketten, selecteert u de dll-bestanden en klikt u op **toevoegen**, en klik vervolgens op **OK**.

       ![DLL-bestanden 2 toevoegen](media/storsimple-data-manager-dotnet-jobs/create-new-project-6.png)

5. Voeg de volgende **using**-instructies toe aan het bronbestand (Program.cs) in het project.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```
    
6. De volgende code initialiseert het taakexemplaar voor transformatie van gegevens. Voegt u dit in de **Main-methode**. Vervang de waarden van parameters voor de configuratie die eerder zijn verkregen. De waarden van de invoegtoepassing **groepsnaam voor Accountresources** en **ResourceName**. De **ResourceGroupName** is gekoppeld met de StorSimple-gegevensbeheer waarop de taakdefinitie is geconfigureerd. De **ResourceName** is de naam van uw StorSimple Data Manager-service.

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

8. Voeg de volgende code voor het activeren van een taak voor gegevenstransformatie in de taakdefinitie na de initialisatie. Sluit u de juiste **Taakdefinitienaam**.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);
    Console.WriteLine("jobid: ", jobId);
    Console.ReadLine();

    ```
    Zodra de code is geplakt, maak de oplossing. Hier volgt een schermopname van het codefragment om te initialiseren van het taakexemplaar voor transformatie van gegevens.

   ![Codefragment om te initialiseren van de taak voor gegevenstransformatie](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet-1.png)

9. Deze taak de gegevens die overeenkomt met de hoofdmap worden getransformeerd en bestand filtert op het StorSimple-volume en deze in de opgegeven container /-bestandsshare wordt geplaatst. Wanneer een bestand wordt omgezet, wordt een bericht wordt toegevoegd aan een opslagwachtrij (in hetzelfde opslagaccount als de container/bestandsshare) met dezelfde naam als de taakdefinitie. Dit bericht kan worden gebruikt als een trigger te initiëren verdere verwerking van het bestand.

10. Zodra de taak is geactiveerd, kunt u de volgende code om bij te houden van de taak voor de voltooiing. Het is niet verplicht om toe te voegen deze code voor het uitvoeren van taak.

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
    Hier volgt een schermopname van het volledige codevoorbeeld gebruikt voor het activeren van de taak met behulp van .NET.

    ![Volledige codefragment voor het activeren van een .NET-taak](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet.png)

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
