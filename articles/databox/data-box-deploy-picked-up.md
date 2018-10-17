---
title: Microsoft Azure Data Box terugsturen| Microsoft Docs
description: Lees hoe u uw Azure Data Box naar Microsoft verzendt
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
ms.date: 10/03/2018
ms.author: alkohli
ms.openlocfilehash: 05bfba9fffa7db75ef4e1a1167b3170a775e1d34
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785457"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Zelfstudie: Azure Data Box terugsturen en de gegevensupload naar Azure controleren

In deze zelfstudie wordt beschreven hoe u de Azure Data Box terugstuurt en hoe u de gegevens kunt controleren die u naar Azure hebt geüpload.

In deze zelfstudie vindt u informatie over onderwerpen als:

> [!div class="checklist"]
> * De Data Box naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen uit de Data Box

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint ervoor dat u de [Zelfstudie: Gegevens kopiëren naar de Azure Data Box en deze gegevens controleren](data-box-deploy-copy-data.md) hebt voltooid.

## <a name="ship-data-box-back"></a>De Data Box terugsturen

1. Zorg ervoor dat het apparaat uit staat en de kabels zijn verwijderd. Rol het netsnoer op en plaats het veilig aan de achterzijde van het apparaat.
2. Als het apparaat wordt verzonden in de VS, zorgt u ervoor dat het verzendlabel wordt weergegeven op het E-ink-scherm en spreekt u met de vervoerder een tijdstip af waarop het kan worden opgehaald. Als het label is beschadigd of ontbreekt of niet wordt weergegeven op het E-ink-scherm, dient u het verzendlabel te downloaden van de Azure-portal en dit op het apparaat te bevestigen. Ga naar **Overzicht > Verzendlabel downloaden**. 

    Als het apparaat wordt geleverd in Europa, wordt er geen verzendlabel weergegeven op het E-ink-scherm. In dat geval vindt u het retourlabel in het doorzichtige hoesje onder het verzendlabel aan de voorkant. Verwijder het oude verzendlabel en zorg ervoor dat het verzendlabel duidelijk zichtbaar is.
    
3. Plan een ophaalmoment via UPS, als het apparaat in de Verenigde Staten worden geretourneerd. Als u het apparaat in Europa met DHL retourneert, dient u een ophaalverzoek in door op de website van DHL het verzendnummer op te geven. Ga naar de DHL Express-website voor uw land/regio en kies onder **Snel naar > Boek een retour**. 

    Geef het nummer van de luchtvrachtbrief op en klik op **Boek een koerier** om een ophaalmoment te plannen.

4. Nadat de Data Box door de vervoerder is opgehaald en gescand, verandert de orderstatus in de portal in **Opgehaald**. Er wordt ook een tracerings-id weergegeven.

## <a name="verify-data-upload-to-azure"></a>De gegevensupload naar Azure controleren

Wanneer Microsoft het apparaat heeft ontvangen en gescand, wordt de orderstatus bijgewerkt naar **Ontvangen**. Het apparaat wordt vervolgens fysiek gecontroleerd op schade of op tekenen dat ermee is geknoeid. 

Nadat de controle is uitgevoerd, wordt de Data Box met het netwerk in het Azure-datacenter verbonden. Het kopiëren van de gegevens start automatisch. Afhankelijk van de gegevensgrootte kan de kopieerbewerking enkele uren tot enkele dagen duren. U kunt de voortgang van de kopieertaak bewaken via de portal.

Nadat de kopie is voltooid, wordt de orderstatus bijgewerkt naar **Voltooid**.

Controleer of uw gegevens zich in de opslagaccount(s) bevinden voordat u deze uit de bron verwijdert. 

## <a name="erasure-of-data-from-data-box"></a>Gegevens verwijderen uit de Data Box
 
 Nadat de gegevens naar Azure zijn geüpload, worden de gegevens door de Data Box van de schijven gewist volgens de [richtlijnen van NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi). 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * De Data Box naar Microsoft verzenden
> * De gegevensupload naar Azure controleren
> * Gegevens verwijderen uit de Data Box

Ga naar het volgende artikel voor informatie over het beheren van de Data Box via de lokale webgebruikersinterface.

> [!div class="nextstepaction"]
> [De lokale webgebruikersinterface gebruiken voor het beheren van de Azure Data Box](./data-box-local-web-ui-admin.md)


