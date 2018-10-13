---
title: Een raster toevoegen aan de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een nieuwe groeps-id toevoegen aan een pagina in de Remote Monitoring solution accelerator-Webgebruikersinterface.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/04/2018
ms.topic: conceptual
ms.openlocfilehash: e1c694847a1ec16d4d7a7b1118df71cb06396186
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165927"
---
# <a name="add-a-custom-grid-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface

In dit artikel wordt beschreven hoe u een nieuw raster op een pagina toevoegen in de Remote Monitoring solution accelerator-Webgebruikersinterface. Dit artikel wordt beschreven:

- Klik hier voor meer informatie over het voorbereiden van een lokale ontwikkelingsomgeving.
- Hoe u een nieuw raster toevoegen aan een pagina in de web-UI.

Het voorbeeld van een raster in dit artikel worden de gegevens van de service weergegeven die de [een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-service.md) procedures artikel leest u hoe om toe te voegen.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze handleiding hebt voltooid, moet u de volgende software is geïnstalleerd op uw lokale ontwikkelcomputer:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Voordat u begint

U kunt de stappen in de volgende artikelen moet uitvoeren voordat u doorgaat:

- [Een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-page.md).
- [Een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-service.md)

## <a name="add-a-grid"></a>Een raster toevoegen

Een raster toevoegen aan de web-UI, moet u de bronbestanden die het raster definiëren toevoegen en wijzigen van bestaande bestanden zodat de web-UI op de hoogte van het nieuwe onderdeel.

### <a name="add-the-new-files-that-define-the-grid"></a>Toevoegen van de nieuwe bestanden die het raster definiëren

Aan de slag te gaan, de **scenario/src/onderdelen/pageWithGrid/pagina's / exampleGrid** map bevat de bestanden die een raster definiëren:

**exampleGrid.js**

[!code-javascript[Example grid](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithGrid/exampleGrid/exampleGrid.js?name=grid "Example grid")]

**exampleGridConfig.js**

[!code-javascript[Example grid configuration](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithGrid/exampleGrid/exampleGridConfig.js?name=gridconfig "Example grid configuration")]

Kopieer de **scenario/src/onderdelen/pageWithGrid/pagina's / exampleGrid** map die u wilt de **src/onderdelen/pagina's / voorbeeld** map.

### <a name="add-the-grid-to-the-page"></a>Het raster toevoegen aan de pagina

Wijzig de **src/components/pages/example/basicPage.container.js** als volgt voor het importeren van de servicedefinities:

```js
import { connect } from 'react-redux';
import { translate } from 'react-i18next';

import {
  epics as exampleEpics,
  getExamples,
  getExamplesError,
  getExamplesLastUpdated,
  getExamplesPendingStatus
} from 'store/reducers/exampleReducer';
import { BasicPage } from './basicPage';

// Pass the data
const mapStateToProps = state => ({
  data: getExamples(state),
  error: getExamplesError(state),
  isPending: getExamplesPendingStatus(state),
  lastUpdated: getExamplesLastUpdated(state)
});

// Wrap the dispatch method
const mapDispatchToProps = dispatch => ({
  fetchData: () => dispatch(exampleEpics.actions.fetchExamples())
});

export const BasicPageContainer = translate()(connect(mapStateToProps, mapDispatchToProps)(BasicPage));
```

Wijzig de **src/components/pages/example/basicPage.js** als volgt om toe te voegen van de grid:

```js
// Copyright (c) Microsoft. All rights reserved.

import React, { Component } from 'react';

import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';

import './basicPage.css';

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null };
  }

  componentDidMount() {
    const { isPending, lastUpdated, fetchData } = this.props;
    if (!lastUpdated && !isPending) fetchData();
  }

  onGridReady = gridReadyEvent => this.gridApi = gridReadyEvent.api;

  onContextMenuChange = contextBtns => this.setState({ contextBtns });

  render() {
    const { t, data, error, isPending, lastUpdated, fetchData } = this.props;
    const gridProps = {
      onGridReady: this.onGridReady,
      rowData: isPending ? undefined : data || [],
      onContextMenuChange: this.onContextMenuChange,
      t: this.props.t
    };

    return [
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
      </ContextMenu>,
      <PageContent className="basic-page-container" key="page-content">
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
    ];
  }
}
```

Wijzig de **src/components/pages/example/basicPage.test.js** als volgt om bij te werken van de tests:

```js
// Copyright (c) Microsoft. All rights reserved.

import React from 'react';
import { shallow } from 'enzyme';
import 'polyfills';

import { BasicPage } from './basicPage';

describe('BasicPage Component', () => {
  it('Renders without crashing', () => {

    const fakeProps = {
      data: undefined,
      error: undefined,
      isPending: false,
      lastUpdated: undefined,
      fetchData: () => { },
      t: () => { },
    };

    const wrapper = shallow(
      <BasicPage {...fakeProps} />
    );
  });
});
```

## <a name="test-the-grid"></a>Het raster testen

Als de web UI niet al lokaal wordt uitgevoerd, voert u de volgende opdracht uit in de hoofdmap van de lokale kopie van de opslagplaats:

```cmd/sh
npm start
```

De vorige opdracht wordt uitgevoerd de Webinterface lokaal op [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Navigeer naar de **voorbeeld** pagina om te bekijken van het raster weergeven van gegevens uit de service.

## <a name="select-rows"></a>Rijen selecteren

Er zijn twee opties voor het inschakelen van een gebruiker om rijen te selecteren in het raster:

### <a name="hard-select-rows"></a>Harde-Selecteer rijen

Als een gebruiker nodig heeft om te reageren op meerdere rijen op hetzelfde moment, gebruikt u de selectievakjes in rijen:

1. Harde-selectie van rijen inschakelen door toe te voegen een **checkboxColumn** naar de **columnDefs** geleverd aan het raster. **checkboxColumn** is gedefinieerd in **/src/components/shared/pcsGrid/pcsGrid.js**:

    ```js
    this.columnDefs = [
      checkboxColumn,
      exampleColumnDefs.id,
      exampleColumnDefs.description
    ];
    ```

1. Voor toegang tot de geselecteerde items, krijgt u een verwijzing naar de interne grid-API:

    ```js
    onGridReady = gridReadyEvent => {
      this.gridApi = gridReadyEvent.api;
      // Call the onReady props if it exists
      if (isFunc(this.props.onGridReady)) {
        this.props.onGridReady(gridReadyEvent);
      }
    };
    ```

1. Context knoppen op de pagina opgeven als een rij in het raster harde geselecteerd is:

    ```js
    this.contextBtns = [
      <Btn key="context-btn-1" svg={svgs.reconfigure} onClick={this.doSomething()}>Button 1</Btn>,
      <Btn key="context-btn-2" svg={svgs.trash} onClick={this.doSomethingElse()}>Button 2</Btn>
    ];
    ```

    ```js
    onHardSelectChange = (selectedObjs) => {
      const { onContextMenuChange, onHardSelectChange } = this.props;
      // Show the context buttons when there are rows checked.
      if (isFunc(onContextMenuChange)) {
        onContextMenuChange(selectedObjs.length > 0 ? this.contextBtns : null);
      }
      //...
    }
    ```

1. Wanneer een context wordt geklikt, krijgt u de harde-geselecteerde items voor uw werk op:

    ```js
    doSomething = () => {
      //Just for demo purposes. Don't console log in a real grid.
      console.log('hard selected rows', this.gridApi.getSelectedRows());
    };
    ```

### <a name="soft-select-rows"></a>Voorlopig Selecteer rijen

Als de gebruiker is alleen nodig heeft om te reageren op een enkele rij, configureert u een koppeling voorlopig selecteren voor een of meer kolommen in de **columnDefs**.

1. In **exampleGridConfig.js**, toevoegen **SoftSelectLinkRenderer** als de **cellRendererFramework** voor een **columnDef**.

    ```js
    export const exampleColumnDefs = {
      id: {
        headerName: 'examples.grid.name',
        field: 'id',
        sort: 'asc',
        cellRendererFramework: SoftSelectLinkRenderer
      }
    };
    ```

1. Wanneer een voorlopig-Selecteer-koppeling wordt geklikt, wordt de **onSoftSelectChange** gebeurtenis. Welke actie vereist is voor die rij, zoals het openen van een details-flyout uitvoeren In dit voorbeeld schrijft u gewoon naar de console:

    ```js
    onSoftSelectChange = (rowId, rowEvent) => {
      const { onSoftSelectChange } = this.props;
      const obj = (this.gridApi.getDisplayedRowAtIndex(rowId) || {}).data;
      if (obj) {
        //Just for demo purposes. Don't console log a real grid.
        console.log('Soft selected', obj);
        this.setState({ softSelectedObj: obj });
      }
      if (isFunc(onSoftSelectChange)) {
        onSoftSelectChange(obj, rowEvent);
      }
    }
    ```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar om u te helpen bij het toevoegen of aanpassen van pagina's in de web-UI in de oplossingsverbetering voor externe controle.

Nu u een raster hebt gedefinieerd, wordt de volgende stap is het [een aangepaste flyout toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-flyout.md) die wordt weergegeven op de voorbeeldpagina.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md).
