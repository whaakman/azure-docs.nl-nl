---
title: EdgeAgent en EdgeHub gewenste eigenschappen van referentie - Azure IoT Edge | Microsoft Docs
description: Controleer de specifieke eigenschappen en hun waarden voor de edgeAgent en edgeHub moduledubbels
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: e8a8170023c8f529894522e27a4c6231325089af
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190986"
---
# <a name="properties-of-the-iot-edge-agent-and-iot-edge-hub-module-twins"></a>Eigenschappen van de IoT Edge-agent en de IoT Edge hub moduledubbels

De IoT Edge-agent en de IoT Edge hub zijn twee modules die gezamenlijk de IoT Edge-runtime. Zie voor meer informatie over welke functies elke module wordt uitgevoerd, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md). 

Dit artikel bevat de gewenste eigenschappen en de gerapporteerde eigenschappen van de runtime-moduledubbels. Zie voor meer informatie over het implementeren van modules op IoT Edge-apparaten [meer informatie over het implementeren van modules en routes instellen in IoT Edge](module-composition.md).

Een moduledubbel bevat: 

* **Gewenste eigenschappen**. De oplossing back-end gewenste eigenschappen kunt instellen en deze kan worden gelezen door de module. De module kan ook met het ontvangen van meldingen van wijzigingen in de gewenste eigenschappen. Gewenste eigenschappen worden gebruikt samen met de gerapporteerde eigenschappen om te synchroniseren van de configuratie van de module of voorwaarden.

* **Gerapporteerde eigenschappen**. De module gerapporteerde eigenschappen kunt instellen en de oplossing back-end kan lezen en query's voor uitvoert. Gerapporteerde eigenschappen worden samen met de gewenste eigenschappen gebruikt voor het synchroniseren van de configuratie van de module of voorwaarden. 

## <a name="edgeagent-desired-properties"></a>EdgeAgent gewenste eigenschappen

De moduledubbel voor de IoT Edge-agent wordt aangeroepen `$edgeAgent` en coördineert de communicatie tussen de IoT Edge-agent die wordt uitgevoerd op een apparaat en IoT-Hub. De gewenste eigenschappen worden ingesteld bij het toepassen van een manifest van de implementatie op een specifiek apparaat als onderdeel van de implementatie van een één-apparaat of op schaal. 

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| runtime.type | Moet "docker" | Ja |
| runtime.settings.minDockerVersion | Ingesteld op de minimaal vereiste door deze implementatie manifest Docker-versie | Ja |
| runtime.settings.loggingOptions | Een stringified JSON met de opties voor logboekregistratie voor de IoT Edge agent-container. [Docker-opties voor logboekregistratie](https://docs.docker.com/engine/admin/logging/overview/) | Nee |
| runtime.settings.registryCredentials<br>. {registryId} .username | De gebruikersnaam van het containerregister. De gebruikersnaam is voor Azure Container Registry, gewoonlijk de naam van het containerregister.<br><br> Registerreferenties zijn nodig om alle module-installatiekopieën die niet openbaar zijn. | Nee |
| runtime.settings.registryCredentials<br>. {registryId} .password | Het wachtwoord voor het containerregister. | Nee |
| runtime.settings.registryCredentials<br>. {registryId} .address | Het adres van het containerregister. Voor Azure Container Registry, is het adres meestal *{registernaam}.azurecr.IO*. | Nee |  
| systemModules.edgeAgent.type | Moet "docker" | Ja |
| systemModules.edgeAgent.settings.image | De URI van de installatiekopie van de IoT Edge-agent. De IoT Edge-agent is momenteel niet kunnen automatisch te laten bijwerken. | Ja |
| systemModules.edgeAgent.settings<br>.createOptions | Een stringified JSON met de opties voor het maken van de IoT Edge agent-container. [Opties voor docker maken](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Nee |
| systemModules.edgeAgent.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | IoT Hub stelt deze eigenschap wanneer het manifest wordt toegepast met behulp van een implementatie. Geen deel uitmaakt van een manifest van de implementatie. |
| systemModules.edgeHub.type | Moet "docker" | Ja |
| systemModules.edgeHub.status | Moet worden 'running' | Ja |
| systemModules.edgeHub.restartPolicy | 'Altijd' moet | Ja |
| systemModules.edgeHub.settings.image | De URI van de installatiekopie van de IoT Edge hub. | Ja |
| systemModules.edgeHub.settings<br>.createOptions | Een stringified JSON met de opties voor het maken van de IoT Edge hub-container. [Opties voor docker maken](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Nee |
| systemModules.edgeHub.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | IoT Hub stelt deze eigenschap wanneer het manifest wordt toegepast met behulp van een implementatie. Geen deel uitmaakt van een manifest van de implementatie. |
| modules. {moduleId} .version | Een gebruiker gedefinieerde tekenreeks voor de versie van deze module. | Ja |
| modules. {moduleId} .type | Moet "docker" | Ja |
| modules. {moduleId} .status | {'running' \| 'stopped'} | Ja |
| modules. {moduleId} .restartPolicy | {"nooit" \| 'on-failure' \| 'van-niet in orde' \| "altijd"} | Ja |
| modules.{moduleId}.settings.image | De URI naar de installatiekopie van de module. | Ja |
| modules. {moduleId}.settings.createOptions | Een stringified JSON met de opties voor het maken van de module-container. [Opties voor docker maken](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Nee |
| modules. {moduleId}.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | IoT Hub stelt deze eigenschap wanneer het manifest wordt toegepast met behulp van een implementatie. Geen deel uitmaakt van een manifest van de implementatie. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent gerapporteerde eigenschappen

De IoT Edge agent gerapporteerde eigenschappen van de drie belangrijkste stukjes informatie zijn:

1. De status van de toepassing van de laatst gezien gewenste eigenschappen;
2. De status van de modules die momenteel wordt uitgevoerd op het apparaat, zoals gemeld door de IoT Edge-agent; en
3. Een kopie van de gewenste eigenschappen die momenteel wordt uitgevoerd op het apparaat.

Deze laatste stukje informatie, een kopie van de huidige gewenste eigenschappen is handig om te zien of het apparaat de meest recente gewenste eigenschappen is toegepast, of is nog een manifest van de vorige implementatie wordt uitgevoerd.

> [!NOTE]
> De gerapporteerde eigenschappen van de IoT Edge-agent zijn nuttig omdat ze kunnen worden opgevraagd met de [IoT Hub-querytaal](../iot-hub/iot-hub-devguide-query-language.md) voor het onderzoeken van de status van implementaties op grote schaal. Zie voor meer informatie over het gebruik van de eigenschappen van de IoT Edge-agent voor de status van [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal](module-deployment-monitoring.md).

De volgende tabel bevat geen informatie die is opgehaald uit de gewenste eigenschappen.

| Eigenschap | Description |
| -------- | ----------- |
| lastDesiredVersion | Dit geheel getal verwijst naar de laatste versie van de gewenste eigenschappen die door de IoT Edge-agent verwerkt. |
| lastDesiredStatus.code | Deze statuscode verwijst naar de laatst gewenste eigenschappen zichtbaar voor de IoT Edge-agent. Toegestane waarden: `200` Bewerking is geslaagd, `400` ongeldige configuratie `412` Ongeldige schemaversie `417` de gewenste eigenschappen zijn leeg, `500` is mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| de apparaatstatus | `healthy` Als de runtimestatus van alle modules `running` of `stopped`, `unhealthy` anders |
| configurationHealth. .health {deploymentId} | `healthy` Als de runtimestatus van alle modules die zijn ingesteld door de implementatie {deploymentId} `running` of `stopped`, `unhealthy` anders |
| runtime.platform.OS | Rapportage van het besturingssysteem op het apparaat |
| runtime.platform.architecture | Rapportage van de architectuur van de CPU op het apparaat |
| systemModules.edgeAgent.runtimeStatus | De gerapporteerde status van IoT Edge-agent: {'running' \| 'slecht'} |
| systemModules.edgeAgent.statusDescription | Beschrijving van de gerapporteerde status van de IoT Edge-agent. |
| systemModules.edgeHub.runtimeStatus | Status van IoT Edge hub: {'running' \| 'stopped' \| "mislukt" \| 'uitstel' \| 'slecht'} |
| systemModules.edgeHub.statusDescription | Beschrijving van de status van IoT Edge hub als niet in orde. |
| systemModules.edgeHub.exitCode | De afsluitcode gerapporteerd door de IoT Edge hub-container als de container wordt afgesloten |
| systemModules.edgeHub.startTimeUtc | Tijdstip waarop IoT Edge hub voor het laatst is gestart |
| systemModules.edgeHub.lastExitTimeUtc | Tijd waarop de IoT Edge hub laatst afgesloten |
| systemModules.edgeHub.lastRestartTimeUtc | Keer als IoT Edge hub voor het laatst is gestart |
| systemModules.edgeHub.restartCount | Aantal keren dat die deze module opnieuw als onderdeel van het beleid voor opnieuw opstarten opgestart is. |
| modules.{moduleId}.runtimeStatus | Status van de module: {'running' \| 'stopped' \| "mislukt" \| 'uitstel' \| 'slecht'} |
| modules. {moduleId} .statusDescription | Beschrijving van de status van de module als niet in orde. |
| modules.{moduleId}.exitCode | De afsluitcode gerapporteerd door de module-container als de container wordt afgesloten |
| modules.{moduleId}.startTimeUtc | Tijdstip waarop de module voor het laatst is gestart |
| modules.{moduleId}.lastExitTimeUtc | Tijd wanneer de module laatst afgesloten |
| modules.{moduleId}.lastRestartTimeUtc | Tijd wanneer de module voor het laatst is gestart |
| modules. {moduleId} .restartCount | Aantal keren dat die deze module opnieuw als onderdeel van het beleid voor opnieuw opstarten opgestart is. |

## <a name="edgehub-desired-properties"></a>EdgeHub gewenste eigenschappen

De moduledubbel voor de IoT Edge hub heet `$edgeHub` en coördineert de communicatie tussen de IoT Edge hub die wordt uitgevoerd op een apparaat en IoT-Hub. De gewenste eigenschappen worden ingesteld bij het toepassen van een manifest van de implementatie op een specifiek apparaat als onderdeel van de implementatie van een één-apparaat of op schaal. 

| Eigenschap | Description | In het manifest van de implementatie vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| routes.{routeName} | Een tekenreeks die een IoT Edge hub route vertegenwoordigt. Zie voor meer informatie, [declareren routes](module-composition.md#declare-routes). | De `routes` element aanwezig maar leeg kan zijn. |
| storeAndForwardConfiguration.timeToLiveSecs | De tijd in seconden die IoT Edge hub houdt berichten als losgekoppeld van de routering-eindpunten of IoT-Hub of een lokale module. De waarde kan een positief geheel getal zijn. | Ja |

## <a name="edgehub-reported-properties"></a>EdgeHub gerapporteerde eigenschappen

| Eigenschap | Description |
| -------- | ----------- |
| lastDesiredVersion | Dit geheel getal verwijst naar de laatste versie van de gewenste eigenschappen die door de IoT Edge hub verwerkt. |
| lastDesiredStatus.code | De statuscode die verwijzen naar de laatst gewenste eigenschappen zichtbaar voor de IoT Edge hub. Toegestane waarden: `200` Bewerking is geslaagd, `400` ongeldige configuratie `500` is mislukt |
| lastDesiredStatus.description | Beschrijving van de status. |
| -clients. {apparaat of moduleId} .status | De status van dit apparaat of de module. Mogelijke waarden {"verbonden" \| 'verbinding verbroken'}. Alleen module-id's kunnen zich in niet-verbonden status. Downstream apparaten verbinden met IoT Edge hub worden alleen weergegeven wanneer die zijn verbonden. |
| -clients. {apparaat of moduleId} .lastConnectTime | Laatste keer dat het apparaat of de module die zijn verbonden. |
| -clients. {apparaat of moduleId} .lastDisconnectTime | Laatste keer dat het apparaat of de module verbroken. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over deze eigenschappen gebruiken voor het bouwen van implementatie manifesten, [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).
