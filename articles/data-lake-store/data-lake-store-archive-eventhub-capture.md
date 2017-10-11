---
title: Vastleggen van gegevens uit Event Hubs in Azure Data Lake Store | Microsoft Docs
description: Azure Data Lake Store gebruiken voor het vastleggen van gegevens uit Event Hubs
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: a9e69576958ae96d22a4eb03d0df429f0b307298
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Azure Data Lake Store gebruiken voor het vastleggen van gegevens uit Event Hubs

Informatie over het gebruik van Azure Data Lake Store voor het vastleggen van gegevens die zijn ontvangen door Azure Event Hubs.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md).

*  **Een naamruimte Event Hubs**. Zie voor instructies [maken van een naamruimte Event Hubs](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Zorg ervoor dat de Data Lake Store-account en de naamruimte van Event Hubs zijn in dezelfde Azure-abonnement.


## <a name="assign-permissions-to-event-hubs"></a>Wijs machtigingen toe aan de Event Hubs

In deze sectie maakt u een map in het account waarbij u wilt vastleggen van de gegevens uit Event Hubs. U ook toewijzen machtigingen aan Event Hubs zodat van gegevens naar een Data Lake Store-account schrijven. 

1. Open de Data Lake Store-account waar u wilt vastleggen van gegevens uit Event Hubs en klik vervolgens op **Data Explorer**.

    ![Data Lake Store Gegevensverkenner](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Gegevensverkenner Data Lake Store")

2.  Klik op **nieuwe map** en voer een naam voor de map waar u de gegevens vastlegt.

    ![Een nieuwe map maken in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "een nieuwe map maken in Data Lake Store")

3. Wijs machtigingen in de hoofdmap van de Data Lake Store toe. 

    a. Klik op **Data Explorer**, selecteer de hoofdmap van het Data Lake Store-account en klik vervolgens op **toegang**.

    ![Machtigingen voor Data Lake Store-basis toe te wijzen](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "toegangsrechten voor de hoofdmap van de Data Lake Store")

    b. Onder **toegang**, klikt u op **toevoegen**, klikt u op **gebruiker of groep selecteren**, en zoek vervolgens naar `Microsoft.EventHubs`. 

    ![Machtigingen voor Data Lake Store-basis toe te wijzen](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "toegangsrechten voor de hoofdmap van de Data Lake Store")
    
    Klik op **Selecteren**.

    c. Onder **machtigingen toewijzen**, klikt u op **Selecteer machtigingen**. Stel **machtigingen** naar **uitvoeren**. Stel **toevoegen aan** naar **deze map en alle onderliggende**. Stel **toevoegen als** naar **een machtigingsvermelding toegang en een standaard machtigingsvermelding**.

    ![Machtigingen voor Data Lake Store-basis toe te wijzen](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "toegangsrechten voor de hoofdmap van de Data Lake Store")

    Klik op **OK**.

4. Wijs machtigingen voor de map in Data Lake Store-account waar u wilt vastleggen van gegevens.

    a. Klik op **Data Explorer**, selecteer de map in de Data Lake Store-account en klik vervolgens op **toegang**.

    ![Wijs machtigingen voor Data Lake Store-map](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "machtigingen voor Data Lake Store-map toewijzen")

    b. Onder **toegang**, klikt u op **toevoegen**, klikt u op **gebruiker of groep selecteren**, en zoek vervolgens naar `Microsoft.EventHubs`. 

    ![Wijs machtigingen voor Data Lake Store-map](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "machtigingen voor Data Lake Store-map toewijzen")
    
    Klik op **Selecteren**.

    c. Onder **machtigingen toewijzen**, klikt u op **Selecteer machtigingen**. Stel **machtigingen** naar **lezen, schrijven,** en **uitvoeren**. Stel **toevoegen aan** naar **deze map en alle onderliggende**. Tot slot stelt **toevoegen als** naar **een machtigingsvermelding toegang en een standaard machtigingsvermelding**.

    ![Wijs machtigingen voor Data Lake Store-map](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "machtigingen voor Data Lake Store-map toewijzen")
    
    Klik op **OK**. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Event Hubs voor het vastleggen van gegevens naar Data Lake Store configureren

In deze sectie maakt u een Event Hub in een Event Hubs-naamruimte. U ook configureren de Event Hub voor het vastleggen van gegevens naar een Azure Data Lake Store-account. Deze sectie wordt ervan uitgegaan dat u al een naamruimte Event Hubs hebt gemaakt.

2. Van de **overzicht** deelvenster van de naamruimte van Event Hubs, klikt u op **+ Event Hub**.

    ![Event Hub maken](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Event Hub maken")

3. Geef de volgende waarden voor het configureren van Event Hubs voor het vastleggen van gegevens naar Data Lake Store.

    ![Event Hub maken](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Event Hub maken")

    a. Geef een naam voor de Event Hub.
    
    b. Voor deze zelfstudie stelt **aantal partities** en **bericht bewaren** op de standaardwaarden.
    
    c. Stel **vastleggen** naar **op**. Stel de **tijdvenster** (hoe vaak om vast te leggen) en **grootte venster** (gegevensgrootte voor het vastleggen van). 
    
    d. Voor **vastleggen Provider**, selecteer **Azure Data Lake Store** en het selecteren van de Data Lake Store u eerder hebt gemaakt. Voor **pad naar de Data Lake**, voer de naam van de map die u hebt gemaakt in de Data Lake Store-account. U hoeft alleen te bieden het relatieve pad naar de map.

    e. Laat de **voorbeeld vastleggen bestandsindelingen naam** op de standaardwaarde. Deze optie bepaalt de mapstructuur die wordt gemaakt onder de map vastleggen.

    f. Klik op **Create**.

## <a name="test-the-setup"></a>De instellingen testen

U kunt nu de oplossing testen door gegevens te verzenden naar de Azure Event Hub. Volg de instructies voor [gebeurtenissen verzenden naar Azure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Wanneer u begint met het verzenden van de gegevens, ziet u de gegevens in Data Lake Store worden weergegeven met de mapstructuur die u hebt opgegeven. Bijvoorbeeld, ziet u een mapstructuur, zoals wordt weergegeven in de volgende schermafbeelding, in uw Data Lake Store.

![Voorbeeld van EventHub-gegevens in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Sample EventHub-gegevens in Data Lake Store")

> [!NOTE]
> Zelfs als er geen inkomende berichten bij Event Hubs, schrijft Event Hubs leeg bestanden met alleen de kopteksten in de Data Lake Store-account. De bestanden zijn geschreven met de dezelfde tijdsinterval die u hebt opgegeven tijdens het maken van de Event Hubs.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Gegevens analyseren in Data Lake Store

Nadat de gegevens Data Lake Store is, kunt u analytische taken uitvoeren om te verwerken en crisis de gegevens. Zie [USQL Avro voorbeeld](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) op hoe u kunt dit doen met Azure Data Lake Analytics.
  

## <a name="see-also"></a>Zie ook
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
