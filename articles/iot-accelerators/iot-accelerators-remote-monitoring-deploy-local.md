---
title: Implementeer de oplossing voor externe controle lokaal - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing laat zien hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/06/2018
ms.topic: conceptual
ms.openlocfilehash: aaaf31d5c1faae8176dd9909f74c70300c3f0b4e
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716420"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally"></a>De Remote Monitoring solution accelerator lokaal implementeren

In dit artikel leest u hoe de oplossingsverbetering voor externe bewaking implementeren naar uw lokale computer voor het testen en ontwikkeling. Deze benadering implementeert de microservices naar een lokale Docker-container en maakt gebruik van IoT Hub, Cosmos DB en Azure Time Series Insights-services in de cloud. U gebruikt de oplossingsversnellers (pc's) CLI voor het implementeren van de Azure-cloud-services.

## <a name="prerequisites"></a>Vereisten

Voor het implementeren van de Azure-services die worden gebruikt door de oplossingsverbetering voor externe controle, moet u een actief Azure-abonnement.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

Voor het voltooien van de lokale implementatie, moet u de volgende hulpprogramma's geïnstalleerd op uw lokale ontwikkelcomputer:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Docker compose](https://docs.docker.com/compose/install/)
* [Node.js](https://nodejs.org/) -deze software is een vereiste voor de CLI PCS.

> [!NOTE]
> Deze hulpprogramma's zijn beschikbaar op verschillende platforms, waaronder Windows, Linux en iOS.

### <a name="download-the-source-code"></a>De broncode downloaden

 De bewaking op afstand source code GitHub-opslagplaats bevat de Docker-configuratiebestanden die u wilt downloaden, configureren en uitvoeren van de Docker-installatiekopieën die de microservices bevatten. Om te klonen en maken van een lokale versie van de opslagplaats, uw opdrachtregelomgeving gebruiken om te navigeren naar een geschikte map op uw lokale computer en voer een van de volgende opdrachten:

Voer het volgende voor het installeren van de Java-implementaties van de microservices:

```cmd/sh
git clone --recurse-submodules -j8 https://github.com/Azure/azure-iot-pcs-remote-monitoring-java
```

Voer het volgende voor het installeren van de .net-implementaties van de microservices:

```cmd\sh
git clone --recurse-submodules -j8 https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet
```

> [!NOTE]
> Deze opdrachten downloaden de broncode voor alle microservices. Hoewel u niet de broncode voor het uitvoeren van de microservices in Docker nodig hebt, is de broncode is handig als u later wilt wijzigen van de vooraf geconfigureerde oplossing en de wijzigingen lokaal testen.

## <a name="deploy-the-azure-services"></a>De Azure-services implementeren

Hoewel dit artikel u hoe u de microservices lokaal uitvoert leest, worden ze vertrouwen op Azure-services die worden uitgevoerd in de cloud. U kunt deze Azure-services implementeren [handmatig via de Azure-portal](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Manual-steps-to-create-azure-resources-for-local-setup), of het opgegeven script. Het volgende scriptvoorbeelden wordt ervan uitgegaan dat u de .NET-opslagplaats op een Windows-machine. Als u in een andere omgeving werkt, aanpassen op de juiste wijze de paden, bestandsextensies en padscheidingstekens. Het opgegeven script gebruiken:

1. In de opdrachtregel-omgeving, gaat u naar de **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch** map in de gekloonde kopie van de opslagplaats.

1. Voer de **start.cmd** script en volg de aanwijzingen. Het script vraagt u om de volgende informatie:
    * De naam van een oplossing.
    * Het te gebruiken Azure-abonnement.
    * De locatie van het Azure-datacenter te gebruiken.

    Het script wordt de resourcegroep gemaakt in Azure met de naam van uw oplossing.

1. In de opdrachtregel-omgeving, gaat u naar de **azure-iot-pcs-remote-monitoring-dotnet\services\scripts\local\launch\os\win** map in de gekloonde kopie van de opslagplaats.

1. Voer de **set-env-uri.cmd** script.

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