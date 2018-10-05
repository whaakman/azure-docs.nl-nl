---
title: Gegevens kopiëren naar uw Microsoft Azure Data Box-schijf| Microsoft Docs
description: In deze zelfstudie leest u hoe u gegevens kopieert naar uw Azure Data Box-schijf
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 776f70b6b24288006d52cb0e91797d1074180160
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452612"
---
# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Zelfstudie: gegevens kopiëren naar de Azure Data Box-schijf en deze gegevens controleren

In deze zelfstudie wordt beschreven hoe u vanaf uw hostcomputer gegevens kopieert en vervolgens controlesommen genereert om de gegevensintegriteit te controleren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gegevens kopiëren naar de Data Box-schijf
> * Gegevens controleren

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:
- U hebt de [zelfstudie: Uw Azure Data Box-schijf installeren en configureren](data-box-disk-deploy-set-up.md) voltooid.
- Uw schijven worden ontgrendeld en verbonden met een clientcomputer.
- Er moet een [ondersteund besturingssysteem](data-box-disk-system-requirements.md) worden uitgevoerd op de clientcomputer die wordt gebruikt om gegevens naar de schijven te kopiëren.


## <a name="copy-data-to-disks"></a>Gegevens naar schijven kopiëren

Voer de volgende stappen uit om verbinding te maken en gegevens van uw computer naar de Data Box-schijf te kopiëren.

1. Geef de inhoud van het ontgrendelde station weer. 

    ![De inhoud van het station weergeven](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Kopieer de gegevens die als blok-blobs moeten worden geïmporteerd in de BlockBlob-map. Kopieer VHD-/VHDX-gegevens naar de PageBlob-map. 

    Er wordt voor elke submap onder de BlockBlob- en PageBlob-mappen een container gemaakt in het Azure-opslagaccount. Alle bestanden onder de mappen BlockBlob en PageBlob worden naar een standaardcontainer `$root` onder het Azure-opslagaccount gekopieerd. Bestanden in de `$root`-container worden altijd als blok-blobs geüpload.

    Als er in de hoofdmap bestanden en mappen voorkomen, moet u deze naar een andere map verplaatsen voordat u met het kopiëren van gegevens begint.

    Houd u aan de Azure-naamgevingsvereisten voor container- en blobnamen.

    #### <a name="azure-naming-conventions-for-container-and-blob-names"></a>Azure-naamconventies voor namen van containers en blobs
    |Entiteit   |Conventies  |
    |---------|---------|
    |Namen van containers voor blok-blobs en pagina-blobs     |Moeten beginnen met een letter of cijfer en mogen alleen kleine letters, cijfers en het koppelteken (-) bevatten. Elk koppelteken (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. In namen worden geen opeenvolgende koppeltekens toegestaan. <br>Moet een geldige DNS-naam zijn die tussen de 3 en 63 tekens bevat.          |
    |Blobnamen voor blok-blob en pagina-blob    |Blobnamen zijn hoofdlettergevoelig en kunnen elke combinatie van tekens bevatten. <br>Een blobnaam moet 1 tot 1024 tekens bevatten.<br>Gereserveerde tekens voor URL's moeten op de juiste wijze van een escape-teken zijn voorzien.<br>Het aantal padsegmenten dat de blobnaam omvat, mag niet meer dan 254 zijn. Een padsegment is de tekenreeks tussen opeenvolgende scheidingstekens (bijvoorbeeld de slash '/') die overeenkomt met de naam van een virtuele map.         |

    > [!IMPORTANT] 
    > Alle containers en blobs moeten voldoen aan de [Azure-naamgevingsregels](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions). Als u zich niet aan deze regels houdt, mislukt de gegevensupload naar Azure.

3. Wanneer u bestanden kopieert, moet u ervoor zorgen dat de bestanden de ~4,7 TiB voor blok-blobs en ~8 TiB voor pagina-blobs niet overschrijden. 
4. U kunt door middel van slepen en neerzetten de gegevens vanuit Verkenner kopiëren. U kunt ook elk programma voor het kopiëren van bestanden dat compatibel is met SMB, zoals Robocopy, gebruiken om de gegevens te kopiëren. Er kunnen meerdere kopieertaken worden gestart met de volgende Robocopy-opdracht:

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    De parameters en opties voor de opdracht worden in de volgende tabel weergegeven:
    
    |Parameters/opties  |Beschrijving |
    |--------------------|------------|
    |Bron            | Hiermee geeft u het pad naar de bronmap op.        |
    |Doel       | Hiermee geeft u het pad naar de doelmap op.        |
    |/E                  | Hiermee kopieert u submappen, met inbegrip van lege mappen. |
    |/MT[:N]             | Hiermee maakt u kopieën met meerdere (N) threads, waarbij N een geheel getal tussen 1 en 128 is. <br>De standaardwaarde voor N is 8.        |
    |/R: <N>             | Hiermee geeft u het aantal nieuwe pogingen bij mislukte kopieerbewerkingen op. De standaardwaarde van N is 1.000.000 (één miljoen nieuwe pogingen).        |
    |/W: <N>             | Hiermee geeft u de wachttijd tussen nieuwe pogingen op in seconden. De standaardwaarde van N is 30 (wachttijd 30 seconden).        |
    |/NFL                | Hiermee geeft u op dat bestandsnamen niet moeten worden vastgelegd.        |
    |/NDL                | Hiermee geeft u op dat mapnamen niet moeten worden vastgelegd.        |
    |/FFT                | Hiermee wordt uitgegaan van FAT-bestandstijden (precisie van twee seconden).        |
    |/Log:<Log File>     | Hiermee schrijft u de statusuitvoer naar het logboekbestand (en overschrijft u het bestaande logboekbestand).         |

    Er kunnen gelijktijdig meerdere schijven worden gebruikt terwijl er op elke schijf meerdere taken worden uitgevoerd. 

6. Controleer de kopieerstatus wanneer de taak wordt uitgevoerd. In het volgende voorbeeld ziet u de uitvoer van de Robocopy-opdracht voor het kopiëren van bestanden naar de Data Box-schijf.

    ```
    C:\Users>robocopy
        -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:53 PM
           Simple Usage :: ROBOCOPY source destination /MIR
    
                 source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                   /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                   Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
       Files :        17        17         0         0         0         0
       Bytes :     3.9 k     3.9 k         0         0         0         0
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    
7. Open de doelmap om de gekopieerde bestanden weer te geven en te controleren. Als er fouten zijn opgetreden tijdens het kopiëren, downloadt u de logboekbestanden om de problemen op te lossen. De logboekbestanden bevinden zich op de locatie die in de Robobopy-opdracht is aangegeven.
 


> [!IMPORTANT]
> - Het is uw verantwoordelijkheid om ervoor te zorgen dat u de gegevens kopieert naar mappen met de juiste gegevensindeling. U moet bijvoorbeeld de blok-blobgegevens naar de map voor blok-blobs kopiëren. Als de gegevensindeling niet overeenkomt met de betreffende map (opslagtype), zal de gegevensupload naar Azure op een later tijdstip mislukken.
> -  Zorg er tijdens het kopiëren van gegevens voor dat de gegevensgrootte voldoet aan de limieten die staan beschreven in de [limieten voor Azure-opslag en Data Box-schijven](data-box-disk-limits.md). 
> - Als de gegevens die door Data Box Disk worden geüpload gelijktijdig door andere toepassingen buiten Data Box Disk worden geüpload, kan dit tot fouten voor de uploadtaak en beschadigde gegevens leiden.

## <a name="verify-data"></a>Gegevens controleren 

Voer de volgende stappen uit om de gegevens te controleren.

1. Voer `DataBoxDiskValidation.cmd` uit in de map *AzureImportExport* van het station om de controlesom te controleren. 
    
    ![Uitvoer van validatieprogramma van Data Box Disk](media/data-box-disk-deploy-copy-data/data-box-disk-validation-tool-output.png)

2. Kies de juiste optie. **Wij adviseren om altijd optie 2 te selecteren om de bestanden te valideren en controlesommen te genereren**. Afhankelijk van de gegevensgrootte kan deze stap enige tijd in beslag nemen. Als het script is voltooid, sluit u het opdrachtvenster. Als er fouten optreden tijdens de validatie en het genereren van de controlesom, krijgt u hiervan een melding en ziet u ook een koppeling naar de foutenlogboeken.

    ![Uitvoer van de controlesom](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

    > [!TIP]
    > - Reset het programma tussen twee uitvoeringen.
    > - Gebruik optie 1 om bestanden te valideren als het gaat om een grote gegevensset met alleen kleine bestanden (~ kB). In deze gevallen kan het genereren van een controlesom erg lang duren en de prestaties dus tegenvallen.

3. Als u meerdere schijven gebruikt, voert u de opdracht uit voor elke schijf.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot de Azure Data Box-schijf, zoals:

> [!div class="checklist"]
> * Gegevens kopiëren naar de Data Box-schijf
> * De gegevensintegriteit controleren

Ga naar de volgende zelfstudie om te lezen hoe u de Data Box-schijf retourneert en de gegevensupload naar Azure controleert.

> [!div class="nextstepaction"]
> [Uw Azure Data Box-schijf retourneren naar Microsoft](./data-box-disk-deploy-picked-up.md)

