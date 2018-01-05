---
title: Het oplossen van exemplaren van de Azure-Container
description: Meer informatie over het oplossen van problemen met Azure Containerexemplaren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 1fd3b2c251860e883519744b11fcfc2b925cd2fa
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Problemen met implementatie oplossen met Azure Containerexemplaren

In dit artikel laat zien hoe het oplossen van problemen bij het implementeren van containers naar Containerexemplaren van Azure. Hierin worden ook enkele van de algemene problemen die u kunt tegenkomen.

## <a name="get-diagnostic-events"></a>Ophalen van diagnostische gebeurtenissen

Om Logboeken te raadplegen van uw toepassingscode binnen een container, kunt u de [az container logboeken] [ az-container-logs] opdracht. Maar als de container niet met succes is geïmplementeerd, moet u de diagnostische informatie verstrekt door de bronprovider van exemplaren van Azure-Container. Uitvoeren als u wilt weergeven van de gebeurtenissen voor de container, de [az container weergeven] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

De uitvoer bevat de basiseigenschappen van de container, samen met de implementatie-gebeurtenissen (weergegeven hier ingekorte):

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:49+00:00",
            "lastTimestamp": "2017-12-21T22:50:49+00:00",
            "message": "pulling image \"microsoft/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Successfully pulled image \"microsoft/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Created container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Started container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```

## <a name="common-deployment-issues"></a>Algemene problemen bij de implementatie

Er zijn een aantal veelvoorkomende problemen die account voor de meeste fouten in de implementatie.

## <a name="unable-to-pull-image"></a>Kan geen pull-afbeelding

Als Azure-Containerexemplaren kan niet voor het ophalen van de afbeelding in eerste instantie is, wordt het opnieuw probeert gedurende een bepaalde voordat uiteindelijk mislukken. Als de installatiekopie kan niet worden opgehaald, gebeurtenissen, zoals het volgende worden weergegeven in de uitvoer van [az container weergeven][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

Verwijderen van container om op te lossen, en probeer van uw implementatie, betalende letten dat u de installatiekopie met de naam correct hebt getypt.

## <a name="container-continually-exits-and-restarts"></a>Container voortdurend wordt afgesloten en opnieuw wordt opgestart

Als uw container wordt uitgevoerd voor voltooien en wordt automatisch opnieuw opgestart, moet u mogelijk om in te stellen een [beleid opnieuw opstarten](container-instances-restart-policy.md) van **OnFailure** of **nooit**. Als u opgeeft **OnFailure** en nog steeds Zie continu opnieuw wordt opgestart, wordt er mogelijk een probleem met de toepassing of het script dat wordt uitgevoerd in de container.

De Container exemplaren API bevat een `restartCount` eigenschap. Om te zien van het aantal voor een container, kunt u de [az container weergeven] [ az-container-show] opdracht in de Azure CLI 2.0. In de volgende voorbeelduitvoer (die is afgebroken als beknopt alternatief bevat), ziet u de `restartCount` eigenschap aan het einde van de uitvoer.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Een shell, zoals bash, de meeste container afbeeldingen voor Linux-distributies ingesteld als de opdracht. Omdat een shell zelf geen service van langlopende is, deze containers onmiddellijk afsluiten en worden onderverdeeld in een lus opnieuw opstarten wanneer geconfigureerd met het standaard **altijd** beleid opnieuw opstarten.

## <a name="container-takes-a-long-time-to-start"></a>Container duurt lang om te starten

Als uw container lang duurt om te starten, maar uiteindelijk slaagt, starten door te kijken naar de grootte van de installatiekopie van de container. Omdat Azure Containerexemplaren de installatiekopie van de container op aanvraag haalt, wordt de opstarttijd er rechtstreeks verband houden met de grootte.

U kunt de grootte van de installatiekopie van de container met de Docker CLI bekijken:

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

Grootte klein te houden is ervoor te zorgen dat uw uiteindelijke installatiekopie bevat geen alles wat is niet vereist tijdens runtime. Doen dit is met [fasen builds][docker-multi-stage-builds]. Meerdere fasen bouwt zorg eenvoudig om ervoor te zorgen dat de uiteindelijke installatiekopie bevat alleen de artefacten die u nodig hebt voor uw toepassing en niet een van de extra inhoud die is vereist op build-tijd.

De andere manier om reduceert de gevolgen voor de pull-installatiekopie op de container starten van de tijd is voor het hosten van de installatiekopie van het container met het register van de Container Azure in dezelfde regio waar u wilt gebruiken van Azure Containerexemplaren. Dit verkort het netwerkpad die de installatiekopie van de container reizen moet, aanzienlijk verkorten de downloadtijd.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout

Als gevolg van regionale resource wisselende laden in Azure, ziet u mogelijk de volgende fout bij een poging tot het implementeren van een exemplaar van de container:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Deze fout geeft aan dat vanwege een zware belasting in de regio waarin u wilt implementeren, de bronnen die zijn opgegeven voor de container kunnen niet worden toegewezen op dat moment. Een of meer van de volgende stappen gebruiken om u te helpen het probleem kunt oplossen.

* Controleer of de implementatie-instellingen van container vallen binnen de gedefinieerde parameters in [quota en beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-quotas.md#region-availability)
* Lagere CPU en geheugen instellingen opgeven voor de container
* Implementeren naar een ander Azure-regio
* Op een later tijdstip implementeren

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/

<!-- LINKS - Internal -->
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show