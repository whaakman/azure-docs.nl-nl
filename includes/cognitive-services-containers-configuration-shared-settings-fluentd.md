---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: b08516b35a864eae6d15c4c5c928f0550c64c239
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712449"
---
Fluentd is een open-source-gegevensverzamelaar voor centrale logboekregistratie. De `Fluentd` instellingen beheren van de container-verbinding met een [Fluentd](https://www.fluentd.org) server. De container voorzien van een provider van de logboekregistratie Fluentd, waardoor uw container om te schrijven logboeken en, optioneel, metrische gegevens naar een Fluentd-server.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `Fluentd` sectie.

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Host` | Reeks | De IP-adres of de DNS-hostnaam van de Fluentd-server. |
| `Port` | Geheel getal | De poort van de Fluentd-server.<br/> De standaardwaarde is 24224. |
| `HeartbeatMs` | Geheel getal | De heartbeat-interval in milliseconden. Als er geen verkeer van de gebeurtenis is verzonden voordat dit interval is verstreken, wordt er een heartbeat naar de Fluentd-server verzonden. De standaardwaarde is 60000 milliseconden (1 minuut). |
| `SendBufferSize` | Geheel getal | De bufferruimte netwerk verzendt in bytes, dat wordt toegewezen voor bewerkingen. De standaardwaarde is 32768 bytes (32 kB). |
| `TlsConnectionEstablishmentTimeoutMs` | Geheel getal | De time-out in milliseconden, een SSL/TLS-verbinding met de Fluentd-server tot stand brengen. De standaardwaarde is 10000 milliseconden (10 seconden).<br/> Als `UseTLS` is ingesteld op false, deze waarde wordt genegeerd. |
| `UseTLS` | Booleaans | Geeft aan of SSL/TLS in de container moet worden gebruikt voor communicatie met de Fluentd-server. De standaardwaarde is false. |