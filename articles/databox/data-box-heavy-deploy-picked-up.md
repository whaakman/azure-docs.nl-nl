---
title: Zelfstudie voor Azure Data Box zware terugsturen | Microsoft Docs
description: Leer hoe u uw Azure Data Box zware naar Microsoft verzenden
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 05/28/2019
ms.author: alkohli
ms.openlocfilehash: 31185900ede0fae74a04f98eaecee7379fb1c4fe
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427802"
---
# <a name="tutorial-return-azure-data-box-heavy-and-verify-data-upload-to-azure-preview"></a>Zelfstudie: Azure Data Box zware retourneren en controleer of het uploaden van gegevens naar Azure (preview)


In deze zelfstudie wordt beschreven hoe u Azure Data Box zware retourneren en controleer of de gegevens geüpload naar Azure.

In deze zelfstudie vindt u informatie over onderwerpen als:

> [!div class="checklist"]
> * Vereisten
> * Voorbereiding voor verzending
> * Data Box-zwaar naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Verwijdering van gegevens van Data Box-zwaar

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

- U hebt de zelfstudie [ Gegevens kopiëren naar Azure Data Box en deze gegevens controleren](data-box-heavy-deploy-copy-data.md).
- Kopieertaken zijn voltooid. Voorbereiding voor verzending kan niet worden uitgevoerd als er nog kopieertaken worden uitgevoerd.

## <a name="prepare-to-ship"></a>Voorbereiding voor verzending

[!INCLUDE [data-box-heavy-prepare-to-ship](../../includes/data-box-heavy-prepare-to-ship.md)]

## <a name="ship-data-box-heavy-back"></a>Verzend gegevens in het zware terug

1. Zorg ervoor dat het apparaat is uitgeschakeld en alle kabels zijn verwijderd. In de wachtrij en de 4 stroomkabels veilig plaatsen in de lade die toegankelijk is vanaf de achterkant van het apparaat.
2. Als het apparaat in de Verenigde Staten of Europa levert, is de vervoerder FedEx.

    1. Aanroepen van het lokale nummer voor de vervoerder voor het plannen van het ophalen.
    2. Contact opnemen met [vak gegevensbewerkingen](mailto:DataBoxOps@microsoft.com) om u te informeren over het ophalen en aan het retourlabel.
    3. Zorg ervoor dat het verzendlabel wordt weergegeven op het scherm wissen aan het begin van het apparaat.
    4. Zorg ervoor dat de oude verzending labels van de eerdere verzending van het apparaat worden verwijderd.
3. Zodra de gegevens in het zware opgehaald en gecontroleerd door uw provider, de status van de in de portal voor updates **opgehaald**. Er wordt ook een tracerings-id weergegeven.

## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

Wanneer Microsoft het apparaat heeft ontvangen en gescand, wordt de orderstatus bijgewerkt naar **Ontvangen**. Het apparaat wordt vervolgens fysiek gecontroleerd op schade of op tekenen dat ermee is geknoeid.

Nadat de verificatie voltooid is, is de Data Box-zwaar verbonden met het netwerk in het Azure-datacenter. Het kopiëren van de gegevens start automatisch. Afhankelijk van de gegevensgrootte kan de kopieerbewerking enkele uren tot enkele dagen duren. U kunt de voortgang van de kopieertaak bewaken via de portal.

Nadat de kopie is voltooid, wordt de orderstatus bijgewerkt naar **Voltooid**.

Controleer of uw gegevens is geüpload naar Azure voordat u het verwijderen van de bron. Uw gegevens, kunnen zich in:

- Uw Azure-Opslagaccount (s). Als u de gegevens naar Data Box kopieert, worden de gegevens naar een van de volgende paden in uw Azure Storage-account geüpload, afhankelijk van het gegevenstype.

  - Voor blok-blobs en pagina-blobs: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Voor Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    U kunt ook naar uw Azure-opslagaccount in de Azure-portal gaan en van daaruit navigeren.

- Uw beheerde schijf resourcegroepen bevinden. Bij het maken van beheerde schijven, wordt de VHD's worden geüpload als pagina-blobs, en vervolgens geconverteerd naar beheerde schijven. De beheerde schijven zijn gekoppeld aan de opgegeven op het moment van het maken van resourcegroepen. 

    - Als uw kopiëren naar beheerde schijven in Azure voltooid is, gaat u naar de **detailgegevens Order** in Azure portal en maak een notitie van de resourcegroepen die is opgegeven voor beheerde schijven.

        ![Beheerde schijf resourcegroepen identificeren](media/data-box-deploy-copy-data-from-vhds/order-details-managed-disk-resource-groups.png)

        Ga naar de resourcegroep hebt genoteerd en zoek uw beheerde schijven.

        ![Beheerde schijf die is gekoppeld aan resourcegroepen](media/data-box-deploy-copy-data-from-vhds/managed-disks-resource-group.png)

    - Als u een VHDX- of een dynamische/differentiërende VHD hebt gekopieerd, klikt u vervolgens de VHDX/VHD geüpload naar het tijdelijke opslagaccount dat als een pagina-blob, maar de conversie van VHD naar een beheerde schijf mislukt. Ga naar uw fasering **Storage-account > Blobs** en selecteer vervolgens de juiste container - Standard-SSD, harde schijf van Standard of Premium SSD. De VHD's worden geüpload als pagina-blobs in uw faseringsopslagaccount.

## <a name="erasure-of-data-from-data-box-heavy"></a>Verwijdering van gegevens van Data Box-zwaar
 
Nadat de gegevens naar Azure zijn geüpload, worden de gegevens door de Data Box van de schijven gewist volgens de [richtlijnen van NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi). Nadat de verwijdering voltooid is, kunt u [downloaden van de ordergeschiedenis](data-box-portal-admin.md#download-order-history).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Vereisten
> * Voorbereiding voor verzending
> * Data Box-zwaar naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Verwijdering van gegevens van Data Box-zwaar

Ga naar het volgende artikel voor meer informatie over het beheren van gegevens in het zware via de lokale webgebruikersinterface.

> [!div class="nextstepaction"]
> [De lokale webgebruikersinterface gebruiken voor het beheren van de Azure Data Box](./data-box-local-web-ui-admin.md)


