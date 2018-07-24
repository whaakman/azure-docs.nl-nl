---
title: Liveness tests configureren in Azure Container Instances
description: Informatie over het configureren van liveness tests om het opnieuw opstarten niet in orde containers in Azure Container Instances
services: container-instances
author: jluk
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: juluk
ms.openlocfilehash: 1582f0d7ec688bc72cc9d1aa6ae0ddb0a6ad3a17
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213068"
---
# <a name="configure-liveness-probes"></a>Activiteitstests configureren

Toepassingen in containers mogelijk uitgevoerd gedurende langere tijd leidt tot verbroken statussen die worden hersteld moeten door de container opnieuw te starten. Azure Container Instances biedt ondersteuning voor liveness tests om de configuraties opnemen, zodat uw container start opnieuw op als essentiële functionaliteit niet werkt.

In dit artikel wordt uitgelegd hoe u de containergroep van een met een activiteitstest aan te tonen het automatisch opnieuw opstarten van een gesimuleerde niet in orde container implementeren.

## <a name="yaml-deployment"></a>YAML-implementatie

Maak een `liveness-probe.yaml` bestand door het volgende codefragment. Dit bestand definieert een containergroep die bestaat uit een NGNIX-container die uiteindelijk slecht wordt.

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

Voer de volgende opdracht voor het implementeren van deze containergroep met de bovenstaande YAML-configuratie:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>De opdracht start

De implementatie definieert een vanaf opdracht uit om te worden uitgevoerd wanneer de container de eerste keer start actief is, gedefinieerd door de `command` eigenschap waarmee een matrix met tekenreeksen accepteert. In dit voorbeeld wordt een bash-sessie starten en maken van een bestand met de naam `healthy` binnen de `/tmp` map door te geven met deze opdracht:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Deze wordt vervolgens naar de slaapstand gedurende 30 seconden voordat het bestand verwijderen en vervolgens een tien minuten slaapstand in werking treedt.

### <a name="liveness-command"></a>Liveness opdracht

Deze implementatie definieert een `livenessProbe` die ondersteuning biedt een `exec` liveness opdracht die als de controle liveness fungeert. Als deze opdracht wordt afgesloten met een andere waarde dan nul, de container wordt afgesloten en opnieuw opgestart, signalering de `healthy` bestand kan niet worden gevonden. Als deze opdracht wordt is afgesloten met afsluitcode 0, wordt geen actie ondernomen.

De `periodSeconds` eigenschap wijst de liveness opdracht om de vijf seconden moet worden uitgevoerd.

## <a name="verify-liveness-output"></a>Liveness uitvoer controleren

Binnen de eerste 30 seconden, de `healthy` bestand dat is gemaakt door de opdracht start bestaat. Wanneer de opdracht liveness controleert de `healthy` sprake is van het bestand, de statuscode retourneert een nul, signalering geslaagd, zodat geen opnieuw optreedt.

Na 30 seconden, de `cat /tmp/healthy` zullen mislukken, waardoor niet in orde en doden gebeurtenissen optreden.

Deze gebeurtenissen kunnen worden bekeken via Azure portal of Azure CLI.

![Portal onjuiste gebeurtenis][portal-unhealthy]

Door het bekijken van de gebeurtenissen in Azure portal, gebeurtenissen van het type `Unhealthy` na het mislukken van de opdracht liveness wordt geactiveerd. De volgende gebeurtenis worden van het type `Killing`, wat inhoudt container verwijderen zodat met opnieuw opstarten beginnen kunt. Het aantal opnieuw opstarten voor de container wordt verhoogd telkens wanneer dit het geval is.

Opnieuw opstarten worden uitgevoerd in-place zodat resources, zoals openbare IP-adressen en knooppunt-specifieke inhoud blijft behouden.

![Teller van de portal opnieuw opstarten][portal-restart]

Als de activiteitstest continu mislukt en wordt geactiveerd te veel opnieuw wordt opgestart, voert de container een exponentieel uitstel vertraging.

## <a name="liveness-probes-and-restart-policies"></a>Liveness tests en het beleid voor opnieuw opstarten

Beleid voor opnieuw opstarten hebben voorrang boven het gedrag voor opnieuw opstarten geactiveerd door liveness tests. Als u bijvoorbeeld een `restartPolicy = Never` *en* een activiteitstest de containergroep wordt niet opnieuw opgestart in het geval van een mislukte liveness uit. De containergroep in plaats daarvan moet voldoen aan beleid voor het opnieuw starten van de containergroep van `Never`.

## <a name="next-steps"></a>Volgende stappen

Scenario's op basis van een taak is mogelijk een activiteitstest waarmee automatisch opnieuw opstarten als een vereiste functie niet goed werkt. Zie voor meer informatie over het uitvoeren van containers op basis van een taak [taken in containers uitvoeren in Azure Container Instances](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
