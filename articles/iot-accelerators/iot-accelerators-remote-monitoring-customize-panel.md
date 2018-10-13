---
title: Een paneel toevoegen aan de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een nieuw deelvenster toevoegen aan het dashboard in de Remote Monitoring solution accelerator-Webgebruikersinterface.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 3b855c3bed75945f44b55463bdacd049b7930aa7
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165875"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>Een aangepaste paneel toevoegen aan het dashboard in de Remote Monitoring solution accelerator-Webgebruikersinterface

In dit artikel wordt beschreven hoe u een nieuw deelvenster op een dashboardpagina toevoegen in de Remote Monitoring solution accelerator-Webgebruikersinterface. Dit artikel wordt beschreven:

- Klik hier voor meer informatie over het voorbereiden van een lokale ontwikkelingsomgeving.
- Hoe u een nieuw deelvenster toevoegen aan een dashboardpagina in de web-UI.

Het deelvenster Voorbeeld in dit artikel wordt weergegeven op de dashboardpagina van de bestaande.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze handleiding hebt voltooid, moet u de volgende software is geïnstalleerd op uw lokale ontwikkelcomputer:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Voordat u begint

U moet de stappen in de [een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-page.md) artikel voordat u doorgaat.

## <a name="add-a-panel"></a>Een deelvenster toevoegen

Als u wilt toevoegen een deelvenster aan de web-UI, moet u de bronbestanden die definiëren van het paneel toevoegen en wijzig vervolgens het dashboard om het deelvenster weer te geven.

### <a name="add-the-new-files-that-define-the-panel"></a>Toevoegen van de nieuwe bestanden die het paneel definiëren

Aan de slag te gaan, de **src/overzicht/onderdelen/pagina's / dashboard/panelen/examplePanel** map bevat de bestanden die een deelvenster definieert, met inbegrip van:

**examplePanel.js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

Kopieer de **src/overzicht/onderdelen/pagina's / dashboard/panelen/examplePanel** map die u wilt de **src/onderdelen/pagina's / dashboard/panelen** map.

Voeg de volgende uitvoer naar de **src/walkthrough/components/pages/dashboard/panels/index.js** bestand:

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>Het paneel aan het dashboard toevoegen

Wijzig de **src/components/pages/dashboard/dashboard.js** om toe te voegen van het paneel.

De voorbeeld-deelvenster toevoegen aan de lijst van de invoer van deelvensters:

```js
import {
  OverviewPanel,
  AlertsPanel,
  TelemetryPanel,
  AnalyticsPanel,
  MapPanel,
  transformTelemetryResponse,
  chartColorObjects,
  ExamplePanel
} from './panels';
```

De celdefinitie van de volgende toevoegen aan het raster in de pagina-inhoud:

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-flyout"></a>De flyout testen

Als de web UI niet al lokaal wordt uitgevoerd, voert u de volgende opdracht uit in de hoofdmap van de lokale kopie van de opslagplaats:

```cmd/sh
npm start
```

De vorige opdracht wordt uitgevoerd de Webinterface lokaal op [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Navigeer naar de **Dashboard** pagina om het nieuwe deelvenster weer te geven.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar om u te helpen bij het toevoegen of aanpassen van dashboards in de web-UI in de oplossingsverbetering voor externe controle.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md).
