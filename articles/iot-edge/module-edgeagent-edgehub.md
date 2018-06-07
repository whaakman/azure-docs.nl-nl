---
title: Naslaginformatie over Azure IoT EdgeAgent en EdgeHub | Microsoft Docs
description: Bekijk de specifieke eigenschappen en hun waarden voor de module edgeAgent en edgeHub horende
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0b9e7421bb09e619b4a820910db5faa9edfcc5d5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632904"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Eigenschappen van de rand agent en de rand hub module horende

De Edge-agent en de rand hub zijn twee modules die gezamenlijk de rand van de IoT-runtime. Zie voor meer informatie over welke rechten elke module uitvoert, [begrijpen van de rand van Azure IoT-runtime en de bijbehorende architectuur](iot-edge-runtime.md). 

Dit artikel bevat de gewenste eigenschappen en gerapporteerde eigenschappen van de module runtime horende. Zie [implementatie en controle] [ lnk-deploy] voor meer informatie over het implementeren van modules die zich in IoT Edge-apparaten.

## <a name="edgeagent-desired-properties"></a>Eigenschappen van de gewenste EdgeAgent

De module-twin voor de agent rand wordt aangeroepen `$edgeAgent` en coördineert de communicatie tussen de rand agent wordt uitgevoerd op een apparaat en IoT Hub. De gewenste eigenschappen worden ingesteld wanneer een manifest voor implementatie op een specifiek apparaat toegepast als onderdeel van een implementatie met één apparaat of geschaald. 

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| runtime.type | Moet 'docker' | Ja |
| runtime.settings.minDockerVersion | Ingesteld op de minimaal vereiste door deze implementatiemanifest Docker-versie | Ja |
| runtime.settings.loggingOptions | Een stringified JSON met de opties voor logboekregistratie voor de container van de agent rand. [Docker-opties voor logboekregistratie][lnk-docker-logging-options] | Nee |
| systemModules.edgeAgent.type | Moet 'docker' | Ja |
| systemModules.edgeAgent.settings.image | De URI van de installatiekopie van de Edge-agent. Op dit moment wordt kan de Edge-agent niet automatisch te laten bijwerken. | Ja |
| systemModules.edgeAgent.settings.createOptions | Een stringified JSON met de opties voor het maken van de rand agent-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| systemModules.edgeAgent.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze wordt ingesteld door de IoT Hub wanneer dit manifest wordt toegepast met behulp van een implementatie. Geen deel uit van een manifest voor implementatie. |
| systemModules.edgeHub.type | Moet 'docker' | Ja |
| systemModules.edgeHub.status | Heeft ' actief ' | Ja |
| systemModules.edgeHub.restartPolicy | Moet 'altijd' | Ja |
| systemModules.edgeHub.settings.image | De URI van de installatiekopie van de Edge-hub. | Ja |
| systemModules.edgeHub.settings.createOptions | Een stringified JSON met de opties voor het maken van de rand hub-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| systemModules.edgeHub.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze wordt ingesteld door de IoT Hub wanneer dit manifest wordt toegepast met behulp van een implementatie. Geen deel uit van een manifest voor implementatie. |
| modules. {moduleId} .version | Een gebruiker gedefinieerde tekenreeks die de versie van deze module vertegenwoordigt. | Ja |
| modules. {moduleId} .type | Moet 'docker' | Ja |
| modules. {moduleId} .restartPolicy | {{'nooit' \| 'op-is mislukt' \| 'op-slechte' \| "altijd"} | Ja |
| modules.{moduleId}.settings.image | De URI op de installatiekopie van de module. | Ja |
| modules. {moduleId}.settings.createOptions | Een stringified JSON met de opties voor het maken van de module-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| modules. {moduleId}.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze wordt ingesteld door de IoT Hub wanneer dit manifest wordt toegepast met behulp van een implementatie. Geen deel uit van een manifest voor implementatie. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent gerapporteerd eigenschappen

De agent rand gerapporteerd eigenschappen zijn onder meer de drie belangrijkste stukjes informatie:

1. De status van de toepassing van de laatst gezien gewenste eigenschappen;
2. De status van de modules die momenteel worden uitgevoerd op het apparaat, zoals gemeld door de agent rand; en
3. Een kopie van de gewenste eigenschappen die momenteel worden uitgevoerd op het apparaat.

Deze laatste stukje informatie is nuttig wanneer de meest recente gewenste eigenschappen niet door de runtime toegepast zijn en het apparaat wordt nog steeds uitgevoerd voor een eerdere versie van het implementatiemanifest.

> [!NOTE]
> De gerapporteerde eigenschappen van de Edge-agent zijn handig als ze kunnen worden opgevraagd met de [IoT Hub-querytaal] [ lnk-iothub-query] voor het onderzoeken van de status van implementaties op grote schaal. Raadpleeg [implementaties] [ lnk-deploy] voor meer informatie over het gebruik van deze functie.

De volgende tabel bevat niet de informatie die wordt opgehaald uit de gewenste eigenschappen.

| Eigenschap | Beschrijving |
| -------- | ----------- |
| lastDesiredVersion | Dit geheel getal verwijst naar de laatste versie van de gewenste eigenschappen verwerkt door de agent van de rand. |
| lastDesiredStatus.code | Dit zijn de statuscode die verwijst naar de laatste gewenste eigenschappen die zichtbaar zijn voor de agent van de rand. Toegestane waarden: `200` geslaagd, `400` ongeldige configuratie `412` Ongeldige schemaversie `417` de gewenste eigenschappen zijn leeg is, `500` mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| DeviceHealth | `healthy` Als de runtimestatus van de van alle modules `running` of `stopped`, `unhealthy` anders |
| configurationHealth. .health {deploymentId} | `healthy` Als de runtimestatus van de van alle modules die zijn ingesteld door de implementatie {deploymentId} `running` of `stopped`, `unhealthy` anders |
| runtime.platform.OS | Melden van het besturingssysteem dat wordt uitgevoerd op het apparaat |
| runtime.platform.architecture | Rapportage van de architectuur van de CPU-capaciteit op het apparaat |
| systemModules.edgeAgent.runtimeStatus | De gerapporteerde status van agent van de rand: {'actief' \| 'slechte'} |
| systemModules.edgeAgent.statusDescription | De tekstbeschrijving van de gerapporteerde status van de agent van de rand. |
| systemModules.edgeHub.runtimeStatus | Huidige status van de rand hub: {'actief' \| 'gestopt' \| 'is mislukt' \| 'backoff' \| 'slechte'} |
| systemModules.edgeHub.statusDescription | De tekstbeschrijving van de huidige status van de rand hub als slecht. |
| systemModules.edgeHub.exitCode | Als het is afgesloten, de afsluitcode gerapporteerd door de hub rand container |
| systemModules.edgeHub.startTimeUtc | Tijdstip waarop rand hub voor het laatst is gestart |
| systemModules.edgeHub.lastExitTimeUtc | Tijd wanneer Edge hub laatste is afgesloten |
| systemModules.edgeHub.lastRestartTimeUtc | Keer wanneer Edge hub laatst opnieuw is gestart |
| systemModules.edgeHub.restartCount | Het aantal keren dat deze module opnieuw als onderdeel van het beleid opnieuw op te starten gestart is. |
| modules.{moduleId}.runtimeStatus | Huidige status van de module: {'actief' \| 'gestopt' \| 'is mislukt' \| 'backoff' \| 'slechte'} |
| modules. {moduleId} .statusDescription | Beschrijving van de huidige status van de module als slecht. |
| modules.{moduleId}.exitCode | Als het is afgesloten, de afsluitcode gerapporteerd door de module-container |
| modules.{moduleId}.startTimeUtc | Tijdstip waarop de module voor het laatst is gestart |
| modules.{moduleId}.lastExitTimeUtc | Tijd wanneer de module laatste is afgesloten |
| modules.{moduleId}.lastRestartTimeUtc | Tijd wanneer de module voor het laatst is gestart |
| modules. {moduleId} .restartCount | Het aantal keren dat deze module opnieuw als onderdeel van het beleid opnieuw op te starten gestart is. |

## <a name="edgehub-desired-properties"></a>Eigenschappen van de gewenste EdgeHub

De module-twin voor de hub rand heet `$edgeHub` en coördineert de communicatie tussen de rand hub uitgevoerd op een apparaat en IoT Hub. De gewenste eigenschappen worden ingesteld wanneer een manifest voor implementatie op een specifiek apparaat toegepast als onderdeel van een implementatie met één apparaat of geschaald. 

| Eigenschap | Beschrijving | In het implementatiemanifest is vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| routes.{routeName} | Een tekenreeks die een Edge hub route vertegenwoordigt. | De `routes` element kunt aanwezig maar leeg zijn. |
| storeAndForwardConfiguration.timeToLiveSecs | De tijd in seconden waarmee rand hub berichten in geval van een niet-verbonden routering eindpunten, bijvoorbeeld losgekoppeld van de IoT-Hub of lokale module blijft | Ja |

## <a name="edgehub-reported-properties"></a>EdgeHub gerapporteerd eigenschappen

| Eigenschap | Beschrijving |
| -------- | ----------- |
| lastDesiredVersion | Dit geheel getal verwijst naar de laatste versie van de gewenste eigenschappen verwerkt door de Edge-hub. |
| lastDesiredStatus.code | Dit zijn de statuscode die verwijst naar de laatste gewenste eigenschappen die zichtbaar zijn voor de Edge-hub. Toegestane waarden: `200` geslaagd, `400` ongeldige configuratie `500` mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| -clients. {apparaat of module identiteit} .status | De status van dit apparaat of de module. Mogelijke waarden {'verbonden' \| 'verbinding verbroken'}. Module-identiteiten kunnen zich in de status voor verbroken verbindingen. Downstream apparaten verbinden met rand hub worden alleen weergegeven wanneer verbonden. |
| -clients. {apparaat of module identiteit} .lastConnectTime | Laatste keer dat het apparaat of de module verbonden |
| -clients. {apparaat of module identiteit} .lastDisconnectTime | Laatste keer dat het apparaat of de module verbroken |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van deze eigenschappen voor het bouwen van implementatiemanifesten, [begrijpen hoe IoT rand modules kunnen worden gebruikt, geconfigureerd en hergebruikt](module-composition.md).

<!--links -->
[lnk-deploy]: module-deployment-monitoring.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
