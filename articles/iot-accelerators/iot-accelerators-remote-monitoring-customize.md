---
title: Aanpassen van de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel bevat informatie over hoe u toegang tot de broncode voor de Remote Monitoring solution accelerator gebruikersinterface en een aantal aanpassingen doen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/09/2018
ms.topic: conceptual
ms.openlocfilehash: dc2b38f8e8065b8d8763365bf0cbad56ae00cd4b
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55565425"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>De oplossingsverbetering voor externe bewaking aanpassen

In dit artikel bevat informatie over hoe u toegang tot de broncode en aanpassen van de Remote Monitoring solution accelerator gebruikersinterface.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Een lokale ontwikkelomgeving voorbereiden in de gebruikersinterface

De Remote Monitoring solution accelerator UI-code is geïmplementeerd met behulp van de React.js-framework. U vindt de broncode in de [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) GitHub-opslagplaats.

Als u wilt wijzigingen aanbrengt in de gebruikersinterface, kunt u een kopie van het lokaal uitvoeren. Voor het voltooien van acties zoals het ophalen van telemetrie, is de lokale kopie maakt verbinding met een geïmplementeerd exemplaar van de oplossing.

De volgende stappen beschrijven het proces voor het instellen van een lokale omgeving voor het ontwikkelen van de gebruikersinterface:

1. Implementeer een **basic** exemplaar van de solution accelerator met behulp van de **pc's** CLI. Noteer de naam van uw implementatie en de referenties die u hebt opgegeven voor de virtuele machine. Zie voor meer informatie, [implementeren met behulp van de CLI](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Om in te schakelen SSH-toegang tot de virtuele machine die als host fungeert voor de microservices in uw oplossing, de Azure-portal of de Azure Cloud Shell te gebruiken. Bijvoorbeeld:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Alleen SSH-toegang inschakelen tijdens het testen en ontwikkeling. Als u SSH schakelt, [moet u dit uitschakelen als u klaar bent met het](../security/azure-security-network-security-best-practices.md#disable-rdpssh-access-to-virtual-machines).

1. De Azure-portal of de Azure Cloud Shell gebruiken om te zoeken naar de naam en het openbare IP-adres van uw virtuele machine. Bijvoorbeeld:

    ```azurecli-interactive
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Gebruik SSH om verbinding met uw virtuele machine te maken. Het IP-adres uit de vorige stap en de referenties die u hebt opgegeven toen u uitvoerde **pc's** om de oplossing te implementeren. De `ssh` opdracht is beschikbaar in de Azure Cloud Shell.

1. Als u wilt toestaan dat de lokale UX om verbinding te maken, moet u de volgende opdrachten uitvoeren in de bash-shell in de virtuele machine:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Nadat u ziet de opdracht is voltooid en de website wordt gestart, kunt u de verbinding verbreken van de virtuele machine.

1. In de lokale kopie van de [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) opslagplaats, bewerken de **.env** -bestand naar de URL van uw geïmplementeerde oplossing toevoegen:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Bij een opdrachtprompt, gaat u naar de lokale kopie van de `azure-iot-pcs-remote-monitoring-webui` map.

1. Als u wilt de vereiste bibliotheken installeren en lokaal uitvoeren van de gebruikersinterface, voer de volgende opdrachten:

    ```cmd/sh
    npm install
    npm start
    ```

1. De vorige opdracht wordt uitgevoerd de Webinterface lokaal op http://localhost:3000/dashboard. U kunt de code bewerken terwijl de site wordt uitgevoerd en zien dynamisch bijwerken.

## <a name="customize-the-layout"></a>De indeling aanpassen

Elke pagina in de oplossing voor externe controle bestaat uit een set besturingselementen, aangeduid als *panelen* in de broncode. De **Dashboard** pagina bestaat uit vijf deelvensters: Overzicht, kaart, meldingen, Telemetrie en Analytics. U vindt de broncode dat elke pagina en de deelvensters in definieert de [pc's-remote-monitoring-webinterface](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub-opslagplaats. Bijvoorbeeld, de code die definieert de **Dashboard** pagina, de indeling en de panelen op de pagina bevindt zich in de [src/onderdelen/pagina's / dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) map.

Omdat de panelen hun eigen lay-out en schaling beheren, kunt u eenvoudig de indeling van een pagina wijzigen. Breng de volgende wijzigingen aan de **PageContent** -element in de `src/components/pages/dashboard/dashboard.js` van het bestand in:

* De positie van de kaart en telemetrie panelen wisselen.
* De relatieve breedte van de kaart en analyses panelen wijzigen.

```javascript
<PageContent className="dashboard-container">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        activeDeviceGroup={activeDeviceGroup}
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={analyticsIsPending || devicesIsPending}
        error={deviceGroupError || devicesError || analyticsError}
        t={t} />
    </Cell>
    <Cell className="col-6">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <AlertsPanel
        alerts={currentActiveAlertsWithName}
        isPending={analyticsIsPending || rulesIsPending}
        error={rulesError || analyticsError}
        t={t}
        deviceGroups={deviceGroups} />
    </Cell>
    <Cell className="col-4">
      <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          analyticsVersion={analyticsVersion}
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlert={devicesInAlert}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || analyticsIsPending}
          error={azureMapsKeyError || devicesError || analyticsError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <AnalyticsPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        topAlerts={topAlertsWithName}
        alertsPerDeviceId={alertsPerDeviceType}
        criticalAlertsChange={criticalAlertsChange}
        isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || analyticsError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    {
      Config.showWalkthroughExamples &&
      <Cell className="col-4">
        <ExamplePanel t={t} />
      </Cell>
    }
  </Grid>
</PageContent>
```

![Deelvenster-indeling wijzigen](./media/iot-accelerators-remote-monitoring-customize/layout.png)

U kunt ook toevoegen meerdere exemplaren van het paneel met dezelfde of verschillende versies als u [dupliceren en een deelvenster aanpassen](#duplicate-and-customize-an-existing-control). Het volgende voorbeeld ziet u hoe u twee exemplaren van het paneel telemetrie toe te voegen. Deze wijzigingen wilt aanbrengen, bewerk de `src/components/pages/dashboard/dashboard.js` bestand:

```javascript
<PageContent className="dashboard-container">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        activeDeviceGroup={activeDeviceGroup}
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={analyticsIsPending || devicesIsPending}
        error={deviceGroupError || devicesError || analyticsError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <AlertsPanel
        alerts={currentActiveAlertsWithName}
        isPending={analyticsIsPending || rulesIsPending}
        error={rulesError || analyticsError}
        t={t}
        deviceGroups={deviceGroups} />
    </Cell>
    <Cell className="col-4">
      <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          analyticsVersion={analyticsVersion}
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlert={devicesInAlert}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || analyticsIsPending}
          error={azureMapsKeyError || devicesError || analyticsError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <AnalyticsPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        topAlerts={topAlertsWithName}
        alertsPerDeviceId={alertsPerDeviceType}
        criticalAlertsChange={criticalAlertsChange}
        isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || analyticsError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    {
      Config.showWalkthroughExamples &&
      <Cell className="col-4">
        <ExamplePanel t={t} />
      </Cell>
    }
  </Grid>
</PageContent>
```

U kunt vervolgens verschillende telemetrie bekijken in elk Configuratiescherm:

![Meerdere telemetrie-panelen](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

## <a name="duplicate-and-customize-an-existing-control"></a>Dubbele en aanpassen van een bestaand besturingselement

De volgende stappen beschrijven het dupliceren van een bestaande Configuratiescherm, wijzigen en gebruik vervolgens de gewijzigde versie. De stappen wordt gebruikgemaakt van de **waarschuwingen** deelvenster als een voorbeeld:

1. In de lokale kopie van de opslagplaats, maakt u een kopie van de **waarschuwingen** map in de `src/components/pages/dashboard/panels` map. Noem de nieuwe kopie **cust_alerts**.

1. In de **alertsPanel.js** -bestand in de **cust_alerts** map, de naam van de klasse moet bewerken **CustAlertsPanel**:

    ```javascript
    export class CustAlertsPanel extends Component {
    ```

1. Voeg de volgende regel aan de `src/components/pages/dashboard/panels/index.js` bestand:

    ```javascript
    export * from './cust_alerts';
    ```

1. Vervang `alertsPanel` met `CustAlertsPanel` in de `src/components/pages/dashboard/dashboard.js` bestand:

    ```javascript
    import {
      OverviewPanel,
      CustAlertsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlertsPanel
        alerts={currentActivealertsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

U hebt nu de oorspronkelijke vervangen **waarschuwingen** Configuratiescherm met behulp van een kopie met de naam **CustAlerts**. Dit exemplaar is hetzelfde als het origineel. U kunt nu de kopie wijzigen. Bijvoorbeeld, om te wijzigen van de kolom bestellen in de **waarschuwingen** Configuratiescherm:

1. Open het `src/components/pages/dashboard/panels/cust_alerts/alertsPanel.js`-bestand.

1. Wijzig de kolomdefinities zoals wordt weergegeven in het volgende codefragment:

    ```javascript
    this.columnDefs = [
      rulesColumnDefs.severity,
      {
        headerName: 'rules.grid.count',
        field: 'count'
      },
      {
        ...rulesColumnDefs.ruleName,
        minWidth: 200
      },
      rulesColumnDefs.explore
    ];
    ```

De volgende schermafbeelding ziet u de nieuwe versie van de **waarschuwingen** Configuratiescherm:

![paneel waarschuwingen is bijgewerkt](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>De telemetrie-grafiek aanpassen

De bestanden in de `src/components/pages/dashboard/panels/telemtry` map definiëren de grafiek telemetrie op de **Dashboard** pagina. De gebruikersinterface van de telemetrie opgehaald uit de back-end oplossing in de `src/services/telemetryService.js` bestand. De volgende stappen laten zien hoe u de geselecteerde periode weergegeven in de grafiek telemetrie van 15 tot 5 minuten wijzigen:

1. In de `src/services/telemetryService.js` bestand, zoek de aangeroepen functie **getTelemetryByDeviceIdP15M**. Maak een kopie van deze functie en de kopie als volgt wijzigen:

    ```javascript
    static getTelemetryByDeviceIdP5M(devices = []) {
      return TelemetryService.getTelemetryByMessages({
        from: 'NOW-PT5M',
        to: 'NOW',
        order: 'desc',
        devices
      });
    }
    ```

1. Voor het gebruik van deze nieuwe functie voor het vullen van de grafiek telemetrie, opent u de `src/components/pages/dashboard/dashboard.js` bestand. Zoek de regel die de telemetriestroom geïnitialiseerd en als volgt wijzigen:

    ```javascript
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

De telemetrie-grafiek toont de vijf minuten van telemetrische gegevens:

![Telemetrie-grafiek met van één dag](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Een nieuwe KPI toevoegen

De **Dashboard** pagina worden weergegeven voor KPI's in de **Analytics** deelvenster. Deze KPI's worden berekend in de `src/components/pages/dashboard/dashboard.js` bestand. De KPI's worden weergegeven door de `src/components/pages/dashboard/panels/analytics/analyticsPanel.js` bestand. De volgende stappen wordt beschreven hoe u om te berekenen en een nieuwe KPI-waarde weergegeven op de **Dashboard** pagina. Het voorbeeld is het toevoegen van een nieuwe Wijzigingspercentage in waarschuwingsmeldingen KPI:

1. Open het `src/components/pages/dashboard/dashboard.js`-bestand. Wijzig de **initialState** object om op te nemen een **warningAlertsChange** eigenschap als volgt te werk:

    ```javascript
    const initialState = {
      ...

      // Analytics data
      analyticsVersion: 0,
      currentActiveAlerts: [],
      topAlerts: [],
      alertsPerDeviceId: {},
      criticalAlertsChange: 0,
      warningAlertsChange: 0,
      analyticsIsPending: true,
      analyticsError: null

      ...
    };
    ```

1. Wijzig de **currentAlertsStats** object om op te nemen **totalWarningCount** als een eigenschap:

    ```javascript
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alertsPerDeviceId: updatedAlertsPerDeviceId
    };
    ```

1. De nieuwe KPI berekenen. Zoek de berekening van het aantal kritieke waarschuwingen. De code dupliceren en de kopie als volgt wijzigen:

    ```javascript
    // ================== Warning Alerts Count - START
    const currentWarningAlerts = currentAlertsStats.totalWarningCount;
    const previousWarningAlerts = previousAlerts.reduce(
      (cnt, { severity }) => severity === Config.ruleSeverity.warning ? cnt + 1 : cnt,
      0
    );
    const warningAlertsChange = ((currentWarningAlerts - previousWarningAlerts) / currentWarningAlerts * 100).toFixed(2);
    // ================== Warning Alerts Count - END
    ```

1. Zodat de nieuwe **warningAlertsChange** KPI in de KPI-stroom:

    ```javascript
    return ({
      analyticsIsPending: false,
      analyticsVersion: this.state.analyticsVersion + 1,

      // Analytics data
      currentActiveAlerts,
      topAlerts,
      criticalAlertsChange,
      warningAlertsChange,
      alertsPerDeviceId: currentAlertsStats.alertsPerDeviceId,

      ...
    });
    ```

1. Zodat de nieuwe **warningAlertsChange** KPI in de van statusgegevens die worden gebruikt om de gebruikersinterface weer te geven:

    ```javascript
    const {
      ...

      analyticsVersion,
      currentActiveAlerts,
      topAlerts,
      alertsPerDeviceId,
      criticalAlertsChange,
      warningAlertsChange,
      analyticsIsPending,
      analyticsError,

      ...
    } = this.state;
    ```

1. Update voor de gegevens aan het deelvenster KPI's doorgegeven:

    ```javascript
    <AnalyticsPanel
      timeSeriesExplorerUrl={timeSeriesParamUrl}
      topAlerts={topAlertsWithName}
      alertsPerDeviceId={alertsPerDeviceType}
      criticalAlertsChange={criticalAlertsChange}
      warningAlertsChange={warningAlertsChange}
      isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || analyticsError}
      theme={theme}
      colors={chartColorObjects}
      t={t} />
    ```

U hebt nu de wijzigingen in de `src/components/pages/dashboard/dashboard.js` bestand. De volgende stappen beschrijven de wijzigingen aanbrengen in de `src/components/pages/dashboard/panels/analytics/analyticsPanel.js` bestand om de nieuwe KPI weer te geven:

1. Wijzig de volgende regel code voor het ophalen van de nieuwe KPI-waarde als volgt:

    ```javascript
    const { t, isPending, criticalAlertsChange, warningAlertsChange, alertsPerDeviceId, topAlerts, timeSeriesExplorerUrl, error } = this.props;
    ```

1. Wijzig de markering om de nieuwe KPI-waarde als volgt weer te geven:

    ```javascript
    <div className="analytics-cell">
      <div className="analytics-header">{t('dashboard.panels.analytics.criticalAlerts')}</div>
      <div className="critical-alerts">
        {
          !showOverlay &&
            <div className="analytics-percentage-container">
              <div className="analytics-value">{ !isNaN(criticalAlertsChange) ? criticalAlertsChange : 0 }</div>
              <div className="analytics-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="critical-alerts">
        {
          !showOverlay &&
            <div className="analytics-percentage-container">
              <div className="analytics-value">{ !isNaN(warningAlertsChange) ? warningAlertsChange : 0 }</div>
              <div className="analytics-percentage-sign">%</div>
            </div>
        }
      </div>
    </div>
    ```

De **Dashboard** pagina wordt nu weergegeven voor de nieuwe KPI-waarde:

![Waarschuwing KPI](./media/iot-accelerators-remote-monitoring-customize/new-kpi.png)

## <a name="customize-the-map"></a>De kaart aanpassen

Zie de [kaart aanpassen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) pagina in GitHub voor meer informatie over de onderdelen van de kaart in de oplossing.

<!--
### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

-->

## <a name="other-customization-options"></a>Andere opties voor het aanpassen

Als u wilt de visualisaties en presentatie-laag in de oplossing voor externe controle verder aanpassen, kunt u de code bewerken. De relevante GitHub-opslagplaatsen zijn:

* [De configuratie van microservice voor Azure IoT Solutions (.NET)](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config)
* [De configuratie van microservice voor Azure IoT-oplossingen (Java)](https://github.com/Azure/remote-monitoring-services-java/tree/master/config)
* [Azure IoT-pc's-externe controle van Web-UI](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar voor het aanpassen van de web-UI in de oplossingsverbetering voor externe controle. Zie voor meer informatie over het aanpassen van de gebruikersinterface van de volgende artikelen:

* [Een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-page.md)
* [Een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-service.md)
* [Een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md)
* [Een aangepaste flyout toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-flyout.md)
* [Een aangepaste paneel toevoegen aan het dashboard in de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-panel.md)

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle microservices [aanpassen en opnieuw implementeren van een microservice](iot-accelerators-microservices-example.md).
<!-- Next tutorials in the sequence -->
