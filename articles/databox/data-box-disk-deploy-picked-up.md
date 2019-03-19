---
title: Microsoft Azure Data Box Disk terugsturen| Microsoft Docs
description: In deze zelfstudie leest u hoe u uw Azure Data Box Disk terugstuurt naar Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 0dd0474ad1ad360fd82cfdf746d2e9837f74833a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58108372"
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
2. Verpak alle schijven en de verbindingskabels in bubbelplastic en plaats deze in de verzenddoos.
3. Gebruik het retourlabel in de doorzichtige plastic hoes op de doos. Als het label is beschadigd of ontbreekt, dient u een nieuw verzendlabel te downloaden via Azure Portal en deze op het apparaat te bevestigen. Ga naar **Overzicht > Verzendlabel downloaden**.

    ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

    Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

    ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)

4. Verzegel de verpakking en zorg ervoor dat het retourlabel zichtbaar is.
5. Plan een ophaalmoment via UPS, als het apparaat in de Verenigde Staten worden geretourneerd. Als u het apparaat in Europa met DHL retourneert, dient u een ophaalverzoek in door op de website van DHL het verzendnummer op te geven. Ga naar de DHL Express-website voor uw land/regio en kies onder **Snel naar > Boek een retour**.

    ![DHL retourverzending](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
    Geef het nummer van de luchtvrachtbrief op en klik op **Boek een koerier** om een ophaalmoment te plannen.

      ![Ophalen plannen](media/data-box-disk-deploy-picked-up/dhl-ship-2.png)

7. Nadat de schijven door de vervoerder zijn opgehaald, verandert de orderstatus in de portal in **Opgehaald**. Er wordt ook een tracerings-id weergegeven.

    ![Schijven opgehaald](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

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


