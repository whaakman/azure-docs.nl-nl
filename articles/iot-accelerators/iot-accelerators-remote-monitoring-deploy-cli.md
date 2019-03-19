---
title: Implementeren van de oplossing voor externe controle met behulp van CLI - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing wordt beschreven hoe u voor het inrichten van de oplossingsversnellers bewaking op afstand met behulp van de CLI.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: a1e65466bb409007a2f112422f393eee786d7b3c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182129"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>De oplossingsversnellers bewaking op afstand met behulp van de CLI implementeren

In deze gebruiksaanwijzing ziet u hoe u de oplossingsverbetering voor externe bewaking implementeert. U implementeert de oplossing met behulp van de CLI. U kunt ook de oplossing met behulp van de gebruikersinterface voor het web gebaseerde op azureiotsolutions.com, voor meer informatie over deze optie Zie implementeren de [de oplossingsverbetering voor externe bewaking implementeren](quickstart-remote-monitoring-deploy.md) Quick Start.

## <a name="prerequisites"></a>Vereisten

Als u wilt de oplossingsverbetering voor externe bewaking implementeert, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Als u wilt uitvoeren van de CLI, moet u [Node.js](https://nodejs.org/) geïnstalleerd op uw lokale computer.

## <a name="install-the-cli"></a>De CLI installeren

Voer de volgende opdracht in uw opdrachtregelomgeving voor het installeren van de CLI:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Meld u aan bij de CLI

Voordat u de oplossingsversnellers implementeren kunt, moet u zich aanmelden bij uw Azure-abonnement met behulp van de CLI:

```cmd/sh
pcs login
```

Volg de aanwijzingen op het scherm instructies om het proces aanmelden te voltooien.

## <a name="deployment-options"></a>Implementatieopties

Wanneer u de oplossingsversneller implementeert, zijn er verschillende opties die het implementatieproces configureren:

| Optie | Waarden | Description |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Een _basic_ implementatie bedoeld is voor testen en demonstraties, deze alle microservices op een enkele virtuele machine implementeert. Een _standard_ implementatie bedoeld is voor productie, deze implementeert de microservices naar meerdere virtuele machines. Een _lokale_ implementatie configureert u een Docker-container voor het uitvoeren van de microservices op uw lokale computer en maakt gebruik van Azure cloudservices, zoals opslag en Cosmos DB. |
| Runtime | `dotnet`, `java` | Hiermee selecteert u de implementatie van de taal van de microservices. |

Zie voor meer informatie over het gebruik van de optie voor lokale implementatie, [die lokaal wordt uitgevoerd de oplossing voor externe controle](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-and-standard-deployments"></a>Basic en standard-implementaties

In deze sectie bevat een overzicht van de belangrijkste verschillen tussen een basic en standard-implementatie.

### <a name="basic"></a>Basic

De Basic-implementatie is bedoeld om de oplossing te demonstreren. Als u wilt verlagen, worden alle microservices worden geïmplementeerd in een enkele virtuele machine. Deze implementatie gebruik geen van een architectuur gereed is voor productie.

Een eenvoudige implementatie wordt gemaakt van de volgende services in uw Azure-abonnement:

| Count | Resource                       | Type         | Gebruikt voor |
|-------|--------------------------------|--------------|----------|
| 1     | [Virtuele Linux-Machine](https://azure.microsoft.com/services/virtual-machines/) | Standard D1 V2  | Microservices die als host fungeert |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1, Standard-laag | Beheer van apparaten en communicatie |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Opslaan van configuratiegegevens, regels, waarschuwingen en andere koude opslag |  
| 1     | [Azure Storage-account](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Opslag voor virtuele machine en de controlepunten streaming |
| 1     | [Web-App](https://azure.microsoft.com/services/app-service/web/)        |                 | Front-endwebtoepassing die als host fungeert |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Het beheren van identiteiten en beveiliging |
| 1     | [Azure Maps](https://azure.microsoft.com/services/azure-maps/)        | Standard                | Asset-locaties weergeven |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 eenheden              | Realtime analyses inschakelen |
| 1     | [Azure Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Inrichten van apparaten op schaal |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1: 1 eenheid              | Opslag voor analyse van berichten gegevens en maakt uitgebreide telemetrie |

### <a name="standard"></a>Standard

Een standaardimplementatie is een gereed is voor productie-implementatie die een ontwikkelaar kunt aanpassen en uitbreiden. Gebruik de optie standaard-implementatie wanneer u klaar om aan te passen van een architectuur gereed is voor productie bent, die is gebouwd voor schaal en uitbreidbaarheid. Toepassing microservices worden gebouwd als Docker-containers en geïmplementeerd met behulp van de Azure Kubernetes Service. De Kubernetes-orchestrator implementeert, kan worden geschaald en beheert de microservices.

Een standaardimplementatie maakt de volgende services in uw Azure-abonnement:

| Count | Resource                                     | SKU / grootte      | Gebruikt voor |
|-------|----------------------------------------------|-----------------|----------|
| 1     | [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service)| Gebruik een volledig beheerde Kubernetes-container orchestration-service, standaard ingesteld op 3 agents|
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2-Standard-laag | Beheer van apparaten, de opdracht en controle |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Opslaan van configuratiegegevens en telemetrie van apparaten, zoals regels, waarschuwingen en berichten |
| 5     | [Azure Storage-Accounts](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | 4 voor VM-opslag en 1 voor de streaming controlepunten |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Toepassingsgateway via SSL |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Het beheren van identiteiten en beveiliging |
| 1     | [Azure Maps](https://azure.microsoft.com/services/azure-maps/)        | Standard                | Asset-locaties weergeven |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 eenheden              | Realtime analyses inschakelen |
| 1     | [Azure Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Inrichten van apparaten op schaal |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1: 1 eenheid              | Opslag voor analyse van berichten gegevens en maakt uitgebreide telemetrie |

> [!NOTE]
> U vindt informatie over prijzen voor deze services op [ https://azure.microsoft.com/pricing ](https://azure.microsoft.com/pricing). U kunt vinden, gebruik en facturering van details van uw abonnement in de [Azure Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>De oplossingsversnellers implementeren

Voorbeelden van implementatie:

### <a name="example-deploy-net-version"></a>Voorbeeld: .NET versie implementeren

Het volgende voorbeeld laat zien hoe de basic-, .NET-versie van de oplossingsverbetering voor externe bewaking implementeren:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Voorbeeld: implementeren van Java-versie

Het volgende voorbeeld ziet u hoe u de standaard, Java-versie van de oplossingsverbetering voor externe bewaking implementeert:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opties voor de opdracht pc 's

Bij het uitvoeren van de `pcs` opdracht voor het implementeren van een oplossing u wordt gevraagd om:

- Een naam voor uw oplossing. Deze naam moet uniek zijn.
- Het te gebruiken Azure-abonnement.
- Een locatie.
- Referenties voor de virtuele machines die als host fungeren voor de microservices. U kunt deze referenties gebruiken voor toegang tot de virtuele machines voor het oplossen van problemen.

Wanneer de `pcs` opdracht is voltooid, wordt de URL van uw nieuwe oplossingsverbetering weergegeven. De `pcs` opdracht maakt ook een bestand `{deployment-name}-output.json` die bevat informatie zoals de naam van de IoT-Hub die deze gemaakt.

Voor meer informatie over de opdrachtregelparameters worden uitgevoerd:

```cmd/sh
pcs -h
```

Zie voor meer informatie over de CLI, [over het gebruik van de CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u:

> [!div class="checklist"]
> * De oplossingsversneller configureren
> * De oplossingsversnellers implementeren
> * Aanmelden bij de oplossingsversnellers

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](./quickstart-remote-monitoring-deploy.md).

<!-- Next how-to guides in the sequence -->
