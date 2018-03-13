---
title: De oplossing voor externe controle lokaal - Azure implementeren | Microsoft Docs
description: Deze zelfstudie laat zien hoe u de vooraf geconfigureerde oplossing voor externe controle implementeert in uw lokale computer voor het testen en ontwikkeling.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 03/07/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 77f40e2f10cbdb9930a22a4248e19bb3356af7af
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-locally"></a>De vooraf geconfigureerde oplossing voor externe controle lokaal implementeren

Dit artikel laat zien hoe u de vooraf geconfigureerde oplossing voor externe controle implementeert in uw lokale computer voor het testen en ontwikkeling. Deze benadering de microservices implementeert naar een lokale Docker-container en maakt gebruik van IoT Hub, Cosmos DB en Azure storage-services in de cloud. U de vooraf geconfigureerde oplossingen (pc's) CLI om de Azure-cloud-services te implementeren.

## <a name="prerequisites"></a>Vereisten

Als u wilt de Azure-services die worden gebruikt door de vooraf geconfigureerde oplossing voor externe controle implementeert, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -deze software is een vereiste voor de PCS CLI.
* PC 'S CLI

> [!NOTE]
> Deze hulpprogramma's zijn beschikbaar op verschillende platforms, waaronder Windows, Linux en iOS.

### <a name="install-the-pcs-cli"></a>CLI-pc's installeren

Voer de volgende opdracht in uw omgeving opdrachtregelprogramma voor het installeren van de CLI:

```cmd/sh
npm install iot-solutions -g
```

Zie voor meer informatie over de CLI [het gebruik van de CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="deploy-the-azure-services"></a>De Azure-services implementeren

Hoewel dit artikel wordt beschreven hoe u de microservices lokaal uitvoeren, is ze afhankelijk zijn van de drie Azure-services uitgevoerd in de cloud. U kunt deze Azure-services handmatig via de Azure portal implementeren of de CLI PCS gebruiken. Dit artikel laat zien hoe u de `pcs` hulpprogramma.

### <a name="sign-in-to-the-cli"></a>Aanmelden bij de CLI

Voordat u de vooraf geconfigureerde oplossing implementeert kunt, moet u zich aanmelden bij uw Azure-abonnement met de CLI als volgt:

```cmd/sh
pcs login
```

Volg de aanwijzingen op het scherm instructies om het proces aanmelden te voltooien.

### <a name="run-a-local-deployment"></a>Uitvoeren van een lokale implementatie

Gebruik de volgende opdracht in de implementatie van de lokale te starten:

```cmd/pcs
pcs -s local
```

Het script vraagt u om de volgende informatie:

* De oplossingsnaam van een.
* Het te gebruiken Azure-abonnement.
* De locatie van het Azure datacenter te gebruiken.

Het script maakt een IoT-Hub-exemplaar, een Cosmos-DB-exemplaar en een Azure storage-account in een resourcegroep in uw Azure-abonnement. De naam van de resourcegroep is de naam van de oplossing die u hebt gekozen tijdens het uitvoeren van de `pcs` hulpprogramma.

Het script duurt enkele minuten om uit te voeren. Als deze is voltooid, ziet u een bericht `Copy the following environment variables to /scripts/local/.env file:`. De omgeving variabele definities na het bericht kopiëren, u deze gebruiken in een latere stap.

## <a name="download-the-source-code"></a>De broncode downloaden

De opslagplaats externe controle source code bevat de Docker-configuratiebestanden die u wilt downloaden, configureren en uitvoeren van de Docker-afbeeldingen die de microservices bevatten. De opslagplaats klonen, gaat u naar een geschikte map op uw lokale computer en voer een van de volgende opdrachten:

Installeert de Java-implementaties van de microservices te voeren:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Voor het installeren van de .net-implementaties van de microservices uitvoeren:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Deze opdrachten downloaden de broncode voor de microservices. Hoewel u niet de broncode hoeft voor het uitvoeren van de microservices in Docker, is de broncode handig als u later wilt wijzigen van de vooraf geconfigureerde oplossing en uw wijzigingen lokaal testen.

## <a name="run-the-microservices-in-docker"></a>De microservices in Docker uitvoeren

Als u wilt de microservices in Docker uitvoert, bewerkt u eerst de **scripts\\lokale\\.env** bestand in het lokale exemplaar van de opslagplaats. Vervang de volledige inhoud van het bestand met de omgeving variabele definities die u hebt tijdens het uitvoeren van de `pcs` eerder opdracht. Deze omgevingsvariabelen inschakelen de microservices in de Docker-container verbinding maken met de Azure-services gemaakt door de `pcs` hulpprogramma.

Voor het uitvoeren van de vooraf geconfigureerde oplossing, gaat u naar de **scripts\local** map in uw opdrachtregelomgeving en voer de volgende opdracht:

```cmd\sh
docker-compose up
```

De eerste keer dat u deze opdracht uitvoert downloadt Docker de afbeeldingen microservice van Docker-hub voor het bouwen van de containers lokaal. In latere runs Docker de containers wordt onmiddellijk uitgevoerd.

U kunt een afzonderlijke shell gebruiken om de logboeken van de container weer te geven. Zoek eerst de container-ID via de `docker ps -a` opdracht. Gebruik vervolgens `docker logs {container-id} --tail 1000` om weer te geven van de laatste 1000 logboekvermeldingen voor de opgegeven container.

Voor toegang tot het dashboard externe controle oplossing, gaat u naar [http://localhost: 8080](http://localhost:8080) in uw browser.

## <a name="tidy-up"></a>Opruimen

Om te voorkomen onnodige kosten wanneer u klaar bent met het testen, verwijdert u de cloudservices van uw Azure-abonnement. De eenvoudigste manier om te verwijderen van de services is de Azure portal gebruiken om te verwijderen van de resourcegroep die is gemaakt door de `pcs` hulpprogramma.

Gebruik de `docker-compose down --rmi all` opdracht voor het verwijderen van de Docker-installatiekopieën en maak ruimte vrij op uw lokale computer. U kunt ook de lokale kopie van de externe controle opslagplaats gemaakt wanneer u de broncode van GitHub gekloond verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De vooraf geconfigureerde oplossing configureren
> * De vooraf geconfigureerde oplossing implementeren
> * Aanmelden bij de vooraf geconfigureerde oplossing

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het dashboard van de oplossing](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->