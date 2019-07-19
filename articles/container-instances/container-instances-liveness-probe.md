---
title: Tests van de liveiteit configureren in Azure Container Instances
description: Meer informatie over het configureren van Live-tests om beschadigde containers opnieuw te starten in Azure Container Instances
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: 28205d6db85d7a5051f283445d95dd2375e174c8
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325871"
---
# <a name="configure-liveness-probes"></a>Activiteitstests configureren

Container toepassingen kunnen worden uitgevoerd gedurende langere Peri Oden, wat resulteert in verbroken statussen die mogelijk moeten worden hersteld door de container opnieuw op te starten. Azure Container Instances biedt ondersteuning voor de berekenings sondes, zodat de container opnieuw kan worden opgestart als de essentiële functionaliteit niet werkt.

In dit artikel wordt uitgelegd hoe u een container groep implementeert die een afsluitings sonde bevat, waarbij het automatisch opnieuw opstarten van een gesimuleerde beschadigde container wordt gedemonstreerd.

## <a name="yaml-deployment"></a>YAML-implementatie

Maak een `liveness-probe.yaml` bestand met het volgende code fragment. Dit bestand definieert een container groep die bestaat uit een NGNIX-container die uiteindelijk slecht wordt.

```yaml
apiVersion: 2018-06-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command:
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Voer de volgende opdracht uit om deze container groep te implementeren met de bovenstaande YAML-configuratie:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Opdracht starten

De implementatie definieert een begin opdracht die moet worden uitgevoerd wanneer de container voor het eerst wordt gestart, `command` gedefinieerd door de eigenschap waarmee een matrix met teken reeksen wordt geaccepteerd. In dit voor beeld wordt een bash-sessie gestart en wordt een bestand gemaakt `healthy` dat wordt `/tmp` aangeroepen in de Directory door de volgende opdracht door te geven:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 De slaap stand wordt 30 seconden ingedrukt voordat het bestand wordt verwijderd. vervolgens wordt er een slaap stand van tien minuten ingevoerd.

### <a name="liveness-command"></a>Opdracht van liveiteit

In deze implementatie wordt `livenessProbe` een configuratie gedefinieerd `exec` die ondersteuning biedt voor een opdracht van de optie voor het controleren van de levens duur. Als deze opdracht wordt afgesloten met een waarde die niet gelijk is aan nul, wordt de container afgebroken en opnieuw gestart `healthy` , waardoor het bestand niet kan worden gevonden. Als de opdracht met de afsluit code 0 is voltooid, wordt er geen actie ondernomen.

De `periodSeconds` eigenschap geeft aan dat de opdracht voor de liveiteit elke vijf seconden moet worden uitgevoerd.

## <a name="verify-liveness-output"></a>Uitvoer van de liveiteit controleren

In de eerste 30 seconden bestaat het `healthy` bestand dat is gemaakt met de opdracht starten. Wanneer de opdracht van de bewerking voor het `healthy` bestaan van het bestand wordt gecontroleerd, retourneert de status code een nul, het signaal is geslaagd, waardoor het niet opnieuw wordt gestart.

Na 30 seconden mislukt de `cat /tmp/healthy` fout, waardoor er een slechte en afnemende gebeurtenis optreedt.

Deze gebeurtenissen kunnen worden bekeken vanuit de Azure Portal of Azure CLI.

![De portal heeft een slechte gebeurtenis][portal-unhealthy]

Door de gebeurtenissen in de Azure portal weer te geven, worden `Unhealthy` gebeurtenissen van het type geactiveerd op het moment dat de opdracht voor de bewerking wordt uitgevoerd. De volgende gebeurtenis is van het type `Killing`, waardoor een container wordt verwijderd, zodat het opnieuw opstarten kan worden gestart. Het aantal opnieuw te starten voor de container wordt elke keer verhoogd.

Opnieuw opstarten is in-place voltooid zodat resources zoals open bare IP-adressen en platformspecifieke inhoud behouden blijven.

![Teller voor opnieuw starten van portal][portal-restart]

Als de duur van de bewerking continu wordt uitgevoerd en er te veel opnieuw moet worden opgestart, wordt in de container een exponentiële uitstel vertraging ingevoerd.

## <a name="liveness-probes-and-restart-policies"></a>Online tests en beleid voor opnieuw opstarten

Beleid voor opnieuw opstarten vervangt het gedrag voor opnieuw opstarten dat door de Live-tests wordt geactiveerd. Als u bijvoorbeeld een `restartPolicy = Never` - *en* een-test voor de duur instelt, wordt de container groep niet opnieuw opgestart in het geval van een mislukte online controle. In plaats daarvan wordt de container groep gevolgd door het beleid voor het opnieuw opstarten `Never`van de container groep.

## <a name="next-steps"></a>Volgende stappen

Voor scenario's op basis van taken kan het nodig zijn om automatisch opnieuw opstarten in te scha kelen als een vereiste functie niet goed werkt. Zie [taken in een container uitvoeren in azure container instances](container-instances-restart-policy.md)voor meer informatie over het uitvoeren van taak containers.

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
