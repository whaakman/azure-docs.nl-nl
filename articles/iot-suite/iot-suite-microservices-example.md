---
title: Wijzigen en implementeren van een microservice | Microsoft Docs
description: Deze zelfstudie leert u hoe u kunt wijzigen en implementeren van een microservice in externe controle
services: ''
suite: iot-suite
author: giyeh
manager: hegate
ms.author: giyeh
ms.service: iot-suite
ms.date: 04/19/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: b9be74b4ef5a1239f6ce753ebf41af6b5dbacb5e
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="customize-and-redeploy-a-microservice"></a>Een microservice implementeren en aanpassen

Deze zelfstudie laat zien hoe u een van de microservices in de oplossing voor externe controle te bewerken, bouwen van een installatiekopie van uw microservice, de installatiekopie implementeren op uw docker-hub en vervolgens worden gebruikt in een oplossing voor externe controle. Als u wilt introduceren dit concept, de zelfstudie een eenvoudige scenario waarin u een microservice-API aanroepen en wijzigen van het statusbericht van 'Actief is en goed' gebruikt om te 'Nieuw bewerkt maken hier!'

Oplossing voor externe controle gebruikt microservices die zijn gemaakt met behulp van docker-installatiekopieën die zijn opgehaald uit een docker-hub. 

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Bewerken en bouwen van een microservice in de oplossing voor externe controle
> * Een docker-installatiekopie bouwen
> * Een installatiekopie van een docker push naar de docker-hub
> * Ophalen van de nieuwe docker-afbeelding
> * De wijzigingen te visualiseren 

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, hebt u het volgende nodig:

>[!div class="checklist"]
> * [De vooraf geconfigureerde oplossing voor externe controle lokaal implementeren](../iot-accelerators/iot-accelerators-remote-monitoring-deploy-local.md)
> * [Een Docker-account](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) - nodig zijn om weer te geven van de API-reactie

## <a name="call-the-api-and-view-response-status"></a>Aanroepen van de API en de weergave response-status

In dit gedeelte maakt aanroepen u de standaard IoT hub manager microservice API. De API retourneert een statusbericht dat u later wijzigen door de microservice aanpassen.

1. Zorg ervoor dat de oplossing voor externe controle lokaal op uw computer wordt uitgevoerd.
2. Zoek waar u Postman hebt gedownload en open deze.
3. In Postman, voert u het volgende in op te HALEN: http://localhost:8080/iothubmanager/v1/status.
4. Het retourtype bekijken en u ziet, 'Status': ' OK: actief is en goed '.
![Actief is en goed Postman bericht](media/iot-suite-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Wijzig de status en de installatiekopie maken

Het statusbericht dat van de Iot Hub Manager microservice "Nieuwe wijzigingen aangebracht hier!" nu wijzigen en bouw vervolgens opnieuw de docker-installatiekopie met de nieuwe status. Als u hier problemen ondervindt, raadpleegt u onze [probleemoplossing](#Troubleshoot) sectie.

1. Controleer of dat uw terminal is geopend en Ga naar de map waar u de oplossing voor externe controle hebt gekloond. 
2. Wijzig de directory in '... azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/WebService/v1/Controllers'.
3. StatusController.cs in een teksteditor of IDE die u wilt openen. 
4. Zoek de volgende code:

    ```javascript
    return new StatusApiModel(true, "Alive and well");
    ```

    Wijzig dit in de onderstaande code en sla het.

    ```javascript
    return new StatusApiModel(true, "New Edits Made Here!");
    ```

5. Ga terug naar uw terminal maar nu wijzigen in de volgende map: "... azure-iot-pcs-remote-monitoring-dotnet/iothub-manager/scripts/docker '.
6. Uw nieuwe docker-installatiekopie bouwen, typt u het volgende:

    ```cmd/sh
    sh build
    ```

7. Om te controleren of dat de nieuwe installatiekopie is gemaakt, typt u het volgende:

    ```cmd/sh
    docker images 
    ```

De opslagplaats moet ' azureiotpcs/iothub-manager-dotnet'.

![Geslaagde docker-afbeelding](media/iot-suite-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Tag en push de afbeelding
Voordat u uw nieuwe docker-installatiekopie naar een docker-hub pushen kunt, verwacht Docker uw afbeeldingen worden gelabeld. Als u hier problemen ondervindt, raadpleegt u onze [probleemoplossing](#Troubleshoot) sectie.

1. Zoek de afbeeldings-ID van de docker-installatiekopie die u hebt gemaakt door te typen:

    ```cmd/sh
    docker images
    ```

2. Voor het taggen van uw installatiekopie met het type 'Test'

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Als u wilt uw zojuist TIF pushen naar uw docker-hub, typt u het volgende:

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Open uw browser en Ga naar uw [docker hub](https://hub.docker.com/) en meld u aan.
5. U ziet nu uw zojuist pushed docker-installatiekopie op de docker-hub.
![Afbeelding van docker in docker-hub](media/iot-suite-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Bijwerken van uw oplossing voor externe controle
U moet nu uw lokale docker-compose.yml ophalen van uw nieuwe docker-installatiekopie van uw hub docker bijwerken. Als u hier problemen ondervindt, raadpleegt u onze [probleemoplossing](#Troubleshoot) sectie.

1. Ga terug naar de terminal en Ga naar de volgende map: '... Azure-IOT-PCs-Remote-Monitoring-DotNet/scripts/local'.
2. Docker-compose.yml in een teksteditor of IDE die u wilt openen.
3. Zoek de volgende code:

    ```docker
    image: azureiotpcs/pcs-auth-dotnet:testing
    ```

    en u de onderstaande afbeelding eruit en opslaan wijzigen.

    ```cmd/sh
    image: [docker ID]/pcs-auth-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>De nieuwe antwoordstatus weergeven
Voltooien door opnieuw distribueren van een lokaal exemplaar van de oplossing voor externe controle en het nieuwe statusantwoord in Postman te bekijken.

1. Ga terug naar de terminal en Ga naar de volgende map: '... Azure-IOT-PCs-Remote-Monitoring-DotNet/scripts/local'.
2. Start uw lokale exemplaar van de oplossing voor externe controle door de volgende opdracht te typen in de terminal:

    ```cmd/sh
    docker-compose up
    ```

3. Zoek waar u Postman hebt gedownload en open deze.
4. Voer de volgende aanvraag in op te HALEN in Postman,: http://localhost:8080/iothubmanager/v1/status. U ziet nu, 'Status': ' OK: nieuwe bewerkingen die u hier! '.

![Nieuw aangebracht hier bewerkt postman bericht](media/iot-suite-microservices-example/new-postman-message.png)

## <a name="Troubleshoot"></a>Problemen oplossen

Als u in problemen uitvoert, verwijder de docker-installatiekopieën en containers op de lokale computer.

1. Voor het verwijderen van alle containers, moet u eerst alle actieve containers stoppen. Open de terminal en typ

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Voor het verwijderen van alle afbeeldingen, open de terminal en het type 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. U kunt controleren of er geen containers op de computer door op te geven zijn

    ```cmd/sh
    docker ps -aq 
    ```

    Als u alle containers is verwijderd, weergegeven niets.

4. U kunt controleren of er afbeeldingen op de computer door op te geven zijn

    ```cmd/sh
    docker images
    ```

    Als u alle containers is verwijderd, weergegeven niets.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bewerken en bouwen van een microservice in de oplossing voor externe controle
> * Een docker-installatiekopie bouwen
> * Een installatiekopie van een docker push naar de docker-hub
> * Ophalen van de nieuwe docker-afbeelding
> * De wijzigingen te visualiseren 

Probeer het volgende dat is [aanpassen van het apparaat simulator microservice in de oplossing voor externe controle](iot-suite-remote-monitoring-test.md)

Voor meer informatie voor ontwikkelaars over de oplossing voor externe controle, Zie:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

