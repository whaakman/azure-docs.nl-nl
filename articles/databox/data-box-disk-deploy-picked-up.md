---
title: Microsoft Azure Data Box Disk terugsturen| Microsoft Docs
description: In deze zelfstudie leest u hoe u uw Azure Data Box Disk terugstuurt naar Microsoft
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
ms.date: 07/10/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 783c837bbb9ce22e7354252523e6725753776836
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011055"
---
# <a name="tutorial-return-azure-data-box-disk-and-verify-data-upload-to-azure"></a>Zelfstudie: Azure Data Box Disk terugsturen en de gegevensupload naar Azure controleren

Dit is de laatste zelfstudie van de serie 'Een Azure Data Box Disk implementeren'. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Data Box Disk naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen van de Data Box Disk

## <a name="prerequisites"></a>Vereisten

Zorg er voordat u begint voor dat u de [Zelfstudie: gegevens kopiëren naar de Azure Data Box Disk en deze gegevens controleren](data-box-disk-deploy-copy-data.md) hebt voltooid.

## <a name="ship-data-box-disk-back"></a>Een Data Box Disk terugsturen

1. Koppel de schijven los, nadat de gegevensvalidatie is voltooid. Verwijder de verbindingskabels.
2. Verpak alle schijven en de verbindingskabels in bubbelplastic en plaats deze in de verzenddoos.
3. Gebruik het retourlabel in de doorzichtige plastic hoes op de doos. Als het label is beschadigd of ontbreekt, dient u een nieuw verzendlabel te downloaden via Azure Portal en deze op het apparaat te bevestigen. Ga naar **Overzicht > Verzendlabel downloaden**. 

    ![Verzendlabel downloaden](media/data-box-disk-deploy-picked-up/download-shipping-label.png)

    Hiermee downloadt u een retourzendingslabel, zoals hieronder wordt weergegeven.

    ![Voorbeeld van verzendlabel](media/data-box-disk-deploy-picked-up/exmple-shipping-label.png)

4. Verzegel de verpakking en zorg ervoor dat het retourlabel zichtbaar is.
5. Plan een ophaalmoment via UPS, als het apparaat in de Verenigde Staten worden geretourneerd. Als u het apparaat in Europa met DHL retourneert, dient u een ophaalverzoek in door op de website van DHL het verzendnummer op te geven. Ga naar de DHL Express-website voor uw land/regio en kies onder **Snel naar > Boek een retour**.

    ![Boek een retour](media/data-box-disk-deploy-picked-up/dhl-ship-1.png)
    
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

Controleer of uw gegevens zich in de opslagaccount(s) bevinden voordat u deze uit de bron verwijdert. Om te controleren of de gegevens naar Azure zijn geüpload, dient u de volgende stappen uit te voeren:

1. Ga naar het opslagaccount dat is gekoppeld aan uw schijforder.
2. Ga naar **Blob-service > Blobs verkennen**. De lijst met containers wordt weergegeven. Er worden containers gemaakt in uw opslagaccount met dezelfde naam als die van de submappen die u hebt gemaakt onder de mappen *BlockBlob* en *PageBlob*.
    Als de mapnamen niet de naamgevingsregels van Azure volgen, mislukt het uploaden van gegevens naar Azure.

4. Als u wilt controleren of de volledige gegevensset is geladen, kunt u Microsoft Azure Storage Explorer gebruiken. Koppel het opslagaccount dat overeenkomt met de schijfhuurorder en bekijk de lijst met blobcontainers. Selecteer een container, klikt u op **... Meer** en klik vervolgens op **Mapstatistieken**. De statistieken voor die map worden dan weergegeven in het deelvenster **Activiteiten**, waaronder het aantal blobs en de totale grootte van de blobs. De totale blobgrootte in bytes moet overeenkomen met de grootte van de gegevensset.

    ![Mapstatistieken in Storage Explorer](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Gegevens verwijderen van de Data Box Disk

Nadat de kopie is voltooid en u de gegevens in het Azure-opslagaccount hebt gecontroleerd, worden de schijven veilig gewist volgens de NIST-standaard. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box Disk, zoals:

> [!div class="checklist"]
> * Een Data Box Disk naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen van de Data Box Disk


Ga verder met de volgende handleiding voor meer informatie over het beheren van Data Box Disk via Azure Portal.

> [!div class="nextstepaction"]
> [Azure Portal gebruiken om Azure Data Box Disk te beheren](./data-box-portal-ui-admin.md)


