---
title: .NET SDK gebruiken voor Microsoft Azure StorSimple Data Manager-taken | Microsoft Docs
description: Informatie over het gebruik van .NET SDK StorSimple Data Manager-taken (private preview) starten
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
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 44d243a034b20b99faf284c8615e470bc6f9d020
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-net-sdk-to-initiate-data-transformation-private-preview"></a>De .net SDK gebruiken om te zetten van gegevenstransformatie (afgeschermd voorbeeld)

## <a name="overview"></a>Overzicht

Dit artikel wordt uitgelegd hoe u de functie voor transformatie van gegevens in de StorSimple Data Manager-service kunt gebruiken voor het transformeren van gegevens van de StorSimple-apparaat. De getransformeerde gegevens worden vervolgens die door andere Azure-services in de cloud gebruikt. Het artikel heeft ook een overzicht voor het maken van een steekproef .NET-consoletoepassing een transformatie-taak starten en te volgen voor de voltooiing.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt voordat u begint:
*   Een systeem met Visual Studio 2012, 2013 of 2015 2017 geïnstalleerd.
*   Azure Powershell is geïnstalleerd. [Download de Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Configuratie-instellingen voor het initialiseren van de taak gegevenstransformatie (instructies voor het verkrijgen van deze instellingen zijn opgenomen hier).
*   De taakdefinitie van een die correct is geconfigureerd in een hybride Gegevensresource binnen een resourcegroep.
*   Alle vereiste dll. Download deze DLL-bestanden van de [GitHub-opslagplaats](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) uit de github-opslagplaats.

## <a name="step-by-step"></a>Stapsgewijs

De volgende stappen voor het gebruik van .NET starten van een transformatie-taak uitvoeren.

1. Voor het ophalen van de configuratieparameters, voer de volgende stappen uit:
    1. Download de `Get-ConfigurationParams.ps1` uit het script github-opslagplaats in `C:\DataTransformation` locatie.
    1. Voer de `Get-ConfigurationParams.ps1` script vanaf de github-opslagplaats. Typ de volgende opdracht:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        U kunt geen waarden doorgeven voor de ActiveDirectoryKey en AppName.


2. Dit script levert de volgende waarden:
    * Client-ID
    * Tenant-id
    * Active Directory-sleutel (zelfde als een dat hierboven is opgegeven)
    * Abonnements-id

3. Met behulp van Visual Studio 2012, 2013 of 2015, en maak een C# .NET-consoletoepassing.

    1. Start **Visual Studio 2012-2013-2015**.
    1. Klik op **File**, houd de muisaanwijzer op **New** en klik op **Project**.
    2. Vouw **Templates** uit en selecteer **Visual C#**.
    3. Selecteer **Console Application** uit de lijst met projecttypen aan de rechterkant.
    4. Voer **DataTransformationApp** voor de **naam**.
    5. Selecteer **C:\DataTransformation** voor de **locatie**.
    6. Klik op **OK** om het project te maken.

4.  Voeg nu alle dll's aanwezig zijn in de [dll's](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) map als **verwijzingen** in het project dat u hebt gemaakt. Voor het downloaden van de dll-bestanden het volgende doen:

    1. In Visual Studio, gaat u naar **weergave > Solution Explorer**.
    1. Klik op de pijl naar links van Data Transformation App-project. Klik op **verwijzingen** en klik vervolgens met de rechtermuisknop op **verwijzing toevoegen**.
    2. Blader naar de locatie van de map pakketten, selecteert u alle dll's en op **toevoegen**, en klik vervolgens op **OK**.

5. Voeg de volgende **using**-instructies toe aan het bronbestand (Program.cs) in het project.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. De volgende code initialiseert het taakexemplaar voor transformatie van gegevens. Voeg deze in de **Main-methode**. Vervang de waarden van configuratieparameters als u eerder hebt verkregen. Sluit de waarden van **Resourcegroepnaam** en **hybride gegevens resourcenaam**. De **Resourcegroepnaam** die als host fungeert voor de hybride Gegevensresource waarop de taakdefinitie is geconfigureerd.

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

    ```

9. Deze taak wordt de overeenkomende bestanden aanwezig zijn onder de hoofdmap op het StorSimple-volume naar de opgegeven container geüpload. Wanneer een bestand is geüpload, wordt een bericht in de wachtrij (in hetzelfde opslagaccount als de container) verwijderd met dezelfde naam als de taakdefinitie. Dit bericht kan worden gebruikt als een trigger initiëren verdere verwerking van het bestand.

10. Zodra de taak is geactiveerd, voeg de volgende code om bij te houden van de taak voor voltooiing.

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


## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
