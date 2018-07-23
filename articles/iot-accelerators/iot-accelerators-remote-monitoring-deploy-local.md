---
title: Implementeer de oplossing voor externe controle lokaal - Azure | Microsoft Docs
description: Deze zelfstudie leert u hoe u de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/07/2018
ms.topic: conceptual
ms.openlocfilehash: 21bc8c27a44c940279b0c5bdcdbe04e579dc4bfa
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188790"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>De Remote Monitoring solution accelerator lokaal implementeren

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. Deze benadering implementeert de microservices naar een lokale Docker-container en maakt gebruik van IoT Hub, Cosmos DB en Azure storage-services in de cloud. U gebruikt de oplossingsversnellers (pc's) CLI voor het implementeren van de Azure-cloud-services.

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS.
* PC 'S-CLI
* Lokale opslagplaats van de broncode

> [!NOTE]
> Deze hulpprogramma's zijn beschikbaar op verschillende platforms, waaronder Windows, Linux en iOS.

### <a name="install-the-pcs-cli"></a>Installeer de CLI-pc 's

Voer de volgende opdracht in uw opdrachtregelomgeving voor het installeren van de CLI PCS via npm:

```cmd/sh
npm install iot-solutions -g
```

Zie voor meer informatie over de CLI, [over het gebruik van de CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

### <a name="download-the-source-code"></a>De broncode downloaden

 Externe controle opslagplaats van de broncode bevat de Docker-configuratiebestanden die u wilt downloaden, configureren en uitvoeren van de Docker-installatiekopieën die de microservices bevatten. Als u wilt klonen en maak een lokale versie van de opslagplaats, gaat u naar een geschikte map op uw lokale computer door uw favoriete vanaf de opdrachtregel of de terminal en voer een van de volgende opdrachten:

Voer het volgende voor het installeren van de Java-implementaties van de microservices:

```cmd/sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Voer het volgende voor het installeren van de .net-implementaties van de microservices:

```cmd\sh
git clone --recursive https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

Externe opslagplaats van de vooraf geconfigureerde oplossing voor Monioring & submodules [ [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) | [.Net](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) ]

> [!NOTE]
> Deze opdrachten downloaden de broncode voor alle microservices. Hoewel u niet de broncode voor het uitvoeren van de microservices in Docker nodig hebt, is de broncode is handig als u later wilt wijzigen van de vooraf geconfigureerde oplossing en de wijzigingen lokaal testen.

## <a name="deploy-the-azure-services"></a>De Azure-services implementeren

Hoewel dit artikel u hoe u de microservices lokaal uitvoert leest, worden ze vertrouwen op drie Azure-services die worden uitgevoerd in de cloud. U kunt deze Azure-services implementeren [handmatig via de Azure-portal](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), of de CLI PCS gebruiken. In dit artikel leest u hoe u de `pcs` hulpprogramma.

### <a name="sign-in-to-the-cli"></a>Meld u aan bij de CLI

Voordat u de oplossingsversnellers implementeren kunt, moet u zich aanmelden bij uw Azure-abonnement met de CLI als volgt:

```cmd/sh
pcs login
```

Volg de aanwijzingen op het scherm instructies om het proces aanmelden te voltooien. Zorg ervoor dat u niet klikt u op een willekeurige plaats in de binnen die de CLI of de aanmelding kan mislukken. U ziet een bericht geslaagde aanmelding in de CLI als u aanmelding hebt voltooid. 

### <a name="run-a-local-deployment"></a>Een lokale implementatie uitvoeren

Gebruik de volgende opdracht om de lokale implementatie te starten. Dit maakt de vereiste azure-resources en omgevingsvariabelen in de console afdrukken. 

```cmd/pcs
pcs -s local
```

Het script vraagt u om de volgende informatie:

* De naam van een oplossing.
* Het te gebruiken Azure-abonnement.
* De locatie van het Azure-datacenter te gebruiken.

> [!NOTE]
> Het script maakt u een IoT-Hub exemplaar, een Cosmos DB-exemplaar en een Azure storage-account in een resourcegroep in uw Azure-abonnement. De naam van de resourcegroep is de naam van de oplossing die u hebt gekozen tijdens het uitvoeren van de `pcs` hulpprogramma hierboven. 

> [!IMPORTANT]
> Het script duurt enkele minuten om uit te voeren. Wanneer deze is voltooid, ziet u een bericht `Copy the following environment variables to /scripts/local/.env file:`. Kopieer in de omgeving variabele definities die volgt op het bericht u gebruikt deze in een latere stap.

## <a name="run-the-microservices-in-docker"></a>Uitvoeren van de microservices in Docker

Als u wilt de microservices in Docker uitvoert, bewerkt u eerst de **scripts\\lokale\\.env** bestand in de lokale kopie van de opslagplaats die u hebt gekloond in een eerdere stap hierboven. Vervang de volledige inhoud van het bestand met de omgeving variabele definities die u hebt tijdens het uitvoeren van de `pcs` opdracht in de laatste stap. Deze omgevingsvariabelen inschakelen de microservices in Docker-container verbinding maken met de Azure-services die zijn gemaakt door de `pcs` hulpprogramma.

Als u wilt uitvoeren in de solution accelerator, gaat u naar de **scripts\local** map in uw opdrachtregelomgeving en voer de volgende opdracht:

```cmd\sh
docker-compose up
```

De eerste keer dat u deze opdracht uitvoert downloadt Docker de microservice-installatiekopieën van Docker hub kunt u de containers lokaal bouwen. In latere runs, Docker de containers wordt onmiddellijk uitgevoerd.

U kunt een afzonderlijke shell gebruiken om de logboeken van de container weer te geven. Zoek eerst de container-ID met de `docker ps -a` opdracht. Gebruik vervolgens `docker logs {container-id} --tail 1000` om de laatste 1000 logboekvermeldingen voor de opgegeven container weer te geven.

Voor toegang tot het oplossingsdashboard voor externe controle, gaat u naar [ http://localhost:8080 ](http://localhost:8080) in uw browser.

## <a name="clean-up"></a>Opruimen

Verwijder onnodige om kosten te voorkomen, wanneer u klaar bent met het testen, cloudservices van uw Azure-abonnement. De eenvoudigste manier om het verwijderen van de services is om te navigeren naar de [Azure-portal](https://ms.portal.azure.com) en verwijder de resourcegroep die u hebt gemaakt via de `pcs` hulpprogramma.

Gebruik de `docker-compose down --rmi all` opdracht voor het verwijderen van de Docker-installatiekopieën en maak ruimte vrij op uw lokale computer. U kunt ook de lokale kopie van de bewaking op afstand opslagplaats gemaakt wanneer u de broncode van GitHub gekloond verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een lokale ontwikkelingsomgeving instellen
> * De oplossingsversneller configureren
> * De oplossingsversnellers implementeren
> * Aanmelden bij de oplossingsversnellers

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->