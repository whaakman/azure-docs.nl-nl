---
title: Zelfstudie voor het kopiëren van gegevens naar Azure Data Box-blobopslag via REST API's | Microsoft Docs
description: In deze zelfstudie leest u hoe u gegevens kopieert naar uw Azure Data Box-blobopslag via REST API's
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 2c66b94cbcfa4688d9dc45d99688abe76fa55d17
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595740"
---
# <a name="tutorial-copy-data-to-azure-data-box-blob-storage-via-rest-apis"></a>Zelfstudie: Gegevens kopiëren naar Azure Data Box-blobopslag via REST API's  

In deze zelfstudie worden procedures beschreven voor het maken van een *http*- of *https*-verbinding met Azure Data Box-blobopslag via REST API's. Eenmaal verbinding hebben, worden de stappen die nodig zijn om te kopiëren van de gegevens naar gegevens in Blob-opslag worden beschreven.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten
> * Verbinding maken met Data Box-blobopslag via *http* of *https*
> * Gegevens kopiëren naar Data Box-zwaar

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de zelfstudie [ Instellen van Azure Data Box zware](data-box-heavy-deploy-set-up.md).
2. U hebt uw Data Box-zwaar ontvangen en de status van de volgorde in de portal **geleverd**.
3. U hebt de [systeemvereisten voor Data Box-blobopslag](data-box-system-requirements-rest.md) bekeken en bent bekend met de ondersteunde versies van API's, SDK's en hulpprogramma's.
4. U hebt toegang tot een hostcomputer waarmee de gegevens die u kopiëren wilt via Data Box zware heeft. Op uw hostcomputer moet
    - Een [ondersteund besturingssysteem](data-box-system-requirements.md) worden uitgevoerd.
    - Verbonden zijn met een netwerk met hoge snelheid. Voor de snelste kopie snelheden worden bereikt, kunnen twee 40-GbE-verbindingen (één per knooppunt) gelijktijdig worden gebruikt. Als u geen 40-GbE-verbinding beschikbaar hebt, wordt u aangeraden dat u ten minste twee 10 GbE-verbindingen (één per knooppunt hebt). 
5. [Download AzCopy 7.1.0](https://aka.ms/azcopyforazurestack20170417) op de hostcomputer. U gebruikt AzCopy om gegevens te kopiëren van uw hostcomputer naar Azure Data Box-blobopslag.


## <a name="connect-via-http-or-https"></a>Verbinding maken via http of https

U kunt verbinding maken met Data Box-blobopslag via *http* of *https*.

- *HTTPS* is de beveiligde en aanbevolen manier om verbinding maken met gegevens in Blob-opslag.
- *Http* wordt gebruikt voor verbindingen via vertrouwde netwerken.

De stappen om verbinding te verschillen wanneer u verbinding met gegevens in Blob-opslag via maken *http* of *https*.

## <a name="connect-via-http"></a>Verbinding maken via http

Voor een verbinding met Data Box-blobopslag via REST API's via *http* zijn de volgende stappen vereist:

- De apparaat-IP en het blobservice-eindpunt aan de externe host toevoegen
- Software van derden configureren en de verbinding controleren

Deze stappen worden afzonderlijk beschreven in de volgende gedeelten.

> [!IMPORTANT]
> Voor gegevens in het intensief moet u alle instructies verbinding om verbinding met het tweede knooppunt te herhalen.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>IP-apparaatadres toevoegen en blob-service-eindpunt

[!INCLUDE [data-box-add-device-ip](../../includes/data-box-add-device-ip.md)]



### <a name="configure-partner-software-and-verify-connection"></a>Partnersoftware configureren en verbinding controleren

[!INCLUDE [data-box-configure-partner-software](../../includes/data-box-configure-partner-software.md)]

[!INCLUDE [data-box-verify-connection](../../includes/data-box-verify-connection.md)]

## <a name="connect-via-https"></a>Verbinding maken via https

Voor een verbinding met Azure-blobopslag via REST API's via https zijn de volgende stappen vereist:

- Het certificaat downloaden van Azure Portal
- Importeer het certificaat op de client of de externe host
- De apparaat-IP toevoegen en blob-service-eindpunt voor de client of de externe host
- Software van derden configureren en de verbinding controleren

Deze stappen worden afzonderlijk beschreven in de volgende gedeelten.

> [!IMPORTANT]
> Voor gegevens in het intensief moet u alle instructies verbinding om verbinding met het tweede knooppunt te herhalen.

### <a name="download-certificate"></a>Certificaat downloaden

Gebruik Azure Portal om het certificaat te downloaden.

1. Meld u aan bij Azure Portal.
2. Ga naar uw Data Box-bestelling en ga vervolgens naar **Algemeen > Apparaatdetails**.
3. Ga onder **Apparaatreferenties** naar **API-toegang** tot het apparaat. Klik op **Downloaden**. Deze actie downloadt een  **\<de ordernaam van uw > .cer** certificaatbestand. **Sla** dit bestand op. U installeert dit certificaat op de client- of hostcomputer waarmee u verbinding maakt met het apparaat.

    ![Certificaat downloaden in Azure Portal](media/data-box-deploy-copy-data-via-rest/download-cert-1.png)
 
### <a name="import-certificate"></a>Certificaat importeren 

Toegang tot gegevens in Blob storage via HTTPS, is een SSL-certificaat vereist voor het apparaat. De manier waarop dit certificaat beschikbaar wordt gesteld aan de clienttoepassing is afhankelijk van de toepassing voor besturingssystemen en -distributies. Sommige toepassingen hebben toegang tot het certificaat nadat deze zijn geïmporteerd in het certificaatarchief van het systeem, terwijl andere toepassingen geen maken gebruik van dit mechanisme.

Specifieke informatie voor sommige toepassingen wordt vermeld in deze sectie. Raadpleeg de documentatie voor de toepassing en het besturingssysteem dat wordt gebruikt voor meer informatie over andere toepassingen.

Volg deze stappen voor het importeren van de `.cer` -bestand in het basisarchief van een client met Windows of Linux. Op een Windows-systeem, kunt u Windows PowerShell of de gebruikersinterface van Windows Server gebruiken om te importeren en het certificaat installeren op uw systeem.

#### <a name="use-windows-powershell"></a>Gebruik Windows PowerShell

1. Start een Windows PowerShell-sessie als beheerder.
2. Typ in de opdrachtprompt:

    ```
    Import-Certificate -FilePath C:\temp\localuihttps.cer -CertStoreLocation Cert:\LocalMachine\Root
    ```

#### <a name="use-windows-server-ui"></a>Gebruikersinterface van WindowsServer gebruiken

1.  Met de rechtermuisknop op de `.cer` bestand en selecteer **installeren certificaat**. Deze actie start de Wizard Certificaat importeren.
2.  Selecteer bij **Archieflocatie** de optie **Lokale computer** en klik op **Volgende**.

    ![Certificaat importeren met PowerShell](media/data-box-deploy-copy-data-via-rest/import-cert-ws-1.png)

3.  Selecteer **Alle certificaten in het onderstaande archief opslaan** en klik op **Bladeren**. Navigeer naar het basisarchief van de externe host en klik vervolgens op **Volgende**.

    ![Certificaat importeren met PowerShell](media/data-box-deploy-copy-data-via-rest/import-cert-ws-2.png)

4.  Klik op **Voltooien**. Er wordt een bericht weergegeven dat het importeren is geslaagd.

    ![Certificaat importeren met PowerShell](media/data-box-deploy-copy-data-via-rest/import-cert-ws-3.png)

#### <a name="use-a-linux-system"></a>Een Linux-systeem gebruiken

Distributie is afhankelijk van de methode om een certificaat te importeren.

> [!IMPORTANT]
> Voor gegevens in het intensief moet u alle instructies verbinding om verbinding met het tweede knooppunt te herhalen.

Verschillende, zoals Ubuntu en Debian, gebruikt u de `update-ca-certificates` opdracht.  

- Wijzig de naam van het bestand met Base64 gecodeerd certificaat in een `.crt` extensie en kopieer het naar de `/usr/local/share/ca-certificates directory`.
- Voer de opdracht `update-ca-certificates` uit.

Recente versies van RHEL, Fedora en CentOS maken gebruik van de `update-ca-trust` opdracht.

- Kopieer het certificaatbestand in de `/etc/pki/ca-trust/source/anchors` directory.
- Voer `update-ca-trust` uit.

Raadpleeg de documentatie die specifiek zijn voor uw distributie voor meer informatie.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>IP-apparaatadres toevoegen en blob-service-eindpunt 

Volg dezelfde stappen om te [IP-apparaatadres toevoegen en blob-service-eindpunt als u verbinding maakt via *http*](#add-device-ip-address-and-blob-service-endpoint).

### <a name="configure-partner-software-and-verify-connection"></a>Partnersoftware configureren en verbinding controleren

Volg de stappen voor het [configureren van de partnersoftware die u hebt gebruikt tijdens het verbinding maken via *http*](#configure-partner-software-and-verify-connection). Het enige verschil is dat u de optie *HTTP gebruiken* uitgeschakeld laat.

## <a name="copy-data-to-data-box-heavy"></a>Gegevens kopiëren naar Data Box-zwaar

Nadat u verbinding met de Data Box-blobopslag hebt gemaakt, moet u de gegevens kopiëren. Neem de volgende overwegingen door voordat u gegevens kopieert:

-  Tijdens het kopiëren van gegevens, ervoor te zorgen dat de grootte van de voldoet aan de maximale grootte wordt beschreven in de [Azure-opslag en gegevens in het zware limieten](data-box-limits.md).
- Als gegevens die door Data Box-zwaar wordt geüpload, gelijktijdig door andere toepassingen buiten de gegevens in het zware is geüpload, kan dit uploaden taak fouten en beschadigde gegevens leiden.

In deze zelfstudie wordt AzCopy gebruikt om gegevens te kopiëren naar Data Box-blobopslag. U kunt de gegevens ook kopiëren met Azure Storage Explorer (als u liever een GUI-hulpprogramma gebruikt) of met partnersoftware.

De kopieerprocedure bestaat uit de volgende stappen:

- Een container maken
- De inhoud van een map uploaden naar Data Box-blobopslag
- Gewijzigde bestanden uploaden naar Data Box-blobopslag


Deze stappen worden afzonderlijk in detail beschreven in de volgende gedeelten.

> [!IMPORTANT]
> Voor gegevens in het intensief moet u herhaalt u de kopie-instructies voor het kopiëren van gegevens naar het tweede knooppunt.

### <a name="create-a-container"></a>Een container maken

De eerste stap is het maken van een container, omdat blobs altijd naar een container moeten worden geüpload. Met containers ordent u groepen blobs net zoals u bestanden in mappen op uw computer ordent. Volg deze stappen om een blobcontainer te maken.

1. Open Storage Explorer.
2. Vouw in het linkerdeelvenster het opslagaccount uit waarin u de blobcontainer wilt maken.
3. Klik met de rechtermuisknop op **Blob-containers** en selecteer **Blobcontainer maken**.

   ![Het contextmenu Blobcontainers maken](media/data-box-deploy-copy-data-via-rest/create-blob-container-1.png)

4. Onder de map **Blobcontainers** wordt een tekstvak weergegeven. Voer een naam in voor de blobcontainer. Zie [De container maken en machtigingen instellen](../storage/blobs/storage-quickstart-blobs-dotnet.md) voor informatie over regels en beperkingen voor namen van blobcontainers.
5. Druk op **Enter** om de blobcontainer te maken of op **Esc** om te annuleren. Als de blobcontainer is gemaakt, wordt deze weergegeven in de map **Blobcontainers** voor het geselecteerde opslagaccount.

   ![Gemaakte blobcontainer](media/data-box-deploy-copy-data-via-rest/create-blob-container-2.png)

### <a name="upload-contents-of-a-folder-to-data-box-blob-storage"></a>De inhoud van een map uploaden naar Data Box-blobopslag

U kunt AzCopy gebruiken om alle bestanden in een map te uploaden naar Blob-opslag in Windows of Linux. Als u alle blobs in een map wilt uploaden, voert u de volgende AzCopy-opdracht uit:

#### <a name="linux"></a>Linux

    azcopy \
        --source /mnt/myfolder \
        --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
        --dest-key <key> \
        --recursive

#### <a name="windows"></a>Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S


Vervang `<key>` door uw accountsleutel. Ga naar uw opslagaccount om uw accountsleutel in Azure Portal op te halen. Ga naar **Instellingen > Toegangssleutels**, selecteer een sleutel en plak deze in de AzCopy-opdracht.

Als de opgegeven doelcontainer niet bestaat, wordt deze door AzCopy gemaakt en wordt het bestand erin geüpload. Werk het bronpad bij in uw gegevensmap en vervang `data-box-storage-account-name` in de doel-URL door de naam van het opslagaccount dat is gekoppeld aan uw Data Box.

Als u de inhoud van de opgegeven map recursief wilt uploaden naar Blob-opslag, geeft u de optie `--recursive` (Linux) of `/S` optie (Windows) op. Wanneer u AzCopy uitvoert met een van deze opties, worden alle submappen en de bijbehorende bestanden ook geüpload.

### <a name="upload-modified-files-to-data-box-blob-storage"></a>Gewijzigde bestanden uploaden naar Data Box-blobopslag

Gebruik AzCopy om bestanden te uploaden op basis van de datum/tijd waarop deze het laatst zijn gewijzigd. Als u dit wilt uitproberen, wijzigt u bestanden of maakt u nieuwe bestanden in uw bronmap voor testdoeleinden. Als u alleen bijgewerkte of nieuwe bestanden wilt uploaden, voegt u de parameter `--exclude-older` (Linux) of `/XO` (Windows) toe aan de AzCopy-opdracht.

Als u alleen resources wilt kopiëren die niet in het doel bestaan, geeft u zowel `--exclude-older` als `--exclude-newer` (Linux) of `/XO` als `/XN` (Windows) als parameters op in de AzCopy-opdracht. Door AzCopy worden alleen de bijgewerkte gegevens geüpload, op basis van het tijdstempel.

#### <a name="linux"></a>Linux
    azcopy \
    --source /mnt/myfolder \
    --destination https://data-box-heavy-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
    --dest-key <key> \
    --recursive \
    --exclude-older

#### <a name="windows"></a>Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-heavy-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S /XO

Als er fouten optreden tijdens het verbinding maken of kopiëren, Zie [oplossen van problemen met gegevens in Blob-opslag](data-box-troubleshoot-rest.md).

In de laatste stap gaat u het apparaat voorbereiden voor verzending.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Vereisten
> * Verbinding maken met Data Box-blobopslag via *http* of *https*
> * Gegevens kopiëren naar Data Box-zwaar


Ga naar de volgende zelfstudie om te lezen hoe u uw Data Box naar Microsoft verzendt.

> [!div class="nextstepaction"]
> [Uw Azure Data Box-zwaar naar Microsoft verzenden](./data-box-heavy-deploy-picked-up.md)
