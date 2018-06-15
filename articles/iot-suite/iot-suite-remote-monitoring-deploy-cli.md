---
title: Implementeren van de externe bewakingsoplossing - Azure Java | Microsoft Docs
description: Deze zelfstudie laat zien hoe u voor het inrichten van de externe controle oplossingsverbetering met behulp van de CLI.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: f158af573475964eca1ff168ecf3eadc58c2a394
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33774224"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>De externe controle oplossingsverbetering met behulp van de CLI implementeren

Deze zelfstudie laat zien hoe de oplossingsverbetering voor externe controle inrichten. U implementeert de oplossing met behulp van de CLI. U kunt ook de oplossing met behulp van de gebruikersinterface voor het web gebaseerde op azureiotsuite.com, voor meer informatie over deze optie Zie implementeren [implementeren van de externe controle oplossingsverbetering](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de externe controle oplossingsverbetering, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Als u wilt de CLI uitvoeren, moet u [Node.js](https://nodejs.org/) geïnstalleerd op uw lokale machine.

## <a name="install-the-cli"></a>De CLI installeren

Voer de volgende opdracht in uw omgeving opdrachtregelprogramma voor het installeren van de CLI:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Aanmelden bij de CLI

Voordat u de oplossingsverbetering implementeren kunt, moet u zich aanmelden bij uw Azure-abonnement met de CLI als volgt:

```cmd/sh
pcs login
```

Volg de aanwijzingen op het scherm instructies om het proces aanmelden te voltooien.

## <a name="deployment-options"></a>Implementatieopties

Wanneer u de oplossingsverbetering implementeert, zijn er verschillende opties die het implementatieproces configureren:

| Optie | Waarden | Beschrijving |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Een _basic_ implementatie is bedoeld voor test- en demonstraties, deze alle microservices op één virtuele machine implementeert. Een _standaard_ implementatie is bedoeld voor productie, deze de microservices op meerdere virtuele machines implementeert. Een _lokale_ implementatie configureert u een Docker-container voor het uitvoeren van de microservices op uw lokale computer en maakt gebruik van Azure-services, zoals opslag en Cosmos-database in de cloud. |
| Runtime | `dotnet`, `java` | Hiermee selecteert u de implementatie van de taal van de microservices. |

Zie voor meer informatie over het gebruik van de lokale implementatie, [die lokaal wordt uitgevoerd met de oplossing voor externe controle](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Basic vs. Standaard-implementaties

### <a name="basic"></a>Basic
Eenvoudige implementatie is die is afgestemd op de oplossing weergeven. Als u de kosten van deze demonstratie, zijn alle microservices geïmplementeerd in een enkele virtuele machine; Dit wordt niet beschouwd als een architectuur gereed is voor productie.

Onze standaardimplementatie moet worden gebruikt wanneer u klaar bent voor het aanpassen van een architectuur gereed is voor productie, gebouwd voor schaal en uitbreidingsmogelijkheden.

Wanneer u een eenvoudige oplossing leidt ertoe dat de volgende Azure-services in uw Azure-abonnement tegen kosten wordt ingericht: 

| Count | Resource                       | Type         | Gebruikt voor |
|-------|--------------------------------|--------------|----------|
| 1     | [Virtuele Linux-Machine](https://azure.microsoft.com/services/virtual-machines/) | Standaard D1 V2  | Hosting microservices |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1: basisstaffel | Apparaatbeheer en communicatie |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Het opslaan van configuratiegegevens en telemetriegegevens, zoals regels, waarschuwingen en berichten |  
| 1     | [Azure Storage-Account](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Opslag voor virtuele machine en de controlepunten streaming |
| 1     | [Webtoepassing](https://azure.microsoft.com/services/app-service/web/)        |                 | Hosting-front-endwebtoepassing |

### <a name="standard"></a>Standard
De standaardimplementatie is een gereed is voor productie-implementatie van een ontwikkelaar kan aanpassen en uitbreiden om hun behoeften te voldoen. Betrouwbaarheid en schaal toepassing microservices als Docker-containers zijn gemaakt en geïmplementeerd met behulp van een orchestrator ([Kubernetes](https://kubernetes.io/) standaard). De orchestrator is verantwoordelijk voor de implementatie, schaalbaarheid en beheer van de toepassing.

Maken van een standaardoplossing leidt ertoe dat de volgende Azure-services in uw Azure-abonnement tegen kosten wordt ingericht:

| Count | Resource                                     | SKU / Size      | Gebruikt voor |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Virtuele Linux-machines](https://azure.microsoft.com/services/virtual-machines/)   | Standaard D2 V2  | 1 master en 3 agents voor het hosten van microservices met redundantie |
| 1     | [Azure Container Service](https://azure.microsoft.com/services/container-service/) |                 | [Kubernetes](https://kubernetes.io) orchestrator |
| 1     | [Azure IoT Hub] [https://azure.microsoft.com/services/iot-hub/]                     | S1: basisstaffel | Beheer van apparaten, de opdracht en controle |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Het opslaan van configuratiegegevens en telemetriegegevens, zoals regels, waarschuwingen en berichten |
| 5     | [Azure Storage-Accounts](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | 4 voor VM-opslag en 1 voor de streaming controlepunten |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Toepassingsgateway via SSL |

> Prijsinformatie voor deze services vindt [hier](https://azure.microsoft.com/pricing). Gebruikshoeveelheden en Factureringsdetails voor uw abonnement vindt u in de [Azure Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>De accelerator oplossing implementeren

### <a name="example-deploy-net-version"></a>Voorbeeld: .NET versie implementeren

Het volgende voorbeeld ziet u hoe de basic, .NET-versie van de externe controle oplossingsverbetering implementeren:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Voorbeeld: implementatie van Java-versie

Het volgende voorbeeld ziet u hoe de standaard, Java-versie van de externe controle oplossingsverbetering implementeren:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opties voor de opdracht pc 's

Bij het uitvoeren van de `pcs` opdracht voor het implementeren van een oplossing wordt u gevraagd om:

- Een naam voor uw oplossing. Deze naam moet uniek zijn.
- Het te gebruiken Azure-abonnement.
- Een locatie.
- Referenties voor de virtuele machines die als host fungeren voor de microservices. U kunt deze referenties gebruiken voor toegang tot de virtuele machines voor het oplossen van problemen.

Wanneer de `pcs` opdracht is voltooid, wordt de URL van de nieuwe oplossing accelerator distributie weergegeven. De `pcs` opdracht maakt ook een bestand `{deployment-name}-output.json` met aanvullende informatie zoals de naam van de IoT-Hub die voor u is ingericht.

Voor meer informatie over de opdrachtregelparameters uitvoeren:

```cmd/sh
pcs -h
```

Zie voor meer informatie over de CLI [het gebruik van de CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De oplossingsverbetering configureren
> * De accelerator oplossing implementeren
> * Aanmelden bij de oplossingsverbetering

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het dashboard van de oplossing](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->