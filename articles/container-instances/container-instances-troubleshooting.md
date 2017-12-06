---
title: Het oplossen van exemplaren van de Azure-Container
description: Meer informatie over het oplossen van problemen met Azure Containerexemplaren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 11/18/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6d8fbddc2f26fe739dd725f417961d7b3d7f77e6
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Problemen met implementatie oplossen met Azure Containerexemplaren

In dit artikel laat zien hoe het oplossen van problemen bij het implementeren van containers naar Containerexemplaren van Azure. Hierin worden ook enkele van de algemene problemen die u kunt tegenkomen.

## <a name="get-diagnostic-events"></a>Ophalen van diagnostische gebeurtenissen

Om Logboeken te raadplegen van uw toepassingscode binnen een container, kunt u de [az container logboeken](/cli/azure/container#logs) opdracht. Maar als de container niet met succes is geïmplementeerd, moet u de diagnostische informatie verstrekt door de bronprovider van exemplaren van Azure-Container. Als u wilt weergeven van de gebeurtenissen voor de container, voer de volgende opdracht:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

De uitvoer bevat de basiseigenschappen van de container, samen met de implementatie-gebeurtenissen:

```bash
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
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Algemene problemen bij de implementatie

Er zijn een aantal veelvoorkomende problemen die account voor de meeste fouten in de implementatie.

## <a name="unable-to-pull-image"></a>Kan geen pull-afbeelding

Als Azure-Containerexemplaren kan niet voor het ophalen van de afbeelding in eerste instantie is, wordt het opnieuw probeert gedurende een bepaalde voordat uiteindelijk mislukken. Als de installatiekopie kan niet worden opgehaald, worden gebeurtenissen, zoals het volgende weergegeven:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

Verwijderen van container om op te lossen, en probeer van uw implementatie, betalende letten dat u de installatiekopie met de naam correct hebt getypt.

## <a name="container-continually-exits-and-restarts"></a>Container voortdurend wordt afgesloten en opnieuw wordt opgestart

Als uw container wordt uitgevoerd voor voltooien en wordt automatisch opnieuw opgestart, moet u mogelijk om in te stellen een [beleid opnieuw opstarten](container-instances-restart-policy.md) van **OnFailure** of **nooit**. Als u opgeeft **OnFailure** en nog steeds Zie continu opnieuw wordt opgestart, wordt er mogelijk een probleem met de toepassing of het script dat wordt uitgevoerd in de container.

De Container exemplaren API bevat een `restartCount` eigenschap. Om te zien van het aantal voor een container, kunt u de [az container weergeven](/cli/azure/container#az_container_show) opdracht in de Azure CLI 2.0. In de volgende voorbeelduitvoer (die is afgebroken als beknopt alternatief bevat), ziet u de `restartCount` eigenschap aan het einde van de uitvoer.

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

Grootte klein te houden is ervoor te zorgen dat uw uiteindelijke installatiekopie bevat geen alles wat is niet vereist tijdens runtime. Doen dit is met [fasen builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Meerdere fasen bouwt zorg eenvoudig om ervoor te zorgen dat de uiteindelijke installatiekopie bevat alleen de artefacten die u nodig hebt voor uw toepassing en niet een van de extra inhoud die is vereist op build-tijd.

De andere manier om reduceert de gevolgen voor de pull-installatiekopie op de container starten van de tijd is voor het hosten van de installatiekopie van het container met het register van de Container Azure in dezelfde regio waar u wilt gebruiken van Azure Containerexemplaren. Dit verkort het netwerkpad die de installatiekopie van de container reizen moet, aanzienlijk verkorten de downloadtijd.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout

Als gevolg van regionale resource wisselende laden in Azure, ziet u mogelijk de volgende fout bij een poging tot het implementeren van een exemplaar van de container:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Deze fout geeft aan dat vanwege een zware belasting in de regio waarin u wilt implementeren, de bronnen die zijn opgegeven voor de container kunnen niet worden toegewezen op dat moment. Een of meer van de risicobeperking volgt voor uw probleem op te lossen.

* Controleer of de implementatie-instellingen van container vallen binnen de gedefinieerde parameters in [beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-region-availability.md)
* Lagere CPU en geheugen instellingen opgeven voor de container
* Implementeren naar een ander Azure-regio
* Op een later tijdstip implementeren
