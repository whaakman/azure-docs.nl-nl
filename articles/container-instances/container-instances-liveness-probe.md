---
title: Liveness tests configureren in Azure Containerexemplaren
description: Informatie over het configureren van liveness tests om slecht containers in Azure Containerexemplaren opnieuw te starten
services: container-instances
author: jluk
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: juluk
ms.openlocfilehash: 76ca4db28d99702532ae656a19f0d54b479a13fe
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249279"
---
# <a name="configure-liveness-probes"></a>Liveness tests configureren

Beperkte toepassingen kunnen worden uitgevoerd voor langere tijd waardoor verbroken statussen die worden hersteld moeten door de container opnieuw te starten. Azure Containerexemplaren ondersteunt liveness tests zodanig configuraties zodat uw container opnieuw opstarten kunt als essentiële functionaliteit niet werkt. 

In dit artikel wordt uitgelegd hoe u voor het implementeren van de containergroep van een met een test liveness automatisch opnieuw wordt gestart van een gesimuleerde slecht container te demonstreren.

## <a name="yaml-deployment"></a>YAML-implementatie

Maak een `liveness-probe.yaml` bestand met het volgende fragment. Dit bestand definieert een containergroep die bestaat uit een NGNIX-container die uiteindelijk slecht wordt. 

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
  restartPolicy: Never
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Voer de volgende opdracht voor het implementeren van deze containergroep met de bovenstaande YAML-configuratie:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Opdracht voor starten

De implementatie definieert een eerste opdracht worden uitgevoerd wanneer de container voor het eerst opstart uitgevoerd, gedefinieerd door de `command` eigenschap die een matrix met tekenreeksen accepteert. In dit voorbeeld wordt een bash-sessie starten en maken van een bestand met de naam `healthy` binnen de `/tmp` map door deze opdracht:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Vervolgens wordt slaapstand gedurende 30 seconden alvorens het bestand te verwijderen en vervolgens een tien minuten slaapstand in werking treedt.

### <a name="liveness-command"></a>Liveness-opdracht

Deze implementatie definieert een `livenessProbe` die ondersteuning biedt een `exec` liveness opdracht die als de controle van liveness fungeert. Als u deze opdracht wordt afgesloten met een andere waarde dan nul, de container wordt afgesloten en opnieuw opgestart,-signalering de `healthy` bestand kan niet worden gevonden. Als u deze opdracht met succes is afgesloten met afsluitcode 0, wordt er geen actie ondernomen.

De `periodSeconds` eigenschap geeft de liveness moet worden uitgevoerd om de vijf seconden.

## <a name="verify-liveness-output"></a>Controleer of liveness uitvoer

Binnen de eerste 30 seconden, de `healthy` dat is gemaakt door de opdracht start bestaat. Wanneer de opdracht liveness controleert de `healthy` van bestand bestaan, de statuscode retourneert een nul-signalering geslaagd, zodat geen opnieuw optreedt.

Na 30 seconden, de `cat /tmp/healthy` begint mislukken, waardoor onjuiste en doden gebeurtenissen optreden. 

Deze gebeurtenissen kunnen bekeken worden vanuit de Azure-portal of Azure CLI 2.0.

![Portal slechte gebeurtenis][portal-unhealthy]

Door het bekijken van gebeurtenissen in de Azure portal, gebeurtenissen van het type `Unhealthy` na het mislukken van de opdracht liveness wordt geactiveerd. De volgende gebeurtenis worden van het type `Killing`, waarmee de verwijdering van een container zodat met een herstart beginnen kunt wordt aangeduid. Het aantal opnieuw opstarten voor de container wordt verhoogd telkens wanneer dit gebeurt.

Opnieuw opstarten worden uitgevoerd in-place zodat resources, zoals openbare IP-adressen en knooppuntspecifieke inhoud blijven behouden.

![De teller Portal opnieuw opstarten][portal-restart]

Als de test liveness continu mislukt en te veel opnieuw wordt opgestart activeert, wordt de container een exponentiële back uitschakelen vertraging invoeren.

## <a name="liveness-probes-and-restart-policies"></a>Liveness tests en het beleid voor opnieuw opstarten

Opnieuw opstarten-beleidsregels vervangen de geactiveerd door liveness tests gedrag voor opnieuw opstarten. Als u bijvoorbeeld een `restartPolicy = Never` *en* liveness-test de containergroep wordt niet opnieuw opstarten in geval van een mislukte liveness controle. De containergroep in plaats daarvan moet voldoen aan het beleid opnieuw starten van de containergroep van `Never`.

## <a name="next-steps"></a>Volgende stappen

Scenario's op basis van een taak moet mogelijk een liveness test aangepaste automatisch opnieuw opstarten inschakelen als een vereiste functie is niet goed werkt. Zie voor meer informatie over het uitvoeren van containers taakgebaseerde [beperkte taken uitvoeren in Azure Containerexemplaren](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png