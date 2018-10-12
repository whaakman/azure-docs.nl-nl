---
title: Een service toevoegen aan de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u voor het toevoegen van een nieuwe service in de Remote Monitoring solution accelerator-Webgebruikersinterface.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: e44aa8ade512a6005959e795cb1d4ad861da1338
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094578"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface

In dit artikel wordt beschreven hoe u voor het toevoegen van een nieuwe service in de Remote Monitoring solution accelerator-Webgebruikersinterface. Dit artikel wordt beschreven:

- Klik hier voor meer informatie over het voorbereiden van een lokale ontwikkelingsomgeving.
- Hoe u een nieuwe service toevoegen aan de web-UI.

De voorbeeld-service in dit artikel bevat de gegevens voor een raster die de [een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md) procedures artikel leest u hoe om toe te voegen.

In een React-toepassing communiceert een service doorgaans met een back-endservice. Voorbeelden in de oplossingsverbetering voor externe controle zijn services die interactie hebben met de IoT hub-beheer en de configuratie van microservices.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze handleiding hebt voltooid, moet u de volgende software is geïnstalleerd op uw lokale ontwikkelcomputer:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Voordat u begint

U moet de stappen in de [een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-page.md) artikel met instructies voordat u doorgaat.

## <a name="add-a-service"></a>Een service toevoegen

Een service toevoegen aan de web-UI, moet u de bronbestanden die de service ook definiëren toevoegen en wijzigen van bestaande bestanden zodat de web-UI op de hoogte van de nieuwe service.

### <a name="add-the-new-files-that-define-the-service"></a>Toevoegen van de nieuwe bestanden die de service ook definiëren

Aan de slag te gaan, de **src/overzicht/services** map bevat de bestanden die u een eenvoudige service definieert:

**exampleService.js**

[!code-javascript[Example service](~/remote-monitoring-webui/src/walkthrough/services/exampleService.js?name=service "Example service")]

Zie voor meer informatie over hoe de services zijn geïmplementeerd, [de inleiding tot reactieve programmering u hebt gemist](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754).

**model/exampleModels.js**

[!code-javascript[Example model](~/remote-monitoring-webui/src/walkthrough/services/models/exampleModels.js?name=models "Example model")]

Kopie **exampleService.js** naar de **src/services** map en kopieer **exampleModels.js** naar de **src/services/modellen** map.

Update de **index.js** -bestand in de **src/services** map voor het exporteren van de nieuwe service:

```js
export * from './exampleService';
```

Update de **index.js** -bestand in de **src/services/modellen** map voor het exporteren van het nieuwe model:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Instellen van de aanroepen naar de service uit de store

Aan de slag te gaan, de **src/overzicht/store/reducers tegelijkertijd** map bevat een voorbeeld-reducer:

**exampleReducer.js**

[!code-javascript[Example reducer](~/remote-monitoring-webui/src/walkthrough/store/reducers/exampleReducer.js?name=reducer "Example reducer")]

Kopie **exampleReducer.js** naar de **src/store/reducers tegelijkertijd** map.

Voor meer informatie over de reducer en **Epics**, Zie [redux zichtbaar](https://redux-observable.js.org/).

### <a name="configure-the-middleware"></a>De middleware configureren

Voor het configureren van de middleware, toevoegen de reducer naar de **rootReducer.js** -bestand in de **src/store** map:

```js
import { reducer as exampleReducer } from './reducers/exampleReducer';

const rootReducer = combineReducers({
  ...appReducer,
  ...devicesReducer,
  ...rulesReducer,
  ...simulationReducer,
  ...exampleReducer
});
```

Toevoegen van de epics naar de **rootEpics.js** -bestand in de **src/store** map:

```js
import { epics as exampleEpics } from './reducers/exampleReducer';

// Extract the epic function from each property object
const epics = [
  ...appEpics.getEpics(),
  ...devicesEpics.getEpics(),
  ...rulesEpics.getEpics(),
  ...simulationEpics.getEpics(),
  ...exampleEpics.getEpics()
];
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar om u te helpen bij het toevoegen of aanpassen van services in de web-UI in de oplossingsverbetering voor externe controle.

Nu u een service hebt gedefinieerd, wordt de volgende stap is het [een aangepaste raster toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-grid.md) die gegevens die zijn geretourneerd door de service worden weergegeven.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md).
