---
title: Het oplossen van exemplaren van de Azure-Container
description: Meer informatie over het oplossen van problemen met Azure Containerexemplaren
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: ff6da0ce95d0405714602c3872da34a2bff344d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Problemen met implementatie oplossen met Azure Containerexemplaren

In dit artikel laat zien hoe het oplossen van problemen bij het implementeren van containers naar Containerexemplaren van Azure. Hierin worden ook enkele van de algemene problemen die u kunt uitvoeren in.

## <a name="getting-diagnostic-events"></a>Ophalen van diagnostische gebeurtenissen

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

### <a name="unable-to-pull-image"></a>Kan geen pull-afbeelding

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

### <a name="container-continually-exits-and-restarts"></a>Container voortdurend wordt afgesloten en opnieuw wordt opgestart

Op dit moment ondersteunt Azure Containerexemplaren alleen langlopende services. Als uw container wordt uitgevoerd om te voltooien en zichzelf afsluit, het automatisch opnieuw wordt opgestart en wordt opnieuw uitgevoerd. Als dit gebeurt, worden gebeurtenissen, zoals die na weergegeven. Houd er rekening mee dat de container kan worden gestart en snel opnieuw wordt opgestart. De Container exemplaren API bevat een `retryCount` eigenschap die laat zien hoe vaak een bepaalde container opnieuw is opgestart.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> Een shell, zoals bash, de meeste container afbeeldingen voor Linux-distributies ingesteld als de opdracht. Aangezien een shell zelf geen langlopende service is, wordt deze containers onmiddellijk afsluiten en kunnen worden onderverdeeld in een lus opnieuw opstarten.

### <a name="container-takes-a-long-time-to-start"></a>Container duurt lang om te starten

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

### <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout

Als gevolg van regionale resource wisselende laden in Azure, ziet u mogelijk de volgende fout bij een poging tot het implementeren van een exemplaar van de container:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Deze fout geeft aan dat vanwege een zware belasting in de regio waarin u wilt implementeren, de bronnen die zijn opgegeven voor de container kunnen niet worden toegewezen op dat moment. Een of meer van de risicobeperking volgt voor uw probleem op te lossen.

* Controleer of de implementatie-instellingen van container vallen binnen de gedefinieerde parameters in [beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-region-availability.md)
* Lagere CPU en geheugen instellingen opgeven voor de container
* Implementeren naar een ander Azure-regio
* Op een later tijdstip implementeren
