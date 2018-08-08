---
title: Aanpassen van de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel bevat informatie over hoe u toegang tot de broncode voor de Remote Monitoring solution accelerator gebruikersinterface en een aantal aanpassingen doen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2018
ms.topic: conceptual
ms.openlocfilehash: 9700a76284e2ee2a652ae0dbcbaa2885ab515f79
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596552"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>De oplossingsverbetering voor externe bewaking aanpassen

In dit artikel bevat informatie over hoe u toegang tot de broncode en aanpassen van de Remote Monitoring solution accelerator gebruikersinterface. Dit artikel wordt beschreven:

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Een lokale ontwikkelomgeving voorbereiden in de gebruikersinterface

De Remote Monitoring solution accelerator UI-code is geïmplementeerd met behulp van de React.js-framework. U vindt de broncode in de [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) GitHub-opslagplaats.

Als u wilt wijzigingen aanbrengt in de gebruikersinterface, kunt u een kopie van het lokaal uitvoeren. De lokale kopie maakt verbinding met een geïmplementeerd exemplaar van de oplossing voor het uitvoeren van acties zoals het ophalen van telemetrie.

De volgende stappen beschrijven het proces voor het instellen van een lokale omgeving voor het ontwikkelen van de gebruikersinterface:

1. Implementeer een **basic** exemplaar van de solution accelerator met behulp van de **pc's** CLI. Noteer de naam van uw implementatie en de referenties die u hebt opgegeven voor de virtuele machine. Zie voor meer informatie, [implementeren met behulp van de CLI](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Gebruik de Azure-portal of de [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor SSH-toegang tot de virtuele machine die als host fungeert voor de microservices in uw oplossing. Bijvoorbeeld:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    U moet alleen SSH-toegang inschakelen tijdens het testen en ontwikkeling. Als u SSH schakelt, [moet u dit opnieuw zo snel mogelijk uitschakelen](../security/azure-security-network-security-best-practices.md#disable-rdpssh-access-to-azure-virtual-machines).

1. Gebruik de Azure-portal of de [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) om de naam en openbare IP-adres van uw virtuele machine te vinden. Bijvoorbeeld:

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. SSH gebruiken om te verbinden met uw virtuele machine met behulp van het IP-adres uit de vorige stap en de referenties die u hebt opgegeven toen u uitvoerde **pc's** om de oplossing te implementeren.

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

1. Bij een opdrachtprompt in de lokale kopie van de `azure-iot-pcs-remote-monitoring-webui` map, voer de volgende opdrachten op de vereiste bibliotheken installeren en lokaal uitvoeren van de gebruikersinterface:

    ```cmd/sh
    npm install
    npm start
    ```

1. De vorige opdracht wordt uitgevoerd de Webinterface lokaal op http://localhost:3000/dashboard. U kunt de code bewerken terwijl de site wordt uitgevoerd en zien dynamisch bijwerken.

## <a name="customize-the-layout"></a>De indeling aanpassen

Elke pagina in de oplossing voor externe controle bestaat uit een set besturingselementen, aangeduid als *panelen* in de broncode. Bijvoorbeeld, de **Dashboard** pagina bestaat uit vijf deelvensters: overzicht, kaart, waarschuwingen, Telemetrie en KPI's. U vindt de broncode dat elke pagina en de deelvensters in definieert de [pc's-remote-monitoring-webinterface](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub-opslagplaats. Bijvoorbeeld, de code die definieert de **Dashboard** pagina, de indeling en de panelen op de pagina bevindt zich in de [src/onderdelen/pagina's / dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) map.

Omdat de panelen hun eigen lay-out en schaling beheren, kunt u eenvoudig de indeling van een pagina wijzigen. Bijvoorbeeld, de volgende wijzigingen aan de **PageContent** -element in de `src/components/pages/dashboard/dashboard.js` bestand wisselen de posities van de kaart en telemetrie panelen en wijzigt u de relatieve breedte van de kaart en KPI-deelvensters:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-5">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

![Deelvenster-indeling wijzigen](./media/iot-accelerators-remote-monitoring-customize/layout.png)

> [!NOTE]
> De kaart is niet geconfigureerd in de lokale implementatie.

U kunt ook toevoegen meerdere exemplaren van het paneel met dezelfde of verschillende versies als u [dupliceren en een deelvenster aanpassen](#duplicate-and-customize-an-existing-control). Het volgende voorbeeld laat zien hoe u twee exemplaren van het paneel telemetrie toevoegen door te bewerken de `src/components/pages/dashboard/dashboard.js` bestand:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-2">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

U kunt vervolgens verschillende telemetrie bekijken in elk Configuratiescherm:

![Meerdere telemetrie-panelen](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

> [!NOTE]
> De kaart is niet geconfigureerd in de lokale implementatie.

## <a name="duplicate-and-customize-an-existing-control"></a>Dubbele en aanpassen van een bestaand besturingselement

De volgende stappen wordt beschreven hoe u de **alarmen** deelvenster als een voorbeeld van het dupliceren van een bestaande Configuratiescherm, wijzigen en de aangepaste versie gebruiken:

1. In de lokale kopie van de opslagplaats, maakt u een kopie van de **alarmen** map in de `src/components/pages/dashboard/panels` map. Noem de nieuwe kopie **cust_alarms**.

1. In de **alarmsPanel.js** -bestand in de **cust_alarms** map, de naam van de klasse moet bewerken **CustAlarmsPanel**:

    ```nodejs
    export class CustAlarmsPanel extends Component {
    ```

1. Voeg de volgende regel aan de `src/components/pages/dashboard/panels/index.js` bestand:

    ```nodejs
    export * from './cust_alarms';
    ```

1. Vervang `AlarmsPanel` met `CustAlarmsPanel` in de `src/components/pages/dashboard/dashboard.js` bestand:

    ```nodejs
    import {
      OverviewPanel,
      CustAlarmsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

U hebt nu de oorspronkelijke vervangen **alarmen** Configuratiescherm met behulp van een kopie met de naam **CustAlarms**. Dit exemplaar is identiek aan de oorspronkelijke. U kunt nu de kopie wijzigen. Bijvoorbeeld, om te wijzigen van de kolom bestellen in de **alarmen** Configuratiescherm:

1. Open het `src/components/pages/dashboard/panels/cust_alarms/alarmsPanel.js`-bestand.

1. Wijzig de kolomdefinities zoals wordt weergegeven in het volgende codefragment:

    ```nodejs
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

De volgende schermafbeelding ziet u de nieuwe versie van de **alarmen** Configuratiescherm:

![Paneel bijgewerkt](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>De telemetrie-grafiek aanpassen

De grafiek telemetrie op de **Dashboard** pagina wordt gedefinieerd door de bestanden in de `src/components/pages/dashboard/panels/telemtry` map. De gebruikersinterface van de telemetrie opgehaald uit de back-end oplossing in de `src/services/telemetryService.js` bestand. De volgende stappen laten zien hoe u de geselecteerde periode weergegeven in de grafiek telemetrie tussen 15 minuten tot 5 minuten wijzigen:

1. In de `src/services/telemetryService.js` bestand, zoek de aangeroepen functie **getTelemetryByDeviceIdP15M**. Maak een kopie van deze functie en de kopie als volgt wijzigen:

    ```nodejs
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

    ```node.js
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

De telemetrie-grafiek toont de vijf minuten van telemetrische gegevens:

![Telemetrie-grafiek met van één dag](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Een nieuwe KPI toevoegen

De **Dashboard** pagina worden weergegeven voor KPI's in de **System KPI's** deelvenster. Deze KPI's worden berekend in de `src/components/pages/dashboard/dashboard.js` bestand. De KPI's worden weergegeven door de `src/components/pages/dashboard/panels/kpis/kpisPanel.js` bestand. De volgende stappen wordt beschreven hoe u om te berekenen en een nieuwe KPI-waarde weergegeven op de **Dashboard** pagina. Het voorbeeld is het toevoegen van een nieuwe Wijzigingspercentage in waarschuwingssignalen KPI:

1. Open het `src/components/pages/dashboard/dashboard.js`-bestand. Wijzig de **initialState** object om op te nemen een **warningAlarmsChange** eigenschap als volgt te werk:

    ```nodejs
    const initialState = {
      ...

      // Kpis data
      currentActiveAlarms: [],
      topAlarms: [],
      alarmsPerDeviceId: {},
      criticalAlarmsChange: 0,
      warningAlarmsChange: 0,
      kpisIsPending: true,
      kpisError: null,

      ...
    };
    ```

1. Wijzig de **currentAlarmsStats** object om op te nemen **totalWarningCount** als een eigenschap:

    ```nodejs
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alarmsPerDeviceId: updatedAlarmsPerDeviceId
    };
    ```

1. De nieuwe KPI berekenen. Zoek de berekening van het aantal kritieke waarschuwingen. De code dupliceren en de kopie als volgt wijzigen:

    ```nodejs
    // ================== Warning Alarms Count - START
    const currentWarningAlarms = currentAlarmsStats.totalWarningCount;
    const previousWarningAlarms = previousAlarms.reduce(
      (cnt, { severity }) => severity === 'warning' ? cnt + 1 : cnt,
      0
    );
    const warningAlarmsChange = ((currentWarningAlarms - previousWarningAlarms) / currentWarningAlarms * 100).toFixed(2);
    // ================== Warning Alarms Count - END
    ```

1. Zodat de nieuwe **warningAlarmsChange** KPI in de KPI-stroom:

    ```nodejs
    return ({
      kpisIsPending: false,

      // Kpis data
      currentActiveAlarms,
      topAlarms,
      criticalAlarmsChange,
      warningAlarmsChange,
      alarmsPerDeviceId: currentAlarmsStats.alarmsPerDeviceId,

      ...
    });

1. Include the new **warningAlarmsChange** KPI in the state data used to render the UI:

    ```nodejs
    const {
      ...

      currentActiveAlarms,
      topAlarms,
      alarmsPerDeviceId,
      criticalAlarmsChange,
      warningAlarmsChange,
      kpisIsPending,
      kpisError,

      ...
    } = this.state;
    ```

1. Update voor de gegevens aan het deelvenster KPI's doorgegeven:

    ```node.js
    <KpisPanel
      topAlarms={topAlarmsWithName}
      alarmsPerDeviceId={alarmsPerDeviceType}
      criticalAlarmsChange={criticalAlarmsChange}
      warningAlarmsChange={warningAlarmsChange}
      isPending={kpisIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || kpisError}
      colors={chartColorObjects}
      t={t} />
    ```

U bent nu klaar met de wijzigingen in de `src/components/pages/dashboard/dashboard.js` bestand. De volgende stappen beschrijven de wijzigingen aanbrengen in de `src/components/pages/dashboard/panels/kpis/kpisPanel.js` bestand om de nieuwe KPI weer te geven:

1. Wijzig de volgende regel code voor het ophalen van de nieuwe KPI-waarde als volgt:

    ```nodejs
    const { t, isPending, criticalAlarmsChange, warningAlarmsChange, error } = this.props;
    ```

1. Wijzig de markering om de nieuwe KPI-waarde als volgt weer te geven:

    ```nodejs
    <div className="kpi-cell">
      <div className="kpi-header">{t('dashboard.panels.kpis.criticalAlarms')}</div>
      <div className="critical-alarms">
        {
          criticalAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ criticalAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="kpi-header">{t('Warning alarms')}</div>
      <div className="critical-alarms">
        {
          warningAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ warningAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
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

* [De configuratie van microservice voor Azure IoT Solutions (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [De configuratie van microservice voor Azure IoT-oplossingen (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [Azure IoT-pc's-externe controle van Web-UI](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar voor het aanpassen van de web-UI in de oplossingsverbetering voor externe controle.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle [aanpassen en opnieuw implementeren van een microservice](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->