---
title: Uw gegevens exporteren uit Azure IoT Central | Microsoft Docs
description: Het exporteren van gegevens vanuit uw Azure IoT Central-toepassing
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 80ba612bf0f76fb0c1e202bec0bf88fa75fb2c98
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314004"
---
# <a name="export-your-data-from-azure-iot-central"></a>Uw gegevens exporteren uit Azure IoT Central

*In dit onderwerp is bedoeld voor beheerders.*

In dit artikel wordt beschreven hoe u van de continue export-functie in Azure IoT Central uw gegevens te exporteren naar uw eigen **Azure Blob Storage**, **Azure Event Hubs**, en **Azure Service Bus** exemplaren. U kunt exporteren **metingen**, **apparaten**, en **apparaatsjablonen** aan uw eigen bestemming van warm-pad en het koude pad analytics. U kunt gegevens exporteren naar Blob-opslag op lange termijn trendanalyse uitvoeren in Microsoft Power BI of gegevens exporteren naar Event Hubs en Service Bus te transformeren en uitbreiden van uw gegevens in bijna realtime met Azure Logic Apps of Azure Functions.

> [!Note]
> Wanneer u doorlopend gegevens exporteren inschakelt, kunt u alleen de gegevens ophalen vanaf dat moment standaardtarieven. Gegevens kunnen op dit moment niet worden hersteld gedurende een periode wanneer voortdurende gegevensexport uitgeschakeld is. Als u wilt meer historische gegevens behouden, moet u voortdurende gegevensexport vroeg inschakelen.

## <a name="prerequisites"></a>Vereisten

U moet een beheerder in uw IoT Central-toepassing

## <a name="export-to-blob-storage"></a>Exporteren naar Blob Storage

Metingen, apparaten en sjablonen apparaatgegevens worden geëxporteerd naar uw opslagaccount eenmaal per minuut, met elk bestand met de batch wijzigingen sinds de laatste bestand geëxporteerd. De geëxporteerde gegevens [Apache AVRO](https://avro.apache.org/docs/current/index.html) indeling.

Meer informatie over [exporteren naar een blobopslag](howto-export-data-blob-storage.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="export-to-event-hubs-and-service-bus"></a>Exporteren naar Event Hubs en Service Bus

Metingen, apparaten en sjablonen apparaatgegevens worden geëxporteerd naar uw event hub of de Service Bus-wachtrij of onderwerp. Geëxporteerde metingen gegevens in bijna realtime binnenkomt en het geheel van het bericht bevat de apparaten verzonden naar IoT Central, niet alleen de waarden van de metingen zelf. Geëxporteerde apparaten gegevens binnenkomt in batches van één keer per minuut en wijzigingen in de eigenschappen en instellingen van alle apparaten bevat en geëxporteerde apparaatsjablonen bevat wijzigingen aan alle apparaatsjablonen.

Meer informatie over [exporteren naar Event Hubs en Service Bus](howto-export-data-event-hubs-service-bus.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="set-up-export-destination"></a>Export doel instellen

Als u een bestaande opslag/Event Hubs-Service Bus om te exporteren naar niet hebt, volgt u deze stappen:

### <a name="create-storage-account"></a>Storage-account maken

1. Maak een [nieuw opslagaccount in Azure portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). U kunt meer informatie in [Azure Storage-docs](https://aka.ms/blobdocscreatestorageaccount).

2. Kies voor het accounttype **algemeen** of **Blob storage**.

3. Kies een abonnement.

    > [!Note]
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maken met behulp van een verbindingsreeks in dit geval.

4. Maak een container in uw opslagaccount. Ga naar uw opslagaccount. Onder **Blob-Service**, selecteer **door Blobs Bladeren**. Selecteer **+ Container** aan de bovenkant om een nieuwe container te maken.

### <a name="create-event-hubs-namespace"></a>Event Hubs-naamruimte maken

1. Maak een [nieuwe Event Hubs-naamruimte in Azure portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). U kunt meer informatie in [Azure Event Hubs docs](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

2. Kies een abonnement.

    > [!Note]
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maken met behulp van een verbindingsreeks in dit geval.

3. Maak een event hub in uw Event Hubs-naamruimte. Ga naar uw naamruimte en selecteer **+ Event Hub** boven aan het maken van een event hub-instantie.

### <a name="create-service-bus-namespace"></a>Service Bus-naamruimte maken

1. Maak een [nieuwe Service Bus-naamruimte in Azure portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . U kunt meer informatie in [Azure Service Bus-docs](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).

2. Kies een abonnement.

    > [!Note]
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maken met behulp van een verbindingsreeks in dit geval.

3. Ga naar uw Service Bus-naamruimte en selecteer **+ wachtrij** of **+ onderwerp** boven aan het maken van een wachtrij of onderwerp om te exporteren naar.

## <a name="set-up-continuous-data-export"></a>Voortdurende gegevensexport instellen

Nu dat u een opslag/Event Hubs-Service Bus-doel hebt voor het exporteren van gegevens, volg deze stappen voor het instellen van continue gegevensexport.

1. Meld u aan uw IoT Central-toepassing.

2. Selecteer in het menu links **continue gegevensexport**.

    > [!Note]
    > Als er geen continue Export van gegevens in het menu links, bent u niet een beheerder in uw app. Neem contact op met een beheerder voor het instellen van het exporteren van gegevens.

    ![Nieuwe Event Hub maken](media/howto-export-data-experimental/export_menu.png)

3. Selecteer de **+ nieuw** knop in de rechterbovenhoek. Kies een van de **Azure Blob Storage**, **Azure Event Hubs**, of **Azure Service Bus** als de bestemming of het exporteren.

    > [!NOTE]
    > Het maximum aantal uitvoer per app is vijf.

    ![Maken van nieuwe voortdurende gegevensexport](media/howto-export-data-experimental/export_new.png)

4. Selecteer in de vervolgkeuzelijst uw **Storage Account/Event Hubs-naamruimte/Service Bus-naamruimte**. U kunt ook de laatste optie kiezen in de lijst die is **een verbindingsreeks invoeren**. 

    > [!NOTE]
    > U ziet alleen Storage Accounts/Event Hubs-naamruimten/Service Bus-naamruimten in de **hetzelfde abonnement als uw app IoT Central**. Als u exporteren naar een bestemming buiten dit abonnement wilt, kiest u **een verbindingsreeks invoeren** en raadpleegt u stap 5.

    > [!NOTE]
    > Voor zeven dagen proefversie apps, de enige manier om het configureren van doorlopende gegevens exporteren, is via een verbindingsreeks. Dit komt doordat zeven dagen proefversie apps nog geen een gekoppelde Azure-abonnement.

    ![Nieuwe cde Event Hub maken](media/howto-export-data-experimental/export_create.png)

5. (Optioneel) Als u ervoor hebt gekozen **een verbindingsreeks invoeren**, een nieuwe verschijnt u plak de verbindingsreeks. Om op te halen van de verbindingsreeks voor uw:
    - Storage-account, gaat u naar het opslagaccount in de Azure-Portal.
        - Onder **instellingen**, selecteer **toegangssleutels**
        - Kopieer de verbindingsreeks key1 of de key2-verbindingsreeks
    - Eventhubs of Service Bus, gaat u naar de naamruimte in de Azure Portal.
        - Onder **instellingen**, selecteer **beleid voor gedeelde toegang**
        - Kies de standaardwaarden voor **RootManageSharedAccessKey** of een nieuw wachtwoord maken
        - Kopieer de primaire of secundaire verbindingsreeks

6. Kies een Container/Event hub/wachtrij of onderwerp in het vak vervolgkeuzelijst.

7. Onder **gegevens naar de export**, elk type gegevens wilt exporteren door het type in te stellen **op**.

8. Als u wilt inschakelen voortdurende gegevensexport, zorg ervoor dat **gegevensexport** is **op**. Selecteer **Opslaan**.

    ![Voortdurende gegevensexport configureren](media/howto-export-data-experimental/export_list.png)

9. Na een paar minuten uw gegevens worden weergegeven in uw gekozen doel.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hoe u uw gegevens te exporteren weet, gaat u verder met de volgende stap:

> [!div class="nextstepaction"]
> [Gegevens exporteren naar Azure Blob-opslag](howto-export-data-blob-storage.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

> [!div class="nextstepaction"]
> [Gegevens exporteren naar Azure Event Hubs en Azure Service Bus](howto-export-data-event-hubs-service-bus.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

> [!div class="nextstepaction"]
> [Hoe u uw gegevens in Power BI visualiseren](howto-connect-powerbi.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
