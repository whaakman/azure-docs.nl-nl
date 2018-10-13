---
title: Een flyout toevoegen aan de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een nieuwe flyout toevoegen aan een pagina in de Remote Monitoring solution accelerator-Webgebruikersinterface.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: ccb1a7ff6abbc68f42c7632a8ba7a392b2c48794
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167423"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Een aangepaste flyout toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface

In dit artikel wordt beschreven hoe u een nieuwe flyout naar een pagina toevoegen in de Remote Monitoring solution accelerator-Webgebruikersinterface. Dit artikel wordt beschreven:

- Klik hier voor meer informatie over het voorbereiden van een lokale ontwikkelingsomgeving.
- Hoe u een nieuwe flyout toevoegen aan een pagina in de web-UI.

De voorbeeld-flyout in dit artikel wordt weergegeven op de pagina met het raster die de [een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md) procedures artikel leest u hoe om toe te voegen.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze handleiding hebt voltooid, moet u de volgende software is geïnstalleerd op uw lokale ontwikkelcomputer:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Voordat u begint

U kunt de stappen in de volgende artikelen moet uitvoeren voordat u doorgaat:

- [Een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-page.md).
- [Een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-service.md)
- [Een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>Een flyout toevoegen

Een flyout toevoegen aan de web-UI, moet u de bronbestanden die de flyout definiëren toevoegen en wijzigen van bestaande bestanden zodat de web-UI op de hoogte van het nieuwe onderdeel.

### <a name="add-the-new-files-that-define-the-flyout"></a>Toevoegen van de nieuwe bestanden die de flyout definiëren

Aan de slag te gaan, de **src/overzicht/onderdelen/pagina's / pageWithFlyout/vervolgmenu's / exampleFlyout** map bevat de bestanden die een flyout definiëren:

**exampleFlyout.container.js**

[!code-javascript[Example flyout container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example flyout container")]

**exampleFlyout.js**

[!code-javascript[Example flyout](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example flyout")]

Kopieer de **src/overzicht/onderdelen/pagina's / pageWithFlyout/vervolgmenu's** map die u wilt de **src/onderdelen/pagina's / voorbeeld** map.

### <a name="add-the-flyout-to-the-page"></a>De flyout toevoegen aan de pagina

Wijzig de **src/components/pages/example/basicPage.js** om toe te voegen de flyout.

Toevoegen **Btn** aan de invoer van **onderdelen/gedeelde** en toevoegen van invoer voor **svgs** en **ExampleFlyoutContainer**:

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

Voeg een **const** definitie voor **closedFlyoutState** en deze naar de status in de constructor toe te voegen:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

De volgende functies toevoegen de **BasicPage** klasse:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

Voeg de volgende **const** definities voor de **renderen** functie:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Voeg een knop als u wilt de flyout om het contextmenu te openen:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Tekst en de container flyout toevoegen aan de pagina-inhoud:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>De flyout testen

Als de web UI niet al lokaal wordt uitgevoerd, voert u de volgende opdracht uit in de hoofdmap van de lokale kopie van de opslagplaats:

```cmd/sh
npm start
```

De vorige opdracht wordt uitgevoerd de Webinterface lokaal op [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Navigeer naar de **voorbeeld** pagina en klik op **Open Flyout**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar om u te helpen bij het toevoegen of aanpassen van pagina's in de web-UI in de oplossingsverbetering voor externe controle.

Nu u hebt een flyout gedefinieerd op een pagina, de volgende stap is het [een deelvenster toevoegen aan het dashboard in de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-panel.md).

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md).
