---
title: Azure IoT Central uitbreiden met aangepaste analytics | Microsoft Docs
description: Als ontwikkelaar oplossing een IoT Central-toepassing te doen aangepaste analyses en visualisaties te configureren. Deze oplossing maakt gebruik van Azure Databricks.
author: dominicbetts
ms.author: dobett
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e039e2b8d9c183b5bfee1bee47e4addc4c873bf7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66743438"
---
# <a name="extend-azure-iot-central-with-custom-analytics"></a>Azure IoT Central uitbreiden met aangepaste analytics

In deze gebruiksaanwijzing ziet u, als de ontwikkelaar van een oplossing voor het uitbreiden van uw IoT Central-toepassing met aangepaste analyses en visualisaties. Het voorbeeld wordt een [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) werkruimte voor het analyseren van de telemetriestroom IoT Central en voor het genereren van visualisaties, zoals [grafieken vak](https://wikipedia.org/wiki/Box_plot).

In deze gebruiksaanwijzing ziet u hoe u kunt IoT Central uitbreiden buiten deze al met doen kunt de [ingebouwde analyseprogramma's](howto-create-analytics.md).

In deze handleiding leert u hoe u:

* Telemetrie van een IoT Central-toepassing met Stream *voortdurende gegevensexport*.
* Maak een Azure Databricks-omgeving te analyseren en te tekenen telemetrie van apparaten.

## <a name="prerequisites"></a>Vereisten

Voor de stappen in deze handleiding, hebt u een actief Azure-abonnement nodig.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

### <a name="iot-central-application"></a>IoT Central-toepassing

Maken van een IoT Central-toepassing van de [Azure IoT Central - mijn toepassingen](https://aka.ms/iotcentral) pagina met de volgende instellingen:

| Instelling | Value |
| ------- | ----- |
| Betalingsschema | Betalen per gebruik |
| Toepassingsjabloon | Contoso-voorbeeld |
| De naam van de toepassing | Accepteer de standaardwaarde of Kies uw eigen naam |
| URL | Accepteer de standaardwaarde of Kies uw eigen unieke URL-voorvoegsel |
| Directory | Uw Azure Active Directory-tenant |
| Azure-abonnement | Uw Azure-abonnement |
| Regio | US - oost |

De voorbeelden en schermopnamen in dit artikel gebruiken de **VS-Oost** regio. Kies een locatie dicht bij u en zorg ervoor dat u al uw resources in dezelfde regio maken.

### <a name="resource-group"></a>Resourcegroep

Gebruik de [Azure portal om een resourcegroep te maken](https://portal.azure.com/#create/Microsoft.ResourceGroup) met de naam **IoTCentralAnalysis** bevatten de andere resources die u maakt. Uw Azure-resources in dezelfde locatie als uw IoT Central-toepassing maken.

### <a name="event-hubs-namespace"></a>Event Hubs-naamruimte

Gebruik de [Azure portal om te maken van een Event Hubs-naamruimte](https://portal.azure.com/#create/Microsoft.EventHub) met de volgende instellingen:

| Instelling | Waarde |
| ------- | ----- |
| Name    | Kies de naamruimtenaam van uw |
| Prijscategorie | Basic |
| Abonnement | Uw abonnement |
| Resourcegroep | IoTCentralAnalysis |
| Locatie | US - oost |
| Doorvoereenheden | 1 |

### <a name="azure-databricks-workspace"></a>Azure Databricks-werkruimte

Gebruik de [Azure portal om te maken van een Azure Databricks Service](https://portal.azure.com/#create/Microsoft.Databricks) met de volgende instellingen:

| Instelling | Value |
| ------- | ----- |
| Naam van de werkruimte    | Kies de Werkruimtenaam van uw |
| Abonnement | Uw abonnement |
| Resourcegroep | IoTCentralAnalysis |
| Locatie | US - oost |
| Prijscategorie | Standard |

Tijdens het maken van de vereiste resources, uw **IoTCentralAnalysis** resourcegroep ziet eruit als in de volgende schermafbeelding:

![IoT Central analysis-resourcegroep](media/howto-create-custom-analytics/resource-group.png)

## <a name="create-an-event-hub"></a>Een Event Hub maken

U kunt een toepassing IoT Central telemetrie continu exporteren naar een event hub configureren. In deze sectie maakt u een event hub voor het ontvangen van telemetrie van uw IoT Central-toepassing. De event hub biedt de telemetrie naar uw Stream Analytics-taak voor verwerking.

1. In de Azure-portal, gaat u naar uw Event Hubs-naamruimte en selecteer **+ Event Hub**.
1. Naam van uw event hub **centralexport**, en selecteer **maken**.
1. Selecteer in de lijst met eventhubs in uw naamruimte, **centralexport**. Kies vervolgens **beleid voor gedeelde toegang**.
1. Selecteer **+ Toevoegen**. Maak een beleid met de naam **luisteren** met de **luisteren** claim.
1. Wanneer het beleid klaar is, selecteert u deze in de lijst en kopieer vervolgens de **verbindingsreeks-primaire sleutel** waarde.
1. Maak een notitie van deze verbindingsreeks, u deze later gebruiken bij het configureren van uw Databricks-notebook om uit de event hub te lezen.

Uw Event Hubs-naamruimte ziet eruit als in de volgende schermafbeelding:

![Event Hubs-naamruimte](media/howto-create-custom-analytics/event-hubs-namespace.png)

## <a name="configure-export-in-iot-central"></a>Configureren van uitvoer in IoT Central

Navigeer naar de [IoT Central-toepassing](https://aka.ms/iotcentral) u gemaakt op basis van de sjabloon Contoso. In deze sectie configureert u de toepassing om de telemetrie van de gesimuleerde apparaten naar uw event hub streamen. De export configureren:

1. Navigeer naar de **continue gegevensexport** weergeeft, schakelt **+ nieuw**, en vervolgens **Azure Event Hubs**.
1. Gebruik de volgende instellingen naar de export configureren en selecteer vervolgens **opslaan**:

    | Instelling | Value |
    | ------- | ----- |
    | Weergavenaam | Exporteren naar Eventhubs |
    | Enabled | Aan |
    | Event Hubs-naamruimte | De naam van uw Event Hubs-naamruimte |
    | Event Hub | centralexport |
    | Metingen | Aan |
    | Apparaten | Uit |
    | Apparaatsjablonen | Uit |

![Configuratie voor continue export](media/howto-create-custom-analytics/cde-configuration.png)

Wacht totdat de status van de export **met** voordat u doorgaat.

## <a name="configure-databricks-workspace"></a>Databricks-werkruimte configureren

In de Azure-portal, gaat u naar uw Azure Databricks-service en selecteer **werkruimte starten**. Een nieuw tabblad geopend in uw browser en u zich aanmeldt bij uw werkruimte.

### <a name="create-a-cluster"></a>Een cluster maken

Op de **Azure Databricks** pagina, onder de lijst met algemene taken, selecteer **nieuw Cluster**.

Gebruik de informatie in de volgende tabel om uw cluster te maken:

| Instelling | Waarde |
| ------- | ----- |
| Clusternaam | centralanalysis |
| De clustermodus met | Standard |
| Databricks Runtime-versie | 5.3 (Scala 2.11, Spark 2.4.0) |
| Python-versie | 3 |
| Automatisch schalen inschakelen | Nee |
| Beëindigen na aantal minuten van inactiviteit | 30 |
| Werknemertype | Standard_DS3_v2 |
| Werknemers | 1 |
| Stuurprogrammatype | Zelfde als medewerker |

Het maken van een cluster kan enkele minuten duren, wacht tot het maken van het cluster om te voltooien voordat u doorgaat.

### <a name="install-libraries"></a>Bibliotheken installeren

Op de **Clusters** pagina, wacht totdat de clusterstatus van het is **met**.

De volgende stappen laten zien hoe u de bibliotheek uw voorbeeld moet importeren in het cluster:

1. Op de **Clusters** pagina, wachten totdat de status van de **centralanalysis** interactieve cluster **met**.

1. Selecteer het cluster en kies vervolgens de **bibliotheken** tabblad.

1. Op de **bibliotheken** tabblad **installeren nieuwe**.

1. Op de **bibliotheek installeren** pagina, kies **Maven** als de bron van de bibliotheek.

1. In de **coördineert** tekstvak, voert u de volgende waarde: `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`

1. Kies **installeren** de bibliotheek te installeren op het cluster.

1. De status van de tapewisselaar is nu **geïnstalleerde**:

    ![Bibliotheek geïnstalleerd](media/howto-create-custom-analytics/cluster-libraries.png)

### <a name="import-a-databricks-notebook"></a>Een Databricks-notebook importeren

Gebruik de volgende stappen voor het importeren van een Databricks-notebook waarop de Python-code voor het analyseren en visualiseren van uw telemetrie IoT Central:

1. Navigeer naar de **werkruimte** pagina in uw Databricks-omgeving. Selecteer de vervolgkeuzelijst naast de accountnaam van uw en kies vervolgens **importeren**.

1. Kies voor het importeren van een URL en voer het volgende adres: [https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true](https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true)

1. Kies voor het importeren van de notebook **importeren**.

1. Selecteer de **werkruimte** om de geïmporteerde notebook weer te geven:

    ![Geïmporteerde notebook](media/howto-create-custom-analytics/import-notebook.png)

1. De code in de eerste Python-cel om toe te voegen van de Event Hubs-verbindingsreeks die u eerder hebt opgeslagen bewerken:

    ```python
    from pyspark.sql.functions import *
    from pyspark.sql.types import *

    ###### Event Hub Connection strings ######
    telementryEventHubConfig = {
      'eventhubs.connectionString' : '{your Event Hubs connection string}'
    }
    ```

## <a name="run-analysis"></a>Analyse uitvoeren

Als u wilt de analyse uitvoeren, moet u de notebook koppelen aan het cluster:

1. Selecteer **Detached** en selecteer vervolgens de **centralanalysis** cluster.
1. Als het cluster niet wordt uitgevoerd, start u deze.
1. Selecteer de knop uitvoeren voor het starten van de notebook.

Mogelijk ziet u een fout in de laatste cel. Als dit het geval is, controleert u dat de vorige cellen worden uitgevoerd, wacht u even voor sommige gegevens worden geschreven naar de opslag en voer de laatste cel opnieuw uit.

### <a name="view-smoothed-data"></a>Vloeiende gegevens weergeven

In het notitieblok, schuif naar cel 14 om te zien van een diagram van de rolling gemiddelde vochtigheid per apparaattype. In dit diagram wordt continu bijgewerkt als dit streaming telemetrie komt:

![Vloeiend telemetrie-diagram](media/howto-create-custom-analytics/telemetry-plot.png)

U kunt het formaat van de grafiek in het notitieblok.

### <a name="view-box-plots"></a>Weergave grafieken

Schuif omlaag naar cel 20 om te zien in het notitieblok, de [grafieken vak](https://en.wikipedia.org/wiki/Box_plot). De box-grafieken zijn gebaseerd op statische gegevens, zodat u ze updaten moet u de cel opnieuw uitvoeren:

![Vak grafieken](media/howto-create-custom-analytics/box-plots.png)

U kunt het formaat van de grafieken in het notitieblok.

## <a name="tidy-up"></a>Opruimen

Als u wilt opruimen na deze instructies en onnodige kosten voorkomen, verwijdert de **IoTCentralAnalysis** resourcegroep in Azure portal.

U kunt de IoT Central-toepassing uit verwijderen de **Management** pagina in de toepassing.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u:

* Telemetrie van een IoT Central-toepassing met Stream *voortdurende gegevensexport*.
* Maak een Azure Databricks-omgeving te analyseren en telemetrische gegevens tekenen.

Nu u weet hoe u aangepaste analytics maakt, de voorgestelde volgende stap is om te leren hoe u [visualiseren en analyseren van uw Azure IoT Central-gegevens in Power BI-dashboard](howto-connect-powerbi.md).
