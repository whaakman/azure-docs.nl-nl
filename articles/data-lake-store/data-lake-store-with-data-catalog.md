---
title: Registreert gegevens van Data Lake Store in Azure Data Catalog | Microsoft Docs
description: Registreert gegevens van Data Lake Store in Azure Data Catalog
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 93de6a574b306e3fd8959454709e84a57ee4fc10
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registreert gegevens van Data Lake Store in Azure Data Catalog
In dit artikel leert u hoe u Azure Data Lake Store integreert met Azure Data Catalog zodat uw gegevens binnen een organisatie kan worden gedetecteerd door integratie met Data Catalog. Zie voor meer informatie over de gegevens worden gecatalogiseerd, [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). Zie voor informatie over scenario's voor gebruik van Data Catalog, [algemene scenario's voor een Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Uw Azure-abonnement inschakelen** voor openbare Preview van Data Lake Store. Zie [Instructies](data-lake-store-get-started-portal.md).
* **Azure Data Lake Store-account**. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md). Voor deze zelfstudie maken we een Data Lake Store-account genoemd **datacatalogstore**.

    Nadat u het account hebt gemaakt, een verzameling voorbeeldgegevens te uploaden. Voor deze zelfstudie laat het ons uploaden de CSV-bestanden onder de **AmbulanceData** map in de [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). U kunt verschillende clients, zoals [Azure Opslagverkenner](http://storageexplorer.com/), gegevens te uploaden naar een blob-container.
* **Azure Data Catalog**. Uw organisatie moet een Azure Data Catalog gemaakt voor uw organisatie al hebben. Slechts één catalogus is toegestaan voor elke organisatie.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Registratie Data Lake Store als bron voor Data Catalog

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Ga naar `https://azure.microsoft.com/services/data-catalog`, en klik op **aan de slag**.
2. Meld u aan bij de Azure Data Catalog-portal en klikt u op **gegevens publiceren**.

    ![Een gegevensbron registreert](./media/data-lake-store-with-data-catalog/register-data-source.png "een gegevensbron registreren")
3. Klik op de volgende pagina **toepassing starten**. Hiermee wordt het manifestbestand van toepassing op uw computer downloaden. Dubbelklik op het manifestbestand voor het starten van de toepassing.
4. Klik op de welkomstpagina op **aanmelden**, en voer uw referenties.

    ![Welkomstscherm](./media/data-lake-store-with-data-catalog/welcome.screen.png "welkomstscherm")
5. Selecteer op de pagina voor een gegevensbron Selecteer **Azure Data Lake**, en klik vervolgens op **volgende**.

    ![Selecteer gegevensbron](./media/data-lake-store-with-data-catalog/select-source.png "gegevensbron selecteren")
6. Geef de naam van de Data Lake Store-account dat u wilt registreren in de catalogus met gegevens op de volgende pagina. Laat de overige opties als standaard en klik vervolgens op **Connect**.

    ![Verbinding maken met gegevensbron](./media/data-lake-store-with-data-catalog/connect-to-source.png "verbinding maken met gegevensbron")
7. De volgende pagina kan worden onderverdeeld in de volgende segmenten.

    a. De **serverhiërarchie** vak geeft de mapstructuur van de Data Lake Store-account. **$Root** vertegenwoordigt de hoofdmap van de Data Lake Store-account en **AmbulanceData** vertegenwoordigt de map in de hoofdmap van het Data Lake Store-account hebt gemaakt.

    b. De **beschikbare objecten** vak geeft een lijst van de bestanden en mappen onder de **AmbulanceData** map.

    c. **Objecten die geregistreerde vak** geeft een lijst van de bestanden en mappen die u wilt registreren in Azure Data Catalog.

    ![Bekijk gegevensstructuur](./media/data-lake-store-with-data-catalog/view-data-structure.png "gegevensstructuur weergeven")
8. Voor deze zelfstudie moet u alle bestanden in de directory te registreren. Die, klikt u op de (![objecten verplaatsen](./media/data-lake-store-with-data-catalog/move-objects.png "objecten verplaatsen")) om alle bestanden te verplaatsen **te registreren objecten** vak.

    Omdat de gegevens worden geregistreerd in een catalogus met gegevens van de hele organisatie, is een aanbevolen aanpak om toe te voegen, bepaalde metagegevens die u later gebruiken kunt om snel te zoeken de gegevens. U kunt bijvoorbeeld een e-mailadres toevoegen voor de eigenaar van de gegevens (bijvoorbeeld een die van de gegevens uploaden) of tag om te bepalen welke gegevens toevoegen. De schermafbeelding hieronder ziet u een label dat we aan de gegevens toevoegen.

    ![Bekijk gegevensstructuur](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "gegevensstructuur weergeven")

    Klik op **registreren**.
9. De volgende schermopname geeft aan dat de gegevens met succes is geregistreerd in de catalogus met gegevens.

    ![Inschrijving voltooid](./media/data-lake-store-with-data-catalog/registration-complete.png "gegevensstructuur weergeven")
10. Klik op **Portal weergeven** terug te keren naar de Data Catalog-portal en controleer of dat u nu toegang de geregistreerde gegevens vanuit de portal tot hebt. Om te zoeken naar de gegevens, kunt u de code die u hebt gebruikt bij het registreren van de gegevens.

     ![Gegevens zoeken in de catalogus](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "gegevens zoeken in catalogus")
11. U kunt nu acties uitvoeren, zoals documentatie en aantekeningen toevoegen aan de gegevens. Zie de volgende koppelingen voor meer informatie.

    * [Aantekeningen toevoegen aan gegevensbronnen in de catalogus met gegevens](../data-catalog/data-catalog-how-to-annotate.md)
    * [Gegevensbronnen van document in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Zie ook
* [Aantekeningen toevoegen aan gegevensbronnen in de catalogus met gegevens](../data-catalog/data-catalog-how-to-annotate.md)
* [Gegevensbronnen van document in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Data Lake Store integreren met andere Azure-services](data-lake-store-integrate-with-other-services.md)
