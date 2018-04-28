---
title: 'Microsoft genomica: Troubleshooting Guide | Microsoft Docs'
titleSuffix: Azure
description: Meer informatie over probleemoplossing, strategieën
keywords: het oplossen van problemen, fout, foutopsporing
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/13/2018
ms.openlocfilehash: 18761c02cc423affe7b1050700e560b1f0b0594d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen
Dit overzicht beschrijft strategieën voor veelvoorkomende problemen bij het gebruik van de service Microsoft Genomics. Raadpleeg voor algemene FAQ [Veelgestelde vragen](frequently-asked-questions-genomics.md). 


## <a name="how-do-i-check-my-job-status"></a>Hoe kan ik mijn taakstatus controleren?
U kunt de status van uw werkstroom controleren door het aanroepen van `msgen status` vanaf de opdrachtregel, zoals wordt weergegeven. 

```
msgen status -u URL -k KEY -w ID [-f CONFIG] 
```

Er zijn drie vereiste argumenten:
* URL - basis-URI voor de API
* SLEUTEL - de toegangssleutel voor uw account genomica. 
* ID - werkstroom-ID

Uw URL en de sleutel niet vinden, gaat u naar de Azure-portal en uw accountpagina genomica openen. Onder de **Management** kopje kiezen **toegangssleutels**. U zowel de API-URL en toegangssleutels van uw vinden.

U kunt ook het pad naar het configuratiebestand wordt in plaats van rechtstreeks invoeren van de URL en de sleutel opnemen. Houd er rekening mee dat als u deze argumenten in de opdrachtregel, evenals het configuratiebestand opgeeft, de opdrachtregelargumenten voorrang. 

Na het aanroepen `msgen status`, een gebruiksvriendelijke bericht wordt weergegeven, met een beschrijving van de werkstroom is voltooid of of geven een reden voor de taak is mislukt. 


## <a name="get-more-information-about-my-workflow-status"></a>Meer informatie over de status van mijn workflow

Als u meer informatie over waarom een taak niet mogelijk zijn geslaagd, vindt u de logboekbestanden die tijdens de werkstroom wordt geproduceerd. In de uitvoer-container, ziet u een `[youroutputfilename].logs.zip` map.  Deze map ritsen, ziet u de volgende items:

* outputFileList.txt - een lijst van de uitvoerbestanden geproduceerd tijdens de werkstroom
* StandardError.txt - dit bestand is leeg.
* StandardOutput.txt - bevat op het hoogste niveau van logboekregistratie van de werkstroom. 
* Logboekbestanden GATK - alle andere bestanden in de `logs` map

De `standardoutput.txt` bestand is een goede plaats om te bepalen waarom de werkstroom is mislukt, omdat het bevat meer informatie op laag niveau van de werkstroom. 

## <a name="common-issues-and-how-to-resolve-them"></a>Veelvoorkomende problemen en hoe u deze kunt oplossen
Deze sectie kort worden veelvoorkomende problemen en hoe u deze kunt oplossen.

### <a name="fastq-files-are-unmatched"></a>Fastq bestanden komen niet overeen
Fastq bestanden moeten alleen verschillen door de afsluitende /1 of /2 in de voorbeeld-id. Als u niet-overeenkomende FASTQ bestanden per ongeluk hebt ingediend, ziet u mogelijk de volgende foutberichten weergegeven bij het aanroepen van `msgen status`.
* `Encountered an unmatched read`
* `Error reading a FASTQ file, make sure the input files are valid and paired correctly` 

U kunt dit oplossen door controleren als de fastq-bestanden dat is verzonden naar de werkstroom werkelijk een overeenkomende set. 


### <a name="error-uploading-bam-file-output-blob-already-exists-and-the-overwrite-option-was-set-to-false"></a>Fout bij het uploaden van .bam bestand. Er bestaat al een uitvoer-blob en de optie voor objectoverschrijving is ingesteld op False.
Als u het volgende foutbericht weergegeven, ziet `Error uploading .bam file. Output blob already exists and the overwrite option was set to False`, de map voor uitvoer bevat al een bestand voor uitvoer met dezelfde naam.  Verwijder het bestaande bestand voor uitvoer of schakelt u de optie voor overschrijven in het configuratiebestand. Verzend uw werkstroom.

### <a name="when-to-contact-microsoft-genomics-support"></a>Wanneer u contact op met Microsoft Genomics-ondersteuning
Als u de volgende foutberichten ziet, is een interne fout opgetreden. 

* `Error locating input files on worker machine`
* `Process management failure`

Probeer opnieuw te verzenden van uw werkstroom. Als u nog steeds taakfouten hebt, of als u andere vragen hebt, moet u contact op met Microsoft genomica ondersteuning van de Azure-portal.

![Neem contact op met ondersteuning voor Azure-portal](./media/troubleshooting-guide/genomics-contact-support.png "contact op met ondersteuning op Azure-portal")

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe algemene problemen met de service Microsoft Genomics kunt oplossen. Zie voor meer informatie en veelgestelde vragen over de algemene [Veelgestelde vragen](frequently-asked-questions-genomics.md). 