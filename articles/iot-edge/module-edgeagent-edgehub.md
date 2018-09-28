---
title: Naslaginformatie over Azure IoT EdgeAgent en EdgeHub | Microsoft Docs
description: Controleer de specifieke eigenschappen en hun waarden voor de edgeAgent en edgeHub moduledubbels
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5e358992661f7bcf06121a07c1bafca0850316b2
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423134"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Eigenschappen van de Edge agent en Edge hub moduledubbels

De Edge agent en Edge hub zijn twee modules die gezamenlijk de IoT Edge-runtime. Zie voor meer informatie over welke functies elke module wordt uitgevoerd, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md). 

Dit artikel bevat de gewenste eigenschappen en de gerapporteerde eigenschappen van de runtime-moduledubbels. Zie voor meer informatie over het implementeren van modules op IoT Edge-apparaten [implementatie en bewaking][lnk-deploy].

## <a name="edgeagent-desired-properties"></a>EdgeAgent gewenste eigenschappen

De moduledubbel voor de Edge agent heet `$edgeAgent` en coördineert de communicatie tussen de Edge agent wordt uitgevoerd op een apparaat en IoT-Hub. De gewenste eigenschappen worden ingesteld bij het toepassen van een manifest van de implementatie op een specifiek apparaat als onderdeel van de implementatie van een één-apparaat of op schaal. 

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| runtime.type | Moet "docker" | Ja |
| runtime.settings.minDockerVersion | Ingesteld op de minimaal vereiste door deze implementatie manifest Docker-versie | Ja |
| runtime.settings.loggingOptions | Een stringified JSON met de opties voor logboekregistratie voor de Edge agent-container. [Docker-opties voor logboekregistratie][lnk-docker-logging-options] | Nee |
| runtime.settings.registryCredentials<br>. {registryId} .username | De gebruikersnaam van het containerregister. De gebruikersnaam is voor Azure Container Registry, gewoonlijk de naam van het containerregister.<br><br> Registerreferenties zijn nodig om alle module-installatiekopieën die niet openbaar zijn. | Nee |
| runtime.settings.registryCredentials<br>. {registryId} .password | Het wachtwoord voor het containerregister. | Nee |
| runtime.settings.registryCredentials<br>. {registryId} .address | Het adres van het containerregister. Voor Azure Container Registry, is het adres meestal *{registryname}.azurecr.IO*. | Nee |  
| systemModules.edgeAgent.type | Moet "docker" | Ja |
| systemModules.edgeAgent.settings.image | De URI van de installatiekopie van het Edge-agent. De Edge agent is momenteel niet kunnen automatisch te laten bijwerken. | Ja |
| systemModules.edgeAgent.settings<br>.createOptions | Een stringified JSON met de opties voor het maken van de Edge agent-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| systemModules.edgeAgent.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze eigenschap is ingesteld door de IoT Hub wanneer deze het manifest wordt toegepast met behulp van een implementatie. Geen deel uitmaakt van een manifest van de implementatie. |
| systemModules.edgeHub.type | Moet "docker" | Ja |
| systemModules.edgeHub.status | Moet worden 'running' | Ja |
| systemModules.edgeHub.restartPolicy | 'Altijd' moet | Ja |
| systemModules.edgeHub.settings.image | De URI van de installatiekopie van Edge hub. | Ja |
| systemModules.edgeHub.settings<br>.createOptions | Een stringified JSON met de opties voor het maken van de Edge hub-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| systemModules.edgeHub.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze eigenschap is ingesteld door de IoT Hub wanneer deze het manifest wordt toegepast met behulp van een implementatie. Geen deel uitmaakt van een manifest van de implementatie. |
| modules. {moduleId} .version | Een gebruiker gedefinieerde tekenreeks voor de versie van deze module. | Ja |
| modules. {moduleId} .type | Moet "docker" | Ja |
| modules. {moduleId} .restartPolicy | {"nooit" \| 'op-is mislukt' \| 'van-niet in orde' \| "altijd"} | Ja |
| modules.{moduleId}.settings.image | De URI naar de installatiekopie van de module. | Ja |
| modules. {moduleId}.settings.createOptions | Een stringified JSON met de opties voor het maken van de module-container. [Opties voor docker maken][lnk-docker-create-options] | Nee |
| modules. {moduleId}.configuration.id | De ID van de implementatie die deze module wordt geïmplementeerd. | Deze eigenschap is ingesteld door de IoT Hub wanneer deze het manifest wordt toegepast met behulp van een implementatie. Geen deel uitmaakt van een manifest van de implementatie. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent gerapporteerde eigenschappen

De Edge agent gerapporteerde eigenschappen van de drie belangrijkste stukjes informatie zijn:

1. De status van de toepassing van de laatst gezien gewenste eigenschappen;
2. De status van de modules die momenteel wordt uitgevoerd op het apparaat, zoals gemeld door de Edge agent; en
3. Een kopie van de gewenste eigenschappen die momenteel wordt uitgevoerd op het apparaat.

Deze laatste stukje informatie is nuttig als de meest recente gewenste eigenschappen zijn niet is toegepast door de runtime en het apparaat wordt nog steeds uitgevoerd voor een eerdere implementatie-manifest.

> [!NOTE]
> De gerapporteerde eigenschappen van de Edge agent zijn nuttig omdat ze kunnen worden opgevraagd met de [IoT Hub-querytaal] [ lnk-iothub-query] voor het onderzoeken van de status van implementaties op grote schaal. Zie voor meer informatie over het gebruik van de eigenschappen van het Edge-agent voor de status van [inzicht in IoT Edge-implementaties voor individuele apparaten of op schaal][lnk-deploy].

De volgende tabel bevat geen informatie die is opgehaald uit de gewenste eigenschappen.

| Eigenschap | Beschrijving |
| -------- | ----------- |
| lastDesiredVersion | Dit geheel getal verwijst naar de laatste versie van de gewenste eigenschappen die door de Edge agent verwerkt. |
| lastDesiredStatus.code | Dit is de statuscode die verwijzen naar de laatst gewenste eigenschappen zichtbaar voor de Edge agent. Toegestane waarden: `200` geslaagd, `400` ongeldige configuratie `412` Ongeldige schemaversie `417` de gewenste eigenschappen zijn leeg, `500` is mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| de apparaatstatus | `healthy` Als de runtimestatus van alle modules `running` of `stopped`, `unhealthy` anders |
| configurationHealth. .health {deploymentId} | `healthy` Als de runtimestatus van alle modules die zijn ingesteld door de implementatie {deploymentId} `running` of `stopped`, `unhealthy` anders |
| runtime.platform.OS | Rapportage van het besturingssysteem op het apparaat |
| runtime.platform.architecture | Rapportage van de architectuur van de CPU op het apparaat |
| systemModules.edgeAgent.runtimeStatus | De gerapporteerde status van de Edge agent: {'running' \| 'slecht'} |
| systemModules.edgeAgent.statusDescription | Beschrijving van de gerapporteerde status van de Edge agent. |
| systemModules.edgeHub.runtimeStatus | Huidige status van de Edge hub: {'running' \| 'stopped' \| "mislukt" \| 'uitstel' \| 'slecht'} |
| systemModules.edgeHub.statusDescription | Beschrijving van de huidige status van de Edge hub als niet in orde. |
| systemModules.edgeHub.exitCode | Als is afgesloten, de afsluitcode gerapporteerd door de Edge hub-container |
| systemModules.edgeHub.startTimeUtc | Tijdstip waarop Edge hub voor het laatst is gestart |
| systemModules.edgeHub.lastExitTimeUtc | Tijd waarop de Edge hub laatst afgesloten |
| systemModules.edgeHub.lastRestartTimeUtc | Keer wanneer Edge hub voor het laatst is gestart |
| systemModules.edgeHub.restartCount | Aantal keren dat die deze module opnieuw als onderdeel van het beleid voor opnieuw opstarten opgestart is. |
| modules.{moduleId}.runtimeStatus | Huidige status van de module: {'running' \| 'stopped' \| "mislukt" \| 'uitstel' \| 'slecht'} |
| modules. {moduleId} .statusDescription | Beschrijving van de huidige status van de module als niet in orde. |
| modules.{moduleId}.exitCode | Als is afgesloten, de afsluitcode gerapporteerd door de module-container |
| modules.{moduleId}.startTimeUtc | Tijdstip waarop de module voor het laatst is gestart |
| modules.{moduleId}.lastExitTimeUtc | Tijd wanneer de module laatst afgesloten |
| modules.{moduleId}.lastRestartTimeUtc | Tijd wanneer de module voor het laatst is gestart |
| modules. {moduleId} .restartCount | Aantal keren dat die deze module opnieuw als onderdeel van het beleid voor opnieuw opstarten opgestart is. |

## <a name="edgehub-desired-properties"></a>EdgeHub gewenste eigenschappen

De moduledubbel voor de Edge hub heet `$edgeHub` en coördineert de communicatie tussen de Edge hub die wordt uitgevoerd op een apparaat en IoT-Hub. De gewenste eigenschappen worden ingesteld bij het toepassen van een manifest van de implementatie op een specifiek apparaat als onderdeel van de implementatie van een één-apparaat of op schaal. 

| Eigenschap | Beschrijving | In het manifest van de implementatie vereist |
| -------- | ----------- | -------- |
| schemaVersion | Moet "1.0" | Ja |
| routes.{routeName} | Een tekenreeks die een Edge hub route vertegenwoordigt. | De `routes` element aanwezig maar leeg kan zijn. |
| storeAndForwardConfiguration.timeToLiveSecs | De tijd in seconden die Edge hub berichten in het geval van niet-verbonden routering eindpunten, bijvoorbeeld niet verbonden met IoT Hub of lokale module houdt | Ja |

## <a name="edgehub-reported-properties"></a>EdgeHub gerapporteerde eigenschappen

| Eigenschap | Beschrijving |
| -------- | ----------- |
| lastDesiredVersion | Dit geheel getal verwijst naar de laatste versie van de gewenste eigenschappen die door de Edge hub verwerkt. |
| lastDesiredStatus.code | Dit is de statuscode die verwijzen naar de laatst gewenste eigenschappen zichtbaar voor de Edge hub. Toegestane waarden: `200` geslaagd, `400` ongeldige configuratie `500` is mislukt |
| lastDesiredStatus.description | Beschrijving van de status |
| -clients. {apparaat of moduleId} .status | De status van dit apparaat of de module. Mogelijke waarden {"verbonden" \| 'verbinding verbroken'}. Alleen module-id's kunnen zich in niet-verbonden status. Downstream apparaten verbinden met Edge hub worden alleen weergegeven wanneer die zijn verbonden. |
| -clients. {apparaat of moduleId} .lastConnectTime | Laatste keer dat het apparaat of de module verbonden |
| -clients. {apparaat of moduleId} .lastDisconnectTime | Laatste keer dat het apparaat of de module niet verbonden |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over deze eigenschappen gebruiken voor het bouwen van implementatie manifesten, [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).

<!--links -->
[lnk-deploy]: module-deployment-monitoring.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
