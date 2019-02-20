---
title: Microsoft Azure Data Box terugsturen| Microsoft Docs
description: Lees hoe u uw Azure Data Box naar Microsoft verzendt
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: alkohli
ms.openlocfilehash: 5a72bfccd69a51aa6bafb34bb768b48640e554af
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243005"
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

1. Zorg ervoor dat het apparaat uit staat en de kabels zijn verwijderd. Rol het netsnoer op en plaats het veilig aan de achterzijde van het apparaat.
2. Als het apparaat wordt verzonden in de VS, zorgt u ervoor dat het verzendlabel wordt weergegeven op het E-ink-scherm en spreekt u met de vervoerder een tijdstip af waarop het kan worden opgehaald. Neem contact op met de Microsoft-ondersteuning als het label beschadigd is of ontbreekt of niet wordt weergegeven op het e-inkscherm. Ga, indien het ondersteuningsteam dit aanraadt, in de Azure-portal naar **Overzicht > Verzendlabel downloaden**. Download het verzendlabel en bevestig dat op het apparaat.

    Als het apparaat wordt geleverd in Europa, wordt er geen verzendlabel weergegeven op het E-ink-scherm. In dat geval vindt u het retourlabel in het doorzichtige hoesje onder het verzendlabel aan de voorkant. Verwijder het oude verzendlabel en zorg ervoor dat het verzendlabel duidelijk zichtbaar is.
    
3. Als u het apparaat retourneert, spreekt u met UPS een tijdstip af waarop het kan worden opgehaald. Om een tijdstip te plannen belt u de lokale UPS-locatie (het gratis telefoonnummer specifiek voor uw land) of brengt u de Data Box naar de dichtstbijzijnde afgiftelocatie.

4. Nadat de Data Box door de vervoerder is opgehaald en gescand, verandert de orderstatus in de portal in **Opgehaald**. Er wordt ook een tracerings-id weergegeven.

## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

Wanneer Microsoft het apparaat heeft ontvangen en gescand, wordt de orderstatus bijgewerkt naar **Ontvangen**. Het apparaat wordt vervolgens fysiek gecontroleerd op schade of op tekenen dat ermee is geknoeid.

Nadat de controle is uitgevoerd, wordt de Data Box met het netwerk in het Azure-datacenter verbonden. Het kopiëren van de gegevens start automatisch. Afhankelijk van de gegevensgrootte kan de kopieerbewerking enkele uren tot enkele dagen duren. U kunt de voortgang van de kopieertaak bewaken via de portal.

Nadat de kopie is voltooid, wordt de orderstatus bijgewerkt naar **Voltooid**.

Controleer of uw gegevens zich in de opslagaccount(s) bevinden voordat u deze uit de bron verwijdert. Als u de gegevens naar Data Box kopieert, worden de gegevens naar een van de volgende paden in uw Azure Storage-account geüpload, afhankelijk van het gegevenstype.

- Voor blok-blobs en pagina-blobs: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
- Voor Azure Files: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

U kunt ook naar uw Azure-opslagaccount in de Azure-portal gaan en van daaruit navigeren.

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


