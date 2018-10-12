---
title: Een direct-out toevoegen aan de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een nieuwe fly-out toevoegen aan een pagina in de Remote Monitoring solution accelerator-Webgebruikersinterface.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 9ba58ca887332d2ea224320951b25031cacbef0d
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094579"
---
# <a name="add-a-custom-fly-out-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Een aangepaste fly-out toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface

In dit artikel wordt beschreven hoe u een nieuwe fly-out op een pagina toevoegen in de Remote Monitoring solution accelerator-Webgebruikersinterface. Dit artikel wordt beschreven:

- Klik hier voor meer informatie over het voorbereiden van een lokale ontwikkelingsomgeving.
- Hoe u een nieuwe fly-out toevoegen aan een pagina in de web-UI.

Het voorbeeld direct-out in dit artikel wordt weergegeven op de pagina met het raster die de [een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md) procedures artikel leest u hoe om toe te voegen.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze handleiding hebt voltooid, moet u de volgende software is geïnstalleerd op uw lokale ontwikkelcomputer:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Voordat u begint

U kunt de stappen in de volgende artikelen moet uitvoeren voordat u doorgaat:

- [Een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-page.md).
- [Een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-service.md)
- [Een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-fly-out"></a>Een direct-out toevoegen

Een direct-out toevoegen aan de web-UI, moet u de bronbestanden die de fly-out definiëren toevoegen en wijzigen van bestaande bestanden zodat de web-UI op de hoogte van het nieuwe onderdeel.

### <a name="add-the-new-files-that-define-the-fly-out"></a>Toevoegen van de nieuwe bestanden die het fly-out definiëren

Aan de slag te gaan, de **src/overzicht/onderdelen/pagina's / pageWithFlyout/vervolgmenu's / exampleFlyout** map bevat de bestanden die een direct-out definiëren:

**exampleFlyout.container.js**

[!code-javascript[Example fly-out container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example fly-out container")]

**exampleFlyout.js**

[!code-javascript[Example fly-out](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example fly-out")]

Kopieer de **src/overzicht/onderdelen/pagina's / pageWithFlyout/vervolgmenu's** map die u wilt de **src/onderdelen/pagina's / voorbeeld** map.

### <a name="add-the-fly-out-to-the-page"></a>Het fly-out toevoegen aan de pagina

Wijzig de **src/components/pages/example/basicPage.js** om toe te voegen het fly-out.

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

Voeg een knop om het fly-out om het contextmenu te openen:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Tekst en de container uit elk gewenst moment toevoegen aan de pagina-inhoud:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-fly-out"></a>Test het fly-out

Als de web UI niet al lokaal wordt uitgevoerd, voert u de volgende opdracht uit in de hoofdmap van de lokale kopie van de opslagplaats:

```cmd/sh
npm start
```

De vorige opdracht wordt uitgevoerd de Webinterface lokaal op [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Navigeer naar de **voorbeeld** pagina en klik op **Open Flyout**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar om u te helpen bij het toevoegen of aanpassen van pagina's in de web-UI in de oplossingsverbetering voor externe controle.

Nu u hebt een direct-out ingesteld op een pagina, de volgende stap is het [een deelvenster toevoegen aan het dashboard in de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-panel.md).

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md).
