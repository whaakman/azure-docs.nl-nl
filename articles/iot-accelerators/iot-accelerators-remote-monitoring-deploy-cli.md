---
title: Implementeren van de oplossing externe bewaking van Java - Azure | Microsoft Docs
description: Deze zelfstudie leert u hoe u de oplossingsversneller bewaking op afstand met behulp van de CLI inricht.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/12/2018
ms.topic: conceptual
ms.openlocfilehash: ddb0b5b1a0847200caa7d8d04ecdc9dab4c41d14
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956694"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>De oplossingsversnellers bewaking op afstand met behulp van de CLI implementeren

Deze zelfstudie leert u hoe u de oplossingsverbetering voor externe controle inricht. U implementeert de oplossing met behulp van de CLI. U kunt ook de oplossing implementeren met het web gebaseerde gebruikersinterface op azureiotsuite.com, voor meer informatie over deze optie Zie [de oplossingsverbetering voor externe bewaking implementeren](quickstart-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt de oplossingsverbetering voor externe bewaking implementeert, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Als u wilt uitvoeren van de CLI, moet u [Node.js](https://nodejs.org/) geïnstalleerd op uw lokale computer.

## <a name="install-the-cli"></a>De CLI installeren

Voer de volgende opdracht in uw opdrachtregelomgeving voor het installeren van de CLI:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Meld u aan bij de CLI

Voordat u de oplossingsversnellers implementeren kunt, moet u zich aanmelden bij uw Azure-abonnement met de CLI als volgt:

```cmd/sh
pcs login
```

Volg de aanwijzingen op het scherm instructies om het proces aanmelden te voltooien.

## <a name="deployment-options"></a>Implementatieopties

Wanneer u de oplossingsversneller implementeert, zijn er verschillende opties die het implementatieproces configureren:

| Optie | Waarden | Beschrijving |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Een _basic_ implementatie bedoeld is voor testen en demonstraties, deze alle microservices op een enkele virtuele machine implementeert. Een _standard_ implementatie bedoeld is voor productie, deze implementeert de microservices naar meerdere virtuele machines. Een _lokale_ implementatie configureert u een Docker-container voor het uitvoeren van de microservices op uw lokale computer en maakt gebruik van Azure-services, zoals storage en Cosmos DB, in de cloud. |
| Runtime | `dotnet`, `java` | Hiermee selecteert u de implementatie van de taal van de microservices. |

Zie voor meer informatie over het gebruik van de lokale implementatie [die lokaal wordt uitgevoerd de oplossing voor externe controle](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-vs-standard-deployments"></a>Basic vs. Standard-implementaties

### <a name="basic"></a>Basic
Eenvoudige implementatie is afgestemd op de oplossing die laat zien. Als u wilt de kosten van deze demonstratie verlagen, worden alle microservices geïmplementeerd in een enkele virtuele machine. Dit wordt niet beschouwd als een gereed is voor productie-architectuur.

Onze standaard-implementatie-optie moet worden gebruikt wanneer u klaar bent voor het aanpassen van een architectuur gereed is voor productie, die is gebouwd voor schaal en uitbreidbaarheid.

Het maken van een eenvoudige oplossing zal leiden tot de volgende Azure-services ingericht in uw Azure-abonnement, kosten: 

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
De standaardimplementatie is een gereed is voor productie-implementatie een ontwikkelaar kunt aanpassen en uitbreiden om te voldoen aan hun behoeften. De optie standaard-implementatie moet worden gebruikt wanneer u klaar bent om aan te passen van een architectuur gereed is voor productie, die is gebouwd voor schaal en uitbreidbaarheid. Microservices voor toepassingen worden gebouwd als Docker-containers en geïmplementeerd met behulp van Azure Kubernetes Service (AKS). De orchestrator is verantwoordelijk voor de implementatie, schaling en beheer van de toepassing.


Het maken van een standaardoplossing zal leiden tot de volgende Azure-services ingericht in uw Azure-abonnement, kosten:

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

> Informatie over de prijzen voor deze services vindt [hier](https://azure.microsoft.com/pricing). Gebruiksbedragen en factureringsgegevens voor uw abonnement kunnen worden gevonden in de [Azure Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>De oplossingsversnellers implementeren

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

Bij het uitvoeren van de `pcs` opdracht voor het implementeren van een oplossing wordt u gevraagd om:

- Een naam voor uw oplossing. Deze naam moet uniek zijn.
- Het te gebruiken Azure-abonnement.
- Een locatie.
- Referenties voor de virtuele machines die als host fungeren voor de microservices. U kunt deze referenties gebruiken voor toegang tot de virtuele machines voor het oplossen van problemen.

Wanneer de `pcs` opdracht is voltooid, wordt de URL van uw nieuwe implementatie van de oplossing accelerator weergegeven. De `pcs` opdracht maakt ook een bestand `{deployment-name}-output.json` met aanvullende informatie zoals de naam van de IoT-Hub die is ingericht voor u.

Voor meer informatie over de opdrachtregelparameters worden uitgevoerd:

```cmd/sh
pcs -h
```

Zie voor meer informatie over de CLI, [over het gebruik van de CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De oplossingsversneller configureren
> * De oplossingsversnellers implementeren
> * Aanmelden bij de oplossingsversnellers

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](./quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->
