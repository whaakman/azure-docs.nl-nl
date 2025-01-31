---
title: Quickstart voor Microsoft Azure Data Box Disk | Microsoft Docs
description: Gebruik deze quickstart om uw Azure Data Box Disk snel te implementeren in de Azure-portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: quickstart
ms.date: 02/26/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to quickly deploy Data Box Disk so as to import data into Azure.
ms.openlocfilehash: 65bf4e973ce33b2898abf585fe306a8bc85c64a0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477789"
---
::: zone target="docs"

# <a name="quickstart-deploy-azure-data-box-disk-using-the-azure-portal"></a>Quickstart: Azure Data Box Disk implementeren met behulp van de Azure-portal

::: zone-end

::: zone target="chromeless"

# <a name="get-started-with-azure-data-box-disk-using-azure-portal"></a>Aan de slag met Azure Data Box-schijf met behulp van Azure portal

::: zone-end

::: zone target="docs"

In deze quickstart wordt beschreven hoe u de Azure Data Box Disk implementeert met de Azure-portal. De stappen omvatten hoe u snel een order maakt, schijven ontvangt, uitpakt en verbindt, en gegevens naar schijven kopieert zodat ze naar Azure worden geüpload.

Ga voor gedetailleerde stapsgewijze instructies voor implementatie en tracering naar [Zelfstudie: Azure Data Box Disk bestellen](data-box-disk-deploy-ordered.md). 

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan.

::: zone-end

::: zone target="chromeless"

In deze quickstart wordt beschreven hoe u de Azure Data Box Disk implementeert met de Azure-portal. De stappen zijn onder andere vereisten controleren, de schijven te ontgrendelen, verbinding maken met en gegevens kopiëren naar de schijven, zodat die wordt geüpload naar Azure.

::: zone-end

::: zone target="docs"

## <a name="prerequisites"></a>Vereisten

Voordat u begint:

- Zorg ervoor dat uw abonnement is ingeschakeld voor de Azure Data Box-service. [Registreer u voor de service](https://aka.ms/azuredataboxfromdiskdocs) om uw abonnement in te schakelen voor deze service.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs).

::: zone-end

::: zone target="chromeless"

## <a name="prerequisites"></a>Vereisten

- U hebt uw order voor het gebruik van Data Box-schijf geplaatst de [zelfstudie: Azure Data Box Disk bestellen](data-box-disk-deploy-ordered.md).
- U hebt uw schijven ontvangen en de taakstatus in de portal is bijgewerkt naar **Geleverd**.
- U hebt een clientcomputer beschikbaar van waaruit u de gegevens kunt kopiëren. De clientcomputer moet voldoen aan deze vereisten:

    - Voer een [ondersteund besturingssysteem](data-box-disk-system-requirements.md#supported-operating-systems-for-clients).
    - Hebben [andere vereiste software](data-box-disk-system-requirements.md#other-required-software-for-windows-clients) als het een Windows-client is geïnstalleerd.

::: zone-end

::: zone target="docs"

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

1. Gebruik de meegeleverde kabel om de schijf aan te sluiten op een Windows-/Linux-computer met een ondersteunde versie. Ga naar [Systeemvereisten voor Azure Data Box Disk](data-box-disk-system-requirements.md) voor meer informatie over ondersteunde besturingssysteemversies. 
2. Ontgrendel de schijf als volgt:

    1. Ga in de Azure-portal naar **Algemeen > Apparaatdetails** en haal de wachtwoordsleutel op.
    2. Download en extraheer het Data Box Disk-ontgrendelingsprogramma voor het specifieke besturingssysteem op de computer die wordt gebruikt om de gegevens naar schijven te kopiëren. 
    3. Voer het Data Box Disk-ontgrendelingsprogramma uit en geef de wachtwoordsleutel op. Voor elke schijf die opnieuw wordt geplaatst, voert u het ontgrendelingshulpprogramma opnieuw uit en geeft de wachtwoordsleutel op. **Gebruik niet het BitLocker-dialoogvenster of de BitLocker-sleutel voor het ontgrendelen van de schijf.** Ga naar [Schijven ontgrendelen op een Windows-client](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) of [Schijven ontgrendelen op een Linux-client](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client) voor meer informatie over het ontgrendelen van schijven.
    4. De stationsletter die is toegewezen aan de schijf wordt weergegeven in het hulpprogramma. Noteer de letter van het schijfstation. Deze wordt in de volgende stappen gebruikt.

## <a name="copy-data-and-validate"></a>Gegevens kopiëren en valideren

De duur van deze bewerking hangt af van de hoeveelheid gegevens.

1. Het station bevat *PageBlob*, *BlockBlob*, *AzureFile*, *ManagedDisk*, en *DataBoxDiskImport* mappen. De gegevens die als blok-blobs moeten worden geïmporteerd in de *BlockBlob*-map, kunt u slepen en neerzetten om ze te kopiëren. Op deze manier slepen en neerzetten van gegevens, zoals VHD-/ VHDX op *PageBlob* map en de juiste gegevens *AzureFile*. Kopieer de VHD's die u wilt uploaden als beheerde schijven naar een map onder *ManagedDisk*.

    Er wordt voor elke submap onder de *BlockBlob*- en *PageBlob*-map een container gemaakt in het Azure-opslagaccount. Een bestandsshare is gemaakt voor een submap onder *AzureFile*.

    Alle bestanden onder de mappen *BlockBlob* en *PageBlob* worden naar een standaardcontainer `$root` onder het Azure-opslagaccount gekopieerd. Kopieer bestanden naar een map in *AzureFile*. Alle bestanden gekopieerd rechtstreeks naar de *AzureFile* map mislukken en worden geüpload als blok-blobs.

    > [!NOTE]
    > - Alle containers, blobs en -bestanden moeten voldoen aan [Azure naamgevingsregels](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions). Als u zich niet aan deze regels houdt, mislukt de gegevensupload naar Azure.
    > - Zorg ervoor dat bestanden niet groter zijn dan ~4.75 TiB voor blok-blobs, ~ 8 TiB voor pagina-blobs en ~ 1 TiB voor Azure Files.

2. **(Optioneel maar aanbevolen)**  Nadat de kopie voltooid is, wordt aangeraden ten minste uit te voeren de `DataBoxDiskValidation.cmd` opgegeven in de *DataBoxDiskImport* map en selecteer optie 1 tot en met de bestanden te valideren. We raden ook aan dat u voldoende tijd hebt, u optie 2 gebruiken voor het genereren van controlesommen voor validatie ook (tijd kan duren, afhankelijk van de grootte van de gegevens). Deze stappen beperken de kans op fouten bij het uploaden van de gegevens naar Azure.
3. Veilig verwijderen van het station.

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

::: zone-end
