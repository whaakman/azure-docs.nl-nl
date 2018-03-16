---
title: Het oplossen van exemplaren van de Azure-Container
description: Meer informatie over het oplossen van problemen met Azure Containerexemplaren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a527939d6bc73e3dee5701bc53ef8312e68d2953
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Problemen met implementatie oplossen met Azure Containerexemplaren

In dit artikel laat zien hoe het oplossen van problemen bij het implementeren van containers naar Containerexemplaren van Azure. Hierin worden ook enkele van de algemene problemen die u kunt tegenkomen.

## <a name="view-logs-and-stream-output"></a>Logboeken bekijken en Stroomuitvoer

Wanneer u zich niet normaal gedraagt container hebt, start u door het bekijken van de logboeken met [az container logboeken][az-container-logs], en de standard out en de standaardfout met streaming [az container koppelen] [az-container-attach].

### <a name="view-logs"></a>Logboeken weergeven

Om Logboeken te raadplegen van uw toepassingscode binnen een container, kunt u de [az container logboeken] [ az-container-logs] opdracht.

Hieronder ziet u de uitvoer van de container voorbeeld op basis van een taak in [een beperkte taak uitvoeren in ACI](container-instances-restart-policy.md)nadat het een ongeldige URL verwerken hebben ingevoerd:

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
Traceback (most recent call last):
  File "wordcount.py", line 11, in <module>
    urllib.request.urlretrieve (sys.argv[1], "foo.txt")
  File "/usr/local/lib/python3.6/urllib/request.py", line 248, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/local/lib/python3.6/urllib/request.py", line 223, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/local/lib/python3.6/urllib/request.py", line 532, in open
    response = meth(req, response)
  File "/usr/local/lib/python3.6/urllib/request.py", line 642, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/local/lib/python3.6/urllib/request.py", line 570, in error
    return self._call_chain(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 504, in _call_chain
    result = func(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 650, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found
```

### <a name="attach-output-streams"></a>Uitvoerstromen koppelen

De [az container koppelen] [ az-container-attach] opdracht biedt diagnostische gegevens tijdens het opstarten van de container. Nadat de container is gestart, streams het STDOUT en STDERR naar uw lokale console.

Hier is bijvoorbeeld de uitvoer van de container op basis van een taak in [een beperkte taak uitvoeren in ACI](container-instances-restart-policy.md)nadat hebben opgegeven een geldige URL van een grote tekstbestand verwerken:

```console
$ az container attach --resource-group myResourceGroup --name mycontainer
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-09 23:21:33+00:00) pulling image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Successfully pulled image "microsoft/aci-wordcount:latest"
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Created container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618
(count: 1) (last timestamp: 2018-03-09 23:21:49+00:00) Started container with id e495ad3e411f0570e1fd37c1e73b0e0962f185aa8a7c982ebd410ad63d238618

Start streaming logs:
[('the', 22979),
 ('I', 20003),
 ('and', 18373),
 ('to', 15651),
 ('of', 15558),
 ('a', 12500),
 ('you', 11818),
 ('my', 10651),
 ('in', 9707),
 ('is', 8195)]
```

## <a name="get-diagnostic-events"></a>Ophalen van diagnostische gebeurtenissen

Als uw container niet succesvol zijn geïmplementeerd, moet u de diagnostische informatie verstrekt door de bronprovider van exemplaren van Azure-Container. Uitvoeren als u wilt weergeven van de gebeurtenissen voor de container, de [az container weergeven] [ az-container-show] opdracht:

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

De volgende secties worden veelvoorkomende problemen met dat account voor de meeste fouten in de implementatie van de container:

* [Afbeeldingversie wordt niet ondersteund](#image-version-not-supported)
* [Kan geen pull-afbeelding](#unable-to-pull-image)
* [Container voortdurend wordt afgesloten en opnieuw wordt opgestart](#container-continually-exits-and-restarts)
* [Container duurt lang om te starten](#container-takes-a-long-time-to-start)
* [De fout 'Bron niet beschikbaar'](#resource-not-available-error)

## <a name="image-version-not-supported"></a>Afbeeldingversie wordt niet ondersteund

Als u een installatiekopie die exemplaren van Azure-Container niet ondersteunen kan, Geef een `ImageVersionNotSupported` fout geretourneerd. De waarde van de fout is `The version of image '{0}' is not supported.`, en wordt momenteel toegepast op Windows 1709 installatiekopieën. Om dit probleem beperken, de installatiekopie van een TNS Windows te gebruiken. Ondersteuning voor Windows 1709 installatiekopieën wordt uitgevoerd.

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

De twee primaire factoren bijdragen aan het opstarten van de container in Azure Containerexemplaren zijn:

* [Afbeeldingsgrootte](#image-size)
* [Afbeeldingslocatie](#image-location)

Windows-installatiekopieën hebben [aanvullende overwegingen](#use-recent-windows-images).

### <a name="image-size"></a>Afbeeldingsgrootte

Als uw container lang duurt om te starten, maar uiteindelijk slaagt, starten door te kijken naar de grootte van de installatiekopie van de container. Omdat Azure Containerexemplaren de installatiekopie van de container op aanvraag haalt, wordt de opstarttijd er rechtstreeks verband houden met de grootte.

U kunt de grootte van uw installatiekopie container weergeven met behulp van de `docker images` opdracht in de Docker CLI:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

Grootte klein te houden is ervoor te zorgen dat uw uiteindelijke installatiekopie bevat geen alles wat is niet vereist tijdens runtime. Doen dit is met [fasen builds][docker-multi-stage-builds]. Meerdere fasen bouwt zorg eenvoudig om ervoor te zorgen dat de uiteindelijke installatiekopie bevat alleen de artefacten die u nodig hebt voor uw toepassing en niet een van de extra inhoud die is vereist op build-tijd.

### <a name="image-location"></a>Afbeeldingslocatie

Reduceert de gevolgen voor de pull-installatiekopie op opstarten van de container op een andere manier is voor het hosten van de afbeelding container in [Azure Container register](/azure/container-registry/) in dezelfde regio waar u van plan bent voor het implementeren van containerexemplaren. Dit verkort het netwerkpad die de installatiekopie van de container reizen moet, aanzienlijk verkorten de downloadtijd.

### <a name="use-recent-windows-images"></a>Recente installatiekopieën van Windows gebruiken

Azure Container-exemplaren gebruikt een cachemechanisme te helpen snelheid container starten van de tijd voor de installatiekopieën op basis van bepaalde Windows-installatiekopieën.

Gebruik een van de snelste opstarttijd van de Windows-container, zodat de **drie meest recente** versies van de volgende **twee installatiekopieën** als de basisinstallatiekopie:

* [WindowsServer 2016] [ docker-hub-windows-core] (alleen TNS)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

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
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show