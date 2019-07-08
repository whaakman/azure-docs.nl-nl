---
title: Zelfstudie voor Azure Data Box terugsturen | Microsoft Docs
description: Lees hoe u uw Azure Data Box naar Microsoft verzendt
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 7/08/2019
ms.author: alkohli
ms.openlocfilehash: db0f0ac3073687b7c1cd8ca60e459e4bb3aa03f4
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626363"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Zelfstudie: Azure Data Box terugsturen en de gegevens controleren die u naar Azure hebt geüpload

In deze zelfstudie wordt beschreven hoe u de Azure Data Box terugstuurt en hoe u de gegevens kunt controleren die u naar Azure hebt geüpload.

In deze zelfstudie vindt u informatie over onderwerpen als:

> [!div class="checklist"]
> * Vereisten
> * Voorbereiding voor verzending
> * De Data Box naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen uit de Data Box

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

- U hebt de [Zelfstudie: Gegevens kopiëren naar Azure Data Box en deze gegevens controleren](data-box-deploy-copy-data.md). 
- Kopieertaken zijn voltooid. Voorbereiding voor verzending kan niet worden uitgevoerd als er nog kopieertaken worden uitgevoerd.

## <a name="prepare-to-ship"></a>Voorbereiding voor verzending

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="ship-data-box-back"></a>De Data Box terugsturen

Zorg ervoor dat het kopiëren van gegevens naar het apparaat voltooid is en **voorbereiding voor verzending** uitvoering is geslaagd. De procedure is op basis van de regio waar u het apparaat worden geleverd, anders.


### <a name="ship-in-us-canada-europe"></a>Verzenden in de VS, Canada, Europa

De volgende stappen uitvoeren als het apparaat in de VS, Canada en Europa worden geretourneerd.

1. Zorg ervoor dat het apparaat is uitgeschakeld en kabels worden verwijderd. 
2. Rol het netsnoer op en plaats het veilig aan de achterzijde van het apparaat.
3. Zorg ervoor dat het verzendlabel wordt weergegeven op het E-ink-scherm en plan met de vervoerder een tijdstip waarop het kan worden opgehaald. Neem contact op met de Microsoft-ondersteuning als het label beschadigd is of ontbreekt of niet wordt weergegeven op het e-inkscherm. Als de ondersteuning stelt en vervolgens u naar gaat **overzicht > verzendlabel downloaden** in Azure portal. Download het verzendlabel en bevestig dat op het apparaat. 
4. Als u het apparaat retourneert, spreekt u met UPS een tijdstip af waarop het kan worden opgehaald. Ophalen van objecten plannen:

    - Aanroepen van de lokale Noodvoeding (land/regio-specifieke gratis telefoonnummer).
    - Citeren de omgekeerde verzending traceringsnummer zoals wordt weergegeven in de E-ink-weergave of uw etiket in uw aanroep.
    - Als het volgnummer niet wordt vermeld, moet UPS u een extra kosten betalen tijdens het ophalen.

    In plaats van de planning van het ophalen, kunt u ook de gegevens in de dichtstbijzijnde afgiftelocatie teruglopen.
4. Nadat de Data Box door de vervoerder is opgehaald en gescand, verandert de orderstatus in de portal in **Opgehaald**. Er wordt ook een tracerings-id weergegeven.

### <a name="ship-in-asia-pacific-region"></a>Verzenden in de regio Azië en Stille Oceaan

#### <a name="ship-in-australia"></a>Verzenden in Australië

Azure-datacenters in Australië hebben een melding voor extra beveiliging. De binnenkomende verzendingen moeten een van tevoren hebben. Voer de volgende stappen uit voor verzending in Australië.


1. Behoud de oorspronkelijke vak gebruikt voor het verzenden van het apparaat voor retourverzending.
2. Zorg ervoor dat het kopiëren van gegevens naar het apparaat voltooid is en **voorbereiding voor verzending uitvoeren** is geslaagd.
3. Het apparaat uitschakelen en verwijderen van de kabels.
4. In de wachtrij en het netsnoer die is opgegeven met het apparaat aan de achterzijde van het apparaat veilig te plaatsen.
5. E-mailbericht Quantium oplossingen moeten worden opgehaald. Raadpleeg het nummer van het service-referentie opgegeven op de Azure-portal. Gebruik de volgende e-mailsjabloon:- *aanvraag voor omgekeerde verzendlabel met TAU code*. Zorg ervoor dat u de volgende gegevens in het e-mailbericht: 

    ```
    To: Azure@quantiumsolutions.com
    Subject: Pickup request for Azure｜Reference number：XXX XXX XXX
    Body: 
    - Company name：
    - Address:
    - Contact name:
    - Contact number:
    - Requested pickup date: mm/dd
    ```
6. Quantium oplossingen Australië e een retourlabel.
7. Het retourlabel afdrukken en brengt deze op de verpakking.
8. Hand over het pakket met de courier.

Indien nodig, kunt u e-Quantium oplossing ondersteuning op Azure@quantiumsolutions.com of telefoon.


Voor informatie met betrekking tot uw bestelling via de telefoon:

- Een e-mailbericht dat eerst verzenden.
- Geef de naam van de volgorde op de telefoon.

#### <a name="ship-in-japan"></a>Verzenden in Japan 

1. Behoud de oorspronkelijke vak gebruikt voor het verzenden van het apparaat voor retourverzending.
2. Het apparaat uitschakelen en verwijderen van de kabels.
3. In de wachtrij en het netsnoer die is opgegeven met het apparaat aan de achterzijde van het apparaat veilig te plaatsen.
4. Schrijven van uw bedrijf naam en adres informatie over het begeleidende als uw gegevens van de afzender.
5. E-Quantium-oplossing met behulp van de volgende e-mailsjabloon.

    - Als begeleidende Japan Post Chakubarai niet opgenomen is of ontbreekt, houd er rekening mee dat in dit e-mailbericht. Quantium oplossingen Japan vraagt Japan Post om het begeleidende bij ophalen.
    - Als u meerdere orders, e-mailadres om te controleren of afzonderlijke ophalen.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

3. Een e-mailbevestiging ontvangen van Quantium oplossingen, nadat u traceren die opgehaald zijn hebt. De e-mailbevestiging bevat ook informatie over het begeleidende Chakubarai.

Indien nodig, kunt u contact op met ondersteuning van de oplossing Quantium (Japans) op de volgende informatie: 

- E-mail:Customerservice.JP@quantiumsolutions.com 
- Telefoonnummer: 03-5755-0150 


## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

Wanneer Microsoft het apparaat heeft ontvangen en gescand, wordt de orderstatus bijgewerkt naar **Ontvangen**. Het apparaat wordt vervolgens fysiek gecontroleerd op schade of op tekenen dat ermee is geknoeid.

Nadat de controle is uitgevoerd, wordt de Data Box met het netwerk in het Azure-datacenter verbonden. Het kopiëren van de gegevens start automatisch. Afhankelijk van de gegevensgrootte kan de kopieerbewerking enkele uren tot enkele dagen duren. U kunt de voortgang van de kopieertaak bewaken via de portal.

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

## <a name="erasure-of-data-from-data-box"></a>Gegevens verwijderen uit de Data Box
 
Nadat de gegevens naar Azure zijn geüpload, worden de gegevens door de Data Box van de schijven gewist volgens de [richtlijnen van NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Vereisten
> * Voorbereiding voor verzending
> * De Data Box naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen uit de Data Box

Ga naar het volgende artikel voor informatie over het beheren van de Data Box via de lokale webgebruikersinterface.

> [!div class="nextstepaction"]
> [De lokale webgebruikersinterface gebruiken voor het beheren van de Azure Data Box](./data-box-local-web-ui-admin.md)


