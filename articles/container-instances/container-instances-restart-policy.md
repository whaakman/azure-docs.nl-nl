---
title: Taken in containers uitvoeren in Azure Container Instances met beleid voor opnieuw opstarten
description: Informatie over het gebruik van Azure Container Instances voor het uitvoeren van taken die worden uitgevoerd tot voltooiing, zoals in de build-, test- of image-renderingtaken.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/26/2018
ms.author: marsma
ms.openlocfilehash: f4d30a9902261c0e785a1af36a7c1c7a8a0fec46
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444942"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Taken in containers uitvoeren met beleid voor opnieuw opstarten

Het gemak en de snelheid van de implementatie van containers in Azure Container Instances biedt een aantrekkelijke platform voor het uitvoeren van taken, zoals build-, test- en beeldweergave eenmalig worden uitgevoerd in een containerexemplaar.

Met een beleid opnieuw kunnen worden geconfigureerd, kunt u opgeven dat uw containers worden gestopt wanneer hun processen zijn voltooid. Omdat container instances wordt gefactureerd per seconde, u betaalt alleen voor de compute-resources die worden gebruikt terwijl de container voor het uitvoeren van de taak wordt uitgevoerd.

De voorbeelden die zijn gepresenteerd in dit artikel gebruiken de Azure CLI. Hebt u Azure CLI versie 2.0.21 of hoger [lokaal geïnstalleerd][azure-cli-install], of gebruik de CLI in de [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Beleid voor opnieuw opstarten van container

Wanneer u een container in Azure Container Instances maken, kunt u een van drie instellingen voor het beleid opnieuw opgeven.

| Beleid voor opnieuw opstarten   | Beschrijving |
| ---------------- | :---------- |
| `Always` | Containers in containergroep altijd opnieuw opgestart. Dit is de **standaard** instelling wordt toegepast wanneer er geen beleid voor opnieuw opstarten is opgegeven bij het maken van container. |
| `Never` | Containers in containergroep nooit opnieuw opgestart. De containers maximaal één keer uitgevoerd. |
| `OnFailure` | Containers in containergroep zijn opnieuw gestart wanneer het proces dat wordt uitgevoerd in de container is mislukt (wanneer deze wordt afgesloten met een andere afsluitcode). De containers zijn ten minste één keer uitgevoerd. |

## <a name="specify-a-restart-policy"></a>Een beleid voor opnieuw opstarten opgeven

Geeft u een beleid voor opnieuw opstarten, is afhankelijk van hoe u uw container instances kunt, zoals met de Azure CLI, Azure PowerShell-cmdlets, of in de Azure portal maken. In de Azure CLI, geeft u de `--restart-policy` parameter bij het aanroepen van [az container maken][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Voorbeeld van de voltooiing worden uitgevoerd

Als u wilt het beleid voor opnieuw opstarten in actie zien, maakt een containerinstantie van de [aci-microsoft-wordcount] [ aci-wordcount-image] installatiekopie en geef de `OnFailure` beleid voor opnieuw opstarten. Deze voorbeeld-container wordt uitgevoerd een Python-script dat standaard de tekst van de Shakespeare analyseert [Hamlet](http://shakespeare.mit.edu/hamlet/full.html), schrijft de 10 meest voorkomende woorden STDOUT en vervolgens wordt afgesloten.

De voorbeeld-container uitvoeren met de volgende [az container maken] [ az-container-create] opdracht:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances de container begint en stopt wanneer de toepassing (of het script, in dit geval) wordt afgesloten. Wanneer een container met beleid voor opnieuw opstarten is niet meer in Azure Container Instances `Never` of `OnFailure`, van de container de status is ingesteld op **beëindigd**. U kunt de status van een container met controleren de [az container show] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Voorbeelduitvoer:

```bash
"Terminated"
```

Zodra de status van de container van het voorbeeld laat zien *beëindigd*, ziet u de uitvoer van de taak door de containerlogboeken van de weer te geven. Voer de [az container logs] [ az-container-logs] de uitvoer van opdracht om het script weer te geven:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Uitvoer:

```bash
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

In dit voorbeeld ziet u de uitvoer die het script naar de STDOUT verzonden. Uw taken in containers echter mogelijk in plaats daarvan de uitvoer schrijven naar de permanente opslag voor later gebruik. Bijvoorbeeld, voor een [Azure-bestandsshare](container-instances-mounting-azure-files-volume.md).

## <a name="configure-containers-at-runtime"></a>Containers tijdens runtime configureren

Wanneer u een containerexemplaar maakt, kunt u instellen de **omgevingsvariabelen**, evenals een aangepaste **vanaf de opdrachtregel** om uit te voeren wanneer de container wordt gestart. U kunt deze instellingen in uw batch-taken gebruiken om voor te bereiden van elke container met taak-specifieke configuratie.

## <a name="environment-variables"></a>Omgevingsvariabelen

Omgevingsvariabelen instellen in de container voor dynamische configuratie van de toepassing of script uitvoeren op de container. Dit is vergelijkbaar met de `--env` opdrachtregelargument naar `docker run`.

Bijvoorbeeld, kunt u het gedrag van het script in de container voorbeeld door de volgende omgevingsvariabelen op te geven bij het maken van de containerinstantie wijzigen:

*NumWords*: het aantal woorden die worden verzonden naar de STDOUT.

*MinLength*: het minimale aantal tekens in een woord om te worden geteld. Hoe hoger de waarde wordt genegeerd voor veelvoorkomende woorden als 'van' en 'de'.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Door op te geven `NumWords=5` en `MinLength=8` voor omgevingsvariabelen van de container, de containerlogboeken andere uitvoer moeten worden weergegeven. Zodra de containerstatus wordt weergegeven als *beëindigd* (Gebruik `az container show` om te controleren of de status ervan), de logboeken om te zien van de nieuwe uitvoer weergegeven:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Uitvoer:

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="command-line-override"></a>Onderdrukking van de opdrachtregel

Een opdrachtregel opgeven wanneer u een containerexemplaar voor de onderdrukking van de opdrachtregel standaard in de containerinstallatiekopie maakt. Dit is vergelijkbaar met de `--entrypoint` opdrachtregelargument naar `docker run`.

U kunt bijvoorbeeld de voorbeeld-container dan tekst analyseren hebben *Hamlet* door een andere vanaf de opdrachtregel op te geven. Het Python-script uitgevoerd op de container *wordcount.py*, een URL als een argument accepteert en wordt de inhoud van deze pagina in plaats van de standaard verwerken.

Bijvoorbeeld, om te bepalen van de eerste 3 vijf letters woorden in *Valentijn en Juliet*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer3 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Nogmaals, als de container *beëindigd*, de uitvoer weergeven door Logboeken van de container weer te geven:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer3
```

Uitvoer:

```bash
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Volgende stappen

### <a name="persist-task-output"></a>Taakuitvoer behouden

Zie voor meer informatie over het behouden van de uitvoer van de containers die worden uitgevoerd tot voltooiing [koppelen van een Azure-bestandsshare met Azure Container Instances](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
