---
title: Zelfstudie voor het kopiëren van gegevens via SMB op Azure Data Box zware | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens naar uw Azure Data Box zware via SMB
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 05/28/2019
ms.author: alkohli
ms.openlocfilehash: 6bc7af30d409fb7add321953aa2e956a0a3c1840
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427772"
---
# <a name="tutorial-copy-data-to-azure-data-box-heavy-via-smb-preview"></a>Zelfstudie: Gegevens kopiëren naar Azure Data Box zware via SMB (preview)

Deze zelfstudie beschrijft hoe u verbinding kunt maken en gegevens kunt kopiëren vanaf uw hostcomputer via de lokale gebruikersinterface.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verbinding maken met Data Box-zwaar
> * Gegevens kopiëren naar Data Box-zwaar


## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de zelfstudie [ Instellen van Azure Data Box zware](data-box-deploy-set-up.md).
2. U hebt uw Data Box-zwaar ontvangen en de status van de volgorde in de portal **geleverd**.
3. U hebt een hostcomputer waarmee de gegevens die u kopiëren wilt via Data Box zware heeft. Op uw hostcomputer moet
    - Een [ondersteund besturingssysteem](data-box-system-requirements.md) worden uitgevoerd.
    - Verbonden zijn met een netwerk met hoge snelheid. Voor de snelste kopie snelheden worden bereikt, kunnen twee 40-GbE-verbindingen (één per knooppunt) gelijktijdig worden gebruikt. Als u geen 40-GbE-verbinding beschikbaar hebt, wordt u aangeraden dat u ten minste twee 10 GbE-verbindingen (één per knooppunt hebt).

## <a name="connect-to-data-box-heavy-shares"></a>Verbinding maken met gegevens in het zware shares

Op basis van het opslagaccount dat is geselecteerd, gegevens in het zware wordt gemaakt tot:
- Drie shares voor elk gekoppeld opslagaccount voor GPv1 en GPv2.
- Een share voor een premium-opslag.
- Een share voor blob storage-account.

Deze shares worden gemaakt op beide knooppunten van het apparaat.

Onder shares voor block blob en pagina-blob:
- Eerste niveau entiteiten zijn containers.
- Op het tweede niveau entiteiten zijn-blobs.

Onder de shares voor Azure Files:
- Eerste niveau entiteiten zijn shares.
- Op het tweede niveau entiteiten zijn bestanden.

De volgende tabel ziet u het UNC-pad naar de shares op uw gegevens in het zware en Azure Storage pad naar de URL waar de gegevens is geüpload. De uiteindelijke URL van het Azure Storage-pad kan worden afgeleid van het UNC-pad naar de shares.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure-blok-blobs | <li>UNC-pad naar shares: `\\<DeviceIPAddress>\<StorageAccountName_BlockBlob>\<ContainerName>\files\a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure-pagina-blobs  | <li>UNC-pad naar shares: `\\<DeviceIPAddres>\<StorageAccountName_PageBlob>\<ContainerName>\files\a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Files       |<li>UNC-pad naar shares: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>\files\a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |      

De stappen voor het verbinding maken met behulp van een Windows- of een client voor Linux zijn verschillend.

> [!NOTE]
> Volg dezelfde stappen om te verbinden met beide knooppunten van het apparaat parallel.

### <a name="connect-on-a-windows-system"></a>Verbinding maken op een Windows-systeem

Als een computer met Windows Server-host, volgt u deze stappen voor het verbinding maken met de Data Box-zwaar.

1. U moet eerst een verificatie uitvoeren en een sessie starten. Ga naar **Verbinding maken en kopiëren**. Klik op **Referenties ophalen** om de toegangsreferenties te krijgen voor de shares die aan uw opslagaccount zijn gekoppeld.

    ![Sharereferenties 1 ophalen](media/data-box-heavy-deploy-copy-data/get-share-credentials-1.png)

2. Kopieer in het dialoogvenster Verbinding maken met share en gegevens kopiëren de **Gebruikersnaam** en het **Wachtwoord** voor de share. Klik op **OK**.
    
    ![Sharereferenties 1 ophalen](media/data-box-heavy-deploy-copy-data/get-share-credentials-2.png)

3. Voor toegang tot de shares die zijn gekoppeld aan uw storage-account (*databoxe2etest* in het volgende voorbeeld) vanaf uw hostcomputer, open een opdrachtvenster. Typ in de opdrachtprompt:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Afhankelijk van de gegevensindeling, zijn dit de paden voor de shares:
    - Azure-blok-blob - `\\10.100.10.100\databoxe2etest_BlockBlob`
    - Azure-pagina-blob - `\\10.100.10.100\databoxe2etest_PageBlob`
    - Azure Files - `\\10.100.10.100\databoxe2etest_AzFile`
    
4. Voer het wachtwoord voor de share in wanneer er om wordt gevraagd. Het volgende voorbeeld laat zien hoe u verbinding met een share kunt maken via de voorgaande opdracht.

    ```
    C:\Users\Databoxuser>net use \\10.100.10.100\databoxe2etest_BlockBlob /u:databoxe2etest
    Enter the password for 'databoxe2etest' to connect to '10.100.10.100':
    The command completed successfully.
    ```

4. Druk op Windows-toets+R. Geef in het venster **Uitvoeren** het `\\<device IP address>` op. Klik op **OK** om Verkenner te openen.
    
    ![Verbinding met de share maken via Verkenner 2](media/data-box-heavy-deploy-copy-data/connect-shares-file-explorer-1.png)

    Als het goed is, worden de shares nu weergegeven als mappen.
    
    ![Verbinding met de share maken via Verkenner 2](media/data-box-heavy-deploy-copy-data/connect-shares-file-explorer-2.png)

    **Maak altijd een map voor de bestanden die u van plan bent te kopiëren in de bestandsshare en kopieer de bestanden vervolgens naar die map**. De map gemaakt onder shares met blok-blobs en pagina-blobs vertegenwoordigt een container waarnaar gegevens als blobs worden geüpload. Het is niet mogelijk om bestanden rechtstreeks te kopiëren naar de *root*-map in het opslagaccount.
    
### <a name="connect-on-a-linux-system"></a>Verbinding maken op een Linux-systeem

Als u een Linux-client gebruikt, gebruikt u de volgende opdracht om de SMB-share koppelen.

```
sudo mount -t nfs -o vers=2.1 10.126.76.172:/databoxe2etest_BlockBlob /home/databoxubuntuhost/databox
```

De `vers` parameter is de versie van SMB die ondersteuning biedt voor uw Linux-host. Sluit de juiste versie in de bovenstaande opdracht.

Zie voor versies van SMB die ondersteuning biedt voor de Data Box-zwaar [bestandssystemen ondersteund voor clients met Linux](data-box-heavy-system-requirements.md#supported-file-systems-for-linux-clients).

## <a name="copy-data-to-data-box-heavy"></a>Gegevens kopiëren naar Data Box-zwaar

Als u met de gegevens in het zware shares verbonden bent, wordt de volgende stap is om gegevens te kopiëren.

### <a name="copy-considerations"></a>Overwegingen met betrekking tot kopiëren

Neem de volgende punten door voordat u gegevens gaat kopiëren:

- Zorg dat u de gegevens kopieert naar shares die overeenkomen met de juiste gegevensindeling. U moet bijvoorbeeld de blok-blobgegevens naar de share voor blok-blobs kopiëren. Kopieer de VHD's naar pagina-blob.

    Als de gegevensindeling niet overeenkomt met het betreffende sharetype, zal het uploaden van gegevens naar Azure op een later tijdstip mislukken.
-  Tijdens het kopiëren van gegevens, zorg ervoor dat de grootte van de voldoet aan de maximale grootte wordt beschreven in de [Azure-opslag en gegevens in het zware limieten](data-box-heavy-limits.md).
- Als gegevens die door Data Box-zwaar wordt geüpload, gelijktijdig door andere toepassingen buiten de gegevens in het zware is geüpload, kan dit uploaden taak fouten en beschadigde gegevens leiden.
- We raden aan dat:
    - U niet zowel SMB als NFS tegelijkertijd gebruikt.
    - Dezelfde gegevens naar dezelfde eindbestemming kopieert in Azure.
     
  In dergelijke gevallen kan de definitieve uitkomst namelijk niet worden vastgesteld.
- Maak altijd een map voor de bestanden die u van plan bent te kopiëren in de bestandsshare en kopieer de bestanden vervolgens naar die map. De map gemaakt onder shares met blok-blobs en pagina-blobs vertegenwoordigt een container waarnaar gegevens als blobs worden geüpload. Het is niet mogelijk om bestanden rechtstreeks te kopiëren naar de *root*-map in het opslagaccount.

Begin met het kopiëren van gegevens nadat u verbinding met de SMB-share hebt gemaakt.

1. U kunt elk programma voor het kopiëren van bestanden dat compatibel is met SMB, zoals Robocopy, gebruiken om de gegevens te kopiëren. Er kunnen meerdere kopieertaken worden gestart met Robocopy. Gebruik de volgende opdracht:
    
    ```
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile>
    ```
    De kenmerken worden in de volgende tabel beschreven.
    
    |Kenmerk  |Description  |
    |---------|---------|
    |/e      |Hiermee kopieert u submappen, met inbegrip van lege mappen.         |
    |/r:     |Hiermee geeft u het aantal nieuwe pogingen bij mislukte kopieerbewerkingen op.         |
    |/w:     |Hiermee geeft u de wachttijd tussen nieuwe pogingen op in seconden.         |
    |/is     |Neemt dezelfde bestanden op.         |
    |/nfl    |Hiermee geeft u op dat bestandsnamen niet moeten worden vastgelegd.         |
    |/ndl    |Hiermee geeft u op dat mapnamen niet moeten worden vastgelegd.        |
    |/np     |Geeft op dat de voortgang van de kopieerbewerking (het aantal bestanden of mappen dat tot nu toe is gekopieerd) niet zal worden weergegeven. Als de voortgang wordt weergegeven, zullen de prestaties aanzienlijk verminderen.         |
    |/MT     | Gebruik multithreading, 32 of 64-threads wordt aanbevolen. Deze optie wordt niet gebruikt voor versleutelde bestanden. U moet versleutelde en niet-versleutelde bestanden mogelijk scheiden. Kopiëren met één thread verlaagt de prestaties echter aanzienlijk.           |
    |/fft    | Gebruik deze optie om de granulariteit van tijdstempels voor een bestandssysteem te verminderen.        |
    |/b      | Kopieert bestanden in de back-upmodus.        |
    |/z      | Kopieert bestanden in de modus voor opnieuw opstarten; gebruik deze optie als de omgeving instabiel is. Deze optie beperkt doorvoer vanwege de aanvullende logboekregistratie.      |
    | /zb    | Gebruikt de modus voor opnieuw opstarten. Deze optie gebruikt de back-upmodus als de toegang is geweigerd. Deze optie beperkt doorvoer vanwege controlepunten.         |
    |/efsraw | Kopieert alle versleutelde bestanden in de onbewerkte EFS-modus. Alleen gebruiken met versleutelde bestanden.         |
    |log+:\<LogFile>| Voegt de uitvoer toe aan het bestaande logboekbestand.|
    
 
    Het volgende voorbeeld wordt de uitvoer van de robocopy-opdracht bestanden naar de Data Box-zwaar te kopiëren.

    ```   
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.100.10.100\devicemanagertest1_AzFile\templates /MT:24
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
        Started : Thursday, April 4, 2019 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
        Dest : \\10.100.10.100\devicemanagertest1_AzFile\templates\
        Files : *.*
        Options : *.* /DCOPY:DA /COPY:DAT /MT:24 /R:5 /W:60
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
    C:\Users>
    ```       

2. Gebruik de volgende parameters in Robocopy om de prestaties te optimaliseren als u de gegevens kopieert. (De onderstaande getallen vertegenwoordigen de beste use-casescenario's.)

    | Platform    | Voornamelijk kleine bestanden van < 512 KB    | Voornamelijk middelgrote bestanden 512 KB - 1 MB  | Voornamelijk grote bestanden van > 1 MB                             |
    |-------------|--------------------------------|----------------------------|----------------------------|
    | Data Box Heavy | 6 Robocopy-sessies <br> 24 threads per sessie | 6 Robocopy-sessies <br> 16 threads per sessie | 6 Robocopy-sessies <br> 16 threads per sessie |


    Ga voor meer informatie over opdrachten voor Robocopy naar [Robocopy en een paar voorbeelden](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

3. Open de doelmap om de gekopieerde bestanden weer te geven en te controleren.

    ![Gekopieerde bestanden weergeven](media/data-box-heavy-deploy-copy-data/view-copied-files-1.png)


4. Als de gegevens worden gekopieerd:

    - De namen van bestanden, grootten en -indeling worden gevalideerd om ervoor te zorgen die voldoen aan de limieten voor Azure-object en de opslag, evenals de Azure-bestand en de container naamconventies.
    - Controlesom is om te controleren of de integriteit van gegevens, ook berekende inline.

    Download de foutbestanden om problemen op te lossen als er fouten zijn opgetreden tijdens het kopiëren. Selecteer het pijlpictogram om de foutbestanden te downloaden.

    ![Foutbestanden downloaden](media/data-box-heavy-deploy-copy-data/download-error-files.png)

    Zie voor meer informatie, [foutenlogboeken weergeven tijdens het kopiëren van gegevens naar Data Box zware](data-box-logs.md#view-error-log-during-data-copy-to-data-box). Zie voor een gedetailleerde lijst met fouten tijdens het kopiëren van gegevens, [Data Box zware oplossen van problemen met](data-box-troubleshoot.md).

5. Open het foutbestand in Kladblok. De volgende fout bij het bestand geeft aan dat de gegevens niet goed wordt uitgelijnd.

    ![Fout bij openen bestand](media/data-box-heavy-deploy-copy-data/open-error-file.png)
    
    Voor een pagina-blob moet de gegevens 512 bytes uitgelijnd. Nadat u deze gegevens worden verwijderd, wordt de fout omgezet zoals wordt weergegeven in de volgende schermafbeelding.

    ![Fout opgelost](media/data-box-heavy-deploy-copy-data/error-resolved.png)

6. Nadat de kopie voltooid is, gaat u naar **Dashboard weergeven** pagina. Controleer of de gebruikte ruimte en de vrije ruimte op uw apparaat.
    
    ![Vrije en ongebruikte ruimte verifiëren op het dashboard](media/data-box-heavy-deploy-copy-data/verify-used-space-dashboard.png)

Herhaal de bovenstaande stappen voor het kopiëren van gegevens naar het tweede knooppunt van het apparaat.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over Azure Data Box zware onderwerpen, zoals:

> [!div class="checklist"]
> * Verbinding maken met Data Box-zwaar
> * Gegevens kopiëren naar Data Box-zwaar


Ga naar de volgende zelfstudie voor meer informatie over uw gegevens in het zware naar Microsoft verzenden.

> [!div class="nextstepaction"]
> [Uw Azure Data Box-zwaar naar Microsoft verzenden](./data-box-heavy-deploy-picked-up.md)

