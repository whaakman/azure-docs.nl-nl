---
title: Zelf studie voor het verzenden van Azure Data Box Disk | Microsoft Docs
description: In deze zelfstudie leest u hoe u uw Azure Data Box Disk terugstuurt naar Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 9e3159a083d21e4a7a81df2fcf7a7d5db882b757
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976920"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Zelfstudie: Azure Data Box Disk terugsturen en de gegevensupload naar Azure controleren

Dit is de laatste zelfstudie van de serie Azure Data Box Disk implementeren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Data Box Disk naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen van de Data Box Disk

## <a name="prerequisites"></a>Vereisten

Voltooi voordat u begint de [Zelfstudie: Gegevens kopiëren naar de Azure Data Box-schijf en deze gegevens controleren](data-box-disk-deploy-copy-data.md).


## <a name="ship-data-box-disk-back"></a>Een Data Box Disk terugsturen

1. Koppel de schijven los, nadat de gegevensvalidatie is voltooid. Verwijder de verbindingskabels.
2. Verpak alle schijven en de verbindingskabels in bubbelplastic en plaats deze in de verzenddoos. Er kunnen kosten in rekening worden gebracht als de accessoires ontbreken.
    - De verpakking van de eerste verzen ding opnieuw gebruiken.  
    - U wordt aangeraden schijven te verpakken met een goed beveiligde ballon.
    - Zorg ervoor dat het passend is Snug om de bewegingen in het vak te verminderen.

De volgende stappen worden bepaald door de locatie waar u het apparaat retourneert.

### <a name="pick-up-in-us-canada"></a>Orderverzamelen in Nederland, Canada

Voer de volgende stappen uit als u het apparaat in VS of Canada retourneert.

1. Gebruik het retourlabel in de doorzichtige plastic hoes op de doos. Als het label is beschadigd of verloren is gegaan:
    - Ga naar **Overzicht > Verzendlabel downloaden**.

        ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

        ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Breng het label op het apparaat.

2. Verzegel de verpakking en zorg ervoor dat het retourlabel zichtbaar is.
3. Een ophaling plannen met UPS. Een ophaling plannen:

    - Roep de lokale nood voeding (land/regio-specifiek gratis nummer) aan.
    - Neem in uw gesprek het tracerings nummer van omgekeerde verzen ding op zoals weer gegeven in het afgedrukte label.
    - Als het tracking nummer niet wordt vermeld, moet u tijdens het ophalen een extra kosten betalen.
    - In plaats van het ophalen te plannen, kunt u ook de Data Box Disk op de dichtstbijzijnde uitval locatie afhalen.


### <a name="pick-up-in-europe"></a>Ophalen in Europa

Voer de volgende stappen uit als u het apparaat in Europa wilt retour neren.

1. Gebruik het retourlabel in de doorzichtige plastic hoes op de doos. Als het label is beschadigd of verloren is gegaan:
    - Ga naar **Overzicht > Verzendlabel downloaden**.

        ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

        ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Breng het label op het apparaat.

2. Verzegel de verpakking en zorg ervoor dat het retourlabel zichtbaar is.
3. Als u het apparaat in Europa met DHL retourneert, dient u een ophaalverzoek in door op de website van DHL het verzendnummer op te geven.
4. Ga naar de pagina land/regio DHL Express en kies **een courier-verzameling boeken > eReturn-verzen ding**.

    ![DHL retour zending](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
3. Geef het nummer van de luchtvrachtbrief op en klik op **Boek een koerier** om een ophaalmoment te plannen.

      ![Ophalen plannen](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

### <a name="pick-up-in-asia-pacific-region"></a>Ophalen in Azië-Pacific-regio

Deze regio bevat instructies voor het ophalen in Japan, Korea, Australië en Singapore.

#### <a name="pick-up-in-australia"></a>Orderverzamelen in Australië

Azure-data centers in Australië hebben een extra beveiligings melding. Alle inkomende verzen dingen moeten een geavanceerde melding hebben. Voer de volgende stappen uit voor het ophalen in Australië.

1. E-mail `adbops@microsoft.com` om het verzend label met een unieke inkomende id of de letterlijke code te vragen. Plaats de aanvraag ten minste drie dagen vóór de geplande verzend datum om het label in de tijd op te halen.
2. Het onderwerp van de e-mail moet zijn: *aanvraag voor reverse-verzend label met de letter tau-code*. Zorg ervoor dat u de volgende gegevens opneemt in het e-mail bericht: 

    - Ordernaam
    - Adres
    - Naam van contactpersoon

#### <a name="pick-up-in-japan"></a>Ophalen in Japan

1. Schrijf uw bedrijfs naam en adres gegevens in de begeleide notitie als gegevens van de afzender.
2. Quantium-oplossing voor e-mail met de volgende e-mail sjabloon.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Microsoft Azure Data Box Disk｜Job Name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
        a. 08：00-13：00 
        b. 13：00-15：00 
        c. 15：00-17：00 
        d. 17：00-19：00 
    ```
    - **Als u ophaalt in Osaka**, wijzigt u het onderwerp in het e-mail sjabloon naar `Pickup request for Microsoft Azure OSA`:.
    - Als er geen Chakubarai-zending van Japan post is opgenomen of ontbreekt, noteert u deze in dit e-mail bericht. Quantium Solutions Japan vraagt een Japanse post aan om de zending op te halen wanneer deze wordt opgehaald.
    - Als u meerdere orders hebt, moet u een e-mail verzenden om afzonderlijke op te halen.

3. Ontvang een e-mail bevestiging van Quantium-oplossingen nadat u een ophaling hebt vastgelegd. De e-mail bevestiging bevat ook informatie over het Chakubarai-begeleidings document.

Als dat nodig is, kunt u contact opnemen met de ondersteuning van de Quantium-oplossing (Japanse taal) met de volgende informatie: 

- E-mail:Customerservice.JP@quantiumsolutions.com 
- Telefoon: 03-5755-0150 

#### <a name="pick-up-in-korea"></a>Ophalen in Korea

1. Zorg ervoor dat u de retour consigning-Opmerking opneemt.
2. Ophalen aanvragen wanneer een begeleidende partij aanwezig is:
    1. Roep *Quantium Solutions International* hotline op 070-8231-1418 tijdens kantoor uren (10 tot 5 uur, maandag t/m vrijdag). Offerte *Microsoft Azure ophalen* en het aanvraag nummer van de service om een verzameling te rangschikken.  
    2. Als de Hotline bezet is, e-mail `microsoft@rocketparcel.com`, met het onderwerp van de e-mail *Microsoft Azure ophalen* en het nummer van de service aanvraag als referentie.
    3. Roep *Quantium Solutions International* Hotline aan voor alternatieve regelingen als de courier niet arriveert voor verzameling. 
    4. U ontvangt een e-mail bevestiging voor het ophaal schema.
3. Voer deze stap alleen uit als de begeleidende notitie niet aanwezig is. Ophalen aanvragen:
    1. Roep *Quantium Solutions International* hotline op 070-8231-1418 tijdens kantoor uren (10 tot 5 uur, maandag t/m vrijdag). Offerte *Microsoft Azure ophalen* en het aanvraag nummer van de service om een verzameling te rangschikken. Geef op dat u een nieuw begeleidings bericht nodig hebt om een verzameling te rangschikken. Geef Sender (klant), receiver Information (Azure Data Center) en referentie nummer (service aanvraag nummer) op. 
    2. Als de Hotline bezet is, e-mail `microsoft@rocketparcel.com`, met het onderwerp van de e-mail *Microsoft Azure ophalen* en het nummer van de service aanvraag als referentie.
    3. Roep *Quantium Solutions International* Hotline aan voor alternatieve regelingen als de courier niet arriveert voor verzameling. 
    4. U ontvangt een mondelinge bevestiging als de aanvraag via de telefoon wordt gedaan.

### <a name="pick-up-in-singapore"></a>Ophalen in Singapore

1. Druk het verzend label af en voeg het toe aan het vak. Als het label is beschadigd of verloren is gegaan:
    - Ga naar **Overzicht > Verzendlabel downloaden**.

        ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

        ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Breng het label op het apparaat. Zorg ervoor dat het label zichtbaar is.

2. Ophalen aanvragen:
    - Roep **SingPost** hotline op **6845 6485** tijdens kantoor uren (9:00 tot tot 17:00 uur, maandag t/m vrijdag).  
    - Prijs opgave *Microsoft Azure ophalen* en het nummer van de service aanvraag (tracking nummer op het retour verzendings label) om een verzameling te rangschikken. 
    - U krijgt een mondelinge bevestiging voor het ophaal schema. 
    - Als de courier niet arriveert voor verzameling, roept u **SingPost** op **6845 6485** aan voor alternatieve regelingen. 
3. Hand aan de Courier. 


## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

Nadat de schijven door de vervoerder zijn opgehaald, verandert de orderstatus in de portal in **Opgehaald**. Er wordt ook een tracerings-id weergegeven.

![Schijven opgehaald](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

Wanneer Microsoft de schijf heeft ontvangen en gescand, wordt de taakstatus bijgewerkt naar **Ontvangen**. 

![Schijven ontvangen](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

De gegevens worden automatisch gekopieerd zodra de schijven worden verbonden met een server in het Azure-datacenter. Afhankelijk van de gegevensgrootte kan de kopieerbewerking enkele uren tot enkele dagen duren. U kunt de voortgang van de kopieertaak bewaken via de portal.

Nadat de kopie is voltooid, wordt de orderstatus bijgewerkt naar **Voltooid**.

![Gegevens kopiëren voltooid](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Zie [problemen met upload fouten oplossen](data-box-disk-troubleshoot-upload.md)als de Kopieer bewerking is voltooid.

Controleer of uw gegevens zich in de opslagaccounts bevinden voordat u deze uit de bron verwijdert. Uw gegevens kunnen de volgende zijn:

- Uw Azure Storage-account (s). Als u de gegevens naar Data Box kopieert, worden de gegevens naar een van de volgende paden in uw Azure Storage-account geüpload, afhankelijk van het gegevenstype.

  - Voor blok-blobs en pagina-blobs: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Voor Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    U kunt ook naar uw Azure-opslagaccount in de Azure-portal gaan en van daaruit navigeren.

- De resource groep (en) van de beheerde schijf. Bij het maken van beheerde schijven worden de Vhd's geüpload als pagina-blobs en vervolgens geconverteerd naar Managed disks. De beheerde schijven worden gekoppeld aan de resource groepen die zijn opgegeven op het moment dat de order wordt gemaakt.

  - Als uw kopie naar beheerde schijven in Azure is geslaagd, gaat u naar de details van de **order** in het Azure Portal en noteert u de resource groep die is opgegeven voor beheerde schijven.

      ![Bestellingsgegevens bekijken](media/data-box-disk-deploy-picked-up/order-details-resource-group.png)

    Ga naar de genoteerde resource groep en zoek uw beheerde schijven.

      ![Resource groep voor beheerde schijven](media/data-box-disk-deploy-picked-up/resource-group-attached-managed-disk.png)

  - Als u een VHDX of een dynamische/differentiërende VHD hebt gekopieerd, wordt de VHDX/VHD geüpload naar het opslag account voor staging als een blok-blob. Ga naar uw staging- **opslag account > blobs** en selecteer vervolgens de juiste container-StandardSSD, StandardHDD of PremiumSSD. De VHDX/Vhd's moeten worden weer gegeven als blok-blobs in uw staging Storage-account.

Om te controleren of de gegevens naar Azure zijn geüpload, dient u de volgende stappen uit te voeren:

1. Ga naar het opslagaccount dat is gekoppeld aan uw schijforder.
2. Ga naar **Blob-service > Blobs verkennen**. De lijst met containers wordt weergegeven. Er worden containers gemaakt in uw opslagaccount met dezelfde naam als die van de submappen die u hebt gemaakt onder de mappen *BlockBlob* en *PageBlob*.
    Als de mapnamen niet de naamgevingsregels van Azure volgen, mislukt het uploaden van gegevens naar Azure.

4. Als u wilt controleren of de volledige gegevensset is geladen, kunt u Microsoft Azure Storage Explorer gebruiken. Koppel het opslagaccount dat overeenkomt met de schijfhuurorder en bekijk de lijst met blobcontainers. Selecteer een container, klikt u op **... Meer** en klik vervolgens op **Mapstatistieken**. De statistieken voor die map worden dan weergegeven in het deelvenster **Activiteiten**, waaronder het aantal blobs en de totale grootte van de blobs. De totale blobgrootte in bytes moet overeenkomen met de grootte van de gegevensset.

    ![Mapstatistieken in Storage Explorer](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Gegevens verwijderen van de Data Box Disk

Nadat de kopie is voltooid en u de gegevens in het Azure-opslagaccount hebt gecontroleerd, worden de schijven veilig gewist volgens de NIST-standaard.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie zijn verschillende onderwerpen besproken over de Azure Data Box-schijf, zoals:

> [!div class="checklist"]
> * Een Data Box Disk naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen van de Data Box Disk


Ga verder met de volgende handleiding voor meer informatie over het beheren van Data Box Disk via Azure Portal.

> [!div class="nextstepaction"]
> [Azure Portal gebruiken om Azure Data Box Disk te beheren](./data-box-portal-ui-admin.md)

::: zone-end


::: zone target="chromeless"

# <a name="return-the-disk-and-verify-upload-to-azure"></a>De schijf retour neren en het uploaden naar Azure controleren

## <a name="ship-to-azure"></a>Verzenden naar Azure

1. Wanneer de gegevens validatie is voltooid, koppelt u de schijven los. Verwijder de verbindingskabels.
2. Verpak alle schijven en de verbindingskabels in bubbelplastic en plaats deze in de verzenddoos. Er kunnen kosten in rekening worden gebracht als de accessoires ontbreken.
    - De verpakking van de eerste verzen ding opnieuw gebruiken.  
    - U wordt aangeraden schijven te verpakken met een goed beveiligde ballon.
    - Zorg ervoor dat het passend is Snug om de bewegingen in het vak te verminderen.
3. De volgende stappen worden bepaald door de locatie waar u het apparaat retourneert.
    - [Plan een ophaling met ups bij het retour neren van het apparaat in VS en Canada](data-box-disk-deploy-picked-up.md#pick-up-in-us-canada).
    - [Plan een ophaling met DHL voor Europa](data-box-disk-deploy-picked-up.md#pick-up-in-europe) door de website ervan te bezoeken en het Airway-factuur nummer op te geven.
    - [Plan een ophaling voor landen in Australië-Pacific-regio](data-box-disk-deploy-picked-up.md#pick-up-in-asia-pacific-region) , zoals Australië, Japan, Korea en Singapore.
4. Zodra de schijven door uw provider worden opgehaald, wordt de status van de bestelling in de portal-updates en een tracerings-ID weer gegeven.

## <a name="verify-upload-to-azure"></a>Upload naar Azure controleren

Nadat de gegevens zijn geüpload naar Azure, controleert u of de gegevens zich in de opslag account (s) bevindt voordat u deze verwijdert uit de bron. Uw gegevens kunnen de volgende zijn:

- Uw Azure Storage-account (s). Als u de gegevens naar Data Box kopieert, worden de gegevens naar een van de volgende paden in uw Azure Storage-account geüpload, afhankelijk van het gegevenstype.

    - **Voor blok-blobs en pagina**-blobs: https://< storage_account_name >. blob. core. Windows.<containername>net//files/a.txt

    - **Voor Azure files**: https://< storage_account_name >. file. core. Windows. net/<sharename>/files/a.txt

    U kunt ook naar uw Azure-opslagaccount in de Azure-portal gaan en van daaruit navigeren.

- De resource groep (en) van de beheerde schijf. Bij het maken van beheerde schijven worden de Vhd's geüpload als pagina-blobs en vervolgens geconverteerd naar Managed disks. De beheerde schijven worden gekoppeld aan de resource groepen die zijn opgegeven op het moment dat de order wordt gemaakt.

::: zone-end


