---
title: Vastleggen van gegevens uit Event Hubs in Azure Data Lake Storage Gen1 | Microsoft Docs
description: Azure Data Lake Storage Gen1 gebruiken voor het vastleggen van gegevens uit Event Hubs
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
ms.openlocfilehash: a921a717445df93c83518c89970050e63a51ddf3
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345195"
---
# <a name="use-azure-data-lake-storage-gen1-to-capture-data-from-event-hubs"></a>Azure Data Lake Storage Gen1 gebruiken voor het vastleggen van gegevens uit Event Hubs

Informatie over het gebruik van Azure Data Lake Storage Gen1 om vast te leggen die worden ontvangen door de Azure Event Hubs.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Een account met Azure Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md).

*  **Een Event Hubs-naamruimte**. Zie voor instructies [maken van een Event Hubs-naamruimte](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace). Zorg ervoor dat de Gen1 van Data Lake Storage-account en de Event Hubs-naamruimte zich in hetzelfde Azure-abonnement.


## <a name="assign-permissions-to-event-hubs"></a>Machtigingen toewijzen aan Event Hubs

In deze sectie maakt u een map in het account waarbij u wilt vastleggen van de gegevens van Event Hubs. U ook toewijzen machtigingen aan Event Hubs zodat deze gegevens naar een Gen1 van Data Lake Storage-account schrijven kan. 

1. Open het Data Lake Storage Gen1 account waar u wilt vastleggen van gegevens uit Event Hubs en klik vervolgens op **Data Explorer**.

    ![Data Lake Storage Gen1 Gegevensverkenner](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Storage Gen1 data explorer")

1.  Klik op **nieuwe map** en voer een naam voor de map waar u de gegevens vast te leggen.

    ![Een nieuwe map maken in Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "een nieuwe map maken in Data Lake Storage Gen1")

1. Machtigingen in de hoofdmap van Data Lake Storage Gen1 toewijzen. 

    a. Klik op **Data Explorer**, selecteert u de hoofdmap van het Data Lake Storage Gen1-account en klik vervolgens op **toegang**.

    ![Toewijzen van machtigingen voor de hoofdmap van de Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "machtigingen voor de hoofdmap van de Data Lake Storage Gen1 toewijzen")

    b. Onder **toegang**, klikt u op **toevoegen**, klikt u op **Select User or Group**, en zoek vervolgens `Microsoft.EventHubs`. 

    ![Toewijzen van machtigingen voor de hoofdmap van de Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "machtigingen voor de hoofdmap van de Data Lake Storage Gen1 toewijzen")
    
    Klik op **Selecteren**.

    c. Onder **machtigingen toewijzen**, klikt u op **Selecteer machtigingen**. Stel **machtigingen** naar **uitvoeren**. Stel **toevoegen aan** naar **deze map en alle onderliggende**. Stel **toevoegen als** naar **vermelding van een toegangsmachtiging en een standaardmachtiging**.

    > [!IMPORTANT]
    > Bij het maken van een nieuwe maphiërarchie voor het vastleggen van gegevens die zijn ontvangen door de Azure Event Hubs, is dit een eenvoudige manier om te controleren of de toegang tot de doelmap.  Machtigingen op alle onderliggende objecten van een map op het hoogste niveau toe te voegen met veel onderliggende bestanden en mappen kan echter een lange tijd duren.  Als de hoofdmap een groot aantal bestanden en mappen bevat, kan het zijn sneller om toe te voegen **Execute** machtigingen voor `Microsoft.EventHubs` afzonderlijk naar elke map in het pad naar de map van uw uiteindelijke bestemming. 

    ![Toewijzen van machtigingen voor de hoofdmap van de Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "machtigingen voor de hoofdmap van de Data Lake Storage Gen1 toewijzen")

    Klik op **OK**.

1. Wijs machtigingen voor de submap onder de Gen1 van Data Lake Storage-account waar u gegevens vast te leggen.

    a. Klik op **Data Explorer**, selecteer de map in de Gen1 van Data Lake Storage-account en klik vervolgens op **toegang**.

    ![Toewijzen van machtigingen voor de map Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "machtigingen voor de map Data Lake Storage Gen1 toewijzen")

    b. Onder **toegang**, klikt u op **toevoegen**, klikt u op **Select User or Group**, en zoek vervolgens `Microsoft.EventHubs`. 

    ![Toewijzen van machtigingen voor de map Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "machtigingen voor de map Data Lake Storage Gen1 toewijzen")
    
    Klik op **Selecteren**.

    c. Onder **machtigingen toewijzen**, klikt u op **Selecteer machtigingen**. Stel **machtigingen** naar **lezen, schrijven,** en **uitvoeren**. Stel **toevoegen aan** naar **deze map en alle onderliggende**. Tot slot stelt **toevoegen als** naar **vermelding van een toegangsmachtiging en een standaardmachtiging**.

    ![Toewijzen van machtigingen voor de map Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "machtigingen voor de map Data Lake Storage Gen1 toewijzen")
    
    Klik op **OK**. 

## <a name="configure-event-hubs-to-capture-data-to-data-lake-storage-gen1"></a>Event Hubs voor het vastleggen van gegevens naar Data Lake Storage Gen1 configureren

In deze sectie maakt u een Event Hub in een Event Hubs-naamruimte. U ook configureren de Event Hub voor het vastleggen van gegevens naar een Azure Data Lake Storage Gen1-account. In deze sectie wordt ervan uitgegaan dat u al een Event Hubs-naamruimte hebt gemaakt.

1. Uit de **overzicht** deelvenster van de Event Hubs-naamruimte, klikt u op **+ Event Hub**.

    ![Event Hub maken](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Event Hub maken")

1. Geef de volgende waarden voor het configureren van Event Hubs voor het vastleggen van gegevens naar Data Lake Storage Gen1.

    ![Event Hub maken](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Event Hub maken")

    a. Geef een naam voor de Event Hub.
    
    b. Voor deze zelfstudie stelt **aantal partities** en **bewaarperiode van bericht** op de standaardwaarden.
    
    c. Stel **vastleggen** naar **op**. Stel de **tijdvenster** (hoe vaak om vast te leggen) en **venstergrootte** (gegevensgrootte om vast te leggen). 
    
    d. Voor **Capture-Provider**, selecteer **Azure Data Lake Store** en selecteer vervolgens het Gen1 van Data Lake Storage-account dat u eerder hebt gemaakt. Voor **Data Lake-pad**, voer de naam van de map die u hebt gemaakt in de Gen1 van Data Lake Storage-account. U hoeft alleen het relatieve pad naar de map opgeven.

    e. Laat de **voorbeeld vastleggen bestandsindelingen naam** op de standaardwaarde. Deze optie bepaalt de mapstructuur die wordt gemaakt onder de map vastleggen.

    f. Klik op **Create**.

## <a name="test-the-setup"></a>De instellingen testen

U kunt nu de oplossing testen door het verzenden van gegevens naar de Azure Event Hub. Volg de instructies op [gebeurtenissen verzenden naar Azure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md). Als u het verzenden van de gegevens hebt gestart, ziet u de gegevens weergegeven in Data Lake Storage Gen1 met behulp van de map structuur, u hebt opgegeven. Bijvoorbeeld, ziet u een mapstructuur, zoals wordt weergegeven in de volgende schermafbeelding, in uw Data Lake Storage Gen1-account.

![Voorbeeld van EventHub-gegevens in Data Lake Storage Gen1](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "voorbeeld EventHub-gegevens in Data Lake Storage Gen1")

> [!NOTE]
> Zelfs als u geen berichten in Event Hubs, schrijft Event Hubs lege bestanden met alleen de headers naar de Gen1 van Data Lake Storage-account. De bestanden zijn geschreven met de dezelfde tijdsinterval die u hebt opgegeven tijdens het maken van de Event Hubs.
> 
>

## <a name="analyze-data-in-data-lake-storage-gen1"></a>Gegevens analyseren in Data Lake Storage Gen1

Wanneer de gegevens zich bevinden in Data Lake Storage Gen1, kunt u analytische taken uitvoeren om te verwerken en benut de gegevens. Zie [USQL Avro voorbeeld](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) op hoe u dit doet met behulp van Azure Data Lake Analytics.
  

## <a name="see-also"></a>Zie ook
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
