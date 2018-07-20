---
title: 'Microsoft Genomics: problemen oplossen met | Microsoft Docs'
titleSuffix: Azure
description: Meer informatie over strategieën voor probleemoplossing
keywords: problemen oplossen, fout, foutopsporing
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 07/18/2018
ms.openlocfilehash: 9bd1690003fd37b6c2edd0f0421cf8d0e74f8cb5
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144173"
---
# <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
In dit overzicht beschrijft strategieën voor het oplossen van veelvoorkomende problemen met bij het gebruik van de service Microsoft Genomics. Zie voor veelgestelde vragen over algemene, [Veelgestelde vragen over](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>Hoe kan ik mijn taakstatus controleren?
U kunt de status van uw werkstroom controleren door het aanroepen van `msgen status` vanaf de opdrachtregel, zoals wordt weergegeven. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Er zijn drie vereiste argumenten:
* URL - basis-URI voor de API
* SLEUTEL - de toegangssleutel voor uw Genomics-account. 
* ID - de werkstroom-ID

Als u wilt uw URL en de sleutel vinden, gaat u naar Azure portal en open de pagina van uw Genomics-account. Onder de **Management** kop, kies **toegangssleutels**. U vindt er, zowel de API-URL en toegangssleutels van uw.

U kunt ook het pad naar het configuratiebestand in plaats van rechtstreeks invoeren van de URL en de sleutel opnemen. Houd er rekening mee dat als u deze argumenten in de opdrachtregel, evenals de config-bestand opgeeft, de opdrachtregelargumenten voorrang. 

Na het aanroepen `msgen status`, een gebruiksvriendelijke bericht wordt weergegeven, waarin wordt beschreven of de werkstroom is voltooid of geven een reden op voor de taak is mislukt. 


## <a name="get-more-information-about-my-workflow-status"></a>Meer informatie over de status van mijn workflow

Voor meer informatie over waarom een taak niet mogelijk zijn geslaagd, kunt u de logboekbestanden die worden geproduceerd tijdens de werkstroom kunt verkennen. In de uitvoercontainer ziet u een `[youroutputfilename].logs.zip` map.  Deze map uitpakken, ziet u de volgende items:

* outputFileList.txt - een lijst van de uitvoerbestanden die tijdens de werkstroom worden geproduceerd
* StandardError.txt - dit bestand is leeg.
* StandardOutput.txt - bevat op het hoogste niveau van logboekregistratie van de werkstroom. 
* Logboekbestanden GATK - alle andere bestanden in de `logs` map

De `standardoutput.txt` bestand is een goede plaats om te starten om te bepalen waarom de werkstroom is mislukt, omdat deze informatie op laag niveau van de werkstroom bevat. 

## <a name="common-issues-and-how-to-resolve-them"></a>Veelvoorkomende problemen en hoe u ze op te lossen
In deze sectie kort worden veelvoorkomende problemen en hoe u ze op te lossen.

### <a name="fastq-files-are-unmatched"></a>Fastq-bestanden zijn niet-overeenkomende
Fastq-bestanden moeten alleen verschillen van de afsluitende /1 of /2 in de voorbeeld-id. Als u niet-overeenkomende FASTQ-bestanden per ongeluk hebt ingediend, ziet u mogelijk de volgende foutberichten bij het aanroepen van `msgen status`.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

U kunt dit oplossen controleren als de fastq-bestanden dat is verzonden naar de werkstroom werkelijk een overeenkomende reeks. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>Fout bij uploaden van bestand .bam. Er bestaat al een uitvoer-blob en de optie voor objectoverschrijving is ingesteld op False.
Als u het volgende foutbericht ziet `Error uploading .bam file. Output blob already exists and the overwrite option was set to False`, de map voor uitvoer bevat al een bestand voor uitvoer met dezelfde naam.  Verwijder het bestaande bestand voor uitvoer of schakel de optie voor overschrijven in het configuratiebestand. Verzend uw werkstroom.

### <a name="when-to-contact-microsoft-genomics-support"></a>Wanneer u contact op met ondersteuning voor Microsoft Genomics
Als u de volgende foutberichten ziet, is een interne fout opgetreden. 

* `Error locating input files on worker machine`
* `Process management failure`

Probeer het opnieuw indienen van uw werkstroom. Als u nog steeds taakfouten hebt, of als u andere vragen hebt, neem dan contact op met ondersteuning voor Microsoft Genomics in Azure portal. Meer informatie over hoe u een ondersteuningsaanvraag indienen vindt [hier](file-support-ticket-genomics.md).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u problemen op te lossen van veelvoorkomende problemen met de Microsoft Genomics-service. Zie voor meer informatie en veelgestelde vragen over de algemene [Veelgestelde vragen over](frequently-asked-questions-genomics.md). 
