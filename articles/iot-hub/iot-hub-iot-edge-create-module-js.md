---
title: Een Azure IoT-Edge-Module maken met behulp van Node.js | Microsoft Docs
description: Deze zelfstudie gepresenteerd het schrijven van een Aanmeldingsprompt gegevens converter module met behulp van de meest recente Azure IoT rand NPM pakketten en Yeoman generator.
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: ba466f47e157d805600c41fa3d84ed5a0363969c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Een Azure IoT-Edge-Module maken met behulp van Node.js

Deze zelfstudie maken van een module voor Azure IoT rand in de JS gepresenteerd.

In deze zelfstudie we doorlopen omgeving in te stellen en het schrijven van een [uitschakelen](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) gegevens converter module met behulp van de meest recente Azure IoT rand NPM-pakketten.

## <a name="prerequisites"></a>Vereisten

In deze sectie, moet u uw omgeving voor het ontwikkelen van de module IoT rand instellen. Van toepassing op beide *64-bits Windows* en *64-bits Linux (Ubuntu) 14 +* besturingssystemen.

De volgende software is vereist:
* [GIT Client](https://git-scm.com/downloads).
* [Knooppunt TNS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Architectuur

Het platform Azure IoT rand sterk neemt de [Von Neumann architectuur](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Wat betekent dat de volledige rand van Azure IoT-architectuur is een systeem dat invoer verwerkt en produceert uitvoer; en elke afzonderlijke module is ook een klein input-output-subsysteem. In deze zelfstudie stellen we de volgende twee modules:

1. Een module die u een gesimuleerde ontvangt [uitschakelen](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signaal en converteert naar een geformatteerde [JSON](https://en.wikipedia.org/wiki/JSON) bericht.
2. Een module die worden afgedrukt de ontvangen [JSON](https://en.wikipedia.org/wiki/JSON) bericht.

De volgende afbeelding geeft de typische end-to-end-gegevensstroom voor dit project weer:

![Gegevensstroom tussen drie modules](media/iot-hub-iot-edge-create-module/dataflow.png "invoer: gesimuleerde uitschakelen Module. Processor: Converter Module. Uitvoer: Printer Module")

## <a name="set-up-the-environment"></a>De omgeving instellen
Hieronder wordt beschreven hoe u snel omgeving instellen om te beginnen met het schrijven van uw eerste uitschakelen converter module met JS.

### <a name="create-module-project"></a>Module-project maken
1. Open een opdrachtregelvenster, voert u `yo az-iot-gw-module`.
2. Volg de stappen op het scherm voor het voltooien van de initialisatie van de module-project.

### <a name="project-structure"></a>Projectstructuur
Een module JS project bestaat uit de volgende onderdelen:

`modules`-De aangepaste JS module bronbestanden. De standaard vervangen `sensor.js` en `printer.js` met uw eigen module-bestanden.

`app.js`-De post-bestand om het exemplaar van de rand te starten.

`gw.config.json`-Het configuratiebestand voor het aanpassen van de modules moeten worden geladen door een rand.

`package.json`-Informatie over de metagegevens voor module project.

`README.md`-De basisdocumentatie voor module project.


### <a name="package-file"></a>Het pakketbestand

Dit `package.json` declareert alle informatie over de metagegevens die nodig is voor een module-project met de naam, versie, post, scripts, runtime en ontwikkeling afhankelijkheden.

Volgende codefragment toont hoe u configureert voor converter-voorbeeldproject uitschakelen.
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a>Post-bestand
De `app.js` definieert de manier waarop het exemplaar van de rand initialiseren. Er hoeft niet hier een wijziging aanbrengt.

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a>Interface van de module
U kunt een module voor Azure IoT rand behandelen als een processor van de gegevens waarvan de taak is: invoer ontvangen, verwerken en uitvoer te produceren.

De invoer mogelijk gegevens van hardware (zoals een detectie beweging), een bericht van andere modules of iets anders (zoals een willekeurig getal die regelmatig worden gegenereerd door een timer).

De uitvoer is vergelijkbaar met de invoer gebruikt, kan het gedrag van hardware (zoals de knipperende LED), een bericht naar de andere modules of iets anders (zoals afdrukken naar de console) activeren.

Modules communiceren met elkaar met behulp van `message` object. De **inhoud** van een `message` is een bytematrix die geschikt is voor elk soort gegevens die u wilt dat vertegenwoordigt. **Eigenschappen** zijn ook beschikbaar in de `message` en gewoon de toewijzing van een tekenreeks-naar-tekenreeks zijn. U kunt zien **eigenschappen** als de kopteksten in een HTTP-aanvraag of de metagegevens van een bestand.

Om een Azure-IoT-Edge van de module JS ontwikkelt, moet u een nieuwe module-object dat de vereiste methoden implementeert maken `receive()`. Op dit punt wordt u mogelijk ook voor kiezen voor het implementeren van de optionele `create()` of `start()`, of `destroy()` ook methoden. Het volgende codefragment bevat de steiger van JS module-object.

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a>Converter module
| Invoer                    | Processor                              | Uitvoer                 | Bronbestand            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Bericht temperatuur-gegevens | Parseren en het maken van een nieuwe JSON-bericht | Structuur JSON-bericht | `converter.js` |

Dit is een typische Azure IoT Edge-module. De cmdlet accepteert temperatuur berichten van andere modules (een hardware-module of in dit geval onze gesimuleerde uitschakelen module); en vervolgens Normaliseert het temperatuur-bericht in een gestructureerde JSON-bericht (inclusief het toevoegen van de bericht-ID, de eigenschap van het of we moeten de temperatuur waarschuwing activeren, enzovoort).

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a>Module printer
| Invoer                          | Processor | Uitvoer                     | Bronbestand          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Een bericht ontvangen van andere modules | N.v.t.       | Meld u het bericht aan console | `printer.js` |

Deze module is eenvoudig, zelfverklarend, die het terminalvenster uitgang van de ontvangen berichten (eigenschap, inhoud).

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Configuratie
De laatste stap voordat u de modules is voor het configureren van de rand van de IoT Azure en de verbindingen tussen modules.

Eerst moet u declareren onze `node` loader (sinds het Azure IoT rand ondersteunt laadprogramma's van verschillende talen) die kan worden verwezen door de `name` in de secties later.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Wanneer we onze laadprogramma's hebt gedeclareerd, moeten we ook onze modules ook declareren. Net als bij het declareren van het laadprogramma's, kan ook worden verwezen door hun `name` kenmerk. Als u een module definieert, moeten we de lader van de het moet gebruiken (dit moet zijn voordat is gedefinieerd) opgeven en het-ingangspunt (moet de genormaliseerde klassenaam van onze module zijn) voor elke module. De `simulated_device` is een systeemeigen module die is opgenomen in de Azure IoT rand core runtime-pakket. Omvatten `args` in de JSON-bestand, zelfs indien deze `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

Aan het einde van de configuratie maken we de verbindingen. Elke verbinding wordt uitgedrukt door `source` en `sink`. Ze moeten beide verwijzen naar een vooraf gedefinieerde module. Het uitvoerbericht van `source` module wordt doorgestuurd naar de invoer van `sink` module.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a>De modules uitgevoerd
1. `npm install`
2. `npm start`

Als u wilt dat de toepassing beëindigt, drukt u op `<Enter>` sleutel.

> [!IMPORTANT]
> Het is niet raadzaam gebruik Ctrl + c drukken om de rand van de IoT-toepassing te beëindigen. Als op deze manier ertoe leiden het proces is abnormaal beëindigd dat kan.
