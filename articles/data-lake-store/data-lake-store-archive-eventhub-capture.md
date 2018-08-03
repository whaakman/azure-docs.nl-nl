---
title: Vastleggen van gegevens uit Event Hubs in Azure Data Lake Store | Microsoft Docs
description: Gebruik Azure Data Lake Store om vast te leggen van de gegevens uit Event Hubs
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: bda52acc12aad3cad20143c319f557f11d760c42
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435149"
---
# <a name="use-azure-data-lake-store-to-capture-data-from-event-hubs"></a>Gebruik Azure Data Lake Store om vast te leggen van de gegevens uit Event Hubs

Informatie over het gebruik van Azure Data Lake Store om vast te leggen die worden ontvangen door de Azure Event Hubs.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een Azure Data Lake Store-account**. Zie [Aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md) voor instructies over hoe u zo’n account maakt.

*  **Een Event Hubs-naamruimte**. Zie voor instructies [maken van een Event Hubs-naamruimte](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Zorg ervoor dat de Data Lake Store-account en de Event Hubs-naamruimte zich in hetzelfde Azure-abonnement.


## <a name="assign-permissions-to-event-hubs"></a>Machtigingen toewijzen aan Event Hubs

In deze sectie maakt u een map in het account waarbij u wilt vastleggen van de gegevens van Event Hubs. U ook toewijzen machtigingen aan Event Hubs zodat deze gegevens naar een Data Lake Store-account schrijven kan. 

1. Open het Data Lake Store-account waar u wilt vastleggen van gegevens uit Event Hubs en klik vervolgens op **Data Explorer**.

    ![Data Lake Store-Gegevensverkenner](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "data explorer van Data Lake Store")

1.  Klik op **nieuwe map** en voer een naam voor de map waar u de gegevens vast te leggen.

    ![Een nieuwe map maken in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "een nieuwe map maken in Data Lake Store")

1. Toewijzen van machtigingen in de hoofdmap van de Data Lake Store. 

    a. Klik op **Data Explorer**, selecteert u de hoofdmap van het Data Lake Store-account en klik vervolgens op **toegang**.

    ![Machtigingen voor de hoofdmap van de Data Lake Store toewijzen](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "toewijzen van machtigingen voor de hoofdmap van de Data Lake Store")

    b. Onder **toegang**, klikt u op **toevoegen**, klikt u op **Select User or Group**, en zoek vervolgens `Microsoft.EventHubs`. 

    ![Machtigingen voor de hoofdmap van de Data Lake Store toewijzen](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "toewijzen van machtigingen voor de hoofdmap van de Data Lake Store")
    
    Klik op **Selecteren**.

    c. Onder **machtigingen toewijzen**, klikt u op **Selecteer machtigingen**. Stel **machtigingen** naar **uitvoeren**. Stel **toevoegen aan** naar **deze map en alle onderliggende**. Stel **toevoegen als** naar **vermelding van een toegangsmachtiging en een standaardmachtiging**.

    > [!IMPORTANT]
    > Bij het maken van een nieuwe maphiërarchie voor het vastleggen van gegevens die zijn ontvangen door de Azure Event Hubs, is dit een eenvoudige manier om te controleren of de toegang tot de doelmap.  Machtigingen op alle onderliggende objecten van een map op het hoogste niveau toe te voegen met veel onderliggende bestanden en mappen kan echter een lange tijd duren.  Als de hoofdmap een groot aantal bestanden en mappen bevat, kan het zijn sneller om toe te voegen **Execute** machtigingen voor `Microsoft.EventHubs` afzonderlijk naar elke map in het pad naar de map van uw uiteindelijke bestemming. 

    ![Machtigingen voor de hoofdmap van de Data Lake Store toewijzen](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "toewijzen van machtigingen voor de hoofdmap van de Data Lake Store")

    Klik op **OK**.

1. Wijs machtigingen voor de submap onder de Data Lake Store-account waar u gegevens vast te leggen.

    a. Klik op **Data Explorer**, selecteer de map in het Data Lake Store-account en klik vervolgens op **toegang**.

    ![Toewijzen van machtigingen voor Data Lake Store-map](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "toewijzen van machtigingen voor Data Lake Store-map")

    b. Onder **toegang**, klikt u op **toevoegen**, klikt u op **Select User or Group**, en zoek vervolgens `Microsoft.EventHubs`. 

    ![Toewijzen van machtigingen voor Data Lake Store-map](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "toewijzen van machtigingen voor Data Lake Store-map")
    
    Klik op **Selecteren**.

    c. Onder **machtigingen toewijzen**, klikt u op **Selecteer machtigingen**. Stel **machtigingen** naar **lezen, schrijven,** en **uitvoeren**. Stel **toevoegen aan** naar **deze map en alle onderliggende**. Tot slot stelt **toevoegen als** naar **vermelding van een toegangsmachtiging en een standaardmachtiging**.

    ![Toewijzen van machtigingen voor Data Lake Store-map](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "toewijzen van machtigingen voor Data Lake Store-map")
    
    Klik op **OK**. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-store"></a>Event Hubs voor het vastleggen van gegevens naar Data Lake Store configureren

In deze sectie maakt u een Event Hub in een Event Hubs-naamruimte. U ook configureren de Event Hub voor het vastleggen van gegevens naar een Azure Data Lake Store-account. In deze sectie wordt ervan uitgegaan dat u al een Event Hubs-naamruimte hebt gemaakt.

1. Uit de **overzicht** deelvenster van de Event Hubs-naamruimte, klikt u op **+ Event Hub**.

    ![Event Hub maken](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Event Hub maken")

1. Geef de volgende waarden voor het configureren van Event Hubs voor het vastleggen van gegevens naar Data Lake Store.

    ![Event Hub maken](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Event Hub maken")

    a. Geef een naam voor de Event Hub.
    
    b. Voor deze zelfstudie stelt **aantal partities** en **bewaarperiode van bericht** op de standaardwaarden.
    
    c. Stel **vastleggen** naar **op**. Stel de **tijdvenster** (hoe vaak om vast te leggen) en **venstergrootte** (gegevensgrootte om vast te leggen). 
    
    d. Voor **Capture-Provider**, selecteer **Azure Data Lake Store** en het selecteren van de Data Lake Store u eerder hebt gemaakt. Voor **Data Lake-pad**, voer de naam van de map die u hebt gemaakt in de Data Lake Store-account. U hoeft alleen het relatieve pad naar de map opgeven.

    e. Laat de **voorbeeld vastleggen bestandsindelingen naam** op de standaardwaarde. Deze optie bepaalt de mapstructuur die wordt gemaakt onder de map vastleggen.

    f. Klik op **Create**.

## <a name="test-the-setup"></a>De instellingen testen

U kunt nu de oplossing testen door het verzenden van gegevens naar de Azure Event Hub. Volg de instructies op [gebeurtenissen verzenden naar Azure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Als u het verzenden van de gegevens hebt gestart, ziet u de gegevens weergegeven in Data Lake Store met behulp van de mapstructuur die u hebt opgegeven. Bijvoorbeeld, ziet u een mapstructuur, zoals wordt weergegeven in de volgende schermafbeelding, in uw Data Lake Store.

![Voorbeeld van EventHub-gegevens in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "voorbeeld EventHub-gegevens in Data Lake Store")

> [!NOTE]
> Zelfs als u geen berichten in Event Hubs, schrijft Event Hubs lege bestanden met alleen de headers naar de Data Lake Store-account. De bestanden zijn geschreven met de dezelfde tijdsinterval die u hebt opgegeven tijdens het maken van de Event Hubs.
> 
>

## <a name="analyze-data-in-data-lake-store"></a>Gegevens analyseren in Data Lake Store

Wanneer de gegevens zich bevinden in Data Lake Store, kunt u analytische taken uitvoeren om te verwerken en benut de gegevens. Zie [USQL Avro voorbeeld](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) op hoe u dit doet met behulp van Azure Data Lake Analytics.
  

## <a name="see-also"></a>Zie ook
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
