---
title: Mediabestanden met Azure Media Indexer indexeren
description: Azure Media Indexer kunt u de inhoud van uw mediabestanden doorzoekbaar maken en voor het genereren van de tekst van een volledige-tekstindex voor gesloten ondertiteling en sleutelwoorden. Dit onderwerp leest het gebruik van Media indexeerfunctie.
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: f75be3280ffd869339972859c028a178ec728480
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Mediabestanden met Azure Media Indexer indexeren
Azure Media Indexer kunt u de inhoud van uw mediabestanden doorzoekbaar maken en voor het genereren van de tekst van een volledige-tekstindex voor gesloten ondertiteling en sleutelwoorden. U kunt één mediabestand verwerken of meerdere mediabestanden als batch verwerken.  

> [!IMPORTANT]
> Wanneer u inhoud, zorg er dan voor dat gebruikmaken van mediabestanden die duidelijk speech (zonder achtergrondmuziek, ruis, effecten of microfoon hiss) hebben. Enkele voorbeelden van de betreffende inhoud zijn: vastgelegd vergaderingen, lezingen en presentaties. De volgende inhoud is mogelijk niet geschikt voor indexering: films, televisieprogramma alles met gemengde Beeld en geluid effecten slecht opgenomen inhoud met achtergrondruis (hiss).
> 
> 

Een indexing-taak kan de volgende uitvoer gegenereerd:

* Gesloten bijschrift bestanden in de volgende indelingen: **SAMI**, **TTML**, en **WebVTT**.
  
    Ondertitelingsbestanden bestanden bevatten een tag Recognizability, welke scores een indexing-taak op basis van hoe herkenbare de spraak in de bronvideo-is aangeroepen.  U kunt de waarde van Recognizability scherm uitvoerbestanden voor bruikbaarheid. Een lage score betekent slechte indexering resultaten vanwege audio kwaliteit.
* Trefwoordenbestand (XML).
* Audio indexeren van blob-bestand (AIB) voor gebruik met SQL server.
  
    Zie voor meer informatie [AIB-bestanden gebruiken met Azure Media Indexer en SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

In dit onderwerp wordt beschreven hoe maakt indexeertaken naar **Index van een asset** en **Index meerdere bestanden**.

Zie voor de meest recente updates voor het Azure Media Indexer [Media Services-blogs](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Met behulp van configuratie-en manifest voor indexering van taken
U kunt meer details opgeven voor uw indexering taken met behulp van de taakconfiguratie van een. U kunt bijvoorbeeld opgeven dat welke metagegevens moet worden gebruikt voor het mediabestand. Deze metagegevens door de taal-engine wordt gebruikt om uit te breiden de woordenlijst en de nauwkeurigheid van de spraakherkenning aanzienlijk verbeterd.  Bent u ook uw gewenste uitvoerbestanden opgeven.

U kunt ook meerdere mediabestanden tegelijk verwerken met behulp van een manifestbestand.

Zie voor meer informatie [taak vooraf voor Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Index van een asset
De volgende methode uploadt een mediabestand als een actief en maakt een taak voor het indexeren van de asset.

Houd er rekening mee dat als er geen configuratiebestand is opgegeven, het mediabestand wordt geïndexeerd met alle standaardinstellingen.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Uitvoerbestanden
Standaard worden de volgende uitvoerbestanden gegenereerd door een indexing-taak. De bestanden worden opgeslagen in de eerste uitvoerasset.

Wanneer er meer dan één invoer mediabestand, wordt de indexeerfunctie een manifestbestand voor de taak uitvoer, met de naam 'JobResult.txt' gegenereerd. Voor elk mediabestand invoer, worden de resulterende AIB, SAMI TTML, WebVTT en sleutelwoord bestanden, opeenvolgend genummerd en met de naam met de 'Alias'.

| Bestandsnaam | Beschrijving |
| --- | --- |
| **InputFileName.aib** |Audio-indexering blob-bestand. <br/><br/> Audiobestand Blob indexeren (AIB) is een binair bestand dat kan worden doorzocht in Microsoft SQL server met behulp van zoeken in volledige tekst.  Het bestand AIB is krachtiger dan de bestanden eenvoudige bijschrift omdat hierin alternatieven voor elk woord, zodat een veel rijkere ervaring voor de zoekopdracht. <br/> <br/>De installatie van de indexeerfunctie SQL-invoegtoepassing op een computer waarop Microsoft SQL server 2008 of hoger vereist. Zoeken naar de AIB met behulp van Microsoft SQL biedt server zoekopdracht in volledige tekst nauwkeuriger zoekresultaten dan zoeken naar de ondertiteling-bestanden die worden gegenereerd door WAMI. Dit komt doordat de AIB word alternatieven welke vergelijkbaar geluid bevat terwijl de ondertiteling-bestanden de hoogste vertrouwen word voor elk segment van de audio bevatten. Als u zoeken naar gesproken woorden is van belang zijn upmost, verdient het aanbeveling voor het gebruik van de AIB In combinatie met Microsoft SQL Server.<br/><br/> De invoegtoepassing downloaden, klikt u op <a href="http://aka.ms/indexersql">SQL-invoegtoepassing voor Azure Media Indexer</a>. <br/><br/>Het is ook mogelijk gebruikmaken van andere zoekmachines zoals Apache Lucene/Solr gewoon index van de video op basis van de ondertiteling en sleutelwoord XML-bestanden, maar dit leidt tot minder nauwkeurig zoekresultaten. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |Gesloten bijschrift (CC)-bestanden in SAMI TTML en WebVTT indelingen.<br/><br/>Ze kunnen worden gebruikt om ervoor te audio-en toegankelijk voor mensen met een handicap horen.<br/><br/>Gesloten bijschrift bestanden bevatten een tag aangeroepen <b>Recognizability</b> die een indexing-taak op basis van hoe herkenbaar is van de spraak in de bronvideo scores.  U kunt de waarde van <b>Recognizability</b> naar scherm uitvoerbestanden voor bruikbaarheid. Een lage score betekent slechte indexering resultaten vanwege audio kwaliteit. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Sleutelwoord en info-bestanden. <br/><br/>Trefwoordenbestand is een XML-bestand dat trefwoorden geëxtraheerd uit de inhoud spraak met frequentie en offset informatie bevat. <br/><br/>Bestand met scriptgegevens is een tekstbestand bevat gedetailleerde informatie over elke term herkend. De eerste regel is speciaal en de score Recognizability bevat. Elke volgende regel wordt een lijst met door tabs gescheiden van de volgende gegevens: start de tijd, eindtijd, woord of woordgroep, vertrouwen. De tijden worden vermeld in seconden en het vertrouwen wordt weergegeven als een getal tussen 0-1. <br/><br/>Voorbeeld van de regel: '1,20 1,45 word 0.67' <br/><br/>Deze bestanden worden gebruikt voor een aantal doeleinden, zoals spraak analytics, uitvoeren of zoekmachines zoals Bing, Google of Microsoft SharePoint voor het maken van de mediabestanden beter kunnen worden gevonden, of zelfs gebruikt voor het afleveren van meer relevante advertenties worden weergegeven. |
| **JobResult.txt** |Uitvoer manifest, alleen beschikbaar wanneer er meerdere bestanden, met de volgende gegevens te indexeren:<br/><br/><table border="1"><tr><th>Invoerbestand</th><th>Alias</th><th>MediaLength</th><th>Fout</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Als dat niet alle invoer mediabestanden worden geïndexeerd, de indexering taak mislukt met foutcode 4000. Zie voor meer informatie [foutcodes](#error_codes).

## <a name="index-multiple-files"></a>Index van meerdere bestanden
De volgende methode meerdere mediabestanden als een asset uploadt, en maakt een taak voor het indexeren van deze bestanden in een batch.

Een manifestbestand met de extensie .lst is gemaakt en in de asset uploaden. Het manifestbestand bevat de lijst van alle assetbestanden. Zie voor meer informatie [taak vooraf voor Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Gedeeltelijk geslaagd taak
Als dat niet alle invoer mediabestanden worden geïndexeerd, de indexering taak mislukt met foutcode 4000. Zie voor meer informatie [foutcodes](#error_codes).

De dezelfde uitvoer (als geslaagd taken) worden gegenereerd. U kunt verwijzen naar het uitvoerbestand manifest om erachter te komen welke invoerbestanden, volgens de kolomwaarden fout mislukt. GEEN er wordt bestanden invoerbestanden die zijn mislukt, de resulterende AIB, SAMI, TTML, WebVTT en sleutelwoord gegenereerd.

### <a id="preset"></a>Taak definitie voor Azure Media Indexer
De verwerking van Azure Media Indexer kan worden aangepast door een optionele taak voorinstelling samen met de taak.  De volgende tabel ziet de indeling van deze configuratie-xml.

| Naam | Require | Beschrijving |
| --- | --- | --- |
| **invoer** |ONWAAR |Asset-bestanden die u wilt indexeren.</p><p>Azure Media Indexer ondersteunt de volgende bestandsindelingen van media: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Kunt u de bestandsnaam (s) in de **naam** of **lijst** kenmerk van de **invoer** element (zoals hieronder wordt weergegeven). Als u niet welke assetbestand naar index opgeeft, wordt het primaire bestand opgenomen. Als er geen primaire assetbestand is ingesteld, wordt het eerste bestand in de invoer asset geïndexeerd.</p><p>Geef expliciet de bestandsnaam van de asset door het volgende te doen:<br/>`<input name="TestFile.wmv">`<br/><br/>U kunt ook meerdere assetbestanden tegelijk (maximaal 10) index. Om dit te doen:<br/><br/><ol class="ordered"><li><p>Maak een tekstbestand (manifestbestand) en geef deze extensie .lst. </p></li><li><p>Een lijst met alle namen van de asset-bestand in uw invoer asset toevoegen aan deze manifestbestand. </p></li><li><p>(Uploaden) thanifest bestand toevoegen aan de asset.  </p></li><li><p>Geef de naam van het manifestbestand in de invoer lijst kenmerk.<br/>`<input list="input.lst">`</li></ol><br/><br/>Opmerking: Als u meer dan 10 bestanden aan het manifestbestand toevoegt, de indexering taak mislukt met foutcode 2006. |
| **metagegevens** |ONWAAR |De metagegevens voor de opgegeven asset die u gebruikt voor aanpassing van de woordenlijst.  Dit is handig voor het voorbereiden van de indexeerfunctie voor het herkennen van niet-standaard woordenlijst woorden zoals namen.<br/>`<metadata key="..." value="..."/>` <br/><br/>U kunt opgeven **waarden** voor vooraf gedefinieerde **sleutels**. De volgende sleutels worden momenteel ondersteund:<br/><br/>"title" en "beschrijving' - gebruikt voor aanpassing van woordenlijst voor aanpassen van de taal model van uw werk en spraakherkenning verbeteren.  De waarden seed-zoekacties op Internet om te zoeken contextueel relevante tekstdocumenten verbeteren van de interne woordenlijst voor de duur van de taak indexering met behulp van de inhoud.<br/>`<metadata key="title" value="[Title of the media file]" />`<br/>`<metadata key="description" value="[Description of the media file] />"` |
| **functies** <br/><br/> Toegevoegd in versie 1.2. De enige ondersteunde functie is momenteel spraakherkenning ('ASR'). |ONWAAR |De functie Spraakherkenning heeft de sleutels van de volgende instellingen:<table><tr><th><p>Sleutel</p></th>        <th><p>Beschrijving</p></th><th><p>Voorbeeldwaarde</p></th></tr><tr><td><p>Taal</p></td><td><p>De natuurlijke taal in het bestand multimedia worden herkend.</p></td><td><p>Engels, Spaans</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>een door puntkomma's gescheiden lijst van de gewenste bijschrift uitvoerindelingen (indien aanwezig)</p></td><td><p>ttml; sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Een Booleaanse vlag die aangeeft of een AIB-bestand (voor gebruik met SQL Server en de klant indexeerfunctie IFilter) vereist is.  Zie voor meer informatie <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">AIB-bestanden gebruiken met Azure Media Indexer en SQL Server</a>.</p></td><td><p>Waar is; De waarde False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Een Booleaanse vlag die aangeeft of een sleutelwoord XML-bestand vereist is.</p></td><td><p>Waar is; De waarde False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Een Booleaanse vlag die aangeeft of om af te dwingen volledige bijschriften (ongeacht vertrouwensniveau) of niet.  </p><p>Standaard is ingesteld op false, in dat geval woorden en woordgroepen waarvoor een kleiner dan 50% vertrouwensniveau worden weggelaten uit de uitvoer van de laatste bijschrift en vervangen door de weglatingstekens ('...').  Het beletselteken zijn handig voor beheer op bijschrift kwaliteit en controle.</p></td><td><p>Waar is; De waarde False. </p></td></tr></table> |

### <a id="error_codes"></a>Foutcodes
In het geval van een fout Azure Media Indexer verslag moet één van de volgende foutcodes:

| Code | Naam | Mogelijke oorzaken |
| --- | --- | --- |
| 2000 |Ongeldige configuratie |Ongeldige configuratie |
| 2001 |Ongeldige invoer activa |Invoer activa of lege asset ontbreekt. |
| 2002 |Ongeldig manifest |Het manifest is leeg of het manifest ongeldig items bevat. |
| 2003 |Downloaden van mediabestand is mislukt |Ongeldige URL in het manifestbestand. |
| 2004 |Niet-ondersteund protocol |Protocol van de URL van de media wordt niet ondersteund. |
| 2005 |Niet-ondersteund bestandstype |Type invoer mediabestand wordt niet ondersteund. |
| 2006 |Te veel invoerbestanden |Er zijn meer dan 10 bestanden in het manifest van de invoer. |
| 3000 |Mediabestand decoderen is mislukt |De codec wordt niet ondersteund <br/>of<br/> Beschadigde mediabestand <br/>of<br/> Er is geen audiostroom invoer media. |
| 4000 |Indexeren van batch gedeeltelijk is voltooid |Sommige van de invoer mediabestanden zijn kan niet worden geïndexeerd. Zie voor meer informatie <a href="#output_files">uitvoerbestanden</a>. |
| andere |Interne fouten |Neem contact op met het ondersteuningsteam van. indexer@microsoft.com |

## <a id="supported_languages"></a>Ondersteunde talen
Op dit moment is worden het Engels en Spaans talen ondersteund. Zie voor meer informatie [het blogbericht versie 1.2 release](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[AIB bestanden met Azure Media Indexer en SQL Server gebruiken](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indexeren van Media-bestanden met Azure Media Indexer 2 Preview](media-services-process-content-with-indexer2.md)

