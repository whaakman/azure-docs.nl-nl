---
title: Implementeren van de externe bewakingsoplossing - Azure Java | Microsoft Docs
description: Deze zelfstudie laat zien hoe u de vooraf geconfigureerde oplossing voor externe controle Java microsoervices met behulp van de CLI inrichten.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: ea3764299d07f548abbc2857a3adbfb4dc50dec8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-using-the-cli"></a>De vooraf geconfigureerde oplossing voor externe controle met behulp van de CLI implementeren

In deze zelfstudie leert u hoe de vooraf geconfigureerde oplossing voor externe controle inricht. U implementeert de oplossing met behulp van de CLI. U kunt ook de oplossing met behulp van de gebruikersinterface voor het web gebaseerde op azureiotsuite.com, voor meer informatie over deze optie Zie implementeren [implementeren van de vooraf geconfigureerde oplossing voor externe controle](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt de vooraf geconfigureerde oplossing voor externe controle implementeert, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Als u wilt de CLI uitvoeren, moet u [Node.js](https://nodejs.org/) geïnstalleerd op uw lokale machine.

## <a name="install-the-cli"></a>De CLI installeren

Voer de volgende opdracht in uw omgeving opdrachtregelprogramma voor het installeren van de CLI:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Aanmelden bij de CLI

Voordat u de vooraf geconfigureerde oplossing implementeert kunt, moet u zich aanmelden bij uw Azure-abonnement met de CLI als volgt:

```cmd/sh
pcs login
```

Volg de aanwijzingen op het scherm instructies om het proces aanmelden te voltooien.

## <a name="deployment-options"></a>Implementatieopties

Wanneer u de vooraf geconfigureerde oplossing implementeert, zijn er verschillende opties die het implementatieproces configureren:

| Optie | Waarden | Beschrijving |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard` | Een _basic_ implementatie is bedoeld voor test- en demonstraties, deze alle microservices op één virtuele machine implementeert. Een _standaard_ implementatie is bedoeld voor productie, deze de microservices op meerdere virtuele machines implementeert. |
| Runtime | `dotnet`, `java` | Hiermee selecteert u de implementatie van de taal van de microservices. |

## <a name="deploy-the-preconfigured-solution"></a>De vooraf geconfigureerde oplossing implementeren

### <a name="example-deploy-net-version"></a>Voorbeeld: .NET versie implementeren

Het volgende voorbeeld ziet u hoe u de basis, .NET-versie van de vooraf geconfigureerde oplossing voor externe controle implementeert:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Voorbeeld: implementatie van Java-versie

Het volgende voorbeeld ziet u hoe u de standaard, Java-versie van de vooraf geconfigureerde oplossing voor externe controle implementeert:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opties voor de opdracht pc 's

Bij het uitvoeren van de `pcs` opdracht voor het implementeren van een oplossing wordt u gevraagd om:

- Een naam voor uw oplossing. Deze naam moet uniek zijn.
- Het te gebruiken Azure-abonnement.
- Een locatie.
- Referenties voor de virtuele machines die als host fungeren voor de microservices. U kunt deze referenties gebruiken voor toegang tot de virtuele machines voor het oplossen van problemen.

Wanneer de `pcs` opdracht is voltooid, wordt de URL van uw nieuwe implementatie van de vooraf geconfigureerde oplossing weergegeven. De `pcs` opdracht maakt ook een bestand `{deployment-name}-output.json` met aanvullende informatie zoals de naam van de IoT-Hub die voor u is ingericht.

Voor meer informatie over de opdrachtregelparameters uitvoeren:

```cmd/sh
pcs -h
```

Zie voor meer informatie over de CLI [het gebruik van de CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De vooraf geconfigureerde oplossing configureren
> * De vooraf geconfigureerde oplossing implementeren
> * Aanmelden bij de vooraf geconfigureerde oplossing

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het dashboard van de oplossing](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->