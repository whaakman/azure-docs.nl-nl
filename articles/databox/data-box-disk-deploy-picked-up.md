---
title: Zelfstudie voor Azure Data Box-schijf terugsturen | Microsoft Docs
description: In deze zelfstudie leest u hoe u uw Azure Data Box Disk terugstuurt naar Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 023542dbc22234fc57e4ce8b662a9760be4efe04
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150750"
---
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
2. Verpak alle schijven en de verbindingskabels in bubbelplastic en plaats deze in de verzenddoos. Kan in rekening gebracht als de accessoires ontbreekt.
    - Het inpakken van de eerste verzending opnieuw.  
    - Het is raadzaam dat u schijven met een goed beveiligde bubbled wrap pack.
    - Zorg ervoor dat het geschikt snug eventuele bewegingen in het vak te verkleinen.

De volgende stappen worden bepaald door waar het retourneren van het apparaat.

### <a name="pick-up-in-us-canada"></a>In de VS, Canada ophalen

De volgende stappen uitvoeren als het apparaat in de Verenigde Staten of Canada worden geretourneerd.

1. Gebruik het retourlabel in de doorzichtige plastic hoes op de doos. Als het label is beschadigd of verloren:
    - Ga naar **Overzicht > Verzendlabel downloaden**.

        ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

        ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Brengt het label op het apparaat.

2. Verzegel de verpakking en zorg ervoor dat het retourlabel zichtbaar is.
3. Een ophalen met UPS plannen. Ophalen van objecten plannen:

    - Aanroepen van de lokale Noodvoeding (landspecifieke gratis telefoonnummer).
    - In uw aanroep citeren de omgekeerde verzending traceringsnummer zoals wordt weergegeven in het afgedrukte label.
    - Als het volgnummer niet wordt vermeld, moet UPS u een extra kosten betalen tijdens het ophalen.
    - In plaats van de planning van het ophalen, kunt u ook de Data Box-schijf op de dichtstbijzijnde afgiftelocatie teruglopen.


### <a name="pick-up-in-europe"></a>In Europa ophalen

De volgende stappen uitvoeren als het apparaat in Europa worden geretourneerd.

1. Gebruik het retourlabel in de doorzichtige plastic hoes op de doos. Als het label is beschadigd of verloren:
    - Ga naar **Overzicht > Verzendlabel downloaden**.

        ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

        Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

        ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)
    - Brengt het label op het apparaat.

2. Verzegel de verpakking en zorg ervoor dat het retourlabel zichtbaar is.
3. Als u het apparaat in Europa met DHL retourneert, dient u een ophaalverzoek in door op de website van DHL het verzendnummer op te geven.
4. Ga naar de DHL Express-website voor uw land/regio en kies onder **Snel naar > Boek een retour**.

    ![DHL retourverzending](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
3. Geef het nummer van de luchtvrachtbrief op en klik op **Boek een koerier** om een ophaalmoment te plannen.

      ![Ophalen plannen](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

### <a name="pick-up-in-asia-pacific-region"></a>In de regio Azië en Stille Oceaan ophalen

Deze regio bevat instructies voor het ophalen van objecten in Japan, Korea en Australië.

#### <a name="pick-up-in-australia"></a>Pikken in Australië

Azure-datacenters in Australië hebben een melding voor extra beveiliging. De binnenkomende verzendingen moeten een van tevoren hebben. De volgende stappen voor ophalen in Australië.

1. E-mailbericht `adbops@microsoft.com` aanvraag verzending label met de unieke ID van binnenkomend of de code TAU. Plaats de aanvraag ten minste 3 dagen vóór de geplande verzenddatum om op te halen van het label in de tijd.
2. Het e-mailonderwerp moet - *aanvraag voor omgekeerde verzendlabel met TAU code*. Zorg ervoor dat u de volgende gegevens in het e-mailbericht: 

    - Ordernaam
    - Adres
    - Naam van contactpersoon

#### <a name="pick-up-in-japan"></a>In Japan ophalen

1. Zorg ervoor dat de geretourneerde connote van Japan Post Chakubarai.
2. Schrijven van uw bedrijf naam en adres informatie over de connote als uw gegevens van de afzender.
3. Japan Post ophaalaanvragen nummer 0800-0800-111 (gratis op verzoek) aanroepen. De postcode van 7 cijfers voor ophalen-adres kiezen en vervolgens doorsturen naar de dichtstbijzijnde post office.
    - Beschikbare tijden voor de aanvraag voor het ophalen, is afhankelijk van de respectieve post kantoren.
    - Laat weten dat de verzending voor Japan Post Chakubarai Yu-Pack is.
    - Gebruik de Chakubarai Werkelijke die is opgenomen.
4. Als de werkelijke Japan Post Chakubarai is niet opgenomen, e- *Quantium oplossingen* op `Customerservice.JP@quantiumsolutions.com`. *Quantium oplossingen* vraagt Japan Post pikken en vraag deze om de connote bij ophalen.
    - Geven de verwijzing naar het nummer op de Chakubarai Werkelijke Opmerking kolom dat Japan Post ervoor zorgt dat.
    - Voer het adres zoals hieronder wordt weergegeven:   
        ```
        3F N7 Prologis Park Tokyo Ohta, 1-3-6 Tokai Ohta-ku, Tokyo 143-0001
        Microsoft Service Center c/o Quantium Solutions Japan
        TEL: 03-5755-0150
        ```

Als de Chakubarai Werkelijke ontbreekt, kunt u de ophalen via e-mail aanvragen. Gebruik de volgende e-mailsjabloon om aan te vragen worden opgehaald.

```
To: Customerservice.JP@quantiumsolutions.com
Subject: Pickup request for Azure Data Box Disk｜Job Name： 
Body: 
- Azure Data Box Disk job name：
- Reference number:  
- Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
```

#### <a name="pick-up-in-korea"></a>Pikken in Korea

1. Zorg ervoor dat de geretourneerde connote.
2. Om aan te vragen ophalen:
    1. Bel *Quantium oplossingen internationale* hotline op 070-8231-1418 tijdens kantooruren (10 naar 5: 00, maandag tot vrijdag). Citeren *Microsoft ophalen* en het aantal connote moeten worden geschikt voor een verzameling.  
    2. Als de hotline bezet is, stuurt u e-mail `microsoft@rocketparcel.com`, met het e-mailonderwerp *Microsoft Pickup* en het aantal connote als verwijzing.
    3. Als de koerier niet heeft bereikt voor de verzameling, belt u *Quantium oplossingen internationale* hotline voor alternatieve regelingen. 

## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

Nadat de schijven door de vervoerder zijn opgehaald, verandert de orderstatus in de portal in **Opgehaald**. Er wordt ook een tracerings-id weergegeven.

![Schijven opgehaald](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

Wanneer Microsoft de schijf heeft ontvangen en gescand, wordt de taakstatus bijgewerkt naar **Ontvangen**. 

![Schijven ontvangen](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

De gegevens worden automatisch gekopieerd zodra de schijven worden verbonden met een server in het Azure-datacenter. Afhankelijk van de gegevensgrootte kan de kopieerbewerking enkele uren tot enkele dagen duren. U kunt de voortgang van de kopieertaak bewaken via de portal.

Nadat de kopie is voltooid, wordt de orderstatus bijgewerkt naar **Voltooid**.

![Gegevens kopiëren voltooid](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Controleer of uw gegevens zich in de opslagaccount(s) bevinden voordat u deze uit de bron verwijdert. Uw gegevens, kunnen zich in:

- Uw Azure-Opslagaccount (s). Als u de gegevens naar Data Box kopieert, worden de gegevens naar een van de volgende paden in uw Azure Storage-account geüpload, afhankelijk van het gegevenstype.

  - Voor blok-blobs en pagina-blobs: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Voor Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    U kunt ook naar uw Azure-opslagaccount in de Azure-portal gaan en van daaruit navigeren.

- Uw beheerde schijf resourcegroepen bevinden. Bij het maken van beheerde schijven, wordt de VHD's worden geüpload als pagina-blobs, en vervolgens geconverteerd naar beheerde schijven. De beheerde schijven zijn gekoppeld aan de opgegeven op het moment van het maken van resourcegroepen.

  - Als uw kopiëren naar beheerde schijven in Azure voltooid is, gaat u naar de **detailgegevens Order** in Azure portal en maak een notitie van de resourcegroep die is opgegeven voor beheerde schijven.

      ![Details van de volgorde weergeven](media/data-box-disk-deploy-picked-up/order-details-resource-group.png)

    Ga naar de resourcegroep hebt genoteerd en zoek uw beheerde schijven.

      ![Resourcegroep voor beheerde schijven](media/data-box-disk-deploy-picked-up/resource-group-attached-managed-disk.png)

  - Als u een VHDX- of een dynamische/differentiërende VHD hebt gekopieerd, is klikt u vervolgens de VHDX/VHD geüpload naar het tijdelijke opslagaccount dat als een blok-blob. Ga naar uw fasering **Storage-account > Blobs** en selecteer vervolgens de juiste container - StandardSSD, StandardHDD of PremiumSSD. De VHDX/VHD's weergegeven als blok-blobs in uw faseringsopslagaccount.

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


