---
title: Oplossen van problemen met Azure Container Instances
description: Meer informatie over het oplossen van problemen met Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 01/08/2019
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 609d52f9f2c5dce1bbfd668e94db25aca3d52f69
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119047"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Algemene problemen in Azure Container Instances oplossen

In dit artikel laat zien hoe algemene problemen voor het beheren of containers implementeren in Azure Container Instances oplossen.

## <a name="naming-conventions"></a>Naamconventies

Bij het definiëren van uw container-specificatie vereisen bepaalde parameters voldoet aan de naamgeving van beperkingen. Hieronder ziet u een tabel met specifieke vereisten voor container groepseigenschappen. Zie voor meer informatie over naamgevingsregels voor Azure [naamconventies] [ azure-name-restrictions] in het Azure Architecture Center.

| Bereik | Lengte | Hoofdlettergebruik | Geldige tekens | Voorgesteld patroon | Voorbeeld |
| --- | --- | --- | --- | --- | --- | --- |
| Naam van container | 1-64 |Niet hoofdlettergevoelig |Alfanumeriek en afbreekstreepje overal behalve de eerste of laatste teken |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Containernaam | 1-64 |Niet hoofdlettergevoelig |Alfanumeriek en afbreekstreepje overal behalve de eerste of laatste teken |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Container-poorten | Tussen 1 en 65535 |Geheel getal |Geheel getal tussen 1 en 65535 zijn |`<port-number>` |`443` |
| DNS-naamlabel | 5-63 |Niet hoofdlettergevoelig |Alfanumeriek en afbreekstreepje overal behalve de eerste of laatste teken |`<name>` |`frontend-site1` |
| Omgevingsvariabele | 1-63 |Niet hoofdlettergevoelig |Alfanumeriek en een willekeurige plaats, behalve de eerste of laatste teken, onderstrepingsteken (_) |`<name>` |`MY_VARIABLE` |
| Volumenaam | 5-63 |Niet hoofdlettergevoelig |Kleine letters en cijfers en afbreekstreepjes overal behalve de eerste of laatste teken. Mag niet twee opeenvolgende afbreekstreepjes bevatten. |`<name>` |`batch-output-volume` |

## <a name="os-version-of-image-not-supported"></a>Versie van het besturingssysteem van de installatiekopie niet ondersteund

Als u een installatiekopie die biedt geen ondersteuning voor Azure Container Instances, een `OsVersionNotSupported` fout wordt geretourneerd. De fout is vergelijkbaar met de volgende, waar u `{0}` is de naam van de installatiekopie die u hebt geprobeerd te implementeren:

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

Deze fout wordt meestal veroorzaakt als implementatie Windows-installatiekopieën die zijn gebaseerd op een semi-Annual-kanaal (SAC). Bijvoorbeeld Windows versie 1709 en 1803 SAC releases zijn en deze fout bij de implementatie wordt gegenereerd.

Azure Container Instances biedt momenteel ondersteuning voor Windows-installatiekopieën op basis van alleen de **Windows Server 2016 Long-Term Servicing Channel (LTSC)** release. Voor het oplossen van dit probleem bij het implementeren van Windows-containers implementeert u altijd op basis van Windows Server 2016 LTSC afbeeldingen. Installatiekopieën die zijn gebaseerd op Windows Server 2019 (LTSC) worden niet ondersteund.

Zie voor meer informatie over de versies van Windows voor LTSC en SAC [overzicht van Windows Server semi-Annual-kanaal][windows-sac-overview].

## <a name="unable-to-pull-image"></a>Kan geen pull-afbeelding

Als u Azure Container Instances is in eerste instantie kan niet voor het ophalen van uw installatiekopie, het opnieuw probeert voor een bepaalde periode. Als de afbeelding pull-bewerking blijft mislukken, ACI uiteindelijk de implementatie mislukt en ziet u mogelijk een `Failed to pull image` fout.

U lost dit probleem, de containerinstantie verwijderen en voer de implementatie opnieuw uit. Zorg ervoor dat de afbeelding bestaat in het register, en u de naam van de installatiekopie juist hebt getypt.

Als de afbeelding kan niet worden opgehaald, gebeurtenissen, zoals hieronder worden weergegeven in de uitvoer van [az container show][az-container-show]:

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

## <a name="container-continually-exits-and-restarts-no-long-running-process"></a>Container voortdurend wordt afgesloten en opnieuw wordt opgestart (geen langdurig proces)

Containergroepen standaard ingesteld op een [beleid voor opnieuw opstarten](container-instances-restart-policy.md) van **altijd**, zodat de containers in containergroep altijd opnieuw opstarten nadat ze volledig worden uitgevoerd. Mogelijk moet u deze optie om te wijzigen **OnFailure** of **nooit** als u van plan bent om uit te voeren van containers op basis van een taak. Als u opgeeft **OnFailure** en nog steeds Zie continu opnieuw wordt opgestart, kan er een probleem met de toepassing of het script dat wordt uitgevoerd in de container.

Bij het uitvoeren van containergroepen zonder langlopende processen ziet u mogelijk herhaalde wordt afgesloten en opnieuw opstarten met de installatiekopieën, zoals Ubuntu of Alpine. Verbinding maken via [EXEC](container-instances-exec.md) werkt niet naar de container heeft geen proces dat deze actief te houden. Om op te lossen dit ook een opdracht start als volgt met uw implementatie van de groep container naar de container actief blijft.

```azurecli-interactive
## Deploying a Linux container
az container create -g MyResourceGroup --name myapp --image ubuntu --command-line "tail -f /dev/null"
```

```azurecli-interactive 
## Deploying a Windows container
az container create -g myResourceGroup --name mywindowsapp --os-type Windows --image microsoft/windowsservercore:ltsc2016
 --command-line "ping -t localhost"
```

De Container Instances API en Azure portal bevat een `restartCount` eigenschap. Om te controleren of het aantal herstarten van apparaten voor een container, kunt u de [az container show] [ az-container-show] opdracht in de Azure CLI. In het volgende voorbeeld uitvoert (die is afgekapt voor beknoptheid), ziet u de `restartCount` eigenschap aan het einde van de uitvoer.

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
> Een shell, zoals bash, instellen de meeste containerinstallatiekopieën voor Linux-distributies als de standaardopdracht. Omdat een shell op een eigen niet een langlopende-service, deze containers onmiddellijk afsluiten en kunnen worden onderverdeeld in een lus voor opnieuw opstarten wanneer geconfigureerd met de standaard **altijd** beleid voor opnieuw opstarten.

## <a name="container-takes-a-long-time-to-start"></a>Container duurt lang om te starten

De twee primaire factoren die aan de opstarttijd container in Azure Container Instances bijdragen zijn:

* [De grootte van installatiekopie](#image-size)
* [Installatiekopie-locatie](#image-location)

Windows-installatiekopieën hebben [aanvullende overwegingen](#cached-windows-images).

### <a name="image-size"></a>De grootte van installatiekopie

Als uw container lang duurt om te starten, maar uiteindelijk slaagt, starten door te kijken naar de grootte van uw containerinstallatiekopie. Omdat Azure Container Instances uw containerinstallatiekopie op aanvraag haalt, wordt de opstarttijd u ziet direct gerelateerd aan de grootte.

U kunt de grootte van uw containerinstallatiekopie weergeven met behulp van de `docker images` opdracht in de Docker-CLI:

```console
$ docker images
REPOSITORY                  TAG       IMAGE ID        CREATED        SIZE
microsoft/aci-helloworld    latest    7f78509b568e    13 days ago    68.1MB
```

De sleutel voor de grootte klein te houden is ervoor te zorgen dat uw uiteindelijke installatiekopie bevat alles wat is niet vereist zijn tijdens runtime. Een manier om te doen dit is met [meerfasige builds][docker-multi-stage-builds]. Meerdere fasen bouwt maken het eenvoudig om ervoor te zorgen dat de uiteindelijke installatiekopie bevat alleen de artefacten die u nodig hebt voor uw toepassing en niet een van de extra inhoud die is vereist tijdens het opbouwen.

### <a name="image-location"></a>Installatiekopie-locatie

Een andere manier om te beperken de gevolgen voor de pull-installatiekopie op de opstarttijd van de container is voor het hosten van de containerinstallatiekopie in [Azure Container Registry](/azure/container-registry/) in dezelfde regio waar u van plan bent om te containerinstanties implementeren. Dit verkort de netwerkpad dat de container-installatiekopie moet worden verzonden, aanzienlijk verkorten van de downloadtijd.

### <a name="cached-windows-images"></a>Windows-installatiekopieën in de cache

Azure Container Instances gebruikt een cachemechanisme om te snelheid container opstarttijd voor op basis van bepaalde installatiekopieën voor virtuele Windows-installatiekopieën.

Om te controleren of de snelste opstarttijd van de Windows-container, gebruikt u een van de **drie meest recente** versies van de volgende **twee installatiekopieën** als de basisinstallatiekopie:

* [WindowsServer 2016] [ docker-hub-windows-core] (alleen TNS)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

### <a name="windows-containers-slow-network-readiness"></a>Gereedheid voor Windows-containers langzaam netwerk

Eerste gemaakt hebben Windows-containers geen binnenkomende of uitgaande connectiviteit tot 30 seconden (of langer, in uitzonderlijke gevallen). Als uw containertoepassing een internetverbinding moet, vertraging toevoegen en Pogingslogica om toe te staan van 30 seconden tot stand brengen van verbinding met Internet. Na de initiële configuratie, moet op de juiste wijze containernetwerken hervat.

## <a name="resource-not-available-error"></a>Resource niet beschikbaar-fout

Vanwege de verschillende regionale resource laden in Azure, ontvangt u mogelijk de volgende fout wanneer u probeert om een containerexemplaar te implementeren:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Deze fout geeft aan dat vanwege een zware belasting in de regio waarin u probeert te implementeren, de resources die zijn opgegeven voor de container kunnen niet worden toegewezen op dat moment. Een of meer van de volgende stappen voor risicobeperking gebruiken voor het oplossen van uw probleem.

* Controleer of de implementatie-instellingen van uw container vallen binnen de gedefinieerde parameters in [quota en beschikbaarheid in regio's voor Azure Container Instances](container-instances-quotas.md#region-availability)
* Geef instellingen op lagere CPU en geheugen voor de container
* Implementeren naar een andere Azure-regio
* Implementeren op een later tijdstip

## <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>Kan geen verbinding maken met onderliggende Docker API of bevoegde containers uitvoeren

Azure Container Instances maakt niet beschikbaar voor directe toegang tot de onderliggende infrastructuur die als host fungeert voor groepen met containers. Dit omvat toegang tot de Docker-API die wordt uitgevoerd op de host van de container en bevoorrechte containers uitvoeren. Als u nodig hebt voor interactie met Docker, controleert u de [REST-referentiedocumentatie](https://aka.ms/aci/rest) om te zien wat de ACI API ondersteunt. Als er iets ontbreekt, een aanvraag indienen op de [ACI feedbackforums](https://aka.ms/aci/feedback).

## <a name="ips-may-not-be-accessible-due-to-mismatched-ports"></a>IP-adressen mogelijk niet toegankelijk omdat het niet-overeenkomende poorten
Azure Container Instances biedt momenteel geen ondersteuning poort toewijzen, zoals met de configuratie van de reguliere docker, maar deze hotfix op de planning is. Als u het IP-adressen zijn niet toegankelijk als u denkt dat deze moet worden gevonden, controleert u of u hebt geconfigureerd om de containerinstallatiekopie om te luisteren naar de dezelfde poorten die u beschikbaar in uw containergroep met maakt de `ports` eigenschap.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [containerlogbestanden & gebeurtenissen ophalen](container-instances-get-logs.md) om u te helpen bij foutopsporing van uw containers.

<!-- LINKS - External -->
[azure-name-restrictions]: https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions
[windows-sac-overview]: https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/r/microsoft/windowsservercore/
[docker-hub-windows-nano]: https://hub.docker.com/r/microsoft/nanoserver/

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az-container-show
