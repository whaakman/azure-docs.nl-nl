---
title: Indexeren van mediabestanden met Azure Media Indexer
description: Azure Media Indexer kunt u de inhoud van uw mediabestanden doorzoekbaar maken en voor het genereren van een volledig uitgeschreven tekstversie voor gesloten ondertiteling en sleutelwoorden. In dit onderwerp laat zien hoe Media Indexer gebruiken.
services: media-services
documentationcenter: ''
author: Asolanki
manager: cfowler
editor: ''
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: b6e0b2027dd4edbf1688a6c71efe2dde37fbbf96
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54473364"
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Indexeren van mediabestanden met Azure Media Indexer
Azure Media Indexer kunt u de inhoud van uw mediabestanden doorzoekbaar maken en voor het genereren van een volledig uitgeschreven tekstversie voor gesloten ondertiteling en sleutelwoorden. U kunt één mediabestand verwerken of meerdere mediabestanden als batch verwerken.  

> [!IMPORTANT]
> Wanneer u de inhoud indexeren, zorg er dan voor dat gebruikmaken van mediabestanden waarvoor wissen spraak (zonder achtergrondmuziek, geluid, effecten of microfoon hiss). Enkele voorbeelden van de betreffende inhoud zijn: vastgelegd vergaderingen, lezingen en presentaties. De volgende inhoud is mogelijk niet geschikt voor indexering: films, tv-programma's, alles met gemengde audio en geluid effecten, slecht opgenomen inhoud met achtergrondgeluiden (hiss).
> 
> 

Een indexeringstaak kunt de volgende uitvoer gegenereerd:

* Ondertitelingsbestand in de volgende indelingen: **SAMI**, **TTML**, en **WebVTT**.
  
    Ondertitelingsbestand bevatten een tag met de naam Recognizability, welke scores een indexeringstaak op basis van hoe herkenbare de gesproken tekst in de bronvideo.  U kunt de waarde van Recognizability scherm uitvoerbestanden voor gebruiksgemak. Een lage score betekent slechte indexering resultaten vanwege audio kwaliteit.
* Trefwoordenbestand (XML).
* Audio indexering van blob-bestand (AIB) voor gebruik met SQL server.
  
    Zie voor meer informatie, [AIB-bestanden met behulp van Azure Media Indexer en SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

In dit artikel wordt beschreven hoe maakt indexeertaken laten naar **indexeren van een asset** en **Index van meerdere bestanden**.

Zie voor de meest recente updates voor Azure Media Indexer, [Media Services-blogs](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Met behulp van configuratie-en-manifest voor indexering van taken
U kunt meer informatie voor uw indexering taken opgeven met behulp van de taakconfiguratie van een. U kunt bijvoorbeeld opgeven welke metagegevens te gebruiken voor uw media-bestand. Deze metagegevens wordt gebruikt door de engine taal om uit te breiden de vocabulaire en aanzienlijk verbetert de nauwkeurigheid van de spraakherkenning.  Bent u ook uw gewenste uitvoerbestanden opgeven.

U kunt ook meerdere mediabestanden in één keer verwerken met behulp van een manifestbestand.

Zie voor meer informatie, [taak vooraf voor Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Indexeren van een asset
De volgende methode uploadt een mediabestand als een asset en maakt u een taak voor het indexeren van de asset.

Als er is geen configuratiebestand is opgegeven, wordt het mediabestand geïndexeerd met alle standaardinstellingen.

```csharp
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
```

<!-- __ -->
### <a id="output_files"></a>Uitvoerbestanden
Standaard genereert een indexeringstaak de volgende uitvoerbestanden. De bestanden worden opgeslagen in de eerste uitvoerasset.

Wanneer er meer dan één invoer mediabestand, genereert de indexeerfunctie een manifestbestand voor de taakuitvoer, met de naam 'JobResult.txt'. Voor elk mediabestand invoerkaart, de resulterende AIB, SAMI, TTML, WebVTT en sleutelwoord-bestanden zijn sequentieel genummerd en met de naam met behulp van de "Alias."

| Bestandsnaam | Description |
| --- | --- |
| **InputFileName.aib** |Audio indexering blob-bestand. <br/><br/> Audiobestand indexeren Blob (AIB) is een binair bestand dat kan worden doorzocht in Microsoft SQL server met behulp van zoeken in volledige tekst.  De AIB-bestand is krachtiger zijn dan de eenvoudige bijschriftbestanden, omdat deze alternatieven voor elk woord, zodat een veel uitgebreider zoekervaring bevat. <br/> <br/>Dit is de installatie van de indexeerfunctie voor SQL-invoegtoepassing op een computer waarop Microsoft SQL server 2008 of hoger vereist. Zoeken naar de AIB met behulp van Microsoft SQL biedt server zoekopdracht in volledige tekst meer nauwkeurige zoekresultaten dan de ondertiteling-bestanden die worden gegenereerd door WAMI zoeken. Dit komt doordat de AIB word alternatieven die klinken bevat terwijl de ondertiteling-bestanden de hoogste betrouwbaarheid word voor elk segment van de audio bevatten. Als het zoeken naar gesproken woorden is van belang upmost, en vervolgens het verdient aanbeveling de AIB In combinatie met Microsoft SQL Server gebruiken.<br/><br/> Als u wilt de invoegtoepassing downloaden, klikt u op <a href="https://aka.ms/indexersql">SQL-invoegtoepassing van Azure Media Indexer</a>. <br/><br/>Het is ook mogelijk gebruikmaken van andere zoekmachines zoals Apache Lucene/Solr gewoon indexeren van de video die op basis van de ondertiteling en sleutelwoord XML-bestanden, maar dit leidt tot minder nauwkeurige zoekresultaten. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |Gesloten bijschrift (CC)-bestanden in SAMI, TTML en WebVTT-indeling.<br/><br/>Ze kunnen worden gebruikt om audio-of videobestanden toegankelijk voor mensen met een handicap horen.<br/><br/>Gesloten bijschriftbestanden bevatten een tag met de naam <b>Recognizability</b> die een indexeringstaak op basis van hoe herkenbaar is van de gesproken tekst in de bronvideo beoordeelt.  U kunt de waarde van <b>Recognizability</b> naar de uitvoerbestanden scherm voor gebruiksgemak. Een lage score betekent slechte indexering resultaten vanwege audio kwaliteit. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Sleutelwoord en info-bestanden. <br/><br/>Trefwoordenbestand is een XML-bestand sleutelwoorden die van de gesproken inhoud, met de frequentie en offset informatie bevat. <br/><br/>Bestand met scriptgegevens is een tekstbestand met gedetailleerde informatie over elke term wordt herkend. De eerste regel is bijzonder en de score Recognizability bevat. Elke volgende regel wordt een lijst met door tabs gescheiden van de volgende gegevens: starten van de tijd, eindtijd, woord/bepaalde woordgroep, vertrouwen. De tijden worden uitgedrukt in seconden en het vertrouwen wordt gegeven als een getal van 0-1. <br/><br/>Voorbeeld van de regel: "1.20 1,45 word 0.67" <br/><br/>Deze bestanden kunnen worden gebruikt voor verschillende doeleinden, zoals, voor het uitvoeren van spraakanalyse, of zoekmachines, zoals Bing, Google of Microsoft SharePoint om te zorgen dat de mediabestanden beter worden gedetecteerd, of zelfs gebruikt voor het leveren van meer relevante advertenties worden weergegeven. |
| **JobResult.txt** |Uitvoer manifest, aanwezig zijn alleen tijdens het indexeren van meerdere bestanden, met de volgende gegevens:<br/><br/><table border="1"><tr><th>InputFile</th><th>Alias</th><th>MediaLength</th><th>Fout</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Als u niet alle invoer mediabestanden worden geïndexeerd, de indexeringstaak mislukt met foutcode 4000. Zie voor meer informatie, [foutcodes](#error_codes).

## <a name="index-multiple-files"></a>Index van meerdere bestanden
De volgende methode meerdere mediabestanden als een asset uploadt, en maakt een taak voor het indexeren van deze bestanden in een batch.

Een manifestbestand met de extensie '.lst' is gemaakt en in de asset uploaden. Het manifestbestand bevat de lijst van alle assetbestanden. Zie voor meer informatie, [taak vooraf voor Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

```csharp
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
```

### <a name="partially-succeeded-job"></a>Gedeeltelijk geslaagd taak
Als dat niet alle invoer mediabestanden worden geïndexeerd, de indexeringstaak mislukt met foutcode 4000. Zie voor meer informatie, [foutcodes](#error_codes).

De dezelfde uitvoer (als geslaagd taken) worden gegenereerd. U kunt verwijzen naar het manifestbestand van de uitvoer wilt weten welke invoerbestanden zijn mislukt, op basis van de kolomwaarden fout. Voor de invoerbestanden dat is mislukt, de resulterende AIB, SAMI, TTML, WebVTT en trefwoord wordt bestanden niet worden gegenereerd.

### <a id="preset"></a> Taken vooraf instellen voor Azure Media Indexer
De verwerking van Azure Media Indexer kan worden aangepast door te geven van een optionele taak voorinstelling samen met de taak.  De volgende tabel ziet de indeling van deze configuratie-xml.

| Name | Require | Description |
| --- | --- | --- |
| **input** |false |Asset-bestanden die u wilt indexeren.</p><p>Azure Media Indexer biedt ondersteuning voor de volgende indelingen voor media: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>U kunt de naam van het bestand (s) opgeven de **naam** of **lijst** kenmerk van de **invoer** element (zoals hieronder wordt weergegeven). Als u niet welke assetbestand index opgeeft, wordt het primaire bestand opgehaald. Als er geen primaire assetbestand is ingesteld, wordt het eerste bestand in het invoeractivum geïndexeerd.</p><p>Als u wilt de naam van het asset expliciet opgeven, doet u het:<br/>`<input name="TestFile.wmv">`<br/><br/>U kunt ook meerdere assetbestanden in één keer (maximaal 10) index. Om dit te doen:<br/><br/><ol class="ordered"><li><p>Maak een tekstbestand (manifest-bestand) en wijs hieraan de extensie .lst. </p></li><li><p>Een lijst met alle namen van de asset-bestand in uw invoeractivum toevoegen aan deze manifestbestand. </p></li><li><p>(Uploaden) het manifest-bestand toevoegen aan de asset.  </p></li><li><p>Geef de naam van het manifestbestand in kenmerk van de invoer.<br/>`<input list="input.lst">`</li></ol><br/><br/>Opmerking: Als u meer dan 10 bestanden aan het manifestbestand toevoegt, mislukt de indexeringstaak met foutcode 2006. |
| **metadata** |false |Metagegevens voor de opgegeven asset-bestanden die worden gebruikt voor de Woordenlijstaanpassing.  Dit is handig om voor te bereiden indexeerfunctie voor het herkennen van niet-standaard vocabulaire woorden, zoals de juiste woorden.<br/>`<metadata key="..." value="..."/>` <br/><br/>U kunt opgeven **waarden** voor vooraf gedefinieerde **sleutels**. De volgende sleutels worden momenteel ondersteund:<br/><br/>"title" en 'description' - gebruikt voor de woordenlijstaanpassing aanpassen van de taal model van uw taak en de nauwkeurigheid van de spraakherkenning te verbeteren.  De waarden seed-zoekacties op Internet om te zoeken contextueel relevante tekstdocumenten, met behulp van de inhoud om te verbeteren van de interne woordenlijst voor de duur van de Indexing-taak.<br/>`<metadata key="title" value="[Title of the media file]" />`<br/>`<metadata key="description" value="[Description of the media file] />"` |
| **Functies** <br/><br/> Toegevoegd in versie 1.2. Op dit moment is het enige ondersteunde kenmerk spraakherkenning ('ASR'). |false |De functie voor spraakherkenning heeft de volgende instellingen voor sleutels:<table><tr><th><p>Sleutel</p></th>        <th><p>Description</p></th><th><p>Voorbeeldwaarde</p></th></tr><tr><td><p>Taal</p></td><td><p>De natuurlijke taal moet worden herkend in het multimediabestand.</p></td><td><p>Engels, Spaans</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>een door puntkomma's gescheiden lijst van de gewenste uitvoer bijschrift-indelingen (indien aanwezig)</p></td><td><p>ttml;sami;webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Een Booleaanse vlag die aangeeft of een AIB-bestand (voor gebruik met SQL Server en de klant indexeerfunctie IFilter) vereist is.  Zie voor meer informatie, <a href="https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">AIB-bestanden met behulp van Azure Media Indexer en SQL Server</a>.</p></td><td><p>True; False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Een Booleaanse vlag die aangeeft of de XML-bestand van een sleutelwoord vereist is.</p></td><td><p>True; False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Een Booleaanse vlag die aangeeft of om af te dwingen volledige bijschriften (ongeacht vertrouwensniveau) of niet.  </p><p>Standaard is ingesteld op false, in welk geval woorden en zinnen die kleiner dan 50% vertrouwensniveau hebben zijn weggelaten uit de uitvoer van de laatste bijschrift en vervangen door de weglatingstekens ('...').  De weglatingstekens zijn handig voor kwaliteitscontrole bijschrift en controle.</p></td><td><p>True; False. </p></td></tr></table> |

### <a id="error_codes"></a>Foutcodes
In het geval van een fout op Azure Media Indexer wilt laten rapporteren back-ups maken een van de volgende foutcodes:

| Code | Name | Mogelijke oorzaken |
| --- | --- | --- |
| 2000 |Ongeldige configuratie |Ongeldige configuratie |
| 2001 |Ongeldige invoer activa |Invoer activa of lege asset ontbreekt. |
| 2002 |Ongeldig manifest |Het manifest is leeg of manifest bevat ongeldige items. |
| 2003 |Mediabestand is niet gedownload |Ongeldige URL in het manifestbestand. |
| 2004 |Niet-ondersteund protocol |Protocol van media-URL wordt niet ondersteund. |
| 2005 |Niet-ondersteund bestandstype |Invoer mediabestandstype wordt niet ondersteund. |
| 2006 |Te veel invoerbestanden |Er zijn meer dan 10 bestanden in het manifest van de invoer. |
| 3000 |Mediabestand decoderen is mislukt |Niet-ondersteunde mediacodec <br/>of<br/> Beschadigd bestand <br/>of<br/> Er is geen audiostream in de invoer media. |
| 4000 |Indexeren van batch voltooid gedeeltelijk |Enkele van de invoer mediabestanden worden kan niet worden geïndexeerd. Zie voor meer informatie, <a href="#output_files">uitvoerbestanden</a>. |
| andere |Interne fouten |Neem contact op met voor ondersteuningsteam. indexer@microsoft.com |

## <a id="supported_languages"></a>Ondersteunde talen
Op dit moment worden het Engels en Spaans talen ondersteund. Zie voor meer informatie, [het blogbericht van v1.2 release](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services Analytics-overzicht](media-services-analytics-overview.md)

[AIB-bestanden met Azure Media Indexer en SQL Server gebruiken](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indexeren van mediabestanden met Azure Media Indexer 2-Preview](media-services-process-content-with-indexer2.md)

