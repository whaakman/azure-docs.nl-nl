---
title: Implementeren van de oplossing externe bewaking van Java - Azure | Microsoft Docs
description: Deze zelfstudie leert u hoe u de oplossingsversneller bewaking op afstand met behulp van de CLI inricht.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: dd696330c9ee78ef84ac9fcf85946c837ad5b824
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188007"
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

Zie voor meer informatie over het gebruik van de lokale implementatie [die lokaal wordt uitgevoerd de oplossing voor externe controle](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Basic vs. Standard-implementaties

### <a name="basic"></a>Basic
Eenvoudige implementatie is afgestemd op de oplossing die laat zien. Als u wilt de kosten van deze demonstratie verlagen, worden alle microservices geïmplementeerd in een enkele virtuele machine. Dit wordt niet beschouwd als een gereed is voor productie-architectuur.

Onze standaard-implementatie-optie moet worden gebruikt wanneer u klaar bent voor het aanpassen van een architectuur gereed is voor productie, die is gebouwd voor schaal en uitbreidbaarheid.

Het maken van een eenvoudige oplossing zal leiden tot de volgende Azure-services ingericht in uw Azure-abonnement, kosten: 

| Count | Resource                       | Type         | Gebruikt voor |
|-------|--------------------------------|--------------|----------|
| 1     | [Virtuele Linux-Machine](https://azure.microsoft.com/services/virtual-machines/) | Standard D1 V2  | Microservices die als host fungeert |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1, Standard-laag | Beheer van apparaten en communicatie |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Opslaan van configuratiegegevens en telemetrie van apparaten, zoals regels, waarschuwingen en berichten |  
| 1     | [Azure Storage-Account](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Opslag voor virtuele machine en de controlepunten streaming |
| 1     | [Web-App](https://azure.microsoft.com/services/app-service/web/)        |                 | Front-endwebtoepassing die als host fungeert |

### <a name="standard"></a>Standard
De standaardimplementatie is een gereed is voor productie-implementatie een ontwikkelaar kunt aanpassen en uitbreiden om te voldoen aan hun behoeften. Voor de betrouwbaarheid en schaal toepassing microservices worden gebouwd als Docker-containers en geïmplementeerd met behulp van een orchestrator ([Kubernetes](https://kubernetes.io/) standaard). De orchestrator is verantwoordelijk voor de implementatie, schaling en beheer van de toepassing.

Het maken van een standaardoplossing zal leiden tot de volgende Azure-services ingericht in uw Azure-abonnement, kosten:

| Count | Resource                                     | SKU / grootte      | Gebruikt voor |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Virtuele Linux-machines](https://azure.microsoft.com/services/virtual-machines/)   | Standard D2 V2  | 1 master en 3 agents voor het hosten van microservices met redundantie |
| 1     | [Azure Container Service](https://azure.microsoft.com/services/container-service/) |                 | [Kubernetes](https://kubernetes.io) orchestrator |
| 1     | [Azure IoT Hub] [https://azure.microsoft.com/services/iot-hub/]                     | S2-Standard-laag | Beheer van apparaten, de opdracht en controle |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Opslaan van configuratiegegevens en telemetrie van apparaten, zoals regels, waarschuwingen en berichten |
| 5     | [Azure Storage-Accounts](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | 4 voor VM-opslag en 1 voor de streaming controlepunten |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Toepassingsgateway via SSL |

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
