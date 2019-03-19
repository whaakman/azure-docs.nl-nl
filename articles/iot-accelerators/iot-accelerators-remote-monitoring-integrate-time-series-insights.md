---
title: Time Series Insights integreren met externe controle - Azure | Microsoft Docs
description: In deze procedure leert u over het configureren van Time Series Insights voor een bestaande oplossing voor externe controle die nog geen Time Series Insights.
author: aditidugar
manager: timlt
ms.author: adugar
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 850d8bbb525763e0e7d0c0441173180b7c469dd8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58085147"
---
# <a name="integrate-azure-time-series-insights-with-remote-monitoring"></a>Integreer Azure Time Series Insights met Externe bewaking

Azure Time Series Insights is een volledig beheerde analyse-, opslag- en visualisatieservice waarmee u tijdseriegegevens op IoT-schaal kunt beheren in de cloud. U kunt de Time Series Insights gebruiken opslaan en beheren van tijdseriegegevens, verkennen en visualiseren gebeurtenissen tegelijkertijd, uitvoeren van de hoofdoorzaak wordt onderzocht, en om meerdere sites en assets te vergelijken.

De oplossingsverbetering voor externe controle biedt nu de automatische implementatie en integratie met Time Series Insights. In deze procedures leert u hoe het configureren van Time Series Insights voor een bestaande oplossing voor externe controle die nog geen Time Series Insights.

> [!NOTE]
> Time Series Insights is momenteel niet beschikbaar in de Azure China-cloud. Nieuwe Remote Monitoring solution accelerator implementaties in de cloud met Azure China Cosmos DB gebruiken voor alle opslag.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze procedures hebt voltooid, moet u een oplossing voor externe controle al hebt geïmplementeerd:

* [De oplossingsverbetering voor externe bewaking implementeren](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Een consumentengroep maken

Maak een speciale klantengroep in uw IoT-Hub moet worden gebruikt voor het streamen van gegevens naar de Time Series Insights.

> [!NOTE]
> Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens van Azure IoT Hub. Elke consumentengroep kan maximaal vijf uitvoer consumenten. Voor elke vijf sinks uitvoer en kunt u maximaal 32 consumergroepen maken, moet u een nieuwe consumergroep maken.

1. Klik op de knop Cloud Shell in de Azure-portal.

1. Voer de volgende opdracht om een nieuwe consumergroep maakt. De naam van de IoT-hub in uw implementatie voor externe controle en de naam van uw implementatie van externe controle gebruiken als de naam van de resourcegroep:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="deploy-time-series-insights"></a>Time Series Insights implementeren

Vervolgens Time Series Insights implementeren als een extra bron in uw oplossing voor externe controle en deze verbinden met de IoT-hub.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer **een resource maken** > **Internet of Things** > **Time Series Insights**.

    ![Nieuwe Time Series Insights](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights.png)

1. Voor het maken van uw Time Series Insights-omgeving gebruikt u de waarden in de volgende tabel:

    | Instelling | Value |
    | ------- | ----- |
    | Naam van de omgeving | De volgende schermafbeelding wordt gebruikt voor de naam van de **contorosrmtsi**. Kies uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Abonnement | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resourcegroep | **Gebruik bestaande**. Selecteer de naam van uw bestaande resourcegroep voor externe controle. |
    | Locatie | We gebruiken **VS-Oost**. Indien mogelijk de omgeving maken in dezelfde regio als uw oplossing voor externe controle. |
    | Sku |**S1** |
    | Capaciteit | **1** |

    ![Time Series Insights maken](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights-create.png)

1. Klik op **Create**. Duurt het even de tijd voor de omgeving moet worden gemaakt.

## <a name="create-event-source"></a>Gebeurtenisbron maken

Maak een nieuwe gebeurtenisbron verbinding maken met uw IoT-hub. Zorg ervoor dat u de consumergroep in de vorige stappen hebt gemaakt. Time Series Insights is vereist voor elke service hebben een speciale klantengroep niet wordt gebruikt door een andere service.

1. Navigeer naar uw nieuwe Time Series Insights-omgeving.

1. Selecteer aan de linkerkant **gebeurtenisbronnen**.

    ![Bronnen van gebeurtenissen weergeven](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources.png)

1. Klik op **Add**.

    ![De bron van gebeurtenis toevoegen](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources-add.png)

1. Voor het configureren van uw IoT-hub als een nieuwe gebeurtenisbron, gebruikt u de waarden in de volgende tabel:

    | Instelling | Value |
    | ------- | ----- |
    | Naam van gebeurtenisbron | De volgende schermafbeelding wordt gebruikt voor de naam van de **contosorm-iot-hub**. Gebruik uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Bron | **IoT Hub** |
    | Importoptie | **IoT-Hub uit de beschikbare abonnementen gebruiken** |
    | Abonnements-id | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Naam van IoT Hub | **contosorma57a6**. Gebruik de naam van uw IoT-hub in uw oplossing voor externe controle. |
    | Naam van het IoT Hub-beleid | **iothubowner** Zorg ervoor dat het beleid op waarmee een beleid voor de eigenaar van is. |
    | Sleutel voor het IoT Hub-beleid | Dit veld wordt automatisch gevuld. |
    | Consumentengroep voor IoT Hub | **timeseriesinsights** |
    | Serialisatie-indeling voor gebeurtenissen | **JSON**     | 
    | Naam van de timestamp-eigenschap | Leeg laten |

    ![Gebeurtenisbron maken](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-source-create.png)

1. Klik op **Create**.

## <a name="configure-the-data-access-policy"></a>Het toegangsbeleid van gegevens configureren

Om te controleren of kunnen alle gebruikers die toegang tot uw oplossing voor externe controle hebben gegevens in de Verkenner van Time Series Insights verkennen, voegt u uw toepassing en gebruikers onder beleid voor gegevenstoegang in Azure portal. 

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies de **ContosoRM** resourcegroep.

1. Kies **contosormtsi** in de lijst met Azure-resources.

1. Kies **beleid voor gegevenstoegang** om te zien van de huidige lijst van roltoewijzingen.

1. Kies **toevoegen** openen de **Selecteer gebruiker regel** deelvenster.

   Als u geen machtigingen voor het toewijzen van rollen, niet ziet u de **toevoegen** optie.

1. In de **rol** vervolgkeuzelijst, selecteer een rol, zoals **lezer** en **Inzender**.

1. Selecteer in de lijst **Selecteren** een gebruiker, groep of toepassing. Als u de beveiligings-principal niet in de lijst ziet staan, kunt u tekst typen in het vak **Selecteren** om te zoeken naar weergavenamen, e-mailadressen en object-id's.

1. Kies **Opslaan** om de roltoewijzing te maken. Na enkele ogenblikken is de beveiligings-principal de rol in het beleid voor gegevenstoegang toegewezen.

> [!NOTE]
> Als u moet extra gebruikers toegang verlenen tot de Verkenner van Time Series Insights, kunt u deze stappen voor het [gegevenstoegang verlenen](../time-series-insights/time-series-insights-data-access.md#grant-data-access).

## <a name="configure-azure-stream-analytics"></a>Azure Stream Analytics configureren 

De volgende stap is het configureren van de Manager van Azure Stream Analytics-microservice als u wilt stoppen met het verzenden van berichten naar Cosmos DB en op te slaan in Time Series Insights. Deze stap overslaan als u wilt dupliceren van uw berichten in Cosmos DB.

1. Kies **Resourcegroepen** in de navigatielijst.

1. Kies de **ContosoRM** resourcegroep.

1. De Azure Stream Analytics (ASA) streaming-taak in de lijst met resources vinden. Naam van de resource wordt gestart met **streamingjobs -**.

1. Klik op de knop als u wilt stoppen van de ASA streamingtaken aan de bovenkant.

1. De ASA-query bewerkt en verwijdert u de **Selecteer**, **INTO**, en **FROM** van de EU die naar de berichten verwijzen streamen in Cosmos DB. Deze componenten moeten aan de onderkant van de query en moet eruitzien als in het volgende voorbeeld:

    ```sql
    SELECT
            CONCAT(T.IoTHub.ConnectionDeviceId, ';', CAST(DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) AS nvarchar(max))) as id,
            1 as [doc.schemaVersion],
            'd2cmessage' as [doc.schema],
            T.IoTHub.ConnectionDeviceId as [device.id],
            'device-sensors;v1' as [device.msg.schema],
            'StreamingJobs' as [data.schema],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', System.Timestamp) as [device.msg.created],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) as [device.msg.received],
            udf.removeUnusedProperties(T) as Data
    INTO
        Messages
    FROM
        DeviceTelemetry T PARTITION BY PartitionId TIMESTAMP BY T.EventEnqueuedUtcTime
    ```

6. Start opnieuw op de Azure Stream Analytics streaming-taken.

7. Haal de meest recente wijzigingen aan de Azure Stream Analytics manager microservice door de volgende opdracht te typen bij de opdrachtprompt:

.NET: 

```cmd/sh
docker pull azureiotpcs/asa-manager-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/asa-manager-java:1.0.2
```

## <a name="configure-the-telemetry-microservice"></a>De telemetrie-microservice configureren

Haal de meest recente telemetrie microservice door de volgende opdracht te typen bij de opdrachtprompt:

.NET:

```cmd/sh
docker pull azureiotpcs/telemetry-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/telemetry-java:1.0.2
```

## <a name="optional-configure-the-web-ui-to-link-to-the-time-series-insights-explorer"></a>*[Optioneel]*  Configureren van de web-UI te koppelen aan de Verkenner van Time Series Insights

Als u wilt uw gegevens eenvoudig bekijken in de Verkenner van Time Series Insights, wordt u aangeraden de gebruikersinterface eenvoudig koppelen aan de omgeving aanpassen. Haal de meest recente wijzigingen in de Web-UI met behulp van de volgende opdracht uit om dit te doen:

```cmd/sh
docker pull azureiotpcs/pcs-remote-monitoring-webui:1.0.2
```

## <a name="configure-the-environment-variables"></a>De omgevingsvariabelen configureren

Voor het voltooien van de Time Series Insights-integratie, moet u de omgeving van uw implementatie voor de bijgewerkte microservices configureren.

### <a name="basic-deployments"></a>Basic-implementaties

Configureren van de omgeving van `basic` implementatie voor de bijgewerkte microservices.

1. Klik in de Azure-portal op de **Azure Active Directory** tabblad in het linkerdeelvenster.

1. Klik op **App-registraties**.

1. Zoek en klik op uw **ContosoRM** toepassing.

1. Navigeer naar **instellingen** > **sleutels** en maak vervolgens een nieuwe sleutel voor uw toepassing. Zorg ervoor dat u de waarde van de sleutel kopiëren naar veilige locatie.

1. Haal de [meest recente docker compose yaml-bestand](https://github.com/Azure/pcs-cli/tree/5a9b4e0dbe313172eff19236e54a4d461d4f3e51/solutions/remotemonitoring/single-vm) van GitHub-opslagplaats met behulp van de meest recente code. 

1. Voeg SSH toe aan de virtuele machine met de volgende stappen die worden beschreven op [maken en gebruiken van SSH-sleutels](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

1. Eenmaal verbinding hebben, typ `cd /app`.

1. Voeg de volgende omgevingsvariabelen op elke microservice in de docker compose yaml-bestand en de `env-setup` script in de virtuele machine:

    ```sh
    PCS_TELEMETRY_STORAGE_TYPE=tsi
    PCS_TSI_FQDN={TSI Data Access FQDN}
    PCS_AAD_TENANT={AAD Tenant Id}
    PCS_AAD_APPID={AAD application Id}
    PCS_AAD_APPSECRET={AAD application key}
    ```

1. Navigeer naar de **telemetrieservice** en ook bewerken de docker bestand samenstellen door de bovenstaande dezelfde omgevingsvariabelen toe te voegen.

1. Navigeer naar de **ASA manager-service** en bewerken van de docker compose bestand door toe te voegen `PCS_TELEMETRY_STORAGE_TYPE`.

1. Opnieuw opstarten van de docker-containers met behulp van `sudo ./start.sh` van de virtuele machine.

### <a name="standard-deployments"></a>Standard-implementaties

Configureren van de omgeving van `standard` implementatie voor de bovenstaande bijgewerkte microservices

1. Voer op de opdrachtregel `kubectl proxy`. Zie voor meer informatie, [toegang tot de Kubernetes API](https://kubernetes.io/docs/tasks/access-kubernetes-api/http-proxy-access-api/#using-kubectl-to-start-a-proxy-server).

1. Open de beheerconsole van Kubernetes.

1. De configuratie-kaart toe te voegen van de volgende nieuwe omgevingsvariabelen voor TSI zoeken:

    ```yaml
    telemetry.storage.type: "tsi"
    telemetry.tsi.fqdn: "{TSI Data Access FQDN}"
    security.auth.serviceprincipal.secret: "{AAD application service principal secret}"
    ```

4. De sjabloon yaml-bestand voor telemetrie service pod bewerken:

    ```yaml
    - name: PCS_AAD_TENANT
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.tenant
    - name: PCS_AAD_APPID
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.audience
    - name: PCS_AAD_APPSECRET
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.serviceprincipal.secret
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    - name: PCS_TSI_FQDN
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.tsi.fqdn
    ```

5. De sjabloon yaml-bestand voor ASA manager service pod bewerken:

    ```yaml
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    ```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over hoe u uw gegevens verkennen en analyseren van een waarschuwing in de Verkenner van Time Series Insights, onze zelfstudie op [analyse uitvoeren van een basis van de hoofdoorzaak](iot-accelerators-remote-monitoring-root-cause-analysis.md).

* Meer informatie over het verkennen en het opvragen van gegevens in de Verkenner van Time Series Insights, Zie de documentatie over de [Azure Time Series Insights explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
