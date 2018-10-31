---
title: 'Microsoft Genomics: problemen oplossen met | Microsoft Docs'
titleSuffix: Azure
description: Meer informatie over strategieën voor probleemoplossing
keywords: problemen oplossen, fout, foutopsporing
services: microsoft-genomics
author: ruchir
editor: jasonwhowell
ms.author: ruchir
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 10/29/2018
ms.openlocfilehash: 2c10259e4b9fa180d09ceef0359e7ec99e8200b1
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239896"
---
# <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen

Hier volgen enkele tips voor probleemoplossing voor enkele van de algemene problemen die u bij het gebruik van de service Microsoft Genomics, MSGEN kan tegenkomen.

 Voor Zie veelgestelde vragen, niet met betrekking tot het oplossen van problemen [Veelgestelde vragen over](frequently-asked-questions-genomics.md).
## <a name="step-1-locate-error-codes-associated-with-the-workflow"></a>Stap 1: Zoek foutcodes die zijn gekoppeld aan de werkstroom

U vindt de foutberichten die zijn gekoppeld aan de werkstroom door:

1. Via de opdrachtregel en in te typen  `msgen status`
2. De inhoud van standardoutput.txt wordt gecontroleerd.

### <a name="1-using-the-command-line-msgen-status"></a>1. Via de opdrachtregel `msgen status`

```bash
msgen status -u URL -k KEY -w ID 
```




Er zijn drie vereiste argumenten:

* URL - basis-URI voor de API
* SLEUTEL - de toegangssleutel voor uw Genomics-account
    * Als u wilt uw URL en de sleutel vinden, gaat u naar Azure portal en open de Microsoft Genomics-account-pagina. Onder de **Management** kop, kies **toegangssleutels**. U vindt er, zowel de API-URL en toegangssleutels van uw.

  
* ID - de werkstroom-ID
    * Om te vinden van het type van de werkstroom-ID in `msgen list` opdracht. Op dezelfde locatie bevinden als uw msgen exe, ervan uitgaande dat uw config-bestand bevat de URL en toegangssleutels van uw en zich bevindt is de opdracht er als volgt: 
        
        ```bash
        msgen list -f "config.txt"
        ```
        Uitvoer van deze opdracht er als volgt:
        
        ```bash
            Microsoft Genomics command-line client v0.7.4
                Copyright (c) 2018 Microsoft. All rights reserved.
                
                Workflow List
                -------------
                Total Count  : 1
                
                Workflow ID     : 10001
                Status          : Completed successfully
                Message         :
                Process         : snapgatk-20180730_1
                Description     :
                Created Date    : Mon, 27 Aug 2018 20:27:24 GMT
                End Date        : Mon, 27 Aug 2018 20:55:12 GMT
                Wall Clock Time : 0h 27m 48s
                Bases Processed : 1,348,613,600 (1 GBase)
        ```

 > [!NOTE]
 >  U kunt ook het pad naar het configuratiebestand in plaats van rechtstreeks invoeren van de URL en de sleutel opnemen. Als u deze argumenten in de opdrachtregel, evenals de config-bestand opgeeft, wordt de opdrachtregelargumenten voorrang.  

Werkstroom-ID 1001 en config.txt-bestand geplaatst in hetzelfde pad hebben als de msgen uitvoerbare, er de opdracht als volgt:

```bash
msgen status -w 1001 -f "config.txt"
```

### <a name="2--examine-the-contents-of-standardoutputtxt"></a>2.  Bekijk de inhoud van standardoutput.txt 
Ga naar de uitvoercontainer voor de werkstroom in kwestie. Hiermee maakt u MSGEN, `[workflowfilename].logs.zip` map na elke werkstroomuitvoering. Pak deze uit de map om de inhoud ervan weer te geven:

* outputFileList.txt - een lijst van de uitvoerbestanden die tijdens de werkstroom worden geproduceerd
* StandardError.txt - dit bestand is leeg.
* StandardOutput.txt - registreert alle op het hoogste niveau statusberichten, met inbegrip van fouten die zijn opgetreden tijdens het uitvoeren van de werkstroom.
* Logboekbestanden GATK - alle andere bestanden in de `logs` map

Bekijk de inhoud van standardoutput.txt en Let op eventuele foutberichten die worden weergegeven voor het oplossen van problemen.


## <a name="step-2-try-recommended-steps-for-common-errors"></a>Stap 2: Probeer de aanbevolen stappen uit voor veelvoorkomende fouten

In deze sectie worden kort veelvoorkomende fouten uitvoer door Microsoft Genomics-service (msgen) en de strategieën die u kunt ze op te lossen. 

De service Microsoft Genomics (msgen) kan de volgende twee soorten fouten genereren:

1. Interne servicefouten: Fouten die intern voor de service, die mogelijk niet worden omgezet zijn door parameters of de invoerbestanden op te lossen. Soms opnieuw de werkstroom kan deze fouten zijn opgelost.
2. Invoerfouten: Fouten die kunnen worden opgelost met behulp van de juiste argumenten of vaststelling bestandsindelingen.

### <a name="1-internal-service-errors"></a>1. Interne servicefouten

Een interne servicefout is geen gebruiker bruikbare. U kunt de werkstroom opnieuw verzenden, maar als dit niet werkt, neem dan contact op met ondersteuning voor Microsoft Genomics

| Foutbericht                                                                                                                            | Aanbevolen stappen voor probleemoplossing                                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Er is een interne fout opgetreden. Probeer de werkstroom opnieuw indienen. Als u deze fout opnieuw ziet, contact op met Microsoft Genomics-ondersteuning voor hulp | De werkstroom opnieuw indienen. Neem contact op met Microsoft Genomics ondersteuning voor hulp als het probleem zich blijft voordoen door het maken van een [ticket](file-support-ticket-genomics.md ). |

### <a name="2-input-errors"></a>2. Invoerfouten

Deze fouten worden bruikbare gebruiker. Op basis van het type van bestands- en foutcode, voert Microsoft Genomics-service verschillende foutcodes. Volg de aanbevolen stappen hieronder vermeld.

| Type bestand | Foutcode | Foutbericht                                                                           | Aanbevolen stappen voor probleemoplossing                                                                                         |
|--------------|------------|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| Alle          | 701        | Lezen [readId] heeft [numberOfBases] bases, maar de limiet is [maxReadLength]           | De meest voorkomende reden voor deze fout is het bestand beschadigd leidt tot een samenvoeging van twee leesbewerkingen. Controleer uw invoerbestanden. |                                |
| BAM          | 200        |   Kan niet lezen van bestand [yourFileName].                                                                                       | Controleer de indeling van de BAM-bestand. De werkstroom opnieuw verzenden met een correct opgemaakt bestand.                                                                           |
| BAM          | 201        |  Kan niet lezen BAM-bestand [bestandsnaam].                                                                                      |Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                            |
| BAM          | 202        | Kan niet lezen BAM-bestand [bestandsnaam]. Bestand te klein en ontbrekende header.                                                                                        | Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                            |
| BAM          | 203        |   Kan niet lezen BAM-bestand [bestandsnaam]. Koptekst van bestand is beschadigd.                                                                                      |Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                           |
| BAM          | 204        |    Kan niet lezen BAM-bestand [bestandsnaam]. Koptekst van bestand is beschadigd.                                                                                     | Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                           |
| BAM          | 205        |    Kan niet lezen BAM-bestand [bestandsnaam]. Koptekst van bestand is beschadigd.                                                                                     | Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                            |
| BAM          | 206        |   Kan niet lezen BAM-bestand [bestandsnaam]. Koptekst van bestand is beschadigd.                                                                                      | Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                            |
| BAM          | 207        |  Kan niet lezen BAM-bestand [bestandsnaam]. Het bestand is afgekapt bij offset [offset].                                                                                       | Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                            |
| BAM          | 208        |   Ongeldige BAM-bestand. De ReadID [Read_Id] heeft geen reeks in [bestandsnaam]-bestand.                                                                                      | Controleer de indeling van de BAM-bestand.  De werkstroom verzenden met een correct opgemaakt bestand.                                                                             |
| FASTQ        | 300        |  Kan niet lezen van FASTQ-bestand. [Bestandsnaam] eindigt niet met een nieuwe regel.                                                                                     | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                           |
| FASTQ        | 301        |   Kan niet lezen van FASTQ-bestand [bestandsnaam]. FASTQ-record is groter dan buffergrootte bij offset: [_offset]                                                                                      | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                         |
| FASTQ        | 302        |     Fout bij het FASTQ-syntaxis. [Bestandsnaam]-bestand heeft een lege regel.                                                                                    | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                         |
| FASTQ        | 303        |       Fout bij het FASTQ-syntaxis. Bestand [bestandsnaam] heeft een ongeldig teken beginnen bij verschuiving: [_offset], regel typt: [line_type] tekens: [_char]                                                                                  | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                         |
| FASTQ        | 304      |  FASTQ syntaxisfout op readID [_ReadID].  Eerste lezen van batch geen readID hebben die eindigt op /1 in het bestand [bestandsnaam]                                                                                       | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                         |
| FASTQ        | 305        |  FASTQ syntaxisfout op readID [_readID]. Tweede lezen van batch geen readID hebben die eindigt op /2 in het bestand [bestandsnaam]                                                                                      | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                          |
| FASTQ        | 306        |  FASTQ syntaxisfout op readID [_ReadID]. Eerste lezen van een paar van beschikt niet over een ID die op /1 in het bestand [bestandsnaam eindigt]                                                                                       | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                          |
| FASTQ        | 307        |   FASTQ syntaxisfout op readID [_ReadID]. ReadID eindigt niet met /1 of / 2. Bestand [bestandsnaam] kan niet worden gebruikt als een gekoppelde FASTQ-bestand.                                                                                      |Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                          |
| FASTQ        | 308        |  FASTQ lezen fout bij. Leesbewerkingen van beide kanten anders heeft gereageerd. U de juiste FASTQ-bestanden kiezen?                                                                                       | Corrigeer de indeling van de FASTQ-bestand en de werkstroom opnieuw indienen.                                                                         |
|        |       |                                                                                        |                                                                           |

## <a name="step-3-contact-microsoft-genomics-support"></a>Stap 3: Neem contact op met Microsoft Genomics-ondersteuning

Als u nog steeds taakfouten hebt, of als u andere vragen hebt, neem dan contact op met ondersteuning voor Microsoft Genomics in Azure portal. Meer informatie over hoe u een ondersteuningsaanvraag indienen vindt [hier](file-support-ticket-genomics.md).

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u problemen op te lossen van veelvoorkomende problemen met de Microsoft Genomics-service. Zie voor meer informatie en veelgestelde vragen over de algemene [Veelgestelde vragen over](frequently-asked-questions-genomics.md). 
