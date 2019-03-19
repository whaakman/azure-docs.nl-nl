---
title: Implementeren van de oplossing voor externe controle lokaal - Docker - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing laat zien hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer met behulp van Docker voor testen en ontwikkeling.
author: avneet723
manager: hegate
ms.author: avneet723
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: c00e62e237fe263f54926c8e74fb6211a2e5a4e4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993056"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---docker"></a>De Remote Monitoring solution accelerator lokaal - Docker implementeren

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. Leert u hoe u implementeert de microservices in lokale Docker-containers. Een lokale microservices-implementatie maakt gebruik van de volgende cloudservices: IoT Hub, Cosmos DB, Azure stream Analytics en Azure Time Series Insights-services in de cloud.

Als u wilt dat de oplossingsverbetering voor externe controle uitvoeren in een IDE op uw lokale computer, raadpleegt u [Remote Monitoring solution accelerator lokaal - Visual Studio implementeren](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

### <a name="machine-setup"></a>Machine-instellingen

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/) : als u van plan bent om de microservices te wijzigen.
* [Node.js v8](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS die de scripts gebruiken voor het maken van Azure-resources. Gebruik geen Node.js v10.

> [!NOTE]
> Deze hulpprogramma's zijn beschikbaar op verschillende platforms, waaronder Windows, Linux en iOS.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices-in-docker"></a>Uitvoeren van de microservices in Docker

Open een nieuwe opdrachtprompt om er zeker van te zijn dat toegang hebben tot de omgevingsvariabelen ingesteld door de **start.cmd** script. Op Windows, kunt u controleren of dat de omgevingsvariabelen worden ingesteld door de volgende opdracht uit:

```cmd
set PCS
```

De opdracht toont alle omgevingsvariabelen ingesteld door de **start.cmd** script.

Zorg ervoor dat Docker wordt uitgevoerd op uw lokale computer.
> [!NOTE]
> Docker moet worden uitgevoerd [Linux-containers](https://docs.docker.com/docker-for-windows/) als deze wordt uitgevoerd op Windows.

De microservices die worden uitgevoerd in de lokale Docker-containers nodig hebt voor toegang tot de Azure-cloud-services. U kunt de internetverbinding van uw Docker-omgeving met behulp van de volgende opdracht uit om te pingen van een internetadres uit binnen een container testen:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Als u wilt uitvoeren in de solution accelerator, gaat u naar de **services\\scripts\\lokale** map in uw opdrachtregelomgeving en voer de volgende opdracht:

```cmd/sh
docker-compose up
```

> [!NOTE] 
> Zorg ervoor dat u [delen van een lokaal station](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/issues/115) met Docker voordat u uitvoert `docker-compose up`.

De eerste keer dat u deze opdracht uitvoert downloadt Docker de microservice-installatiekopieën van Docker hub kunt u de containers lokaal bouwen. Op het volgende wordt uitgevoerd, Docker de containers wordt onmiddellijk uitgevoerd.

> [!TIP]
> Microsoft publiceert regelmatig nieuwe Docker-installatiekopieën met nieuwe functionaliteit. U kunt de volgende reeks opdrachten worden opgeschoond gebruiken uw lokale Docker-containers en de bijbehorende afbeeldingen voordat u de meest recente waarden ophaalt:

```cmd/sh
docker list
docker rm <list_of_containers>
docker rmi <list_of_images>
```

U kunt een afzonderlijke shell gebruiken om de logboeken van de container weer te geven. Zoek eerst de container-ID met de `docker ps` opdracht. Gebruik vervolgens `docker logs {container-id} --tail 1000` om de laatste 1000 vermeldingen voor de opgegeven container weer te geven.

### <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten

Volg deze stappen voor het starten van de Stream Analytics-taak:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
1. Navigeer naar de **resourcegroep** gemaakt voor uw oplossing. De naam van de resourcegroep is de naam die u voor uw oplossing hebt gekozen bij het uitvoeren van de **start.cmd** script.
1. Klik op de **Stream Analytics-taak** in de lijst met resources.
1. Op de Stream Analytics-taak **overzicht** pagina, klikt u op de **Start** knop. Klik vervolgens op **Start** de taak om nu te starten.

### <a name="connect-to-the-dashboard"></a>Verbinding maken met het dashboard

Voor toegang tot het oplossingsdashboard voor externe controle, gaat u naar `http://localhost:8080` in uw browser. U kunt nu de Web-UI en de lokale microservices.

## <a name="clean-up"></a>Opruimen

Om te voorkomen dat onnodige verwijderen kosten, wanneer u klaar bent met de test cloudservices uit uw Azure-abonnement. Als u wilt verwijderen van de services, gaat u naar de [Azure-portal](https://ms.portal.azure.com) en verwijder de resource-groep die de **start.cmd** script dat is gemaakt.

Gebruik de `docker-compose down --rmi all` opdracht voor het verwijderen van de Docker-installatiekopieën en maak ruimte vrij op uw lokale computer. U kunt ook de lokale kopie van de bewaking op afstand opslagplaats gemaakt wanneer u de broncode van GitHub gekloond verwijderen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het oplossingsdashboard](quickstart-remote-monitoring-deploy.md).
