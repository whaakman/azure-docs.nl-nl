---
title: Gegevens uit Azure Data Lake Storage Gen1 registreren in Azure Data Catalog | Microsoft Docs
description: Gegevens uit Azure Data Lake Storage Gen1 registreren in Azure Data Catalog
services: data-lake-store,data-catalog
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 3294d91e-a723-41b5-9eca-ace0ee408a4b
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: fd887560c0011fb1ec2141e33f02f7e3d8a39c81
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877881"
---
# <a name="register-data-from-azure-data-lake-storage-gen1-in-azure-data-catalog"></a>Gegevens uit Azure Data Lake Storage Gen1 registreren in Azure Data Catalog
In dit artikel leert u hoe u Azure Data Lake Storage Gen1 integreert met Azure Data Catalog uw gegevens kunnen worden gedetecteerd binnen een organisatie maken door deze te integreren met Data Catalog. Zie voor meer informatie over gegevens catalogiseren, [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). Zie voor meer informatie over scenario's waarin u Data Catalog kunt gebruiken, [algemene scenario's voor Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Uw Azure-abonnement inschakelen** voor Data Lake Storage Gen1. Zie [Instructies](data-lake-store-get-started-portal.md).
* **De account van een Data Lake Storage Gen1**. Volg de instructies op [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-Portal](data-lake-store-get-started-portal.md). Voor deze zelfstudie maakt u een Data Lake Storage Gen1-account met de naam **datacatalogstore**.

    Nadat u het account hebt gemaakt, een verzameling voorbeeldgegevens te uploaden. Voor deze zelfstudie laat het ons uploaden alle CSV-bestanden onder de **AmbulanceData** map in de [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). U kunt verschillende clients, zoals [Azure Storage Explorer](https://storageexplorer.com/), het uploaden van gegevens naar een blob-container.
* **Azure Data Catalog**. Uw organisatie moet een Azure Data Catalog gemaakt voor uw organisatie al hebben. Slechts één catalogus is toegestaan voor elke organisatie.

## <a name="register-data-lake-storage-gen1-as-a-source-for-data-catalog"></a>Data Lake Storage Gen1 registreren als een bron voor Data Catalog

> [!VIDEO https://channel9.msdn.com/Series/AzureDataLake/ADCwithADL/player]

1. Ga naar `https://azure.microsoft.com/services/data-catalog`, en klikt u op **aan de slag**.
1. Meld u aan bij de Azure Data Catalog-portal en klikt u op **gegevens publiceren**.

    ![Een gegevensbron registreert](./media/data-lake-store-with-data-catalog/register-data-source.png "een gegevensbron registreren")
1. Klik op de volgende pagina **toepassing starten**. Hiermee downloadt u het manifestbestand van toepassing op uw computer. Dubbelklik op het manifestbestand voor het starten van de toepassing.
1. Klik op de pagina Welkom **aanmelden**, en voer uw referenties.

    ![Welkomstscherm](./media/data-lake-store-with-data-catalog/welcome.screen.png "welkomstscherm wordt weergegeven")
1. Selecteer op de selecteert u een pagina gegevensbron **Azure Data Lake Store**, en klik vervolgens op **volgende**.

    ![Selecteer gegevensbron](./media/data-lake-store-with-data-catalog/select-source.png "gegevensbron selecteren")
1. Geef op de volgende pagina de naam van het Data Lake Storage Gen1 die u wilt registreren in Data Catalog. Laat de standaard de andere opties en klik vervolgens op **Connect**.

    ![Verbinding maken met gegevensbron](./media/data-lake-store-with-data-catalog/connect-to-source.png "verbinding maken met gegevensbron")
1. De volgende pagina kan worden onderverdeeld in de volgende segmenten.

    a. De **serverhiërarchie** vak geeft de mapstructuur van de Data Lake Storage Gen1-account. **$Root** vertegenwoordigt de hoofdmap van de Gen1 van Data Lake Storage-account en **AmbulanceData** Hiermee geeft u de map in de hoofdmap van het Data Lake Storage Gen1-account hebt gemaakt.

    b. De **beschikbare objecten** vak geeft een lijst van de bestanden en mappen aan onder de **AmbulanceData** map.

    c. **Te registreren objecten** vak geeft een lijst van de bestanden en mappen die u wilt registreren in Azure Data Catalog.

    ![Bekijk gegevensstructuur](./media/data-lake-store-with-data-catalog/view-data-structure.png "gegevensstructuur weergeven")
1. Voor deze zelfstudie, moet u alle bestanden in de directory te registreren. Hiervoor klikt u op de (![objecten verplaatsen](./media/data-lake-store-with-data-catalog/move-objects.png "objecten verplaatsen")) om de bestanden te verplaatsen **te registreren objecten** vak.

    Omdat de gegevens wordt geregistreerd in een catalogus met gegevens van de hele organisatie, is het een aanbevolen aanpak voor het toevoegen van een aantal metagegevens die u later gebruiken kunt om snel te vinden de gegevens. U kunt bijvoorbeeld een e-mailadres toevoegen voor de eigenaar van de gegevens (bijvoorbeeld een die van de gegevens uploaden) of een code voor het identificeren van de gegevens toevoegen. De schermafbeelding hieronder ziet u een label dat u aan de gegevens toevoegt.

    ![Bekijk gegevensstructuur](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "gegevensstructuur weergeven")

    Klik op **registreren**.
1. De volgende schermopname geeft aan dat de gegevens in de Data Catalog is geregistreerd.

    ![Registratie is voltooid](./media/data-lake-store-with-data-catalog/registration-complete.png "gegevensstructuur weergeven")
1. Klik op **Portal weergeven** gaat u terug naar de Data Catalog-portal en controleer of dat u nu toegang de geregistreerde gegevens vanuit de portal tot kunt. Als u wilt zoeken naar de gegevens, kunt u de code die u hebt gebruikt tijdens het registreren van de gegevens.

     ![Zoeken naar gegevens in de catalogus](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "gegevens in de catalogus doorzoeken")
1. Nu kunt u bewerkingen zoals het toevoegen van aantekeningen en documentatie aan de gegevens uitvoeren. Zie de volgende koppelingen voor meer informatie.

    * [Aantekeningen toevoegen aan gegevensbronnen in Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
    * [Gegevensbronnen documenteren in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Zie ook
* [Aantekeningen toevoegen aan gegevensbronnen in Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Gegevensbronnen documenteren in Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Data Lake Storage Gen1 integreren met andere Azure-services](data-lake-store-integrate-with-other-services.md)
