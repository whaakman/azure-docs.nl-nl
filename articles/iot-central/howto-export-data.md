---
title: Exporteren van gegevens in Azure IoT Central | Microsoft Docs
description: Het exporteren van gegevens vanuit uw Azure IoT Central-toepassing
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: cba0bad2e81ffddedfc4ca04e82e17e4286b389b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312116"
---
# <a name="export-your-data-in-azure-iot-central"></a>Uw gegevens in Azure IoT Central exporteren

*In dit onderwerp is bedoeld voor beheerders.*

In dit artikel wordt beschreven hoe u van de continue export-functie in Azure IoT Central uw gegevens te exporteren naar uw eigen **Azure Blob Storage**, **Azure Event Hubs**, en **Azure Service Bus** exemplaren. U kunt exporteren **metingen**, **apparaten**, en **apparaatsjablonen** aan uw eigen bestemming van warm-pad en het koude pad analytics. U kunt gegevens exporteren naar Blob-opslag op lange termijn trendanalyse uitvoeren in Microsoft Power BI of gegevens exporteren naar Event Hubs en Service Bus te transformeren en uitbreiden van uw gegevens in bijna realtime met Azure Logic Apps of Azure Functions.

> [!Note]
> Wanneer u doorlopend gegevens exporteren inschakelt, kunt u alleen de gegevens ophalen vanaf dat moment standaardtarieven. Gegevens kunnen op dit moment niet worden hersteld gedurende een periode wanneer voortdurende gegevensexport uitgeschakeld is. Als u wilt meer historische gegevens behouden, moet u voortdurende gegevensexport vroeg inschakelen.

## <a name="prerequisites"></a>Vereisten

- U moet een beheerder in uw IoT Central-toepassing

## <a name="export-to-blob-storage"></a>Exporteren naar Blob Storage

Metingen, apparaten en sjablonen apparaatgegevens worden geëxporteerd naar uw opslagaccount eenmaal per minuut, met elk bestand met de batch wijzigingen sinds de laatste bestand geëxporteerd. De geëxporteerde gegevens [Apache AVRO](https://avro.apache.org/docs/current/index.html) indeling.

Meer informatie over [exporteren naar een blobopslag](howto-export-data-blob-storage.md).

## <a name="export-to-event-hubs-and-service-bus"></a>Exporteren naar Eventhubs en Servicebus

Metingen, apparaten en sjablonen apparaatgegevens worden geëxporteerd naar uw event hub of de Service Bus-wachtrij of onderwerp. Geëxporteerde metingen gegevens in bijna realtime binnenkomt en het geheel van het bericht bevat de apparaten verzonden naar IoT Central, niet alleen de waarden van de metingen zelf. Geëxporteerde apparaten gegevens binnenkomt in batches van één keer per minuut en wijzigingen in de eigenschappen en instellingen van alle apparaten bevat en geëxporteerde apparaatsjablonen bevat wijzigingen aan alle apparaatsjablonen.


Meer informatie over [exporteren naar Event Hubs en Service Bus](howto-export-data-event-hubs-service-bus.md).

## <a name="set-up-export-destination"></a>Export doel instellen

Als u een bestaande opslag/Event Hubs-Service Bus om te exporteren naar niet hebt, volgt u deze stappen:

### <a name="create-storage-account"></a>Storage-account maken

1. Maak een [nieuw opslagaccount in Azure portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). U kunt meer informatie in [Azure Storage-docs](https://aka.ms/blobdocscreatestorageaccount).
2. Kies voor het accounttype **algemeen** of **Blob storage**.
3. Kies een abonnement. 

    > [!Note] 
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maakt met een verbindingsreeks in dit geval.

4. Maak een container in uw opslagaccount. Ga naar uw storage-account. Onder **Blob-Service**, selecteer **door Blobs Bladeren**. Selecteer **+ Container** aan de bovenkant om een nieuwe container te maken.

### <a name="create-event-hubs-namespace"></a>Event Hubs-naamruimte maken

1. Maak een [nieuwe Event Hubs-naamruimte in Azure portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). U kunt meer informatie in [Azure Event Hubs docs](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Kies een abonnement. 

    > [!Note] 
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maakt met een verbindingsreeks in dit geval.
3. Maak een event hub in uw Event Hubs-naamruimte. Ga naar uw naamruimte en selecteer **+ Event Hub** boven aan het maken van een event hub-instantie.

### <a name="create-service-bus-namespace"></a>Service Bus-naamruimte maken

1. Maak een [nieuwe Service Bus-naamruimte in Azure portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . U kunt meer informatie in [Azure Service Bus-docs](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Kies een abonnement. 

    > [!Note] 
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maakt met een verbindingsreeks in dit geval.

3. Ga naar uw Service Bus-naamruimte en selecteer **+ wachtrij** of **+ onderwerp** boven aan het maken van een wachtrij of onderwerp om te exporteren naar.

## <a name="set-up-continuous-data-export"></a>Voortdurende gegevensexport instellen

Nu dat u een opslag/Event Hubs-Service Bus-doel hebt voor het exporteren van gegevens, volg deze stappen voor het instellen van continue gegevensexport. 

1. Meld u aan uw IoT Central-toepassing.

2. Klik in het menu links op **continue gegevensexport**.

    > [!Note]
    > Als er geen continue Export van gegevens in het menu links, bent u niet een beheerder in uw app. Neem contact op met een beheerder voor het instellen van het exporteren van gegevens.

    ![Nieuwe cde Event Hub maken](media/howto-export-data/export_menu.PNG)

3. Klik op de **+ nieuw** knop in de rechterbovenhoek. Kies een van de **Azure Blob Storage**, **Azure Event Hubs**, of **Azure Service Bus** als de bestemming of het exporteren. 

    > [!NOTE] 
    > Het maximum aantal uitvoer per app is vijf. 

    ![Maken van nieuwe voortdurende gegevensexport](media/howto-export-data/export_new.PNG)

4. Selecteer in de vervolgkeuzelijst uw **Storage Account/Event Hubs-naamruimte/Service Bus-naamruimte**. U kunt ook de laatste optie kiezen in de lijst die is **een verbindingsreeks invoeren**. 

    > [!NOTE] 
    > U ziet alleen Storage Accounts/Event Hubs-naamruimten/Service Bus-naamruimten in de **hetzelfde abonnement als uw app IoT Central**. Als u exporteren naar een bestemming buiten dit abonnement wilt, kiest u **een verbindingsreeks invoeren** en raadpleegt u stap 5.

    > [!NOTE] 
    > Voor zeven dagen proefversie apps, de enige manier om het configureren van doorlopende gegevens exporteren, is via een verbindingsreeks. Dit komt doordat zeven dagen proefversie apps nog geen een gekoppelde Azure-abonnement.

    ![Nieuwe cde Event Hub maken](media/howto-export-data/export_create.PNG)

5. (Optioneel) Als u ervoor hebt gekozen **een verbindingsreeks invoeren**, een nieuwe verschijnt u plak de verbindingsreeks. Om op te halen van de verbindingsreeks voor uw:
    - Storage-account, gaat u naar het opslagaccount in de Azure-Portal.
        - Onder **instellingen**, klikt u op **toegangssleutels**
        - Kopieer de verbindingsreeks key1 of de key2-verbindingsreeks
    - Eventhubs of Service Bus, gaat u naar de naamruimte in de Azure Portal.
        - Onder **instellingen**, klikt u op **beleid voor gedeelde toegang**
        - Kies de standaardwaarden voor **RootManageSharedAccessKey** of een nieuw wachtwoord maken
        - Kopieer de primaire of secundaire verbindingsreeks
 
6. Kies een Container/Event hub/wachtrij of onderwerp in het vak vervolgkeuzelijst.

7. Onder **gegevens naar de export**, elk type gegevens wilt exporteren door het type in te stellen **op**.

6. Als u wilt inschakelen voortdurende gegevensexport, zorg ervoor dat **gegevensexport** is **op**. Selecteer **Opslaan**.

  ![Voortdurende gegevensexport configureren](media/howto-export-data/export_list.PNG)

7. Na een paar minuten verschijnt uw gegevens in uw gekozen bestemming.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hoe u uw gegevens te exporteren weet, gaat u verder met de volgende stap:

> [!div class="nextstepaction"]
> [Gegevens exporteren naar Azure Blob-opslag](howto-export-data-blob-storage.md)

> [!div class="nextstepaction"]
> [Gegevens exporteren naar Azure Event Hubs en Azure Service Bus](howto-export-data-event-hubs-service-bus.md)

> [!div class="nextstepaction"]
> [Hoe u uw gegevens in Power BI visualiseren](howto-connect-powerbi.md)
