---
title: Zelfstudie voor het kopiëren van gegevens naar Azure Data Box zware via NFS | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens naar uw Azure Data Box zware via NFS
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 4361cee3d07408c3abb5031d2ab18c15c92c5e0a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595747"
---
# <a name="tutorial-copy-data-to-azure-data-box-heavy-via-nfs"></a>Zelfstudie: Gegevens kopiëren naar Azure Data Box zware via NFS

In deze zelfstudie wordt beschreven hoe u verbinding maken met en gegevens kopiëren van de hostcomputer met behulp van de lokale web-UI voor uw Azure Data Box zware.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten
> * Verbinding maken met Data Box-zwaar
> * Gegevens kopiëren naar Data Box-zwaar

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de zelfstudie [ Instellen van Azure Data Box zware](data-box-heavy-deploy-set-up.md).
2. U hebt uw Data Box-zwaar ontvangen en de status van de volgorde in de portal **geleverd**.
3. U hebt een hostcomputer waarmee de gegevens die u kopiëren wilt via Data Box zware heeft. Op uw hostcomputer moet
    - Een [ondersteund besturingssysteem](data-box-heavy-system-requirements.md) worden uitgevoerd.
    - Verbonden zijn met een netwerk met hoge snelheid. Voor de snelste kopie snelheden worden bereikt, kunnen twee 40-GbE-verbindingen (één per knooppunt) gelijktijdig worden gebruikt. Als u geen 40-GbE-verbinding beschikbaar hebt, wordt u aangeraden dat u ten minste twee 10 GbE-verbindingen (één per knooppunt hebt). 

## <a name="connect-to-data-box-heavy"></a>Verbinding maken met Data Box-zwaar

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
| Azure-blok-blobs | <li>UNC-pad naar shares: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure-pagina-blobs  | <li>UNC-pad naar shares: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Files       |<li>UNC-pad naar shares: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

Als u van een Linux-host-computer gebruikmaakt, moet u de volgende stappen voor het configureren van uw apparaat voor toegang tot de NFS-clients uitvoeren.

1. Geef de IP-adressen op van de clients die toegang hebben tot de share. Ga in de lokale gebruikersinterface naar de pagina **Verbinding maken en kopiëren**. Klik onder **NFS-instellingen** op **NFS-clienttoegang**. 

    ![NFS-clienttoegang configureren 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. Geef het IP-adres op van de NFS-client en klik op **Toevoegen**. U kunt deze stap herhalen om toegang voor meerdere NFS-clients te configureren. Klik op **OK**.

    ![NFS-clienttoegang configureren 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Zorg dat er een [ondersteunde versie](data-box-system-requirements.md) van de NFS-client op de Linux-hostcomputer is geïnstalleerd. Gebruik de specifieke versie voor uw Linux-distributie. 

3. Gebruik na het installeren van de NFS-client de volgende opdracht om de NFS-share op uw Data Box-apparaat te koppelen:

    `sudo mount <Data Box Heavy device IP>:/<NFS share on Data Box Heavy device> <Path to the folder on local Linux computer>`

    Het volgende voorbeeld ziet hoe u verbinding maakt via NFS naar een Data Box zware share. De gegevens in het zware IP-adres `10.161.23.130`, de share `Mystoracct_Blob` is gekoppeld aan de ubuntuVM, koppelen punt wordt `/home/databoxheavyubuntuhost/databoxheavy`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`
    
    Voor Mac-clients moet u als volgt een extra optie toevoegen: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`

    **Maak altijd een map voor de bestanden die u van plan bent te kopiëren in de bestandsshare en kopieer de bestanden vervolgens naar die map**. De map gemaakt onder shares met blok-blobs en pagina-blobs vertegenwoordigt een container waarnaar gegevens als blobs worden geüpload. Het is niet mogelijk om bestanden rechtstreeks te kopiëren naar de *root*-map in het opslagaccount.

## <a name="copy-data-to-data-box-heavy"></a>Gegevens kopiëren naar Data Box-zwaar

Zodra u met de gegevens in het zware shares verbonden bent, wordt de volgende stap is om gegevens te kopiëren. Neem de volgende punten door voordat u gegevens gaat kopiëren:

- Zorg dat u de gegevens kopieert naar shares die overeenkomen met de juiste gegevensindeling. U moet bijvoorbeeld de blok-blobgegevens naar de share voor blok-blobs kopiëren. Kopieer VHD's naar pagina-blobs. Als de gegevensindeling niet met het betreffende sharetype overeenkomt, zal het uploaden van gegevens naar Azure op een later tijdstip mislukken.
-  Tijdens het kopiëren van gegevens, ervoor te zorgen dat de grootte van de voldoet aan de maximale grootte wordt beschreven in de [Azure-opslag en gegevens in het zware limieten](data-box-heavy-limits.md). 
- Als gegevens die door Data Box-zwaar wordt geüpload, gelijktijdig door andere toepassingen buiten de gegevens in het zware is geüpload, kan dit uploaden taak fouten en beschadigde gegevens leiden.
- We adviseren u om niet SMB en NFS tegelijk te gebruiken of om dezelfde gegevens naar dezelfde eindbestemming in Azure te kopiëren. In dergelijke gevallen kan de definitieve uitkomst niet worden vastgesteld.
- **Maak altijd een map voor de bestanden die u van plan bent te kopiëren in de bestandsshare en kopieer de bestanden vervolgens naar die map**. De map gemaakt onder shares met blok-blobs en pagina-blobs vertegenwoordigt een container waarnaar gegevens als blobs worden geüpload. Het is niet mogelijk om bestanden rechtstreeks te kopiëren naar de *root*-map in het opslagaccount.
- Als hoofdlettergevoelig map- en namen van een NFS-share aan NFS op gegevens in het zware opnemen: 
    - De aanvraag blijft behouden in de naam.
    - De bestanden zijn niet hoofdlettergevoelig.
    
    Bijvoorbeeld, als kopiëren `SampleFile.txt` en `Samplefile.Txt`, de aanvraag worden bewaard in de naam wanneer gekopieerd naar het apparaat, maar de tweede bestand, het eerste item wordt overschreven als deze worden beschouwd als hetzelfde bestand.


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

    `sudo mkdir /mnt/databoxheavy`

 - Koppel het volume.

    `sudo mount -t NFS4  //Databox-heavy-IP-Address/share_name /mnt/databoxheavy` 

 - Spiegel de mapstructuur.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databoxheavy`

 - Kopieer bestanden. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databoxheavy/{}`

     waarbij j het aantal voor parallelle uitvoering opgeeft en X staat voor het aantal parallelle kopieën

     We adviseren u om met 16 parallelle kopieën te beginnen en het aantal threads te verhogen op basis van de beschikbare resources.

> [!IMPORTANT]
> De volgende typen van de Linux-bestanden worden niet ondersteund: symbolische koppelingen, tekenbestanden, bestanden blokkeren, sockets en pipes. Deze bestandstypen zal leiden tot fouten tijdens de **voorbereiding voor verzending** stap.

Open de doelmap om de gekopieerde bestanden weer te geven en te controleren. Download de foutbestanden om problemen op te lossen als er fouten zijn opgetreden tijdens het kopiëren. Zie voor meer informatie, [foutenlogboeken weergeven tijdens het kopiëren van gegevens naar Data Box zware](data-box-logs.md#view-error-log-during-data-copy). Zie voor een gedetailleerde lijst met fouten tijdens het kopiëren van gegevens, [Data Box zware oplossen van problemen met](data-box-troubleshoot.md).

Om de gegevensintegriteit te garanderen wordt de controlesom inline berekend terwijl de gegevens worden gekopieerd. Verifieer de gebruikte ruimte en vrije ruimte op uw apparaat na het kopiëren.
    
   ![Vrije en ongebruikte ruimte verifiëren op het dashboard](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over Azure Data Box zware onderwerpen, zoals:

> [!div class="checklist"]
> * Vereisten
> * Verbinding maken met Data Box-zwaar
> * Gegevens kopiëren naar Data Box-zwaar


Ga naar de volgende zelfstudie om te lezen hoe u uw Data Box naar Microsoft verzendt.

> [!div class="nextstepaction"]
> [Uw Azure Data Box-zwaar naar Microsoft verzenden](./data-box-heavy-deploy-picked-up.md)

