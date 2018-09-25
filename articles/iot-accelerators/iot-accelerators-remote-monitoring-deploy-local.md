---
title: Implementeer de oplossing voor externe controle lokaal - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing laat zien hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling.
author: asdonald
manager: timlt
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/17/2018
ms.topic: conceptual
ms.openlocfilehash: 5853730a5e3408e33deb483f6ce6652c1c22efab
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034974"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>De Remote Monitoring solution accelerator lokaal implementeren

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. Deze benadering implementeert de microservices naar een lokale Docker-container en maakt gebruik van IoT Hub, Cosmos DB en Azure Time Series Insights-services in de cloud.

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js v8](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS die de scripts gebruiken voor het maken van Azure-resources. Gebruik geen Node.js v10.

> [!NOTE]
> Deze hulpprogramma's zijn beschikbaar op verschillende platforms, waaronder Windows, Linux en iOS.

### <a name="download-the-source-code"></a>De broncode downloaden

De bewaking op afstand source code GitHub-opslagplaats bevat de Docker-configuratiebestanden die u wilt downloaden, configureren en uitvoeren van de Docker-installatiekopieën die de microservices bevatten. Om te klonen en maken van een lokale versie van de opslagplaats, uw opdrachtregelomgeving gebruiken om te navigeren naar een geschikte map op uw lokale computer en voert vervolgens een van de volgende opdrachten:

Voor het downloaden van de meest recente versie van de Java-microservices-implementaties, voert u de volgende uit:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git
cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

Voor het downloaden van de meest recente versie van de .NET-microservice-implementaties, voert u de volgende uit:

```cmd\sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

> [!NOTE]
> Deze opdrachten download de broncode voor de microservices naast de scripts die u kunt de microservices lokaal uitvoeren. Hoewel u niet de broncode voor het uitvoeren van de microservices in Docker nodig hebt, is de broncode is handig als u later wilt wijzigen van de oplossingsversnellers en uw wijzigingen lokaal testen.

## <a name="deploy-the-azure-services"></a>De Azure-services implementeren

Hoewel dit artikel u hoe u de microservices lokaal uitvoert leest, worden ze vertrouwen op Azure-services die worden uitgevoerd in de cloud. U kunt deze Azure-services implementeren [handmatig via de Azure-portal](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), of het opgegeven script. Het volgende scriptvoorbeelden wordt ervan uitgegaan dat u de .NET-opslagplaats op een Windows-machine. Als u in een andere omgeving werkt, aanpassen op de juiste wijze de paden, bestandsextensies en padscheidingstekens. De bijgeleverde scripts te gebruiken:

### <a name="create-new-azure-resources"></a>Nieuwe Azure-resources maken

Als u de vereiste Azure-resources is nog niet hebt gemaakt, volgt u deze stappen:

1. In de opdrachtregel-omgeving, gaat u naar de **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch** map in de gekloonde kopie van de opslagplaats.

2. Voer de **start.cmd** script en volg de aanwijzingen. Het script vraagt u zich aanmeldt bij uw Azure-account en het script nogmaals starten. Het script vraagt u vervolgens de volgende informatie:
    * De naam van een oplossing.
    * Het te gebruiken Azure-abonnement.
    * De locatie van het Azure-datacenter te gebruiken.

    Het script wordt de resourcegroep gemaakt in Azure met de naam van uw oplossing. Deze resourcegroep bevat de Azure-resources die maakt gebruik van de solution accelerator.

3. Wanneer het script is voltooid, wordt een lijst met omgevingsvariabelen weergegeven. Volg de instructies voor het opslaan van deze variabelen aan de **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\.env** bestand.

### <a name="use-existing-azure-resources"></a>Bestaande Azure-bronnen

Als u al hebt gemaakt de vereiste Azure-resources bewerken met de variabele definities van de omgeving in de **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\.env** bestand met de vereiste waarden. De **.env** bestand bevat gedetailleerde informatie over waar u kunt de vereiste waarden vinden.

## <a name="run-the-microservices-in-docker"></a>Uitvoeren van de microservices in Docker

Als u wilt uitvoeren in de solution accelerator, gaat u naar de **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local** map in uw opdrachtregelomgeving en voer de volgende opdracht:

```cmd\sh
docker-compose up
```

De eerste keer dat u deze opdracht uitvoert downloadt Docker de microservice-installatiekopieën van Docker hub kunt u de containers lokaal bouwen. In latere runs, Docker de containers wordt onmiddellijk uitgevoerd.

U kunt een afzonderlijke shell gebruiken om de logboeken van de container weer te geven. Zoek eerst de container-ID met de `docker ps -a` opdracht. Gebruik vervolgens `docker logs {container-id} --tail 1000` om de laatste 1000 logboekvermeldingen voor de opgegeven container weer te geven.

Voor toegang tot het oplossingsdashboard voor externe controle, gaat u naar [ http://localhost:8080 ](http://localhost:8080) in uw browser.

## <a name="clean-up"></a>Opruimen

Verwijder onnodige om kosten te voorkomen, wanneer u klaar bent met het testen, cloudservices van uw Azure-abonnement. De eenvoudigste manier om het verwijderen van de services is om te navigeren naar de [Azure-portal](https://ms.portal.azure.com) en verwijder de resourcegroep die is gemaakt bij het uitvoeren van de **start.cmd** script.

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
