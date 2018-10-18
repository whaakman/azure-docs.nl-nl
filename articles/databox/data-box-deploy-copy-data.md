---
title: Gegevens kopiëren naar uw Microsoft Azure Data Box| Microsoft Docs
description: Leer hoe u gegevens kopieert naar uw Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 10/10/2018
ms.author: alkohli
ms.openlocfilehash: b59830677ac8c07c6b7adbab24c82ca25d71f5a0
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093456"
---
# <a name="tutorial-copy-data-to-azure-data-box"></a>Zelfstudie: Gegevens kopiëren naar Azure Data Box 

Deze zelfstudie beschrijft hoe u verbinding kunt maken en gegevens kunt kopiëren vanaf uw hostcomputer via de lokale gebruikersinterface, waarna u de Data Box gereed kunt maken voor verzending.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verbinding maken met Data Box
> * Gegevens kopiëren naar Data Box
> * Data Box voorbereiden voor verzending.

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de [zelfstudie: Azure Data Box instellen](data-box-deploy-set-up.md) voltooid.
2. U hebt uw Data Box ontvangen en de bestelstatus in de portal is bijgewerkt naar **Geleverd**.
3. U hebt een hostcomputer waarop de gegevens staan die u naar de Data Box wilt kopiëren. Op uw hostcomputer moet
    - Een [ondersteund besturingssysteem](data-box-system-requirements.md) worden uitgevoerd.
    - Verbonden zijn met een netwerk met hoge snelheid. Het wordt aangeraden dat u beschikt over minstens één 10 GbE-verbinding. Als er geen 10 GbE-verbinding beschikbaar is, kan een 1 GbE-gegevenskoppeling worden gebruikt, maar dit heeft invloed op de kopieersnelheden. 

## <a name="connect-to-data-box"></a>Verbinding maken met Data Box

Data Box maakt op basis van het geselecteerde opslagaccount maximaal:
- Drie shares voor elk gekoppeld opslagaccount voor GPv1 en GPv2.
- Een share voor premium- of blob-opslagaccount. 

Onder blok-blob- en pagina-blob-shares zijn entiteiten op het eerste niveau containers en entiteiten op het tweede niveau blobs. Onder shares voor Azure Files zijn entiteiten op het eerste niveau shares en entiteiten op het tweede niveau bestanden.

Overweeg het volgende scenariovoorbeeld. 

- Opslagaccount: *Mystoracct*
- Share voor blok-blob: *Mystoracct_BlockBlob/my-container/blob*
- Share voor pagina-blob: *Mystoracct_BlockBlob/my-container/blob*
- Share voor bestand: *Mystoracct_AzFile/my-share*

De stappen om verbinding te maken en gegevens te kopiëren kunnen verschillen en zijn afhankelijk van of uw Data Box met een Windows Server-hostcomputer of Linux-host is verbonden.

### <a name="connect-via-smb"></a>Verbinding maken via SMB 

Voer de volgende stappen uit om verbinding met de Data Box te maken als u een Windows Server-hostcomputer gebruikt.

1. U moet eerst een verificatie uitvoeren en een sessie starten. Ga naar **Verbinding maken en kopiëren**. Klik op **Referenties ophalen** om de toegangsreferenties te krijgen voor de shares die aan uw opslagaccount zijn gekoppeld. 

    ![Sharereferenties 1 ophalen](media/data-box-deploy-copy-data/get-share-credentials1.png)

2. Kopieer in het dialoogvenster Verbinding maken met share en gegevens kopiëren de **Gebruikersnaam** en het **Wachtwoord** voor de share. Klik op **OK**.
    
    ![Sharereferenties 1 ophalen](media/data-box-deploy-copy-data/get-share-credentials2.png)

3. Open de shares die aan uw opslagaccount zijn gekoppeld (Mystoracct in het volgende voorbeeld). Gebruik het pad `\\<IP of the device>\ShareName` om toegang tot de shares te krijgen. Maak, afhankelijk van uw gegevensindeling, verbinding met de shares (gebruik de naam van de share) op het volgende adres: 
    - *\\<IP address of the device>\Mystoracct_Blob*
    - *\\<IP address of the device>\Mystoracct_Page*
    - *\\<IP address of the device>\Mystoracct_AzFile*
    
    Open een opdrachtvenster om verbinding te maken met de shares vanaf uw hostcomputer. Typ in de opdrachtprompt:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Voer het wachtwoord voor de share in wanneer er om wordt gevraagd. Het volgende voorbeeld laat zien hoe u verbinding met een share kunt maken via de voorgaande opdracht.

    ```
    C:\Users\Databoxuser>net use \\10.126.76.172\devicemanagertest1_BlockBlob /u:devicemanagertest1
    Enter the password for 'devicemanagertest1' to connect to '10.126.76.172':
    The command completed successfully.
    ```

4. Druk op Windows-toets+R. Geef in het venster **Uitvoeren** het `\\<device IP address>` op. Klik op **OK**. Verkenner wordt nu geopend. U moet nu de shares kunnen zien in de vorm van mappen.
    
    ![Verbinding met de share maken via Verkenner 2](media/data-box-deploy-copy-data/connect-shares-file-explorer1.png)

5.  **Maak altijd een map voor de bestanden die u van plan bent te kopiëren in de bestandsshare en kopieer de bestanden vervolgens naar die map**. De mappen kunnen af en toe een grijs kruis weergeven. Het kruis wijst niet op een fout. De mappen worden door de toepassing gemarkeerd om de status bij te houden.
    
    ![Verbinding met de share maken via Verkenner 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) ![Verbinding met de share maken via Verkenner 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) 

### <a name="connect-via-nfs"></a>Verbinding maken via NFS 

Voer de volgende stappen uit om Data Box zo te configureren dat toegang tot NFS-clients wordt toegestaan als u een Linux-hostcomputer gebruikt.

1. Geef de IP-adressen op van de clients die toegang hebben tot de share. Ga in de lokale gebruikersinterface naar de pagina **Verbinding maken en kopiëren**. Klik onder **NFS-instellingen** op **NFS-clienttoegang**. 

    ![NFS-clienttoegang configureren 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. Geef het IP-adres op van de NFS-client en klik op **Toevoegen**. U kunt deze stap herhalen om toegang voor meerdere NFS-clients te configureren. Klik op **OK**.

    ![NFS-clienttoegang configureren 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Zorg dat er een [ondersteunde versie](data-box-system-requirements.md) van de NFS-client op de Linux-hostcomputer is geïnstalleerd. Gebruik de specifieke versie voor uw Linux-distributie. 

3. Gebruik na het installeren van de NFS-client de volgende opdracht om de NFS-share op uw Data Box-apparaat te koppelen:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Het volgende voorbeeld toont hoe u via NFS verbinding maakt met een Data Box-share. Het IP-adres van het Data Box-apparaat is `10.161.23.130`, de share `Mystoracct_Blob` wordt gekoppeld op de Ubuntu-VM, het koppelpunt is `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`


## <a name="copy-data-to-data-box"></a>Gegevens kopiëren naar Data Box

Nadat u verbinding met de Data Box-shares hebt gemaakt, moet u de gegevens kopiëren. Neem de volgende overwegingen door voordat u gegevens kopieert:

- Zorg dat u de gegevens kopieert naar shares die overeenkomen met de juiste gegevensindeling. U moet bijvoorbeeld de blok-blobgegevens naar de share voor blok-blobs kopiëren. Als de gegevensindeling niet met het betreffende sharetype overeenkomt, zal het uploaden van gegevens naar Azure op een later tijdstip mislukken.
-  Zorg er tijdens het kopiëren van gegevens voor dat de gegevensgrootte voldoet aan de limieten die staan beschreven in de [limieten voor Azure-opslag en Data Box](data-box-limits.md). 
- Als de gegevens die door Data Box worden geüpload gelijktijdig door andere toepassingen buiten Data Box worden geüpload, kan dit tot fouten voor de uploadtaak en beschadigde gegevens leiden.
- We adviseren u om niet SMB en NFS tegelijk te gebruiken of om dezelfde gegevens naar dezelfde eindbestemming in Azure te kopiëren. In dergelijke gevallen kan de definitieve uitkomst niet worden vastgesteld.

### <a name="copy-data-via-smb"></a>Gegevens kopiëren via SMB

Start met het kopiëren van gegevens nadat u verbinding met de SMB-share hebt gemaakt. 

U kunt elk programma voor het kopiëren van bestanden dat compatibel is met SMB, zoals Robocopy, gebruiken om de gegevens te kopiëren. Er kunnen meerdere kopieertaken worden gestart met Robocopy. Gebruik de volgende opdracht:
    
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile> 
  
 De kenmerken worden in de volgende tabel beschreven.
    
|Kenmerk  |Beschrijving  |
|---------|---------|
|/e     |Hiermee kopieert u submappen, met inbegrip van lege mappen.         |
|/r:     |Hiermee geeft u het aantal nieuwe pogingen bij mislukte kopieerbewerkingen op.         |
|/w:     |Hiermee geeft u de wachttijd tussen nieuwe pogingen op in seconden.         |
|/is     |Neemt dezelfde bestanden op.         |
|/nfl     |Hiermee geeft u op dat bestandsnamen niet moeten worden vastgelegd.         |
|/ndl    |Hiermee geeft u op dat mapnamen niet moeten worden vastgelegd.        |
|/np     |Geeft op dat de voortgang van de kopieerbewerking (het aantal bestanden of mappen dat tot nu toe is gekopieerd) niet zal worden weergegeven. Als de voortgang wordt weergegeven, zullen de prestaties aanzienlijk verminderen.         |
|/MT     | Gebruik multithreading, 32 of 64-threads wordt aanbevolen. Deze optie wordt niet gebruikt voor versleutelde bestanden. U moet versleutelde en niet-versleutelde bestanden mogelijk scheiden. Kopiëren met één thread verlaagt de prestaties echter aanzienlijk.           |
|/fft     | Gebruik deze optie om de granulariteit van tijdstempels voor een bestandssysteem te verminderen.        |
|/b     | Kopieert bestanden in de back-upmodus.        |
|/z    | Kopieert bestanden in de modus voor opnieuw opstarten; gebruik deze optie als de omgeving instabiel is. Deze optie beperkt doorvoer vanwege de aanvullende logboekregistratie.      |
| /zb     | Gebruikt de modus voor opnieuw opstarten. Deze optie gebruikt de back-upmodus als de toegang is geweigerd. Deze optie beperkt doorvoer vanwege controlepunten.         |
|/efsraw     | Kopieert alle versleutelde bestanden in de onbewerkte EFS-modus. Alleen gebruiken met versleutelde bestanden.         |
|log+:<LogFile>| Voegt de uitvoer toe aan het bestaande logboekbestand.|    
 
In het volgende voorbeeld ziet u de uitvoer van de Robocopy-opdracht voor het kopiëren van bestanden naar de Data Box.
    
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
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:32
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
            Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
        Options : *.* /DCOPY:DA /COPY:DAT /MT:32 /R:5 /W:60
    
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
       

Gebruik de volgende parameters in Robocopy om de prestaties te optimaliseren als u de gegevens kopieert.

|    Platform    |    Voornamelijk kleine bestanden van < 512 KB                           |    Voornamelijk middelgrote bestanden van 512 KB - 1 MB                      |    Voornamelijk grote bestanden van > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy-sessies <br> 16 threads per sessie    |    3 Robocopy-sessies <br> 16 threads per sessie    |    2 Robocopy-sessies <br> 24 threads per sessie    |  |


Ga voor meer informatie over opdrachten voor Robocopy naar [Robocopy en een paar voorbeelden](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

Open de doelmap om de gekopieerde bestanden weer te geven en te controleren. Download de foutbestanden om problemen op te lossen als er fouten zijn opgetreden tijdens het kopiëren.

Om de gegevensintegriteit te garanderen wordt de controlesom inline berekend terwijl de gegevens worden gekopieerd. Verifieer de gebruikte ruimte en vrije ruimte op uw apparaat na het kopiëren.
    
   ![Vrije en ongebruikte ruimte verifiëren op het dashboard](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)

### <a name="copy-data-via-nfs"></a>Gegevens kopiëren via NFS

Gebruik een hulpprogramma voor kopiëren dat op Robocopy lijkt als u een Linux-hostcomputer gebruikt. Voorbeelden van beschikbare alternatieven in Linux zijn [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) en [Ultracopier](https://ultracopier.first-world.info/).  

De opdracht `cp` is een van de beste opties om een map te kopiëren. Ga voor meer informatie over het gebruik naar [cp man-pagina’s](http://man7.org/linux/man-pages/man1/cp.1.html).

Volg deze richtlijnen als u rsync gebruikt voor een kopie met meerdere threads:

 - Installeer het **CIFS Utils**- of **NFS Utils**-pakket, afhankelijk van het bestandssysteem dat uw Linux-client gebruikt.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

 -  Installeer **Rsync** en **Parallel** (varieert en is afhankelijk van de gedistribueerde Linux-versie).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

 - Maak een koppelpunt.

    `sudo mkdir /mnt/databox`

 - Koppel het volume.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

 - Spiegel de mapstructuur.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

 - Kopieer bestanden. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     waarbij j het aantal voor parallelle uitvoering opgeeft en X staat voor het aantal parallelle kopieën

     We adviseren u om met 16 parallelle kopieën te beginnen en het aantal threads te verhogen op basis van de beschikbare resources.

## <a name="prepare-to-ship"></a>Voorbereiding voor verzending

De laatste stap is de voorbereiding van het apparaat voor verzending. In deze stap worden alle shares van apparaten offline geplaatst. De shares zijn niet toegankelijk wanneer u het apparaat voorbereidt voor verzending.
1. Ga naar **Voorbereiden voor verzending** en klik op **Voorbereiden starten**. 
   
    ![Voorbereiding voor verzending 1](media/data-box-deploy-copy-data/prepare-to-ship1.png)

2. Standaard worden controlesommen inline berekend tijdens de voorbereiding op verzending. Het berekenen van de controlesom kan even duren, afhankelijk van de grootte van uw gegevens. Klik op **Voorbereiding starten**.
    1. De shares van het apparaat gaan offline en het apparaat wordt vergrendeld als we de voorbereiden voor verzending uitvoeren.
        
        ![Voorbereiding voor verzending 1](media/data-box-deploy-copy-data/prepare-to-ship2.png) 
   
    2. De status van het apparaat wordt bijgewerkt naar *Gereed voor verzending* nadat de voorbereiding van het apparaat is voltooid. 
        
        ![Voorbereiding voor verzending 1](media/data-box-deploy-copy-data/prepare-to-ship3.png)

    3. Download de lijst met bestanden (het manifest) die in dit proces werden gekopieerd. U kunt deze lijst later gebruiken om de bestanden te verifiëren die naar Azure zijn geüpload.
        
        ![Voorbereiding voor verzending 1](media/data-box-deploy-copy-data/prepare-to-ship4.png)

3. Schakel het apparaat uit. Ga naar de pagina **Uitschakelen of opnieuw opstarten** en klik op **Afsluiten**. Klik op **OK** om verder te gaan als u om bevestiging wordt gevraagd.
4. Verwijder de kabels. U moet het apparaat vervolgens naar Microsoft verzenden.

 
<!--## Appendix - robocopy parameters

This section describes the robocopy parameters used when copying the data to optimize the performance.

|    Platform    |    Mostly small files < 512 KB                           |    Mostly medium  files 512 KB-1 MB                      |    Mostly large files > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy sessions <br> 16 threads per sessions    |    3 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 24 threads per sessions    |  |
|    Data Box Heavy     |    6 Robocopy sessions <br> 24 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |   
|    Data Box Disk         |    4 Robocopy sessions <br> 16 threads per sessions             |    2 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 16 threads per sessions    |   
-->

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Verbinding maken met Data Box
> * Gegevens kopiëren naar Data Box
> * Data Box voorbereiden voor verzending

Ga naar de volgende zelfstudie om te leren hoe u gegevens kunt instellen en naar uw Data Box kunt kopiëren.

> [!div class="nextstepaction"]
> [Uw Azure Data Box verzenden naar Microsoft](./data-box-deploy-picked-up.md)

