---
title: Een Azure IoT-Edge-Module maken met behulp van Java | Microsoft Docs
description: Deze zelfstudie gepresenteerd het schrijven van een Aanmeldingsprompt gegevens converter module met behulp van de meest recente Azure IoT rand Maven-pakketten.
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: eddeb5cc13aac7ab33305adcd266465a5b143462
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a>Een Azure IoT-Edge-Module maken met behulp van Java

Deze zelfstudie wordt gepresenteerd hoe een een-module voor Azure IoT rand in Java mogelijk maken.

In deze zelfstudie we helpt bij het instellen van de omgeving en het schrijven van een [uitschakelen](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) gegevens converter module met behulp van de meest recente Azure IoT rand Maven-pakketten.

## <a name="prerequisites"></a>Vereisten

In deze sectie wordt u uw omgeving voor het ontwikkelen van de module IoT rand instellen. Van toepassing op beide *64-bits Windows* en *64-bits Linux (8 Ubuntu/Debian)* besturingssystemen.

De volgende software is vereist:

* [GIT Client](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Open een opdrachtregel terminalvenster en de volgende opslagplaats klonen:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Algehele architectuur

Het platform Azure IoT rand sterk neemt de [Von Neumann architectuur](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Wat betekent dat de volledige rand van Azure IoT-architectuur is een systeem dat invoer verwerkt en produceert uitvoer; en elke afzonderlijke module is ook een klein input-output-subsysteem. In deze zelfstudie stellen we de volgende twee modules:

1. Een module die u een gesimuleerde ontvangt [uitschakelen](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signaal en converteert naar een geformatteerde [JSON](https://en.wikipedia.org/wiki/JSON) bericht.
2. Een module van de ontvangen afgedrukt [JSON](https://en.wikipedia.org/wiki/JSON) bericht.

De volgende afbeelding geeft de typische end-to-end-gegevensstroom voor dit project weer:

![Gegevensstroom tussen drie modules](media/iot-hub-iot-edge-create-module/dataflow.png "invoer: gesimuleerde uitschakelen Module. Processor: Converter Module. Uitvoer: Printer Module")

## <a name="understanding-the-code"></a>Wat is de code?

### <a name="maven-project-structure"></a>De structuur van de maven-project

Omdat Azure IoT rand pakketten zijn gebaseerd op Maven, moeten we een typische Maven projectstructuur maken, waarin een `pom.xml` bestand.

De POM neemt over van de `com.microsoft.azure.gateway.gateway-module-base` pakket dat alle afhankelijkheden die nodig is voor een module-project met de binaire bestanden van de runtime, het bestandspad van de gateway-configuratie en het uitvoeringsgedrag declareert. Hierdoor hoeven we veel tijd en hoeft te schrijven en herschrijven honderden coderegels dan opnieuw.

Moet het bestand pom.xml bijwerken door op te geven van de vereiste afhankelijkheden/plugins en de naam van het configuratiebestand moet worden gebruikt door de module, zoals wordt weergegeven in het volgende codefragment.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set the filename of the Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Basiskennis van een Azure-IoT-Edge-module

U kunt een module voor Azure IoT rand behandelen als een processor van de gegevens waarvan de taak is: invoer ontvangen, verwerken en uitvoer te produceren.

De invoer mogelijk gegevens van hardware (zoals een detectie beweging), een bericht van andere modules of iets anders (zoals een willekeurig getal die regelmatig worden gegenereerd door een timer).

De uitvoer is vergelijkbaar met de invoer gebruikt, kan het gedrag van hardware (zoals de knipperende LED), een bericht naar de andere modules of iets anders (zoals afdrukken naar de console) activeren.

Modules communiceren met elkaar met behulp van `com.microsoft.azure.gateway.messaging.Message` klasse. De **inhoud** van een `Message` is een bytematrix die geschikt is voor elk soort gegevens die u wilt dat vertegenwoordigt. **Eigenschappen** zijn ook beschikbaar in de `Message` en gewoon de toewijzing van een tekenreeks-naar-tekenreeks zijn. U kunt zien **eigenschappen** als de kopteksten in een HTTPS-aanvraag of de metagegevens van een bestand.

Als u wilt een Edge van Azure IoT-module in Java ontwikkelt, moet u maakt een nieuwe moduleklasse die eigenschappen van overneemt `com.microsoft.azure.gateway.core.GatewayModule` en implementeren van de vereiste abstracte methoden `receive()` en `destroy()`. Op dit punt wordt u mogelijk ook voor kiezen voor het implementeren van de optionele `start()` of `create()` ook methoden. Het volgende codefragment ziet u hoe aan de slag ontwerpen van een Azure-IoT-Edge-module.

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a>Converter module

| Invoer                    | Processor                              | Uitvoer                 | Bronbestand            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Bericht temperatuur-gegevens | Parseren en het maken van een nieuwe JSON-bericht | Structuur JSON-bericht | `ConverterModule.java` |

Dit is een typische Azure IoT Edge-module. De cmdlet accepteert temperatuur berichten van andere modules (een hardware-module of in dit geval onze gesimuleerde uitschakelen module); en vervolgens Normaliseert het temperatuur-bericht in een gestructureerde JSON-bericht (inclusief het toevoegen van de bericht-ID, de eigenschap van het of we moeten de temperatuur waarschuwing activeren, enzovoort).

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a>Module printer

| Invoer                          | Processor | Uitvoer                     | Bronbestand          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Een bericht ontvangen van andere modules | N.v.t.       | Meld u het bericht aan console | `PrinterModule.java` |

Dit is een eenvoudige, zelfverklarend, module dit de ontvangen berichten naar de terminal-venster levert.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Azure IoT Edge-configuratie

De laatste stap voordat u de modules is voor het configureren van de rand van de IoT Azure en de verbindingen tussen modules.

Eerst moet u declareren onze Java-laadprogramma (sinds het Azure IoT rand ondersteunt laadprogramma's van verschillende talen) die kan worden verwezen door de `name` in de secties later.

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

Wanneer we onze laadprogramma's hebt gedeclareerd, moeten we ook onze modules ook declareren. Net als bij het declareren van het laadprogramma's, kan ook worden verwezen door hun `name` kenmerk. Als u een module definieert, moeten we de lader van de het moet gebruiken (dit moet zijn voordat is gedefinieerd) opgeven en het-ingangspunt (moet de genormaliseerde klassenaam van onze module zijn) voor elke module. De `simulated_device` is een systeemeigen module die is opgenomen in de Azure IoT rand core runtime-pakket. U moet altijd opnemen `args` in de JSON-bestand, zelfs indien deze `null`.

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
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  }
]
```

Aan het einde van de configuratie maken we de verbindingen. Elke verbinding wordt uitgedrukt door `source` en `sink`. Ze moeten beide verwijzen naar een vooraf gedefinieerde module. Het uitvoerbericht van `source` module zal worden doorgestuurd naar de invoer van `sink` module.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "print"
  }
]
```

## <a name="running-the-modules"></a>De modules uitgevoerd

Gebruik `mvn package` bouwen alles in het `target/` map. `mvn clean package`ook wordt aanbevolen voor een nieuwe build.

Gebruik `mvn exec:exec` om uit te voeren van de rand van de IoT Azure en u rekening moeten houden dat de temperatuur-gegevens en alle eigenschappen die worden afgedrukt aan de console op een vast tarief.

Als u wilt dat de toepassing beëindigt, drukt u op `<Enter>` sleutel.

> [!IMPORTANT]
> Het is niet raadzaam gebruik Ctrl + c drukken om de rand van de IoT gateway-toepassing te beëindigen. Als dit ertoe leiden het proces is abnormaal beëindigd dat kan.

