---
title: Quickstart voor Microsoft Azure Data Box Disk | Microsoft Docs
description: Gebruik deze quickstart om uw Azure Data Box Disk snel te implementeren in de Azure-portal.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: 596c4b15ea6ef76d4471bca6994377bf4d5ddc01
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143420"
---
# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal-preview"></a>Quickstart: De Azure Data Box Disk implementeren met de Azure-portal (preview)

In deze quickstart wordt beschreven hoe u de Azure Data Box Disk implementeert met de Azure-portal. De stappen omvatten hoe u snel een order maakt, schijven ontvangt, uitpakt en verbindt, en gegevens naar schijven kopieert zodat ze naar Azure worden geüpload. 

Ga naar [Zelfstudie: Azure Data Box Disk bestellen](data-box-disk-deploy-ordered.md) voor gedetailleerde stapsgewijze implementatie- en volginstructies. 

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) aan.

> [!IMPORTANT]
> Data Box Disk is in de preview-fase. Lees de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.

## <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat uw abonnement is ingeschakeld voor de Azure Data Box-service. [Registreer u voor de service](http://aka.ms/azuredataboxfromdiskdocs) om uw abonnement in te schakelen voor deze service.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [http://aka.ms/azuredataboxfromdiskdocs](http://aka.ms/azuredataboxfromdiskdocs).

## <a name="order"></a>Bestellen

Deze stap neemt ongeveer 5 minuten in beslag.

1. Maak een nieuwe Azure Data Box-resource in de Azure-portal. 
2. Selecteer een abonnement dat is ingeschakeld voor deze service, en kies **Importeren** als overdrachtstype. Geef het **Bronland** op waar de gegevens zijn opgeslagen, en geef de **Azure-doelregio** voor de gegevensoverdracht op.
3. Selecteer **Data Box Disk**. De maximale oplossingscapaciteit is 35 TB, en voor grotere hoeveelheden gegevens kunt u meerdere schijforders maken.  
4. Voer de order- en verzendgegevens in. Als de service beschikbaar is in uw regio, geeft u e-mailadressen voor meldingen op, controleert u de samenvatting en maakt u vervolgens de order. 

Zodra de order is gemaakt, worden de schijven voorbereid voor verzending. 

## <a name="unpack"></a>Uitpakken

Deze stap neemt ongeveer 5 minuten in beslag.

De Data Box Disks worden verzonden in een UPS Express Box. Open de doos en controleer of deze het volgende bevat:

- Eén tot vijf USB-schijven in bubbeltjesplastic.
- Eén verbindingskabel per schijf. 
- Een verzendlabel voor retourzending.

## <a name="connect-and-unlock"></a>Verbinden en ontgrendelen

Deze stap neemt ongeveer 5 minuten in beslag.

1. Gebruik de meegeleverde kabel om de schijf aan te sluiten op een Windows-computer met een ondersteunde versie. Ga naar [Systeemvereisten voor Azure Data Box Disk](data-box-disk-system-requirements.md) voor meer informatie over ondersteunde besturingssysteemversies. 
2. Ontgrendel de schijf als volgt:

    1. Ga in de Azure-portal naar **Algemeen > Apparaatdetails** en haal de wachtwoordsleutel op.
    2. Download en extraheer het Data Box Disk-ontgrendelingsprogramma op de computer die wordt gebruikt om de gegevens naar schijven te kopiëren. 
    3. Voer *DataBoxDiskUnlock.exe* uit en geef de wachtwoordsleutel op. Voor elke schijf die opnieuw wordt geplaatst, voert u het ontgrendelingshulpprogramma opnieuw uit en geeft de wachtwoordsleutel op. **Gebruik niet het BitLocker-dialoogvenster of de BitLocker-sleutel voor het ontgrendelen van de schijf.** 
    4. De stationsletter die is toegewezen aan de schijf wordt weergegeven in het hulpprogramma. Noteer de letter van het schijfstation. Deze wordt in de volgende stappen gebruikt.

## <a name="copy-data-and-verify"></a>Gegevens kopiëren en verifiëren

De duur van deze bewerking hangt af van de hoeveelheid gegevens. 

1. Het station bevat de mappen *PageBlob*, *BlockBlob* en *AzureImportExport*. De gegevens die als blok-blobs moeten worden geïmporteerd in de *BlockBlob*-map, kunt u slepen en neerzetten om ze te kopiëren. Kopieer op dezelfde manier VHD-/VHDX-gegevens naar de *PageBlob*-map.

    Er wordt voor elke submap onder de *BlockBlob*- en *PageBlob*-map een container gemaakt in het Azure-opslagaccount. Alle bestanden onder de mappen *BlockBlob* en *PageBlob* worden naar een standaardcontainer `$root` onder het Azure-opslagaccount gekopieerd.

    > [!NOTE] 
    > - Alle containers en blobs moeten voldoen aan de [Azure-naamgevingsregels](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions). Als u zich niet aan deze regels houdt, mislukt de gegevensupload naar Azure.
    > - Zorg ervoor dat de bestanden de ~4,7 TiB voor blok-blobs en ~8 TiB voor pagina-blobs niet overschrijden.

2. (Optioneel) Zodra het kopiëren is voltooid, is het raadzaam de `AzureExpressDiskService.cmd` in de map *AzureImportExport* uit te voeren om controlesommen te genereren voor validatie. Afhankelijk van de gegevensgrootte kan deze stap enige tijd in beslag nemen. 
3. Ontkoppel de schijf. 


## <a name="ship-to-azure"></a>Verzenden naar Azure

Deze stap neemt ongeveer 5 tot 7 minuten in beslag.

1. Plaats alle schijven samen in de oorspronkelijke verpakking. Gebruik het meegeleverde verzendlabel. Als het label beschadigd of kwijt is, kunt u het downloaden via de portal. Ga naar **Overzicht** en klik in de opdrachtbalk op **Verzendlabel downloaden**.
2. Lever het verzegelde pakket in bij een vestiging van de verzender.  

De Data Box Disk-service verzendt een melding via e-mail en werkt de orderstatus bij in de Azure-portal.


## <a name="verify-your-data"></a>Uw gegevens verifiëren

De duur van deze bewerking hangt af van de hoeveelheid gegevens.

1. Wanneer de Data Box Disk met het datacenternetwerk van Azure is verbonden, wordt de gegevensupload naar Azure automatisch gestart. 
2. De Azure Data Box-service laat u weten dat het kopiëren van de gegevens is voltooid via de Azure-portal. 
    
    1. Controleer foutenlogboeken op eventuele fouten en onderneem toepasselijke acties.
    2. Controleer of uw gegevens zich in de opslagaccount(s) bevinden voordat u deze uit de bron verwijdert.

## <a name="clean-up-resources"></a>Resources opschonen

Deze stap neemt 2 tot 3 minuten in beslag.

Als u wilt opschonen, kunt u de Data Box-order annuleren en vervolgens verwijderen.

- U kunt de Data Box-order in de Azure-portal annuleren voordat de order wordt verwerkt. Zodra de order is verwerkt, kan deze niet meer worden geannuleerd. De order doorloopt verwerkingsfasen totdat deze is voltooid. 

    Als u de order wilt annuleren, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Annuleren**.  

- U kunt de order verwijderen zodra de status als **Voltooid** of **Geannuleerd** wordt weergegeven in de Azure-portal. 

    Als u de order wilt verwijderen, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u Azure Data Box Disk geïmplementeerd om u te helpen uw gegevens in Azure te importeren. Ga verder naar de volgende zelfstudie voor meer informatie over Azure Data Box Disk-beheer: 

> [!div class="nextstepaction"]
> [De Azure-portal gebruiken om Data Box Disk te beheren](data-box-portal-ui-admin.md)


