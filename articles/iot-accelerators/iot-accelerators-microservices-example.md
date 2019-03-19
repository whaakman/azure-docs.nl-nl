---
title: Wijzigen en opnieuw implementeren van een microservice - Azure | Microsoft Docs
description: Deze zelfstudie leert u hoe u kunt wijzigen en opnieuw implementeren van een microservice in externe controle
author: dominicbetts
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/19/2018
ms.topic: conceptual
ms.openlocfilehash: 1552c54afe2195d58a032e9cc7bfa5aa70c844b1
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58004120"
---
# <a name="customize-and-redeploy-a-microservice"></a>Een microservice aanpassen en opnieuw implementeren

Deze zelfstudie leert u hoe u bewerkt een van de [microservices](https://azure.com/microservices) in de oplossing voor externe controle, bouw een installatiekopie van uw microservice, de installatiekopie naar uw docker hub implementeren en vervolgens worden gebruikt in de oplossing voor externe controle. Om te introduceren dit concept, gebruikgemaakt van een eenvoudige scenario waarin u een microservice-API aanroepen en wijzigen van het statusbericht van 'Leven en ook' naar "New bewerkt maken hier!"

Oplossing voor externe bewaking maakt gebruik van microservices die zijn gebouwd met behulp van docker-installatiekopieën die zijn opgehaald uit een docker-hub. 

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Bewerken en maken van een microservice in de oplossing voor externe controle
> * Bouw een docker-installatiekopie
> * Een docker-installatiekopie naar uw docker hub pushen
> * Haal de nieuwe docker-installatiekopie
> * De wijzigingen te visualiseren 

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze zelfstudie volgen, hebt u het volgende nodig:

>[!div class="checklist"]
> * [De Remote Monitoring solution accelerator lokaal implementeren](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Een Docker-account](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) - nodig is om te bekijken van de API-reactie

## <a name="call-the-api-and-view-response-status"></a>Aanroepen van de status van de API en de weergave-antwoord

In dit gedeelte maakt aanroepen u de standaard IoT hub manager microservice API. De API retourneert een statusbericht dat u later door aan te passen de microservice wijzigen.

1. Zorg ervoor dat de oplossing voor externe controle lokaal wordt uitgevoerd op uw computer.
2. Zoek waar u Postman hebt gedownload en open deze.
3. In Postman, voert u het volgende in de GET: `http://localhost:8080/iothubmanager/v1/status`.
4. Het rendement weergeven en u ziet, 'Status': "OK: Alive en goed '.

    ![Actief en goed Postman-bericht](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Wijzig de status en de installatiekopie bouwen

Wijzig nu het statusbericht van de Iot Hub-beheer microservice "Nieuwe wijzigingen aangebracht hier!" en vervolgens opnieuw de docker-installatiekopie met de nieuwe status. Als u hier problemen ondervindt, raadpleegt u onze [probleemoplossing](#Troubleshoot) sectie.

1. Zorg ervoor dat uw terminal is geopend en Ga naar de map waar u de oplossing voor externe controle hebt gekloond. 
1. Wijzig de directory naar 'azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/Services'.
1. StatusService.cs in een teksteditor of IDE die u wilt openen. 
1. Zoek het volgende code op:

    ```csharp
    var result = new StatusServiceModel(true, "Alive and well!");
    ```

    en dit wijzigen in de onderstaande code en sla deze.

    ```csharp
    var result = new StatusServiceModel(true, "New Edits Made Here!");
    ```

5. Ga terug naar uw terminal maar nu wijzigen in de volgende map: 'azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/scripts/docker'.
6. Voor het bouwen van uw nieuwe docker-installatiekopie, typt u het volgende:

    ```sh
    sh build
    ```
    
    of op Windows:
    
    ```cmd
    ./build.cmd
    ```

7. Als u wilt controleren of dat de nieuwe installatiekopie is gemaakt, typt u het volgende:

    ```cmd/sh
    docker images 
    ```

De opslagplaats moet ' azureiotpcs/iothub-manager-dotnet'.

![Geslaagde docker-installatiekopie](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>De installatiekopie taggen en pushen
Voordat u uw nieuwe docker-installatiekopie naar een docker-hub pushen kunt, is Docker wordt verwacht dat uw installatiekopieën worden getagd. Als u hier problemen ondervindt, raadpleegt u onze [probleemoplossing](#Troubleshoot) sectie.

1. Zoek de afbeeldings-ID van de docker-installatiekopie die u hebt gemaakt door te typen:

    ```cmd/sh
    docker images
    ```

2. Voor het taggen van uw installatiekopie met het type 'Test'

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Als u wilt uw zojuist gelabelde installatiekopie pushen naar uw docker hub, typt u het volgende:

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Open uw webbrowser en Ga naar uw [docker hub](https://hub.docker.com/) en meld u aan.
5. U ziet nu de nieuwe gepushte docker-installatiekopie op uw docker hub.
![Docker-installatiekopie van docker-hub](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Bijwerken van uw oplossing voor externe controle
Nu moet u uw lokale docker-compose.yml om op te halen van de nieuwe docker-installatiekopie van uw docker hub bijwerken. Als u hier problemen ondervindt, raadpleegt u onze [probleemoplossing](#Troubleshoot) sectie.

1. Ga terug naar de terminal en Ga naar de volgende map: 'azure-iot-pcs-remote-monitoring-dotnet/services/scripts/local'.
2. Docker-compose.yml in een teksteditor of IDE die u wilt openen.
3. Zoek het volgende code op:

    ```yml
    image: azureiotpcs/iothub-manager-dotnet:testing
    ```

    en pas deze als lijken op de onderstaande afbeelding en op te slaan.

    ```yml
    image: [docker ID]/iothub-manager-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>De nieuwe antwoordstatus weergeven
Voltooien door een lokaal exemplaar van de oplossing voor externe controle opnieuw te implementeren en de nieuwe status reactie in Postman te bekijken.

1. Ga terug naar de terminal en Ga naar de volgende map: 'azure-iot-pcs-remote-monitoring-dotnet/scripts/local'.
2. Start uw lokale exemplaar van de oplossing voor externe controle door de volgende opdracht te typen in de terminal:

    ```cmd/sh
    docker-compose up
    ```

3. Zoek waar u Postman hebt gedownload en open deze.
4. Voer de volgende in de GET-aanvraag in Postman: `http://localhost:8080/iothubmanager/v1/status`. U ziet nu, 'Status': "OK: Nieuwe wijzigingen hier! ".

![Nieuw gemaakt hier bewerkt postman bericht](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="Troubleshoot"></a>Problemen oplossen

Als u problemen uitvoert, probeert u het verwijderen van de docker-installatiekopieën en containers op de lokale computer.

1. Als u wilt alle containers verwijderen, moet u eerst alle actieve containers te stoppen. Open de terminal en typ

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Als u wilt verwijderen van alle installatiekopieën, open de terminal en het type 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. U kunt controleren of er geen containers op de computer door te typen zijn

    ```cmd/sh
    docker ps -aq 
    ```

    Als u alle containers is verwijderd, weergegeven niets.

4. U kunt controleren of er afbeeldingen op de computer door te typen zijn

    ```cmd/sh
    docker images
    ```

    Als u alle containers is verwijderd, weergegeven niets.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe u:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Bewerken en maken van een microservice in de oplossing voor externe controle
> * Bouw een docker-installatiekopie
> * Een docker-installatiekopie naar uw docker hub pushen
> * Haal de nieuwe docker-installatiekopie
> * De wijzigingen te visualiseren 

Nu moeten we om te proberen [aanpassen van het apparaat simulator microservice in de oplossing voor externe controle](iot-accelerators-microservices-example.md)

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

