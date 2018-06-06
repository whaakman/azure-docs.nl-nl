---
title: Het oplossen van exemplaren van de Azure-Container
description: Meer informatie over het oplossen van problemen met Azure Containerexemplaren
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/14/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 39c43c079ea4d10686bd656ba2d451ff42aac9f6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700227"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Algemene problemen in Azure Containerexemplaren

In dit artikel laat zien hoe algemene problemen voor het beheren of containers implementeren naar Azure Containerexemplaren.

## <a name="naming-conventions"></a>Naamconventies

Bij het definiëren van de container-specificatie vereisen bepaalde parameters voldoen aan de naamgeving van beperkingen. Hieronder ziet u een tabel met specifieke vereisten voor de container groepseigenschappen.
Zie voor meer informatie over Azure naamconventies [naamconventies](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) in het midden van de architectuur van Azure.

| Bereik | Lengte | Hoofdlettergebruik | Geldige tekens | Voorgestelde patroon | Voorbeeld |
| --- | --- | --- | --- | --- | --- | --- |
| De naam van container | 1-64 |Niet hoofdlettergevoelig |Alfanumeriek en het koppelstreepje overal behalve de eerste of laatste teken |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Containernaam | 1-64 |Niet hoofdlettergevoelig |Alfanumeriek en het koppelstreepje overal behalve de eerste of laatste teken |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Container poorten | Tussen 1 en 65535 |Geheel getal |Geheel getal tussen 1 en 65535 |`<port-number>` |`443` |
| Label van DNS-naam | 5-63 |Niet hoofdlettergevoelig |Alfanumeriek en het koppelstreepje overal behalve de eerste of laatste teken |`<name>` |`frontend-site1` |
| Omgevingsvariabele | 1-63 |Niet hoofdlettergevoelig |Alfanumeriek en de chracter '_' overal behalve de eerste of laatste teken |`<name>` |`MY_VARIABLE` |
| Volumenaam | 5-63 |Niet hoofdlettergevoelig |Kleine letters, cijfers en afbreekstreepjes overal behalve de eerste of laatste teken. Kan geen twee opeenvolgende afbreekstreepjes bevatten. |`<name>` |`batch-output-volume` |

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

Windows-installatiekopieën hebben [aanvullende overwegingen](#cached-windows-images).

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

### <a name="cached-windows-images"></a>Windows-installatiekopieën in de cache

Azure Container-exemplaren gebruikt een cachemechanisme te helpen snelheid container starten van de tijd voor de installatiekopieën op basis van bepaalde Windows-installatiekopieën.

Gebruik een van de snelste opstarttijd van de Windows-container, zodat de **drie meest recente** versies van de volgende **twee installatiekopieën** als de basisinstallatiekopie:

* [WindowsServer 2016] [ docker-hub-windows-core] (alleen TNS)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

### <a name="windows-containers-slow-network-readiness"></a>Gereedheid van de Windows-containers langzaam netwerk

Windows-containers mogelijk geen verbinding binnenkomend of uitgaand kosten voor 5 seconden op het eerste maken. Container netwerken moet na de initiële installatie op de juiste wijze hervat.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout

Als gevolg van regionale resource wisselende laden in Azure, ziet u mogelijk de volgende fout bij een poging tot het implementeren van een exemplaar van de container:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Deze fout geeft aan dat vanwege een zware belasting in de regio waarin u wilt implementeren, de bronnen die zijn opgegeven voor de container kunnen niet worden toegewezen op dat moment. Een of meer van de volgende stappen gebruiken om u te helpen het probleem kunt oplossen.

* Controleer of de implementatie-instellingen van container vallen binnen de gedefinieerde parameters in [quota en beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-quotas.md#region-availability)
* Lagere CPU en geheugen instellingen opgeven voor de container
* Implementeren naar een ander Azure-regio
* Op een later tijdstip implementeren

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [ophalen van Logboeken van de container en gebeurtenissen](container-instances-get-logs.md) om u te helpen bij foutopsporing van uw containers.

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az_container_show